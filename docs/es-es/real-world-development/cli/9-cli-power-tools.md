---
title: "Lección 9 - Comandos de barra de Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Explora algunos más.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para examinar los modelos disponibles.
- aprenderás a exportar o compartir una sesión con `/share`.
- explorarás comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo opcionalmente al [agente en la nube de Copilot][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas grandes o complejas puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta; también puedes inspeccionarla o compactarla con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a ejecutar `/context` y compara el resultado. Si la conversación ya es pequeña, quizá apenas haya cambios.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` para iniciar una conversación nueva al cambiar a una tarea distinta.

## Elegir un modelo

Los modelos tienen distintas fortalezas y cada persona desarrolladora puede tener preferencias diferentes. Copilot CLI permite mostrar y seleccionar modelos.

1. Escribe:

   ```plaintext
   /model
   ```

2. Examina los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Compartir el trabajo y los aprendizajes ayuda al equipo a mejorar el uso de herramientas de IA. El comando `/share` puede exportar una sesión a Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado, por ejemplo `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir mensajes, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no quiera compartir.

## Opcional: ampliar o delegar el trabajo

El taller principal ha terminado. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una PR.

Estos comandos son opcionales porque pueden crear worktrees adicionales o trabajo remoto. Empieza por una tarea nueva y bien acotada, y revisa el resultado con tu flujo habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y pasos siguientes

Los comandos de barra de Copilot CLI permiten configurar la herramienta, compartir sesiones y consultar lo que ocurre en segundo plano. En esta lección:

- has usado `/context` y `/compact` para explorar la gestión del contexto.
- has usado `/model` para examinar los modelos disponibles.
- has aprendido a exportar o compartir sesiones con `/share`.
- has explorado comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

Copilot CLI incluye muchos más comandos de barra. Para cerrar el recorrido, [repasa lo que has aprendido][next-lesson] y los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
