---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del desarrollo. Al definirlos en archivos Markdown del directorio `.github/agents`, puedes darles instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad o uno que mantenga patrones de pruebas coherentes.

Los archivos de agentes personalizados pueden estar en `.github/agents` del proyecto o, globalmente, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con al menos `name` y `description`, seguido de instrucciones Markdown que definen el comportamiento, la experiencia y las pautas del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen: ambos se definen principalmente mediante archivos Markdown e indican a la IA cómo realizar tareas. Una forma sencilla de distinguirlos: el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este ejercicio, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; puede usar habilidades como una para gestionar solicitudes de incorporación de cambios u otra para ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de escribir un agente personalizado. Como con cualquier tecnología de IA, prueba distintas opciones y adáptalas al entorno y a las situaciones de uso.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio aprenderás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible a todas las personas. Algunos comentarios indican que el tema oscuro resulta difícil de leer por el bajo contraste entre el texto y el fondo. El equipo de diseño pide implementar un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que utilizarás un agente personalizado para generar la funcionalidad cuanto antes.

## Revisar el agente personalizado de accesibilidad

Ya hay un agente personalizado para accesibilidad. Revisa su contenido para entender cómo guía a Copilot.

1. Vuelve a tu codespace y abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las responsabilidades principales, las prácticas recomendadas de accesibilidad, los ejemplos de HTML, CSS y JavaScript, y los errores habituales.

## Crear y asignar una incidencia

Mission control es el centro para trabajar con agentes: puedes asignar tareas, supervisar su progreso y orientar la sesión.

1. En tu repositorio, abre **Issues**, selecciona **New issue** y, después, **Blank issue**.
2. Escribe `Add high contrast mode to website` como título.
3. En la descripción, escribe:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

4. Selecciona **Create** y, en el panel derecho, **Assign to Copilot**.
5. Selecciona **Accessibility agent** en la lista de agentes personalizados y, después, **Assign**.

    ![Vista de asignación del agente en la nube con el agente de accesibilidad seleccionado](../../../_images/ex5-select-custom-agent.png)

Copilot empezará a trabajar en segundo plano.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir los estándares y conocimientos del equipo en agentes reutilizables. En este ejercicio:

- has revisado un agente personalizado de accesibilidad.
- has asignado una tarea al agente.

Mientras Copilot implementa el modo de alto contraste, aprenderás a [supervisar y orientar la sesión][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
