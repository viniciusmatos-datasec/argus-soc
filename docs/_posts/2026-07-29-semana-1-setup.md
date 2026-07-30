---
layout: post
title: "Semana 1: Setup e Metodologia Ágil no Argus SOC"
date: 2026-07-29
categories: [Fase 1, Setup]
---

## O Início da Jornada

Nesta primeira semana, iniciamos oficialmente a construção do **Argus SOC**, um laboratório prático que simula um Managed Security Service Provider (MSSP). O objetivo desta fase de fundação é preparar o terreno para a coleta de dados e monitoramento da nossa cliente fictícia, a TechNova Varejo.

Antes de escrever qualquer linha de código voltada para segurança ou dados, o foco foi estabelecer processos de engenharia de software sólidos. 

## 🛠️ O que foi construído

1. **Estrutura Profissional de Repositório:** 
   Criamos a arquitetura de pastas que suportará as 12 semanas de projeto (`src/`, `docs/`, `infra/`, `notebooks/`, `data/`, `models/`), deixando tudo pronto para os pipelines de ETL e Machine Learning.

2. **Padrão de Qualidade:**
   Implementamos o guia `CONTRIBUTING.md` focado no padrão *Conventional Commits*. Isso garante um histórico rastreável (ex: `feat:`, `docs:`, `chore:`), algo essencial em times reais de Engenharia de Dados e Segurança.

3. **Metodologia Ágil:**
   Configuramos um Board Kanban no GitHub Projects (Backlog, Em andamento, Revisão, Feito). Todo o backlog das 12 semanas foi mapeado em cards, dando total visibilidade ao roteiro do projeto.

## 🧠 Aprendizados da Semana

* Entender a diferença entre apenas "subir arquivos no GitHub" e estruturar um projeto escalável.
* Utilizar a linha de comando para gerenciar a criação do repositório remoto via `gh repo create` e lidar com os fluxos do Git.
* A importância de planejar antes de executar: o board Kanban reduz a ansiedade de tentar fazer tudo ao mesmo tempo.

## 🔜 Próximos Passos

Para a Semana 2, entraremos na parte prática de segurança com o início da **Operação Sombra Silenciosa**. O objetivo será instalar o SIEM (Wazuh) e começar a gerar/coletar os primeiros logs simulando ataques de força bruta na VPN.
