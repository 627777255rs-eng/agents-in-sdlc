---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y explora la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, además de proporcionar a Copilot automatizaciones adicionales. Para ello se utilizan los servidores MCP, que amplían Copilot CLI con herramientas y servicios.

En esta lección:

- aprenderás qué es el Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor Playwright MCP si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar los cambios en la interfaz hay que interactuar con ella. Quieres que Copilot pueda utilizar el sitio como lo haría una persona y automatizar más el proceso de comprobar que los cambios funcionan según lo previsto.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente y los servicios externos. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para administrar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: permite automatizar un navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y fomentar las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor es **STDIO** pulsando <kbd>Enter</kbd> y, a continuación, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad funciona. Ahora utilizaremos el servidor Playwright MCP disponible en Copilot para repetir la comprobación.

1. Envía este prompt para pedir a Copilot que utilice el servidor Playwright MCP y valide la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente no hace falta pedir explícitamente a Copilot que utilice el servidor MCP, ya que suele determinarlo por su cuenta. Aun así, indicarlo puede ayudarle a elegir la herramienta adecuada, mejorar la coherencia y ahorrar tokens.

2. Observa cómo Copilot enumera los pasos que sigue en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y próximos pasos

Has utilizado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En concreto:

- aprendiste qué es Model Context Protocol y cómo lo utiliza Copilot CLI.
- añadiste el servidor Playwright MCP si aún no estaba disponible.
- pediste al agente que controlara un navegador y explorara la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
