<!-- 
TEMPLATE DIÁRIO (Copie e cole abaixo dessa linha sempre que iniciar um novo dia)

### 📅 Data: DD/MM/AAAA
#### 🎯 O que foi feito hoje?
- 
#### 🛠️ Ferramentas e Tecnologias
- 
#### 💻 Comandos e Códigos Úteis
- 
#### 🚧 Desafios e Perrengues
- 
---
-->

# 📓 Diário de Bordo - Argus SOC

### 📅 Data: 29/07/2026

#### 🎯 O que foi feito hoje?
- Reestruturação do projeto como simulação de empresa: definido o MSSP fictício **Argus SOC**, cliente fictício **TechNova Varejo**, e o incidente que atravessa as 12 semanas: **Operação Sombra Silenciosa**
- Definidos os papéis por fase (Engenheiro de Dados e Plataforma → Engenheiro de Detecção/Analista de SOC → Líder de Resposta a Incidentes/Analista de GRC)
- Revisão de escopo a partir de um feedback externo: incorporada a arquitetura por serviços e a divisão do projeto em **4 versões evolutivas (v1 MVP → v2 Infraestrutura → v3 Autonomia → v4 Inteligência aumentada)**, com API enxuta (FastAPI), LLM Analyst enxuto e segurança básica (JWT + `.env`); descartado o que ficaria fora do escopo de 12 semanas solo (Kafka, Qdrant, MLflow completo, Prometheus/Grafana, arquitetura de agentes)
- Instalação do Git e da GitHub CLI (`gh`) no macOS, autenticação feita com `gh auth login`
- Criação do repositório **argus-soc** (`github.com/viniciusmatos-datasec/argus-soc`), com primeiro commit e estrutura de pastas (`src/`, `tests/`, `docs/`, `infra/`, `notebooks/`, `data/`, `models/`)
- Criação e organização do board Kanban no GitHub Projects (colunas Backlog → Esta semana → Em andamento → Revisão → Feito), labels por categoria e Milestone da Semana 1
- Escrita e publicação dos documentos principais do projeto:
  - `README.md` (visão geral, arquitetura, versões v1-v4, stack, como rodar)
  - `CONTRIBUTING.md` (Conventional Commits, incluindo os tipos novos `arch`, `api`, `llm`)
  - `docs/architecture.md` (arquitetura por serviços e decisões registradas)
  - `docs/roadmap.md` (detalhamento semana a semana mapeado às versões v1-v4)
  - `docs/argus-soc-projeto.md` (documento consolidado do projeto)
  - `.gitignore` (incluindo `.DS_Store` e `.env`)
  - `.env.example` (variáveis de ambiente esperadas pelo projeto)
- Configuração do GitHub Pages com Jekyll para hospedar o blog semanal
- Criação do sistema de diário de bordo contínuo em Markdown

#### 🛠️ Ferramentas e Tecnologias
- Terminal (nano)
- Git e GitHub (GitHub CLI, GitHub Projects, GitHub Pages)
- Markdown

#### 💻 Comandos e Códigos Úteis
- `nano arquivo.md` para editar arquivos direto pelo terminal
- Fluxo completo de versionamento: `git add .` → `git commit -m "..."` → `git push`
- `gh repo create argus-soc --public --source=. --remote=origin` para criar e conectar o repositório em um único comando
- `git pull --rebase origin main` para trazer mudanças do remoto sem duplicar histórico, quando o `push` é rejeitado
- `mv env.example .env.example` para corrigir um arquivo salvo sem o ponto inicial

#### 🚧 Desafios e Perrengues
- Fiz o commit localmente mas esqueci de mandar para a nuvem — descobri na prática a importância do `git push` para efetivar as mudanças no repositório remoto
- Tentei dar `git push` antes de criar o repositório no GitHub (`gh repo create`) — o Git reclamou que o `origin` não existia
- Um `push` foi rejeitado porque o repositório remoto tinha um commit que a máquina local não tinha — resolvido com `git pull --rebase origin main`
- Um arquivo `.env.example` foi salvo sem o ponto inicial (`env.example`) — corrigido renomeando com `mv`
---
