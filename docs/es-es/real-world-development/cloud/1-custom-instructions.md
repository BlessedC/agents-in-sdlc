---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información adicional, conviene proporcionar ese contexto. Los [archivos de instrucciones][instruction-files] indican a Copilot no solo *qué* hacer, sino también *cómo* hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones del repositorio y archivos específicos de rutas.
- añadirás un estándar general a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no compararás el resultado de un mensaje antes y después: el agente en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que su efecto es más difícil de mostrar en tiempo real. Verás la influencia de las instrucciones más adelante, al revisar las PR del agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para el desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su propósito.

Usarás archivos de instrucciones para que Copilot trabaje de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y los comentarios que quieres. Hay dos tipos:

- `.github/copilot-instructions.md`: un único archivo que se envía en todas las solicitudes del repositorio y que contiene contexto general, como la pila tecnológica, el proyecto, las prácticas recomendadas y otras pautas globales.
- `.github/instructions/*.instructions.md`: archivos con instrucciones para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> Al trabajar en un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerir las siguientes ediciones. Copilot Chat, Copilot CLI y el agente en la nube también utilizan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo`. Copilot admite otros formatos, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

- Mantén `copilot-instructions.md` centrado en pautas generales del proyecto, como su descripción, estructura y estándares globales.
- Usa `*.instructions.md` para pautas específicas de tipos de archivo o tareas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de cómo debería ser el código.

No existe una única forma de crear archivos de instrucciones. Experimenta para descubrir qué funciona en tu proyecto.

> [!TIP]
> Los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de instrucciones. Puedes explorar las que ya incluye este proyecto, por ejemplo, las de [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions]. Copilot también puede generar instrucciones. Para obtener plantillas, consulta [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones del proyecto

El repositorio incluye un `copilot-instructions.md` principal y archivos `*.instructions.md` para tareas diferentes. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md` y observa la descripción y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas pautas se aplican a las interacciones con Copilot.
2. Explora `.github/instructions`. Allí encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros temas.
3. Abre `.github/instructions/unit-tests.instructions.md` y observa que `applyTo` define un patrón para los archivos a los que se aplican las instrucciones. Por ejemplo, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
4. Revisa las instrucciones específicas para pruebas unitarias.
5. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y archivos existentes. Las rutas son relativas a ese archivo.

> [!NOTE]
> La sección **Code formatting requirements** documenta los estándares del proyecto, pero aún no exige documentación en el código. Añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Añadir un estándar para todo el repositorio

Añadirás reglas para los comentarios TSDoc y los encabezados de archivo. El agente en la nube leerá estas instrucciones cuando trabaje en incidencias posteriores.

1. En el terminal del codespace, crea una rama:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abre `.github/copilot-instructions.md` y localiza **Code formatting requirements**, cerca de la línea 27.
3. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda el archivo.

> [!TIP]
> Las instrucciones del repositorio son adecuadas para estándares generales, como el requisito de incluir comentarios de documentación. Los archivos `*.instructions.md` sirven para pautas de lenguajes, tipos de archivo o tareas concretas.

## Confirmar, enviar y fusionar la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. En el ejercicio siguiente partirá de `main`, así que la actualización debe llegar a `main`.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y fusiónala.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir la rama y confirmar el cambio directamente allí. La rama sirve para practicar un flujo habitual en un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza las instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica a las PR del agente en la nube durante el resto del taller.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para mejorar las instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
