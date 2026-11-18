---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del desarrollo. Puedes definirlos mediante archivos Markdown en `.github/agents` y proporcionar instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos propios del dominio. Los equipos pueden convertir sus conocimientos en agentes reutilizables: uno de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que un **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades y otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; para ello podría utilizar habilidades de flujo de solicitudes de incorporación de cambios o para ejecutar y administrar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar el enfoque adecuado para tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio explorarás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios de usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que pueda activarse y desactivarse.

Como la accesibilidad es importante, utilizarás un agente personalizado para generar esta funcionalidad.

## Revisa el agente personalizado de accesibilidad

Ya hay un agente personalizado para trabajar con accesibilidad. Revisaremos su contenido para entender cómo guía a Copilot.

Vuelve al codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales que deben evitarse.

## Crea y asigna una incidencia

El panel de agentes de GitHub es el centro de control para trabajar con agentes: puedes asignar tareas, supervisarlas y proporcionar indicaciones adicionales. Vamos a asignar al agente una tarea para crear un modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. En **Description**, escribe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot**.
9. En la lista de agentes personalizados, selecciona **Accessibility agent**.

    ![Asignación del agente en la nube con el agente de accesibilidad seleccionado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y próximos pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas o ámbitos concretos. Los equipos pueden codificar sus conocimientos y estándares en agentes reutilizables para guiar el trabajo de Copilot.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo asignarles una tarea.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y guiar la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparación para utilizar agentes personalizados en una organización][org-custom-agents]
- [Preparación para utilizar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y administrar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
