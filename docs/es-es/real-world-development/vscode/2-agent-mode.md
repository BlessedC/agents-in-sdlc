---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso un cambio sencillo suele afectar a varios archivos y requerir que se ejecuten pruebas. El proceso de desarrollo puede implicar buscar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, encontrar el archivo que falta y volver a modificar el proyecto. El modo de agente de Copilot puede explorar la estructura, editar varios archivos y ejecutar tareas como las pruebas. Veamos cómo utilizarlo para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, con ciclos rápidos de comentarios y respuestas. El agente en la nube trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- hacer que el modo de agente explore el proyecto, identifique archivos y coordine cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas antes de fusionarlos.

## Escenario

El catálogo de juegos crece y quieres permitir el filtrado por categoría y por quien publica. En el ejercicio anterior añadiste una función auxiliar para obtener quienes publican. Ahora completarás el trabajo de la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio actual. Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base SQLite local con Drizzle ORM; no hay una API de backend independiente ni un marco de interfaz en el cliente. Las funciones de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API DOM estándar.

### Iniciar el sitio web

La tarea `npm run dev` inicia el servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.
6. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost del terminal, se redirige a una dirección como `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del codespace.

### Explorar el sitio web

La página principal muestra una lista de juegos de mesa con títulos, imágenes y descripciones. Al seleccionar un juego, se abre su página de detalles con más información, como el título, la descripción, quien lo publica y la categoría.

## Explorar la lista de tareas con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; cada paso indica lo que debes seleccionar.

La aplicación funciona, pero queremos añadir capacidades. Pide a Copilot que muestre las incidencias incluidas en la plantilla.

1. Selecciona **Agent** en el selector de agentes de Chat.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.

> [!CAUTION]
> No se recomienda un modelo en particular. Se utilizó Claude Sonnet 4.5 al crear este taller para obtener resultados relativamente coherentes, pero las sugerencias pueden variar. Es normal que los resultados de un modelo de lenguaje grande (LLM) sean probabilísticos.

> [!NOTE]
> Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía este mensaje:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren las incidencias.
5. Revisa la lista y las prioridades sugeridas.

## Revisar los archivos de instrucciones

Antes de pedirle al agente que genere código, revisa el archivo de instrucciones de interfaz de usuario (UI), que explica cómo añadir funciones al sitio.

1. Abre `.github/instructions/ui.instructions.md` en el codespace.
2. Revisa la arquitectura, los principios de diseño de componentes, las pautas de accesibilidad y pruebas, y los enlaces a las instrucciones de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos y ejemplos. Las rutas relativas facilitan la reutilización y permiten dividir pautas grandes en otras más manejables.

## Implementar el filtrado

Para completar el filtrado hay que actualizar la lógica de `src/lib/`, añadir o modificar las pruebas y actualizar la página de listado con la interfaz de filtrado. Las pruebas deben ejecutarse correctamente antes de fusionar.

1. Continúa en la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** está seleccionado.
4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Envía este mensaje:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza la capa de datos, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar, modificar archivos y ejecutar pruebas. Puede tardar unos minutos. Durante el proceso quizá veas botones **Keep** y **Undo**; no hace falta seleccionarlos mientras el agente trabaja.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.
   ![Cuadro de diálogo de Copilot Chat para confirmar que quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)
8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.
   ![Sesión completa de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)
9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o por cualquier herramienta de IA.

10. Vuelve al navegador con el sitio y prueba la nueva funcionalidad.
11. Cuando hayas revisado el código y confirmado que funciona, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has usado el modo de agente para añadir funcionalidades a Tailspin Toys y has aprendido a explorar el proyecto, coordinar cambios y revisar las pruebas antes de fusionar.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

La lista de juegos necesitará paginación cuando crezca. Pide a Copilot que implemente la paginación, siguiendo las instrucciones existentes. Decide si quienes usan el sitio pueden elegir el tamaño de página o si debe ser fijo. Incluye el contexto necesario en el mensaje e itera con Copilot según haga falta.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
