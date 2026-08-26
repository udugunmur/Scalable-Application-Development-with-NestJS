# Parte 3: Pruebas y Depuración de APIs en NestJS

## Capítulo 8: Pruebas y Depuración de APIs GraphQL en NestJS

¡Bienvenido al Capítulo 8, donde nos sumergiremos profundamente en el mundo de las APIs GraphQL con NestJS! Si pensabas que probar y depurar APIs REST era divertido, espera a ver lo que GraphQL trae a la mesa. Estamos hablando de un patio de recreo completamente nuevo de estrategias de prueba y técnicas de depuración que son tan únicas como GraphQL mismo. No se trata solo de asegurarse de que las cosas funcionen; se trata de desbloquear todo el poder de GraphQL para crear APIs que no solo estén libres de errores, sino que sean totalmente a prueba de balas.

¿Recuerdas la versión GraphQL del sistema de gestión de usuarios que construimos en el [Capítulo 6](https://subscription.packtpub.com/book/web-development/9781835468609/6)? Bueno, está a punto de volverse mucho más interesante a medida que nos preparamos para aplicarle algo de verdadera magia de pruebas GraphQL. Desde los detalles esenciales de las pruebas unitarias de los resolvers de GraphQL hasta la emoción de las pruebas de extremo a extremo (*end-to-end* o E2E), tenemos mucho que cubrir. No olvidemos el trabajo detectivesco de la depuración, donde te armaremos con las herramientas y trucos para rastrear y resolver esos molestos problemas que no te dejan dormir por la noche.

Pero aquí está la clave: las pruebas y la depuración no son simples elementos en una lista de verificación. Son el ingrediente secreto para construir aplicaciones que resistan la prueba del tiempo, aplicaciones que no solo sean funcionales, sino verdaderamente excepcionales y fáciles de escalar. Así que, al final de este capítulo, estarás equipado con el conocimiento y las habilidades para garantizar que tus aplicaciones sean confiables, de alto rendimiento y seguras.

¿Listo para embarcarte en esta aventura? Esto es lo que está en nuestra agenda:

- Pruebas unitarias para resolvers de GraphQL
- Pruebas E2E en GraphQL
- Depuración de APIs GraphQL

Para cuando hayamos terminado, estarás bien preparado para abordar de lleno el desarrollo de APIs GraphQL, armado con las mejores prácticas que llevarán tus proyectos al siguiente nivel. ¡Comencemos y convirtamos esos desafíos de GraphQL en triunfos!

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Pruebas unitarias para resolvers de GraphQL

Las pruebas unitarias de resolvers GraphQL en NestJS consisten en verificar que cada resolver se comporte como se espera, independientemente de los demás. En un enfoque *code-first*, donde el esquema se genera basándose en decoradores de TypeScript, las pruebas se vuelven cruciales para garantizar que el esquema represente con precisión la lógica de negocio subyacente encapsulada dentro de estos resolvers.

Para nuestra demostración, extenderemos el sistema de gestión de usuarios desarrollado en el [Capítulo 6](https://subscription.packtpub.com/book/web-development/9781835468609/6), centrándonos en los resolvers dentro de nuestra API GraphQL *code-first*. Este enfoque nos permite validar la lógica para consultar, crear, actualizar y eliminar usuarios, asegurando que nuestra API GraphQL sea robusta y se comporte de manera predecible bajo diversas condiciones.

#### Configuración para las pruebas

Antes de sumergirnos en la escritura de pruebas, configuremos nuestro entorno de pruebas. Asumiendo que has seguido el ritmo desde el [Capítulo 6](https://subscription.packtpub.com/book/web-development/9781835468609/6), deberías tener un sistema de gestión de usuarios con resolvers listos para ser probados. Si no es así, te animo a consultar la rama *code-first* en el repositorio de GraphQL de nuestro GitHub como punto de partida.

La configuración inicial para probar resolvers de GraphQL implica crear archivos de prueba para cada resolver. En NestJS, estos suelen ubicarse junto a los archivos de resolver y nombrarse usando el sufijo `.spec.ts`. Para `UsersResolver`, el archivo de prueba sería `users.resolver.spec.ts`.

Con el archivo de prueba generado por la CLI de Nest, intenta ejecutar el archivo de prueba y asegúrate de que todo funcione sin ningún error.

#### Escritura de pruebas unitarias para resolvers

La piedra angular de las pruebas unitarias en NestJS es el módulo `@nestjs/testing`, que proporciona utilidades para crear un entorno de pruebas que simula el tiempo de ejecución de tu aplicación. Aquí hay una estructura básica para una suite de pruebas que prueba `UsersResolver`:

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { UsersResolver } from './users.resolver';
import { UsersService } from './users.service';

describe('UsersResolver', () => {
  let resolver: UsersResolver;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [UsersResolver, UsersService],
    }).compile();

    resolver = module.get<UsersResolver>(UsersResolver);
  });

  it('should be defined', () => {
    expect(resolver).toBeDefined();
  });

  // Additional tests will go here
});
```

Esta configuración asegura que nuestro resolver esté debidamente instanciado con todas sus dependencias y listo para las pruebas. Es un paso fundamental que verifica que nuestra infraestructura de pruebas esté configurada correctamente antes de proceder con pruebas más específicas.

En la siguiente sección, aprendamos sobre la estrategia de simulación (*mocking*) y cómo se puede aplicar a las dependencias de un módulo.

#### Simulación de dependencias (Mocking dependencies)

Dado que los resolvers a menudo dependen de servicios para la obtención y manipulación de datos, necesitamos simular (*mock*) estas dependencias para aislar nuestras pruebas. El *mocking* nos permite simular el comportamiento de estos servicios sin requerir una conexión real a la base de datos o una llamada a una API externa; ya estás familiarizado con la estrategia de simulación, ya que hicimos prácticamente lo mismo en el capítulo anterior para los controladores.

Así es como podrías simular `UsersService` para nuestras pruebas:

```typescript
providers: [
  UsersResolver,
  {
    provide: UsersService,
    useValue: {
      findAll: jest.fn(() => [
        // Mocked user data
      ]),
      // Additional mocked functions
    },
  },
],
```

Al proporcionar una implementación simulada para `UsersService`, podemos controlar los valores de retorno y simular varios escenarios para nuestras pruebas de resolver.

Dado que conocemos la forma del objeto devuelto por nuestros archivos de servicio, la simulación es sencilla.

Ahora, probemos la lógica del resolver en la siguiente sección.

#### Prueba de la lógica del resolver

Con nuestra configuración de pruebas y *mocks* en su lugar, ahora podemos escribir pruebas que validen el comportamiento de nuestros resolvers. Por ejemplo, para probar el resolver `findAll`, podríamos escribir un caso de prueba como este:

```typescript
it('should return an array of users', async () => {
  const users = await resolver.findAll();
  expect(users).toBeInstanceOf(Array);
  expect(users).toHaveLength(mockedUsers.length);
});
```

Esta prueba asegura que nuestro resolver `findAll` devuelva un array de usuarios, coincidiendo con el número esperado de usuarios simulados.

Con todo en su lugar, así es como debería verse el archivo de prueba del resolver:

```typescript
// users.resolver.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { UsersResolver } from './users.resolver';
import { UsersService } from './users.service';
import { CreateUserInput } from './dto/create-user.input';

describe('UsersResolver', () => {
  let resolver: UsersResolver;
  let mockUsersService: Partial<UsersService>;

  beforeEach(async () => {
    mockUsersService = {
      create: jest.fn((dto: CreateUserInput) => ({ id: Date.now(), ...dto })),
      // other methods
    };

    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersResolver,
        {
          provide: UsersService,
          useValue: mockUsersService,
        },
      ],
    }).compile();

    resolver = module.get<UsersResolver>(UsersResolver);
  });

  it('should be defined', () => {
    expect(resolver).toBeDefined();
  });

  it('should create a user', () => {
    const dto: CreateUserInput = {
      name: 'Test User',
      email: 'test@test.com',
    };
    expect(resolver.createUser(dto)).toEqual({
      id: expect.any(Number),
      ...dto,
    });
    expect(mockUsersService.create).toHaveBeenCalledWith(dto);
  });
});
```

En el fragmento de código proporcionado, hemos unido todo y probado la creación de usuarios. Observa que casi todo es similar a lo que teníamos en la API REST; de hecho, la función del resolver, una vez simulada, se comporta exactamente igual que las de los controladores.

#### Ejercicio práctico

Ahora que sabes cómo probar funciones de resolver y de controlador, completa este archivo con otros casos de prueba y asegúrate de que los casos límite (*edge cases*) también sean probados.

Las pruebas unitarias de resolvers GraphQL en NestJS son una práctica esencial para garantizar la confiabilidad y corrección de tu API GraphQL. Siguiendo los pasos descritos, desde la configuración de tu entorno de pruebas hasta la simulación de dependencias y la escritura de casos de prueba específicos, puedes construir una base sólida para tus aplicaciones GraphQL.

Bien hecho; hemos probado nuestros archivos de resolvers y aprendido sobre estrategias de simulación (*mocking*). En la siguiente sección, obtendremos beneficios al implementar pruebas E2E en nuestra aplicación, haciéndola aún más robusta.

---

### Pruebas E2E en GraphQL

Solo hemos probado bloques de código por separado, pero no hemos probado los decoradores de Nest, como `@Query` o `@Mutation`. En esta sección, hagámoslo.

Veamos cómo podemos probar la consulta `GetHello`, una de las operaciones que tenemos en nuestro archivo de resolver, ya que es la más simple.

Por defecto, Nest instala el paquete `supertest` por nosotros; este paquete nos ayudará a simular peticiones HTTP.

Como ya sabes, todas las peticiones GraphQL (mutaciones y consultas) son simplemente peticiones POST al punto de entrada de la aplicación, que es `/graphql`. Tener esto en mente nos ayudará a enfocarnos en probar nuestra API GraphQL como una API REST de un solo endpoint.

La siguiente pregunta sería esta: ¿Qué objeto en el cuerpo (*body*) y encabezados o parámetros de consulta utilizo para simular una API GraphQL?

Desde una aplicación cliente (usamos este punto de vista ya que nuestra prueba E2E simulará las peticiones de los clientes), la Figura 8.1 muestra lo que enviamos como carga útil (*payload*) a ese punto de entrada.

*Figura 8.1 – Peticiones GraphQL*

Como puedes ver aquí, nuestra consulta GraphQL se envía como una cadena de texto en el campo `query` de la petición HTTP, y enviamos variables usando la propiedad `variables`. Para simular esto, creemos archivos auxiliares (*helpers*) que contendrán estos parámetros por nosotros.

Bajo `src/common/helpers/graphql.helper.ts`, añade este contenido:

```typescript
export const GRAPHQL_ENDPOINT = '/graphql';
export const HELL0_WORLD = 'Hello World!';

export const GET_HELLO_OPERATION_NAME = 'Query';

export const GET_HELLO = `query Query {
  getHello
}`;
```

Como puedes ver, la cadena `GET_HELLO` contendrá la operación GraphQL como un string, tal como lo tenemos en nuestro Apollo Playground.

Actualiza el archivo existente `app.e2e-spec.ts` con el siguiente código:

```typescript
// test/app.e2e-spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from '../src/app.module';
import {
  GET_HELLO,
  GET_HELLO_OPERATION_NAME,
  GRAPHQL_ENDPOINT,
  HELL0_WORLD,
} from '../src/common/helpers/graphql.helper';

jest.setTimeout(70000);

describe('AppResolver (e2e)', () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  afterEach(async () => {
    await app.close();
  });

  it('should get a hello world', () => {
    return request(app.getHttpServer())
      .post(GRAPHQL_ENDPOINT)
      .send({
        operationName: GET_HELLO_OPERATION_NAME,
        query: GET_HELLO,
      })
      .expect(200)
      .expect((res) => {
        expect(res.body.data.getHello).toBe(HELL0_WORLD);
      });
  });
});
```

En el fragmento de código anterior, hemos hecho lo siguiente:

- Hemos configurado un tiempo de espera (*timeout*) de 70000 ms. A veces, puedes experimentar tiempos de espera agotados, especialmente porque hemos creado pruebas de integración, que pueden ser la suma de más de un método ejecutándose juntos, y cuando tenemos operaciones asíncronas en nuestros archivos de prueba que interactúan con una base de datos simulada.
- Hemos utilizado los métodos de `request` de `supertest` para simular peticiones HTTP; no dudes en leer más sobre `supertest` ([https://github.com/ladjs/supertest](https://github.com/ladjs/supertest)), un paquete muy popular para servidores Node.js. Con `supertest`, puedes simular peticiones POST, GET, PUT, PATCH, tal como lo hicimos en la API REST. Aquí, solo usaremos `post` (para simular una petición POST) con sus métodos encadenados como `send()`, `expect()`, etcétera.

Ahora, veamos cómo podemos probar la creación de un usuario. Primero, agreguemos archivos auxiliares (*helpers*) con nombres de operación, variables, mutaciones, etcétera.

Para ponerlo en contexto, no olvides que la mutación `createUser` se ve de la siguiente manera:

```typescript
@Mutation(() => User)
createUser(@Args('createUserInput') createUserInput: CreateUserInput) {
  return this.usersService.create(createUserInput);
}
```

La mutación anterior espera un argumento de entrada llamado `createUserInput` en la consulta. La cadena de consulta que debemos proporcionar tiene que tomar esto en consideración; trabajemos en ello.

Bajo el archivo `common/helpers/create-user.helper.ts`, simplemente pega esto:

```typescript
// common/helpers/create-user.helper.ts
import * as Chance from 'chance';
const chance = new Chance();

export const CREATE_USER_OPERATION_NAME = 'CreateUser';

export const CREATE_USER_MUTATION = `mutation CreateUser($createUserInput: CreateUserInput!) {
   createUser(createUserInput: $createUserInput) {
     id
     email
     name
   }
 }`;

export const generateCreateUserVariables = () => {
  return {
    createUserInput: {
      name: chance.name(),
      email: chance.email(),
    },
  };
};
```

En el fragmento de código anterior, hemos hecho lo siguiente:

- Hemos creado todas las piezas necesarias para enviar peticiones, como variables, operaciones GraphQL, etcétera.
- Hemos utilizado `chance`, una librería que nos ayuda a crear datos falsos. Aquí necesitábamos un nombre y un correo electrónico, y en lugar de valores fijos en el código (*hardcoded*), `chance` generará valores aleatorios por nosotros.
- `CREATE_USER_MUTATION` está escrita tal como una aplicación cliente consultaría nuestra API en el futuro, especificando lo que necesitamos recibir en la respuesta, siendo una de las características clave de las APIs GraphQL.

Ahora, creemos un archivo para probar los resolvers de usuario de extremo a extremo (E2E), dentro de la carpeta `test`. Añadamos el archivo `users/users.e2e-spec.ts` y peguemos el siguiente código allí:

```typescript
// test/users/users.e2e-spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';

import { AppModule } from '../../src/app.module';

import { GRAPHQL_ENDPOINT } from '../../src/common/helpers/graphql.helper';
import {
  CREATE_USER_MUTATION,
  CREATE_USER_OPERATION_NAME,
  generateCreateUserVariables,
} from '../../src/common/helpers/create-user.helper';

import { User } from '../../src/users/entities/user.entity';

jest.setTimeout(70000);

describe('User resolver (e2e)', () => {
  let app: INestApplication;
  let user: User;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  afterEach(async () => {
    await app.close();
  });

  it('Should create an user with user mutation', () => {
    const createUserInput = generateCreateUserVariables().createUserInput;
    return request(app.getHttpServer())
      .post(GRAPHQL_ENDPOINT)
      .send({
        operationName: CREATE_USER_OPERATION_NAME,
        query: CREATE_USER_MUTATION,
        variables: { createUserInput },
      })
      .expect(200)
      .expect((res) => {
        expect(res.body.data.createUser).toBeDefined();
        user = res.body.data.createUser;
        expect(user.id).toBeDefined();
        expect(user.name).toBe(createUserInput.name);
        expect(user.email).toBe(createUserInput.email);
      });
  });
});
```

Se ve similar a lo que teníamos antes, ¿verdad?

Esperamos recibir un código de estado 200, y la respuesta que obtenemos tendrá un objeto `body.data` con el contenido (*payload*) de la respuesta.

Ahora, ejecutemos nuestras pruebas E2E:

```bash
# run end-to-end tests
$ yarn test:e2e
```

Si has estado siguiendo los pasos, podrás obtener algo similar a la Figura 8.2:

*Figura 8.2 – Resultados de las pruebas E2E*

#### Ejercicio práctico

Ahora, intenta replicar el mismo proceso creando pruebas E2E para las funcionalidades `updateUser`, `removeUser` y `getUser`.

¡Abróchate el cinturón! Acabamos de dominar las pruebas unitarias de nuestros servicios y resolvers e incluso agregamos algunas excelentes pruebas E2E.

Esperamos que estés adquiriendo verdaderos superpoderes en lo que respecta a la manipulación de APIs GraphQL. Ahora, es momento de depurar en la siguiente sección.

---

### Depuración de APIs GraphQL

Depurar APIs GraphQL puede ser un desafío debido a la complejidad involucrada en la ejecución de consultas, las definiciones de esquemas y la lógica de los resolvers. El desafío se vuelve aún más pronunciado en un proyecto como el nuestro, que utiliza el enfoque *code-first* para generar el esquema. Con un esquema generado automáticamente a partir del código, pueden surgir problemas a raíz de tipos incompatibles, consultas inválidas o malas configuraciones de los resolvers. A diferencia de las APIs REST, que dependen de distintos endpoints, la estructura de endpoint único de GraphQL agrega otra capa de complejidad, lo que dificulta identificar la causa raíz de un problema.

En esta sección, exploraremos cómo depurar APIs GraphQL de manera eficiente dentro del contexto de nuestro proyecto. Cubriremos herramientas y prácticas adaptadas al enfoque *code-first*, asegurando que el esquema, las consultas y las mutaciones funcionen como se espera.

#### Comprensión de los desafíos de depuración para APIs GraphQL

Depurar APIs GraphQL presenta desafíos distintos en comparación con las APIs REST tradicionales. En GraphQL, el esquema lo une todo: consultas, mutaciones y tipos. En el enfoque *code-first*, donde el esquema se genera a partir de decoradores de TypeScript, cualquier problema en el código puede propagarse a la capa de GraphQL, provocando errores en tiempo de ejecución que a menudo son difíciles de rastrear hasta su origen.

Por ejemplo, considera el siguiente esquema generado con *code-first* para nuestro proyecto:

```graphql
type User {
  id: Int!
  name: String!
  email: String!
}

type Query {
  users: [User!]!
  user(id: Int!): User!
}

type Mutation {
  createUser(createUserInput: CreateUserInput!): User!
  updateUser(updateUserInput: UpdateUserInput!): User!
  removeUser(id: Int!): User!
}

input CreateUserInput {
  name: String!
  email: String!
}

input UpdateUserInput {
  id: Int!
  name: String
  email: String
}
```

Los desafíos de depuración pueden derivar de lo siguiente:

- **Malas configuraciones de los resolvers:** Los resolvers deben coincidir con el esquema y manejar los datos correctamente. Por ejemplo, un error en el resolver `user(id: Int!)` podría devolver el usuario incorrecto o fallar si la lógica del resolver no es adecuada.
- **Fallos en la validación de entradas:** GraphQL puede fallar al ejecutar mutaciones como `createUser` si la validación de entrada no se maneja adecuadamente.
- **Problemas de sincronización del esquema:** Dado que el esquema se genera automáticamente, pueden existir inconsistencias entre el código TypeScript y el esquema final de GraphQL. Por ejemplo, campos requeridos faltantes o tipos de datos incorrectos en el código TypeScript se propagarán al esquema y causarán problemas durante la ejecución.

Estos problemas pueden ser difíciles de detectar con las técnicas de depuración típicas, lo que hace crucial el uso de herramientas especializadas y mejores prácticas. Al aprovechar las herramientas adecuadas y comprender el funcionamiento interno del flujo de ejecución de GraphQL, puedes reducir el tiempo dedicado a identificar y resolver estos problemas.

#### Aprovechamiento de la depuración con herramientas integradas de NestJS

NestJS incluye un servicio `Logger` integrado que puede registrar información crítica en diferentes niveles (`log`, `error`, `warn`, `debug`, `verbose`). Por defecto, el registrador (*logger*) está habilitado y registra información durante el arranque (*bootstrapping*) de la aplicación, así como en casos tales como mostrar excepciones capturadas. Este sistema de registro integrado ayuda a monitorear y depurar diferentes partes de tu API GraphQL.

Aquí hay un ejemplo del uso del servicio `Logger` en un resolver de GraphQL:

```typescript
import { Resolver, Query, Args } from '@nestjs/graphql';
import { Logger } from '@nestjs/common';
import { UserService } from './user.service';
import { User } from './user.entity';

@Resolver(() => User)
export class UserResolver {
  private readonly logger = new Logger(UserResolver.name);

  constructor(private readonly userService: UserService) {}

  @Query(() => User)
  async user(@Args('id') id: number): Promise<User> {
    this.logger.debug(`Fetching user with id: ${id}`);
    const user = await this.userService.findOne(id);
    if (!user) {
      this.logger.warn(`User with id ${id} not found`);
      throw new Error(`User with id ${id} not found`);
    }
    this.logger.log(
      `User fetched successfully: ${JSON.stringify(user)}`
    );
    return user;
  }
}
```

En el fragmento de código anterior, el servicio `Logger` se instancia para la clase `UserResolver`. El método `logger.debug()` se utiliza para registrar el ID del usuario que se está consultando, `logger.warn()` emite una advertencia si no se encuentra al usuario, y `logger.log()` proporciona un registro general cuando un usuario se recupera con éxito. Esto demuestra cómo los niveles de registro pueden ayudar a rastrear varias etapas del ciclo de vida de una petición.

Para configurar globalmente los registros (*logs*) en toda tu aplicación NestJS, puedes especificar los niveles de registro durante el proceso de arranque (*bootstrap*) de la aplicación en el archivo `main.ts`:

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule, {
    logger: ['log', 'warn', 'debug'], // Selecting specific log levels
  });
  await app.listen(3000);
}
bootstrap();
```

En el fragmento de código anterior, solo se capturarán globalmente los registros de `log`, `warn` y `debug`. También puedes desactivar el registro por completo configurando `logger: false`.

#### Inspección de peticiones y respuestas con middleware

NestJS admite middleware personalizado que te permite interceptar y registrar peticiones y respuestas HTTP, brindando una visión más profunda del comportamiento de tu API GraphQL. El middleware puede ser especialmente valioso para monitorear y depurar operaciones a nivel de red.

Aquí hay un ejemplo de un middleware que registra cada petición GraphQL entrante:

```typescript
import { Injectable, NestMiddleware, Logger } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggingMiddleware implements NestMiddleware {
  private readonly logger = new Logger(LoggingMiddleware.name);

  use(req: Request, res: Response, next: NextFunction) {
    this.logger.log(
      `Incoming Request: ${req.method} ${req.url}`
    );
    next();
  }
}
```

En el fragmento de código anterior, `LoggingMiddleware` registra cada petición HTTP entrante usando `logger.log()`. El objeto `req` captura detalles sobre la petición (como el método y la URL), mientras que `next()` pasa el control al siguiente middleware o controlador en la canalización (*pipeline*). Este middleware ayuda a rastrear cada consulta, mutación o suscripción de GraphQL, brindándote información sobre qué operaciones se están ejecutando.

Para usar este middleware, necesitas aplicarlo a tu aplicación registrándolo en el archivo `main.ts` o en la configuración de middleware de tu módulo.

#### Uso de filtros de excepciones para un manejo de errores mejorado

Los filtros de excepciones en NestJS ofrecen una forma estructurada de capturar y manejar errores en toda tu aplicación, incluyendo en los resolvers de GraphQL. Estos filtros pueden capturar y registrar detalles de los errores, lo que facilita la identificación y resolución de problemas.

Aquí hay un ejemplo de un filtro de excepciones que registra información detallada de los errores:

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, Logger } from '@nestjs/common';
import { GqlArgumentsHost } from '@nestjs/graphql';
import { GraphQLError } from 'graphql';

@Catch(GraphQLError)
export class GraphQLExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(GraphQLExceptionFilter.name);

  catch(exception: GraphQLError, host: ArgumentsHost) {
    const gqlHost = GqlArgumentsHost.create(host);
    this.logger.error(
      `Error in GraphQL resolver: ${exception.message}`,
      exception.stack
    );
    return exception;
  }
}
```

En el ejemplo anterior, `GraphQLExceptionFilter` es un filtro de excepciones que captura excepciones `GraphQLError`. Registra el mensaje de error y el rastreo de pila (*stack trace*) usando `logger.error()`. Al registrar estos errores, obtienes información valiosa sobre problemas en los resolvers o fallos en la ejecución de GraphQL, facilitando la depuración del sistema. Este filtro se puede aplicar a tu módulo de GraphQL para garantizar que todas las excepciones se registren de manera efectiva.

Al utilizar las herramientas de depuración integradas de NestJS, como el servicio `Logger`, variables de entorno para el modo de depuración, middleware para la inspección de peticiones y filtros de excepciones para el manejo de errores, puedes optimizar el proceso de depuración de tus APIs GraphQL. Estas herramientas ofrecen una comprensión profunda del funcionamiento interno de tu aplicación, desde la ejecución de consultas hasta problemas con los resolvers, brindándote la visibilidad necesaria para diagnosticar y resolver errores eficientemente.

#### Herramientas de depuración de Apollo Server

Apollo Server, comúnmente utilizado con NestJS para GraphQL, proporciona varias herramientas de depuración que pueden ayudarte a comprender mejor el rendimiento y la ejecución de tu API GraphQL. Una de las características más destacadas de Apollo es su sólido sistema de plugins, que permite a los desarrolladores rastrear el rendimiento de las resoluciones de campos individuales y dar formato a los errores de manera que ofrezcan más información sobre posibles problemas.

##### Registro a nivel de campo con plugins de Apollo

Apollo te permite agregar registros personalizados y seguimiento de rendimiento a un nivel más granular mediante plugins. Puedes registrar el rendimiento de campos específicos, lo cual es especialmente útil al solucionar consultas lentas o rastrear resolvers ineficientes.

Aquí hay un ejemplo de cómo crear un plugin personalizado que registre el tiempo de ejecución de cada resolver de campo en tu esquema:

```typescript
import { ApolloServerPlugin } from 'apollo-server-plugin-base';

export const fieldLoggingPlugin: ApolloServerPlugin = {
  requestDidStart() {
    return {
      didResolveField(field) {
        const start = Date.now();
        return () => {
          const duration = Date.now() - start;
          console.log(
            `Resolved ${field.name} in ${duration}ms`
          );
        };
      },
    };
  },
};
```

En el fragmento de código anterior, `fieldLoggingPlugin` se engancha en el ciclo de vida de una petición GraphQL. Específicamente, la función `didResolveField` se invoca cada vez que se ejecuta un resolver de campo. Se registra la hora de inicio y, una vez que el resolver finaliza, se registra el tiempo empleado en resolver el campo. Esto te permite medir el rendimiento de resolvers específicos e identificar cuellos de botella en tu API.

##### Formateo de errores en Apollo

Apollo también proporciona mecanismos sólidos de manejo de errores, incluido el formateo personalizado de errores. Al dar formato a los errores de GraphQL, puedes incluir contexto adicional en las respuestas de error que ayudará en la depuración, particularmente en entornos de producción.

Así es como se personaliza el formato de errores en Apollo:

```typescript
import { ApolloError } from 'apollo-server-express';

const server = new ApolloServer({
  typeDefs,
  resolvers,
  formatError: (err) => {
    if (err instanceof ApolloError) {
      return err;
    }
    return {
      message: err.message,
      code: err.extensions?.code || 'INTERNAL_SERVER_ERROR',
      stack: process.env.NODE_ENV === 'development' ? err.stack : null,
    };
  },
});
```

En el fragmento de código anterior, la función `formatError` te permite controlar la estructura de la respuesta de error enviada a los clientes. Si un error es una instancia de `ApolloError`, se devuelve tal cual. Para otros errores, se incluyen campos adicionales como un código de error personalizado y el rastreo de pila (*stack trace*, solo en modo de desarrollo). Esto te brinda una mejor visibilidad sobre el origen de los errores, garantizando al mismo tiempo que la información confidencial, como los rastreos de pila, permanezca oculta en producción.

##### Uso de requestDidStart para el rastreo de consultas

Apollo proporciona un hook del ciclo de vida `requestDidStart` que resulta útil para rastrear el ciclo de vida completo de una petición GraphQL. Puedes usar este hook para monitorear cuándo comienza y termina una consulta, lo que ayuda a rastrear peticiones lentas o fallidas, como se muestra en el siguiente ejemplo:

```typescript
const server = new ApolloServer({
  typeDefs,
  resolvers,
  plugins: [
    {
      requestDidStart() {
        console.log('Query started');
        return {
          willSendResponse() {
            console.log('Query finished');
          },
        };
      },
    },
  ],
});
```

En el fragmento de código anterior, `requestDidStart` registra un mensaje cuando comienza una consulta y `willSendResponse` registra otro mensaje cuando la consulta finaliza. Esta implementación simple te permite rastrear la ejecución de las peticiones y señalar con precisión dónde podrían surgir problemas.

Para hacer que estas estrategias de depuración sean más prácticas, exploremos algunos escenarios del mundo real donde estas herramientas se pueden usar para resolver problemas comunes en una API en producción. Aquí hay algunos ejemplos:

**Resolución lenta de consultas:** Imagina que estás ejecutando una API GraphQL en producción y recibes informes de que una consulta específica es mucho más lenta de lo esperado. Al usar el registro a nivel de campo de Apollo, puedes aislar el problema a un campo específico. Pasos para diagnosticar y resolver:

1. **Habilitar el registro a nivel de campo:** Utiliza el `fieldLoggingPlugin` descrito anteriormente para rastrear el tiempo de ejecución de cada campo.
2. **Monitorear campos lentos:** Una vez habilitado el registro, monitorea los logs para encontrar el campo que tarda más en resolverse.
3. **Optimizar la lógica del resolver:** Después de identificar el campo lento, revisa y optimiza la lógica de su resolver. El problema podría ser una consulta ineficiente a la base de datos o una recuperación excesiva de datos.
4. **Probar las mejoras:** Después de realizar optimizaciones, monitorea nuevamente los logs para asegurarte de que el rendimiento de la consulta haya mejorado.

**Depuración de la integración con servicios GraphQL de terceros:** En otro escenario, podrías estar integrando un servicio GraphQL de terceros en tu API y enfrentando errores relacionados con las respuestas de dicho servicio. Pasos para diagnosticar y resolver:

1. **Usar el formateo de errores de Apollo:** Da formato a los errores como se demostró anteriormente para incluir tantos detalles como sea posible, especialmente para servicios de terceros.
2. **Habilitar registro detallado (verbose):** Si el problema persiste, habilita el registro detallado usando la variable de entorno `DEBUG=nestjs*` para capturar información detallada sobre cómo se maneja internamente la petición.
3. **Verificar las cargas útiles (*payloads*) de respuesta:** Inspecciona las cargas útiles de peticiones y respuestas mediante middleware para asegurarte de que se envíen y reciban los datos correctos.
4. **Colaborar con el servicio de terceros:** Si identificas problemas relacionados con el servicio de terceros, utiliza los registros y mensajes de error para comunicarte más eficazmente con su equipo de soporte.

Las estrategias proporcionadas guiarán tu camino de depuración, pero siempre debemos ser conscientes de que las situaciones pueden diferir mucho según el contexto de la aplicación que estés construyendo. Sin embargo, algunas mejores prácticas te facilitarán la vida; exploremos algunas en la siguiente sección.

#### Mejores prácticas para depurar APIs GraphQL en producción

Al depurar APIs GraphQL en producción, es importante seguir las mejores prácticas para garantizar un funcionamiento fluido sin comprometer el rendimiento ni la seguridad. Aquí tienes algunas estrategias que puedes implementar para mejorar tu proceso de depuración.

##### Registro estructurado (Structured logging)

Implementa un registro estructurado para capturar información crítica sin sobrecargar el sistema con logs excesivos. Herramientas como Winston o Pino pueden ayudar a estructurar tus registros para una mejor capacidad de búsqueda y análisis:

```typescript
import * as winston from 'winston';
import { WinstonModule } from 'nest-winston';

@Module({
  imports: [
    WinstonModule.forRoot({
      transports: [
        new winston.transports.Console({
          format: winston.format.combine(
            winston.format.timestamp(),
            winston.format.json()
          ),
        }),
      ],
    }),
  ],
})
export class AppModule {}
```

En este ejemplo, Winston se configura como la biblioteca de registro en NestJS. Los registros se formatean con marcas de tiempo y se emiten en formato JSON, el cual puede ser analizado fácilmente por herramientas de monitoreo.

##### Monitoreo y seguimiento del rendimiento

Utiliza herramientas de monitoreo, como Apollo Studio o Grafana, para visualizar métricas de rendimiento y rastrear posibles cuellos de botella en tiempo real. Estas herramientas pueden proporcionar información sobre el rendimiento de las consultas, las tasas de éxito de las peticiones y las apariciones de errores.

##### Manejo de entornos de producción a gran escala

Al escalar APIs GraphQL en producción, sigue estas prácticas adicionales:

- **Implementar limitación de tasa (*rate limiting*):** Para evitar sobrecargar tu sistema, considera limitar la frecuencia de peticiones en el endpoint de GraphQL.
- **Manejo de errores:** Utiliza el formateo de errores personalizado de Apollo para garantizar respuestas de error consistentes e informativas.
- **Monitorear fugas de memoria (*memory leaks*):** Utiliza herramientas como PM2 o instantáneas del *heap* de Node.js para monitorear el uso de memoria y prevenir fugas de memoria.
- **Usar almacenamiento en caché (*caching*):** Almacenar en caché los resultados a nivel de resolver o de recuperación de datos puede mejorar significativamente el rendimiento, especialmente para los datos solicitados con frecuencia.

Al incorporar estas herramientas y mejores prácticas, estarás mejor equipado para manejar los problemas comunes que surgen en las APIs GraphQL en producción. Desde el registro a nivel de campo hasta el manejo estructurado de errores, estas técnicas de depuración ofrecen la perspectiva y el control que necesitas para mantener una API confiable y de alto rendimiento.

---

### Resumen

Al concluir el Capítulo 8, nos hemos basado en los cimientos establecidos en el capítulo anterior para adaptar nuestras estrategias de prueba y depuración específicamente para GraphQL dentro del ecosistema de NestJS. Esta exploración focalizada en las necesidades de prueba únicas de GraphQL subraya la importancia de enfoques especializados para garantizar que nuestras APIs sean tanto eficaces como resilientes.

En este capítulo, nos concentramos en las pruebas unitarias de resolvers de GraphQL, enfatizando el papel fundamental de aislar y probar las funcionalidades que impulsan nuestras APIs GraphQL. Al demostrar cómo simular (*mock*) dependencias de manera efectiva y simular escenarios, hemos proporcionado las herramientas necesarias para garantizar que nuestros resolvers operen según lo previsto bajo diversas condiciones.

Nuestro recorrido a través de las pruebas E2E de APIs GraphQL ha reforzado el valor de probar nuestras aplicaciones como un todo integrado, asegurando que cada capa, desde el resolver hasta la base de datos, funcione en armonía. Esta estrategia integral de pruebas es clave para entregar APIs GraphQL robustas y listas para producción.

A través de un enfoque pragmático de la depuración, hemos descubierto métodos para identificar y resolver de manera eficiente problemas específicos de GraphQL, mejorando la estabilidad y confiabilidad de nuestras APIs. Este conocimiento práctico nos equipa para abordar las complejidades del desarrollo con GraphQL con total confianza.

Al revisitar el sistema de gestión de usuarios con un enfoque en GraphQL, aplicamos lo aprendido en un contexto del mundo real, consolidando nuestra comprensión de las capacidades de NestJS para el desarrollo de APIs GraphQL. Esta experiencia práctica no solo refuerza nuestro aprendizaje, sino que también nos prepara para aplicar estas estrategias en nuestros propios proyectos.

Mirando hacia adelante, las metodologías y conocimientos adquiridos en este capítulo serán fundamentales a medida que continuemos desarrollando y refinando nuestras APIs GraphQL. Adoptar los principios de pruebas rigurosas y depuración efectiva garantizará que nuestras APIs no solo sean funcionales, sino también confiables y eficientes. Armados con estas habilidades, estamos mejor preparados para navegar los desafíos del desarrollo moderno de APIs, aprovechando al máximo NestJS y GraphQL.
