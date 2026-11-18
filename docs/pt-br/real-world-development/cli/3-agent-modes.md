---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o modo Plan para concordar com uma abordagem, o modo Autopilot para implementar filtros a partir de uma issue e o modo interativo para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. No entanto, alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi desenvolvido para funcionar com o fluxo de trabalho já usado pela organização e ajudar a criar as coisas certas da maneira correta. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: começará com uma issue para gerar um novo recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará a integração bem-sucedida ao projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo de trabalho do recurso. Normalmente, você usaria branches ou PRs diferentes para os vários tipos de arquivos, mas vamos simplificar para manter o foco nos conceitos principais.

Para começar, nesta lição você:

- iniciará uma nova conversa do Copilot a partir de uma issue do GitHub.
- definirá os requisitos no modo Plan.
- implementará o novo recurso usando o modo Autopilot.
- revisará o código.
- validará manualmente o recurso em um navegador encaminhado.

Ao continuar este trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como combinar categorias precisam ser acordados antes da programação. Você usará o modo Plan para resolver essas decisões e, em seguida, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

Introduzir agentes de programação de IA no seu fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria dos desenvolvedores segue um fluxo semelhante a este:

1. Começar com uma issue registrada que descreva o que precisa ser feito.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com sucesso, integrar o código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos fluxos segue alguma variação do processo acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e faz iterações até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Comece pela issue

Antes de começar a filtragem, volte ao Codespace e verifique se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme se o PR das classificações por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que agora o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite trazer uma issue ou um pull request (PR) do GitHub para o contexto.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem para implementar um recurso ou realizar uma tarefa antes de delegá-la ao Copilot. Para tarefas complexas, é sempre bom dedicar algum tempo ao planejamento. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt a seguir:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começa a elaborar o plano! Ele explora o projeto e, em seguida, determina a melhor abordagem.

3. Durante esse processo, o Copilot pode fazer perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências. Não há respostas erradas!
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a tela para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo! Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere na solução até considerar que a tarefa está concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com rótulo semelhante na versão instalada.
2. Confirme que o indicador abaixo do prompt exibe **Autopilot**.
3. Observe enquanto o Copilot segue o plano estabelecido, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, ele precisa ser revisado antes da integração, como qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador do modo não exibe **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação dos filtros e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise a saída das verificações do projeto e peça ao Copilot que resolva as falhas.

## Explore a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente os filtros de categoria e editora, além das combinações acordadas no plano.
4. Confirme se os comportamentos de redefinição dos filtros e de resultados vazios correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o novo recurso usando o modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Na próxima lição, vamos nos aprofundar em como o código é gerado e garantir que ele siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
