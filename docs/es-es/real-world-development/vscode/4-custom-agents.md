---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos que ayuden a Copilot a realizar determinados tipos de trabajo. Los equipos pueden convertir sus conocimientos en agentes reutilizables: uno de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades y otros agentes. En este ejercicio, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad y podría utilizar habilidades, por ejemplo, para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como sucede con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la adecuada para tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio aprenderás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios de los usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, por lo que utilizarás un agente personalizado para generar la funcionalidad cuanto antes.

## Revisa el agente personalizado de accesibilidad

Ya hay un agente personalizado de accesibilidad. Revisaremos el archivo para entender cómo guía a Copilot.

Vuelve al codespace y crea una rama nueva a partir de `main` para mantener separados los cambios de accesibilidad y la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios en los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales que deben evitarse.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para utilizar sus instrucciones y herramientas en una sesión de chat.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio.

1. En el menú desplegable de agentes de Chat, selecciona **Agent**, si aún no está seleccionado.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes situado en la parte inferior de la vista de chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. En la lista de agentes disponibles, selecciona **Accessibility agent**.
4. Envía este prompt:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que puedes revisar.
6. A medida que se apliquen las ediciones, el indicador **Files changed** actualizará la lista de archivos modificados.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, CSS y pruebas relacionadas.

Revisarás y dirigirás el trabajo en curso en el ejercicio siguiente.

## Resumen y próximos pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas o ámbitos concretos. Permiten convertir conocimientos y estándares del equipo en agentes reutilizables que guían a Copilot.

En este ejercicio:

- aprendiste cómo se definen los agentes personalizados.
- utilizaste un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba tu funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
