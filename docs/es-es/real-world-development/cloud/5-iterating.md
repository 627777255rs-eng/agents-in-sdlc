---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Administrar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en distintas tareas para mejorar la experiencia de usuario y añadir funcionalidad. Le pediste que documentara el código, añadiera una sección de juegos relacionados y creara funciones de accesibilidad, incluidos controles para el alto contraste y el modo claro. Ahora revisaremos el código y, si hace falta, pediremos cambios antes de habilitar la revisión para el resto del equipo.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de que el resto del equipo revise las propuestas de GitHub Copilot, examinaremos las solicitudes de documentación, juegos relacionados y accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

El agente realiza tareas de forma asíncrona y sin supervisión, por lo que existen medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente se ejecuta en GitHub Actions, dentro de un entorno separado y efímero.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [De forma predeterminada, el acceso a recursos externos está limitado](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empezaremos con la primera solicitud de incorporación de cambios (PR) que generó el agente: la documentación del código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, es posible que veas una advertencia que indica que el firewall bloqueó a GitHub Copilot. Es **normal**: de forma predeterminada, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests**.
3. Abre la solicitud cuyo título se parezca a **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; el proceso puede tardar varios minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios exactos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Proceden de los archivos de instrucciones que revisaste al principio del Ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. Esto puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, como harías con otra persona del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como el resultado no es determinista, no hay un texto único que debas utilizar. Puedes pedirle, por ejemplo, que:

- añada a cada archivo de código un encabezado con una breve descripción de su función.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot**. Puedes utilizar una de las ideas anteriores.
2. Selecciona **View Session** para ver el trabajo de Copilot. Observarás que empieza una sesión nueva.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes iterar hasta que el resultado te satisfaga. Cuando esté listo, puedes convertir el borrador en una PR lista para revisión y fusionarla en la rama principal.

![Convertir una PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Ve a tu repositorio en GitHub.com y selecciona la pestaña **Pull Requests**.
2. Abre la PR cuyo título se parezca a **Show related games on the game details page**.
3. Selecciona **Files changed** y revisa el código generado.
4. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
5. Si los flujos de trabajo necesitan aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

6. Comprueba que las tareas se pongan en cola en la sección de comprobaciones y que las pruebas del proyecto se superen.
7. **Opcional:** cambia a la rama en tu codespace para probar manualmente la funcionalidad. Sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la sección de juegos relacionados. Puedes pedirle cambios adicionales mediante un comentario en la pestaña **Conversation**, por ejemplo, cambiar cuántos juegos se muestran o añadir encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste de la Lección 3 y el modo claro que solicitaste desde el centro de control en la Lección 4.

1. Ve a tu repositorio en GitHub.com y selecciona la pestaña **Pull Requests**.
2. Abre la PR cuyo título se parezca a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

3. Selecciona **Files changed** y revisa el código.
4. Presta atención especialmente a:
   - los controles para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para el alto contraste y el modo claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.).
   - el código JavaScript/TypeScript que gestiona los modos.
5. Vuelve a **Conversation** y desplázate hacia abajo.
6. Si los flujos de trabajo necesitan aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. Comprueba que las tareas se pongan en cola en la sección de comprobaciones y que las pruebas se superen.
8. **Opcional:** cambia a la rama en el codespace para probar las funciones de accesibilidad. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

   Inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias persisten al recargar la página.

El agente personalizado de accesibilidad ayudó a guiar a Copilot para implementar estas funciones siguiendo prácticas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que lo corrija.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegar tareas reales del backlog mientras tú te ocupas de otras cosas. Para practicar, crea algunas incidencias y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles del juego.
- implementar la paginación en la página de lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones del repositorio** para observar su efecto en el trabajo del agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** de accesibilidad para añadir controles de alto contraste y modo claro.
- **Utilizaste la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad.
- **Revisaste las solicitudes de incorporación de cambios del agente** e iteraste sobre ellas mencionando a `@copilot` y aprobando flujos de trabajo.

## Revisión y próximos pasos

Has completado el itinerario del agente en la nube. Si quieres seguir aprendiendo, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: publica dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el comando `/delegate`.

En tu propio repositorio, prueba a:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Administrar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
