---
title: "Lección 5 - Personaliza y utiliza la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que el código funciona y hemos utilizado archivos de instrucciones para asegurarnos de que sigue los estándares. ¿Y las pruebas, el lint y el resto de los aspectos de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo realizar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos complementarios que un agente carga cuando los necesita. Una habilidad consiste principalmente en una carpeta con un archivo Markdown llamado `SKILL.md`. El archivo contiene un frontmatter con el nombre y la descripción de la habilidad, una descripción general de sus funciones e indicaciones sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas y reutilizarlas en el equipo, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Exploraremos la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el campo `name` y la descripción del principio. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Observa que todavía no existe una sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

Puedes llamar a las habilidades directamente desde Copilot CLI o utilizar lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe final.

## Personaliza el informe

Queremos que el informe indique qué se ha ejecutado, si se ha completado correctamente y qué resultados han devuelto las herramientas. Actualizaremos la habilidad para generarlo.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Veamos la habilidad actualizada en acción. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe y compáralo con el primero.

## Resumen y próximos pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- exploraste `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- personalizaste el formato de sus resultados.
- volviste a cargar y ejecutar la habilidad.

Este cambio acompañará al filtrado en la PR de funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia, no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación integrado: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
