---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Has creado el filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y pedirás a Copilot que controle un navegador para comprobar la funcionalidad. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplía Copilot con herramientas.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar el filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es más potente cuando puede acceder a recursos externos. Model Context Protocol (MCP) permite que Copilot se comunique con herramientas y servicios externos mediante un protocolo estándar.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Un servidor MCP hace de puente entre el agente y las herramientas externas, como las API existentes o herramientas locales como los paquetes npm. Cada servidor ofrece un conjunto diferente de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios de GitHub, crear repositorios y administrar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: permite automatizar el navegador, navegar por páginas, rellenar formularios y seleccionar botones.

GitHub también aloja un [registro MCP](https://github.com/mcp) para facilitar el descubrimiento de servidores y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Utiliza únicamente servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en el espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías encontrar un servidor `github` ya configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla incluye esta entrada; por eso Copilot pudo leer la lista de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], que VS Code abre mediante HTTP. La primera vez que Copilot use una herramienta, tendrás que autenticarte con GitHub.

## Añadir el servidor Playwright MCP

Añadirás un segundo servidor. [Playwright MCP][playwright-mcp-server] permite que Copilot controle un navegador para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local: VS Code lo inicia en tu equipo con `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana, algo necesario en un codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador ya estará instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar y confiar en el servidor

VS Code inicia los servidores MCP cuando se necesitan. La primera vez que Copilot utiliza uno, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece encima de la entrada `playwright`, para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pide confirmar que confías en el servidor, revisa la configuración y acepta para iniciarlo.

Este paso es opcional: si no lo inicias manualmente, VS Code lo iniciará la primera vez que Copilot necesite sus herramientas.

## Probar el filtrado

El servidor Playwright MCP ofrece a Copilot un navegador real. El agente puede abrir una página, aplicar filtros, leer los resultados y resumirlos sin que tengas que salir de la conversación.

Internamente, Playwright MCP usa el árbol de accesibilidad de la página, no capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados, como botones, enlaces y listas, de forma similar a las tecnologías de asistencia. Por eso, una comprobación funcional también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, envía este mensaje:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá los pasos y te informará de los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide más información o solicita que Copilot corrija el código antes de crear la PR.

> [!NOTE]
> La aplicación debe estar disponible en `http://localhost:4321`. Si has detenido el servidor, vuelve a iniciarlo. La primera vez que se use Playwright MCP quizá haya que descargar un navegador; si falta, pide a Copilot que ejecute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Una vez que hayas confirmado que la funcionalidad funciona, publica la rama `filtering-vscode`.

1. En el panel **Source Control** del codespace, revisa los cambios de Copilot.
2. Selecciona el icono **+** para prepararlos.
3. Usa el botón **Sparkle** para generar un mensaje de confirmación.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Puedes crear una PR desde github.com, desde la interfaz de línea de comandos (CLI) de GitHub o pedir a Copilot que la cree y la vincule con la incidencia.

1. En Copilot Chat, selecciona **New Chat**.
2. Selecciona **Agent** para que Copilot pueda utilizar las herramientas de GitHub.
3. Pídele que cree la PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Selecciona **Continue** para permitir que Copilot busque la información y realice las operaciones. La primera vez que use una herramienta de GitHub, quizá debas autenticarte.
5. Comprueba que Copilot encuentre la incidencia adecuada y cree la PR.
6. Selecciona el enlace de la PR para revisarla, pero **no la fusiones todavía**.

## Resumen y pasos siguientes

En este ejercicio:

- has aprendido qué es MCP y cómo amplía Copilot con herramientas.
- has añadido Playwright MCP al espacio de trabajo.
- has usado el servidor para probar el filtrado en un navegador antes de publicarlo.
- has publicado la rama y abierto una PR.

Has usado MCP para probar una funcionalidad, pero es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
