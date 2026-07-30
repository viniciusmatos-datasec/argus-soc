# Roadmap — Argus SOC

Detalhamento semana a semana do projeto, organizado por fase e mapeado às versões evolutivas (v1-v4). Ver [`architecture.md`](architecture.md) para a visão por serviços e o documento consolidado do projeto para o contexto completo das versões.

---

## Fase 1 — Fundação (v1 — MVP) — Semanas 1-4

**Papel simulado**: Engenheiro de Dados e Plataforma

### Semana 1 — Setup do projeto e metodologia ágil
- Repositório GitHub com estrutura profissional (`src/`, `tests/`, `docs/`, `infra/`, `notebooks/`, `data/`, `models/`)
- README com objetivo, arquitetura, stack e como rodar localmente
- `CONTRIBUTING.md` com Conventional Commits
- Board Kanban (GitHub Projects): Backlog → Esta semana → Em andamento → Revisão → Feito
- Milestones criados para cada semana; labels de categoria (`fase-1`, `data-science`, `security`, `infra`, `docs`, `sombra-silenciosa`)
- **Entregável**: repositório estruturado, README, CONTRIBUTING.md e board ativo

### Semana 2 — Geração e coleta de logs de segurança (início do caso Sombra Silenciosa)
- Instalar Wazuh (agente) numa VM local ou WSL2
- Baixar dataset público de ataques (CICIDS2017 ou UNSW-NB15) para complementar
- Script Python simulando eventos: logins falhos na VPN, varredura de portas, acesso a arquivos
- Primeiro sinal do padrão de força bruta na VPN da TechNova
- **Entregável**: `data/raw/` com logs reais/simulados e script de geração commitado

### Semana 3 — Pipeline ETL com Python, Excel e primeira versão com DVC
- Notebook Jupyter: carregar logs com pandas, tratar nulos, normalizar timestamps
- Exportar para Excel via openpyxl (resumo, dados brutos, estatísticas, gráficos)
- Salvar dados limpos em `data/processed/` em Parquet
- Inicializar DVC (`dvc init`, configurar remote) e versionar `data/raw/` e `data/processed/`
- Testes pytest para as funções de transformação
- **Entregável**: notebook ETL, Excel automatizado, dados versionados com DVC, testes passando no CI

### Semana 4 — Infraestrutura como código com Terraform na AWS
- Conta AWS Free Tier + Terraform: VPC, subnet, security group, EC2 t2.micro
- Backend remoto do state no S3 com lock via DynamoDB
- Logs processados subidos para S3
- GitHub Action rodando `terraform plan` em todo Pull Request
- **Entregável**: infra em `infra/aws/`, EC2 provisionado, pipeline CI com `terraform plan`

---

## Fase 2 — Inteligência (v2 — Infraestrutura) — Semanas 5-7

**Papel simulado**: Engenheiro de Detecção / Analista de SOC

### Semana 5 — Detecção de anomalias com Machine Learning (com DVC)
- Feature engineering sobre os dados processados
- Trilha Python: Isolation Forest e LOF (scikit-learn), avaliados com matriz de confusão
- Trilha SAS: mesma detecção refeita no SAS Viya for Learners, para comparação
- Mapeamento das anomalias para táticas MITRE ATT&CK
- Modelo e dataset de treino versionados com DVC (rastreabilidade dado → modelo → detecção)
- Testes pytest cobrindo a trilha Python
- **Entregável**: modelo versionado com DVC, comparação Python vs SAS documentada, testes passando

### Semana 6 — SIEM com ELK Stack na Azure
- VM na Azure via Terraform (azurerm provider) rodando ELK via docker-compose
- Pipeline Logstash ingerindo logs e alertas do modelo
- Filebeat para envio contínuo de eventos
- Dashboards Kibana e alerta simples para eventos críticos
- **Entregável**: SIEM rodando na Azure, `infra/azure/`, dashboards Kibana

### Semana 7 — Dashboard executivo com Power BI e Databricks
- Workspace Databricks Community Edition + notebook PySpark
- Power BI conectado, KPIs de segurança (top ameaças, IPs suspeitos, tendência semanal)
- Medidas DAX, relatório publicado com GIF de demonstração
- **Entregável**: notebook Databricks, `.pbix` publicado, GIF no README

---

## Fase 2 final + Fase 3 (v3 — Autonomia) — Semanas 8-10

**Papel simulado**: Líder de Resposta a Incidentes / Analista de GRC (início)

### Semana 8 — Multi-Cloud: AWS + Azure + GCP
- Recurso equivalente no GCP via Terraform (google provider)
- Módulos separados por provedor (`infra/aws/`, `infra/azure/`, `infra/gcp/`) com workspaces
- Comparação de custos e latência entre nuvens
- Diagrama de arquitetura multi-cloud e um ADR justificando as escolhas
- **Entregável**: infra multi-cloud versionada, diagrama e ADR

### Semana 9 — SOAR + PostgreSQL: resposta automática ao Sombra Silenciosa
- TheHive + Shuffle/Cortex via Docker
- SIEM conectado ao TheHive: anomalias críticas viram casos automaticamente
- **Subir um container PostgreSQL via Docker (mesmo `docker-compose` do TheHive)**
- **Criar o schema SQL: tabelas `incidents` (id, título, severidade, status, criado_em) e `alerts` (id, incident_id, origem, descrição, criado_em)**
- Escrever playbook Python: quando severidade = crítica, bloquear o IP, abrir ticket no TheHive **e inserir o registro no PostgreSQL**
- Integrar notificação automática por Slack ou e-mail quando o playbook é executado
- Registrar cada ação automática em um log de auditoria imutável (append-only)
- **Escrever queries SQL reais para consulta: incidentes por severidade, tempo médio entre alerta e resposta**
- Escrever testes pytest simulando o disparo do playbook e validando as ações tomadas (incluindo a inserção no banco)
- **Entregável**: playbook funcionando, casos no TheHive, schema PostgreSQL versionado em `src/db/migrations/`, queries SQL documentadas, log de auditoria versionado

### Semana 10 — GRC: o Sombra Silenciosa vira estudo de caso
- Mapeamento contra ISO 27001 Anexo A e NIST CSF
- Matriz de riscos no Excel (probabilidade x impacto)
- Política de segurança e retenção de logs considerando a LGPD
- Relatório GRC único documentando qual controle falhou no caso e o que foi corrigido, usando as queries SQL da Semana 9 como evidência (tempo até detecção, histórico de incidentes)
- **Entregável**: relatório GRC completo em `docs/grc/`

---

## Fase 3 final (v4 — Inteligência aumentada) — Semanas 11-12

**Papel simulado**: Analista de GRC / entrega final de portfólio

### Semana 11 — API, LLM Analyst, testes e CI/CD
- API FastAPI enxuta: `GET /api/events`, `GET /api/incidents` (lendo do PostgreSQL), `GET /health`, autenticação via JWT
- LLM Analyst: endpoint que recebe um caso do TheHive/PostgreSQL e retorna resumo + recomendação de ação
- Segredos fora do Git (`.env` + `.gitignore`), incluindo credenciais do PostgreSQL
- Cobertura de testes revisada, linting (ruff) e formatação (black)
- SAST com Bandit, logs estruturados em JSON
- Pipeline CI/CD completo: lint → testes → SAST → build
- Badges de status no README
- **Entregável**: API e LLM Analyst funcionando, pipeline CI/CD verde a cada push

### Semana 12 — Documentação, portfólio e apresentação final
- README final revisado, diagrama de arquitetura unindo todas as versões (Mermaid)
- Vídeo de demo (3-5 min) mostrando o Sombra Silenciosa sendo detectado e neutralizado ponta a ponta
- Artigo no LinkedIn contando a jornada; página do projeto no GitHub Pages
- **Entregável**: portfólio publicado — repositório, README final, vídeo, artigo

---

## Ritual semanal

- Cada sprint fecha com um "relatório ao board": a atualização do README/artigo daquela semana
- Commits seguem Conventional Commits (ver `CONTRIBUTING.md`)
- Board Kanban ativo o tempo todo, refletindo o estado real do trabalho
