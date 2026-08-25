# Parte 2: APIs REST y GraphQL en NestJS

## Capítulo 6: Desatando el Poder de GraphQL y la Arquitectura de Apollo Federation en NestJS

¡Bienvenido a este viaje transformador al mundo de GraphQL con NestJS! Después de profundizar en los aspectos esenciales de NestJS y el desarrollo de APIs REST en los capítulos anteriores, ahora estamos preparados para elevar nuestras habilidades al siguiente nivel. El Capítulo 6 está dedicado a descifrar las funciones dinámicas y potentes de GraphQL, un lenguaje de consulta avanzado para APIs, y a explorar cómo se integra a la perfección con NestJS. Este capítulo también te presentará Apollo Federation, sentando las bases para construir arquitecturas escalables basadas en microservicios.

¿Estás listo para sumergirte profundamente en el reino del desarrollo moderno de APIs? Aquí está el emocionante camino que recorreremos juntos en este capítulo:

- Comprensión de los fundamentos de GraphQL en NestJS
- Construcción y optimización de una API GraphQL con NestJS
- Datos en tiempo real con suscripciones GraphQL
- Asegurando GraphQL: autenticación y autorización
- Introducción a Apollo Federation

A medida que avances en este capítulo, te animo a que programes a la par y pongas estos conceptos en práctica. Cada sección incluye ejercicios prácticos diseñados para reforzar tu aprendizaje y mejorar tus habilidades. Al final de este capítulo, no solo tendrás un conocimiento profundo de GraphQL y Apollo Federation, sino también experiencia práctica en la creación de APIs eficientes y escalables con NestJS.

Por lo tanto, ¡embarquémonos en esta emocionante aventura y liberemos todo el potencial de GraphQL y Apollo Federation en nuestros proyectos de desarrollo web!

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Comprensión de los fundamentos de GraphQL en NestJS

En este capítulo, nos embarcaremos en un viaje para comprender GraphQL, un lenguaje de consulta revolucionario que ha redefinido la forma en que pensamos sobre las APIs. A diferencia de las APIs REST tradicionales, GraphQL ofrece un enfoque más flexible y eficiente para manejar peticiones y respuestas de datos. Descubramos los conceptos centrales de GraphQL y exploremos cómo NestJS mejora estas características, guiándote para construir APIs más flexibles y eficientes.

#### ¿Qué es GraphQL?

GraphQL, desarrollado por Facebook en 2015, es un lenguaje de consulta para APIs que permite a los clientes solicitar solo los datos que necesitan, proporcionando una alternativa más eficiente y flexible a las APIs RESTful tradicionales. Con GraphQL, los clientes pueden especificar la forma y estructura exactas de los datos que requieren del servidor, reduciendo los problemas de recuperación excesiva (*over-fetching*) y recuperación insuficiente (*under-fetching*), algo que es común en las arquitecturas RESTful. Permite a los desarrolladores definir un esquema que describe los datos disponibles en la API, tras lo cual los clientes pueden consultar este esquema para obtener con precisión los datos que necesitan, mejorando el rendimiento y la productividad del desarrollador. Además, GraphQL admite actualizaciones de datos en tiempo real a través de suscripciones, lo que lo hace adecuado para aplicaciones web y móviles modernas, sin importar la base de datos o la tecnología utilizada para construir tu aplicación.

Estas son las características clave de GraphQL:

- **Recuperación de datos eficiente:** A diferencia de REST, donde a menudo se recuperan datos de más o de menos, GraphQL permite a los clientes solicitar exactamente lo que necesitan: nada más y nada menos. Esta precisión reduce significativamente el tráfico de red y mejora el rendimiento.
- **Endpoint único:** Las APIs GraphQL tienen un único endpoint, lo que simplifica la estructura de la API. Este punto único de entrada contrasta con los múltiples endpoints en las APIs REST.
- **Fuertemente tipado:** Las APIs GraphQL están fuertemente tipadas. Esto significa que cada operación está definida por un sistema de tipos y se ejecuta con predictibilidad.
- **Datos en tiempo real con suscripciones:** GraphQL admite actualizaciones de datos en tiempo real mediante suscripciones, lo que lo hace ideal para aplicaciones que requieren retroalimentación en tiempo real.

#### GraphQL y NestJS: una combinación perfecta

NestJS proporciona soporte de primera clase para GraphQL, lo que facilita su configuración y uso dentro de tu aplicación. Aprovecha el poder de TypeScript y se integra a la perfección con el módulo GraphQL.

#### Configuración de GraphQL en NestJS

Cuando trabajas en cualquier proyecto NestJS, puedes agregar soporte para GraphQL. Sigue estos pasos para hacerlo:

1. Para comenzar a usar GraphQL, debes instalar los paquetes requeridos:

```bash
$ npm install @nestjs/graphql graphql-tools graphql apollo-server-express
```

2. NestJS te permite crear un módulo dedicado para GraphQL. Este módulo puede configurar el servidor GraphQL:

```typescript
// graphql.module.ts import { Module } from '@nestjs/common'; import { GraphQLModule } from '@nestjs/graphql'; @Module({ imports: [ GraphQLModule.forRoot({ autoSchemaFile: true, }), ], }) export class GraphqlModule {}
```

En esta configuración, `autoSchemaFile: true` le indica a NestJS que genere automáticamente el esquema basándose en tus definiciones de TypeScript. Este enfoque se llama *code-first* (código primero), y lo exploraremos con más detalle en breve.

#### Ventajas de usar GraphQL con NestJS

Construir APIs GraphQL con NestJS tiene varias ventajas:

- **Seguridad de tipos con TypeScript:** El soporte de TypeScript en NestJS mejora la experiencia de desarrollo con GraphQL al proporcionar seguridad de tipos y reducir los errores en tiempo de ejecución.
- **Estructura modular:** La estructura modular de NestJS complementa el enfoque de GraphQL. Puedes organizar tus resolvers y tipos dentro de módulos dedicados, manteniendo una base de código limpia y escalable.
- **Integración con características existentes:** GraphQL, cuando se usa en NestJS, puede integrarse a la perfección con otras características como guards, filtros e interceptores, ofreciendo una rica experiencia de desarrollo.
- **Herramientas potentes:** Con herramientas como Apollo Federation, puedes construir servicios GraphQL escalables y eficientes, algo que exploraremos en detalle en las siguientes secciones.

Comprender los fundamentos de GraphQL sienta las bases para construir APIs más eficientes, flexibles y escalables. Su integración con NestJS no solo mejora estas capacidades sino que también proporciona un entorno familiar y potente para los desarrolladores. A medida que avancemos, profundizaremos en la construcción de una API GraphQL con NestJS, aprovechando estos fundamentos para crear una API que no solo sea potente sino también un placer de usar.

#### Grandes empresas que utilizan GraphQL en producción

Al momento de escribir este texto, es común encontrar empresas que utilizan GraphQL en producción. Veamos algunas de las más famosas:

- **Meta:** Anteriormente conocida como Facebook, Meta es la empresa que creó GraphQL. Después de enfrentar demasiados desafíos relacionados con la recuperación excesiva e insuficiente de recursos, decidieron migrar de APIs RESTful a GraphQL. Más tarde, alrededor de 2015, lo lanzaron como código abierto y lo abrieron a la adopción global.
- **Netflix:** Alrededor de 2018, los ingenieros de Netflix decidieron utilizar GraphQL y mejoraron la entrega de contenido de video, lo que resultó en menos cuellos de botella en el ancho de banda.
- **Zalando:** El equipo de ingeniería de Zalando pasó de un diseño de API RESTful a una solución unificada de Backend for Frontend (*UBFF*), exponiendo un único punto de entrada para consumir en sus aplicaciones cliente (aplicaciones móviles y sitios web).

Hay muchas otras historias de éxito con la adopción de APIs GraphQL volviéndose cada vez más popular, incluidas las de Samsung, LinkedIn y muchas más.

---

### Construcción y optimización de una API GraphQL con NestJS

A medida que nos adentramos más en el mundo de GraphQL, llegamos a un momento crucial en nuestro viaje: construir y optimizar una API GraphQL utilizando NestJS. Este capítulo es donde la teoría se encuentra con la práctica, y aplicaremos nuestro nuevo conocimiento de GraphQL para construir una aplicación del mundo real. Nuestro enfoque será desarrollar el mismo sistema de gestión de usuarios que construimos en el [Capítulo 5](https://subscription.packtpub.com/book/web-development/9781835468609/5) con APIs REST, pero esta vez, aprovecharemos el poder de GraphQL para mejorar sus capacidades y eficiencia.

#### Contextualización de nuestra aplicación

Primero, reconstruiremos nuestro sistema de gestión de usuarios, un proyecto fundamental que incluye funciones básicas como crear, recuperar, actualizar y eliminar información de usuarios. Sin embargo, con GraphQL, abordaremos estas tareas con mayor delicadeza y flexibilidad. El objetivo es demostrar cómo GraphQL puede transformar la forma en que manejamos las operaciones de datos, ofreciendo un enfoque más simplificado y centrado en el cliente en comparación con los métodos tradicionales de RESTful.

#### Dos enfoques para GraphQL con NestJS

La versatilidad de NestJS con GraphQL nos permite explorar dos enfoques distintos para construir nuestra API:

- **Enfoque Code-first (código primero):** Con este enfoque, nos centraremos en el uso de clases y decoradores de TypeScript para definir el esquema de GraphQL. NestJS generará automáticamente el esquema basándose en nuestro código, lo que nos permitirá trabajar con conceptos y patrones familiares. Este enfoque es particularmente beneficioso para aquellos que prefieren trabajar con lenguajes fuertemente tipados y disfrutar de los beneficios de la retroalimentación inmediata y la validación que los acompaña.
- **Enfoque Schema-first (esquema primero):** Alternativamente, exploraremos el enfoque schema-first, donde comenzaremos escribiendo el Lenguaje de Definición de Esquemas (*Schema Definition Language* - SDL) de GraphQL. Este método a menudo se prefiere por su sencillez y la facilidad de definir el esquema por adelantado. Es particularmente útil cuando el esquema se acuerda de antemano o cuando trabajamos en equipos, donde el esquema sirve como un contrato.

La belleza de este capítulo radica en la construcción paralela del mismo sistema de gestión de usuarios utilizando ambos enfoques. Esta perspectiva dual no solo profundizará tu comprensión de GraphQL en NestJS, sino que también te equipará con los conocimientos necesarios para elegir el enfoque que mejor se adapte a las necesidades de tu proyecto.

En la siguiente sección, construiremos nuestra API utilizando el enfoque code-first.

#### Enfoque Code-first

Construyamos una aplicación de gestión de usuarios, como hicimos en el [Capítulo 5](https://subscription.packtpub.com/book/web-development/9781835468609/5), pero utilizando el enfoque code-first.

Primero, creemos un nuevo proyecto NestJS ejecutando el siguiente comando en la terminal:

```bash
$ nest new user-management-graphql
```

Este comando generará un nuevo proyecto llamado `user-management-graphql`.

> **Consejo**  
> ⚒ Tómate un momento para explorar nuevamente la estructura del proyecto generado. Como puedes ver, la estructura del proyecto se parece a la de un proyecto de API REST con archivos de controlador para manejar peticiones HTTP.

Dado que aquí estamos construyendo una API GraphQL, nuestro primer desafío será transformar este proyecto en uno de GraphQL. Para hacerlo, debemos seguir unos sencillos pasos. En este punto, no olvides crear una rama llamada `code-first` o algo similar para que puedas implementar el enfoque schema-first más adelante:

1. Instala todos los paquetes requeridos usando el siguiente comando:

```bash
$ yarn add @nestjs/graphql @apollo/server @nestjs/apollo graphql apollo-server-express apollo-server-core
```

2. En el archivo `app.module.ts`, cambia el arreglo `imports` vacío para que se vea así:

```typescript
// app/app.module.ts imports: [ GraphQLModule.forRoot<ApolloDriverConfig>({ driver: ApolloDriver, autoSchemaFile: join(process.cwd(), 'src/schema.gql'), playground: false, plugins: [ ApolloServerPluginLandingPageLocalDefault() ], }), ],
```

Aquí, `GraphQLModule` se ha importado de `@nestjs/graphql`, `ApolloDriver` y `ApolloDriverConfig` de `@nestjs/apollo`, y `ApolloServerPluginLandingPageLocalDefault` de `@apollo/server/plugin/landingPage/default`.

Hemos agregado un par de propiedades más a `GraphQLModule`:

- Al establecer la propiedad `autoSchemaFile` en `join(process.cwd(), 'src/schema.gql')`, podemos usar el enfoque code-first en lugar del enfoque schema-first. De esta manera, NestJS generará automáticamente nuestros esquemas basados en nuestras clases de modelo. Hemos optado por almacenar el archivo de esquema en la ubicación `src/schema.gql`, pero también puedes dejar que NestJS determine la ubicación simplemente configurando `autoSchemaFile` en `true`.
- Establecemos `playground` en `false` para que podamos usar Apollo Sandbox en lugar de `graphql-playground` como un IDE de GraphQL para el desarrollo local.
- Definimos los complementos como `ApolloServerPluginLandingPageLocalDefault` según el valor de `playground`. Si se establece en `true`, no necesariamente necesitamos agregarlo para que forme parte de nuestros complementos.

3. Ahora, renombremos el archivo `app.controller.ts` a `app.resolver.ts` e ingresemos el siguiente código:

```typescript
// app/app.resolver.ts import { Query, Resolver } from '@nestjs/graphql'; import { AppService } from './app.service'; @Resolver() export class AppResolver { constructor( private readonly appService: AppService ) {} @Query(() => String) getHello(): string { return this.appService.getHello(); } }
```

Hay algunas diferencias en este código en comparación con el código que vimos anteriormente:

- El decorador `@Controller` se ha cambiado a `@Resolver`.
- El decorador `@Get` se ha cambiado a `@Query(() => String)`.
- Todo lo demás permanece igual.

4. Volvamos a nuestro archivo `app.module.ts` y corrijamos las importaciones haciendo lo siguiente:
   - Cambiar la importación del controlador. Ya no deberíamos tener uno, sino un resolver en su lugar.
   - Eliminar el controlador `AppController` del arreglo `controllers`.
   - Agregar `AppResolver` al arreglo `providers` junto a `AppService`.

Nuestro archivo ahora debería verse así:

```typescript
import { Module } from '@nestjs/common'; import { AppResolver } from './app.resolver'; import { AppService } from './app.service'; import { GraphQLModule } from '@nestjs/graphql'; import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo'; import { join } from 'path'; import { ApolloServerPluginLandingPageLocalDefault } from '@apollo/server/plugin/landingPage/default'; @Module({ imports: [ GraphQLModule.forRoot<ApolloDriverConfig>({ driver: ApolloDriver, autoSchemaFile: join(process.cwd(), 'src/schema.gql'), playground: false, plugins: [ ApolloServerPluginLandingPageLocalDefault() ], }), ], controllers: [], providers: [AppService, AppResolver], }) export class AppModule {}
```

Con eso, hemos transformado nuestra API REST inicial en una de GraphQL en solo unos pocos pasos.

Ejecutemos la aplicación una vez más y confirmemos que nuestra API es ahora una API GraphQL:

```bash
$ yarn start:dev
```

Deberías ver la siguiente salida:

*Figura 6.1: GraphQL playground*

¡Bien hecho: has hecho funcionar tu primera API GraphQL con NestJS! Es bastante impresionante cómo pasamos de una API REST a una de GraphQL tan rápidamente. Todo esto es gracias al framework que estamos usando.

Ahora, echemos un vistazo a lo que se ha generado para nosotros como el esquema para el proyecto hasta ahora. Para hacerlo, abre el archivo que creaste en `src/schema.gql`; observa que hemos generado algo de código. Así es como funciona el enfoque code-first: no tenemos que preocuparnos por el esquema del proyecto; todo lo que tenemos que hacer es concentrar nuestra energía en escribir código, especificar los tipos correctos y escribir nuestras mutaciones y consultas. NestJS generará el esquema por nosotros.

> **⚒ Prueba esto**  
> Juega con el playground y realiza algunas peticiones para ver cómo responde la API.

En esta etapa, podemos construir nuestro módulo de usuarios y agregarle algunas funciones. Ten en cuenta que ahora usaremos la CLI de Nest con más frecuencia, ya que aumenta la productividad. Sigamos estos sencillos pasos juntos:

1. El primer paso es generar el recurso de usuarios. Podemos hacer esto usando la CLI de Nest, de la siguiente manera:

```bash
$ nest g resource users # ❯ GraphQL (code first) # ? Would you like to generate CRUD entry points? (Y/n) y
```

Este comando te pedirá que elijas la capa de transporte que estás utilizando. Asegúrate de elegir `GraphQL (code first)`. Después, te preguntará si deseas que genere puntos de entrada CRUD para ti; simplemente acepta.

Al hacer esto, la carpeta `users` se creará con todos los archivos requeridos en ella, como los de nuestros resolvers, servicios y DTOs.

> **⚒ Prueba esto**  
> Echa un vistazo a la base de código generada y anota cualquier diferencia en comparación con la API REST que construimos.

Ahora, usemos esto como punto de partida para que podamos construir nuestra aplicación de gestión de usuarios.

Demos forma al módulo generado para que nuestra aplicación funcione como queremos. Primero, actualiza el archivo `entities/user.entity.ts` con el siguiente contenido:

```typescript
// users/entities/user.entity.ts import { ObjectType, Field, Int } from '@nestjs/graphql'; @ObjectType() export class User { @Field(() => Int, { description: 'the unique id of a user' }) id: number; @Field(() => String, { description: 'the name of a user' }) name: string; @Field(() => String, { description: 'the email of a user' }) email: string; }
```

Esta es la misma entidad que usamos en nuestra API REST; la única diferencia son los decoradores que agregamos a la definición. Todo esto ayuda a NestJS a construir el archivo de esquema. Estamos usando el decorador `@Field()` para especificar que la propiedad de clase es un campo de GraphQL; en otras palabras, ese decorador no es necesario para las APIs REST.

Ahora, echemos un vistazo a nuestro archivo de servicio. Necesitamos usar la misma estrategia de simulación (*mocking*) que usamos anteriormente, ya que todavía no estamos interactuando con datos reales de una base de datos. Actualicemos el código existente:

```typescript
import { Injectable } from '@nestjs/common'; import { CreateUserInput } from './dto/create-user.input'; import { UpdateUserInput } from './dto/update-user.input'; import { User } from './entities/user.entity'; @Injectable() export class UsersService { private readonly users: User[] = [ { id: 1, name: 'Tshimanga Mukendi', email: 'tshim@myapp.com', }, { id: 2, name: 'Kasereka Akim', email: 'kase@myapp.com', }, { id: 3, name: 'Ushindi Joseph', email: 'ushindi@myapp.com', }, { id: 4, name: 'Kabeya Jean', email: 'kabeya@myapp.com', }, ]; create(createUserInput: CreateUserInput) { this.users.push(createUserInput); return createUserInput; } findAll() { return this.users; } findOne(id: number) { return this.users.find((user) => user.id === id); } update(id: number, updateUserInput: UpdateUserInput) { const user = this.users.find( (user) => user.id === id ); if (!user) { return null; } Object.assign(user, updateUserInput); return user; } remove(id: number) { const userIndex = this.users.findIndex( (user) => user.id === id ); if (userIndex >= 0) { this.users.splice(userIndex, 1); } } }
```

En este archivo, actualizamos las operaciones CRUD para que coincidan con el contexto de nuestra aplicación de gestión de usuarios. Ten en cuenta que estamos utilizando los DTOs `CreateUserInput` y `UpdateUserInput`. Al realizar nuestra operación de escritura, es posible que recibas algunos errores en el código a través de TypeScript.

> **Ejercicio práctico**  
> ⚒ Actualiza los archivos `dtos/*` para que se eliminen todos los errores de TypeScript.

Dado que todos los parámetros en el archivo de servicio eran los mismos, no necesitamos tocar nuestro archivo de resolver por ahora; seguirán funcionando como se espera.

Probemos esto ejecutando el servidor nuevamente.

Después de agregar estas nuevas entidades, mutaciones y consultas, notarás que el archivo `schema.gql` ha evolucionado. Aquí está su nuevo contenido:

```graphql
# ------------------------------------------------------ # THIS FILE WAS AUTOMATICALLY GENERATED (DO NOT MODIFY) # ------------------------------------------------------ type User { """the unique id of a user""" id: Int! """the name of a user""" name: String! """the email of a user""" email: String! } type Query { getHello: String! users: [User!]! user(id: Int!): User! } type Mutation { createUser(createUserInput: CreateUserInput!): User! updateUser(updateUserInput: UpdateUserInput!): User! removeUser(id: Int!): User! } input CreateUserInput { """ unique id for a user """ id: Int! """name of a user""" name: String! """ email of a user """ email: String! } input UpdateUserInput { id: Int! """ name of a user "" name: String """ email of a user """ email: String }
```

En un escenario ideal, este archivo nunca se tocará y continuará evolucionando a medida que agreguemos más módulos.

¡Felicitaciones: has construido una API GraphQL utilizando el enfoque code-first! Ahora, hagamos lo mismo utilizando el enfoque schema-first para que podamos ver las diferencias.

#### Enfoque Schema-first

En esta sección, exploraremos el enfoque schema-first para que podamos construir nuestra aplicación de gestión de usuarios en GraphQL con NestJS. Este enfoque implica definir el esquema de GraphQL por adelantado y luego implementar los resolvers correspondientes. Es particularmente útil cuando la estructura de la API está predeterminada o cuando se trabaja en colaboración en el diseño de la API.

#### Configuración para el enfoque Schema-first

Configuremos nuestro proyecto para el enfoque schema-first siguiendo estos sencillos pasos:

1. Para diferenciarte del enfoque code-first, crea una nueva rama —por ejemplo, `schema-first`— en tu proyecto.
2. Comienza creando un archivo `.graphql` para que puedas definir tu esquema. Para nuestra aplicación de gestión de usuarios, definiremos el esquema en `src/user.graphql`:

```graphql
# src/user.graphql type User { id: ID! name: String! email: String! } type Query { users: [User!]! user(id: ID!): User } type Mutation { createUser( name: String!, email: String! ): User! updateUser( id: ID!, name: String, email: String ): User deleteUser( id: ID! ): Boolean }
```

Este esquema describe la estructura del tipo `User`, junto con las consultas y mutaciones que admitiremos.

3. A continuación, asegúrate de que se hayan instalado los paquetes GraphQL requeridos:

```bash
$ npm install @nestjs/graphql graphql-tools graphql apollo-server-express
```

4. Ahora, configura `GraphQLModule` en `app.module.ts` para que use tu archivo de esquema:

```typescript
// app/module.ts import { GraphQLModule } from '@nestjs/graphql'; @Module({ imports: [ GraphQLModule.forRoot({ typePaths: ['./**/*.graphql'], }), ], // other configurations }) export class AppModule {}
```

En el fragmento de código anterior, `typePaths` apunta a la ubicación de tus archivos `.graphql`.

#### Construcción del módulo de usuarios

Sigue estos pasos para construir el módulo de usuarios:

1. Usa la CLI de Nest para generar el módulo de usuarios y su archivo de servicio:

```bash
$ nest generate module users $ nest generate service users
```

2. En esta etapa, necesitamos crear algunos resolvers:
   - En el módulo de usuarios, crea un archivo de resolver (`users.resolver.ts`) para que puedas implementar la lógica para las consultas y mutaciones definidas en el esquema.
   - En el archivo de resolver recién creado, pega el siguiente código:

```typescript
// users/users.resolver.ts import { Query, Resolver, Args, Mutation } from '@nestjs/graphql'; import { UsersService } from './users.service'; import { User } from './entities/user.entity'; @Resolver('User') export class UsersResolver { constructor( private readonly usersService: UsersService ) {} @Query('users') async getUsers(): Promise<User[]> { return this.usersService.findAll(); } @Mutation('createUser') async addUser( @Args('name') name: string, @Args('email') email: string ): Promise<User> { return this.usersService.create({ name, email }); } // Additional resolvers for updateUser and // deleteUser }
```

Aquí, implementamos la clase `UsersResolver` usando el decorador `@Resolver()`. En el archivo de resolver, tenemos una consulta y una mutación. Estas son muy básicas ya que son para fines de demostración y consisten en las operaciones `getUsers` y `addUser`.

Ten en cuenta que `UsersService` contendrá la lógica de negocio para manejar los datos de los usuarios, similar a la API RESTful que construimos anteriormente. Puedes simplemente reutilizar el que construimos siguiendo el enfoque code-first. Lo mismo se aplica a los archivos DTO y de entidad.

3. En el arreglo `providers` de `UsersModule`, agreguemos la clase `UsersResolver` para que podamos consultar la lógica dentro del resolver:

```typescript
@Module({ providers: [UsersService, UsersResolver], }) export class UsersModule {}
```

4. Ahora, debes probar tu API GraphQL siguiendo el enfoque schema-first:
   - Ejecuta tu aplicación y usa un GraphQL playground para probar tus consultas y mutaciones:

```bash
$ npm run start:dev
```

Asegúrate de que la API se comporte como se espera, creando, recuperando, actualizando y eliminando datos de usuarios.

Al seguir el enfoque schema-first, has experimentado una metodología diferente para construir una API GraphQL en NestJS. Este enfoque enfatiza el diseño inicial y puede ser particularmente beneficioso en escenarios donde el contrato de la API debe acordarse antes de implementarlo. Tanto el enfoque code-first como el schema-first tienen sus ventajas, y cómo elijas uno sobre el otro depende de los requisitos de tu proyecto y de las preferencias personales o del equipo.

En la siguiente sección, aprenderemos sobre las suscripciones de GraphQL y cómo NestJS admite esta función, lo que permite a los desarrolladores crear aplicaciones en tiempo real. ¡No perdamos tiempo y saltemos a ello!

---

### Aprendiendo sobre las suscripciones de GraphQL

Bienvenido al intrigante mundo de las suscripciones de GraphQL en NestJS, donde elevaremos nuestras aplicaciones a la interactividad en tiempo real. Esta sección está diseñada para desentrañar las complejidades de las suscripciones de GraphQL y ayudarnos a integrarlas a la perfección en nuestro sistema de gestión de usuarios basado en NestJS. A medida que avancemos, descubrirás cómo estas suscripciones mejoran la capacidad de respuesta y la experiencia del usuario de la aplicación, haciéndola más dinámica y atractiva.

Las suscripciones en GraphQL cambian las reglas del juego en el panorama de las APIs, ofreciendo una nueva dimensión a la interacción de datos. A diferencia de los modelos tradicionales de petición-respuesta, las suscripciones mantienen una conexión persistente, lo que permite que tu servidor envíe actualizaciones a los clientes a medida que ocurren los eventos. Esta capacidad es crucial para funciones como notificaciones en vivo, fuentes de datos en tiempo real y entornos colaborativos, donde las actualizaciones inmediatas son esenciales.

Embarquémonos en este viaje y exploremos cómo NestJS facilita la implementación de suscripciones de GraphQL, permitiéndonos construir aplicaciones web más interactivas y eficientes. Al integrar actualizaciones en tiempo real, nuestro objetivo es agregar una capa de inmediatez y participación al sistema de gestión de usuarios, enriqueciendo la experiencia general del usuario.

#### Implementación y prueba de suscripciones en NestJS

La integración de suscripciones de GraphQL en nuestra aplicación de gestión de usuarios existente aporta una emocionante capa de funcionalidad en tiempo real. Utilizando el enfoque code-first en NestJS, mejoraremos nuestra aplicación para que notifique a los clientes instantáneamente sobre cambios, como nuevos registros de usuarios o actualizaciones de usuarios existentes. Profundicemos en los pasos prácticos para lograr esto, asegurándonos de mantener el estilo atractivo e informativo que hemos establecido.

Para introducir suscripciones, necesitamos modificar nuestros tipos de GraphQL. En el enfoque code-first, esto significa actualizar nuestras clases de TypeScript para que definan nuevos campos de suscripción.

#### Definición de tipos de suscripción

En tu archivo de tipo GraphQL de usuario existente (`user.entity.ts`), agrega los campos de suscripción necesarios. Estos campos representan los eventos a los que los clientes pueden suscribirse:

```typescript
// user.entity.ts import { ObjectType, Field, ID } from '@nestjs/graphql'; @ObjectType() export class UserType { @Field(() => ID) id: string; @Field() username: string; @Field() email: string; } @ObjectType() export class UserSubscriptionType { @Field(() => UserType) userAdded: UserType; @Field(() => UserType) userUpdated: UserType; }
```

Aquí, `UserSubscriptionType` incluye dos campos: `userAdded` y `userUpdated`, los cuales utilizaremos en nuestros resolvers de suscripción.

#### Implementación de los resolvers de suscripción

En este punto, necesitamos resolvers de suscripción en nuestro archivo de resolver. Para garantizar que todo lo que ya tenemos permanezca igual, he aquí cómo podemos agregar una función de Suscripción a nuestro archivo `users.resolver.ts`:

En `UsersResolver`, implementa métodos para manejar suscripciones. NestJS nos permite usar el decorador `@Subscription()` para este propósito:

```typescript
// users.resolver.ts import { PubSub } from 'graphql-subscriptions'; const pubSub = new PubSub(); @Resolver(() => User) export class UsersResolver { // ... the rest of the code // Subscriptions @Subscription(() => User, { name: 'userCreated', }) userCreated() { return pubSub.asyncIterator('userCreated'); } @Subscription(() => User, { name: 'userUpdated', }) userUpdated() { return pubSub.asyncIterator('userUpdated'); } }
```

Estamos utilizando la clase `PubSub` del paquete `graphql-subscriptions` para crear una nueva instancia de `pubSub` que nos ayude a escuchar eventos como `userCreated` o `userUpdated`.

#### Publicación de eventos

Con los escuchadores en su lugar, ahora necesitamos publicarlos en algunos puntos de nuestros resolvers. Para publicar eventos, podemos usar el método `publish` de la instancia de `pubSub` justo después de la creación y actualización del usuario.

Para hacerlo, modifiquemos los métodos `createUser` y `updateUser` en la clase resolver con las siguientes líneas:

```typescript
// users.resolver.ts @Mutation(() => User) updateUser(@Args('updateUserInput') updateUserInput: UpdateUserInput { const updatedUser = this.usersService.update( updateUserInput.id, updateUserInput, ); pubSub.publish('userUpdated', { userUpdated: updatedUser }); return this.usersService.update(updateUserInput.id, updateUserInput); } @Mutation(() => User) createUser(@Args('createUserInput') createUserInput: CreateUserInput) { const newUser = this.usersService.create(createUserInput); pubSub.publish('userCreated', { userCreated: newUser }); return newUser; }
```

En el código anterior, actualizamos los métodos `createUser` y `updateUser` para que también podamos publicar eventos. Ten en cuenta que el nombre colocado como primer parámetro del método `publish()` es el mismo que usamos en los escuchadores; de lo contrario, el evento no se manejará adecuadamente.

Ten en cuenta que el archivo `schema.gql` generado automáticamente ahora contiene un nuevo tipo `Subscription` con las suscripciones recién creadas.

#### Probando suscripciones en acción

Para probar las suscripciones, usa un cliente de GraphQL como Apollo Client o una herramienta como GraphQL Playground. Luego, suscríbete a `userAdded` y `userUpdated` para ver actualizaciones en tiempo real cuando se agrega o actualiza un usuario en el sistema:

```graphql
subscription { userAdded { id username email } userUpdated { id username email } }
```

Finalmente, ejecuta la suscripción y realiza acciones de adición o actualización de usuarios. Deberías ver la respuesta de la suscripción actualizarse en tiempo real.

Finalmente, en el playground, crea un nuevo usuario utilizando el payload y asegúrate de que puedas ver el evento escuchado cada vez que creas o actualizas un usuario, como se muestra en la Figura 6.2 a continuación:

*Figura 6.2: Suscripción GraphQL en acción*

Ten en cuenta que la llamada de suscripción envía la información sobre la marca de tiempo y contiene un campo `data` con el tipo de objeto correspondiente. Si ocurre algún error, obtendrás un dato `null` con un campo de error que explica lo sucedido.

Al integrar suscripciones de GraphQL, nuestro sistema de gestión de usuarios adquiere una impresionante función de comunicación en tiempo real, mejorando la experiencia del usuario y la interactividad. Esta implementación muestra la flexibilidad y el poder de NestJS con GraphQL, lo que nos permite crear aplicaciones web modernas y sofisticadas. A medida que experimentes con estas funciones, apreciarás la profundidad y las posibilidades que las suscripciones de GraphQL aportan a tus proyectos.

En la siguiente sección, hablaremos sobre cómo puedes proteger tu API GraphQL para que puedas evitar construir zombis para tus clientes.

---

### Asegurando GraphQL: autenticación y autorización

Proteger una API GraphQL, especialmente en un entorno NestJS, requiere un enfoque matizado en comparación con las APIs REST tradicionales. Si bien los principios fundamentales de autenticación y autorización siguen siendo los mismos, los detalles de implementación pueden diferir debido a la naturaleza del endpoint único de GraphQL y sus complejas estructuras de consulta. Profundicemos en las complejidades de proteger una API GraphQL, destacando las diferencias con las APIs REST y explorando mecanismos para manejar la autenticación y autorización de manera efectiva.

#### Comprendiendo las diferencias

Ahora estamos familiarizados con los mecanismos de autenticación y autorización disponibles para las APIs REST. Sin embargo, construir APIs GraphQL seguras puede ser un poco diferente. Estas son algunas de las diferencias clave a las que debes prestar atención:

- **Desafío del endpoint único:** A diferencia de las APIs REST, que normalmente tienen múltiples endpoints correspondientes a diferentes recursos, una API GraphQL generalmente expone un único endpoint. Esto significa que la lógica de autenticación y autorización no puede basarse en URLs o métodos HTTP y debe ser más granular y consciente del contexto.
- **Estructuras de consulta complejas:** Las consultas GraphQL pueden ser anidadas y multifacéticas, obteniendo datos de múltiples recursos en una sola petición. Por lo tanto, proteger una API GraphQL requiere un enfoque más dinámico de autenticación y autorización que considere los campos de datos y tipos específicos solicitados.

#### Autenticación en GraphQL

La autenticación funciona casi igual que con las APIs REST. Estos son algunos mecanismos de autenticación conocidos y populares por los que puedes optar:

- **Autenticación basada en tokens:**
  - Similar a REST, la autenticación basada en tokens (como JWT) es un enfoque común en GraphQL. Los clientes envían un token de autenticación, generalmente en el encabezado HTTP, con cada petición.
  - En NestJS, puedes usar middleware o guards para interceptar peticiones entrantes y validar tokens de autenticación.
- **Autenticación contextual:**
  - NestJS te permite inyectar el contexto del usuario en tus resolvers de GraphQL. Este contexto incluye la información del usuario autenticado, que puedes utilizar para implementar un control de acceso detallado.
  - Al configurar el módulo GraphQL, asegúrate de que el contexto incluya los detalles de autenticación del usuario:

```typescript
// app.module.ts GraphQLModule.forRoot({ context: ({ req }) => ({ user: req.user }), // other configurations... });
```

#### Autorización en GraphQL

Estos son algunos de los mecanismos de autorización populares que quizás desees utilizar en tus futuras aplicaciones:

- **Autorización a nivel de campo (*Field-level authorization*):**
  - Dada la naturaleza de las consultas GraphQL, la autorización puede necesitar aplicarse a nivel de campo, no solo a nivel de operación.
  - Usa decoradores personalizados o enfoques basados en directivas en NestJS para proteger campos individuales según los roles o permisos de los usuarios.
- **Autorización a nivel de resolver (*Resolver-level authorization*):**
  - Para operaciones más sencillas, como mutaciones, puedes implementar la lógica de autorización a nivel de resolver utilizando guards.
  - Los guards de NestJS pueden inspeccionar el contexto del usuario y decidir si una petición debe procesarse o denegarse, de manera similar a los controladores de API REST.
- **Manejo de consultas complejas:**
  - Las consultas complejas que involucran múltiples recursos plantean un desafío único. Implementa un mecanismo que analice la estructura de la consulta y aplique la lógica de autorización en consecuencia.
  - Considera el uso de herramientas como GraphQL Shield, que proporciona una capa de middleware para tu API GraphQL y te permite establecer reglas de permisos según la estructura de la consulta.

Todas las mejores prácticas que aprendimos en el [Capítulo 5](https://subscription.packtpub.com/book/web-development/9781835468609/5) siguen siendo válidas aquí, incluida la limitación de velocidad y la validación.

Proteger una API GraphQL en NestJS requiere un enfoque integral y flexible que se adapte a las características únicas de GraphQL. Al centrarse en la autenticación contextual, la autorización granular y la seguridad a nivel de campo, puedes asegurarte de que tu API GraphQL no solo sea potente y eficiente, sino también segura y resistente. A medida que GraphQL continúa evolucionando, mantenerse actualizado sobre las mejores prácticas y las estrategias de seguridad emergentes será crucial para mantener aplicaciones sólidas y seguras.

En la siguiente sección, presentaremos un concepto muy importante conocido como Apollo Federation. Usaremos esto cuando comencemos a construir casos de uso reales relacionados con microservicios GraphQL más adelante en este libro.

---

### Introducción a Apollo Federation

Apollo Federation está diseñado para construir una API GraphQL única y coherente a partir de múltiples servicios, un concepto que es especialmente beneficioso en una arquitectura de microservicios. Su implementación en NestJS aprovecha el poder tanto de GraphQL como de Apollo para crear una API GraphQL robusta, escalable y eficiente.

#### Federation en la arquitectura de microservicios

A medida que nos adentramos en el reino de GraphQL y sus implementaciones, un concepto innovador se destaca por su capacidad de redefinir cómo construimos y escalamos nuestras APIs GraphQL: Apollo Federation. En esta introducción, exploraremos los principios fundamentales de Apollo Federation, comprenderemos su propósito y cómo revoluciona la forma en que abordamos GraphQL en una arquitectura de microservicios.

#### ¿Qué es Apollo Federation?

Apollo Federation es una arquitectura para construir un grafo distribuido a través de múltiples servicios. Desarrollado por Apollo, líder en tecnología GraphQL, Federation te permite componer múltiples servicios GraphQL separados en un solo grafo de datos. Este enfoque proporciona varias ventajas sobre el servidor GraphQL monolítico tradicional.

#### Los principios básicos de Apollo Federation

Estos son los principios fundamentales de Apollo Federation:

- **Descomposición de APIs monolíticas:** Federation te permite dividir una API GraphQL monolítica en microservicios más pequeños y manejables. Cada microservicio puede definir su propio esquema GraphQL, centrándose en un dominio o funcionalidad específica.
- **Grafo de datos único:** A pesar del proceso de descomposición, Apollo Federation permite que estos esquemas separados se combinen en una sola API GraphQL unificada. Desde la perspectiva del cliente, interactúa con un único endpoint de GraphQL, independientemente de cuántos servicios contribuyan al grafo.
- **Facilidad de extensión:** Agregar nuevas funciones o servicios a una arquitectura federada no requiere realizar cambios en los servicios existentes. Los nuevos servicios se pueden desarrollar y desplegar de forma independiente, lo que mejora la escalabilidad y flexibilidad de la aplicación.

#### El propósito de Apollo Federation

El objetivo principal de Apollo Federation es abordar los desafíos asociados con la construcción de una API GraphQL compleja y escalable en una arquitectura de microservicios:

- **Gestión de la complejidad:** A medida que crecen las APIs GraphQL, administrarlas como una base de código monolítica única puede volverse difícil de manejar. Federation permite un enfoque más manejable al dividir la API en microservicios más pequeños orientados al dominio.
- **Escalabilidad:** Federation promueve la escalabilidad. Los equipos pueden trabajar en diferentes servicios de forma independiente, desplegar actualizaciones sin afectar a todo el grafo y escalar los servicios individualmente según la demanda.
- **Rendimiento mejorado:** Al permitir a los clientes obtener datos de un único grafo unificado, Federation optimiza la cantidad de peticiones y la recuperación excesiva de datos, lo cual es un problema común en arquitecturas basadas en microservicios.

#### Cómo funciona Apollo Federation

Apollo Federation se basa en un conjunto de especificaciones y herramientas:

- **Esquemas federados:** Los servicios definen su parte del grafo utilizando el Lenguaje de Definición de Esquemas (SDL) estándar de GraphQL, junto con directivas específicas de Federation como `@key`, `@extends` y `@external`.
- **Apollo Gateway:** Este es un servicio especial que actúa como punto de entrada para los clientes. Compone los esquemas individuales de los servicios federados en un solo esquema y enruta las consultas a los servicios correspondientes.
- **Servicios de subgrafo (*Subgraph services*):** Estos son los servicios individuales que componen el grafo federado. Cada servicio de subgrafo es responsable de una parte específica del grafo de datos y se puede desarrollar y desplegar de forma independiente.

Apollo Federation es un enfoque transformador para construir APIs GraphQL en una arquitectura de microservicios. Proporciona una solución elegante a las complejidades y desafíos de escalar y mantener implementaciones de GraphQL a gran escala. A medida que avancemos en los siguientes capítulos, profundizaremos en las complejidades de Apollo Federation, explorando cómo construir esquemas federados y administrar dependencias en un entorno GraphQL distribuido. Este viaje te equipará con el conocimiento y las habilidades necesarias para aprovechar todo el potencial de Apollo Federation en tus proyectos de GraphQL.

---

### Resumen

Al concluir el Capítulo 6, reflexionamos sobre un viaje que nos ha llevado a lo más profundo del mundo de GraphQL, revelando sus capacidades, complejidades y cómo se integra a la perfección con NestJS para crear APIs sofisticadas.

Comenzamos desmitificando GraphQL, comprendiendo sus principios fundamentales y cómo ofrece un enfoque más eficiente y flexible para la recuperación de datos en comparación con las APIs REST tradicionales. Al enfatizar su estructura de endpoint único y sus capacidades de consulta complejas, exploramos cómo GraphQL revoluciona la obtención y manipulación de datos.

Nuestro viaje práctico comenzó con la construcción y optimización de una API GraphQL en NestJS, adoptando tanto el enfoque code-first como el schema-first. Mostramos cómo definir esquemas GraphQL, configurar resolvers e implementar operaciones CRUD, destacando la versatilidad y la solidez de NestJS en el desarrollo de APIs GraphQL.

Al profundizar en las suscripciones de GraphQL, agregamos funcionalidades de datos en tiempo real a nuestro sistema de gestión de usuarios. Esta sección aclaró cómo funcionan las suscripciones bajo el capó, utilizando WebSockets para mantener una conexión persistente y enviar actualizaciones desde el servidor al cliente, mejorando la interactividad y la capacidad de respuesta de las aplicaciones.

Proteger nuestra API GraphQL fue nuestro siguiente enfoque. Aquí, abordamos los desafíos únicos que plantean el endpoint único y las consultas complejas de GraphQL, analizando estrategias para implementar mecanismos robustos de autenticación y autorización. Esto aseguró que nuestra API no solo fuera potente sino también segura y resistente.

La introducción a Apollo Federation marcó un punto fundamental en nuestra exploración. En esta sección, aprendimos cómo Apollo Federation permite que las APIs GraphQL monolíticas se descompongan en una arquitectura federada, lo que permite la escalabilidad y la flexibilidad. Cubrimos cómo configurar servicios federados y Apollo Gateway en NestJS, ilustrando cómo múltiples servicios pueden contribuir a un esquema GraphQL unificado.

A lo largo de este capítulo, enfatizamos las mejores prácticas, consideraciones de rendimiento y medidas de seguridad que son esenciales para construir APIs GraphQL eficientes y seguras. Desde la optimización de las conexiones WebSockets hasta el manejo de grandes volúmenes de datos en tiempo real y la garantía de suscripciones seguras, cubrimos un amplio espectro de consideraciones que son cruciales para cualquier desarrollador de GraphQL.

En los siguientes dos capítulos, aprenderemos sobre las pruebas en general y su propósito en el desarrollo de aplicaciones modernas. Así que prepárate: nos aseguraremos de que nuestras APIs sean escalables y resistentes probándolas.
