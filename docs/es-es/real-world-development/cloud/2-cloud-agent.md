---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: incidencias de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en la lista de tareas pendientes. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una PR en borrador para que una persona desarrolladora revise el trabajo. Así se pueden delegar tareas tediosas y centrarse en las prioridades de mayor alcance.

Explorarás cómo usar el agente en la nube para:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica. La documentación de la primera versión del sitio está incompleta; como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar, en cada página de detalles, otros juegos de la misma categoría para probar la experiencia. Aunque no hace falta una implementación pulida, esta tarea está bloqueando las pruebas de aceptación. Son buenos ejemplos de tareas que se pueden asignar al agente para que trabaje de forma asíncrona mientras tú te ocupas de otras prioridades.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a una persona desarrolladora. Puedes, por ejemplo, [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Copilot crea una PR en borrador, prepara el entorno y empieza a trabajar. Puedes consultar la sesión mientras se ejecuta o después de que termine. Cuando esté lista para revisar, te mencionará en la PR.

## La importancia de acotar las instrucciones

GitHub Copilot no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas; por eso, [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un asistente de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

Antes de asignar trabajo al agente, revisa los archivos de instrucciones que ya existen. Abre en github.com o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Al asignar la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo de la PR resultante.

## Configurar el entorno de desarrollo del agente

El agente utiliza [GitHub Actions][github-actions] para preparar el entorno. Puedes personalizarlo mediante el flujo de trabajo de configuración `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. En este taller ya se ha configurado para instalar Node.js y las dependencias, los navegadores de Playwright para las pruebas de extremo a extremo y la base SQLite del proyecto Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna a estos pasos los permisos mínimos necesarios. Copilot recibirá su propio token para las operaciones.
    permissions:
      # Para clonar el repositorio durante la configuración (por ejemplo, para instalar dependencias), necesitarás el permiso `contents: read`. Si no lo clonas aquí, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (ahora toda la aplicación usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene dos detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutarlo manualmente desde la pestaña **Actions** para comprobar que funciona.

## Añadir documentación

La falta de documentación puede ralentizar el trabajo y dificultar el mantenimiento o la incorporación de nuevas personas. Asignarás una incidencia para que Copilot añada comentarios de documentación.

1. Abre el repositorio en github.com.
2. Selecciona la pestaña **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Escribe `Code lacks documentation` en **Title**.
5. En **Description**, escribe:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. Selecciona **Create** y, en el panel derecho, **Assign to Copilot**.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

7. Selecciona **Assign**.
   ![Detalles de asignación de Copilot a una incidencia](../../../_images/ex4-assign-copilot-details.png)

8. Selecciona la pestaña **Pull Requests** y abre la PR que Copilot ha generado, con un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
9. Espera a que aparezca la lista de tareas de Copilot; puede tardar varios minutos mientras prepara el entorno y analiza el proyecto. Revisa las tareas que va a completar.
10. En la cronología de la PR, selecciona **View session** para ver la sesión activa.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

Observa cómo Copilot explora el código, analiza el estado del proyecto, planifica e implementa cambios. Mientras el agente trabaja, asígnale otra tarea y dedica el tiempo a programar una funcionalidad.

## Añadir una sección de juegos relacionados

También asignarás a Copilot la tarea de añadir juegos relacionados a la página de detalles.

1. En el repositorio de github.com, abre **Issues** y selecciona **New issue**.
2. Selecciona **Blank issue**.
3. Escribe `Show related games on the game details page` como título.
4. En la descripción, pega:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. Crea la incidencia y selecciona **Assign to Copilot** en el panel derecho.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

6. Selecciona **Assign**. Verás el emoji 👀 en el primer comentario de la incidencia cuando Copilot empiece a trabajar.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente trabajará en la segunda solicitud de forma asíncrona. No hace falta supervisarlo continuamente; revisa el resultado cuando termine. Ahora puedes crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

Has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje en segundo plano y se ocupe de deuda técnica, funcionalidades o migraciones entre marcos de trabajo. En este ejercicio:

- has personalizado el entorno de generación de código.
- has comprobado cómo asegurar las operaciones.
- has definido incidencias con un alcance claro.
- has asignado incidencias a Copilot.

Mientras Copilot trabaja, aprenderás a crear y usar agentes personalizados. El [agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
