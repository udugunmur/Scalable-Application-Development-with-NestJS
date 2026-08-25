# Parte 1: Introducción a NestJS y Arquitectura de Aplicaciones Escalables

## Capítulo 3: Configuración del Entorno de NestJS y Exploración de NestJS – Construcción de una Aplicación Robusta

NestJS es un framework destacado para construir aplicaciones del lado del servidor escalables y mantenibles. Este capítulo proporciona una guía completa para ayudarte a liberar todo su potencial.

Para sentar las bases, iniciaremos la instalación de Node.js y NestJS, incluidas las herramientas de interfaz de línea de comandos (CLI) necesarias. Esto asegura que tengas todo lo que necesitas para comenzar tu viaje de desarrollo. Posteriormente, configuraremos herramientas esenciales como npm y TypeScript, facilitando tu proceso de desarrollo.

Con tu entorno listo, te presentaremos la estructura de un proyecto NestJS, brindándote la confianza para explorar y comprender sus componentes. El viaje continúa con conocimientos prácticos sobre la creación y gestión de módulos, controladores y proveedores. Descubre cómo manejar excepciones con elegancia utilizando filtros y mantener la integridad de los datos mediante la validación con pipes y guards.

Al final del capítulo, poseerás un conjunto de habilidades esenciales: configurar un entorno NestJS, organizar tu aplicación de manera efectiva y crear aplicaciones listas para enfrentar los desafíos del mundo real. Estas capacidades no solo elevan tu destreza en aplicaciones del lado del servidor, sino que también sientan las bases para las mejores prácticas en capítulos posteriores.

Los temas clave en este capítulo incluyen los siguientes:

- Instalación de Node.js y NestJS
- Creación de la estructura base de un nuevo proyecto NestJS (*Scaffolding*)
- Comprensión de la estructura de un proyecto NestJS
- Creación y gestión de módulos
- Construcción de controladores para manejar peticiones
- Implementación de proveedores para la lógica de negocio
- Manejo elegante de excepciones con filtros
- Validación de datos mediante pipes y guards

Prepárate para equiparte con las herramientas y el conocimiento para construir aplicaciones NestJS robustas, escalables y mantenibles. ¡Empecemos!

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Instalación de Node.js y NestJS

Para comenzar tu viaje con NestJS, es imprescindible configurar tu entorno de desarrollo con precisión. En esta sección, te guiaremos a través de la instalación de Node.js y NestJS: la base de las aplicaciones robustas del lado del servidor.

Node.js sirve como el entorno de ejecución que permite la ejecución de JavaScript en el servidor. NestJS lo complementa simplificando el proceso de diseñar arquitecturas escalables para aplicaciones Node.js. Juntos, forman una pareja inigualable en el ámbito del desarrollo web moderno.

Aquí hay un desglose de la instalación:

#### Paso 1 – Instalar Node.js

Para aquellos que ya cuentan con Node.js (versión 16 o superior), siéntanse libres de avanzar. Si no, preparémonos:

1. **Descarga Node.js:** Dirígete al sitio web oficial de Node.js ([https://nodejs.org/](https://nodejs.org/)) y obtén la versión de Soporte a Largo Plazo (*Long-Term Support* - LTS) adaptada a tu sistema operativo.
2. **Ejecuta el instalador:** Abre el instalador descargado y sigue sus indicaciones. Esto no solo instala Node.js sino también npm por defecto.
3. **Verifica la instalación:** Confirma las instalaciones de Node.js y npm comprobando sus versiones a través de tu terminal:

```bash
$ node -v 
$ npm -v
```

#### Paso 2 – Instalar la CLI de NestJS

Para instalar la CLI de NestJS antes de comenzar, debes seguir estos sencillos pasos:

1. **Instala la CLI de NestJS:** Con Node.js en su lugar, instala la CLI de NestJS a nivel global en el sistema. Esta CLI ofrece herramientas robustas para aplicaciones NestJS:

```bash
$ npm install -g @nestjs/cli
```

2. **Verifica la instalación:** Comprueba la instalación de la CLI revelando su versión:

```bash
$ nest --version # or use $ nest info
```

¡Bravo! Has sentado las bases con las instalaciones de Node.js y la CLI de NestJS. El framework ofrece un entorno de ejecución, mientras que la CLI presenta herramientas vitales para esculpir aplicaciones NestJS.

A continuación, refinaremos tu espacio de desarrollo configurando npm y TypeScript, fortaleciéndolo para el desarrollo con NestJS. ¡El desarrollo de tus aplicaciones del lado del servidor está a punto de recibir un gran impulso!

---

### Creación de la estructura base de un nuevo proyecto NestJS (*Scaffolding*)

Ahora que tienes listos Node.js, npm y `nest-cli`, es hora de crear tu primer proyecto NestJS. NestJS proporciona una práctica herramienta de línea de comandos para generar la estructura base de un nuevo proyecto con todo el código repetitivo necesario. En esta sección, recorreremos el proceso de creación de una nueva aplicación NestJS, paso a paso.

> **Nota importante**  
> TypeScript se incluye automáticamente cuando inicializas un proyecto NestJS, por lo que no es necesario configurarlo por separado.

#### Creación de un nuevo proyecto NestJS

Con Node.js, npm y `nest-cli` listos y esperando, los astros se han alineado para que te embarques en la creación de tu primer proyecto NestJS. NestJS despliega la alfombra roja con una herramienta de línea de comandos fácil de usar, asegurando que tu proyecto despegue sin problemas. Juntos, navegaremos este proceso, asegurándonos de que cada paso sea claro y preciso.

> **Dato rápido**  
> NestJS ya incluye TypeScript de serie cuando inicias un proyecto. ¡Eso es una preocupación menos!

#### El nacimiento de un proyecto NestJS

Ahora, creemos nuestro primer proyecto siguiendo estos sencillos pasos:

#### Paso 1 – Preparar el escenario

Desde la terminal que prefieras, navega hasta la carpeta donde deseas ubicar tu proyecto NestJS. Digamos que quiero que el proyecto esté en `Documents/nestjs`.

#### Paso 2 – Da vida a tu creación con un comando

Invoca el poder de la CLI de NestJS con el siguiente comando. Reemplaza `my-nest-app` con un nombre que resuene con tu visión:

```bash
$ nest new my-nest-app
```

A medida que se ejecuta el comando, la CLI de NestJS te solicitará un par de opciones: preferencia del administrador de paquetes y la inclusión de una aplicación inicial. Para esta odisea, optaremos por `yarn` y adoptaremos la aplicación de ejemplo.

La Figura 3.1 muestra cómo debería verse la terminal al inicializar un nuevo proyecto NestJS:

*Figura 3.1: Tu terminal tras inicializar un nuevo proyecto NestJS*

#### Paso 3 – Sumérgete en el laberinto de tu proyecto

El ritual de estructuración inicial ha concluido. Ahora, viaja al corazón de tu proyecto:

```bash
$ cd my-nest-app # Pop open your favorite IDE to behold the structured elegance NestJS offers.
```

La Figura 3.2 muestra aproximadamente cómo debería verse la estructura del proyecto:

*Figura 3.2: La nueva estructura del proyecto NestJS*

Permíteme ser tu guía mientras recorremos este reino digital:

- **`src/`:** El epicentro que alberga el alma de tu aplicación; consiste en módulos, controladores, proveedores y más, todo alineado con la filosofía de TypeScript.
- **`dist/`:** El fruto de tu trabajo; los archivos JavaScript transpilados descansan aquí una vez que ordenas a tu aplicación salir al mundo.
- **`node_modules/`:** La caja de herramientas; cada engranaje, rueda y tornillo (es decir, las dependencias) que hace funcionar tu proyecto está aquí.
- **`test/`:** Tus campos de pruebas; los tests que aseguran la fuerza y resiliencia de tu aplicación encuentran aquí su hogar.
- **`tsconfig.json` y `nest-cli.json`:** Los libros de reglas; estos son los planos de configuración para TypeScript y la CLI de NestJS, respectivamente.

#### Paso 4 – Enciende tu aplicación de ejemplo

Antes de seguir adelante, veamos cómo se ve la aplicación que acabamos de crear cuando la iniciamos, usando el siguiente comando desde el directorio raíz del proyecto:

```bash
$ npm run start:dev
```

Observa cómo tu aplicación NestJS se inicializa en el puerto asignado, típicamente el puerto 3000 por defecto. Deja que tu navegador sea la ventana a este nuevo mundo en [http://localhost:3000](http://localhost:3000/). Disfruta de la cálida bienvenida que te brinda tu aplicación de ejemplo. Esto realiza una simple petición GET a la API que acabas de crear.

La Figura 3.3 muestra cómo debería verse tu terminal al ejecutar el script `start:dev`:

*Figura 3.3: Tu terminal al iniciar el servidor – hermoso, ¿verdad? :)*

La Figura 3.4 muestra la petición a la API desde el navegador web:

*Figura 3.4: NestJS saluda*

¡Victoria! 🎉 No solo has generado la estructura inicial, sino que también has dado vida a un proyecto NestJS completamente nuevo. El horizonte está lleno de aventuras: módulos, controladores, proveedores y mejores prácticas que moldearán tu proyecto en un referente de excelencia.

Acabamos de emprender nuestro viaje con NestJS creando con éxito la estructura de un nuevo proyecto, familiarizándonos con su configuración inicial y ejecutando nuestra aplicación de ejemplo. Con este paso fundacional completado, estamos listos para profundizar más.

A continuación, descifremos las complejidades de la estructura del proyecto NestJS y comprendamos verdaderamente la columna vertebral de nuestra aplicación.

---

### Comprensión de la estructura de un proyecto NestJS

Al ingresar al corazón de NestJS, uno reconoce de inmediato su combinación única de elegancia y diseño orientado a propósitos. Cada directorio y cada archivo tiene un lugar y un propósito. Comprender la estructura del proyecto NestJS es primordial, no solo por el bien de la navegación, sino también para captar verdaderamente su filosofía.

#### ¿Por qué es importante la estructura del proyecto de NestJS?

NestJS se inspira en lenguajes y herramientas maduros, alineándose estrechamente con el diseño modular de Angular (https://angular.io/guide/feature-modules). Este enfoque tiene como objetivo mantener tu base de código mantenible, escalable y organizada, permitiendo a los desarrolladores —tanto novatos como expertos— localizar archivos fácilmente y comprender el flujo y las relaciones dentro de la aplicación.

Consulta la Figura 3.2 para obtener una visión general de cómo se estructura el proyecto de forma predeterminada. A medida que la aplicación crezca, la estructura puede evolucionar, pero ten en cuenta que el rol de cada directorio nos facilitará la vida:

#### El directorio `src/`

Podría decirse que es el directorio más accedido; `src/` es donde ocurre la magia. Aquí residen tus módulos, controladores, proveedores y la lógica central de tu aplicación:

- **Módulos:** Encapsulan dominios específicos de la aplicación, fomentando una separación limpia de responsabilidades. Piensa en ellos como secciones distintas de tu aplicación que pueden operar de forma independiente pero forman parte del todo. Todos los puntos de entrada de los módulos deben nombrarse como `*.module.ts` por convención; esto ayudará en términos de mantenimiento y legibilidad del código.
- **Controladores:** Manejan las peticiones HTTP entrantes y las enrutan a sus funciones de servicio correspondientes. Todos los archivos de controlador deben nombrarse como `*.controller.ts` por convención.
- **Proveedores:** Representan una combinación de servicios, repositorios, fábricas y más. Realizan la lógica de negocio real, manteniendo los controladores delgados y enfocados.

Actualmente, nuestra aplicación tiene un módulo único (el módulo de la aplicación) y su punto de entrada es `app.module.ts`. En una aplicación del mundo real, lo más probable es que tengamos más de un módulo. Mantenerlo todo en una sola carpeta (`src/` como está por defecto) puede hacer que nuestra carpeta `src/` sea difícil de mantener. Se recomienda crear más subcarpetas dentro de `src/` y mantener cada módulo por separado. La Figura 3.6 muestra un ejemplo de una carpeta `src/` con un montón de módulos frente a otra con una estructura bien organizada. Recuerda, NestJS solo nos da una dirección, pero la forma en que lo organicemos estará principalmente en nuestras manos.

La Figura 3.5 muestra la carpeta `src/` inicial creada por `nest-cli` cuando inicializamos el proyecto:

*Figura 3.5: La carpeta src/ inicial*

Cuando nuestro proyecto crece, NestJS nos brinda la flexibilidad de estructurar la carpeta `src/`. Sin embargo, para mantenernos en la filosofía de NestJS y seguir su estructura nativa, podemos usar la CLI para generar recursos para la aplicación, de modo que NestJS se encargue de colocar esos componentes en el lugar correcto.

La Figura 3.6 muestra dos estructuras de proyectos diferentes. La primera muestra únicamente módulos mezclados y sus componentes juntos dentro de la misma carpeta `src/`, mientras que la segunda separa las responsabilidades agrupando los componentes, utilizando la CLI de NestJS. Es posible que terminemos teniendo la segunda estructura y puedes ver claramente lo fácil que puede ser mantener la segunda en lugar de la primera:

*Figura 3.6: Separar módulos en diferentes carpetas bajo la carpeta src/ hace que nuestra estructura de código sea más legible*

Además, y muy importante, en la carpeta `src/` tenemos nuestro punto de entrada principal a la aplicación, que es el archivo `main.ts`. Actualmente, así es como se ve el archivo:

```typescript
import { NestFactory } from '@nestjs/core'; import { AppModule } from './app.module'; async function bootstrap() { const app = await NestFactory.create(AppModule); await app.listen(3000); } bootstrap();
```

Esta es simplemente una función de JavaScript que ejecutamos, lo que nos permite configurar un puerto. Cuando lleguemos a casos de estudio específicos, veremos cuándo, por qué y cómo podemos mejorar este archivo inicial para que coincida con los requisitos de un proyecto determinado.

#### El directorio `dist/`

Cada vez que preparas tu aplicación para el despliegue o cada vez que inicias tu proyecto, el código TypeScript se transpila a JavaScript y los resultados se almacenan en el directorio `dist/`. Esta es la versión de tu aplicación que se ejecutará, y probablemente nunca la tocaremos ni mantendremos ya que la carpeta se genera en tiempo de ejecución. Por lo general, Git ignorará esta carpeta ya que no tiene sentido subirla a ningún entorno.

#### El directorio `node_modules/`

Una vista común en los proyectos de Node.js, `node_modules/` es donde residen todas las dependencias de tu proyecto. Cada paquete que instalas a través de npm o yarn tiene su propio lugar en este directorio.

#### El directorio `test/`

Fiel a la filosofía de NestJS de construir aplicaciones robustas, el directorio `test/` es donde viven tus pruebas unitarias y de extremo a extremo (*End-to-End* - E2E). Es un testimonio del compromiso del framework con la calidad y la confiabilidad.

Cada archivo de lógica de negocio en la carpeta `src/` (como los archivos de servicio, controlador, etc.) puede tener sus propios archivos de prueba anidados en él. Para pruebas unitarias y de integración, tenemos un capítulo completo sobre pruebas y lo explicaremos en detalle cuando lleguemos al [Capítulo 7](https://subscription.packtpub.com/book/web-development/9781835468609/7).

#### Archivos de configuración: `tsconfig.json` y `nest-cli.json`

Cada aplicación requiere un poco de configuración. El archivo `tsconfig.json` satisface las necesidades de TypeScript, definiendo reglas y detalles para el proceso de transpilación. Por otro lado, `nest-cli.json` proporciona configuración para la CLI de NestJS, ajustando cómo se comporta la CLI e interactúa con tu proyecto.

Al apreciar el proceso de pensamiento detrás de la estructura de directorios de NestJS, te alineas con su ideología. Un proyecto bien estructurado es similar a una biblioteca bien organizada: cada libro tiene su lugar y encontrar lo que necesitas se convierte en una tarea fluida. Con este entendimiento, no solo estás programando; estás creando arte que se alinea con la filosofía de NestJS.

A medida que avanzamos, recuerda que cada parte de NestJS está diseñada a propósito, reflejando su compromiso con la productividad del desarrollador y la mantenibilidad de la aplicación. ¿Listo para profundizar más? Continuemos y exploremos las complejidades de crear módulos, construir controladores e implementar proveedores.

---

### Creación y gestión de módulos

NestJS prospera gracias a su arquitectura modular. Si alguna vez te has maravillado ante un edificio bien construido donde cada habitación tiene un propósito diferenciado, esa es la belleza que NestJS aporta al mundo del desarrollo backend. Este capítulo tiene como objetivo convertirte en el maestro arquitecto de tus aplicaciones NestJS, así que profundicemos en el arte y la ciencia de los módulos.

#### ¿Qué son los módulos en NestJS?

En su núcleo, un módulo es una clase adornada con el decorador `@Module()`. Al igual que las habitaciones de un edificio, los módulos encapsulan características distintas de tu aplicación, asegurando una separación limpia de responsabilidades. Desempeñan un papel fundamental en el mantenimiento de la organización, la escalabilidad y la legibilidad, facilitando la adición, modificación o eliminación de características a medida que evoluciona tu aplicación.

#### El rol del módulo raíz (*root module*)

Entre todos los módulos, hay uno que destaca: el módulo raíz. Piensa en él como la entrada principal de un edificio. El módulo raíz es donde la aplicación comienza su viaje, atrayendo a otros módulos y preparando el escenario para toda la aplicación. Su importancia no se puede subestimar; es donde se registran los módulos, los middlewares e incluso los módulos de terceros, orquestando toda la aplicación.

La Figura 3.7 muestra cómo se estructuran los módulos alrededor del módulo raíz (módulo de la aplicación) y está tomada de la documentación oficial de NestJS (https://docs.nestjs.com/):

*Figura 3.7: Organización de módulos en una aplicación NestJS*

#### Creación de un nuevo módulo

La arquitectura de NestJS te da la capacidad de crear tus módulos como desees. Puedes decidir generar módulos utilizando la CLI o crearlos manualmente. Dejar que la CLI los cree por ti vendrá con un par de beneficios, como colocar el módulo en el lugar correcto o importarlo automáticamente en el módulo raíz, conectando tu nuevo módulo con toda la aplicación. A continuación se explica cómo puedes utilizar ambos métodos.

#### Uso de la CLI de NestJS

La CLI, siendo una navaja suiza para los desarrolladores de NestJS, proporciona un comando conveniente para generar un nuevo módulo:

```bash
$ nest generate module <module-name>
```

En su versión abreviada:

```bash
$ nest g mo <module-name>
```

Este comando no solo crea un nuevo archivo de módulo, sino que también lo registra automáticamente en el módulo padre más cercano (o módulo de la aplicación).

#### Creación manual de un módulo

Para aquellos que disfrutan de la artesanía manual:

1. Crea un nuevo archivo llamado `<module-name>.module.ts`.
2. Compleméntalo con la estructura básica:

```typescript
import { Module } from '@nestjs/common'; @Module({ imports: [], controllers: [], providers: [], }) export class <ModuleName>Module {}
```

3. Importa y registra el módulo en el módulo padre deseado (o en el módulo raíz).

#### Momento de decisión: cuándo crear un módulo

Una pregunta común que surge es cuándo se debe crear un nuevo módulo. La respuesta radica en el principio de responsabilidad única (*Single Responsibility Principle* - SRP). Si una característica o dominio tiene una responsabilidad diferenciada que puede operar de forma independiente o considerarse como una unidad autónoma, es un candidato ideal para un nuevo módulo. Piensa en gestión de usuarios, pagos o notificaciones: cada uno de estos puede ser un módulo. Discutimos mucho sobre el diseño modular en el primer capítulo, así que no dudes en volver a ese capítulo si necesitas un repaso.

> **Momento de cuestionario**  
> Para las siguientes preguntas, escribe las respuestas correspondientes:  
> 1. ¿Qué orquesta el módulo raíz en una aplicación NestJS?  
> 2. ¿Cuál es el comando principal para crear un módulo utilizando la CLI de NestJS?  
> 3. ¿Cuándo es ideal considerar la creación de un nuevo módulo?

> **Nota importante**  
> Recuerda siempre que los módulos no son solo para la separación; ayudan a crear bloques reutilizables. Un módulo bien definido se puede importar fácilmente a otro módulo o incluso a otros proyectos NestJS.

#### Gestión de módulos

Los módulos, una vez creados, a menudo necesitan interactuar. Ya sea que estés compartiendo un servicio entre dos módulos o configurando rutas, gestionar las relaciones entre módulos es crucial. Aquí están las propiedades del decorador `@Module` que nos ayudan a gestionar la relación entre módulos:

- **`imports`:** En el decorador `@Module()`, el arreglo `imports` permite que un módulo use los proveedores exportados de otro módulo.
- **`exports`:** Para hacer que los proveedores, guards o filtros estén disponibles para otros módulos, usa el arreglo `exports` en el decorador `@Module()`.
- **`controllers` y `providers`:** Los controladores definen rutas y manejan peticiones entrantes, mientras que los proveedores encapsulan la lógica. Se declaran en los arreglos `controllers` y `providers`, respectivamente.

En el proyecto inicial de NestJS, tenemos un ejemplo de interacción entre módulos incluso si tenemos un solo módulo. Mira lo que tenemos en el archivo `app.module.ts`:

```typescript
import { Module } from '@nestjs/common'; import { AppController } from './app.controller'; import { AppService } from './app.service'; @Module({ imports: [], controllers: [AppController], providers: [AppService], }) export class AppModule {}
```

Como sugiere este código, el módulo de la aplicación funciona por sí mismo por ahora y no tiene ningún módulo importado. El arreglo `imports` sin duda se hará más grande a medida que apuntemos más módulos al módulo raíz.

Los módulos forman la columna vertebral arquitectónica de una aplicación NestJS. Con una comprensión clara de su creación y gestión, estás bien encaminado para dominar la ideología de NestJS. A medida que construimos nuestra aplicación, ten siempre en cuenta la naturaleza modular de NestJS. Garantiza claridad, mantenibilidad y escalabilidad.

¿Listo para el siguiente nivel? Profundicemos en los controladores y su fascinante mundo.

---

### Construcción de controladores para manejar peticiones

Cada aplicación web tiene una puerta de entrada, un lugar donde los invitados (o, en nuestro caso, las peticiones de los clientes) son recibidos y dirigidos a la ubicación correcta. En la gran mansión de nuestra aplicación NestJS, los controladores son los recepcionistas que aseguran que cada petición se maneje de manera eficiente y se dirija al servicio correcto. Si estás ansioso por diseñar estos guardianes cruciales, estás en el lugar correcto. ¡Abróchate el cinturón, porque estamos a punto de sumergirnos profundamente en el reino de los controladores!

#### ¿Qué es un controlador en NestJS?

Los controladores en NestJS son responsables de recibir las peticiones entrantes, manejarlas y devolver respuestas al cliente. Por lo general, están adornados con el decorador `@Controller()` y contienen métodos manejadores para diferentes rutas.

#### Creando tu primer controlador

Crear un controlador es pan comido, ¡especialmente si ya dominas los módulos! Puedes generarlo con la CLI o manualmente, como hicimos con el módulo. He aquí cómo:

#### Uso de la CLI de NestJS

Al igual que con los módulos, la CLI está aquí para simplificar nuestras vidas:

```bash
$ nest generate controller <controller-name>
```

Para los amantes de los atajos:

```bash
$ nest g co <controller-name>
```

Este comando crea un nuevo archivo de controlador y lo registra en el módulo asociado. Además, genera un archivo de prueba junto al archivo del controlador por defecto. Opcionalmente, si no deseas crear el archivo de prueba, puedes pasar el indicador `--no-spec` al comando.

#### Construcción manual de un controlador

Si eres un desarrollador práctico:

1. Crea un nuevo archivo llamado `<controller-name>.controller.ts`.
2. Estructúralo con el formato básico:

```typescript
import { Controller, Get } from '@nestjs/common'; @Controller('<route-name>') export class <ControllerName>Controller { @Get() findAll(): string { return 'This is a sample response!'; } }
```

Este es un controlador básico que muestra cómo debería verse un controlador mínimo de NestJS.

3. Luego, importa y registra el controlador en el módulo respectivo.

#### Manejo de peticiones con decoradores

NestJS proporciona una gran variedad de decoradores para hacer que el manejo de diferentes tipos de peticiones sea muy sencillo:

- **`@Get()`:** Es para manejar peticiones GET.
- **`@Post()`:** ¿Ansioso por recibir peticiones POST? Esta es tu opción ideal.
- **`@Put()`:** Es ideal para actualizar recursos.
- **`@Delete()`:** Cuando algo deba eliminarse, usa esto.

No solo puedes especificar el tipo de petición, sino también la ruta, pasando una cadena como parámetro como en el siguiente fragmento de código:

```typescript
@Get('details') fetchDetails(): string { return 'Fetching the details for you!'; }
```

> **Nota importante**  
> Cada método manejador en un controlador corresponde a una ruta específica y su propósito debe ser singular y claro. Cumplir con esto garantiza la escalabilidad y la mantenibilidad.

> **Momento de cuestionario**  
> Para las siguientes preguntas, escribe las respuestas correspondientes:  
> 1. ¿Cuál es la función principal de los controladores en una aplicación NestJS?  
> 2. ¿Qué decorador se utiliza para manejar peticiones DELETE?  
> 3. ¿Cómo se especifica una ruta particular para un método manejador?

#### Rutas dinámicas y parámetros

A veces querrás diseñar rutas que sean dinámicas, como al obtener los detalles de un usuario en función de su ID. NestJS te respalda; he aquí cómo:

```typescript
@Get('users/:userId') fetchUserDetails(@Param('userId') userId: string): string { return `Details for user with ID: ${userId}`; }
```

El decorador `@Param()` captura la parte dinámica de la ruta, haciéndola disponible dentro de tu método manejador.

A medida que moldeas tu aplicación NestJS, recuerda que los controladores son los soldados de primera línea, que reciben cada petición y se aseguran de que sean atendidas. Con un conjunto sólido de controladores, estás sentando las bases para una aplicación resistente, escalable y eficiente.

Ahora que nuestras puertas están bien custodiadas, es hora de explorar los majestuosos salones interiores: ¡los servicios y proveedores que hacen el trabajo pesado!

---

### Implementación de proveedores para la lógica de negocio

Imagina una cocina bulliciosa en una gran mansión. Mientras los controladores son los recepcionistas que saludan a los invitados, los proveedores son los chefs capacitados detrás de escena, preparando meticulosamente cada plato con delicadeza y experiencia. En el contexto de nuestra mansión NestJS, los proveedores encapsulan la lógica de negocio central, asegurando que nuestra aplicación no solo se vea bien por fuera, sino que también funcione a la perfección por dentro. ¿Listo para ponerte el gorro de chef y preparar una lógica de negocio estelar? ¡Vamos a sumergirnos!

#### ¿Qué es un proveedor en NestJS?

En esencia, un proveedor en NestJS es una clase que actúa como fuente de algo: ya sea cómo obtener datos, ejecutar tareas específicas o realizar cualquier otra acción que sea integral para tu aplicación. Decorados con el decorador `@Injectable()`, los proveedores son piedras angulares del potente sistema de inyección de dependencias de NestJS.

#### Creando tu primer proveedor

El nacimiento de un proveedor en NestJS es un asunto elegante y directo. Al igual que con el controlador y el módulo, NestJS te ofrece dos formas de crear tus increíbles proveedores.

#### Uso de la CLI de NestJS

Para aquellos que aman la automatización:

```bash
$ nest generate service <service-name>
```

La forma abreviada es la siguiente:

```bash
$ nest g s <service-name>
```

Este comando genera un nuevo archivo de servicio listo para albergar tu lógica de negocio.

#### Construcción manual de un proveedor

Para los artesanos que prefieren el trabajo manual:

1. Crea un nuevo archivo llamado `<service-name>.service.ts`.
2. Dibuja la estructura fundacional:

```typescript
import { Injectable } from '@nestjs/common'; @Injectable() export class <ServiceName>Service { performTask(): string { return 'Executing the core task!'; } }
```

3. Asegúrate de importar y registrar el servicio dentro de su módulo asociado.

Hemos creado nuestro módulo personalizado, controlador y proveedores. Básicamente, estas tres piezas serán las más indispensables para tu aplicación NestJS.

Ahora, profundicemos un poco más en el patrón de diseño que hace que NestJS sea tan robusto: la inyección de dependencias.

---

### Inyección de dependencias: la magia detrás de los proveedores

Uno de los puntos más fuertes de NestJS es su robusto sistema de inyección de dependencias (*Dependency Injection* - DI). Con DI, puedes inyectar fácilmente un proveedor en otro, fomentando un código modular y mantenible. ¡Es como tener un sous-chef listo para ayudar cuando sea necesario!

Por ejemplo, si `UserService` necesita las capacidades de `DatabaseService`, he aquí cómo puedes hacer que suceda:

```typescript
import { Injectable } from '@nestjs/common'; import { DatabaseService } from './database.service'; @Injectable() export class UserService { constructor( private readonly databaseService: DatabaseService ) {} fetchUserData(): string { return this.databaseService.getData('user'); } }
```

En el ejemplo de código anterior, hemos inyectado las capacidades de `databaseService` en la clase `UserService`. Esta dependencia debe inyectarse en la clase constructora, asegurando que la instancia de la base de datos esté disponible desde la inicialización de `UserService`, en cualquier lugar donde tengamos una instancia de `UserService`.

> **Nota importante**  
> Recuerda siempre el principio de responsabilidad única. Cada proveedor debe gestionar una funcionalidad singular y diferenciada. Esto asegura claridad y facilidad de mantenimiento.

> **Momento de cuestionario**  
> Para las siguientes preguntas, escribe las respuestas correspondientes:  
> 1. ¿Cómo se marca una clase como proveedor en NestJS?  
> 2. ¿Cuál es el papel principal del decorador `@Injectable()`?  
> 3. ¿Cómo se inyecta un proveedor en otro?

#### Proveedores personalizados: más allá de las clases

Aunque las clases son los proveedores más comunes, NestJS no se limita a ellas. Con proveedores personalizados, también puedes emplear valores, fábricas y más como proveedores. Proporcionan mayor flexibilidad en la gestión de las dependencias de tu aplicación.

Con proveedores expertos en su lugar, la cocina de nuestra mansión NestJS está llena de actividad, produciendo platos deliciosos (o, en nuestro caso, ejecutando una lógica de negocio impecable). Sin embargo, una mansión no está completa con solo una cocina. A medida que avancemos, nos aventuraremos en más aposentos, cada uno vital para crear una aplicación NestJS integral y eficiente. ¿Próxima parada? ¡Manejo de excepciones!

---

### Manejo de excepciones con filtros de excepciones

Imagina que eres un malabarista experto que gestiona con gracia múltiples tareas. Sin embargo, ocasionalmente, una pelota puede caer. Es en esos momentos cuando requerimos una red de seguridad. Dentro de nuestro reino de NestJS, los filtros de excepciones sirven como esa misma red de seguridad, atrapando con gracia cualquier problema imprevisto. En el [Capítulo 4](https://subscription.packtpub.com/book/web-development/9781835468609/4), discutiremos conceptos avanzados de NestJS, incluidas las excepciones, con ejemplos reales.

#### ¿Qué son los filtros de excepciones?

Los filtros de excepciones te brindan control sobre las excepciones lanzadas por tu aplicación. Al extender la clase base `ExceptionFilter`, puedes definir respuestas personalizadas para cuando las cosas salgan mal.

#### Creación de un filtro de excepciones básico

Veamos brevemente cómo funciona esto con el siguiente ejemplo:

```typescript
import { Catch, ArgumentsHost, HttpException, ExceptionFilter } from '@nestjs/common'; import { Response } from 'express'; @Catch(HttpException) export class HttpExceptionFilter implements ExceptionFilter { catch(exception: HttpException, host: ArgumentsHost) { const ctx = host.switchToHttp(); const response = ctx.getResponse<Response>(); response.status(exception.getStatus()).json({ message: exception.message, timestamp: new Date().toISOString(), }); } }
```

Desglosemos este código y veamos cómo funciona:

- **Importaciones:** Estamos importando `Catch`, `ArgumentsHost` y `HttpException` de `@nestjs/common`. El objeto `Response` se toma de `express`, que NestJS utiliza bajo el capó.
- **`@Catch(HttpException)`:** El decorador `@Catch()` le indica a NestJS qué excepciones debe capturar nuestro filtro. En este caso, está configurado para capturar todas las excepciones de tipo `HttpException`.
- **El método `catch`:** Dentro de nuestro filtro, el método `catch` es responsable de manejar la excepción.
  - El parámetro `exception` nos da acceso al objeto de excepción lanzado.
  - El parámetro `host` nos permite cambiar a diferentes contextos. Aquí, lo usamos para acceder al objeto `Response` subyacente de Express.
- **Construcción de la respuesta:** Estamos enviando una respuesta JSON con el estado de la excepción (`response.status(exception.getStatus()).json(...)`) y datos adicionales como el mensaje de la excepción y una marca de tiempo (*timestamp*).

Este filtro sirve como base. Con él, cualquier tipo de instancia de `HttpException` lanzada se transformará en un formato de respuesta más legible y estandarizado.

A continuación, hablemos de la validación. ¿Recuerdas la regla de "nunca confíes en la entrada del usuario"?

---

### Validación de datos con pipes y guards

Los pipes y guards son como los porteros de vista aguda en un club prestigioso, asegurando que cada invitado (todos los datos, en nuestro caso) cumpla con los criterios requeridos antes de ingresar.

#### Pipes: transformando y validando

Los pipes operan en dos capacidades principales: transformación de datos y validación de datos. Al encadenar múltiples pipes, puedes garantizar la integridad y uniformidad de los datos en toda tu aplicación.

Veamos lo siguiente para una descripción general:

```typescript
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common'; @Injectable() export class SamplePipe implements PipeTransform { transform(value: any, metadata: ArgumentMetadata) { // Validation or transformation logic here return transformedValue; } }
```

Podemos desglosar este código de la siguiente manera:

- **Importaciones:** La instancia de `PipeTransform` es un contrato al que se adherirán nuestros pipes personalizados. Los tipos de instancia `Injectable` y `ArgumentMetadata` se importan de `@nestjs/common`. Estos son conceptos centrales en NestJS que ayudan en la creación de pipes y en la comprensión del contexto en el que se utiliza el pipe, respectivamente.
- **`@Injectable()`:** El decorador `@Injectable()` marca nuestra clase `SamplePipe` como un proveedor que puede ser administrado por el sistema DI de NestJS.
- **Implementación de `PipeTransform`:** Nuestra clase `SamplePipe` implementa la interfaz `PipeTransform`, lo que exige la presencia de un método `transform` en nuestra clase.
- **El método `transform`:** Aquí es donde ocurre la magia. El método `transform` toma dos argumentos:
  - `value`: El valor original que se está procesando.
  - `metadata`: Proporciona metadatos adicionales sobre el argumento, como el tipo (`'body'`, `'query'`, etc.).

Dentro de este método, puedes introducir tu lógica de validación o transformación. El tipo de instancia `transformedValue` devuelto reemplazará el tipo de instancia `value` original en los datos que se procesan.

Esta estructura simple pero poderosa facilita una gran cantidad de casos de uso. Desde convertir cadenas numéricas en números reales de JavaScript hasta garantizar que una carga útil recibida se adhiera a un Objeto de Transferencia de Datos (*DTO*) específico, las posibilidades con los pipes son verdaderamente amplias.

#### Guards: protegiendo rutas

Los guards son los centinelas de NestJS que determinan si una petición debe continuar hacia su manejador de ruta. En NestJS, un guard siempre implementará el método `canActivate`. Un guard básico podría verse así:

```typescript
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common'; @Injectable() export class SampleGuard implements CanActivate { canActivate(context: ExecutionContext): boolean { // Logic to determine whether the request should // proceed return true; }}
```

Podemos desglosar este código de la siguiente manera:

- **Importaciones:** El tipo de instancia `CanActivate` es una interfaz que, cuando se implementa, actúa como un guard decidiendo si permitir que la petición continúe. Las instancias `ExecutionContext` e `Injectable` se importan de `@nestjs/common`. Estas ayudan a comprender el contexto en el que se usa el guard y a convertir nuestro guard en un proveedor, respectivamente.
- **`@Injectable()`:** El decorador `@Injectable()` marca la clase `SampleGuard` como un proveedor que puede ser administrado por el sistema DI de NestJS.
- **Implementación de `CanActivate`:** Nuestra clase `SampleGuard` implementa la interfaz `CanActivate`. Esto requiere la definición de un método `canActivate` en nuestra clase.
- **El método `canActivate`:** Este método es el corazón de nuestro guard. Toma un objeto `ExecutionContext`, que encapsula los detalles de la petición en curso.
- El método debe devolver un booleano. Si devuelve `true`, la petición continúa hacia el manejador de ruta. Si es `false`, la petición es denegada.
- El comentario `// Logic to determine whether the request should proceed` es un marcador de posición donde implementarías la lógica para decidir si la petición debe continuar. Esto podría implicar la verificación de tokens de autenticación, roles de usuario y más.

Al usar guards, puedes crear mecanismos de control de acceso complejos, asegurando que cada petición que acceda a tus rutas sea legítima y esté autorizada.

> **Nota**  
> Los pipes y guards son temas amplios. Los exploraremos en un capítulo posterior para comprenderlos en profundidad.

Ahora que tenemos casi todo lo necesario para construir una aplicación NestJS robusta, estamos listos para construir aplicaciones reales. Esto es lo que vamos a hacer juntos en los próximos dos capítulos.

---

### Resumen

Este capítulo proporcionó una guía exhaustiva sobre el uso de NestJS para crear aplicaciones del lado del servidor escalables y mantenibles. Comenzó guiándote a través de la instalación de Node.js y NestJS, junto con las herramientas CLI necesarias. El capítulo garantizó un proceso de desarrollo fluido mostrándote cómo configurar herramientas esenciales como npm y TypeScript.

Luego presentamos la estructura de un proyecto NestJS, que sienta las bases para comprender sus componentes. El capítulo progresó con conocimientos prácticos sobre la creación y gestión de módulos, controladores y proveedores, elementos esenciales de las aplicaciones NestJS. También cubrió cómo manejar excepciones con filtros y mantener la integridad de los datos mediante la validación utilizando pipes y guards.

Al final del capítulo, hemos adquirido habilidades esenciales para configurar un entorno NestJS, organizar una aplicación de manera efectiva y desarrollar aplicaciones que puedan abordar los desafíos del mundo real. Estas habilidades son cruciales para elevar la competencia de uno en el desarrollo de aplicaciones del lado del servidor y sentar las bases para las mejores prácticas que se cubrirán en los capítulos posteriores.

En el próximo capítulo, llevemos las habilidades que adquirimos en este capítulo a otro nivel, explorando las características avanzadas de NestJS. Todo lo que aprendimos aquí nos servirá a medida que dominemos las piezas clave del ecosistema de NestJS.
