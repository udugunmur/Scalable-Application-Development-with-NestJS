# Parte 5: Ejemplos de Aplicaciones del Mundo Real y Estudios de Caso

## Capítulo 13: Estudio de Caso 2 – Plataforma de Red Social

¡Bienvenido a nuestro segundo caso de uso, donde construiremos una aplicación GraphQL de la vida real! Al igual que hicimos en el capítulo anterior, en este recorreremos todos los procesos de diseño e implementación, prácticamente tal como lo haríamos en el desarrollo de aplicaciones en el mundo real.

En este capítulo, aplicaremos nuestra comprensión básica de cómo funciona GraphQL con NestJS para construir una plataforma de red social (*Social Networking Platform* o SNP). Debido a la estructura de datos compleja y en constante evolución de las SNP, GraphQL a menudo se considera la mejor opción para construirlas, evitando la recuperación excesiva (*over-fetching*) o insuficiente (*under-fetching*), que son problemas comunes asociados con las APIs REST.

Utilizaremos bases de datos NoSQL debido al alto volumen de datos diarios en una SNP, las operaciones intensivas de lectura y escritura, y la necesidad de alta disponibilidad y escalabilidad. Las bases de datos NoSQL se adaptan perfectamente a estos desafíos. En nuestro segundo estudio de caso, utilizaremos MongoDB para construir una API escalable para una SNP.

Démosle un nombre a nuestra SNP. ¿Qué tal **NectMe**? ¿Suena bien? No deberíamos preocuparnos por el nombre; lo más importante ahora es tener listo el producto mínimo viable (*Minimum Viable Product* o MVP) para nuestros usuarios finales.

Estos son los temas que cubriremos en este capítulo:

- Comprensión de los requisitos de una plataforma de red social
- Diseño de la estructura de la aplicación para redes sociales
- Implementación de APIs GraphQL para usuarios, publicaciones e interacciones
- Gestión de relaciones de usuario y características sociales
- Publicación de actualizaciones y notificaciones en tiempo real en un contexto de redes sociales

¿Listo para desafiarte a ti mismo? ¡Comencemos!

---

### Requisitos técnicos

Para este capítulo, asegúrate de tener tu entorno local listo siguiendo los pasos del [Capítulo 3](https://subscription.packtpub.com/book/web-development/9781835468609/3). Todo el código fuente utilizado para construir el proyecto se puede encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch13](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch13); asegúrate de revisarlo de vez en cuando para compararlo con tu propia implementación.

---

### Comprensión de los requisitos de una plataforma de red social

Antes de sumergirse en el código, siempre es importante comprender los requisitos de lo que estamos dispuestos a construir y entender nuestro objetivo (*target*) y nuestro alcance (*scope*), para no construir menos ni más de lo que se debe. En esta sección, identifiquemos las necesidades desde la perspectiva del usuario (objetivo) y los requisitos adicionales (alcance).

#### Necesidades de los usuarios

Sin intentar clonar Facebook o Twitter (X), necesitamos entender cómo debería funcionar una SNP básica. Como usuario de la plataforma, quiero poder hacer lo siguiente:

- Iniciar sesión de forma segura en el sistema.
- Navegar a través de las diferentes páginas y secciones de la aplicación fácilmente.
- Escribir y publicar contenido (publicaciones o *posts*); para el MVP, solo permitiremos publicaciones de texto.
- Ver las publicaciones de otras personas según mis intereses y poder reaccionar a esas publicaciones mediante comentarios y *likes*, o guardarlas para leerlas más tarde.
- Actualizar mis preferencias en cualquier momento que desee.

Por ahora, este conjunto de requisitos guiará nuestra toma de decisiones. Dado que estamos utilizando GraphQL y una base de datos NoSQL, cambiar los requisitos y la implementación en el futuro no será un problema. Deberíamos poder hacerlo fácilmente a medida que la aplicación crezca. Por ejemplo, agregar soporte para publicar otros formatos multimedia como imágenes, videos, etcétera, no debería ser demasiado difícil, ya que el esquema de la base de datos puede evolucionar repetidamente.

#### Requisitos funcionales

Estas son algunas de las funciones principales que nuestra aplicación debe admitir:

- **Gestión de cuentas de usuario:** El flujo de autenticación, desde el registro hasta la autorización para realizar ciertas acciones; por ejemplo, solo los usuarios con una cuenta en nuestro sistema que hayan iniciado sesión deberían poder comentar o publicar contenido en la plataforma, mientras que el resto puede tener acceso de solo lectura al contenido menos importante.
- **Gestión de publicaciones (*posts*):** La aplicación debe admitir ciertas operaciones relacionadas con las publicaciones, como la capacidad de publicar (crear), editar (actualizar) o eliminar una publicación. La aplicación debe ser lo suficientemente resistente como para gestionar el efecto en cascada al eliminar una publicación que tenga asociada una cierta cantidad de comentarios y reacciones. Un mecanismo de eliminación suave (*soft deletion*) es imprescindible.
- **Gestión de comentarios:** De manera similar, un comentario es un tipo de publicación y debe heredar todas las operaciones de su padre (*post*).
- **Gestión de reacciones:** Cualquier publicación (o comentario) se puede vincular a una determinada cantidad de reacciones. Para un MVP, una reacción puede ser un *like* o un *upvote*.
- **Gestión de preferencias:** Cada usuario debe poder suscribirse deliberadamente a ciertos tipos de contenido que desea ver en su hilo principal. Debemos tener operaciones que permitan a los usuarios actualizar sus preferencias en cualquier momento.
- **Gestión de notificaciones:** Según las preferencias del usuario, deberíamos poder enviar notificaciones personalizadas a los usuarios cada vez que ocurra un evento determinado en su área de interés, como un "like" en su publicación, una nueva publicación de un tema al que esté suscrito, etcétera.

Basándonos en los requisitos anteriores, deberíamos poder definir el alcance actual y mejorarlo en cualquier momento en el futuro, aprovechando los beneficios de escalabilidad de las APIs GraphQL y las bases de datos NoSQL.

#### Requisitos no funcionales

Además de los requisitos funcionales enumerados anteriormente, nuestra aplicación debe ser lo suficientemente resistente como para satisfacer los siguientes requisitos no funcionales:

- **Rendimiento:** Nuestra aplicación debe ser lo suficientemente eficiente como para atender las necesidades de los clientes.
- **Seguridad:** Debe ser segura para nuestros clientes y debe evitar exponer información confidencial como publicaciones privadas, contraseñas de usuario, etcétera. Los mecanismos como la autenticación y la autorización deben ser robustos.
- **Disponibilidad del sistema:** Si bien este aspecto se tratará en profundidad en las partes de DevOps, debemos tener en cuenta este requisito para permitir que el sistema pueda hacer frente al creciente número de solicitudes que recibirá nuestra aplicación en el futuro.
- **Documentación:** Dado que estamos trabajando en el lado del servidor, debemos brindar a nuestros clientes una documentación bien redactada que les permita experimentar con las operaciones disponibles.

Aunque esta es simplemente una lista no exhaustiva de requisitos, las partes interesadas (clientes, ingenieros y gerentes de producto) deben seguir colaborando en su revisión, permitiendo que todos avancen al mismo ritmo. Para nuestro ejemplo, nos centraremos en los requisitos no funcionales que acabamos de mostrar.

Con los requisitos definidos, podemos entregar el trabajo a los arquitectos para que planifiquen por nosotros… Acabo de recordar… ¡nosotros somos los arquitectos! :) Trabajemos en el diseño de nuestra aplicación en la siguiente sección.

---

### Diseño de la estructura de la aplicación para redes sociales

En el capítulo anterior, esta fase consistió en decidir qué operaciones HTTP necesitábamos incluir en la aplicación y cómo debía reaccionar el servidor ante las solicitudes relacionadas. En el contexto de una aplicación GraphQL, la mayor parte del tiempo exponemos una única operación HTTP, `POST`, en el endpoint `/graphql`. Esto simplifica nuestro trabajo en esta sección; sin embargo, debemos centrarnos en cómo deben lucir los datos en nuestro sistema, abriendo las puertas para futuras mejoras: el modelo de objetos, las relaciones entre objetos, el diseño del esquema y el diseño de la API.

#### Modelo de objetos

En las aplicaciones GraphQL, el modelo de objetos juega un papel fundamental en la definición de cómo se estructuran y se accede a los datos. Este modelo define los tipos de datos en nuestro sistema y cómo estos tipos se interrelacionan. En función de los requisitos descritos anteriormente, podemos diseñar un modelo de objetos para nuestra SNP, NectMe. A continuación, se presenta una descripción general de los objetos clave y sus relaciones.

##### User

El objeto `User` representa a un individuo que utiliza la plataforma. Cada usuario tendrá atributos tales como los siguientes:

- `id`: Identificador único para el usuario.
- `username`: El nombre elegido por el usuario.
- `email`: La dirección de correo electrónico del usuario, utilizada para iniciar sesión y recibir notificaciones.
- `password`: Contraseña hasheada para la autenticación.
- `bio`: Una breve biografía proporcionada por el usuario.
- `preferences`: Preferencias de contenido del usuario.
- `createdAt`: Marca de tiempo de cuándo se creó la cuenta de usuario.
- `updatedAt`: Marca de tiempo de la última actualización de la cuenta de usuario.

##### Post

El objeto `Post` representa un fragmento de contenido creado por un usuario. Los atributos incluyen los siguientes:

- `id`: Identificador único para la publicación.
- `author`: El usuario que creó la publicación.
- `content`: El contenido de texto de la publicación.
- `createdAt`: Marca de tiempo de cuándo se creó la publicación.
- `updatedAt`: Marca de tiempo de la última actualización de la publicación.
- `comments`: Lista de objetos `Comment` relacionados con la publicación.
- `reactions`: Lista de objetos `Reaction` relacionados con la publicación.
- `isDeleted`: Bandera booleana para el borrado suave (*soft deletion*).

##### Comment

El objeto `Comment` es un tipo de publicación que está asociada con otra publicación. Los atributos incluyen los siguientes:

- `id`: Identificador único para el comentario.
- `post`: La publicación a la que pertenece este comentario.
- `author`: El usuario que creó el comentario.
- `content`: El contenido de texto del comentario.
- `createdAt`: Marca de tiempo de cuándo se creó el comentario.
- `updatedAt`: Marca de tiempo de la última actualización del comentario.
- `reactions`: Lista de objetos `Reaction` relacionados con el comentario.
- `isDeleted`: Bandera booleana para el borrado suave (*soft deletion*).

##### Reaction

El objeto `Reaction` representa la reacción de un usuario a una publicación o comentario. Los atributos incluyen los siguientes:

- `id`: Identificador único para la reacción.
- `user`: El usuario que reaccionó.
- `post`: La publicación asociada con la reacción (opcional).
- `comment`: El comentario asociado con la reacción (opcional).
- `type`: El tipo de reacción (por ejemplo, *like* o *upvote*).
- `createdAt`: Marca de tiempo de cuándo se creó la reacción.

##### Preference

El objeto `Preference` representa las preferencias de contenido de un usuario. Los atributos incluyen los siguientes:

- `id`: Identificador único para la preferencia.
- `user`: El usuario asociado con la preferencia.
- `categories`: Lista de categorías o temas en los que el usuario está interesado.
- `createdAt`: Marca de tiempo de cuándo se creó la preferencia.
- `updatedAt`: Marca de tiempo de la última actualización de la preferencia.

##### Notification

El objeto `Notification` representa una notificación generada por el sistema para un usuario. Los atributos incluyen los siguientes:

- `id`: Identificador único para la notificación.
- `user`: El usuario al que se le envía la notificación.
- `content`: El contenido de texto de la notificación.
- `createdAt`: Marca de tiempo de cuándo se creó la notificación.
- `read`: Bandera booleana que indica si la notificación ha sido leída.

##### Relaciones entre objetos

A continuación, se presentan algunas de las relaciones más importantes a implementar:

- Un usuario puede crear múltiples publicaciones.
- Un usuario puede crear múltiples comentarios en publicaciones.
- Un usuario puede reaccionar a múltiples publicaciones y comentarios.
- Un usuario puede tener un conjunto de preferencias.
- Un usuario puede recibir múltiples notificaciones.
- Una publicación puede tener múltiples comentarios y reacciones.
- Un comentario puede tener múltiples reacciones.

Al definir estos objetos y sus relaciones, creamos una estructura de datos robusta que puede soportar eficientemente las funcionalidades principales de nuestra SNP. Esta estructura también brinda la flexibilidad necesaria para escalar y adaptarse a medida que surjan nuevas características y requisitos.

#### Diseño del esquema de base de datos

Con nuestro modelo de objetos definido, necesitamos mapear estos objetos a un esquema de base de datos. Utilizando MongoDB, nuestra base de datos NoSQL de elección, podemos diseñar colecciones para almacenar estos objetos. El diseño de esquema flexible de MongoDB nos permite almacenar datos relacionados juntos, lo que facilita las consultas y el escalado.

##### Colecciones

En el contexto de las bases de datos NoSQL, una colección es un conjunto de documentos con una forma y función similares. Este es el equivalente a las tablas en las bases de datos SQL. Para nuestra aplicación, estas son las colecciones con las que construiremos:

- **Colección `Users`:** Almacena información de los usuarios, incluidos los detalles de autenticación y preferencias.
  - Incrusta subdocumentos para preferencias.
  - Índices en `email` para consultas eficientes.
- **Colección `Posts`:** Almacena publicaciones creadas por los usuarios.
  - Incrusta subdocumentos para comentarios y reacciones.
  - Índices en `author` y `createdAt` para consultas eficientes.
- **Colección `Comments`:** Almacena comentarios asociados a publicaciones.
  - Referencias a la publicación principal y al autor.
  - Índices en `post`, `author` y `createdAt` para consultas eficientes.
- **Colección `Reactions`:** Almacena reacciones a publicaciones y comentarios.
  - Referencias al usuario, publicación o comentario.
  - Índices en `user`, `post` y `comment` para consultas eficientes.
- **Colección `Notifications`:** Almacena notificaciones para los usuarios.
  - Referencias al usuario.
  - Índices en `user` y `createdAt` para consultas eficientes.

##### Flexibilidad del esquema

Uno de los beneficios clave de usar MongoDB es la flexibilidad de su esquema. A medida que nuestra aplicación crece, podemos actualizar fácilmente el esquema para incluir nuevas características, como soporte para contenido multimedia en las publicaciones o tipos de reacción adicionales. Esta flexibilidad nos permite adaptarnos rápidamente a los requisitos cambiantes sin una reestructuración significativa.

#### Diseño de la API

Con nuestro modelo de objetos y esquema de base de datos listos, podemos diseñar la API GraphQL para interactuar con estos objetos. La API definirá *queries* y *mutations* para realizar operaciones CRUD en nuestros datos.

##### Queries

Las *queries* (consultas) son operaciones de solo lectura en el contexto de GraphQL, y estas son las más importantes:

- `getUser`: Recupera detalles del usuario por ID o nombre de usuario.
- `getPost`: Recupera detalles de la publicación por ID.
- `getComments`: Recupera comentarios para una publicación específica.
- `getReactions`: Recupera reacciones para una publicación o comentario específico.

##### Mutations

Las *mutations* (mutaciones) son operaciones de lectura y escritura que actualizan la base de datos; son como los *setters* que tuvimos en el capítulo anterior. Estas son las más importantes:

- `createUser`: Registra un nuevo usuario.
- `updateUser`: Actualiza detalles del usuario y preferencias.
- `createPost`: Crea una nueva publicación.
- `updatePost`: Actualiza el contenido de una publicación.
- `deletePost`: Realiza un borrado suave de una publicación.
- `createComment`: Añade un comentario a una publicación.
- `updateComment`: Actualiza el contenido de un comentario.
- `deleteComment`: Realiza un borrado suave de un comentario.
- `addReaction`: Añade una reacción a una publicación o comentario.
- `removeReaction`: Elimina una reacción de una publicación o comentario.
- `markNotificationRead`: Marca una notificación como leída.

Al definir estas consultas y mutaciones, creamos una API integral que permite a los clientes interactuar con el sistema de manera flexible y eficiente.

Con el modelo de objetos, el esquema de base de datos y el diseño de la API listos, estamos preparados para comenzar a implementar las APIs GraphQL para usuarios, publicaciones e interacciones en la siguiente sección. ¡Empecemos!

---

### Implementación de APIs GraphQL para usuarios, publicaciones e interacciones

La implementación de APIs GraphQL para usuarios, publicaciones e interacciones implica configurar un proyecto NestJS, definir esquemas y Objetos de Transferencia de Datos (*Data Transfer Objects* o DTOs), y crear *resolvers* para manejar las consultas y mutaciones de GraphQL. Este proceso garantiza que nuestra aplicación pueda gestionar usuarios, publicaciones e interacciones de manera eficiente.

En las próximas secciones, configuremos el proyecto y construyamos los módulos principales de nuestra aplicación.

#### Configuración del proyecto

Para comenzar, necesitamos configurar nuestro proyecto NestJS e instalar las dependencias necesarias para GraphQL y MongoDB. Hagámoslo siguiendo estos sencillos pasos:

1. Primero, inicializa un nuevo proyecto NestJS usando la CLI de Nest:

```bash
$ npm i -g @nestjs/cli
$ nest new nect-me
$ cd nect-me
```

Esto configura un nuevo proyecto NestJS llamado `nect-me`; recuerda, puedes elegir cualquier nombre que te suene bien.

2. A continuación, instala las dependencias requeridas para la integración de GraphQL y MongoDB:

```bash
$ npm install @nestjs/graphql graphql-tools graphql apollo-server-express mongoose @nestjs/mongoose
```

Estas dependencias incluyen el módulo GraphQL de NestJS ([https://docs.nestjs.com/graphql/quick-start](https://docs.nestjs.com/graphql/quick-start)), Apollo Server ([https://www.apollographql.com/docs/apollo-server](https://www.apollographql.com/docs/apollo-server)), y Mongoose para MongoDB ([https://mongoosejs.com/docs/](https://mongoosejs.com/docs/)).

3. En `src/app.module.ts`, configura los módulos GraphQL y Mongoose:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { MongooseModule } from '@nestjs/mongoose';
import { join } from 'path';

@Module({
  imports: [
    GraphQLModule.forRoot({
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
    }),
    MongooseModule.forRoot(
      'mongodb://localhost/nectme'
    ),
  ],
})
export class AppModule {}
```

Esta configuración establece Apollo Server para GraphQL y se conecta a una base de datos MongoDB.

Ahora, implementemos el módulo `Users`, que incluirá definiciones de esquema, DTOs y *resolvers*.

#### Creación del módulo Users

Comenzamos definiendo el esquema `User`, creando DTOs para la transferencia de datos y configurando *resolvers* para manejar las operaciones GraphQL:

1. En `src/users/schemas/user.schema.ts`, define el esquema `User` usando decoradores de Mongoose:

```typescript
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

@Schema()
export class User extends Document {
  @Prop({ required: true, unique: true })
  username: string;

  @Prop({ required: true, unique: true })
  email: string;

  @Prop({ required: true })
  password: string;

  @Prop()
  bio: string;

  @Prop({ type: [String] })
  preferences: string[];

  @Prop({ default: Date.now })
  createdAt: Date;

  @Prop({ default: Date.now })
  updatedAt: Date;
}

export const UserSchema = SchemaFactory.createForClass(User);
```

Este esquema define el modelo `User` con propiedades como `username`, `email`, `password`, `bio`, `preferences`, `createdAt` y `updatedAt`.

También utilizamos algunos decoradores específicos de Mongoose como `@Prop()` y `@Schema()`; estos son el equivalente de `@Column()` y `@Entity()` de TypeORM, los cuales exploramos en el capítulo anterior.

2. En `src/users/dto/create-user.dto.ts`, crea un DTO para crear un usuario:

```typescript
import { Field, InputType } from '@nestjs/graphql';

@InputType()
export class CreateUserDto {
  @Field()
  username: string;

  @Field()
  email: string;

  @Field()
  password: string;

  @Field({ nullable: true })
  bio?: string;
}
```

Este DTO define la estructura para los datos de entrada de creación de usuario, incluyendo el campo opcional `bio`. Estamos utilizando algunos decoradores específicos de GraphQL, los cuales exploramos en el [Capítulo 6](https://subscription.packtpub.com/book/web-development/9781835468609/6); no dudes en consultarlo si esto te resulta confuso.

3. A continuación, creamos un *resolver* para manejar las consultas y mutaciones de GraphQL relacionadas con los usuarios. En `src/users/users.resolver.ts`, agrega lo siguiente:

```typescript
import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
import { UsersService } from './users.service';
import { CreateUserDto } from './dto/create-user.dto';
import { User } from './schemas/user.schema';

@Resolver(of => User)
export class UsersResolver {
  constructor(
    private readonly usersService: UsersService
  ) {}

  @Query(returns => [User])
  async users() {
    return this.usersService.findAll();
  }

  @Mutation(returns => User)
  async createUser(
    @Args('createUserDto') createUserDto: CreateUserDto
  ) {
    return this.usersService.create(createUserDto);
  }
}
```

Este *resolver* maneja la consulta `users` para obtener todos los usuarios y la mutación `createUser` para crear un nuevo usuario.

4. En `src/users/users.service.ts`, implementa el servicio `User` para interactuar con la base de datos:

```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { User } from './schemas/user.schema';
import { CreateUserDto } from './dto/create-user.dto';

@Injectable()
export class UsersService {
  constructor(
    @InjectModel (User.name) private userModel: Model<User>
  ) {}

  async findAll(): Promise<User[]> {
    return this.userModel.find().exec();
  }

  async create(createUserDto: CreateUserDto): Promise<User> {
    const createdUser = new this.userModel(createUserDto);
    return createdUser.save();
  }
}
```

Este servicio proporciona métodos para encontrar todos los usuarios y crear un nuevo usuario en la base de datos MongoDB.

5. Finalmente, registra el módulo `User` en `src/app.module.ts`:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { MongooseModule } from '@nestjs/mongoose';
import { join } from 'path';
import { UsersModule } from './users/users.module';

@Module({
  imports: [
    GraphQLModule.forRoot({
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
    }),
    MongooseModule.forRoot(
      'mongodb://localhost/nectme'
    ),
    UsersModule,
  ],
})
export class AppModule {}
```

Ahora que el módulo `User` está configurado, pasemos a implementar los módulos `Post` e `Interaction`. Para reforzar tu comprensión, debemos seguir todos los pasos juntos y experimentar con el código a medida que profundizamos.

Ahora, continuemos con la implementación del módulo `Posts`.

#### Creación del módulo Posts

Ahora que hemos configurado el módulo `User`, pasemos a crear el módulo `Post`. Esto incluirá definir el esquema `Post`, crear DTOs para la transferencia de datos, configurar el *resolver* e implementar el servicio para interactuar con la base de datos:

1. En `src/posts/schemas/post.schema.ts`, define el esquema `Post` usando decoradores de Mongoose:

```typescript
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';
import { User } from '../../users/schemas/user.schema';

@Schema()
export class Post extends Document {
  @Prop({ type: String, required: true })
  content: string;

  @Prop({ type: Date, default: Date.now })
  createdAt: Date;

  @Prop({ type: Date, default: Date.now })
  updatedAt: Date;

  @Prop({ type: Boolean, default: false })
  isDeleted: boolean;

  @Prop({ type: String, ref: 'User', required: true })
  author: User;
}

export const PostSchema = SchemaFactory.createForClass(Post);
```

Este esquema define el modelo `Post` con propiedades como `content`, `createdAt`, `updatedAt`, `isDeleted` y `author`.

2. En `src/posts/dto/create-post.dto.ts`, crea un DTO para crear una publicación:

```typescript
import { Field, InputType } from '@nestjs/graphql';

@InputType()
export class CreatePostDto {
  @Field()
  content: string;

  @Field()
  authorId: string;
}
```

Este DTO define la estructura para los datos de entrada de creación de publicaciones, incluidos `content` y `authorId`.

3. A continuación, creamos un *resolver* para manejar las consultas y mutaciones de GraphQL relacionadas con las publicaciones. En `src/posts/posts.resolver.ts`, agrega lo siguiente:

```typescript
import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
import { PostsService } from './posts.service';
import { CreatePostDto } from './dto/create-post.dto';
import { Post } from './schemas/post.schema';

@Resolver(of => Post)
export class PostsResolver {
  constructor(
    private readonly postsService: PostsService
  ) {}

  @Query(returns => [Post])
  async posts() {
    return this.postsService.findAll();
  }

  @Mutation(returns => Post)
  async createPost(
    @Args('createPostDto') createPostDto: CreatePostDto
  ) {
    return this.postsService.create(createPostDto);
  }
}
```

Este *resolver* maneja la consulta `posts` para obtener todas las publicaciones y la mutación `createPost` para crear una nueva publicación.

4. En `src/posts/posts.service.ts`, implementa el servicio `Post` para interactuar con la base de datos:

```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Post } from './schemas/post.schema';
import { CreatePostDto } from './dto/create-post.dto';

@Injectable()
export class PostsService {
  constructor(
    @InjectModel(Post.name) private postModel: Model<Post>
  ) {}

  async findAll(): Promise<Post[]> {
    return this.postModel.find().exec();
  }

  async create(
    createPostDto: CreatePostDto
  ): Promise<Post> {
    const createdPost = new this.postModel(createPostDto);
    return createdPost.save();
  }
}
```

Este servicio proporciona métodos para buscar todas las publicaciones y crear una nueva publicación en la base de datos MongoDB.

5. Finalmente, registra el módulo `Post` en `src/app.module.ts`:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { MongooseModule } from '@nestjs/mongoose';
import { join } from 'path';
import { UsersModule } from './users/users.module';
import { PostsModule } from './posts/posts.module';

@Module({
  imports: [
    GraphQLModule.forRoot({
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
    }),
    MongooseModule.forRoot(
      'mongodb://localhost/nectme'
    ),
    UsersModule,
    PostsModule,
  ],
})
export class AppModule {}
```

Al completar estos pasos, hemos configurado un módulo `Post` básico en NestJS con GraphQL y MongoDB.

#### Creación del módulo Interaction

El módulo `Interaction` manejará las operaciones relacionadas con comentarios y reacciones. Definiremos los esquemas, crearemos DTOs y configuraremos *resolvers* y servicios para gestionar comentarios y reacciones:

1. En `src/comments/schemas/comment.schema.ts`, define el esquema `Comment` usando decoradores de Mongoose:

```typescript
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';
import { User } from '../../users/schemas/user.schema';
import { Post } from '../../posts/schemas/post.schema';

@Schema()
export class Comment extends Document {
  @Prop({ type: String, required: true })
  content: string;

  @Prop({ type: Date, default: Date.now })
  createdAt: Date;

  @Prop({ type: Date, default: Date.now })
  updatedAt: Date;

  @Prop({ type: Boolean, default: false })
  isDeleted: boolean;

  @Prop({ type: String, ref: 'User', required: true })
  author: User;

  @Prop({ type: String, ref: 'Post', required: true })
  post: Post;
}

export const CommentSchema = SchemaFactory.createForClass(Comment);
```

Este esquema define el modelo `Comment` con propiedades tales como `content`, `createdAt`, `updatedAt`, `isDeleted`, `author` y `post`.

2. En `src/comments/dto/create-comment.dto.ts`, crea un DTO para crear un comentario:

```typescript
import { Field, InputType } from '@nestjs/graphql';

@InputType()
export class CreateCommentDto {
  @Field()
  content: string;

  @Field()
  authorId: string;

  @Field()
  postId: string;
}
```

Este DTO define la estructura para los datos de entrada de creación de comentarios, incluidos `content`, `authorId` y `postId`.

3. A continuación, creamos un *resolver* para manejar las consultas y mutaciones de GraphQL relacionadas con los comentarios. En `src/comments/comments.resolver.ts`, agrega lo siguiente:

```typescript
import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
import { CommentsService } from './comments.service';
import { CreateCommentDto } from './dto/create-comment.dto';
import { Comment } from './schemas/comment.schema';

@Resolver(of => Comment)
export class CommentsResolver {
  constructor(
    private readonly commentsService: CommentsService
  ) {}

  @Query(returns => [Comment])
  async comments() {
    return this.commentsService.findAll();
  }

  @Mutation(returns => Comment)
  async createComment(
    @Args('createCommentDto') createCommentDto: CreateCommentDto
  ) {
    return this.commentsService.create(
      createCommentDto
    );
  }
}
```

Este *resolver* maneja la consulta `comments` para obtener todos los comentarios y la mutación `createComment` para crear un nuevo comentario.

4. En `src/comments/comments.service.ts`, implementa el servicio `Comment` para interactuar con la base de datos:

```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Comment } from './schemas/comment.schema';
import { CreateCommentDto } from './dto/create-comment.dto';

@Injectable()
export class CommentsService {
  constructor(
    @InjectModel(Comment.name) private commentModel: Model<Comment>
  ) {}

  async findAll(): Promise<Comment[]> {
    return this.commentModel.find().exec();
  }

  async create(createCommentDto: CreateCommentDto): Promise<Comment> {
    const createdComment = new this.commentModel(createCommentDto);
    return createdComment.save();
  }
}
```

Este servicio proporciona métodos para buscar todos los comentarios y crear un nuevo comentario en la base de datos MongoDB.

5. Finalmente, registra el módulo `Comments` en `src/app.module.ts`:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { MongooseModule } from '@nestjs/mongoose';
import { join } from 'path';
import { UsersModule } from './users/users.module';
import { PostsModule } from './posts/posts.module';
import { CommentsModule } from './comments/comments.module';

@Module({
  imports: [
    GraphQLModule.forRoot({
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
    }),
    MongooseModule.forRoot(
      'mongodb://localhost/nectme'
    ),
    UsersModule,
    PostsModule,
    CommentsModule,
  ],
})
export class AppModule {}
```

Al completar estos pasos, hemos configurado un módulo `Comments` básico en NestJS con GraphQL y MongoDB.

#### Creación del módulo Reactions

El módulo `Reactions` manejará las operaciones relacionadas con las reacciones (*likes* y *upvotes*). Definiremos el esquema, crearemos DTOs y configuraremos *resolvers* y servicios para gestionar las reacciones.

1. En `src/reactions/schemas/reaction.schema.ts`, define el esquema `Reaction` usando decoradores de Mongoose:

```typescript
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';
import { User } from '../../users/schemas/user.schema';
import { Post } from '../../posts/schemas/post.schema';

@Schema()
export class Reaction extends Document {
  @Prop({ type: String, required: true, enum: ['LIKE', 'UPVOTE'] })
  type: string;

  @Prop({ type: Date, default: Date.now })
  createdAt: Date;

  @Prop({ type: String, ref: 'User', required: true })
  user: User;

  @Prop({ type: String, ref: 'Post', required: true })
  post: Post;
}

export const ReactionSchema = SchemaFactory.createForClass(Reaction);
```

Este esquema define el modelo `Reaction` con propiedades como `type`, `createdAt`, `user` y `post`.

2. En `src/reactions/dto/create-reaction.dto.ts`, crea un DTO para crear una reacción:

```typescript
import { Field, InputType } from '@nestjs/graphql';

@InputType()
export class CreateReactionDto {
  @Field()
  type: string;

  @Field()
  userId: string;

  @Field()
  postId: string;
}
```

Este DTO define la estructura para los datos de entrada de creación de reacciones, incluidos `type`, `userId` y `postId`.

3. A continuación, creamos un *resolver* para manejar las consultas y mutaciones de GraphQL relacionadas con las reacciones. En `src/reactions/reactions.resolver.ts`, agrega lo siguiente:

```typescript
import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
import { ReactionsService } from './reactions.service';
import { CreateReactionDto } from './dto/create-reaction.dto';
import { Reaction } from './schemas/reaction.schema';

@Resolver(of => Reaction)
export class ReactionsResolver {
  constructor(private readonly reactionsService: ReactionsService ) {}

  @Query(returns => [Reaction])
  async reactions() {
    return this.reactionsService.findAll();
  }

  @Mutation(returns => Reaction)
  async createReaction(
    @Args('createReactionDto') createReactionDto: CreateReactionDto
  ) {
    return this.reactionsService.create(
      createReactionDto
    );
  }
}
```

Este *resolver* maneja la consulta `reactions` para obtener todas las reacciones y la mutación `createReaction` para crear una nueva reacción.

4. En `src/reactions/reactions.service.ts`, implementa el servicio `Reaction` para interactuar con la base de datos:

```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Reaction } from './schemas/reaction.schema';
import { CreateReactionDto } from './dto/create-reaction.dto';

@Injectable()
export class ReactionsService {
  constructor(
    @InjectModel(Reaction.name) private reactionModel: Model<Reaction>
  ) {}

  async findAll(): Promise<Reaction[]> {
    return this.reactionModel.find().exec();
  }

  async create(createReactionDto: CreateReactionDto): Promise<Reaction> {
    const createdReaction = new this.reactionModel(createReactionDto);
    return createdReaction.save();
  }
}
```

Este servicio proporciona métodos para encontrar todas las reacciones y crear una nueva reacción en la base de datos MongoDB.

5. Finalmente, registra el módulo `Reactions` en `src/app.module.ts`:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { MongooseModule } from '@nestjs/mongoose';
import { join } from 'path';
import { UsersModule } from './users/users.module';
import { PostsModule } from './posts/posts.module';
import { CommentsModule } from './comments/comments.module';
import { ReactionsModule } from './reactions/reactions.module';

@Module({
  imports: [
    GraphQLModule.forRoot({
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
    }),
    MongooseModule.forRoot(
      'mongodb://localhost/nectme'
    ),
    UsersModule,
    PostsModule,
    CommentsModule,
    ReactionsModule,
  ],
})
export class AppModule {}
```

Al completar estos pasos, hemos configurado un módulo `Reaction` básico en NestJS con GraphQL y MongoDB.

Con estos módulos en su lugar, hemos sentado las bases para una SNP sólida utilizando NestJS, GraphQL y MongoDB. Cada módulo maneja su propio conjunto de operaciones, lo que garantiza que nuestra aplicación sea modular, escalable y mantenible.

Con el módulo `Reaction` completamente implementado, ahora tenemos una base sólida para que los usuarios interactúen con las publicaciones a través de *likes* y *upvotes*. Esta funcionalidad agrega una capa de interacción a nuestra SNP, lo que permite a los usuarios expresar sus opiniones sobre el contenido de manera rápida. A continuación, profundizaremos en la expansión de las capacidades de la plataforma mediante la introducción de relaciones de usuario y características sociales, mejorando la experiencia general del usuario y fomentando una comunidad más conectada.

---

### Relaciones de usuario y características sociales

En esta sección, exploraremos cómo gestionar las relaciones de usuario e integrar características sociales en tu SNP. El objetivo es mejorar las interacciones de los usuarios permitiéndoles seguirse mutuamente, establecer amistades y gestionar diversas actividades sociales, como enviar mensajes, compartir publicaciones y participar en grupos.

#### Adición de características sociales

Aquí tenemos las características sociales más importantes:

- **Gestión de amistades:** Permitir a los usuarios enviar, aceptar y rechazar solicitudes de amistad. Implementar una forma de listar amigos y gestionar los estados de amistad (pendiente, aceptada o bloqueada).
- **Sistema de seguimiento (*following*):** Permitir a los usuarios seguir a otros usuarios y ser seguidos. Mostrar un feed de publicaciones de los usuarios seguidos.
- **Mensajería:** Implementar un sistema de mensajería privada donde los usuarios puedan enviar y recibir mensajes. Considerar la adición de capacidades de mensajería en tiempo real con WebSockets.
- **Grupos y eventos:** Permitir a los usuarios crear y unirse a grupos basados en intereses. Implementar la creación y gestión de eventos dentro de los grupos.

#### Implementación de estas características

Para implementar estas características, tendremos que seguir estos pasos:

- **Cambios en esquemas y DTOs:** Actualiza tus esquemas para incluir los campos necesarios para amigos, seguidores y grupos. Crea nuevos DTOs para solicitudes de amistad, mensajes y gestión de grupos.
- **Resolvers y servicios:** Implementa *resolvers* para gestionar solicitudes de amistad, seguir/dejar de seguir usuarios y mensajería. Extiende tus servicios para manejar la lógica de estas nuevas características.
- **Modelos de base de datos:** Actualiza tus modelos de MongoDB para incluir relaciones entre usuarios, mensajes y grupos. Considera el uso de referencias y la incrustación de documentos para consultas eficientes.
- **Queries y mutations de GraphQL:** Crea nuevas consultas y mutaciones para obtener amigos, seguidores, mensajes y grupos. Asegúrate de contar con la autorización y validación adecuadas para estas operaciones.

#### Pruébalo por tu cuenta

Sigue los pasos a continuación como un ejercicio práctico:

1. Agrega un campo `friendRequests` al esquema `User` para rastrear las solicitudes de amistad entrantes y salientes.
2. Crea DTOs para enviar y responder a solicitudes de amistad.
3. Implementa *resolvers* y servicios para gestionar el envío, la aceptación y el rechazo de solicitudes de amistad.
4. Prueba tu implementación creando consultas y mutaciones de GraphQL para gestionar las solicitudes de amistad.

Al extender tu SNP con estas características, puedes crear una plataforma más atractiva e interactiva para tus usuarios. La flexibilidad de GraphQL y la escalabilidad de MongoDB te ayudarán a gestionar la estructura de datos en evolución y las interacciones de los usuarios de manera eficiente.

En la siguiente sección, construyamos una característica interesante que traerá más vida a nuestra aplicación: actualizaciones en tiempo real.

---

### Publicación de actualizaciones y notificaciones en tiempo real en un contexto de redes sociales

Las actualizaciones y notificaciones en tiempo real son componentes cruciales de una SNP moderna. Mantienen a los usuarios interesados proporcionando retroalimentación inmediata y actualizaciones sobre las actividades que les importan. En esta sección, exploraremos cómo implementar actualizaciones y notificaciones en tiempo real en nuestra SNP utilizando suscripciones (*subscriptions*) de NestJS y GraphQL.

Las actualizaciones en tiempo real se pueden lograr mediante suscripciones de GraphQL. Las suscripciones son una forma de enviar datos desde el servidor al cliente cada vez que ocurre un evento específico. En nuestra SNP, podemos usar suscripciones para notificar a los usuarios sobre nuevas publicaciones, comentarios o reacciones:

Primero, necesitamos configurar las suscripciones de GraphQL en nuestra aplicación NestJS. Esto implica configurar el transporte WebSocket y crear *resolvers* de suscripción: Para configurar el transporte WebSocket, modifica `src/app.module.ts` para incluir el transporte WebSocket:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { MongooseModule } from '@nestjs/mongoose';
import { join } from 'path';
import { UsersModule } from './users/users.module';
import { PostsModule } from './posts/posts.module';
import { CommentsModule } from './comments/comments.module';
import { ReactionsModule } from './reactions/reactions.module';

@Module({
  imports: [
    GraphQLModule.forRoot({
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
      subscriptions: {
        'graphql-ws': {
          onConnect: (context) => {
            // Handle connection
          },
          onDisconnect: (context) => {
            // Handle disconnection
          },
        },
      },
    }),
    MongooseModule.forRoot(
      'mongodb://localhost/nectme'
    ),
    UsersModule,
    PostsModule,
    CommentsModule,
    ReactionsModule,
  ],
})
export class AppModule {}
```

A continuación, crea *resolvers* de suscripción para manejar las actualizaciones en tiempo real. Por ejemplo, creemos una suscripción para nuevas publicaciones. En `src/posts/posts.resolver.ts`, agrega el *resolver* de suscripción:

```typescript
import { Resolver, Query, Mutation, Args, Subscription } from '@nestjs/graphql';
import { PostsService } from './posts.service';
import { CreatePostDto } from './dto/create-post.dto';
import { Post } from './schemas/post.schema';
import { PubSub } from 'graphql-subscriptions';

const pubSub = new PubSub();

@Resolver(of => Post)
export class PostsResolver {
  constructor(
    private readonly postsService: PostsService
  ) {}

  @Query(returns => [Post])
  async posts() {
    return this.postsService.findAll();
  }

  @Mutation(returns => Post)
  async createPost(
    @Args('createPostDto') createPostDto: CreatePostDto
  ) {
    const newPost = await this.postsService.create(createPostDto);
    pubSub.publish(
      'postAdded',
      { postAdded: newPost }
    );
    return newPost;
  }

  @Subscription(returns => Post)
  postAdded() {
    return pubSub.asyncIterator('postAdded');
  }
}
```

El siguiente paso es implementar notificaciones. Podemos enviar notificaciones para alertar a los usuarios sobre diversos eventos, como nuevos comentarios en sus publicaciones, nuevos seguidores o reacciones a sus publicaciones: Primero, define el esquema `Notification`. En `src/notifications/schemas/notification.schema.ts`, agrega lo siguiente:

```typescript
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';
import { User } from '../../users/schemas/user.schema';

@Schema()
export class Notification extends Document {
  @Prop({ required: true })
  message: string;

  @Prop({ type: Date, default: Date.now })
  createdAt: Date;

  @Prop({ type: String, ref: 'User', required: true })
  recipient: User;

  @Prop({ type: Boolean, default: false })
  read: boolean;
}

export const NotificationSchema = SchemaFactory.createForClass(Notification);
```

Crea un DTO para notificaciones. En `src/notifications/dto/create-notification.dto.ts`, agrega esto:

```typescript
import { Field, InputType } from '@nestjs/graphql';

@InputType()
export class CreateNotificationDto {
  @Field()
  message: string;

  @Field()
  recipientId: string;
}
```

Luego, crea un *resolver* para las notificaciones. En `src/notifications/notifications.resolver.ts`, agrega lo siguiente:

```typescript
import { Resolver, Mutation, Args, Subscription } from '@nestjs/graphql';
import { NotificationsService } from './notifications.service';
import { CreateNotificationDto } from './dto/create-notification.dto';
import { Notification } from './schemas/notification.schema';
import { PubSub } from 'graphql-subscriptions';

const pubSub = new PubSub();

@Resolver(of => Notification)
export class NotificationsResolver {
  constructor(
    private readonly notificationsService: NotificationsService
  ) {}

  @Mutation(returns => Notification)
  async createNotification(
    @Args('createNotificationDto') createNotificationDto: CreateNotificationDto
  ) {
    const newNotification = await this.notificationsService.create(
      createNotificationDto
    );
    pubSub.publish(
      'notificationAdded',
      { notificationAdded: newNotification }
    );
    return newNotification;
  }

  @Subscription(returns => Notification, {
    filter: (payload, variables) =>
      payload.notificationAdded.recipientId === variables.recipientId,
  })
  notificationAdded(@Args('recipientId') recipientId: string) {
    return pubSub.asyncIterator('notificationAdded');
  }
}
```

Crea un servicio para notificaciones en `src/notifications/notifications.service.ts`:

```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Notification } from './schemas/notification.schema';
import { CreateNotificationDto } from './dto/create-notification.dto';

@Injectable()
export class NotificationsService {
  constructor(
    @InjectModel(Notification.name)
    private notificationModel: Model<Notification>
  ) {}

  async create(createNotificationDto: CreateNotificationDto): Promise<Notification> {
    const createdNotification = new this.notificationModel(
      createNotificationDto
    );
    return createdNotification.save();
  }
}
```

Finalmente, registra el módulo `Notifications` en `src/app.module.ts`:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { MongooseModule } from '@nestjs/mongoose';
import { join } from 'path';
import { UsersModule } from './users/users.module';
import { PostsModule } from './posts/posts.module';
import { CommentsModule } from './comments/comments.module';
import { ReactionsModule } from './reactions/reactions.module';
import { NotificationsModule } from './notifications/notifications.module';

@Module({
  imports: [
    GraphQLModule.forRoot({
      autoSchemaFile: join(process.cwd(), 'src/schema.gql' ),
      subscriptions: {
        'graphql-ws': {
          onConnect: (context) => {
            // Handle connection
          },
          onDisconnect: (context) => {
            // Handle disconnection
          },
        },
      },
    }),
    MongooseModule.forRoot(
      'mongodb://localhost/nectme'
    ),
    UsersModule,
    PostsModule,
    CommentsModule,
    ReactionsModule,
    NotificationsModule,
  ],
})
export class AppModule {}
```

Al implementar actualizaciones y notificaciones en tiempo real, garantizamos que los usuarios permanezcan interesados e informados sobre actividades importantes dentro de la plataforma. Estas características, impulsadas por las suscripciones de GraphQL y el módulo de notificaciones, brindan una experiencia de usuario dinámica, manteniendo las interacciones frescas e inmediatas. Con estos componentes en su lugar, nuestra SNP ofrece un entorno más receptivo e interactivo, mejorando aún más la satisfacción y retención del usuario.

#### Personalización de notificaciones basadas en las preferencias del usuario

En esta sección, mejoraremos nuestro sistema de notificaciones personalizando el feed de notificaciones de acuerdo con las preferencias del usuario. Este enfoque permite a los usuarios controlar qué notificaciones desean recibir, mejorando su experiencia general. En lugar de un ejercicio interactivo, recorramos la implementación paso a paso.

##### Paso 1 – Extender el esquema User para preferencias

Comenzamos extendiendo el esquema `User` para incluir un campo `preferences`, que almacenará la configuración de notificaciones del usuario. Estas preferencias pueden incluir categorías como nuevas publicaciones, comentarios, *likes*, etcétera.

Aquí está la actualización que debe realizarse en el esquema de usuarios:

```typescript
// src/users/schemas/user.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

@Schema()
export class User extends Document {
  // Existing fields...
  @Prop({ type: Map, of: Boolean, default: {} })
  preferences: Map<string, boolean>;
}

export const UserSchema = SchemaFactory.createForClass(User);
```

Aquí, el campo `preferences` es un mapa donde la clave es una cadena que representa la categoría de notificación (por ejemplo, `newPosts` o `comments`), y el valor es un booleano que indica si el usuario desea recibir notificaciones para esa categoría.

##### Paso 2 – Modificar la lógica de notificación para respetar las preferencias

A continuación, modificamos el método `createNotification` en `NotificationsService` para verificar las preferencias del usuario antes de enviar notificaciones. Esto asegura que solo se creen las notificaciones relevantes, según lo especificado por el usuario.

Para lograrlo, peguemos el siguiente código en el archivo `notification.service.ts`:

```typescript
// src/notifications/notifications.service.ts
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Notification } from './schemas/notification.schema';
import { User } from '../users/schemas/user.schema';

@Injectable()
export class NotificationsService {
  constructor(
    @InjectModel(Notification.name)
    private notificationModel: Model<Notification>,
    @InjectModel(User.name) private userModel: Model<User>,
  ) {}

  async createNotification(userId: string, category: string, content: string) {
    const user = await this.userModel.findById(userId).exec();

    // Check user preferences
    if (user.preferences.get(category)) {
      const notification = new this.notificationModel(
        { userId, category, content }
      );
      return notification.save();
    }

    // If user preference is not set to true, do not create
    // notification
    return null;
  }
}
```

En este ejemplo, antes de crear una notificación, recuperamos las preferencias del usuario y solo creamos la notificación si la preferencia para la categoría especificada es `true`.

##### Paso 3 – Modificar el resolver de suscripción

A continuación, actualizamos el *resolver* de suscripción para filtrar las notificaciones según las preferencias del usuario. Esto garantiza que el usuario solo reciba notificaciones en tiempo real para las categorías que le interesan.

Actualicemos ahora el *resolver* de suscripción:

```typescript
// src/notifications/notifications.resolver.ts
import { Resolver, Subscription } from '@nestjs/graphql';
import { PubSub } from 'graphql-subscriptions';
import { NotificationsService } from './notifications.service';

const pubSub = new PubSub();

@Resolver()
export class NotificationsResolver {
  constructor(
    private notificationsService: NotificationsService
  ) {}

  @Subscription(returns => Notification, {
    filter: (payload, variables, context) => {
      const userPreferences = context.user.preferences;
      return userPreferences.get(
        payload.notification.category) === true;
    }
  })
  notificationAdded() {
    return pubSub.asyncIterator('notificationAdded');
  }
}
```

Aquí, usamos la función `filter` para verificar si la categoría de notificación coincide con las preferencias del usuario antes de enviar la notificación al usuario en tiempo real.

##### Paso 4 – Crear una mutación para actualizar preferencias

Por último, implementamos una mutación de GraphQL que permite a los usuarios actualizar sus preferencias de notificación.

Así es como agregamos una mutación de preferencias:

```typescript
// src/users/users.resolver.ts
import { Resolver, Mutation, Args } from '@nestjs/graphql';
import { UsersService } from './users.service';
import { User } from './schemas/user.schema';

@Resolver(of => User)
export class UsersResolver {
  constructor(private usersService: UsersService) {}

  @Mutation(returns => User)
  async updatePreferences(
    @Args('userId') userId: string,
    @Args('preferences') preferences: Map<string, boolean>
  ): Promise<User> {
    return this.usersService.updatePreferences(userId, preferences);
  }
}
```

Luego, actualizamos las preferencias en el archivo `users.service.ts` con el siguiente código:

```typescript
// src/users/users.service.ts
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { User } from './schemas/user.schema';

@Injectable()
export class UsersService {
  constructor(
    @InjectModel(User.name) private userModel: Model<User>
  ) {}

  async updatePreferences(
    userId: string,
    preferences: Map<string, boolean>
  ): Promise<User> {
    return this.userModel.findByIdAndUpdate(
      userId,
      { preferences },
      { new: true }
    ).exec();
  }
}
```

Esta mutación permite a los usuarios actualizar sus preferencias de notificación, y las preferencias actualizadas se guardan en la base de datos.

Siguiendo estos pasos, has implementado exitosamente un sistema de notificaciones completamente funcional que personaliza las notificaciones según las preferencias del usuario. Esto no solo mejora la experiencia del usuario, sino que también garantiza que los usuarios solo reciban las notificaciones que les interesan.

---

### Resumen

Hemos llegado al final de nuestro segundo estudio de caso, donde aprovechamos el uso de APIs GraphQL y bases de datos NoSQL.

Comenzamos con los requisitos y ajustamos el MVP en función de ellos. Los requisitos nos llevaron a la fase de diseño, donde definimos las operaciones principales, los modelos de datos y los esquemas.

Con una serie de ejercicios interactivos, abrimos las puertas a mejoras y te dejamos espacio para mejorar con la práctica.

En el próximo capítulo, exploraremos cómo podemos aprovechar la arquitectura de microservicios en aplicaciones del mundo real utilizando NestJS. ¿Entusiasmado por el siguiente paso? Conquistemos juntos el mundo de los microservicios.
