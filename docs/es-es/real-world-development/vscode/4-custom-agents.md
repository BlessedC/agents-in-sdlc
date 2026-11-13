---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del desarrollo. Al definirlos en archivos Markdown del directorio `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos. Los equipos pueden convertir sus conocimientos en agentes reutilizables, como uno de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad o uno para pruebas.

Los agentes se definen en archivos Markdown de `.github/agents` del proyecto o, globalmente, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, los campos `name` y `description`, seguido de las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los [agentes personalizados][custom-agents-concept] y las [habilidades de agente][agent-skills-concept] se parecen: ambos se definen mediante archivos Markdown y explican a la IA cómo realizar tareas. Una forma sencilla de distinguirlos: el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes tienen su propia ventana de contexto y coordinan habilidades u otros agentes. En este laboratorio, el agente de accesibilidad revisará el sitio según las pautas de accesibilidad y podría utilizar una habilidad para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de escribir un agente personalizado. Prueba e itera para encontrar lo que funcione en tu entorno y tus situaciones de uso.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible a todo el mundo, con independencia de sus capacidades visuales o preferencias. Algunas personas indican que el tema oscuro es difícil de leer por el bajo contraste. El equipo de diseño pide implementar un modo de alto contraste que se pueda activar y desactivar.

## Revisar el agente de accesibilidad

Ya hay un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guía a Copilot.

1. Vuelve a tu codespace y crea una rama nueva basada en `main` para mantener separada la tarea de accesibilidad de la PR de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md` y observa los campos `name` y `description` del frontmatter.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las responsabilidades, las prácticas de accesibilidad, los ejemplos de HTML, CSS y JavaScript, y los errores habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes los agentes personalizados de `.github/agents`. Seleccionar uno limita la conversación a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; las instrucciones de cada paso indican lo que debes seleccionar.

1. En el selector de agentes de Chat, selecciona **Agent** si todavía no lo está.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. En la lista de agentes, selecciona **Accessibility agent**.
3. Envía este mensaje:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla. El indicador **Files changed** mostrará el conjunto de trabajo modificado.

> [!NOTE]
> El proceso tardará varios minutos. Copilot editará componentes Astro, CSS y las pruebas relacionadas.

Revisarás y orientarás este trabajo en curso en el ejercicio siguiente.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir los estándares y conocimientos del equipo en agentes reutilizables. En este ejercicio:

- has revisado cómo se definen los agentes personalizados.
- has utilizado un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios y añadirás un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
