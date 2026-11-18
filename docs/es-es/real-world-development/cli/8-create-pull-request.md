---
title: "Lección 8 - Crea y fusiona la PR de funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas están en una sola rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios (PR). Fusionaste manualmente la PR de las valoraciones con estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que la PR se ha fusionado.

## Escenario

Durante el trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el resto de las tareas de la PR, pero mantener bajo el control del desarrollador la autorización de fusión.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR —corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a basar la rama cuando es necesario— y, después, activa la fusión automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Utiliza Agent Merge para gestionar la PR

Ya has creado todo el código. Revisémoslo, creemos la PR y dejemos que Agent Merge se encargue del resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR. Cuando todas las comprobaciones se completen correctamente, la fusionará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, revisa el motivo indicado y completa manualmente la fusión revisada cuando la configuración del repositorio lo permita.

## Resumen y próximos pasos

Has automatizado distintas partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora la gestión de la PR. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionaste todos los cambios de la funcionalidad y las pruebas de verificación.
- creaste la PR de filtrado.
- activaste Agent Merge después de la revisión y confirmaste que la PR se había fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para administrar el contexto, los modelos y el uso compartido, y para delegar tareas opcionalmente al agente en la nube.

## Recursos

- [Administrar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
