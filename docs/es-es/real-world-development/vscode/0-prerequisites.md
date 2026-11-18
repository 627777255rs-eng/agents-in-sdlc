---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás una copia propia del repositorio Tailspin Toys, pondrás en marcha un [codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del taller

Crearás una copia del repositorio a partir de la [plantilla][template-repository]. La nueva instancia tendrá todos los archivos necesarios para el taller.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando **Use this template** en la página del repositorio del taller y, después, **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se generará automáticamente una lista de tareas pendientes con incidencias de GitHub. Utilizarás estas incidencias durante el taller; no tienes que crear ninguna.

## Crea un codespace

A continuación, utilizarás un codespace para completar los ejercicios.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un IDE completo con compatibilidad con varios lenguajes, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Crea un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos. Aun así, será más rápida que instalar todos los servicios manualmente. Mientras esperas, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos incluyen los requisitos necesarios. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Utiliza GitHub Copilot Chat y el modo agente

Para acceder al modo agente de GitHub Copilot Chat, debes tener instalada la extensión GitHub Copilot Chat en el IDE. Si utilizas un codespace, debería estar instalada.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, quizá tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve al codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe algo como «Hello world» en la ventana de Copilot Chat y pulsa Enter. Esto debería activar Copilot Chat.
4. Si todavía no has iniciado sesión, sigue las instrucciones para autenticarte con tu cuenta de GitHub.

    ![Ejemplo de una solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana de Copilot Chat.

## Resumen

Has creado una copia del repositorio del taller y has iniciado la creación del codespace que utilizarás para escribir código.

## Siguiente paso

Pondremos Copilot a trabajar. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson], donde aprenderás a explicarle las convenciones del proyecto.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Guía de inicio de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
