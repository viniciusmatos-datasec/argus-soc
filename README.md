# Argus SOC — Autonomous Security Operations Center

> Simulação prática de um SOC (Security Operations Center) autônomo, construído do zero como projeto de portfólio em Engenharia de Dados e Segurança da Informação.

![status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)
![python](https://img.shields.io/badge/python-3.11-blue)
![license](https://img.shields.io/badge/license-MIT-green)

---

## 📖 Sobre o projeto

**Argus SOC** é um MSSP (Managed Security Service Provider) fictício especializado em monitoramento autônomo de infraestrutura híbrida. Este repositório simula, na prática, a construção desse SOC do zero para um cliente fictício — a **TechNova Varejo**, uma rede de varejo de médio porte com exigência contratual de conformidade com LGPD, ISO 27001 e NIST CSF.

O projeto cobre a jornada completa de um SOC moderno: coleta e engenharia de dados, detecção de anomalias com Machine Learning, SIEM, dashboards executivos, infraestrutura multi-cloud, resposta automática a incidentes (SOAR), uma camada enxuta de IA generativa e governança, risco e conformidade (GRC) — tudo amarrado por um incidente fictício que atravessa o projeto do início ao fim.

### O caso: Operação Sombra Silenciosa

Um padrão de tentativas repetidas de login na VPN da TechNova é detectado, investigado, classificado como ameaça real por um modelo de Machine Learning, correlacionado no SIEM, neutralizado automaticamente por um playbook SOAR, e documentado como estudo de caso no relatório de conformidade. Esse incidente serve de fio condutor para todas as fases do projeto.

## 🎯 Objetivo

Construir um portfólio técnico robusto que demonstre competência prática em:

- Engenharia de dados (ETL, pipelines, versionamento)
- Data Science aplicado a segurança (detecção de anomalias, dashboards)
- Infraestrutura como código e multi-cloud
- Segurança ofensiva/defensiva aplicada (SIEM, SOAR)
- IA generativa aplicada a análise de incidentes
- Governança, risco e conformidade (GRC)
- Boas práticas de engenharia de software (testes, CI/CD, logs de auditoria)

## 🏗️ Arquitetura

```
Collectors (Wazuh/Sysmon)
        │
        ▼
   Data Lake (DVC)
        │
        ▼
  Feature Store (DVC)
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
   LLM Analyst (resumo + recomendação)
        │
        ▼
Dashboard (Power BI + Databricks)
        │
        ▼
     API (FastAPI) ──► Portal (GitHub Pages)
```

Infraestrutura provisionada via Terraform em **AWS + Azure + GCP** (multi-cloud). A descrição completa de cada serviço e as decisões de arquitetura registradas (o que foi incluído e o que foi deixado de fora, e por quê) estão em [`docs/architecture.md`](docs/architecture.md).

## 🧩 Versões evolutivas (v1 → v4)

Em vez de depender que todas as tecnologias estejam prontas ao mesmo tempo, o projeto é entregue em versões — cada uma funcional e demonstrável por si só:

| Versão | Foco | Fase |
|---|---|---|
| **v1 — MVP** | ETL + Wazuh + ML básico + Dashboard simples | Fase 1 (S1-4) |
| **v2 — Infraestrutura** | ELK Stack + Terraform + Docker + AWS/Azure | Fase 2 (S5-7) |
| **v3 — Autonomia** | SOAR + GRC + Multi-Cloud | Fase 2 final + Fase 3 (S8-10) |
| **v4 — Inteligência aumentada** | LLM enxuto + API + observabilidade leve + portfólio final | Fase 3 final (S11-12) |

Detalhamento completo em [`docs/argus-soc-projeto.md`](docs/argus-soc-projeto.md).

## 🗺️ Roadmap semanal

| Fase | Semanas | Papel simulado |
|---|---|---|
| **Fase 1 — Fundação** | 1-4 | Engenheiro de Dados e Plataforma |
| **Fase 2 — Inteligência** | 5-8 | Engenheiro de Detecção / Analista de SOC |
| **Fase 3 — Autonomia** | 9-12 | Líder de Resposta a Incidentes / Analista de GRC |

O detalhamento semana a semana está em [`docs/roadmap.md`](docs/roadmap.md).

## 🛠️ Stack

| Categoria | Tecnologias |
|---|---|
| Linguagens | Python, DAX |
| Dados | pandas, openpyxl, PySpark, Parquet, DVC |
| Machine Learning | scikit-learn, SAS Viya |
| SIEM / Segurança | Wazuh, Elastic Stack (Elasticsearch, Logstash, Kibana), TheHive, Shuffle SOAR |
| Cloud / Infra | Terraform, AWS, Azure, GCP, Docker |
| BI / Dashboards | Power BI, Databricks |
| API | FastAPI |
| IA generativa | LLM Analyst (resumo/recomendação de incidentes) |
| GRC | ISO 27001, NIST CSF, LGPD |
| Qualidade | pytest, ruff, black, Bandit, GitHub Actions |
| Versionamento | Git (código), DVC (dados e modelos) |

## 📊 Critérios de qualidade

| Métrica | Meta |
|---|---|
| Cobertura de testes (pytest) | > 90% |
| Linting (ruff) | 0 erros |
| SAST (Bandit) | 0 vulnerabilidades High |
| Tempo de resposta da API | < 200 ms |
| Taxa de falsos positivos | < 5% |

## 📂 Estrutura do repositório

```
argus-soc/
├── src/            # código-fonte (ETL, ML, playbooks, API)
├── tests/          # testes automatizados (pytest)
├── notebooks/      # notebooks Jupyter (ETL, análise de ML)
├── infra/          # Terraform (aws/, azure/, gcp/)
├── data/           # dados brutos e processados (versionados com DVC)
├── models/         # modelos de ML treinados (versionados com DVC)
├── docs/           # roadmap, architecture.md, projeto.md, GRC, ADRs
├── README.md
└── CONTRIBUTING.md
```

## 🚀 Como rodar localmente

```bash
git clone https://github.com/viniciusmatos-datasec/argus-soc.git
cd argus-soc

python -m venv venv
source venv/bin/activate

pip install -r requirements.txt
```

> Instruções detalhadas de cada módulo (pipeline de dados, modelo de ML, infraestrutura, API) serão adicionadas conforme cada versão for implementada.

## 📋 Metodologia

O projeto segue metodologia ágil, com sprints semanais organizados em um board Kanban (GitHub Projects). Cada semana fecha com um "relatório ao board", documentado em [`docs/`](docs/) e resumido em artigos publicados no LinkedIn.

Commits seguem o padrão [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) — detalhes em [`CONTRIBUTING.md`](CONTRIBUTING.md).

## 👤 Autor

**Vinicius Matos** — estudante de Segurança da Informação (FATEC Santana de Parnaíba), em transição para Data Science.

- [LinkedIn](https://www.linkedin.com/in/vinicius-matos-b50470426/)
- [GitHub](https://github.com/viniciusmatos-datasec)

---

*Projeto em desenvolvimento — acompanhe o progresso semana a semana nas [Issues](../../issues) e [Projects](../../projects) deste repositório.*
