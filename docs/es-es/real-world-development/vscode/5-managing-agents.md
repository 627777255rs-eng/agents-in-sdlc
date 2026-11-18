---
title: "Ejercicio 5 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Agentes personalizados][previous-lesson]

En el ejercicio anterior pediste a Copilot que añadiera un modo de alto contraste. Mientras Copilot trabaja en el IDE, puedes ver las llamadas a herramientas y los cambios de archivos a medida que se producen, revisar los diffs en línea, aceptar o rechazar los cambios y guiar la conversación con prompts adicionales.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- pedirás al agente durante la sesión que añada un control para el modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras el trabajo sigue en curso, el equipo de diseño solicita también un modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En vez de empezar una conversación nueva, ampliarás la tarea en la sesión existente.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea, junto con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta, lectura o edición de archivo y comando de terminal.
3. Cuando proponga una edición, el diff aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que termine el modo de alto contraste. El agente mostrará un mensaje final que resume los cambios.

## Pide al agente que añada el modo claro

Una vez implementado el modo de alto contraste, ampliarás la misma conversación con un control para el modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de su trabajo: los componentes que modificó, el enfoque de los estilos y el uso del almacenamiento local.

1. En la entrada de Copilot Chat, envía este prompt:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente explorará los cambios existentes y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa las nuevas ediciones a medida que se realicen, igual que con el modo de alto contraste. Utiliza **Keep** o **Undo** para cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles—, detenlo con el botón **Stop** junto a la entrada de chat y envía una aclaración.

> [!TIP]
> Es más rápido guiar una sesión en curso que empezar desde cero. Usa prompts de seguimiento para solicitar ajustes (por ejemplo, simplificar el CSS con variables en vez de duplicar selectores), corregir el enfoque (integrar el modo claro en el panel existente, sin crear un tercer control) o ampliar el alcance (añadir métodos abreviados de teclado).

## Revisa los cambios

Antes de confirmar el trabajo, revisa los archivos que modificó el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa sus diffs. Comprueba que incluyan los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición, aceptado o rechazado los cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones de archivos concretas.
- guiaste la sesión con prompts de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
