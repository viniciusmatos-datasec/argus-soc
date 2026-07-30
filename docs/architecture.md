# Arquitetura — Argus SOC

Este documento descreve a arquitetura do projeto por serviços, complementando o roadmap semanal. O objetivo é deixar claro como os componentes se encaixam, não apenas quando cada um é construído.

## 1. Diagrama geral

```
┌─────────────────────┐
│     Collectors       │   Wazuh / Sysmon — coleta de logs de segurança
└──────────┬───────────┘
           │
           ▼
┌─────────────────────┐
│      Data Lake        │   data/raw/ , data/processed/ — versionado com DVC
└──────────┬───────────┘
           │
           ▼
┌─────────────────────┐
│   Feature Store        │   Features de detecção — versionadas com DVC
└──────────┬───────────┘
           │
           ▼
┌─────────────────────┐
│     ML Service         │   scikit-learn + SAS Viya (Isolation Forest, LOF)
└──────────┬───────────┘
           │
           ▼
┌─────────────────────┐
│  Detection Engine      │   SIEM — ELK Stack (Elasticsearch, Logstash, Kibana)
└──────────┬───────────┘
           │
           ▼
┌─────────────────────┐        ┌─────────────────────┐
│        SOAR             │──────▶│      PostgreSQL        │
│  TheHive + Shuffle      │        │  incidents, alerts     │
└──────────┬───────────┘        └─────────────────────┘
           │
           ▼
┌─────────────────────┐
│    LLM Analyst          │   Resumo do incidente + recomendação de ação
└──────────┬───────────┘
           │
           ▼
┌─────────────────────┐
│      Dashboard          │   Power BI + Databricks
└──────────┬───────────┘
           │
           ▼
┌─────────────────────┐
│         API             │   FastAPI
└──────────┬───────────┘
           │
           ▼
┌─────────────────────┐
│        Portal            │   GitHub Pages
└─────────────────────┘
```

## 2. Descrição de cada serviço

### Collectors
Responsáveis por gerar e coletar os eventos de segurança que alimentam todo o pipeline.
- **Tecnologia**: Wazuh (agente) e Sysmon
- **Saída**: logs brutos em JSON/CSV
- **Onde vive**: fora do repositório (VM/WSL2 local); os logs coletados são commitados em `data/raw/`

### Data Lake
Armazena os dados brutos e processados do projeto.
- **Tecnologia**: sistema de arquivos local + S3 (a partir da Semana 4), Parquet como formato de dados processados
- **Versionamento**: DVC, não Git direto
- **Localização**: `data/raw/`, `data/processed/`

### Feature Store
Conjunto de features derivadas dos logs, usadas para treinar e servir os modelos de detecção.
- **Tecnologia**: gerado via pandas, versionado com DVC junto ao dataset processado
- **Localização**: `data/processed/features/`

### ML Service
Camada de detecção de anomalias.
- **Tecnologia**: scikit-learn (Isolation Forest, LOF) e SAS Viya (comparação lado a lado)
- **Saída**: score de anomalia por evento, mapeado para táticas MITRE ATT&CK
- **Localização**: `src/ml/`, modelos versionados em `models/`

### Detection Engine (SIEM)
Correlaciona eventos e anomalias em tempo (quase) real.
- **Tecnologia**: Elastic Stack (Elasticsearch, Logstash, Kibana) rodando em uma VM Azure
- **Entrada**: logs brutos (via Filebeat) + scores do ML Service
- **Saída**: dashboards de monitoramento e alertas para severidade crítica

### SOAR
Responde automaticamente a incidentes classificados como críticos.
- **Tecnologia**: TheHive (gestão de casos) + Shuffle (orquestração de playbooks)
- **Ação**: bloqueio de IP, abertura de ticket, notificação via Slack/e-mail
- **Auditoria**: toda ação registrada em log de auditoria imutável (append-only) e também persistida no PostgreSQL

### PostgreSQL
Camada de persistência estruturada para incidentes e alertas — o ponto do projeto onde SQL é aplicado na prática, além do Spark SQL usado no Databricks.
- **Tecnologia**: PostgreSQL, rodando via Docker junto do TheHive/Shuffle
- **Tabelas**: `incidents` (id, título, severidade, status, criado_em) e `alerts` (id, incident_id, origem, descrição, criado_em)
- **Uso**: quando o playbook SOAR age sobre um evento crítico, o registro é inserido no Postgres além de aberto como caso no TheHive
- **Consultas**: incidentes por severidade, tempo médio entre alerta e resposta — usadas no relatório GRC (Semana 10) e no dashboard (Semana 7)
- **Localização**: `src/db/` (conexão e queries), `src/db/migrations/` (schema versionado no Git)

### LLM Analyst
Camada de inteligência aumentada — o que justifica o "autonomous" no nome do projeto, em escopo enxuto.
- **Tecnologia**: chamada a uma API de LLM (ex: Anthropic/OpenAI) a partir do caso aberto no TheHive
- **Função**: resumir o incidente em linguagem natural e sugerir a próxima ação ao analista
- **Escopo deliberadamente pequeno**: um único endpoint de resumo/recomendação — não uma arquitetura de múltiplos agentes autônomos

### Dashboard
Visualização executiva dos KPIs de segurança.
- **Tecnologia**: Power BI conectado a um notebook PySpark no Databricks
- **KPIs**: top ameaças, IPs mais suspeitos, tendência semanal de incidentes (alimentado também pelas queries do PostgreSQL)

### API
Camada de exposição dos dados e funcionalidades do sistema.
- **Tecnologia**: FastAPI
- **Endpoints (escopo enxuto)**:
  - `GET /api/events` — lista de eventos processados
  - `GET /api/incidents` — casos abertos/fechados (lidos do PostgreSQL)
  - `GET /health` — status do serviço
- **Segurança**: autenticação simples via JWT; segredos fora do Git (`.env` + `.gitignore`)

### Portal
Página pública do projeto para portfólio.
- **Tecnologia**: GitHub Pages
- **Conteúdo**: dashboard, vídeo de demonstração, destaques do caso Sombra Silenciosa

## 3. Decisões de arquitetura registradas

| Decisão | O que foi escolhido | Por quê |
|---|---|---|
| Mensageria entre Collectors e Data Lake | Pipeline batch, sem Message Broker (Kafka/RabbitMQ) | Reduz complexidade operacional incompatível com o escopo de 12 semanas solo; fica documentado como evolução futura, não como lacuna |
| Camada de IA generativa | Um único endpoint de resumo/recomendação (LLM Analyst) | Justifica "autonomous" no nome sem exigir uma arquitetura completa de agentes |
| Banco de dados | PostgreSQL enxuto (só `incidents` e `alerts`), sem Qdrant/vetorial | Dá prática real de SQL (schema, queries, joins) sem virar um projeto de banco de dados à parte |
| Observabilidade | Logs estruturados + CI/CD (Semana 11), sem Prometheus/Grafana | Métricas de qualidade (seção "Critérios de qualidade" do documento do projeto) cobrem o essencial sem exigir uma stack de observabilidade própria |
| Segurança do próprio sistema | JWT na API + segredos em `.env` | Cobre o risco mais comum (segredo vazado no Git) sem exigir Vault/RBAC/MFA completos |

## 4. Mapeamento para as versões evolutivas

| Serviço | Versão em que entra |
|---|---|
| Collectors, Data Lake, Feature Store (parcial) | v1 |
| ML Service, Detection Engine (SIEM), Dashboard | v2 |
| SOAR, PostgreSQL | v3 |
| LLM Analyst, API, observabilidade leve | v4 |

Ver `docs/argus-soc-projeto.md` para o detalhamento completo de cada versão.
