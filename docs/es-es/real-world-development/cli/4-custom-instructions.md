---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene proporcionar esa orientación a Copilot. Los [archivos de instrucciones][instruction-files] describen tanto *qué* código quieres como *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizarás el archivo de instrucciones para que se respeten los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene pautas y requisitos para el desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc; también deben documentar el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Hay que conservar las pautas de formato y lint existentes.

Usarás archivos de instrucciones para que Copilot trabaje conforme a estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y el tipo de comentarios que quieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general aplicable a la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`: archivos con pautas para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> Los formatos de instrucciones disponibles varían según el entorno. Consulta la [referencia de compatibilidad de las instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar las instrucciones personalizadas del proyecto

El proyecto inicial ya incluye archivos de instrucciones. Examínalos antes de modificarlos para observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Lee la descripción breve del proyecto y las pautas de programación, aplicables a todas las interacciones con Copilot en este repositorio.
4. Explora los archivos de `.github/instructions`. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`, que indica a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a archivos existentes. Así se pueden dividir instrucciones grandes en otras más pequeñas y reutilizables, y señalar ejemplos que Copilot pueda seguir.

## Actualizar las instrucciones del equipo

Añadirás a `copilot-instructions.md` una regla para incluir comentarios TSDoc en el TypeScript nuevo.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, aproximadamente en la línea 35.
2. Añade esta viñeta al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar el cambio, cierra y reanuda la conversación de filtrado para que Copilot cargue la nueva guía sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc y comprueba que describen el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza las instrucciones del proyecto y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y las específicas de rutas.
- has actualizado las instrucciones para que se respete el estándar de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para ejecutar las pruebas y las comprobaciones de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
