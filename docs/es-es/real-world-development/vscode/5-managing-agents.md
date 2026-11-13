---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Agentes personalizados][previous-lesson]

En modo de agente, Copilot explora el código, propone cambios, edita archivos y ejecuta comandos de forma autónoma. Como el trabajo ocurre en tiempo real en tu equipo, Copilot Chat de VS Code muestra las llamadas a herramientas y las ediciones. Puedes revisar cada diferencia, aceptar o rechazar cambios y orientar la conversación con mensajes de seguimiento.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- orientarás la sesión para añadir un modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño pide añadir también un modo claro. En vez de iniciar otra conversación, orientarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en el chat con controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, lecturas y ediciones.
3. Para cada diferencia, selecciona **Keep** para aceptar el cambio o **Undo** para revertirlo y pedir otro intento.
4. Selecciona **Files changed** en la parte superior del chat para consultar todos los archivos modificados.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que termine el modo de alto contraste. Copilot mostrará un resumen de los cambios.

## Pedir al agente que añada el modo claro

Amplía la misma conversación para añadir un modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de los componentes, estilos y uso del almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente revisará los cambios y propondrá las adiciones para el modo claro.
3. Revisa cada cambio y usa **Keep** o **Undo**.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, refactoriza la interfaz de alternancia de una forma que no te convence—, deténlo con el botón **Stop** junto al campo de mensaje y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Puedes pedir ajustes (por ejemplo, simplificar el CSS y usar variables), corregir el enfoque (integrar el modo claro en el panel existente) o ampliar el alcance (añadir métodos abreviados de teclado).

## Revisar el conjunto de trabajo

Antes de confirmar los cambios, revisa todo lo que haya modificado el agente.

1. Abre **Source Control** en VS Code.
2. Revisa la lista de archivos modificados, que debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Comprueba los atributos ARIA, la navegación con teclado, el HTML semántico y la persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has supervisado y orientado una sesión en curso de Copilot Chat, revisando cada edición, aceptando o rechazando cambios y enviando instrucciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos.
- has orientado una sesión con mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios del agente local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
