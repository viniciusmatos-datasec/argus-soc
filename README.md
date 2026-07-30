# Argus SOC — Autonomous Security Operations Center

> Simulação prática de um SOC (Security Operations Center) autônomo, construído do zero como projeto de portfólio em Engenharia de Dados e Segurança da Informação.

![status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)
![python](https://img.shields.io/badge/python-3.11-blue)
![license](https://img.shields.io/badge/license-MIT-green)

---

## 📖 Sobre o projeto

**Argus SOC** é um MSSP (Managed Security Service Provider) fictício especializado em monitoramento autônomo de infraestrutura híbrida. Este repositório simula, na prática, a construção desse SOC do zero para um cliente fictício — a **TechNova Varejo**, uma rede de varejo de médio porte com exigência contratual de conformidade com LGPD, ISO 27001 e NIST CSF.

O projeto cobre a jornada completa de um SOC moderno: coleta e engenharia de dados, detecção de anomalias com Machine Learning, SIEM, dashboards executivos, infraestrutura multi-cloud, resposta automática a incidentes (SOAR) e governança, risco e conformidade (GRC) — tudo amarrado por um incidente fictício que atravessa o projeto do início ao fim.

### O caso: Operação Sombra Silenciosa

Um padrão de tentativas repetidas de login na VPN da TechNova é detectado, investigado, classificado como ameaça real por um modelo de Machine Learning, correlacionado no SIEM, neutralizado automaticamente por um playbook SOAR, e documentado como estudo de caso no relatório de conformidade. Esse incidente serve de fio condutor para todas as fases do projeto.

## 🎯 Objetivo

Construir, em 12 semanas, um portfólio técnico robusto que demonstre competência prática em:

- Engenharia de dados (ETL, pipelines, versionamento)
- Data Science aplicado a segurança (detecção de anomalias, dashboards)
- Infraestrutura como código e multi-cloud
- Segurança ofensiva/defensiva aplicada (SIEM, SOAR)
- Governança, risco e conformidade (GRC)
- Boas práticas de engenharia de software (testes, CI/CD, logs de auditoria)

## 🗺️ Roadmap

| Fase | Semanas | Foco | Papel simulado |
|---|---|---|---|
| **Fase 1 — Fundação** | 1-4 | Dados, infraestrutura e agilidade | Engenheiro de Dados e Plataforma |
| **Fase 2 — Inteligência** | 5-8 | ML, SIEM, Cloud e Dashboards | Engenheiro de Detecção / Analista de SOC |
| **Fase 3 — Autonomia** | 9-12 | SOAR, GRC e Portfólio | Líder de Resposta a Incidentes / Analista de GRC |

O detalhamento semana a semana está documentado em [`docs/roadmap.md`](docs/roadmap.md).

## 🏗️ Arquitetura

```
Coleta de Logs (Wazuh/Sysmon)
        │
        ▼
  Pipeline ETL (Python/pandas) ──► Excel / Parquet
        │
        ▼
 Detecção de Anomalias (scikit-learn + SAS Viya)
        │
        ▼
    SIEM (ELK Stack na Azure)
        │
        ▼
 Dashboard Executivo (Power BI + Databricks)
        │
        ▼
  SOAR — Resposta Automática (TheHive + Shuffle)
        │
        ▼
  GRC — Conformidade (ISO 27001 / NIST CSF / LGPD)
```

Infraestrutura provisionada via Terraform em **AWS + Azure + GCP** (multi-cloud).

## 🛠️ Stack

| Categoria | Tecnologias |
|---|---|
| Linguagens | Python, DAX |
| Dados | pandas, openpyxl, PySpark, Parquet, DVC |
| Machine Learning | scikit-learn, SAS Viya |
| SIEM / Segurança | Wazuh, Elastic Stack (Elasticsearch, Logstash, Kibana), TheHive, Shuffle SOAR |
| Cloud / Infra | Terraform, AWS, Azure, GCP, Docker |
| BI / Dashboards | Power BI, Databricks |
| GRC | ISO 27001, NIST CSF, LGPD |
| Qualidade | pytest, ruff, Bandit, GitHub Actions |

## 📂 Estrutura do repositório

```
argus-soc/
├── src/            # código-fonte (ETL, ML, playbooks)
├── tests/          # testes automatizados (pytest)
├── notebooks/      # notebooks Jupyter (ETL, análise de ML)
├── infra/          # Terraform (aws/, azure/, gcp/)
├── data/           # dados brutos e processados (versionados com DVC)
├── models/         # modelos de ML treinados (versionados com DVC)
├── docs/           # roadmap, relatório GRC, ADRs
└── README.md
```

## 🚀 Como rodar localmente

```bash
git clone https://github.com/viniciusmatos-datasec/argus-soc.git
cd argus-soc

python -m venv venv
source venv/bin/activate

pip install -r requirements.txt
```

> Instruções detalhadas de cada módulo (pipeline de dados, modelo de ML, infraestrutura) serão adicionadas conforme cada semana for implementada.

## 📋 Metodologia

O projeto segue metodologia ágil, com sprints semanais organizados em um board Kanban (GitHub Projects). Cada semana fecha com um "relatório ao board", documentado em [`docs/`](docs/) e resumido em artigos publicados no LinkedIn.

Commits seguem o padrão [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) — detalhes em [`CONTRIBUTING.md`](CONTRIBUTING.md).

## 👤 Autor

**Vinicius Matos** — estudante de Segurança da Informação (FATEC Santana de Parnaíba), em transição para Data Science.

- [LinkedIn](#)
- [GitHub](https://github.com/viniciusmatos-datasec)

---

*Projeto em desenvolvimento — acompanhe o progresso semana a semana nas [Issues](../../issues) e [Projects](../../projects) deste repositório.*
