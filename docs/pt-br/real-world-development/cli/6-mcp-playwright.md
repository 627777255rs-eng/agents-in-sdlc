---
title: "Lição 6 - Valide a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. Precisamos trabalhar com dados, serviços externos e até disponibilizar outras automações para o Copilot. É aí que entram os servidores MCP. Eles permitem que o Copilot vá além dos recursos incluídos na CLI e tenha acesso a mais ferramentas e serviços.

Nesta lição, você:

- entenderá o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza;
- adicionará o servidor MCP do Playwright, se ele ainda não estiver disponível;
- pedirá ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Os testes de unidade e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria, automatizando ainda mais as alterações e aumentando a confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] oferece aos agentes de IA uma forma de se comunicar com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: fornece recursos de automação de navegador usando o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, digite o comando a seguir para abrir a caixa de diálogo de adição de servidor MCP:

   ```plaintext
   /mcp add
   ```

3. No campo de nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o comando a seguir na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Peça ao Copilot que explore o recurso com o Playwright

Anteriormente, você confirmou manualmente que a funcionalidade funciona como esperado. Agora, vamos pedir ao Copilot que faça a mesma verificação com o servidor MCP do Playwright.

1. Use o prompt a seguir para pedir ao Copilot que utilize o servidor MCP do Playwright para validar a funcionalidade:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário dizer ao Copilot que use o servidor MCP: ele normalmente descobrirá isso sozinho. Mas, como você sabe qual ferramenta deve usar, é útil indicar a direção certa! Isso ajuda a obter resultados mais consistentes e economiza alguns tokens.

2. Observe enquanto o Copilot lista as etapas que executa no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme que tudo funciona como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor MCP do Playwright para explorar seu recurso em um navegador real por meio do Copilot CLI! Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza;
- adicionou o servidor MCP do Playwright, caso ainda não estivesse disponível;
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Na próxima lição, você [criará um agente personalizado de QA][next-lesson] que reúne a skill e as ferramentas do navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
