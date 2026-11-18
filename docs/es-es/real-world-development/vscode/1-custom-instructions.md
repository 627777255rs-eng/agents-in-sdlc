---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de fondo, debes asegurarte de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan pautas para generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones asociadas a rutas.
- generarás la primera parte de los datos para el filtrado (una función auxiliar para editores) con las instrucciones actuales.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y la función de filtrado en `main` para que Copilot utilice las pautas actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares establecidos. Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener exactamente el mismo código.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Utilizarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para que las tareas se ajusten a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para ayudarle a entender el estilo de programación y los requisitos del proyecto. Puedes especificar convenciones, bibliotecas y comentarios, y crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes, como la tecnología utilizada, una descripción general, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivos concretos, como TypeScript, Astro, componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones basados en otros estándares, como AGENTS.md y CLAUDE.md; consulta la [compatibilidad con archivos de instrucciones][custom-instructions-support].

### Prácticas recomendadas para administrar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las pautas generales del proyecto, como su descripción, estructura y estándares globales.
- Utiliza archivos `*.instructions.md` para pautas de tipos concretos de archivos (pruebas unitarias, componentes Astro o la capa de datos) o tareas específicas.
- Escribe en lenguaje natural, con pautas claras y ejemplos de código correctos e incorrectos.

No existe una única forma de crear archivos de instrucciones o utilizar IA. La experimentación te ayudará a encontrar el enfoque adecuado.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás pautas para tareas como [actualizar la interfaz de usuario][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno presenta esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente explica cómo utilizarla cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye el repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas. Puedes abrirlos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Contiene instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`: define un patrón global relativo a la raíz del repositorio. En este caso, se aplica a archivos de prueba TypeScript que coincidan, por ejemplo, con `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y ve al final. Observa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes reutilizar directrices y ejemplos; las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario.

## Crea una rama para los cambios

Crea una rama para trabajar:

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones correspondientes lo indicarán.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo utilice el contexto deseado.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. En el menú desplegable de agentes de Chat, selecciona **Agent** para que Copilot pueda cambiar archivos.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, a menudo en el archivo auxiliar y sus pruebas.
6. Observa que la función propuesta es tipada, recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot funciona de forma probabilística, es posible que añada comentarios de documentación aunque no se le soliciten. No pasa nada; el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar para el repositorio

`.github/copilot-instructions.md` proporciona información general del proyecto. Añadiremos pautas para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Todavía no incluye una regla de documentación, por lo que la función generada no tenía comentarios.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones del repositorio (`.github/copilot-instructions.md`) son adecuados para estándares generales; los archivos `*.instructions.md` son más apropiados para lenguajes, tipos de archivo o tareas concretas.

## Vuelve a ejecutar el prompt y observa el cambio

1. En Copilot Chat, selecciona **New Chat** para borrar el contexto anterior.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc similar a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has guiado a Copilot para que siga un estándar nuevo y lo aplique al código que utilizará el siguiente ejercicio.

## Confirma las instrucciones y envía la rama

Los archivos de instrucciones se administran mediante el control de versiones como cualquier otro recurso.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirma que las instrucciones y los cambios de la función están presentes:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos que Copilot haya modificado para la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona una solicitud de incorporación de cambios

Con la rama enviada, abre una solicitud de incorporación de cambios (PR) y relaciónala con la incidencia sobre las instrucciones del backlog.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub. La primera vez que Copilot utilice una, es posible que VS Code te pida iniciar sesión. Sigue las instrucciones para autorizarlo. Más adelante conocerás Model Context Protocol (MCP).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa la actualización de instrucciones y la base de la función auxiliar de editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si corresponde.
5. Cuando se cree la PR, pide a Copilot que la fusione y vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Cuando Copilot te pida permiso para realizar acciones en GitHub o ejecutar comandos de shell, revísalos y selecciona **Allow** si corresponde.

Has creado y fusionado una solicitud de incorporación de cambios con ayuda de GitHub Copilot.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar para todo el repositorio en `.github/copilot-instructions.md`.
- repetir el prompt y comprobar que el código propuesto sigue el nuevo estándar.
- confirmar las instrucciones y la base de la función auxiliar en `main` para continuar el trabajo en el ejercicio siguiente.

A continuación, utilizarás estas instrucciones en el [modo agente][next-lesson] para añadir una funcionalidad en todo el proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
