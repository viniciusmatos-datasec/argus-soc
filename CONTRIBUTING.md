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
infra(aws): provisiona VPC e EC2 via Terraform
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

## Dados e modelos

- Datasets em `data/` e modelos em `models/` são versionados com **DVC**, não diretamente pelo Git
- Sempre rodar `dvc add` antes de `git add` para arquivos dessas pastas
