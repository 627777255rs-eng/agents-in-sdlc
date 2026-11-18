---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de determinada maneira — ou há informações de contexto que o Copilot deve conhecer —, esse contexto precisa estar disponível. Há várias formas de fornecer contexto específico ao Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que permitem orientar a geração de código.

Neste exercício, você:

- explorará como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos de instruções com escopo de caminho;
- gerará a primeira parte dos dados para filtragem (um auxiliar de editoras) usando as instruções atuais;
- adicionará um novo padrão para todo o repositório em `.github/copilot-instructions.md`;
- enviará novamente o mesmo prompt e verá o código gerado adotar o novo padrão;
- confirmará as alterações nas instruções e na parte de filtragem na branch `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode divergir de alguns padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após a atualização das instruções, não obter um resultado idêntico em todos os detalhes.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Eles incluem:

- A camada de dados sempre precisa ter testes de unidade.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código como comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo de programação e os requisitos. Esse recurso ajuda a orientar as sugestões e os trechos de código. Você pode especificar convenções de programação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Esse arquivo deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações feitas ao Copilot, como a pilha de tecnologias, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações globais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou um novo conjunto de testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados somente para a geração de código no Copilot Chat — não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto o arquivo de instruções do repositório quanto os arquivos `*.instructions.md` com o campo `applyTo` no frontmatter ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha o conteúdo de `copilot-instructions.md` focado em orientações para todo o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões gerais de programação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo (testes de unidade, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve e não deve ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa funcionalidade de uma maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI) — a lição do ambiente em uso explicará como fazer isso quando for relevante.
>
> Está procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório: há um arquivo principal, `copilot-instructions.md`, e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Na seção **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Essas informações se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste exemplo, ele corresponde a qualquer arquivo de teste TypeScript, como os que correspondem a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e arquivos existentes do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar ao Copilot exemplos para seguir ao gerar código. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Crie uma branch para as alterações

Vamos seguir as práticas recomendadas e criar uma branch para as alterações.

1. Volte ao Codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma nova branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício — cada etapa indicará o que usar quando necessário.

Para observar o impacto das instruções personalizadas, envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para que o Copilot saiba onde o auxiliar deve ficar.
3. Na lista suspensa de agentes da visualização Chat, selecione **Agent** para que o Copilot aplique alterações nos arquivos.

   ![Captura de tela mostrando o seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt a seguir:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações no código, muitas vezes abrangendo o arquivo auxiliar e os testes.
6. Observe que a função proposta é tipada, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (que se aplica a `src/lib/*.ts`).
7. Observe que o código proposto **não tem** comentários TSDoc nem um cabeçalho de comentários no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo é observar a *consistência* depois da atualização das instruções.

## Adicione um novo padrão para o repositório

Como mencionado, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de programação do projeto, mas ainda não tem uma regra para documentação no código — por isso, o auxiliar gerado não tinha comentários de documentação.
3. Logo abaixo dos padrões existentes, adicione as linhas Markdown a seguir para instruir o Copilot a incluir cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para definir orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões válidos para todo o projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Reenvie o prompt e observe a alteração

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o histórico.
2. Volte a `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que agora o arquivo proposto começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo a código real que servirá de base para o próximo exercício.

## Confirme as instruções e envie a branch

Os arquivos de instruções são recursos do projeto e devem ser gerenciados pelo controle de versão, como qualquer outro arquivo. Vamos confirmar as alterações e enviar a branch ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as alterações nas instruções e no auxiliar estão presentes executando:

   ```bash
   git status
   ```

3. Prepare e confirme as alterações nas instruções e todos os arquivos modificados pelo Copilot para criar a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. No terminal, envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e integre um pull request

Com a branch publicada, vamos criar um pull request e vinculá-lo à issue de padrões de documentação no backlog. Poderíamos fazer isso manualmente, mas o Copilot pode realizar a tarefa usando as ferramentas do GitHub já conectadas ao projeto. Vamos pedir que encontre a issue, crie um PR para fechá-la e, em seguida, integre-o.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub em seu nome. O template do projeto já conecta as ferramentas do GitHub ao espaço de trabalho, então não há nada para configurar. Na primeira vez que o Copilot usar uma delas, o VS Code talvez solicite que você entre na sua conta do GitHub. Siga as instruções para permitir o acesso. Em um exercício posterior, você aprenderá como essa conexão funciona (Model Context Protocol).

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme se **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue sobre a atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo tanto as atualizações de instruções quanto a base do auxiliar de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando o Copilot solicitar **Allow** para executar ações do GitHub em seu nome, revise o comando e selecione **Allow** conforme apropriado.
6. Depois da criação do PR, peça ao Copilot que o integre em `main` e retorne sua branch local para `main` com o prompt a seguir:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando o Copilot solicitar **Allow** para executar ações do GitHub e comandos de shell em seu nome, revise o comando e selecione **Allow** conforme apropriado.

Você criou e integrou um pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e, em seguida, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *atuais*;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`;
- reenviar o mesmo prompt e observar o código proposto adotar o novo padrão;
- confirmar as alterações nas instruções e na base do auxiliar em `main` para que o próximo exercício possa usá-las.

Na próxima lição, você aplicará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
