---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. En el flujo de trabajo de desarrollo hay que localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. La lista continúa.

El modo agente de Copilot ayuda con este proceso.

El modo agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentre. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona como un compañero de equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- hacer que el modo agente explore el proyecto e identifique los archivos pertinentes para realizar cambios coordinados.
- implementar funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente antes de fusionarlos.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para los editores; ahora completarás con el modo agente el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM. No hay una API de backend independiente ni un framework de interfaz del lado del cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`; las interacciones se implementan con un pequeño elemento `<script>` de Astro y las API estándar del DOM.

### Inicia el sitio

Para iniciar el sitio, se ha configurado la tarea `npm run dev`. Ejecútala en el codespace:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cámbiate a una rama nueva para este trabajo:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal para abrir el sitio.

> [!NOTE]
> En un codespace, al seleccionar el enlace a localhost desde el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Este túnel privado permite acceder al servidor web que se ejecuta en el codespace.

### Explora el sitio web

Las funciones principales son:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: muestra más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio.

La implementación inicial funciona, pero queremos ampliarla. Al crear el proyecto a partir de la plantilla se generó un backlog de incidencias automáticamente. Pide a GitHub Copilot que te muestre esas incidencias.

1. En el menú desplegable de agentes de Chat, selecciona **Agent**. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y utilizar herramientas.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Durante su desarrollo se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes y la experiencia fuese similar. Como los modelos de lenguaje funcionan de forma probabilística, las sugerencias pueden variar. Es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot podría utilizar un comando MCP distinto y aun así completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas sobre cómo añadir funcionalidad, entre ellas:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para varios tipos de archivos, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a los archivos del proyecto. Las rutas son relativas al archivo de instrucciones; así se pueden reutilizar pautas, dividir instrucciones extensas y ofrecer ejemplos.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos con la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios. El modo agente puede realizar estas tareas:

1. Continúa la conversación actual de Copilot o inicia otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la funcionalidad conforme a la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los archivos de datos, interfaz y pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos. Mientras trabaja, quizá aparezcan los botones **Keep** y **Undo** en el editor; espera a que termine antes de aceptar o deshacer los cambios.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas o que fallen algunas pruebas durante el proceso. Copilot alternará entre la generación de código y las pruebas hasta completar la tarea sin detectar errores.

   ![Sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

Has utilizado el modo agente de GitHub Copilot para añadir funcionalidad al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz de usuario y la capa de datos.
- explorar el proyecto, identificar los archivos pertinentes y hacer cambios coordinados.
- revisar los cambios y las pruebas generados por el modo agente antes de fusionarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, puede que sea necesario añadir paginación. Utiliza lo aprendido para pedir a Copilot que actualice el sitio. Entre otras cosas:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo debería funcionar la paginación, por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en el prompt las pautas necesarias para obtener la implementación que quieres.
- itera con GitHub Copilot y proporciona cambios y contexto adicionales. Es normal hacerlo al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
