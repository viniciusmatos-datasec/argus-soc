# 🛡️ Argus SOC — Autonomous Security Operations Center

## 📖 Visão Geral
O **Argus SOC** é um projeto prático de portfólio focado em Engenharia de Dados e Segurança da Informação. Ele simula as operações de um MSSP (Managed Security Service Provider) fictício responsável pelo monitoramento autônomo da infraestrutura híbrida de uma rede de varejo, a TechNova.

O objetivo principal é demonstrar a construção de um SOC autônomo do zero, passando por coleta de logs, detecção de anomalias com Machine Learning, resposta automática a incidentes (SOAR) e conformidade com frameworks de GRC (LGPD, ISO 27001 e NIST CSF).

O projeto é guiado pelo caso investigativo "Operação Sombra Silenciosa", um incidente simulado que evolui à medida que novas camadas de segurança e dados são implementadas.

## 🏗️ Fases e Arquitetura

O desenvolvimento está dividido em três fases principais:

1. **Fase 1 — Fundação (Engenharia de Dados e Plataforma):**
   - Coleta de logs de segurança (Wazuh).
   - Pipeline ETL com Python e Pandas.
   - Versionamento de dados com DVC.
   - Infraestrutura como código (Terraform na AWS).

2. **Fase 2 — Inteligência (Engenharia de Detecção):**
   - Detecção de anomalias via Machine Learning (Isolation Forest e LOF com scikit-learn).
   - Centralização de logs e SIEM com ELK Stack (Azure).
   - Dashboards executivos (Power BI e Databricks).
   - Implantação Multi-Cloud (AWS, Azure e GCP).

3. **Fase 3 — Autonomia (Resposta a Incidentes e GRC):**
   - SOAR (TheHive + Shuffle) para resposta automática.
   - Relatórios GRC documentando falhas e controles (ISO 27001 e NIST CSF).
   - CI/CD, testes (pytest) e análise estática (Bandit).

## 🛠️ Stack Tecnológica

- **Linguagem:** Python
- **Dados & ML:** Pandas, Scikit-Learn, PySpark, DVC, Databricks
- **Segurança:** Wazuh, ELK Stack, TheHive, Shuffle
- **Infraestrutura:** Docker, Terraform, AWS, Azure, GCP
- **Visualização:** Power BI, Excel

## 🚀 Como rodar localmente

### Pré-requisitos
- Git
- Python 3.10+
- Docker e Docker Compose (para simular a infra localmente)
- Conta ativa na AWS/Azure/GCP (para a infraestrutura em nuvem gerada via Terraform)

### Passos iniciais
1. Clone o repositório:
   ```bash
   git clone [https://github.com/viniciusmatos-datasec/argus-soc.git](https://github.com/viniciusmatos-datasec/argus-soc.git)
