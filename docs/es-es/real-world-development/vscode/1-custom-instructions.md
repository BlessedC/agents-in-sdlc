---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene compartir esa información con Copilot. Los [archivos de instrucciones][instruction-files] permiten proporcionar pautas sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto, las pautas de programación y los estándares de documentación mediante instrucciones del repositorio y archivos específicos de rutas.
- generarás la primera parte de la capa de datos para el filtrado (una función auxiliar para obtener quienes publican), con las instrucciones actuales.
- añadirás un estándar general a `.github/copilot-instructions.md`.
- repetirás el mismo mensaje y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás los cambios en `main` para que Copilot use las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot es no determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener siempre el mismo código.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas para el desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su propósito.

Usarás archivos de instrucciones para que Copilot trabaje de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda el estilo de programación y los requisitos. Puedes indicar convenciones, bibliotecas y tipos de comentarios. Hay dos tipos:

- `.github/copilot-instructions.md`: un único archivo que se envía en todas las solicitudes del repositorio y que contiene contexto general aplicable a la mayoría de los mensajes de chat o CLI.
- `.github/instructions/*.instructions.md`: archivos con instrucciones para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> En un IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni sugerir la siguiente edición. Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`. También se admiten otros formatos, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas

- Mantén `copilot-instructions.md` centrado en la información general del proyecto y los estándares globales.
- Usa `*.instructions.md` para pautas de tipos de archivo, como pruebas unitarias, componentes Astro o la capa de datos, o para tareas concretas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos.

No existe una única forma de crear instrucciones; experimenta para descubrir qué funciona en tu proyecto.

> [!TIP]
> Este proyecto incluye instrucciones para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions]. Copilot también puede generar instrucciones. Para obtener plantillas, explora [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones del proyecto

El repositorio incluye un `copilot-instructions.md` principal y archivos `*.instructions.md` para tareas diferentes. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md` y observa secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**, incluidas las pautas anidadas de **GitHub Actions Workflows**.
2. Explora `.github/instructions`. Allí encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
3. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en `applyTo`, que determina qué archivos coinciden, por ejemplo, los que siguen el patrón `**/*.test.ts`.
4. Revisa las pautas específicas para pruebas unitarias.
5. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y ejemplos. Las rutas son relativas a ese archivo.

> [!NOTE]
> **Code formatting requirements** documenta los estándares, pero aún no exige documentación en el código. Añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crear una rama para los cambios

1. Vuelve al codespace y abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Crea y selecciona una rama nueva:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada paso indican lo que debes seleccionar.

Primero enviarás un mensaje con las instrucciones actuales. Después las actualizarás y repetirás el mismo mensaje para comparar el resultado.

1. Cierra las pestañas abiertas del editor para limitar el contexto.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. En la vista Chat, selecciona **Agent** en el selector de agentes.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, a menudo en la función auxiliar y sus pruebas.
6. La función auxiliar propuesta tendrá tipos, recibirá un cliente `db` como primer argumento y devolverá una matriz tipada de quienes publican; esas convenciones proceden de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de archivo.

> [!CAUTION]
> Copilot puede añadir comentarios aunque no se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añadir un estándar para todo el repositorio

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements**, cerca de la línea 27. Todavía no requiere documentación en el código.
3. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones del repositorio son adecuadas para estándares generales; los archivos `*.instructions.md` sirven para lenguajes, tipos de archivo y tareas concretas.

## Repetir el mensaje y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat**.
2. Abre `src/lib/publishers.ts` para centrar el contexto de Copilot en ese archivo.
3. Envía el mismo mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo puede empezar con un bloque parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de quienes publican en la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar la información de quienes publican en la base de datos.
    */
   ```

5. La función también puede incluir un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de quienes publican con su identificador y nombre.
    *
    * @param db - Cliente de base de datos Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de quienes publican.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código que se utilizará en el siguiente ejercicio.

## Confirmar las instrucciones y enviar la rama

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba los cambios:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una solicitud de incorporación de cambios

Asocia la actualización de instrucciones y la función auxiliar con la incidencia correspondiente.

1. Abre Copilot Chat y selecciona **Agent**.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat.
3. Pide a Copilot que encuentre la incidencia y cree una PR desde la rama actual:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot pida permiso para realizar acciones en GitHub, revisa la solicitud y selecciona **Allow** si corresponde.
5. Cuando cree la PR, pídele que la fusione y vuelva a `main` local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revisa las solicitudes de permiso para acciones de GitHub y comandos del shell antes de seleccionar **Allow**.

¡Has creado y fusionado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza las instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar general a `.github/copilot-instructions.md`.
- repetir el mensaje y observar cómo el código incorpora el estándar.
- confirmar las instrucciones y la función auxiliar en `main` para que el siguiente ejercicio pueda utilizarlas.

A continuación, aplicarás las instrucciones en [modo de agente][next-lesson] mientras Copilot añade una funcionalidad.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para mejorar las instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
