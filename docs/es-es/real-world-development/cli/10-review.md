---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo continuo en Tailspin Toys. En este taller:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado.
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y fusionaste manualmente tu primera solicitud de incorporación de cambios (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en el modo Plan, lo implementaste con Autopilot y lo revisaste en el modo Interactive.
- guiaste al agente mediante instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la utilizaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Playwright Model Context Protocol (MCP) y lo utilizaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisaste todos los cambios de filtrado y autorizaste Agent Merge para la PR correspondiente.
- exploraste los comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones con estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has trabajado en la misma conversación y rama. Hemos agrupado el trabajo en una sola PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: pruebas automatizadas, inspección manual en el navegador y exploración del navegador por parte de Copilot mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y generó un informe en el nuevo formato. QA combinó estos resultados con una revisión de los requisitos y de la cobertura de las pruebas antes de crear la PR.

Las pruebas nuevas deben resolver carencias reales; si una ejecución de QA determina que no hacen falta pruebas, puede ser correcto. Revisa el código y las pruebas antes de autorizar la fusión, y actualiza las pruebas de verificación afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Utiliza **Plan** para definir un enfoque antes de empezar, **Interactive** para seguir de cerca los cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo más rápido para ediciones rutinarias y uno más avanzado para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma importante en el resultado.

## Más opciones para explorar

Has completado el flujo de trabajo principal. Aquí tienes otras funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con una herramienta es seguir utilizándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con el equipo y aprovecha lo que aprendan los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Administrar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
