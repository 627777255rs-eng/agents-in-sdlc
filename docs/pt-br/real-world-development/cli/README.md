---
slug: pt-br/real-world-development/cli
title: "CLI do GitHub Copilot"
description: "Crie, valide e entregue duas alterações no Tailspin Toys enquanto aprende modos do Copilot CLI, personalizações, ferramentas MCP e automação de pull requests."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** coloca o GitHub Copilot no seu terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você mantenha o foco sem trocar para um editor gráfico.

O workshop segue um fluxo contínuo de trabalho no Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e conheça o ambiente.
2. Faça uma alteração específica nas classificações por estrelas, revise-a no navegador e integre manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo Plan, implemente-a no modo Autopilot e revise-a no modo interativo.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill existente `quality-checks` e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem em um navegador.
7. Crie um agente personalizado de garantia da qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise a alteração completa de filtragem e use o Agent Merge no PR de filtragem.
9. Explore comandos de barra úteis para contexto, modelos, compartilhamento e delegação opcional ao agente de nuvem.

Para manter o workshop focado, você criará dois PRs: o das classificações por estrelas e o da filtragem, com atualizações das instruções, da skill, do perfil de QA e dos testes. O fluxo de filtragem e qualidade compartilha uma conversa e uma branch para que você possa aproveitar o trabalho já feito enquanto explora cada ferramenta.

## Lições

| Lição | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI |
| [2. Adicione classificações por estrelas][ex2] | Interface | Faça uma alteração pequena e envie seu primeiro pull request |
| [3. Modos de agente][ex3] | Planejamento | Use Plan, Autopilot e modos interativos para criar recursos |
| [4. Instruções personalizadas][ex4] | Contexto | Adicione padrões persistentes para orientar o Copilot CLI |
| [5. Skills de agente][ex5] | Skills | Aprimore o Copilot com skills especializadas |
| [6. Valide com o Playwright MCP][ex6] | Ferramentas externas | Teste o recurso em um navegador real |
| [7. Agente de QA][ex7] | Agentes | Use verificações de QA combinadas com MCP |
| [8. Crie o pull request][ex8] | Entrega | Consolide, revise e integre as alterações |
| [9. Ferramentas avançadas da CLI][ex9] | Recursos da CLI | Explore comandos de barra, contexto e delegação |
| [10. Revisão][ex10] | Resumo | Revise os principais conceitos e os próximos passos |
| [Opcional: Incorpore o Foundry][foundry] | Agentes hospedados | Prepare um modelo, crie e implante um Backer Concierge baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Familiaridade básica com operações de terminal e linha de comando
- [ ] O Git instalado e configurado

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop. Portanto, você pode concluir todos os percursos com esse plano.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, verifique se o administrador habilitou o Copilot CLI para uso.

## Começar

[**Comece pela Lição 0: Pré-requisitos →**][ex0]

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[foundry]: 8-foundry-agent/
[ex10]: 10-review/
[callout-student-plan-education]: https://github.com/education/students
