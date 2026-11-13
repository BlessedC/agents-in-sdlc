---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores has encargado al agente en la nube tres tareas para mejorar la experiencia y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante supervisarlo.

Hay varias herramientas para gestionar las tareas asignadas, como la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes consultar las tareas con solicitudes de incorporación de cambios abiertas, revisar las operaciones realizadas y orientar las sesiones en curso.

En esta lección:

- explorarás la página de agentes para supervisar sus tareas.
- orientarás una sesión activa y pedirás una funcionalidad adicional.

## Escenario

Después de asignar el modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se está actualizando el estilo del sitio y la funcionalidad de alternancia, pedirás al agente que incluya el modo claro.

## Revisar las tareas del agente en la nube

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado, como actualizar la documentación, generar API para modificar juegos y añadir un modo de alto contraste.
3. Selecciona una tarea en curso y revisa sus operaciones: descargar el código, preparar el entorno, configurar servidores MCP y completar la tarea.

> [!NOTE]
> Los pasos exactos dependen del estado de la tarea y del enfoque que siga Copilot.

4. Revisa también el panel de PR, que muestra la solicitud y los archivos modificados.

## Orientar al agente en la nube

Pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende de cómo lo haya nombrado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera a que termine la configuración y empiece a trabajar.
3. En el cuadro **Steer active session while Copilot is working**, escribe:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![Tarea del agente en la nube con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud cuando termine el paso en curso. El proceso tardará varios minutos; puedes aprovechar el tiempo para repasar lo que has aprendido.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar sus tareas. En esta lección:

- has supervisado las tareas del agente en la página de agentes.
- has orientado una sesión en curso para pedir otra funcionalidad.

A continuación, [iterarás sobre las PR de Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
