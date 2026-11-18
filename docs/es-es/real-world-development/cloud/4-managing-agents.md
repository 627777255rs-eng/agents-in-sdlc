---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente de Copilot en la nube tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque el agente trabaja de forma autónoma y asíncrona, es importante supervisar estas tareas.

Puedes administrar las tareas asignadas al agente mediante varias herramientas, como [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas con solicitudes de incorporación de cambios abiertas, consultar las acciones realizadas e incluso dirigir una sesión mientras sigue en curso.

En esta lección:

- explorarás la página de agentes para supervisar el trabajo del agente en la nube.
- guiarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de añadir el modo de alto contraste, el equipo decidió que sería un buen momento para añadir también un modo claro. Como ya se están actualizando los estilos del sitio y añadiendo controles de modo, pedirás al agente que incluya el modo claro además del alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Vamos a consultar el estado de las tareas asignadas al agente.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas de los paneles principal e izquierdo. Deberías ver las tareas que has asignado, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno para que trabaje Copilot.
   - configurar servidores MCP.
   - completar distintos pasos de la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitud de incorporación de cambios (PR) de la derecha. Desde ahí puedes consultar la PR y los archivos modificados.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que utilice Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos a que termine la configuración y empiece el trabajo. Lo sabrás cuando aparezcan mensajes de actividad.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la página de agentes con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo recibe e incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando al recibir el mensaje.

El proceso puede tardar varios minutos. Es un buen momento para repasar lo que has aprendido.

## Resumen y próximos pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso.

Has aprendido a:

- explorar el centro de control y la página de agentes para supervisar las tareas.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
