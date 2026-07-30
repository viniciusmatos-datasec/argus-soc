# Argus SOC — Documento do Projeto

## Autonomous Security Operations Center — Portfólio em Engenharia de Dados e Segurança da Informação

---

## 1. Visão geral

**Argus SOC** é um MSSP (Managed Security Service Provider) fictício especializado em monitoramento autônomo de infraestrutura híbrida. O cliente fictício é a **TechNova Varejo**, uma rede de varejo de médio porte com exigência contratual de conformidade com LGPD, ISO 27001 e NIST CSF.

O projeto simula, na prática, a construção desse SOC do zero — da coleta de dados à resposta automática a incidentes e conformidade regulatória — amarrado por um incidente fictício que atravessa todo o projeto.

**Objetivo de carreira**: demonstrar competência prática em Data Science para conseguir um estágio nessa área, com o projeto servindo depois como ponte para Segurança da Informação.

### O caso: Operação Sombra Silenciosa

| Etapa | O que acontece |
|---|---|
| Coleta | Logs da VPN da TechNova mostram tentativas repetidas de login — possível força bruta |
| Detecção | Modelo de ML classifica o padrão como anomalia de alta severidade |
| Correlação | SIEM correlaciona com outros eventos; dashboard mostra o incidente subindo no ranking de risco |
| Resposta | Playbook SOAR age sozinho: bloqueia IP, abre caso, notifica |
| Conformidade | Incidente vira estudo de caso no relatório GRC |
| Encerramento | Caso de destaque no vídeo de demonstração e no artigo publicado |

---

## 2. Arquitetura por serviços

Em vez de olhar o projeto só como uma sequência de semanas, esta é a visão de como os serviços se encaixam:

```
Collectors (Wazuh / Sysmon)
        │
        ▼
Data Lake (data/raw, data/processed — versionado com DVC)
        │
        ▼
Feature Store (features de detecção — versionadas com DVC)
        │
        ▼
ML Service (scikit-learn + SAS Viya)
        │
        ▼
Detection Engine (SIEM — ELK Stack)
        │
        ▼
SOAR (TheHive + Shuffle)
        │
        ▼
LLM Analyst (resumo e recomendação sobre o incidente)
        │
        ▼
Dashboard (Power BI + Databricks)
        │
        ▼
API (FastAPI) ──► Portal (GitHub Pages)
```

**Decisão registrada**: não há um Message Broker (Kafka/RabbitMQ) entre os Collectors e o Data Lake — os dados fluem via pipeline batch. Isso é proposital: um broker adiciona complexidade operacional que não se justifica no escopo de 12 semanas solo. Fica documentado como possível evolução futura, não como lacuna.

Essa arquitetura está detalhada com mais profundidade em `docs/architecture.md`.

---

## 3. Estrutura em versões evolutivas (v1 → v4)

Em vez de depender que as ~20 tecnologias estejam todas prontas ao mesmo tempo, o projeto é entregue em versões, cada uma funcional e demonstrável por si só:

### v1 — MVP (Fase 1, Semanas 1-4)
ETL + Wazuh + Machine Learning básico + Dashboard simples.
- Coleta de logs, pipeline de dados, primeira versão do dataset
- Já suficiente para mostrar o pipeline ponta a ponta funcionando

### v2 — Infraestrutura (Fase 2, Semanas 5-7)
ELK Stack + Terraform + Docker + Cloud (AWS/Azure).
- SIEM real, dashboards executivos (Power BI/Databricks)
- Detecção de anomalias com scikit-learn e SAS Viya

### v3 — Autonomia (Fase 2 final + Fase 3, Semanas 8-10)
SOAR + automação + GRC + Multi-Cloud.
- Resposta automática a incidentes
- Conformidade documentada (ISO 27001, NIST CSF, LGPD)

### v4 — Inteligência aumentada (Fase 3 final, Semanas 11-12)
LLM enxuto + API + observabilidade leve + portfólio final.
- Endpoint que resume o incidente e recomenda ação (não um sistema de agentes completo)
- API simples servindo o dashboard
- Testes, CI/CD, documentação final

Essa divisão resolve o maior risco do escopo original: se o tempo apertar, cada versão entrega algo completo e funcional — nunca um conjunto de peças pela metade.

---

## 4. O que foi incorporado do feedback externo (e o que ficou de fora)

Um feedback recebido de outra IA trouxe boas sugestões de "deixar o projeto mais corporativo". Delas, foi decidido:

**Incorporado:**
- Arquitetura por serviços (seção 2)
- Versões evolutivas v1-v4 (seção 3)
- Métricas de qualidade mensuráveis (seção 6)
- API enxuta com FastAPI (2-3 endpoints, não uma API completa)
- LLM enxuto (um endpoint de resumo/recomendação, não uma arquitetura de agentes)
- Segurança básica: autenticação simples (JWT) e segredos fora do Git (`.env` + `.gitignore`)

**Deixado de fora (fora do escopo de 12 semanas solo):**
- Message Broker (Kafka/RabbitMQ)
- Banco vetorial (Qdrant) com embeddings de MITRE/CVE/IOC
- MLOps completo (MLflow, registry, retreinamento automático)
- Observabilidade completa (Prometheus/Grafana, tracing distribuído)
- Arquitetura formal baseada em agentes nomeados
- RBAC, MFA, Vault, SBOM, criptografia avançada

---

## 5. Papéis por fase

| Fase | Versão | Papel na Argus |
|---|---|---|
| Fase 1 (S1-4) | v1 | Engenheiro de Dados e Plataforma |
| Fase 2 (S5-8) | v2 → v3 | Engenheiro de Detecção / Analista de SOC |
| Fase 3 (S9-12) | v3 → v4 | Líder de Resposta a Incidentes / Analista de GRC |

---

## 6. Métricas de qualidade

| Métrica | Meta |
|---|---|
| Cobertura de testes (pytest) | > 90% |
| Formatação (black) | 100% do código |
| Linting (ruff) | 0 erros |
| SAST (Bandit) | 0 vulnerabilidades High |
| Tempo de resposta da API | < 200 ms |
| Tempo de inferência do modelo | < 1 s |
| Taxa de falsos positivos (detecção) | < 5% |

---

## 7. Stack completa

| Categoria | Tecnologias |
|---|---|
| Linguagens | Python, DAX |
| Dados | pandas, openpyxl, PySpark, Parquet, DVC |
| Machine Learning | scikit-learn, SAS Viya |
| SIEM / Segurança | Wazuh, Elastic Stack (Elasticsearch, Logstash, Kibana), TheHive, Shuffle SOAR |
| Cloud / Infra | Terraform, AWS, Azure, GCP, Docker |
| BI / Dashboards | Power BI, Databricks |
| API | FastAPI |
| IA generativa | LLM enxuto (resumo/recomendação de incidentes) |
| GRC | ISO 27001, NIST CSF, LGPD |
| Qualidade | pytest, ruff, black, Bandit, GitHub Actions |
| Versionamento | Git (código), DVC (dados e modelos) |

---

## 8. Cadência de trabalho (metodologia)

- Sprints semanais organizados em board Kanban no GitHub Projects (`Backlog → Esta semana → Em andamento → Revisão → Feito`)
- Cada semana fecha com um "relatório ao board" — documentado em `docs/` e resumido em artigos publicados no LinkedIn
- Commits seguem Conventional Commits (`CONTRIBUTING.md`)
- Milestones do GitHub correspondem a cada semana do roadmap
- Datasets e modelos versionados com DVC, separadamente do código

---

## 9. Estrutura do repositório

```
argus-soc/
├── src/            # código-fonte (ETL, ML, playbooks, API)
├── tests/          # testes automatizados (pytest)
├── notebooks/       # notebooks Jupyter (ETL, análise de ML)
├── infra/          # Terraform (aws/, azure/, gcp/)
├── data/           # dados brutos e processados (versionados com DVC)
├── models/         # modelos de ML treinados (versionados com DVC)
├── docs/           # roadmap, architecture.md, GRC, ADRs
├── README.md
└── CONTRIBUTING.md
```

---

## 10. Próximos passos

1. Criar `docs/architecture.md` com o diagrama completo da seção 2
2. Reorganizar o backlog de Issues do GitHub Projects em torno das versões v1-v4 (em vez de só semanas)
3. Adicionar `.gitignore` e `.env.example` ao repositório
4. Seguir a execução prática a partir da Semana 1 (v1 — MVP)
