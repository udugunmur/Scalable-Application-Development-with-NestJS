# Parte 2: APIs REST y GraphQL en NestJS

## Capítulo 5: Construcción y Optimización de APIs REST con NestJS

¡Bienvenido a un emocionante capítulo donde transformaremos el conocimiento en práctica! Después de profundizar en las complejidades de NestJS en los capítulos anteriores, ahora estamos listos para aplicar nuestras habilidades y construir algo tangible e impactante. Este capítulo trata sobre la creación y el ajuste de APIs REST utilizando NestJS, mientras aprovechamos las potentes funciones y las mejores prácticas que hemos descubierto hasta ahora.

¿Estás preparado para programar en el mundo real? Esto es lo que emprenderemos juntos en este capítulo:

- Introducción a las APIs REST en NestJS
- Construcción de una API REST básica en NestJS
- Versionado de tu API
- Manejo de errores
- Documentación de tu API

Al final de este capítulo, no solo tendrás una comprensión sólida del desarrollo de APIs REST con NestJS, sino también experiencia práctica en la construcción de una. Entonces, sin más preámbulos, sumerjámonos y comencemos a construir.

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Introducción a las APIs REST en NestJS

¿Recuerdas cómo dimos inicio a las cosas en el [Capítulo 3](https://subscription.packtpub.com/book/web-development/9781835468609/3) creando una aplicación NestJS simple? Bueno, ahora es el momento de descubrir otra capa. Pudimos hacer una petición GET a la interfaz de programación de aplicaciones (API) que creamos en ese entonces porque, por defecto, NestJS admite el estilo arquitectónico de Transferencia de Estado Representacional (*REpresentational State Transfer* - REST).

Entonces, ¿qué es esto de REST, de todos modos? ¿Y en qué se diferencia de ser RESTful? Esta pregunta puede haber surgido al leer este párrafo, ¡y es genial porque despierta tu curiosidad! Piensa en REST como algo más que un conjunto de herramientas; es una filosofía para diseñar aplicaciones que se comunican a través de una red. Se trata de simplicidad, uso de operaciones sin estado (*stateless*) y provisión de una forma uniforme de direccionar recursos, a menudo a través de nuestro viejo amigo, HTTP. Por otro lado, cuando hablamos de que algo es RESTful, nos referimos a una API que es un ejemplo viviente de estos principios REST y utiliza métodos HTTP familiares como GET, POST, PUT y DELETE de una manera que es a la vez escalable y fácil de usar.

Consideremos un escenario del mundo real. Toma PayPal, por ejemplo. Su columna vertebral está construida sobre los principios REST, asegurando que todo funcione a la perfección. Mientras tanto, ofrece APIs RESTful para que el mundo se conecte con sus servicios, adhiriéndose estrechamente a esos mismos principios.

Ahora, volvamos a NestJS. Este framework no solo admite REST; está prácticamente casado con él. Esto significa que cuando creas una aplicación básica de NestJS, ya estás jugando en el patio de recreo de REST.

¿Y adivina qué? No solo vamos a hablar sobre estas ideas; nos vamos a arremangar y hacer que cobren vida. A continuación, aprenderemos cómo construir nuestra API REST básica utilizando NestJS. Allí, te pondrás en la piel de un constructor, reuniendo tu propia API REST con NestJS. Es hora de convertir esos conceptos en algo que puedas ver, tocar e interactuar. ¡Entremos de lleno y comencemos a armar este rompecabezas!

---

### Construcción de una API REST básica

A medida que nos embarcamos en esta emocionante fase de construcción de una API REST, refresquemos nuestra memoria de los conceptos centrales que hemos aprendido hasta ahora. ¿Recuerdas la configuración del entorno del [Capítulo 3](https://subscription.packtpub.com/book/web-development/9781835468609/3) y la inmersión profunda en módulos, controladores, proveedores, filtros de excepciones, pipes, guards y decoradores en el [Capítulo 4](https://subscription.packtpub.com/book/web-development/9781835468609/4)? Estamos a punto de poner todos estos elementos en juego.

#### Configuración de tu proyecto de API REST

Ya sea que continúes con la aplicación NestJS del [Capítulo 2](https://subscription.packtpub.com/book/web-development/9781835468609/2) o comiences de nuevo, los principios siguen siendo los mismos. Comencemos configurando nuestro entorno de proyecto:

1. Para un nuevo proyecto, ejecuta `nest new project-name` en tu terminal.
2. Para un proyecto existente, navega al directorio de tu proyecto existente.
3. **Comprensión de la estructura:** Observa las carpetas predeterminadas (es decir, controladores, módulos, etc.), tal como lo hicimos en el [Capítulo 2](https://subscription.packtpub.com/book/web-development/9781835468609/2), como una forma de refrescar tu memoria.
4. **Ejecución de la aplicación:** Inicia la aplicación usando `npm run start:dev`. Esto levantará el servidor local.

Con el primer paso completado, profundicemos más.

#### Diseñando tu primer endpoint RESTful

Embarquémonos en la creación de nuestro primer endpoint RESTful. Para mantener nuestra demostración simple de seguir y directa, vamos a construir una API básica de gestión de usuarios que presenta los conceptos básicos de las operaciones CRUD: crear un usuario (POST), recuperar usuarios (GET), actualizar (PUT) y eliminar un usuario (DELETE). Pero primero, nuestra tarea inicial es crear un endpoint GET simple que devolverá un arreglo simulado (*mocked*) de usuarios.

Este es el trato: vamos a configurar todo manualmente esta vez. ¿Por qué? Porque es esencial comprender cómo construir módulos desde cero y conectarlos al módulo raíz. Claro, la CLI de NestJS es súper práctica, pero nada supera el aprendizaje que obtienes al hacer las cosas a mano. ¡Así que empecemos!

#### Creación de `UsersModule`

Dentro de tu directorio `src/`, crea un nuevo subdirectorio llamado `users`.

Ahora, creemos nuestro primer archivo en este nuevo directorio: `users.module.ts`. Aquí es donde definiremos nuestro módulo de Usuarios, la piedra angular de nuestro sistema de gestión de usuarios:

```typescript
import { Module } from '@nestjs/common'; @Module({}) export class UsersModule {}
```

Comenzamos con una clase simple decorada con `@Module`. Esta es la declaración de nuestro módulo.

#### Conexión al módulo raíz

Ahora, integremos este nuevo módulo en el corazón de nuestra aplicación. A partir de ahora, `UsersModule` es un nodo aislado del grafo de la aplicación, por lo que debemos importarlo en `AppModule` para que podamos interactuar con sus componentes, como su controlador y las operaciones HTTP definidas.

En tu archivo `src/app.module.ts`, agrega `UsersModule` al arreglo `imports`:

```typescript
// ... existing imports import { UsersModule } from './users/users.module'; // ... existing code imports: [UsersModule], // Add this line // ... the rest of the code
```

Con esto, nuestro módulo `Users` ahora forma parte del ecosistema de nuestra aplicación.

#### Configuración de la entidad de usuario

En el directorio `src/users/` de nuestro proyecto NestJS, crearemos un nuevo subdirectorio llamado `entities/`. En la arquitectura de NestJS, este directorio es donde colocamos nuestros tipos de entidad relacionados con los recursos. Es especialmente importante cuando tu aplicación interactúa con una base de datos. Este directorio sirve como la ubicación óptima para almacenar cosas como el esquema de recursos o tablas para bases de datos relacionales.

Ahora, dentro de esta carpeta `entities/` recién creada, creemos un archivo llamado `users.entity.ts`. Aquí está el código que debes incluir en ese archivo:

```typescript
// src/users/entities/users.entity.ts export class User { id: number; name: string; email: string; }
```

Esta clase definirá la estructura de una entidad `User` en nuestra aplicación, manteniéndola sencilla por ahora. Ten en cuenta que, según el mapeo objeto-relacional (*Object-Relational Mapping* - ORM) que estés utilizando para tu base de datos, esta clase se puede anotar con los decoradores relacionados del ORM, como `@Schema()` y sus campos, así como decoradores como `@Prop()` y otros, según tu ORM. Para nuestro caso de uso, como no estamos usando ningún ORM, esta clase se adapta a nuestras necesidades.

#### Creación de `UsersService`

A continuación, necesitamos un servicio para manejar nuestra lógica de negocio. En `src/users/`, crea un archivo `users.service.ts`.

Este servicio gestionará nuestros datos de usuario. En casos de uso reales, este será el lugar donde podrás colocar las interacciones con tu base de datos. Por ahora, usaremos un arreglo simple para imitar una base de datos:

```typescript
import { Injectable } from '@nestjs/common'; import { User } from './entities/users.entity'; @Injectable() export class UsersService { private readonly users: User[] = [ // Here are our mock users { id: 1, name: 'Tshimanga Mukendi', email: 'tshim@myapp.com' }, { id: 2, name: 'Kasereka Akim', email: 'kase@myapp.com' }, ]; findAll(): User[] { return this.users; } }
```

Veamos qué hemos hecho en este archivo de servicio:

- Hemos creado una variable de clase del tipo `User[]` con un arreglo simulado de usuarios. En ejemplos del mundo real que consisten en interacciones de bases de datos, deberíamos usar sembradores (*seeders*) para poblar el arreglo o solo confiar en los datos que ya existen en la base de datos para realizar operaciones, en lugar de codificar usuarios de forma fija. Pero para fines de demostración, esto es perfecto (por ahora).
- Luego, implementamos el método `findAll()`, que solo devuelve la lista de usuarios que ya tenemos en la clase.
- Observa el decorador `@Injectable()`, que marca esta clase como un proveedor.

#### Implementación de `UsersController`

Es hora de manejar las peticiones del Protocolo de Transferencia de Hipertexto (*Hypertext Transfer Protocol* - HTTP) entrantes. Para hacerlo, crea un archivo `users.controller.ts` en el directorio `src/users/`.

Este controlador dirigirá nuestras peticiones GET:

```typescript
import { Controller, Get } from '@nestjs/common'; import { UsersService } from './users.service'; @Controller('users') export class UsersController { constructor( private readonly usersService: UsersService ) {} @Get() findAll() { return this.usersService.findAll(); } }
```

Veamos qué hemos hecho en este archivo de controlador:

- Especificamos que es un controlador de usuarios, lo que significa que todas las peticiones bajo el endpoint `users/` se manejarán aquí.
- Implementamos una operación `@Get()`, sin parámetro adicional, que solo usa el método `findAll()` que implementamos anteriormente en el archivo de servicio.
- Como nota adicional, tenemos el constructor de la clase, que inyecta `usersService` como una dependencia.

#### Aplicando los toques finales en `UsersModule`

Asegurémonos de que nuestro servicio y controlador sean reconocidos en el módulo.

En `users.module.ts`, actualiza la definición del módulo:

```typescript
@Module({ providers: [UsersService], controllers: [UsersController], })
```

Esto conecta nuestro servicio y controlador al módulo.

Agregar los proveedores y controladores ayuda a conectar el archivo de servicio al propio módulo para que pueda inyectarse en la clase de controlador (a través del arreglo `providers`) y registrar la operación del controlador en el módulo para que podamos comenzar a escuchar sus peticiones HTTP implementadas.

> **Pista**  
> Algo que te puede ocurrir en ciertos escenarios al crear módulos es que termines omitiendo los proveedores o el controlador en la definición del módulo. La primera omisión te dará el siguiente error:  
> `ERROR [ExceptionHandler] Nest can't resolve dependencies of the UsersController (?). Please make sure that the argument UsersService at index [0] is available in the UsersModule context`  
> Para la segunda omisión, NestJS te informará de que no reconoce la ruta de los usuarios con la siguiente respuesta:  
> `{"message":"Cannot GET /users","error":"Not Found","statusCode":404}`  
> En la definición del módulo, asegúrate de que tanto el controlador como los proveedores estén especificados para evitar encontrarte con uno de estos errores.

#### Probando nuestro endpoint

Inicia tu aplicación NestJS con `yarn start:dev`.

Para probarla, ejecuta el siguiente comando curl o usa Postman:

```bash
$ curl -H 'content-type: application/json' <http://localhost:3000/users>
```

Ahora deberías ver a tus usuarios simulados como respuesta. ¡Gran trabajo!

#### Una nota sobre la CLI de NestJS

Si bien hemos hecho todo manualmente aquí, recuerda que la CLI de NestJS puede acelerar este proceso en el futuro. Es perfecta para tareas de desarrollo rápido, y puedes aprender más sobre ella en la documentación de NestJS ([https://docs.nestjs.com/cli/usages](https://docs.nestjs.com/cli/usages)). Ya hemos utilizado algunos de estos comandos en capítulos anteriores, pero no dudes en experimentar con ellos para aumentar tu productividad.

¡Felicitaciones por configurar con éxito tu primer endpoint RESTful en nuestra API de gestión de usuarios! Hemos sentado las bases con partes esenciales de las operaciones CRUD, conectado nuestros módulos y establecido una base sólida con entidades y servicios básicos. Este enfoque práctico te ha dado una muestra de la simplicidad y el poder de NestJS para crear APIs funcionales y escalables.

Pero el viaje no termina aquí: solo hemos arañado la superficie de lo que NestJS puede hacer. A medida que hacemos la transición desde estos fundamentos, es hora de profundizar y explorar las características más complejas y potentes de NestJS. Estos conceptos avanzados no solo mejorarán la funcionalidad y robustez de tu API, sino que también introducirán las mejores prácticas en el desarrollo moderno de APIs.

---

### Integración de conceptos avanzados en tu API

Estamos a punto de elevar nuestra API de gestión de usuarios a nuevas alturas. NestJS ofrece un conjunto de herramientas y funciones avanzadas que pueden optimizar el rendimiento, reforzar la seguridad y hacer que nuestro código sea más eficiente y fácil de mantener. Algunos de estos conceptos se enseñaron teóricamente en el [Capítulo 4](https://subscription.packtpub.com/book/web-development/9781835468609/4). Si sientes que necesitas un repaso, no dudes en volver a leerlo antes de sumergirnos en los aspectos prácticos.

Exploraremos los siguientes temas en esta sección:

- **Pipes** para validación de entrada y transformación de datos, asegurando la integridad y corrección de los datos que maneja nuestra API.
- **Guards** para proteger nuestras rutas y administrar quién tiene acceso a qué, implementando aspectos esenciales de la seguridad de APIs como autenticación y autorización.
- **Middleware** para manejar peticiones e implementar aspectos transversales (*cross-cutting concerns*) como registro y validación de peticiones.
- **Interceptores** para transformar y registrar respuestas, brindándonos una forma de administrar datos salientes e interacciones de manera eficiente.
- **Proveedores personalizados** para inyección de dependencias avanzada, lo que permite un código más dinámico y flexible.

Estas son solo algunas de las características que cubriremos aquí. Cada una nos acercará a una API robusta de nivel empresarial. Embarquémonos en este viaje y aprovechemos todo el potencial de NestJS, creando una API que no solo sea funcional sino también segura, eficiente y escalable.

#### Pipes para validación y transformación

En el ámbito del desarrollo de APIs, la validación de datos es fundamental. Garantiza que los datos entrantes se adhieran al formato esperado y cumplan con los criterios establecidos por la lógica de nuestra aplicación. En NestJS, los pipes son las herramientas preferidas para esta tarea. No solo validan sino que también pueden transformar los datos de entrada, lo que los hace increíblemente versátiles. Para nuestra API de gestión de usuarios, emplearemos pipes para validar los datos de usuario al crear y actualizar un usuario. Ahora, programemos de verdad.

Antes de comenzar con los pipes, construyamos nuestro Objeto de Transferencia de Datos (*Data Transfer Object* - DTO). Un DTO es un objeto que define cómo se enviarán los datos a través de la red. Podemos usar interfaces para describirlo, pero NestJS recomienda usar clases ya que son parte de los estándares de JavaScript ES6, lo que significa que no necesitan compilarse en tiempo de ejecución. Definamos el que necesitamos para fines de demostración:

#### Definición de DTOs de usuario

Primero, debemos definir la estructura de los datos que esperamos al crear o actualizar un usuario. Aquí es donde entran los DTOs.

En el directorio `src/users/dtos` (nuevo), crea dos archivos llamados `create-user.dto.ts` y `update-user.dto.ts`:

```typescript
// src/users/dtos/create-user.dto.ts export class CreateUserDto { name: string; email: string; } // src/users/dtos/update-user.dto.ts // export class UpdateUserDto { // name?: string; // email?: string; // id: number; // required to identify the user we // are updating // } // a short form and the most used one // you will need to install the package import { PartialType } from '@nestjs/mapped-types'; import { CreateUserDto } from './create-user.dto'; export class UpdateUserDto extends PartialType(CreateUserDto) {}
```

Aquí, `CreateUserDto` define los campos requeridos para crear un usuario, mientras que `UpdateUserDto` usa campos opcionales ya que no todos los detalles del usuario pueden cambiar durante una actualización. Estamos usando `PartialType()` para generar la clase `UpdateUserDto`, una función de utilidad proporcionada por NestJS que devuelve la clase original pasada como parámetro, con todas las propiedades del tipo de entrada establecidas en opcionales. El resultado será el mismo que la línea comentada en el fragmento de código anterior.

#### Implementación de pipes de validación

Ahora que los DTOs están en su lugar y sabemos exactamente cómo deberían verse nuestros datos, implementemos nuestros pipes.

#### Aplicación de pipes de validación

Si bien los pipes nativos de NestJS se utilizan para validar y transformar entradas, primero utilizaremos `class-validator` y `class-transformer`, ambos paquetes ampliamente utilizados y validados por la comunidad de NestJS que hacen cumplir las reglas de validación en nuestros DTOs.

Instala estos paquetes con:
```bash
yarn add class-validator class-transformer
```

#### Mejora de los DTOs con validadores

Agrega reglas de validación a los DTOs utilizando decoradores del paquete `class-validator`:

```typescript
// src/users/dtos/create-user.dto.ts import { IsEmail, IsNotEmpty } from 'class-validator'; export class CreateUserDto { @IsNotEmpty() name: string; @IsEmail() email: string; }
```

Aquí, estamos utilizando algunos de los decoradores integrados del paquete `class-validator` para especificar cómo necesitamos que provengan estos datos del cliente. Al usar el decorador `@IsNotEmpty` en la propiedad `name`, estamos estableciendo una regla que ahora nos impide intentar ingresar una cadena vacía como nombre para un usuario; el decorador `@IsEmail` también nos evita crear expresiones regulares (*regex*) personalizadas para manejar el campo de correo electrónico. Con algo tan simple como esto, nos aseguramos de que ningún dato de entrada no válido llegue a nuestra base de datos, manteniendo nuestros datos consistentes.

#### Implementación de la lógica de negocio para crear y actualizar un usuario

Ahora, centrémonos en desarrollar la lógica para crear y actualizar usuarios en nuestro archivo `users.service.ts`. Si aún no lo has hecho, aquí están los métodos que debemos agregar:

```typescript
// src/users/users.service.ts // ... rest of the code createUser(user: CreateUserDto) { this.users.push({ ...user, id: this.users.length + 1, }); } updateUser(user: UpdateUserDto & { id: number }) { const index = this.users.findIndex( (u) => u.id === user.id); if (index === -1) { throw new Error('User not found'); } this.users[index] = { name: user.name ?? this.users[index].name, email: user.email ?? this.users[index].email, id: this.users[index].id, }; } // .. remaining code
```

En el código anterior, agregamos dos métodos (`createUser` y `updateUser`) a `users.service.ts`. Hay algunos puntos clave a tener en cuenta aquí:

- Actualmente, estamos modificando directamente el arreglo `users` dentro de la clase de servicio. Recuerda que en aplicaciones del mundo real, esta operación normalmente sería asíncrona e interactuaría con una base de datos a través de ORMs. El archivo de servicio es donde deben colocarse tales interacciones con la base de datos.
- El método `updateUser` espera un parámetro `id` adicional, que idealmente debería recibirse del parámetro de ruta en la petición, no del cuerpo de la petición. En el diseño de APIs RESTful, el identificador de recursos (en este caso, el ID del usuario) se coloca mejor en la ruta de la URL en lugar del cuerpo de la petición. Ten en cuenta que `UpdateUserDto` no incluye un campo de ID, ya que está diseñado para la carga útil (*payload*), no para la identificación de recursos. Si bien puede haber excepciones basadas en casos de uso específicos, la práctica estándar es poner el ID en la ruta. Si el ID es incorrecto, lanzamos un error con un mensaje claro mientras nos adherimos a las mejores prácticas en el manejo de errores de APIs.
- Es crucial manejar los errores de manera efectiva y brindar comentarios claros al cliente. Esto es parte de las mejores prácticas para construir APIs robustas. Lo hicimos aquí, lanzando un mensaje de error claro si el ID de usuario es incorrecto.

Al seguir estas pautas, podemos asegurarnos de que nuestra API de gestión de usuarios no solo sea funcional, sino que también se alinee con los principios y prácticas RESTful establecidos. Ahora, pasemos al controlador.

#### Integración de pipes en controladores

Ahora, integremos estos pipes de validación en nuestros controladores. Para hacerlo, modificaremos las rutas POST y PATCH en `UsersController`:

```typescript
// src/users/users.controller.ts import { Body, Controller, Get, Param, ParseIntPipe, Patch, Post, ValidationPipe, } from '@nestjs/common'; import { UsersService } from './users.service'; import { CreateUserDto } from './dto/create-user.dto'; @Controller('users') export class UsersController { // ... existing code @Post() createUser( @Body(new ValidationPipe()) createUserDto: CreateUserDto ) { return this.usersService.createUser( createUserDto ); } // without the PaseIntPipe, the id will be a // string => needs to be converted to a number // manually -- not recommended // @Patch(':id') // updateUser(@Body() updateUserDto: CreateUserDto, // @Param('id', ParseIntPipe) id: string) { // on this line, we are converting the id to a // number -- using the `+` sign // return this.usersService.updateUser({ // ...updateUserDto, id: +id }); // } // using the ParseIntPipe -- recommended @Patch(':id') updateUser( @Body(new ValidationPipe()) updateUserDto: CreateUserDto, @Param('id', ParseIntPipe) id: string, ) { return this.usersService.updateUser({ ...updateUserDto, id }); } // ... rest of the class methods }
```

Aquí hay algunas conclusiones clave con respecto a este fragmento de código:

- El método `@Post()` utiliza el decorador `@Body()` del objeto payload y especifica en qué formato se espera el payload utilizando `CreateUserDto`, que creamos en nuestro código.
- Luego, el objeto se pasa al archivo de servicio para ser procesado; en este caso, para agregar el nuevo usuario al arreglo de usuarios.
- En ambos métodos, pasamos el parámetro `new ValidationPipe()` al decorador `@Body()`. Al hacerlo, vinculamos la instancia del pipe al decorador `@Body()` del manejador de ruta para que nuestro pipe sea llamado a validar el cuerpo del usuario. Omitir esta vinculación puede hacer que NestJS ignore tus reglas y procese cualquier dato.
- El método `@Patch()` desencadena más observaciones. Primero, esperamos que el ID de usuario sea parte de la ruta. Esto significa que para actualizar un usuario, haremos una petición PATCH al endpoint `users/:id`.
- En segundo lugar, dado que cualquier parámetro de ruta viene como una cadena por defecto, nuestra base de datos espera un número. Por lo tanto, necesitamos transformar la entrada actual en un número. Aquí es cuando entra en juego el pipe integrado `ParseIntPipe`. Al agregar el pipe como segundo parámetro al decorador `@Param()`, el parámetro dado se transforma en el formato deseado, y si la transformación falla, el pipe generará una excepción. Nest tiene un conjunto de pipes integrados para tales operaciones que se pueden encontrar en la documentación oficial: [https://docs.nestjs.com/pipes#built-in-pipes](https://docs.nestjs.com/pipes#built-in-pipes).

Con nuestros pipes en su lugar, juguemos con nuestra API una vez más y veámoslos en acción probándola.

Puedes usar cualquier herramienta de tu elección para probar la API y ver cómo responde NestJS.

> **⚒ Conclusión rápida**  
> Intenta crear y actualizar usuarios con datos aleatorios y observa qué respuestas obtienes (por ejemplo, direcciones de correo electrónico incorrectas, nombres vacíos o cualquier cosa que nuestra API no pueda procesar actualmente). Anota tus hallazgos; te serán útiles antes de cerrar este capítulo.

Ahora que sabes cómo validar y transformar las entradas de los usuarios en el formato deseado mientras aprovechas el uso de pipes, aprendamos cómo proteger nuestros endpoints para que no sean accedidos por fuentes desconocidas o no autorizadas.

Al integrar pipes de validación y DTOs, hemos agregado una capa de integridad y estructura de datos a nuestra API de gestión de usuarios. Este enfoque no solo hace que nuestra API sea más robusta, sino también más fácil de mantener y entender. A medida que avancemos, continuaremos mejorando nuestra API con funciones más avanzadas de NestJS, asegurándonos de que no solo sea funcional sino también resistente y segura. Ahora, hablemos de los guardianes.

#### Guards para asegurar nuestras rutas

En NestJS, los guards juegan un papel crucial a la hora de determinar si una petición dada será manejada por el manejador de ruta o no. Son particularmente esenciales para implementar la lógica de autenticación y autorización. En nuestra API de gestión de usuarios, emplearemos un guard personalizado para simular un proceso de autenticación básico, lo que te brindará una comprensión práctica de cómo funcionan los guards en una aplicación NestJS.

Creemos un guard de autenticación simulado con fines de demostración:

#### Conceptualización del guard de autenticación simulado

En un escenario del mundo real, los guards a menudo verifican las credenciales o tokens de usuario contra una base de datos o un servicio de autenticación. Simularemos este proceso con fines de aprendizaje.

Crearemos un guard que verifique un token específico y codificado de forma fija en los encabezados de la petición para simular la autenticación de usuarios.

#### Construcción de `MockAuthGuard`

Crea un nuevo directorio llamado `src/auth` y dentro de él, un archivo llamado `mock-auth.guard.ts`.

Este guard inspeccionará los encabezados de la petición y permitirá el acceso si está presente un token de autenticación simulado:

```typescript
// src/auth/mock-auth.guard.ts import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common'; @Injectable() export class MockAuthGuard implements CanActivate { canActivate(context: ExecutionContext): boolean { const request = context.switchToHttp().getRequest(); // Simple check for a mock token return request.headers['authorization'] === 'Bearer mock-token'; } }
```

En NestJS, recuerda que un guard debe implementar la interfaz `CanActivate` y estar anotado con el decorador `@Injectable()`. En este ejemplo, estamos verificando la autorización del encabezado con un valor de token simulado específico. También tenemos acceso al contexto de ejecución.

Ahora, implementemos y apliquemos el guard que acabamos de crear para que podamos probarlo.

#### Integración del guard en la aplicación

Apliquemos este guard para que podamos proteger ciertas rutas en nuestra API de gestión de usuarios, específicamente las rutas que modifican datos de usuario. En la mayoría de los casos de uso, esas rutas necesitan atención especial para que las operaciones de escritura (creaciones y actualizaciones) no puedan ser realizadas por cualquiera.

Actualiza `UsersController` para que use `MockAuthGuard` para las rutas POST, PATCH y DELETE (si ya la has implementado):

```typescript
// src/users/users.controller.ts // ... imports import { MockAuthGuard } from '../auth/mock-auth.guard'; @Controller('users') export class UsersController { // ...other methods @Post() @UseGuards(MockAuthGuard) createUser( @Body(new ValidationPipe()) createUserDto: CreateUserDto ) { // Logic to create a user } @Patch(':id') @UseGuards(MockAuthGuard) updateUser( @Body(new ValidationPipe()) updateUserDto: CreateUserDto, @Param('id', ParseIntPipe) id: number, ) { // Logic to update a user } // the delete operation }
```

Aquí, `@UseGuards(MockAuthGuard)` se utiliza para proteger nuestras rutas, asegurando que solo las peticiones con el token simulado correcto puedan acceder a estos endpoints.

Finalmente, prueba que tu implementación haya sido un éxito colocando el token simulado en el encabezado `Authorization` y siguiendo estos pocos pasos:

1. Para probar la eficacia de nuestro guard, podemos usar Postman o una herramienta similar para enviar peticiones a nuestras rutas protegidas.
2. Envía peticiones con el encabezado `Authorization: Bearer mock-token` y observa el comportamiento de la API. La petición debe ser exitosa.
3. Luego, envía peticiones sin el encabezado o con un token incorrecto. La API debe rechazar estas peticiones, demostrando la funcionalidad del guard.

Aquí hay algunas conclusiones importantes:

- En aplicaciones reales, los guards normalmente interactuarían con mecanismos de autenticación más complejos, como la validación de JWTs o tokens OAuth, estrategias y más.
- Puedes ampliar este concepto integrando una base de datos para administrar usuarios y tokens y actualizando el guard para que realice comprobaciones de autenticación reales.

Con eso, hemos integrado con éxito un guard de autenticación simulado en nuestra API de gestión de usuarios. Este ejercicio ha proporcionado una comprensión práctica de cómo funcionan los guards en NestJS y su importancia para asegurar las APIs. A medida que avances en tu viaje con NestJS, encontrarás que los guards son indispensables para construir aplicaciones seguras y listas para producción. Ahora, pasemos a algo también muy importante: el middleware.

#### Middleware para manejo de peticiones y registro

En NestJS, el middleware funciona de manera similar al middleware en otros frameworks, como Express.js. Se ubica entre la petición y la respuesta, actuando como una poderosa herramienta para el procesamiento de peticiones, registro e incluso validación de peticiones. Para nuestra API de gestión de usuarios, un middleware de registro puede ayudarnos a realizar un seguimiento de las peticiones entrantes, lo cual es invaluable para la depuración y el monitoreo.

No esperemos más y creemos un middleware de registro de peticiones para nuestra API:

#### Conceptualización del middleware de registro

El objetivo es crear un middleware que registre los detalles de cada petición (como el método HTTP, la ruta y la marca de tiempo) en la consola o en un archivo de registro.

Este middleware se aplicará globalmente a todas las rutas en nuestra API.

También podemos crear middleware con ámbito, pero por ahora, mantengámoslo simple. Tendremos muchas más oportunidades de explorarlos en los próximos capítulos.

#### Implementación del middleware de registro

Crea un nuevo archivo en el directorio `src/common/middleware` (crea el directorio si no existe) llamado `logging.middleware.ts`.

Definiremos nuestro middleware de registro en este archivo:

```typescript
// src/common/middleware/logging.middleware.ts import { Injectable, NestMiddleware } from '@nestjs/common'; import { Request, Response, NextFunction } from 'express'; @Injectable() export class LoggingMiddleware implements NestMiddleware { use(req: Request, res: Response, next: NextFunction) { const { method, originalUrl } = req; const timestamp = new Date().toISOString(); console.log( `[${timestamp}] ${method} ${originalUrl}` ); next(); } }
```

Aquí, registramos la marca de tiempo, el método HTTP y la URL de cada petición, luego pasamos la ejecución a otro manejador usando la función `next()`. Este puede ser otro middleware en la pila o simplemente el manejador de ruta en el controlador. En nuestro caso, solo tendremos un middleware. Sin embargo, en algunos escenarios, podemos tener más.

Ahora, apliquemos el middleware que acabamos de crear globalmente.

#### Integración del middleware en la aplicación

Para aplicar este middleware en el contexto global, lo configuraremos en `AppModule`.

Actualiza `AppModule` para que aplique `LoggingMiddleware` globalmente:

```typescript
// src/app.module.ts import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common'; import { LoggingMiddleware } from './common/middleware/logging.middleware'; import { UsersModule } from './users/users.module'; @Module({ imports: [UsersModule], // ...other configurations }) export class AppModule implements NestModule { configure(consumer: MiddlewareConsumer) { consumer .apply(LoggingMiddleware) .forRoutes('*'); // Apply for all routes } }
```

Aquí, le decimos a NestJS que use `LoggingMiddleware` para cada ruta en la aplicación. Si queremos que esto solo funcione para los endpoints de `user/`, el primer parámetro del método `forRoutes()` es el lugar para nosotros, o dentro de `UserModule` mientras implementamos la misma interfaz `NestModule`.

Para ver el middleware que acabamos de crear en acción, debemos hacer lo siguiente:

1. Ejecutar la aplicación y hacer varias peticiones a nuestros endpoints de la API.
2. Observar la consola o el archivo de registro (dependiendo de dónde estemos registrando los detalles). Deberíamos ver registros para cada petición que se haya realizado a la API, mostrando el método, la ruta y la marca de tiempo.

El middleware, como el que acabamos de implementar, juega un papel crucial en las aplicaciones del mundo real. Se puede utilizar para tareas más complejas, como autenticar tokens de API, manejar CORS o incluso realizar limitación de velocidad (*rate limiting*). La flexibilidad del middleware en NestJS permite a los desarrolladores implementar una amplia gama de funcionalidades que pueden interceptar y modificar peticiones y respuestas según sea necesario.

Con la integración de nuestro middleware de registro, nuestra API de gestión de usuarios ahora tiene una capa adicional de capacidades de monitoreo y depuración. Este ejemplo sirve como base para que crees middleware más sofisticado de acuerdo con los requisitos de tu aplicación.

#### Interceptores: transformando y registrando respuestas de la API

En NestJS, los interceptores tienen dos propósitos principales: transformar los datos que se devuelven desde un manejador de ruta y extender el comportamiento básico de respuesta (como el registro). Son ideales para tareas como envolver datos de respuesta en un formato específico, agregar encabezados adicionales o registrar detalles de respuesta para monitoreo. Hasta ahora, solo hemos recibido respuestas integradas de NestJS, pero la mayor parte del tiempo, es posible que necesitemos tener el control de lo que enviamos al cliente. Podemos hacer esto usando interceptores; veamos cómo.

Creemos un interceptor de transformación de respuesta y registro:

#### Planificación de nuestro interceptor

Nuestro objetivo es crear un interceptor que registre los detalles de la respuesta y envuelva los datos de la respuesta en un formato estándar. De esta manera, podemos establecer un contrato claro entre el equipo del lado del servidor y el equipo del lado del cliente.

Este formato puede incluir campos como `success`, `data` y `timestamp`.

#### Implementación del interceptor

En el directorio `src/common/interceptors` (créalo si no existe), agrega un nuevo archivo llamado `response-logging.interceptor.ts`.

Define el interceptor para que registre los detalles de la respuesta y transforme la estructura de la respuesta:

```typescript
// src/common/interceptors/ // responselogging.interceptor.ts import { CallHandler, ExecutionContext, Injectable, NestInterceptor, } from '@nestjs/common'; import { map, Observable } from 'rxjs'; @Injectable() export class ResponseLoggingInterceptor implements NestInterceptor { intercept( context: ExecutionContext, next: CallHandler): Observable<any> { const request = context.switchToHttp().getRequest(); const start = Date.now(); return next.handle().pipe( map((data) => { const executionTime = Date.now() - start; console.log( `Request to ${request.url} took ${executionTime}ms` ); return { success: !(data instanceof Error) && data !== null, data: data, timestamp: new Date().toISOString(), }; }), ); } }
```

Este código parece manejar mucho. Desglosemos:

- **Importaciones:**
  - `CallHandler`, `ExecutionContext`, `Injectable` y `NestInterceptor`: Se importan de `@nestjs/common`. Son bloques de construcción esenciales para crear un interceptor en NestJS.
  - `map` y `Observable`: Importados de `rxjs`. `map`, al igual que la conocida función `Array.prototype.map`, aplica una proyección a cada valor y emite esa proyección en el `Observable` resultante. Ten en cuenta que `Observable` es un concepto central en RxJS para manejar flujos de datos asíncronos. Para obtener más información sobre RxJS, recomiendo dedicar algún tiempo a leer la documentación oficial: [https://rxjs.dev/api/index/function/map](https://rxjs.dev/api/index/function/map).
- **Declaración del interceptor:**
  - `@Injectable()`: Este decorador marca la clase como un proveedor que puede ser administrado por el sistema de inyección de dependencias de Nest.
  - La clase `ResponseLoggingInterceptor` implementa la interfaz `NestInterceptor`, indicando que esta clase es un interceptor.
- **El método `intercept`:** Cada interceptor en NestJS debe implementar el método `intercept`. Este método proporciona acceso a `ExecutionContext` y `CallHandler`.
  - `ExecutionContext`: Proporciona detalles sobre el proceso de petición actual. Aquí, se utiliza para extraer el objeto de petición HTTP.
  - `CallHandler`: Se utiliza para manejar el método manejador de ruta.
- **Lógica de registro de peticiones:** El método comienza capturando la marca de tiempo actual (`start`) para medir el tiempo de ejecución.
  - `next.handle()`: Esto activa la siguiente pieza de middleware o manejador de ruta. Después de llamar a esta función, la petición continúa al siguiente paso en la canalización (como el manejador de ruta real).
- **Transformación de respuesta y registro:** El método `pipe` de RxJS y el operador `map` se utilizan para transformar los datos de respuesta.
  - Dentro de `map`, calculamos `executionTime` restando la hora de inicio de la marca de tiempo actual.
  - Una declaración de registro imprime la URL de la petición y cuánto tiempo tardó en procesarse.
  - La respuesta se transforma luego para que incluya una bandera `success`, los datos originales y una marca de tiempo. Esta estructura se devuelve al cliente.
- **Retorno Observable:** El método devuelve un `Observable`. Esto es necesario porque NestJS admite manejadores de ruta tanto sincrónicos como asincrónicos.

Ahora, apliquemos el interceptor a nuestra API.

#### Integración del interceptor en la aplicación

Aplica este interceptor globalmente o a rutas específicas según sea necesario.

Para la aplicación global, actualiza `AppModule`:

```typescript
// src/app.module.ts import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common'; import { APP_INTERCEPTOR } from '@nestjs/core'; import { ResponseLoggingInterceptor } from './common/interceptors/ response-logging.interceptor'; import { UsersModule } from './users/users.module'; @Module({ imports: [UsersModule], providers: [ AppService, { provide: APP_INTERCEPTOR, useClass: ResponseLoggingInterceptor, }, ], // ...other configurations }) export class AppModule {}
```

Aquí, el token `APP_INTERCEPTOR` se utiliza para aplicar `ResponseLoggingInterceptor` globalmente.

Ahora, observemos nuestro interceptor en acción:

1. Con el interceptor integrado, podemos hacer peticiones a nuestra API y observar el formato de respuesta.
2. Deberíamos ver los datos de respuesta envueltos en la estructura definida en el interceptor, junto con los registros de la consola que indican el tiempo de ejecución de cada petición.

Si bien nuestro interceptor es relativamente simple, el concepto se puede ampliar para escenarios más complejos. En una aplicación del mundo real, los interceptores se utilizan a menudo para tareas como agregar metadatos de paginación, manejar cifrado y descifrado, o incluso transmitir grandes conjuntos de datos.

Con eso, hemos integrado con éxito un interceptor de registro y transformación de respuestas en nuestra API de gestión de usuarios. Los interceptores son una función potente en NestJS que ofrece amplias posibilidades para manipular y ampliar el comportamiento de nuestras respuestas de API.

#### Proveedores personalizados: mejorando la inyección de dependencias

En NestJS, los proveedores personalizados permiten escenarios más avanzados más allá de la inyección de dependencias estándar basada en clases. Pueden ser particularmente útiles al integrarse con servicios de terceros, crear proveedores dinámicos o manejar escenarios de resolución de dependencias más complejos. En el contexto de nuestra API de gestión de usuarios, imagina una situación en la que necesitamos integrarnos con un servicio externo para la recuperación de datos de usuarios.

Implementemos un proveedor personalizado para un servicio de datos de usuario externo:

#### Conceptualización del proveedor personalizado

- Nuestro objetivo es simular una integración de servicios externos, como obtener datos de usuarios de una API externa o un servicio de base de datos.
- Este servicio externo se representará como un proveedor personalizado en nuestra aplicación NestJS.

#### Construcción del servicio externo

Primero, creemos un servicio simple que simule la recuperación de datos de usuarios externos.

En el directorio `src/external-services` (crea este directorio), agrega un archivo llamado `external-user-data.service.ts`:

```typescript
// src/external-services/external-user-data.service.ts export class ExternalUserDataService { async fetchUsers(): Promise<any[]> { // Simulate an HTTP call to an external service return Promise.resolve([ { id: 3, name: 'External User', email: 'external@example.com' } ]); } }
```

#### Creación del proveedor personalizado

Ahora, definamos un proveedor personalizado que utilice este `ExternalUserDataService`.

Configuraremos este proveedor personalizado en `UsersModule`:

```typescript
// src/users/users.module.ts import { Module } from '@nestjs/common'; import { UsersController } from './users.controller'; import { UsersService } from './users.service'; import { ExternalUserDataService } from '../external-services/ external-user-data.service'; @Module({ controllers: [UsersController], providers: [ UsersService, { provide: 'EXTERNAL_USER_DATA_SERVICE', useClass: ExternalUserDataService, }, ], }) export class UsersModule {}
```

Aquí, estamos proporcionando un proveedor personalizado con el token `EXTERNAL_USER_DATA_SERVICE`, que utiliza `ExternalUserDataService`.

A continuación, comprendamos cómo utilizaremos el proveedor personalizado en el servicio.

#### Integración del proveedor personalizado en `UsersService`

Modificaremos `UsersService` para que pueda utilizar este proveedor personalizado y obtener datos de usuario adicionales.

Inyecta el proveedor personalizado en `UsersService` usando el decorador `@Inject`:

```typescript
// src/users/users.service.ts import { Injectable, Inject } from '@nestjs/common'; import { User } from './entities/users.entity'; import { ExternalUserDataService } from '../external-services/ external-user-data.service'; @Injectable() export class UsersService { private readonly users: User[] = [ /* ...existing users */ ]; constructor( @Inject('EXTERNAL_USER_DATA_SERVICE') private externalUserService: ExternalUserDataService ) {} async findAll(): Promise<User[]> { const externalUsers = await this.externalUserService.fetchUsers(); return [...this.users, ...externalUsers]; } // ...other methods }
```

En este servicio, ahora combinamos nuestros usuarios almacenados localmente con los obtenidos del servicio externo.

Ahora, probemos y observemos la integración del proveedor personalizado:

1. Ejecuta la aplicación y realiza una petición al endpoint que activa `findAll` en `UsersService`.
2. Deberíamos ver una lista combinada de usuarios, incluidos los del servicio externo simulado.

Los proveedores personalizados son una puerta de entrada para integrar dependencias y servicios externos complejos en tu aplicación. En escenarios del mundo real, esto podría incluir conexiones de bases de datos, APIs de terceros o servicios creados dinámicamente según los parámetros de tiempo de ejecución.

Con la integración de un proveedor personalizado, nuestra API de gestión de usuarios ahora demuestra un aspecto avanzado de la inyección de dependencias en NestJS. Esta mejora no solo muestra la flexibilidad de NestJS, sino que también te prepara para escenarios en los que se requieren integraciones de servicios complejas, lo que hace que tu aplicación sea escalable y modular.

En la siguiente breve sección, repasaremos algunas de las mejores prácticas para las APIs REST.

---

### Reflexión sobre las mejores prácticas de las APIs REST

Ahora es el momento de reflexionar sobre lo que hemos hecho hasta ahora. Al programar, es posible que hayamos olvidado un par de mejores prácticas que ya conocemos de capítulos anteriores. Entonces, volvamos y veamos dónde no lo hicimos bien y apreciemos las cosas que hicimos bien:

#### Diseño de URLs coherente y orientado a recursos

- **Mejor práctica:** Las APIs REST deben tener URLs claras, lógicas y orientadas a recursos. Cada URL debe representar de manera consistente un recurso específico o una colección de recursos.
- **Implementación en nuestra API:** Nuestra API de gestión de usuarios utiliza rutas RESTful como `/users` para acceder a la colección de usuarios y `/users/:id` para operaciones de usuario específicas.

Aquí hay un fragmento de código de ejemplo:

```typescript
@Controller('users') export class UsersController { @Get() findAll() { /*...*/ } @Get(':id') findOne(@Param('id') id: string) { /*...*/ } // ...other methods }
```

#### Uso de métodos HTTP y códigos de estado

- **Mejor práctica:** Utilizar métodos HTTP (GET, POST, PUT y DELETE) de manera adecuada y responder con los códigos de estado HTTP correctos.
- **Implementación en nuestra API:** Hemos usado GET para recuperar datos, POST para crear un nuevo usuario, PUT para actualizar y DELETE para eliminar. Se utilizan los códigos de estado adecuados, como 200 para operaciones exitosas, 201 para creación y 404 para no encontrado.

Aquí hay un fragmento de código de ejemplo:

```typescript
@Post() createUser(@Body() createUserDto: CreateUserDto) { // Logic to create a user return { status: 'success', code: HttpStatus.CREATED }; }
```

#### Manejo efectivo de errores

- **Mejor práctica:** Implementar un manejo integral de errores para obtener respuestas de error claras e informativas.
- **Implementación en nuestra API:** Utilizar filtros de excepciones para detectar y dar formato a los errores de manera consistente.

Aquí hay un fragmento de código de ejemplo:

```typescript
@Catch(HttpException) export class HttpErrorFilter implements ExceptionFilter { catch(exception: HttpException, host: ArgumentsHost) { // Custom error formatting }}
```

#### Validación y saneamiento de datos

- **Mejor práctica:** Validar los datos entrantes para asegurarse de que cumplan con las expectativas de la API; sanear los datos para evitar vulnerabilidades de seguridad.
- **Implementación en nuestra API:** Utilizar pipes de validación para validar los datos de la petición según los DTOs.

Aquí hay un fragmento de código de ejemplo:

```typescript
@Post() @UsePipes(new ValidationPipe()) createUser(@Body() createUserDto: CreateUserDto) { // User creation logic }
```

#### Mejores prácticas de seguridad

- **Mejor práctica:** Implementar medidas de seguridad como autenticación, autorización, validación de entrada y limitación de velocidad.
- **Implementación en nuestra API:** Usar guards para la protección de rutas. La limitación de velocidad y la protección CSRF se pueden agregar como middleware.

Aquí hay un fragmento de código de ejemplo que muestra el uso de guards:

```typescript
@UseGuards(AuthGuard) @Delete(':id') deleteUser(@Param('id') id: number) { // Delete user logic }
```

Al reflexionar sobre estas mejores prácticas, podemos ver cómo cada una contribuye a hacer que nuestra API de gestión de usuarios sea más robusta, segura, eficiente y fácil de usar. Si bien nuestra implementación cubre los aspectos fundamentales, las aplicaciones del mundo real pueden requerir adaptaciones más profundas de estas prácticas. Esta reflexión sirve como base para construir APIs REST profesionales y de alta calidad con NestJS.

Al concluir nuestra exploración de la creación de un endpoint RESTful básico en nuestra API de gestión de usuarios, hemos sentado una base sólida. Hemos implementado operaciones cruciales de manera efectiva —desde recuperar y crear usuarios hasta actualizarlos y eliminarlos—, todo mientras garantizamos las mejores prácticas en el diseño de APIs. Nuestro viaje nos ha llevado a través de los matices de NestJS, demostrando su poder y elegancia en la creación de APIs eficientes, escalables y mantenibles.

Pero como cualquier desarrollador experimentado sabe, una API nunca es estática. Evoluciona para cumplir con nuevos requisitos, integrar funciones adicionales y mejorar las experiencias del usuario. Esto nos lleva a un aspecto esencial del desarrollo de APIs: el versionado. ¿Cómo adaptamos y expandimos nuestra API sin interrumpir el ecosistema existente? Profundizaremos en esto en la siguiente sección.

---

### Versionado de tu API

En el dinámico mundo del desarrollo de software, el cambio es la única constante. A medida que nuestra API de gestión de usuarios crece para dar cabida a nuevas funciones y mejoras, es crucial gestionar estos cambios de una manera que minimice la interrupción para los usuarios finales. Aquí es donde entra en juego el versionado de APIs. El versionado es la práctica de actualizar la API mientras se mantienen múltiples versiones simultáneamente, lo que garantiza la compatibilidad con versiones anteriores y una transición sin problemas para los usuarios.

En esta sección, exploraremos los siguientes aspectos:

- **Por qué importa el versionado:** Entenderemos la importancia del versionado para mantener y evolucionar una API.
- **Estrategias para el versionado:** Evaluaremos diferentes enfoques de versionado y aprenderemos a seleccionar el más adecuado para nuestra API.
- **Implementación de la versión 2:** Introduciremos la paginación en nuestro método `findAll` y agregaremos un nuevo campo `address` a nuestra entidad `User`, marcando el lanzamiento de la segunda versión de nuestra API.

A través de este proceso, aprenderemos cómo mantener nuestra API flexible y robusta para que sea capaz de evolucionar sin romper los contratos con los clientes existentes.

#### Contexto para la v2

Antes de pasar a los detalles técnicos de la implementación de la versión 2, establezcamos el contexto. La versión 2 de nuestra API de gestión de usuarios no solo mejorará las funcionalidades existentes, sino que también introducirá nuevas características. Nuestro objetivo es agregar paginación al método `findAll`, mejorando significativamente la eficiencia del manejo de datos, especialmente para grandes conjuntos de usuarios. Además, enriqueceremos nuestro modelo de datos de usuario incluyendo un campo `address`, ofreciendo perfiles de usuario más completos. Estas mejoras reflejan requisitos comunes del mundo real, haciendo que nuestra API sea más versátil y centrada en el usuario.

En las siguientes secciones, te guiaremos a través de cada paso de estas mejoras, demostrando cómo hacer evolucionar con elegancia tu API con NestJS mientras la mantienes organizada y fácil de mantener.

#### Introducción a los conceptos de versionado de APIs

En el mundo del desarrollo de APIs, el versionado es una técnica indispensable. Se refiere a la práctica de gestionar cambios y actualizaciones en tu API sin interrumpir el servicio para los usuarios existentes. Exploremos por qué el versionado es crucial y las estrategias comunes que se emplean.

He aquí por qué el versionado es tan importante para las APIs RESTful:

- **Compatibilidad con versiones anteriores (*Backward compatibility*):** Cuando actualizas una API, corres el riesgo de romper la compatibilidad con los clientes que usan versiones anteriores. El versionado te permite introducir nuevas funciones o realizar cambios sin afectar a los usuarios existentes.
- **Evolución de las APIs con el tiempo:** Proporciona un enfoque estructurado para evolucionar una API, adaptándose a nuevos requisitos de negocio, cambios tecnológicos o comentarios de los usuarios.
- **Mantenimiento de múltiples versiones:** En algunos escenarios, es necesario mantener múltiples versiones de una API simultáneamente mientras se atiende a diferentes conjuntos de usuarios o clientes.

Para que funcione, se pueden utilizar un par de estrategias bien conocidas:

- **Versionado por ruta de URL (*URL path versioning*):** Esto implica incluir el número de versión en la ruta de la API (por ejemplo, `/v1/users`). Es sencillo y fácil de entender, lo que lo convierte en una opción popular.
- **Versionado por encabezado (*Header versioning*):** La información de la versión se incluye en el encabezado de la petición HTTP. Este enfoque mantiene limpia la URL pero puede ser menos intuitivo.
- **Versionado por parámetros (*Parameter versioning*):** El número de versión se envía como un parámetro de consulta en la URL. Este método es fácil de implementar pero puede saturar los parámetros de consulta de la API.

Para nuestra API de gestión de usuarios, adoptaremos la estrategia de versionado por ruta de URL debido a su simplicidad y claridad.

#### Configuración de la infraestructura de la versión 2

Ahora que entendemos la importancia y los métodos de versionado, configuremos la infraestructura para la versión 2 de nuestra API de gestión de usuarios.

Primero, debemos habilitar el tipo de versionado de URI aplicando la siguiente actualización en el archivo `main.ts`:

```typescript
// main.ts file import { VersioningType } from '@nestjs/common'; // ... remaining imports // ... remaining code app.enableVersioning({ type: VersioningType.URI, // prefix: 'v', // defaultVersion: '1', });
```

Aquí, habilitamos el versionado en la aplicación. Además, establecemos el versionado basado en URI para nuestra aplicación.

Por defecto, NestJS antepone la letra `v` y te permite especificar una versión predeterminada para tu aplicación.

Una versión se puede aplicar a un controlador completo, pero también se puede aplicar a una ruta específica. En nuestro caso, solo necesitamos tener la versión 2 en el manejador `findAll`.

¿Pero qué pasa con el campo de entrada? Bueno, también existen estrategias para gestionar nuevos campos de entrada, aunque pueden variar según la naturaleza de la base de datos. Esto puede consistir en ejecutar migraciones —o seeders, en nuestro caso de uso de muestra—. Solo lo haremos opcional y especificaremos tanto la versión 1 como la versión 2 para que podamos recuperar el campo.

Basta de charlas: hagamos algo de código real:

Dado que ya hemos habilitado el versionado para nuestra API, agreguemos el campo `address` al archivo `users/entities/users.entity.ts`, así como en el archivo DTO, utilizando el siguiente código:

```typescript
// users/entities/users.entity.ts // v2 field address?: string; // users/dto/create-user.dto.ts // v2 field @IsNotEmpty() @IsOptional() address?: string;
```

Aquí, estamos actualizando el contenido de dos archivos. En el primero (`user.entity.ts`), simplemente agregamos un nuevo campo `address`, que es opcional. Hicimos casi lo mismo en el segundo (`users/dto/createUser.dto.ts`), utilizando los decoradores de `class-validator` para asegurarnos de que el campo de dirección no sea obligatorio y que no esté vacío cuando se envíe.

Ahora, en el archivo `users.service.ts`, realiza los siguientes cambios:

```typescript
// ... everything remains similar // add these two users to our mock array // v2 mocks { id: 3, name: 'Ushindi Joseph', email: 'ushindi@myapp.com', address: '1234, Kinshasa, DRC', }, { id: 4, name: 'Kabeya Jean', email: 'kabeya@myapp.com', address: '1234, Goma, DRC', }, // implement the findAllV2 // findall v2 -- with pagination findAllV2(page = 1, limit = 10) { const start = (page - 1) * limit; const end = page * limit; return { data: this.users.slice(start, end), meta: { page, limit, total: this.users.length, }, }; } // ... the rest of the class methods
```

Como puedes ver, hemos agregado una nueva función que puede tomar `page` y `limit` como parámetros, con valores numéricos predeterminados que establecen el tipo en número para ambos. En este caso, el método devuelve un objeto que contiene datos y metadatos para que el cliente maneje la paginación.

Ahora, hagamos el trabajo restante en el archivo del controlador y actualicemos el controlador actual con los siguientes cambios:

```typescript
// ... remaining controller class code @Version('1') // don't forget to import Version // from @nestjs/common @Get() findAll() { console.log('version 1'); return this.usersService.findAll(); } @Version('2') @Get() findAllV2() { return this.usersService.findAllV2(); }
```

Con estas actualizaciones, decidimos usar la versión anterior del manejador de ruta `findAll` solo para la versión 1 y la nueva implementación con paginación para la versión 2. Con la ayuda del decorador `@Version()`, al acceder a la API con una petición GET en `users/v1`, obtenemos el mismo comportamiento que se mostró inicialmente, mientras que al acceder a la API con una petición GET en `users/v2` obtenemos una versión paginada.

Aquí hay un desafío: ¿qué pasa con el resto de los manejadores de ruta? ¿Cómo los manejamos? Dado que no hemos especificado con qué versión funcionan, por defecto, serán manejados por `defaultVersion`, que se define en la función `bootstrap()`. Podemos seguir adelante y agregar la siguiente opción al parámetro de opciones del método `app.enableVersioning()`, de la siguiente manera:

```typescript
// ... the rest of the code remains the same app.enableVersioning({ type: VersioningType.URI, defaultVersion: '1', })
```

Esto puede parecer resolver nuestro problema ya que todas las rutas existentes tendrán el prefijo `v1` y serán manejadas por la aplicación.

Otro desafío: ¿qué sucede con los clientes existentes que consumen la API sin el versionado en mente o incluso en un plan? Debemos asegurarnos de que todas las implementaciones anteriores sigan funcionando como lo hacían anteriormente y que los recursos sigan estando disponibles. Si bien cada requisito de negocio puede ser diferente, hagamos nuestro compromiso de diseño (*trade-off*):

1. Todas las peticiones sin versionado en sus rutas serán interceptadas y convertidas en peticiones v1, lo que significa que si hacemos una petición GET en `users/`, debemos asegurarnos de consultar `users/v1` en su lugar.
2. Todas las peticiones con versionado válido no cambian, lo que significa que las peticiones con v1 o v2 en sus rutas se procesan tal como se reciben.
3. Para todas las peticiones con la versión incorrecta, que aún no se hayan implementado o tengan un formato no válido, consultamos su correspondiente v2 en su lugar, lo que significa que una petición GET en `users/v555` o `users/v404x` se tratará como `users/v2` en su lugar.

¿Cómo podríamos lograr esto? Espera un segundo: tenemos una solución: el middleware. ¿Recuerdas que nuestro middleware puede interceptar peticiones y actualizarlas según varios criterios? Este es un ejemplo adicional de cuándo pueden ser muy útiles. Para cumplir con este requisito comercial, creemos un nuevo middleware en `common/middleware/version-management.middleware.ts` y peguemos el siguiente contenido:

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common'; import { NextFunction, Request, Response } from 'express'; @Injectable() export class VersionManagementMiddleware implements NestMiddleware { use(req: Request, res: Response, next: NextFunction) { // Extract the first segment of the path const firstPathSegment = req.originalUrl .split('/')[1] ?.toString() ?.toLowerCase(); // Check if the first segment is a version if (!firstPathSegment.startsWith('v')) { // If not, prepend 'v1' to the path req.url= '/v1' + req.url; } else if (!['v1', 'v2'].includes(firstPathSegment)) { // If an invalid version is detected, set to the // latest version ('v2' in this case) req.originalUrl = req.originalUrl.replace(firstPathSegment, 'v2'); } next(); } }
```

Aquí, acabamos de lograr lo que describimos en los compromisos de diseño. Después de realizar la transformación correcta en la propiedad `originalUrl` de la petición, procedemos al manejador de ruta con la función `next()`.

Ahora, debemos aplicar este middleware globalmente para que cada ruta anterior pueda beneficiarse de él. En el archivo `app.module.ts`, agrega el middleware recién creado, de la siguiente manera:

```typescript
configure(consumer: MiddlewareConsumer) { // needs to be imported consumer .apply( LoggingMiddleware, VersionManagementMiddleware ) .forRoutes('*'); // apply for all routes } // ... the rest of the file remains the same
```

Como puedes ver, hemos agregado el middleware a la función `apply()`. Recuerda que el orden cuenta y el middleware se ejecutará en el mismo orden en que se aplica; es decir, iniciaremos sesión inmediatamente después de recibir la petición antes de aplicar la transformación y verificación del versionado.

¡Bien hecho! Probemos esto y asegurémonos de que todo funcione como se espera usando el comando `yarn start:dev` antes de pasar a otro aspecto importante de las APIs REST escalables.

Bien hecho: esta capacidad de versionado le está dando a nuestra aplicación algunos poderes mágicos. Ahora somos conscientes de cómo podemos escalar nuestra API, implementar cambios importantes para una versión específica de la API y seguir sirviendo al cliente final de manera resistente. Ahora, hablemos del manejo de errores.

---

### Manejo robusto de errores en APIs REST

En el ámbito del desarrollo de APIs RESTful, el manejo robusto de errores no es solo un lujo, es una necesidad. Es crucial para mantener un servicio confiable y fácil de usar. A medida que hemos estado construyendo nuestra API de gestión de usuarios, asegurarnos de que cualquier error se maneje con elegancia mejorará la calidad general y la confiabilidad de la API. Profundicemos en las estrategias e implementaciones que constituyen un manejo robusto de errores en APIs REST.

#### Comprensión del manejo de errores

En las APIs, el manejo de errores implica detectar errores, responder con mensajes de error y códigos de estado apropiados y, cuando sea posible, registrar estos errores para un análisis posterior. Los errores bien manejados brindan claridad al cliente sobre qué salió mal y qué acción pueden tomar, si corresponde. Esto fomenta una relación de confianza y transparencia entre la API y sus consumidores.

#### Aspectos clave del manejo robusto de errores

Estos son algunos de los aspectos clave de un sistema robusto de manejo de errores que debes considerar cuando trabajes en tu próxima API de NestJS:

- **Mensajes de error claros:** Los mensajes de error deben brindar una comprensión clara de qué salió mal. Evita la jerga técnica que pueda resultar confusa o intimidante para el usuario final.
- **Códigos de estado apropiados:** Utiliza los códigos de estado HTTP correctos para indicar la naturaleza del error (4xx para errores del cliente, 5xx para errores del servidor, etc.).
- **Estructura coherente de respuesta de errores:** Asegúrate de que todos los errores devuelvan un formato consistente, facilitando que los clientes los analicen y manejen.
- **Registro y monitoreo:** Implementa el registro para capturar errores con fines de monitoreo y depuración. Esto puede ayudar a identificar problemas rápidamente y mejorar la API.

#### Implementación del manejo de errores en la API de gestión de usuarios

Ahora, implementemos estas mejores prácticas en nuestra API existente para obtener un contexto más práctico a partir de las teorías:

#### Filtros de excepciones personalizados

NestJS permite el uso de filtros de excepciones para el manejo personalizado de errores.

Implementa un filtro de excepciones personalizado que capture las excepciones y les dé formato en una estructura JSON consistente:

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common'; import { Response } from 'express'; @Catch(HttpException) export class HttpErrorFilter implements ExceptionFilter { catch(exception: HttpException, host: ArgumentsHost) { const ctx = host.switchToHttp(); const response = ctx.getResponse<Response>(); const status = exception.getStatus(); const errorResponse = exception.getResponse(); response.status(status).json({ statusCode: status, message: errorResponse['message'] || null, timestamp: new Date().toISOString(), path: ctx.getRequest().url, }); } }
```

Este filtro captura todas las excepciones del tipo `HttpException`, devolviendo una respuesta JSON estructurada que incluye el código de estado, el mensaje, la marca de tiempo y la ruta de la petición.

#### Uso de interceptores para el manejo adicional de errores

Los interceptores también pueden gestionar errores agregando capas adicionales de manejo de errores o transformando las respuestas de error.

#### Errores de validación y de lógica de negocio

- Asegúrate de que todas las entradas de usuario se validen mediante pipes y de que cualquier error de validación devuelva mensajes informativos.
- Para errores de lógica de negocio (como `User not found` o `Email already exists`), lanza excepciones personalizadas que puedan ser capturadas y formateadas por los filtros de excepciones.

#### Prueba de escenarios de error

- Implementa pruebas unitarias y de integración para cubrir varios escenarios de error. Asegúrate de que los errores se capturen y manejen como se espera.
- Prueba varias peticiones de API con datos no válidos e intentos de acceso no autorizados, y simula errores de servidor para validar las respuestas de error.

#### Mejores prácticas para errores orientados al cliente

Aquí hay algunas mejores prácticas a considerar:

- **Evitar exponer detalles confidenciales:** Nunca envíes seguimientos de pila (*stack traces*) ni información confidencial al cliente en los mensajes de error.
- **Limitación de velocidad y manejo de abusos:** Implementa la limitación de velocidad para evitar abusos y manejar las peticiones excesivas con elegancia, utilizando códigos de estado HTTP apropiados como `429 Too Many Requests`.
- **Documentación:** Documenta los errores comunes y sus significados en la documentación de tu API para ayudar a los clientes a entender cómo manejarlos.

El manejo robusto de errores es un componente crítico de una API profesional y confiable. Al implementar filtros de excepciones personalizados, registro de errores adecuado y estructuras de error consistentes, podemos mejorar significativamente la resiliencia y usabilidad de nuestra API de gestión de usuarios. Esto no solo conduce a una mejor experiencia de desarrollo para los consumidores de la API, sino que también facilita el mantenimiento y la resolución de problemas para los desarrolladores de la API. A medida que nuestra API evoluciona, mantener un fuerte enfoque en el manejo de errores garantizará que siga siendo robusta, fácil de usar y confiable.

---

### Documentación de tu API

En el camino del desarrollo de APIs, la documentación no es simplemente un toque final; es una parte integral de la creación de una API exitosa, utilizable y mantenible. Ahora que hemos desarrollado nuestra API de gestión de usuarios, proporcionar una documentación clara y completa es esencial para garantizar que los desarrolladores la entiendan y la utilicen fácilmente. Profundicemos en las mejores prácticas y estrategias para documentar eficazmente una API.

#### La importancia de la documentación de la API

Una buena documentación es crucial para cualquier API. Sirve como guía, explicando cómo funciona la API, qué ofrece y cómo usarla. Esto es especialmente importante en entornos colaborativos y APIs públicas, donde la comunicación clara es clave para la adopción y el éxito de la API.

#### Componentes clave de una documentación de API efectiva

Al documentar una API, hay algunos componentes clave a tener en cuenta:

- **Visión general de la API:** Comienza con una descripción general de alto nivel de lo que hace la API, sus características principales y sus posibles casos de uso.
- **Autenticación y autorización:** Explica claramente cómo los clientes deben autenticarse y autorizarse con tu API. Incluye cualquier clave o token que puedan necesitar.
- **Descripciones de endpoints:** Cada endpoint debe estar completamente documentado con su propósito, URI, encabezados requeridos, formatos de petición y respuesta, y cualquier parámetro de consulta o ruta.
- **Códigos y mensajes de error:** Documenta las respuestas de error comunes y lo que significan para ayudar a los usuarios a solucionar problemas.
- **Ejemplos y casos de uso:** Proporciona ejemplos prácticos de peticiones y respuestas. Los escenarios o casos de uso del mundo real pueden mejorar significativamente la comprensión.

#### Implementación de la documentación en la API de gestión de usuarios

Con una sólida comprensión de la importancia de la documentación de la API, ahora podemos pasar a implementar la documentación para nuestra API de gestión de usuarios en NestJS. Al aprovechar las herramientas de documentación automatizadas, podemos generar documentación completa de la API que se mantiene actualizada a medida que evoluciona nuestra aplicación. NestJS ofrece un excelente soporte para herramientas como Swagger (OpenAPI) para generar y mantener la documentación de la API con facilidad.

#### Adición de anotaciones de Swagger a DTOs y entidades

Para proporcionar una rica documentación de la API, es crucial agregar metadatos a tus DTOs y entidades. NestJS ofrece el decorador `@ApiProperty` del paquete `@nestjs/swagger`, que te permite definir y describir las propiedades de tus DTOs y entidades para Swagger. Esto mejora la claridad de la documentación de tu API al definir explícitamente cada campo, su tipo y cualquier detalle relevante.

Aquí hay un ejemplo de cómo usar `@ApiProperty` con la API de gestión de usuarios:

```typescript
import { ApiProperty } from '@nestjs/swagger'; import { IsString, IsEmail, IsOptional } from 'class-validator'; export class CreateUserDto { @ApiProperty({ description: 'The name of the user', example: 'John Doe', }) @IsString() name: string; @ApiProperty({ description: 'The email address of the user', example: 'john.doe@example.com', }) @IsEmail() email: string; @ApiProperty({ description: 'The password for the user account', example: 'password123', }) @IsString() password: string; @ApiProperty({ description: 'Optional role of the user', example: 'admin', required: false, }) @IsOptional() @IsString() role?: string; }
```

En este ejemplo, el decorador `@ApiProperty` se utiliza para documentar cada propiedad del DTO, incluidas las descripciones y los valores de ejemplo. Esto se reflejará en la documentación de Swagger generada, lo que facilitará a los consumidores de la API la comprensión de los campos obligatorios y opcionales.

#### Configuración de Swagger en el controlador

Además de documentar DTOs y entidades, puedes usar decoradores de Swagger para mejorar la documentación de tu controlador. NestJS proporciona decoradores como `@ApiOperation`, `@ApiBearerAuth` y `@ApiOkResponse` para que puedas documentar el propósito y el comportamiento de cada endpoint.

Aquí hay un ejemplo de cómo documentar un método de controlador en la API de gestión de usuarios:

```typescript
import { Controller, Get, Post, Body, Param } from '@nestjs/common'; import { ApiTags, ApiOperation, ApiBearerAuth, ApiOkResponse } from '@nestjs/swagger'; import { CreateUserDto } from './dto/create-user.dto'; import { UserService } from './user.service'; import { UserEntity } from './entities/user.entity'; @ApiTags('Users') @Controller('users') export class UserController { constructor(private readonly userService: UserService) {} @ApiOperation({ summary: 'Create a new user' }) @ApiOkResponse({ description: 'The user has been successfully created.', type: UserEntity, }) @Post() async create(@Body() createUserDto: CreateUserDto): Promise<UserEntity> { return this.userService.create(createUserDto); } @ApiOperation({ summary: 'Retrieve a user by ID' }) @ApiOkResponse({ description: 'The user has been successfully retrieved.', type: UserEntity, }) @Get(':id') async findOne(@Param('id') id: string): Promise<UserEntity> { return this.userService.findOne(id); } }
```

En este controlador, el decorador `@ApiOperation` proporciona un resumen de la funcionalidad del endpoint, mientras que `@ApiOkResponse` especifica el tipo y la descripción de la respuesta esperada. La documentación de Swagger para estos endpoints incluirá automáticamente esta información.

#### Generación de documentación de Swagger

Una vez que hayas anotado tus DTOs, entidades y controladores, puedes configurar Swagger en tu aplicación NestJS para que genere y sirva automáticamente la documentación de la API. Usa `SwaggerModule` en tu archivo `main.ts` para configurar y exponer la documentación:

```typescript
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger'; import { NestFactory } from '@nestjs/core'; import { AppModule } from './app.module'; async function bootstrap() { const app = await NestFactory.create(AppModule); const config = new DocumentBuilder() .setTitle('User Management API') .setDescription('API for managing users') .setVersion('1.0') .build(); const document = SwaggerModule.createDocument(app, config); SwaggerModule.setup('api', app, document); await app.listen(3000); } bootstrap();
```

En esta configuración, el método `SwaggerModule.setup` se utiliza para servir la interfaz de usuario de Swagger en la ruta `/api`, que mostrará la documentación de tu API basada en las anotaciones proporcionadas en toda tu aplicación.

#### Mantener la documentación actualizada

A medida que tu API evoluciona, es esencial garantizar que tu documentación de Swagger se mantenga precisa y actualizada. Puedes lograr esto haciendo lo siguiente:

- **Mantenimiento de DTOs y entidades:** Asegúrate de que todos los DTOs y entidades estén debidamente anotados con `@ApiProperty`.
- **Versionado:** Indica cambios específicos de la versión en la documentación de tu API si implementas el versionado.
- **Pruebas:** Revisa periódicamente la documentación de Swagger generada para confirmar que refleja el estado actual de la API.

Al seguir estas prácticas, puedes asegurarte de que la documentación de tu API no solo sea precisa sino también útil para los desarrolladores que utilizan tu API.

#### Ejemplo: documentación completa de Swagger de la API de gestión de usuarios

A continuación se muestra un ejemplo de cómo podría verse la documentación completa de Swagger para la API de gestión de usuarios:

- **Visión general de la API:** Enumera todos los endpoints disponibles con resúmenes y descripciones.
- **Definiciones de DTO:** Muestra la estructura de cada DTO, incluidos los campos obligatorios y opcionales.
- **Formatos de respuesta:** Documenta la estructura esperada de las respuestas, incluidos datos de ejemplo.
- **Manejo de errores:** Proporciona detalles sobre códigos de error y problemas potenciales que los usuarios pueden encontrar.
- **Versionado:** Si corresponde, la documentación incluirá diferentes versiones de la API y sus respectivos cambios.

Al usar Swagger junto con `@ApiProperty` y anotaciones de controlador, proporcionas a los desarrolladores una referencia intuitiva y detallada para interactuar con tu API.

#### Mejores prácticas para mantener la documentación de la API

Las siguientes son algunas de las mejores prácticas para la documentación de APIs:

- **Mantener la documentación actualizada:** Actualiza siempre la documentación a la par con los cambios en la API. La documentación desactualizada puede generar confusión y un uso indebido de la API.
- **Comentarios de los usuarios:** Fomenta e incorpora los comentarios de los usuarios para mejorar la documentación. Sus conocimientos pueden revelar qué aspectos son confusos o faltan.
- **Accesibilidad y legibilidad:** Haz que la documentación sea fácilmente accesible y asegúrate de que esté escrita en un lenguaje claro y directo.

Las APIs bien documentadas son tan importantes como las APIs bien diseñadas. Capacitan a los desarrolladores para usar la API de manera correcta y eficiente, reduciendo las curvas de aprendizaje y las posibles frustraciones. En nuestra API de gestión de usuarios, la documentación eficaz actúa como una hoja de ruta, guiando a los usuarios a través de las capacidades y el uso de la API. A medida que continuamos evolucionando y mejorando nuestra API, mantener la documentación clara, completa y actualizada seguirá siendo una prioridad máxima, asegurando que siga siendo un recurso valioso para todos sus usuarios.

El código fuente para el proyecto de muestra de este capítulo se puede encontrar en el repositorio de GitHub de este libro: [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch05](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch05). Siéntete libre de bifurcarlo y compararlo con tu propia versión o usarlo como plantilla para tus futuras APIs REST.

---

### Resumen

En este capítulo, realizamos una inmersión profunda en la creación de una API REST funcional mientras abordamos aspectos clave desde la configuración básica hasta las funciones avanzadas. Comenzamos explorando los principios REST y pasamos rápidamente al desarrollo práctico, configurando rutas, manejando la autenticación e integrando mecanismos de manejo de errores. Luego, nuestro viaje avanzó hacia el versionado de APIs, donde integramos características como la paginación y nuevos atributos de usuario sin problemas, garantizando la compatibilidad con versiones anteriores. Algo esencial para nuestro desarrollo fue el énfasis en un manejo de errores robusto y una documentación efectiva. Lo hicimos utilizando las potentes herramientas de NestJS para mejorar la confiabilidad y la usabilidad de la API.

Con estas habilidades fundamentales en su lugar, el [Capítulo 6](https://subscription.packtpub.com/book/web-development/9781835468609/6) nos espera con un emocionante cambio de enfoque. *Desatando el Poder de GraphQL y la Arquitectura de Apollo Federation en NestJS* promete expandir nuestros horizontes en el reino de GraphQL, ofreciendo una nueva perspectiva sobre el desarrollo de APIs. Exploraremos las complejidades del lenguaje de consulta de GraphQL, configuraremos un servidor GraphQL utilizando Apollo Federation y profundizaremos en funciones avanzadas, preparando el escenario para construir aplicaciones web más dinámicas, eficientes y escalables.
