---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar contexto, selecionar modelos, compartilhar sessões e delegar opcionalmente ao agente de nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra para interagir com ele. Esses comandos expõem funcionalidades avançadas, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você:

- usará `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usará `/model` para explorar os modelos disponíveis;
- aprenderá como `/share` pode exportar ou compartilhar uma sessão;
- explorará comandos opcionais para trabalho em paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal da CLI. Agora, vamos explorar outros recursos: gerenciamento de contexto, troca de modelos, compartilhamento de sessões e delegação opcional ao [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Ao trabalhar em tarefas grandes ou complexas, você pode alcançar o limite da janela de contexto do modelo. Quando necessário, o Copilot CLI compacta a conversa automaticamente. Você também pode inspecionar ou compactar o contexto manualmente usando comandos de barra.

1. Volte ao Codespace e inicie o Copilot CLI a partir da raiz do repositório, se ele ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a distribuição do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. A diferença pode ser pequena se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela se enche. Use `/compact` quando quiser escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação com a atual e quiser iniciar uma conversa limpa.

## Escolha um modelo

Modelos diferentes têm pontos fortes diferentes, e cada pessoa desenvolvedora pode ter preferências próprias. O Copilot CLI permite listar e selecionar o modelo desejado.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todas as pessoas a usar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e revise as opções de `/share` disponíveis na sua versão.
2. Se quiser compartilhar esta sessão, escolha o destino mais apropriado, como `/share file` para exportá-la como um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações de sessões podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretenda compartilhar.

## Opcional: amplie o trabalho ou delegue

O workshop principal está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma worktree isolada do Git para uma tarefa separada.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou trabalho remoto. Antes de experimentá-los, comece uma tarefa nova e com escopo bem definido e revise os resultados pelo fluxo de trabalho normal. Para explorar mais o trabalho assíncrono com agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Com os comandos de barra do Copilot CLI, você pode configurá-lo, compartilhar sessões e entender o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usou `/model` para explorar os modelos disponíveis;
- aprendeu como `/share` pode exportar ou compartilhar uma sessão;
- explorou comandos opcionais para trabalho em paralelo, worktrees e delegação ao agente de nuvem.

Há muitos outros comandos de barra e recursos para explorar no Copilot CLI! Vamos encerrar nossa jornada [revisando o que aprendemos][next-lesson] e os próximos passos para continuar aprendendo.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
