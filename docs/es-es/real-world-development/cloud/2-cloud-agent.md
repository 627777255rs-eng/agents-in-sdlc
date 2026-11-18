---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: incidencias de seguridad pendientes, código heredado que hay que actualizar o funcionalidades que siguen en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dedicar tiempo a cuestiones más importantes.

Explorarás cómo:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, falta por completo. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar cómo se descubren los juegos. Le gustaría que cada página de detalles mostrase juegos relacionados (otros títulos de la misma categoría), aunque por ahora no necesita una implementación perfecta: quiere probar la experiencia y los criterios de aceptación. Es una tarea importante, pero hay otras incidencias con mayor prioridad.

Estos tipos de trabajo pueden quedar relegados con facilidad y son buenos candidatos para delegarlos al agente de Copilot en la nube. El agente puede trabajar en segundo plano mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el resultado.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a un desarrollador. Puedes, por ejemplo, [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna, Copilot crea una PR preliminar para hacer seguimiento del trabajo, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras se ejecuta o después de que termine. Cuando esté lista la solución para revisión, te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no hace milagros: no puedes saltarte el análisis. Incluso las tareas aparentemente sencillas pueden ser complejas, así que [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: divide el trabajo en etapas, aprende, experimenta e itera. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo guían a Copilot. Si ya completaste un ejercicio práctico sobre instrucciones personalizadas, habrás visto funcionar `.github/copilot-instructions.md`; si no, puedes revisarlo ahora. Antes de asignar trabajo al agente, lee los archivos de instrucciones del repositorio para observar su efecto más adelante.

- `.github/copilot-instructions.md`: revisa **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, busca en la PR los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Estas pautas proceden de los archivos de instrucciones que acabas de revisar.

## Configura el entorno de desarrollo del agente

La creación de código requiere un entorno y cierta configuración para instalar herramientas y dependencias. El agente en la nube también los necesita. Utiliza [GitHub Actions][github-actions] y puede ejecutar un [flujo de trabajo de configuración][setup-workflow] especial desde `.github/workflows/copilot-setup-steps.yml` antes de empezar. El flujo de trabajo de este taller ya se ha configurado para instalar Node.js y las dependencias, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y preparada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña Actions del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos necesarios para estos pasos. Copilot utilizará su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitarán el permiso contents: read. Si no lo clonan, Copilot lo hará automáticamente al finalizar los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación es Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y prepara la base de datos SQLite local para que haya datos en las compilaciones y pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo inicie.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esa deuda técnica puede ralentizar el trabajo y dificultar el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena incidencia para delegar al agente en la nube. En un ejercicio posterior revisarás su trabajo.

1. En una nueva pestaña, ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Después de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> Es posible que la lista tarde varios minutos en aparecer. Copilot está preparando el entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando cómo resolver la tarea.

13. Revisa las tareas que va a completar.
14. Desplázate por la cronología de la PR y comprueba que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, analiza su estado, elabora un plan y crea código.

El proceso puede tardar varios minutos. La principal ventaja del agente en la nube es que permite realizar tareas de forma asíncrona. Aprovecharemos esa característica para asignarle otra tarea y ocuparnos mientras tanto de añadir una funcionalidad.

## Añade una sección de juegos relacionados

Otra ventaja del agente en la nube es que puede trabajar en una tarea mientras tú te ocupas de otra. Vamos a pedirle que añada una sección de juegos relacionados en la página de detalles de un juego.

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. En **Title**, escribe `Show related games on the game details page`.
5. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot**.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

8. Selecciona **Assign**.

Al poco tiempo verás 👀 en el primer comentario de la incidencia, que indica que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya trabaja en tu segunda solicitud. No es necesario que lo supervises continuamente; puedes revisar los cambios cuando termine. Mientras tanto, aprenderemos a crear y utilizar agentes personalizados.

## Resumen y próximos pasos

Has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y delegarle deuda técnica, nuevas funcionalidades o migraciones de código.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar la seguridad de las operaciones.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes [crear y utilizar agentes personalizados][next-lesson]. El agente en la nube también puede utilizar [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, como las que ya has explorado.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
