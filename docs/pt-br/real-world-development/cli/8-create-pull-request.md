---
title: "Lição 8 - Crie e integre o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Sua implementação de filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você integrou pessoalmente o pull request (PR) das classificações por estrelas; desta vez, usará o **Agent Merge** para gerenciar o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações de instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma única conversa e branch e incluirá todo esse trabalho neste PR.

Nesta lição, você:

- aprenderá o que é o Agent Merge e como ele automatiza o ciclo de integração;
- inspecionará todas as alterações do recurso e as evidências de verificação;
- criará o PR da filtragem;
- habilitará o Agent Merge somente depois da revisão e confirmará que o PR foi integrado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização para integrá-lo sob o controle de quem desenvolve.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para integrar um pull request. Quando você o habilita, o Copilot resolve o que estiver bloqueando o PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o auto-merge do GitHub se o repositório permitir.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código pronto, vamos revisar as alterações, criar o PR e deixar o Agent Merge gerenciar o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agente digitando `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um novo PR usando o comando `/pr create`.
5. Ative o Agent Merge com o comando `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando tudo for concluído com sucesso, ele fará a integração.
7. Confirme se você recebeu uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e, se o repositório permitir, faça manualmente a integração após a revisão.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração, teste e validação de código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de integração;
- inspecionou todas as alterações do recurso e as evidências de verificação;
- criou o PR da filtragem;
- habilitou o Agent Merge somente após a revisão e confirmou que o PR foi integrado.

Na próxima lição, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
