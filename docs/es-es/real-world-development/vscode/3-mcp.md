---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el servidor Playwright MCP y dejarás que Copilot controle un navegador real. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con herramientas.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más útil cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) permite que Copilot se comunique con herramientas y servicios externos mediante un estándar común.

![Diagrama del funcionamiento del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las herramientas proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden consultar información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente y los servicios externos, como las API existentes o las herramientas locales de paquetes npm. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a las API para administrar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: permite automatizar el navegador mediante Playwright, por ejemplo, para navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y fomentar las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa el código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Utiliza solo servidores de confianza y ten cuidado al otorgar acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Comprueba que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

El proyecto incluye esta entrada `github` en su plantilla. Por eso Copilot pudo leer las incidencias del backlog en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server]; no tienes que instalarlo localmente. VS Code se conecta por HTTP y la primera vez que Copilot utiliza una herramienta te autenticas con GitHub.

## Añade el servidor Playwright MCP

Añade un segundo servidor: [Playwright MCP Server][playwright-mcp-server] proporciona a Copilot un navegador controlable que utilizarás para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en el equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin ventana visible, algo necesario en un codespace, que no tiene escritorio.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador necesario ya está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia y confía en el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide confirmar que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece encima de la entrada `playwright`, y confirma que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará y te pedirá que confirmes la confianza cuando Copilot necesite las herramientas en el paso siguiente.

## Prueba la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] permite que Copilot controle un navegador real. En vez de probar la aplicación manualmente, el agente puede abrir una página, aplicar filtros y leer los resultados. Después, resumirá lo que ha observado. Así puedes comprobar rápidamente el comportamiento de una funcionalidad sin salir de la conversación.

El servidor Playwright MCP utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados (botones, enlaces y elementos de lista), como lo hace la tecnología de asistencia. Por tanto, una prueba funcional también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá cada paso y comunicará los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide más información o solicita que corrija el código antes de crear la PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, inícialo otra vez antes de enviar el prompt. La primera vez que Copilot utilice Playwright MCP, quizá tenga que descargar el navegador; si informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Después de confirmar que la funcionalidad funciona, abre una solicitud de incorporación de cambios (PR) para que el equipo pueda revisarla. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una PR, por ejemplo, desde github.com o con GitHub CLI. En este ejercicio, pedirás a GitHub Copilot que busque la incidencia relacionada y cree una PR vinculada a ella.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que está seleccionado **Agent** para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para autorizar a Copilot a recopilar la información y realizar las operaciones necesarias. La primera vez que utilice una herramienta de GitHub, quizá debas autenticarte; sigue las instrucciones para autorizarla.
5. Observa cómo Copilot busca las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace de Copilot para revisar la PR, pero **no la fusiones todavía**.

## Resumen y próximos pasos

En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con herramientas.
- añadiste el servidor Playwright MCP al espacio de trabajo.
- lo utilizaste para probar manualmente la funcionalidad en un navegador antes de publicarla.
- publicaste la rama y abriste una solicitud de incorporación de cambios.

Has utilizado un servidor MCP para *probar* una funcionalidad; pero MCP ofrece otras formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para automatizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de servidores MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
