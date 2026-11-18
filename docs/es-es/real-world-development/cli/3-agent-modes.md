---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos añadido una pequeña funcionalidad al proyecto, pero los cambios más amplios necesitan un proceso más completo. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta lección y las siguientes seguiremos un proceso habitual de desarrollo con agentes: partir de una incidencia para crear una funcionalidad, comprobar que el código es válido y que funciona como se espera y, por último, integrarlo correctamente en el proyecto.

> [!NOTE]
> Durante el flujo de trabajo de esta funcionalidad usarás la misma conversación y rama. Normalmente se crearían ramas o PR distintas para los diferentes tipos de archivos, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en un navegador reenviado.

Mientras completas esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editor. La incidencia del trabajo pendiente describe la funcionalidad, pero es necesario acordar algunos detalles antes de programar, como la combinación de categorías. Utilizarás el modo Plan para resolver estas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia que describa el trabajo necesario.
2. Crear un plan con lo que se debe desarrollar.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Tras revisar el código y superar la integración continua, fusionarlo.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos siguen un patrón parecido.

Si mantienes este enfoque estándar, te asegurarás de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Te sugiere cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Empieza por la incidencia

Antes de comenzar el trabajo de filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha fusionado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Con las flechas, selecciona la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza con `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir cómo se implementará una funcionalidad o una tarea antes de delegarla a Copilot. Para las tareas complejas, conviene dedicar un tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias: no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Autoriza Autopilot

Con el plan escrito y revisado, ya puedes implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador del modo muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente; revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas.

## Revisa y verifica la implementación

Como cualquier otro código, el código generado debe revisarse antes de fusionarlo. Revisemos el código y ejecutemos el sitio para comprobar el resultado.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Comprueba que el indicador ya no muestre **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría, por editor y las combinaciones que acordaste en el plan.
4. Confirma que el restablecimiento de filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y próximos pasos

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación de Copilot desde una incidencia de GitHub.
- definiste los requisitos en el modo Plan.
- implementaste la funcionalidad con el modo Autopilot.
- revisaste el código.
- validaste la funcionalidad manualmente en un navegador reenviado.

A continuación, aprenderás cómo se genera el código y cómo seguir las prácticas documentadas al [utilizar instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
