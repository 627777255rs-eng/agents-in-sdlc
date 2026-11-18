---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de fondo, debes asegurarte de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda tanto *qué* debe hacer como *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones asociados a rutas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no ejecutarás un prompt antes y después del cambio. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene pautas y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que las tareas se ajusten a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para ayudarle a entender el estilo de programación y los requisitos del proyecto. Puedes especificar convenciones de código, bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las solicitudes de chat o CLI, como la tecnología utilizada, una descripción del proyecto, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivos concretos. Permiten proporcionar pautas para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones basados en otros estándares, como AGENTS.md y CLAUDE.md; consulta la [compatibilidad con archivos de instrucciones][custom-instructions-support].

### Prácticas recomendadas para administrar archivos de instrucciones

Crear archivos de instrucciones queda fuera del alcance de esta conversación, pero los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las pautas generales del proyecto, como su descripción, estructura y estándares globales de código.
- Utiliza archivos `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas concretas.
- Escribe en lenguaje natural y procura que las pautas sean claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No existe una única forma de crear archivos de instrucciones o utilizar la IA. La experimentación te permitirá encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá encuentres pautas para tareas como [actualizar la interfaz de usuario][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente explica cómo usarla cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Contiene instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio: define un patrón global relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, se aplican a los archivos de prueba TypeScript que coincidan, por ejemplo, con `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y ve al final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario de los archivos.

## Añade un nuevo estándar para el repositorio

Este es el único cambio que harás en este ejercicio: añadir una regla para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube utilizará esta regla en las incidencias que le asignes más adelante.

Antes de editar, crea una rama para trabajar. El agente en la nube leerá las instrucciones de la rama de destino de la incidencia, pero así podrás confirmar los cambios por separado:

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado, `.github/copilot-instructions.md` proporciona información general del proyecto. Añadamos un estándar que mejore las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Todavía no contiene una regla para la documentación en el código; por eso el asistente generado no tenía comentarios de documentación.
3. Justo debajo de los estándares existentes, añade estas líneas de Markdown para indicar a Copilot que incluya encabezados de comentario y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para pautas globales o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, envía y fusiona el cambio de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, partirá de `main`; por eso, los cambios de instrucciones deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y fusiónala. Así, el agente en la nube podrá leer las instrucciones cuando trabaje en las incidencias que le asignes.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir el paso de la rama y confirmar los cambios directamente en `main`. La rama se incluye para reflejar cómo se trabaja en un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube en el resto del itinerario.

A continuación, [asigna tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
