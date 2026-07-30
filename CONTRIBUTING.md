# Contribuindo com o Argus SOC

Este documento define as práticas de contribuição usadas no projeto — mesmo sendo um projeto solo, simula o fluxo de trabalho de uma equipe real na Argus SOC.

## Padrão de commits: Conventional Commits

Todo commit segue o formato:

```
<tipo>(<escopo opcional>): <descrição curta>
```

### Tipos usados no projeto

| Tipo | Quando usar |
|---|---|
| `feat` | Nova funcionalidade (ex: novo script de ETL, novo playbook) |
| `fix` | Correção de bug |
| `docs` | Mudanças em documentação (README, roadmap, GRC) |
| `arch` | Mudanças na arquitetura ou em documentos estruturais do projeto (ex: `docs/architecture.md`) |
| `api` | Mudanças na camada FastAPI (endpoints, autenticação, contratos) |
| `llm` | Mudanças no serviço de LLM Analyst (prompt, integração, resumo/recomendação) |
| `sql` | Mudanças no schema, queries ou migrações do PostgreSQL |
| `test` | Adição ou ajuste de testes |
| `chore` | Tarefas de manutenção (estrutura de pastas, configs, dependências) |
| `refactor` | Reorganização de código sem mudar comportamento |
| `ci` | Mudanças em pipelines de CI/CD (GitHub Actions) |
| `infra` | Mudanças em Terraform / infraestrutura |
| `data` | Mudanças em datasets versionados (DVC) |

### Exemplos usados neste projeto

```
feat(etl): adiciona pipeline de limpeza dos logs da Semana 3
fix(ml): corrige normalização de features antes do Isolation Forest
docs(grc): adiciona mapeamento de controles ISO 27001
arch: adiciona docs/architecture.md com visão por serviços
infra(aws): provisiona VPC e EC2 via Terraform
api(events): adiciona endpoint GET /api/events
llm(analyst): integra chamada de resumo de incidente
sql(incidents): cria tabelas incidents e alerts no PostgreSQL
test(scoring): adiciona testes para função de scoring do modelo
chore: estrutura inicial de pastas do projeto
data: versiona novo lote de logs processados com DVC
```

## Fluxo de trabalho

1. Cada tarefa do roadmap vira uma **Issue** no board do GitHub Projects, vinculada ao Milestone da semana correspondente
2. Trabalho é feito em uma branch separada da `main`, nomeada como `semana-N/descricao-curta` (ex: `semana-5/deteccao-anomalias`)
3. Ao concluir, abrir um **Pull Request** para a `main`, referenciando a Issue (ex: `Closes #12`)
4. O Pull Request só é mesclado (`merge`) depois que o pipeline de CI (lint + testes) estiver verde
5. Após o merge, a Issue é movida para "Feito" no board

## Branches

- `main` — código estável, sempre funcional
- `semana-N/*` — branches de trabalho por semana/tarefa

## Estilo de código

- Python formatado com `black` e revisado com `ruff`
- Testes obrigatórios para qualquer função em `src/` (pytest)
- Segurança do código verificada com `bandit` antes de cada merge relevante
- Segredos (chaves de API, tokens, credenciais do PostgreSQL) nunca commitados — sempre via `.env`, com `.env.example` documentando as variáveis esperadas

## Dados e modelos

- Datasets em `data/` e modelos em `models/` são versionados com **DVC**, não diretamente pelo Git
- Sempre rodar `dvc add` antes de `git add` para arquivos dessas pastas

## Banco de dados

- Alterações de schema do PostgreSQL (`incidents`, `alerts`) ficam em `src/db/migrations/`, versionadas no Git normalmente (são código, não dados)
- Toda migração nova é um commit `sql:` separado, nunca misturado com `feat`/`fix`
