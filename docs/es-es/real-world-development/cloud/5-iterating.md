---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

En este laboratorio has trabajado con GitHub Copilot en varias tareas de mejora de la experiencia y nuevas funcionalidades: añadir documentación al código, crear una funcionalidad de juegos relacionados y desarrollar funciones de accesibilidad con modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisarás las propuestas de documentación, juegos relacionados y accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

El agente trabaja de forma asíncrona y sin supervisión, por lo que existen medidas de seguridad:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura **solo** a la rama en la que trabaja.
- El agente se ejecuta en GitHub Actions, dentro de un entorno temporal e independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empieza por la primera PR de Copilot, que añade documentación al código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que aparezca un aviso de que Copilot está bloqueado por un cortafuegos. Es lo esperado: el acceso a recursos externos, incluidos los servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [configurar o deshabilitar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com y selecciona **Pull Requests**.
2. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

3. Selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

4. Examina el código actualizado, que incluye comentarios TSDoc y otra documentación. Los cambios exactos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo que indican las instrucciones personalizadas que revisaste al principio del ejercicio 2.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. Comprueba que los flujos aparezcan en las comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro terminarán correctamente; pueden tardar unos minutos.

## Pedir cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la PR o en comentarios insertados en el código. Copilot leerá el comentario e iniciará otra sesión para resolverlo. Los resultados son no deterministas, así que no hay un mensaje único que debas usar. Por ejemplo, puedes pedirle:

- que añada encabezados breves al principio de cada archivo de código.
- que añada comentarios TSDoc a archivos TypeScript y Astro.
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario y menciona a **@copilot** como mencionarías a otra persona del equipo. Usa una de las ideas anteriores o pide otra mejora de documentación.
2. Selecciona **View Session** para observar el trabajo de Copilot. El agente iniciará una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, marca la PR como lista para revisión y fusiónala en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Ahora revisarás la PR creada por Copilot para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio de GitHub y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
3. Selecciona **Files changed** y revisa el código generado.
4. Cuando termines, vuelve a **Conversation**. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

5. Comprueba que las comprobaciones del proyecto para la aplicación Astro terminen correctamente.
6. **Opcional:** cambia a la rama de Copilot en tu Codespace y prueba manualmente la funcionalidad. Sustituye `<branch-name>` por el nombre de la rama (por ejemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Puedes seguir iterando con el agente en la nube y pedirle que ajuste el número de juegos relacionados o que añada encabezados y comentarios TSDoc. Como esta tarea se asignó antes de actualizar las instrucciones personalizadas, quizá tengas que pedirle que las aplique. Añade una solicitud en la pestaña **Conversation**; Copilot la recibirá y empezará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisarás la PR del agente personalizado de accesibilidad. Debe incluir tanto el modo de alto contraste solicitado en el ejercicio 3 como el modo claro solicitado en mission control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la marca **[WIP]**. Espera unos minutos a que termine.

3. Selecciona **Files changed** y revisa la implementación. Presta atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como etiquetas ARIA y navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los modos.
4. Vuelve a **Conversation**. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows** y comprueba los resultados.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

5. **Opcional:** cambia a la rama en tu Codespace y prueba manualmente las funciones:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que la preferencia se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha guiado a Copilot para aplicar prácticas recomendadas. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario para solicitar cambios.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para delegar tareas reales de la lista de trabajo. Para practicar, crea nuevas incidencias y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para personas que apoyan los juegos, en la página de detalles.
- implementar la paginación de la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- has inspeccionado las instrucciones personalizadas del repositorio y observado su efecto en el trabajo del agente.
- has asignado incidencias al agente en la nube y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- has creado y utilizado un agente personalizado para accesibilidad y has añadido modos de alto contraste y claro.
- has usado la página de agentes de Copilot como centro de control para supervisar y orientar la sesión de accesibilidad.
- has revisado las PR del agente en la nube, has pedido cambios con `@copilot` y has aprobado los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Para seguir aprendiendo, explora los otros entornos:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: publica dos cambios revisados desde el terminal con Plan, Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y la opción `/delegate` para volver al agente en la nube.

En tu propio repositorio, también puedes:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
