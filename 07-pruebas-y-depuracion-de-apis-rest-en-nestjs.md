# Parte 3: Pruebas y Depuración de APIs en NestJS

## Capítulo 7: Pruebas y Depuración de APIs REST en NestJS

Bienvenidos al Capítulo 7. Este capítulo profundiza en los aspectos cruciales para garantizar que sus APIs no solo sean funcionales, sino también robustas y libres de errores. Las pruebas y la depuración son etapas indispensables en el ciclo de vida del desarrollo, ya que proporcionan una red de seguridad contra comportamientos inesperados y aseguran que su aplicación se comporte según lo previsto en diversas circunstancias. A través de este capítulo, adquirirá experiencia práctica e información sobre las metodologías y herramientas que hacen que las pruebas y la depuración en NestJS no solo sean necesarias, sino también manejables y eficientes.

Al centrarnos en la API REST que desarrollamos en el [Capítulo 5](https://subscription.packtpub.com/book/web-development/9781835468609/5), revisaremos el sistema de gestión de usuarios, aplicando rigurosas estrategias de prueba a cada componente. Aprenderá a escribir y ejecutar tanto pruebas unitarias como de integración, asegurando que cada parte de su API se verifique a fondo. Además, exploraremos técnicas efectivas de depuración para identificar y resolver problemas dentro de sus APIs REST rápidamente. Este enfoque práctico no solo consolida su comprensión de las capacidades de prueba de NestJS, sino que también refuerza las mejores prácticas en el desarrollo de APIs.

Las pruebas no consisten únicamente en encontrar errores; son un enfoque proactivo para el aseguramiento de la calidad del software. Al final de este capítulo, estará bien equipado para implementar un marco de pruebas robusto para sus aplicaciones NestJS, mejorando la fiabilidad y la confianza en sus APIs.

En este capítulo, vamos a cubrir los siguientes temas principales:

- Comprender la importancia de las pruebas
- Pruebas unitarias en NestJS
- Pruebas de integración en NestJS
- Pruebas de guards, interceptores y middleware
- Depuración de APIs REST

Al dominar estas áreas, podrá garantizar la funcionalidad y la fiabilidad de sus APIs, así como adoptar un flujo de trabajo que priorice la calidad y la eficiencia desde el inicio del proceso de desarrollo.

---

### Requisitos técnicos

Dado que este es un capítulo práctico, asegúrese de tener la configuración requerida antes de continuar. Los elementos más importantes que debe tener configurados son los siguientes:

- Node.js, NestJS y Nest-CLI instalados en su computadora
- La aplicación de gestión de usuarios del [Capítulo 5](https://subscription.packtpub.com/book/web-development/9781835468609/5) configurada y en ejecución
- Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Comprender la importancia de las pruebas

Imagine construir una hermosa pajarera, adornada con detalles intrincados. En circunstancias normales de temperatura y clima, todo se ve perfecto y se siente orgulloso de sí mismo. La pajarera, que se construyó en la época de verano, parece satisfacer sus necesidades. Si no ha probado su resistencia contra el viento y la lluvia, la primera tormenta podría derribarla, dejando a sus amigos emplumados expuestos y sus esfuerzos desperdiciados.

El mismo principio se aplica a la construcción de software, especialmente a sistemas intrincados como las APIs REST de NestJS. Las pruebas no son un lujo; son la base sólida que garantiza que su API funcione correctamente bajo diversas condiciones, protegiendo a sus usuarios y su reputación.

#### El doloroso coste de omitir las pruebas

Muchos desarrolladores conocen el dolor de omitir las pruebas, solo para encontrar errores en entornos de producción. Imagine estos escenarios:

- Una función crítica de inicio de sesión falla, impidiendo que los usuarios accedan a su aplicación. La frustración aumenta, los tickets de soporte se acumulan y usted lucha por solucionar el problema, lo que puede afectar las operaciones comerciales.
- Una vulnerabilidad de seguridad pasa desapercibida, exponiendo datos confidenciales de los usuarios a los atacantes. Las consecuencias pueden ser inmensas, provocando pérdidas financieras, daños a la reputación e incluso repercusiones legales.
- Cambios simples en el código provocan comportamientos inesperados, rompiendo funcionalidades que antes funcionaban. La depuración se convierte en una pesadilla que consume mucho tiempo, ralentizando el progreso del desarrollo.

Estos son solo algunos ejemplos de los costes del mundo real asociados con descuidar las pruebas. Las pruebas sirven como un escudo proactivo, detectando problemas al principio del ciclo de desarrollo y ahorrándole tiempo, dinero y dolores de cabeza en el futuro.

#### El poder de las pruebas en NestJS

Afortunadamente, NestJS adopta las pruebas y le capacita con herramientas y bibliotecas integradas. Así es como las pruebas desbloquean su superpoder de desarrollo:

- **Detección temprana de errores:** Las pruebas unitarias y de integración actúan como centinelas, identificando problemas dentro de los componentes individuales y sus interacciones. Corrige los problemas antes de que se manifiesten en producción, evitando desastres de cara al usuario.
- **Mejora de la calidad del código:** Escribir pruebas le obliga a pensar críticamente sobre la estructura y la lógica de su código. Esto conduce a un código más limpio y fácil de mantener que es más sencillo de entender y modificar.
- **Mayor confianza:** Un conjunto de pruebas robusto proporciona tranquilidad. Cuando se realizan cambios, puede desplegar con confianza sabiendo que las funcionalidades principales de su API permanecen intactas.
- **Ciclos de desarrollo más rápidos:** Las pruebas automatizadas permiten bucles de retroalimentación rápidos, lo que le permite experimentar e iterar rápidamente sin temor a regresiones.
- **Bases más sólidas para la entrega ágil y continua:** Al integrar las pruebas en su proceso de desarrollo, garantiza una calidad continua a lo largo de todo el pipeline de entrega.

Piense en las pruebas no como una carga, sino como una inversión en la construcción de una API resiliente y fiable. Es el guardián silencioso que le permite escalar con confianza, sabiendo que su creación puede resistir cualquier tormenta.

En las próximas secciones, hablaremos sobre diferentes técnicas que podemos utilizar al probar una API REST construida en NestJS, asegurando que podamos escalar nuestra aplicación con confianza y que todo se pueda detectar antes de intentar salir a producción.

---

### Pruebas unitarias en NestJS

Las pruebas unitarias son un proceso para probar las partes más pequeñas de un sistema que manejan una determinada lógica de negocio en la aplicación y que pueden aislarse del sistema en sí.

Es fundamental comprender que contar con una clara separación de responsabilidades es clave para realizar mejores pruebas unitarias. Una función que gestiona la creación de usuarios, envía correos electrónicos y actualiza la gestión de sesiones al mismo tiempo puede dificultar enormemente el trabajo de un desarrollador a la hora de probarla, ya que es difícil aislarla del sistema. Esto se debe a que maneja múltiples lógicas a la vez.

Hablamos sobre el diseño modular de las aplicaciones NestJS en capítulos anteriores y construimos nuestra API REST base siguiendo este principio, de modo que podamos aislar los bloques de nuestra aplicación y probarlos.

En la filosofía de NestJS, toda la lógica de negocio debe manejarse en los archivos de servicio. En la mayoría de los casos, probar unitariamente el archivo de servicio es suficiente para cubrir las pruebas unitarias de la aplicación.

Ahora que sabemos qué son las pruebas unitarias, apliquémoslas a nuestra API REST existente.

#### Identificación de unidades en la aplicación existente

Antes de profundizar en las pruebas, identificaremos qué piezas de lógica se pueden probar por separado. Dado que la API REST es un sistema de gestión de usuarios simple, actualmente tenemos las siguientes unidades en la aplicación:

- **Funcionalidad para obtener todos los usuarios sin paginación:** Para la versión 1 de la API, tenemos una funcionalidad que nos permite obtener todos los usuarios. Esta lógica se puede probar por sí misma, ya que solo obtiene usuarios y devuelve valores para que el controlador pueda enviar una respuesta de API adecuada.
- **Obtener todos los usuarios con paginación:** Añadimos esta funcionalidad para la versión 2; también se puede probar como una unidad.
- **Crear un usuario:** Con un objeto `CreateUserDto`, deberíamos poder verificar que la creación del usuario funcione como se espera.
- **Actualizar un usuario:** Con un objeto `UpdateUserDto`, deberíamos poder actualizar un usuario.

Estas son las unidades de lógica de negocio que probaremos. Siéntase libre de añadir más, como eliminar un usuario, como ejercicio para llevar a casa.

#### Escritura de pruebas unitarias para el servicio de usuarios

Con la unidad de lógica identificada, podemos implementar pruebas unitarias y asegurarnos de que pasen todas las pruebas.

> [!NOTE]
> **Nota importante**
> En aplicaciones de la vida real, las pruebas unitarias pueden implicar más que simplemente probar unidades de código. Cuando la aplicación interactúa con una base de datos, pueden ser necesarias técnicas como la simulación (mocking) de la base de datos. Por ahora, simplemente probaremos estas funciones sin ninguna interacción real con la base de datos.

Ahora escribamos nuestras pruebas unitarias siguiendo estos sencillos pasos:

1. Cree un archivo `users.service.spec.ts` y luego pegue el siguiente contenido:

```typescript
// users.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { UsersService } from './users.service';

describe('UsersService', () => {
  let service: UsersService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [UsersService],
    }).compile();

    service = module.get<UsersService>(UsersService);
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });
});
```

Por defecto, el código anterior es el que obtenemos cuando generamos un recurso utilizando el CLI de Nest.

Echemos un vistazo más de cerca al fragmento de código anterior:

- **Configuración del módulo de prueba:** El método `Test.createTestingModule` crea un módulo específicamente para pruebas. Este módulo imita la configuración que tendría en su aplicación, pero está diseñado para fines de prueba. Al proporcionar el `UsersService` en el array `providers`, está haciendo que una instancia de `UsersService` esté disponible para la Inyección de Dependencias (DI), al igual que en el módulo de aplicación real. Si el `UsersModule` tuviera más de una dependencia, deberían añadirse de la misma manera que añadimos el `UsersService` anteriormente.
- **Hook:** Estamos utilizando el hook `beforeEach` para crear un nuevo módulo de prueba antes de cada caso de prueba.
- **Instanciación del servicio:** Después de compilar el módulo, la instancia del servicio se recupera utilizando `module.get<UsersService>(UsersService)`. Este método obtiene la instancia de `UsersService` del contenedor de DI de NestJS, asegurando que el servicio, junto con sus dependencias (si las tuviera), se instancie correctamente de acuerdo con la configuración definida en el módulo de prueba.
- **Expectativa de definición:** El caso de prueba `'should be defined'` verifica que la instancia del servicio se cree correctamente y no sea `undefined`. Esta es una comprobación básica de sanidad que garantiza que el servicio esté correctamente instanciado y pueda utilizarse para pruebas posteriores. Es un punto de partida para pruebas más detalladas que verificarían la funcionalidad del servicio.

Este es un archivo de prueba muy básico y, como puede ver, todas las funcionalidades que pretendíamos probar aún no han sido probadas.

Ahora, agreguemos más pruebas para nuestro archivo de servicio comenzando con la creación de un usuario. En el archivo `users.service.spec.ts`, agregue el siguiente caso de prueba:

```typescript
// create a const of the user we will create
const newUser: CreateUserDto = {
  name: 'Kitoko Mwana',
  email: 'kitoko@test.com',
  address: '1234, Lubumbashi, DRC',
};

// then add one more test case to make sure our
// service logic works
it('should create a new user', () => {
  const user = service.createUser(newUser);
  expect(user).toBeDefined();
  expect(user.name).toBe(newUser.name);
  expect(user.email).toBe(newUser.email);
  expect(user.id).toBe(5); // new user id
});
```

Al probar una API, normalmente necesitamos operar con datos que ya conocemos. En este caso, el objeto `newUser` se conforma a partir del `CreateUserDto` y conocemos todas sus propiedades.

Luego llamamos al método `createUser` de nuestro servicio y verificamos que funcione como se espera. Verificamos que el nombre se haya guardado como debería. También comprobamos que el email y el id se hayan añadido con éxito. Sabiendo que nuestro array inicial de usuarios mock tiene cuatro usuarios, la acción de crear usuario aquí normalmente incrementará y lo convertirá en cinco, siendo nuestro último usuario añadido el quinto.

Ahora, comprobemos que las pruebas pasen usando el comando `$ yarn test`. Compruebe la salida y asegúrese de que se vea como en la Figura 7.1 a continuación:

*Figura 7.1: Salida de las pruebas unitarias*

La Figura 7.1 muestra que tenemos dos suites de prueba – normalmente igual al número de archivos de prueba que tenemos – y tres pruebas dentro de esos dos archivos. Por lo que podemos ver, todas pasan.

Probar bloques unitarios de una aplicación consiste en verificar suposiciones que ya conocemos, así como casos extremos (edge cases). Por ejemplo, cuando obtenemos usuarios, en la primera versión de la API, solo devolvemos un array de usuarios sin paginación. En la segunda versión, tenemos una funcionalidad de paginación. ¿Qué sucede si intentamos obtener una lista de usuarios en una página que no existe? Por ejemplo, ¿qué sucede si enviamos una consulta con `page = 100` mientras la base de datos solo tiene cuatro usuarios? Para asegurarnos de que nuestra aplicación envíe la información correcta, escribamos pruebas relacionadas con las funcionalidades `findAll` de usuarios tanto de v1 como de v2, con el siguiente código:

```typescript
it('should return an array of users', async () => {
  const users = await service.findAll();
  expect(users.length).toBeGreaterThan(0);
  expect(users[0].name).toBeDefined();
  expect(users[0].email).toBe('tshim@myapp.com');
  expect(users[1].address).not.toBeDefined();
});

// findall v2
it('should return an array of users with pagination', () => {
  const users = service.findAllV2(1, 2);
  expect(users.data.length).toBe(2);
  expect(users.data[0].name).toBeDefined();
  expect(users.meta).not.toBeNull();
  expect(users.meta.page).toBe(1);
  expect(users.meta.limit).toBe(2);
  expect(users.meta.total).toBe(4);

  // bad request with a wrong page number
  const users2 = service.findAllV2(100, 2);
  expect(users2.data.length).toBe(0); // no data
  expect(users2.meta).not.toBeNull(); // still return
  // meta
});
```

En los casos de prueba anteriores, estamos comprobando que la lista de usuarios simulados tenga una longitud mayor que 0, que el primer usuario tenga un correo electrónico que ya conocemos, que el usuario en el índice 1 no tenga un campo de dirección (usuario v1), y así sucesivamente.

El segundo caso de prueba trata sobre probar la funcionalidad v2. Nos aseguramos de que devuelva un objeto meta y probamos una solicitud incorrecta cuando un cliente solicita una página que no existe.

Ahora bien, ¿qué sucede cuando tenemos excepciones que deben desencadenarse desde un archivo de servicio? Recuerde que en nuestro archivo de servicio, lanzamos un error cuando un usuario intenta actualizar un usuario con un ID incorrecto.

Aquí está el código, a modo de recordatorio:

```typescript
// users.service.ts
// update user feature
updateUser(user: UpdateUserDto & { id: number }) {
  const index = this.users.findIndex(
    (u) => u.id === user.id
  );
  if (index === -1) {
    throw new Error('User not found');
  }
  this.users[index] = {
    name: user.name ?? this.users[index].name,
    email: user.email ?? this.users[index].email,
    id: this.users[index].id,
  };
  return this.users[index];
}
```

El fragmento de código anterior actualiza la información del usuario según su id, y cuando el usuario con el id especificado aún no existe, lanzamos un error.

Probar este comportamiento puede ser un poco complejo. Dado que somos conscientes de que esto va a suceder, necesitamos capturar esta excepción en el archivo de prueba o la prueba fallará. Una técnica utilizada a menudo es añadir un bloque `try catch` en el código y verificar que el objeto de error exista con el mensaje correcto.

Así es como puede probarlo:

```typescript
// users.service.spec.ts
it('should update a user', () => {
  const user = service.updateUser({
    name: 'Tshimanga MUKENDI John',
    id: 1,
  });
  expect(user).toBeDefined();
  expect(user.name).toBe(
    'Tshimanga MUKENDI John'
  ); // name has been updated
  expect(user.email).toBe(
    'tshim@myapp.com'
  ); // email remains the same
  try {
    // should throw an error if user is not found
    service.updateUser({
      name: 'Tshimanga MUKENDI John',
      id: 100,
    });
  } catch (error) {
    expect(error).toBeDefined();
    expect(error.message).toBe('User not found');
  }
});
```

Perfecto, esto se ve bien. Después de probar el comportamiento normal, hemos añadido un bloque `try catch` para que podamos afirmar que el error existe y que devuelve un mensaje de error correcto.

¡Bien hecho! Hemos cubierto una buena cantidad de pruebas. Como ejercicio para llevar a casa, intente añadir una funcionalidad `deleteUser` y sus casos de prueba correspondientes.

Ahora, ejecutemos las pruebas una vez más y verifiquemos que todo se comporte como se espera. La Figura 7.2 ilustra cuál debería ser la salida después de ejecutar `$ yarn test` una vez más:

*Figura 7.2: Pruebas unitarias del servicio de usuarios*

Ahora, ¿cómo puedo decir con confianza que las pruebas que escribí son suficientes en el contexto de mi aplicación? Cuando su base de código crece, puede ser un desafío recordar todo lo que debe probarse o si ya se ha probado. Necesitamos encontrar un mecanismo que nos ayude a determinar si las pruebas cubren la mayoría de nuestras funciones, bloques, declaraciones, etc. Para hacerlo, necesitamos medir la cobertura de pruebas.

#### Cobertura de pruebas

NestJS es muy potente. Tiene soporte de cobertura de pruebas por defecto e incluye cada archivo TypeScript y JavaScript que tenemos en el directorio de trabajo.

Antes de comenzar a medir la cobertura real, especifiquemos qué archivos deben tenerse en cuenta al informar la cobertura de las pruebas. Archivos como `.dto`, `.entity`, constantes, etc., no son relevantes para ser considerados a menos que tengamos alguna lógica compleja dentro de ellos.

Por ahora, vayamos al archivo `package.json`. Debajo de la propiedad `jest`, actualicemos el array `collectCoverageFrom` con lo siguiente:

```json
{
  "collectCoverageFrom": [
    "**/*.(t|j)s",
    "!main.(t|j)s",
    "!**/*.module.(t|j)s",
    "!**/*.dto.(t|j)s",
    "!**/*.entity.(t|j)s",
    "!**/*.args.(t|j)s",
    "!**/*.types.(t|j)s",
    "!**/node_modules/**"
  ]
}
// everything else remains the same
```

Con esta configuración, le informamos a Jest que solo recopile cobertura de archivos `.js` o `.ts` que no sigan los patrones indicados en la lista anterior con una negación.

Para obtener la cobertura, ejecute `$ yarn test:cov`. La salida debería verse como en la Figura 7.3:

*Figura 7.3: Informe de cobertura de pruebas después de añadir algunas pruebas unitarias*

Como podemos ver, el archivo `user.service` está casi completamente probado, pero nuestro controlador apenas está probado. Esto es normal ya que solo probamos las unidades en el archivo de servicio. Sin embargo, ¿qué podemos hacer para asegurarnos de que nuestros controladores, middleware, guards, etc., estén cubiertos por las pruebas?

La respuesta es añadir pruebas de integración. Recuerde que nuestros controladores son responsables de manejar las solicitudes HTTP entrantes y enviar las respuestas adecuadas al cliente. Esto es exactamente lo que harán las pruebas de integración.

La pregunta que nos haremos aquí será: cuando un usuario realiza una determinada solicitud HTTP, ¿qué debe enviar el servidor como respuesta? Para responder a esta pregunta, saltemos a la siguiente sección y exploremos las pruebas de integración en NestJS.

---

### Pruebas de integración en NestJS

En el ámbito del desarrollo de software, particularmente dentro del robusto framework NestJS, las pruebas de integración emergen como una práctica fundamental. Son indispensables para garantizar que los diversos componentes de su aplicación no solo funcionen correctamente por sí mismos, sino que también interactúen a la perfección.

Esta forma de prueba trasciende los límites de las pruebas unitarias, embarcándose en la misión de validar la armonía entre servicios, módulos e integraciones externas. Se trata de pintar una imagen integral del comportamiento de su aplicación en escenarios que reflejan el uso del mundo real.

A medida que giramos desde los fundamentos teóricos de las pruebas de integración hacia sus aplicaciones prácticas, es crucial sentar las bases adecuadamente. La fase de preparación no se trata solo de seleccionar las herramientas adecuadas; se trata de crear un entorno que simule con precisión la configuración de producción, permitiendo un examen meticuloso de cómo se unen las diferentes partes de su aplicación.

#### Preparación para las pruebas de integración

Para embarcarse en las pruebas de integración con NestJS, particularmente para nuestra aplicación de gestión de usuarios, es esencial sentar primero una base sólida. Este proceso comienza con la configuración de un entorno de pruebas que no solo admita sino que también mejore nuestra capacidad para simular con precisión escenarios del mundo real. NestJS, reconocido por su completo conjunto de utilidades de prueba, nos proporciona Jest para aserciones y Supertest para simulaciones de solicitudes HTTP, creando un punto de partida ideal para nuestras pruebas de integración.

Para aquellos que se adentran en un proyecto NestJS existente, encontrarán un archivo de prueba End-to-End (e2e) básico ubicado bajo el directorio `test/`, llamado `app.e2e-spec.ts`. Este archivo es un testimonio de la preparación del framework para las pruebas de integración, ofreciendo una estructura predefinida que podemos ampliar y adaptar a nuestras necesidades de prueba específicas.

Esto es lo que NestJS nos da inicialmente:

```typescript
// Initial test/app.e2e-spec.ts content
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from './../src/app.module';

describe('AppController (e2e)', () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test
      .createTestingModule({
        imports: [AppModule],
      }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  it('/ (GET)', () => {
    return request(app.getHttpServer())
      .get('/')
      .expect(200)
      .expect('Hello World!');
  });
});
```

Si bien este código sigue siendo correcto para el proyecto inicial, ejecutar esta prueba con `yarn test:e2e` podría arrojar un fallo, especialmente si su aplicación, como la nuestra, ha implementado soporte para versionado.

Si bien entendemos que hemos actualizado la forma en que nuestra API envía respuestas al servidor, es vital recordar que tenemos un `response-logging.interceptor` que intercepta todas las solicitudes y envía una respuesta con la siguiente forma:

```javascript
// response-logging.inerceptor.js
return {
  success: !(data instanceof Error) && data !== null,
  data: data,
  timestamp: new Date().toISOString(),
};
```

El entorno de prueba debe ser consciente de este interceptor y estar configurado para manejar las versiones correctamente.

Para alinear nuestro entorno de prueba con el estado actual de la aplicación —particularmente la característica de versionado— se requieren modificaciones en la configuración de la prueba e2e inicial. Esto implica configurar el `AppModule` dentro de nuestras pruebas e2e para reconocer y enrutar adecuadamente las solicitudes versionadas.

A continuación se muestra cómo puede adaptar la configuración inicial para dar cabida al versionado:

```typescript
beforeEach(async () => {
  const moduleFixture: TestingModule = await Test
    .createTestingModule({
      imports: [AppModule],
    }).compile();

  app = moduleFixture.createNestApplication();
  app.enableVersioning({
    type: VersioningType.URI,
  });
  await app.init();
});
```

Al invocar `app.enableVersioning()` y especificar el tipo de versionado, nos aseguramos de que nuestras pruebas de integración consideren la estrategia de versionado implementada en nuestra aplicación. Este ajuste cierra la brecha entre nuestro entorno de prueba y el comportamiento real de la aplicación, preparando el escenario para pruebas de integración más matizadas y precisas.

Sin embargo, esto no es suficiente. Por lo tanto, actualicemos ahora nuestras suposiciones. Conocemos la forma de la respuesta que siempre provendrá del servidor. Actualice el caso de prueba `/ (GET)` con el siguiente código:

```typescript
it('/ (GET)', () => {
  return request(app.getHttpServer())
    .get('/v1')
    .expect(200)
    .expect((res) => {
      expect(res.body.success).toBe(true);
      expect(res.body.data).toBe('Hello World!');
      expect(res.body.timestamp).toBeDefined();
    });
});
```

En el código anterior, probamos las suposiciones de que el tipo de respuesta que estamos obteniendo ya no es una cadena sino un objeto con los campos apropiados: `success` de tipo Boolean, el objeto `data` y `timestamp`.

Con estos preparativos en su lugar, ahora estamos preparados para profundizar en las complejidades de las pruebas de integración. Nuestra aplicación de gestión de usuarios, equipada con un entorno de prueba debidamente configurado, está lista para someterse a una serie de pruebas diseñadas para validar integralmente sus funcionalidades integradas. Esta preparación marca un hito importante en nuestro camino hacia garantizar la fiabilidad y robustez de nuestra aplicación NestJS mediante rigurosas pruebas de integración.

Antes de continuar, ejecute el comando `test:e2e` una vez más y asegúrese de que no obtengamos ningún error.

#### Descripción general de la aplicación de gestión de usuarios

En el camino para garantizar que nuestro sistema de gestión de usuarios construido con NestJS sea robusto y se comporte como se espera en varios escenarios, las pruebas de integración juegan un papel fundamental. Es aquí donde simulamos interacciones del mundo real con nuestra API, profundizando en cómo responden nuestros endpoints a diferentes solicitudes. Manos a la obra y entremos en los pasos prácticos para probar los endpoints HTTP de nuestro sistema de gestión de usuarios.

Al organizar las pruebas en NestJS, adherirse a una estructura consistente es clave para la mantenibilidad y la legibilidad. Para nuestro módulo de usuarios, las pruebas de integración residen en `test/users/users.e2e-spec.ts`. Esta convención ayuda a identificar los archivos de prueba de inmediato y se alinea con la arquitectura modular de las aplicaciones NestJS.

#### Preparación del entorno de prueba

Antes de comenzar a crear nuestras pruebas, asegúrese de que el entorno de pruebas esté configurado correctamente. En el archivo `test/users/users.e2e-spec.ts`, comenzamos importando los módulos necesarios y configurando el contexto de la aplicación Nest para incluir el versionado, lo cual es crucial para simular con precisión el comportamiento de nuestra API.

Aquí está cómo:

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication, VersioningType } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from '../../src/app.module';

describe('User Management (e2e)', () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test
      .createTestingModule({
        imports: [AppModule],
      }).compile();

    app = moduleFixture.createNestApplication();
    app.enableVersioning({
      type: VersioningType.URI,
      defaultVersion: '1',
    });
    await app.init();
  });

  afterEach(async () => {
    await app.close();
  });

  // Test cases will be added here
});
```

En el fragmento de código proporcionado, hemos configurado nuestro archivo de prueba e2e de usuario configurándolo con el `AppModule` correcto —específicamente, un módulo de prueba en lugar del módulo de aplicación real. También hemos introducido un hook `afterEach`. Este paso es crucial ya que garantiza que la aplicación se cierre después de cada caso de prueba, evitando que las pruebas e2e interfieran entre sí en sus resultados o comportamiento. Esta práctica es importante porque garantiza que cada módulo se pruebe de forma aislada, una necesidad dado el orden impredecible en el que podrían ejecutarse las pruebas.

#### Simulación de solicitudes HTTP y aserción de respuestas

Nuestro enfoque se centrará en operaciones clave dentro de nuestro sistema de gestión de usuarios: crear usuarios, recuperar listas de usuarios (con y sin paginación) y actualizar la información de los usuarios. A través de estas pruebas, pretendemos cubrir una amplia gama de escenarios que se espera que maneje nuestra API.

##### Probar el endpoint POST /v1/users

Esta prueba simula la creación de un nuevo usuario, asegurando que la operación sea exitosa y que la respuesta incluya los detalles de usuario esperados:

```typescript
it('POST /v1/users - should create a new user', async () => {
  const response = await request(app.getHttpServer())
    .post('/v1/users')
    .send({ name: 'Justin Dusenge', email: 'justin@example.com' })
    .expect(201);

  expect(response.body).toHaveProperty('data');
  expect(response.body.data.name)
    .toEqual('Justin Dusenge');
  expect(response.body.data.email)
    .toEqual('justin@example.com');
});
```

Si bien esta prueba parece correcta, cuando ejecutamos el comando `test:e2e` nuevamente, obtenemos una respuesta fallida que se parece a la Figura 7.4:

*Figura 7.4: Error tras ejecutar yarn test:e2e con el caso de prueba de crear usuario añadido*

Hmm, parece que no estamos autorizados a realizar esta operación; no estamos autorizados en este momento. ¿Tiene alguna idea de por qué? Déjeme decirle: recuerde que tenemos un `AuthGuard` que simulamos y que se parece al siguiente fragmento de código:

```typescript
@Injectable()
export class MockAuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();
    // Simple check for a mock token
    return request.headers['authorization'] === 'Bearer mock-token';
  }
}
```

Este guard es solo un mock, sin una lógica adecuada, y espera un token Bearer de autorización con la cadena `mock-token` como valor.

Para simular un encabezado en el archivo de prueba, debemos configurarlo antes de realizar la solicitud. Añadimos la siguiente línea de código:

```typescript
const response = await request(app.getHttpServer())
  .post('/v1/users')
  // add the mock token to the request object as a header
  .set('Authorization', 'Bearer mock-token')
  .send({ name: 'Justin Dusenge', email: 'justin@example.com' })
  .expect(201);
```

El fragmento de código anterior se parece casi a lo que teníamos antes, con una ligera diferencia: añadir el encabezado `Authorization` al objeto de solicitud.

Ahora actualicemos el caso de prueba de creación de usuario con un conjunto completo de expectativas con el siguiente fragmento de código:

```typescript
it('POST /v1/users - should create a new user', async () => {
  const response = await request(app.getHttpServer())
    .post('/v1/users')
    // add the mock token to the request object as a
    // header
    .set('Authorization', 'Bearer mock-token')
    .send({ name: 'Justin Dusenge', email: 'justin@example.com' })
    .expect(201);

  const { data, success, timestamp } = response.body;
  expect(data).toHaveProperty('id');
  expect(data.name).toEqual('Justin Dusenge');
  expect(data.email).toEqual('justin@example.com');
  expect(success).toBe(true);
  expect(timestamp).toBeDefined();
});
```

Con estas expectativas añadidas, probemos nuestro proyecto una vez más. Puede especificar el archivo que desea probar añadiendo el nombre del archivo al final del comando. En este caso, puede usar `yarn test:e2e users.e2e-spec.ts` y ver que las pruebas ahora pasan.

¡Bien hecho! Ha podido probar el escenario de creación de usuarios. Ahora, avancemos con los de obtención de usuarios.

##### Probar los endpoints GET /v1/users y /v2/users

Estas pruebas verifican la capacidad de nuestra aplicación para devolver los datos de usuario correctos, incorporando la lógica de paginación en la versión 2.

> [!TIP]
> **Ejercicio práctico**
> Dado que estas pruebas son particularmente simples de probar, inténtelo usted mismo antes de ver el resultado que sigue.

Aquí tenemos el código completo que prueba la funcionalidad de obtención de usuarios:

```typescript
it('GET /v1/users - should retrieve all users', async () => {
  const response = await request(app.getHttpServer())
    .get('/v1/users')
    .expect(200);

  const { data, success, timestamp } = response.body;
  expect(Array.isArray(data)).toBeTruthy();
  expect(data[0]).toHaveProperty('name');
  expect(success).toBe(true);
  expect(timestamp).toBeDefined();

  // negative test
  // version 1 does not have address
  expect(data[0]).not.toHaveProperty('address');
  // version 1 does not have pagination
  expect(data).not.toHaveProperty('meta');
});

it('GET /v2/users - should retrieve paginated users', async () => {
  const response = await request(app.getHttpServer())
    .get('/v2/users?page=1&limit=2')
    .expect(200);

  const { data, success } = response.body;
  expect(data.data.length).toBeGreaterThan(2);
  expect(data).toHaveProperty('meta');
  expect(data.meta).toHaveProperty('total');
  // the default limit is 10 and the default page is 1
  expect(data.meta.limit).toBe(10);
  expect(data.meta.page).toBe(1);
  expect(success).toBe(true);
});
```

Puede añadir tantas suposiciones como necesite, solo asegúrese de que ejecutar la prueba una vez más le dé un resultado exitoso.

Los casos de prueba anteriores prueban diferentes suposiciones dependiendo de la versión en la que estemos obteniendo los usuarios. Estas suposiciones incluyen esperar el campo `meta` en el objeto `data` en la solicitud v2, esperar los campos predeterminados correctos `page`, `limit` y `total` en `meta`, y así sucesivamente.

> [!TIP]
> **Ejercicio práctico**
> Añada más pruebas y asegúrese de probar todos los posibles casos de uso restantes, así como las funcionalidades de actualización y eliminación de usuarios. Siempre puede encontrar una solución en los recursos adjuntos del libro o en el repositorio de GitHub del libro en: [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch07](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch07)

Al construir meticulosamente estas pruebas, no solo validamos los comportamientos individuales de los endpoints, sino que también ganamos confianza en la funcionalidad integrada de nuestro sistema de gestión de usuarios. Esta serie de pruebas resume un amplio espectro de operaciones, sentando una base sólida sobre la cual se pueden construir pruebas adicionales para abarcar más características y escenarios a medida que nuestra aplicación evoluciona.

#### Probar archivos de controladores

En el camino de crear APIs REST robustas y fiables con NestJS, las pruebas juegan un papel fundamental. Si bien las pruebas e2e proporcionan una amplia red de seguridad, asegurando que su aplicación se comporte correctamente como un todo, existe una capa crítica de pruebas que se enfoca más estrechamente en la interacción entre las solicitudes entrantes y sus controladores: probar los archivos de controlador directamente. Este enfoque es particularmente beneficioso para verificar el comportamiento de sus controladores de forma aislada, asegurando que manejen adecuadamente las solicitudes y deleguen la lógica de negocio a los servicios.

##### ¿Por qué probar los controladores por separado?

Los controladores son el punto de entrada a la lógica de negocio de su aplicación. Interpretan las entradas del usuario y determinan qué métodos de servicio invocar. Si bien las pruebas e2e validan el sistema, las pruebas de controladores le permiten hacer lo siguiente:

- **Aislar la lógica del controlador:** Verificar que los controladores estén analizando correctamente las solicitudes, aplicando la validación y llamando a los métodos de servicio adecuados.
- **Simular dependencias (mocking):** Al simular llamadas de servicio, puede probar los controladores sin depender de la implementación real de los servicios, centrándose únicamente en el manejo de solicitudes y respuestas por parte del controlador.
- **Asegurar respuestas HTTP correctas:** Confirmar que para cada endpoint de la API, su controlador responda con los códigos de estado, encabezados y cuerpo correctos.

##### Preparación para las pruebas de controladores

Dado nuestro sistema de gestión de usuarios desarrollado en el [Capítulo 5](https://subscription.packtpub.com/book/web-development/9781835468609/5), centrémonos en probar el `UsersController`. El controlador incluye métodos como `findAll()`, `createUser()` y `updateUser()`, cada uno correspondiente a diferentes endpoints HTTP.

Para estas pruebas, utilizaremos Jest junto con las utilidades de prueba de NestJS para simular las dependencias de `UsersService`, lo que nos permitirá simular diferentes resultados de lógica de negocio y verificar las respuestas del controlador.

Primero, asegúrese de que su entorno de prueba esté configurado correctamente para incluir el módulo `Test` de NestJS, que permite una fácil simulación y prueba de controladores.

##### Ejemplo: probar el endpoint createUser

Probemos el método `createUser` en nuestro `UsersController`. Este método es responsable de crear un nuevo usuario, lo que requiere una autorización adecuada y validación del cuerpo:

```typescript
describe('UsersController', () => {
  let usersController: UsersController;
  let mockUsersService: Partial<UsersService>;

  beforeEach(async () => {
    // Mock the UsersService methods used by the
    // UsersController
    mockUsersService = {
      createUser: jest.fn((dto) => ({ ...dto, id: Date.now() })),
      // Add other methods as necessary
    };

    const module: TestingModule = await Test.createTestingModule({
      controllers: [UsersController],
      providers: [
        {
          provide: UsersService,
          useValue: mockUsersService,
        },
      ],
    }).compile();

    usersController = module.get<UsersController>(
      UsersController
    );
  });

  it('should call UsersService to create a user', () => {
    const createUserDto: CreateUserDto = {
      name: 'John Doe',
      email: 'john@example.com',
    };

    const user = usersController.createUser(createUserDto);

    expect(
      mockUsersService.createUser
    ).toHaveBeenCalledWith(
      createUserDto
    );
    expect(user).toEqual({
      ...createUserDto,
      id: expect.any(Number),
    });
    expect(user.id).toBeDefined();
  });
});
```

En la prueba anterior, nos centramos en garantizar que el método `createUser` en nuestro controlador invoque correctamente el método `createUser` de nuestro `UsersService` simulado con el DTO esperado. Esta prueba valida que el controlador delega correctamente la lógica de creación a la capa de servicio.

Para tener acceso a los métodos del servicio, hemos simulado el método `createUser` utilizando la función `fn` de Jest, dándole el comportamiento esperado y devolviendo un objeto similar al que devuelve la función real.

##### Ejercicio práctico: ampliación de las pruebas de controladores

Siguiendo el ejemplo proporcionado, amplíe sus pruebas para cubrir otros métodos del controlador como `findAll()` y `updateUser()`. Considere escenarios como verificar que `findAll()` recupera correctamente todos los usuarios sin necesidad de autorización o probar `updateUser()` para asegurarse de que requiera autorización y pase correctamente el DTO y el ID a la capa de servicio.

Recuerde simular los métodos del servicio y verificar que el controlador los llame con los parámetros correctos. Además, verifique que los métodos de su controlador devuelvan las respuestas HTTP esperadas, alineándose con los estándares RESTful establecidos para su API. Verifique su solución con el código en el repositorio de GitHub.

Al probar metódicamente sus controladores, se asegura de que cada parte de su aplicación interprete correctamente las solicitudes de los usuarios y organice el flujo de datos a través de sus servicios. Esta capa de pruebas es crucial para identificar y solucionar problemas en las primeras etapas del proceso de desarrollo, lo que da como resultado un código más fiable y fácil de mantener.

A medida que se sienta más cómodo probando controladores, descubrirá que es una práctica invaluable en su flujo de trabajo de desarrollo, que complementa sus pruebas e2e y proporciona una comprensión más profunda de cómo su aplicación maneja las solicitudes entrantes.

Esto no es suficiente; algunas partes de nuestros proyectos aún no se han probado. Al ejecutar el comando `test:cov`, notará que archivos como guards, middleware e interceptores aún no están cubiertos. Abordemos esto en la siguiente sección.

---

### Pruebas de guards, interceptores y middleware

En el complejo ecosistema de una aplicación NestJS, componentes como guards, interceptores y middleware desempeñan roles fundamentales en la gestión de la seguridad, la transformación de datos, el manejo de solicitudes/respuestas y el registro de operaciones (logging).

Estos elementos son los héroes anónimos que trabajan entre bastidores para garantizar que la aplicación funcione de manera segura, eficiente y fiable. Probar estos componentes individualmente es tan crucial como probar sus servicios o controladores. Garantiza que cada pieza de su aplicación no solo realice su función prevista de forma aislada, sino que también interactúe a la perfección dentro del contexto más amplio de la aplicación.

Los guards, por ejemplo, son su primera línea de defensa, determinando si una solicitud en particular debe procesarse o denegarse. Los interceptores ofrecen una forma poderosa de manipular solicitudes y respuestas, permitiendo el registro, la transformación o incluso el encapsulamiento de los valores devueltos. El middleware proporciona un mecanismo para inspeccionar y modificar solicitudes y respuestas, o para ejecutar cualquier código antes de que se llamen a las acciones de su controlador. Dados sus roles críticos, cualquier descuido en su comportamiento puede provocar vulnerabilidades de seguridad, inconsistencias de datos o un comportamiento inesperado de la aplicación.

En esta sección, nos embarcaremos en un viaje para probar rigurosamente estos componentes, asegurando que mantengan los estándares de seguridad, integridad y rendimiento de la aplicación. Probaremos lo siguiente:

- El `MockAuthGuard` simula la autenticación comprobando la presencia de un token simulado específico en los encabezados de la solicitud. Probar este guard asegurará que nuestro mecanismo de seguridad simulado se comporte como se espera.
- El `ResponseLoggingInterceptor` registra los detalles de la solicitud y el tiempo de ejecución, y modifica la estructura de la respuesta. Probar este interceptor asegura que nuestra lógica de registro sea correcta y que transforme correctamente los datos de la respuesta.
- El `VersionManagementMiddleware` maneja el versionado de la API modificando la URL de la solicitud según la versión especificada. Probar este middleware confirma que interpreta y modifica correctamente las rutas de solicitud para enrutar a la versión adecuada de un endpoint.
- El `LoggingMiddleware` registra los detalles de cada solicitud entrante. Probar este middleware garantiza que nuestra aplicación pueda registrar con precisión la información de las solicitudes con fines de monitoreo o depuración.

Al centrarnos en estos componentes, pretendemos consolidar la base de nuestra aplicación, asegurando que cada solicitud esté debidamente autenticada, registrada y enrutada, y que cada respuesta se transforme y registre con precisión. Este enfoque meticuloso para probar guards, interceptores y middleware no solo mejora la fiabilidad y la seguridad de nuestra aplicación, sino que también ejemplifica un compromiso con el aseguramiento integral de la calidad.

#### Probar MockAuthGuard

El `MockAuthGuard` es una parte fundamental de la seguridad de su aplicación, otorgando acceso condicionalmente a varias partes de su aplicación según la presencia y validez de un token de autorización. Probar este guard garantiza que su lógica de autenticación sea sólida, evitando el acceso no autorizado y salvaguardando las operaciones confidenciales. Si bien este es solo un guard simulado, la prueba de un guard real se verá muy similar.

##### Paso 1: Configurar el entorno de prueba

Primero, debe crear un archivo de prueba para su guard, por ejemplo, `auth/mock-auth.guard.spec.ts`. Dentro de este archivo, simulará diferentes contextos de solicitud para probar el comportamiento del guard.

##### Paso 2: Escribir casos de prueba

Sus casos de prueba deben cubrir escenarios donde se debe permitir el acceso (token correcto) y denegarse (token incorrecto o sin token). Esto implica simular el `ExecutionContext` para simular diferentes encabezados de solicitud.

Aquí hay un desglose detallado de cómo podría implementar estas pruebas:

```typescript
import { Test } from '@nestjs/testing';
import { MockAuthGuard } from './mock-auth.guard'; // to implement
import { createMockExecutionContext } from '../common/helpers/mock-execution-context.helper';

describe('MockAuthGuard', () => {
  let guard: MockAuthGuard;

  beforeEach(async () => {
    const module = await Test.createTestingModule({
      providers: [MockAuthGuard],
    }).compile();

    guard = module.get<MockAuthGuard>(MockAuthGuard);
  });

  it('should allow access with correct token', () => {
    const context = createMockExecutionContext(
      'Bearer mock-token'
    );
    expect(guard.canActivate(context)).toBe(true);
  });

  it('should deny access with incorrect token', () => {
    const context = createMockExecutionContext(
      'Bearer incorrect-token'
    );
    expect(guard.canActivate(context)).toBe(false);
  });

  it('should deny access without token', () => {
    const context = createMockExecutionContext('');
    expect(guard.canActivate(context)).toBe(false);
  });
});
```

En el fragmento de código anterior, comprendamos los componentes clave:

- **Configuración del módulo de prueba:** Al utilizar `Test.createTestingModule`, crea un módulo de prueba de NestJS que imita el módulo de su aplicación, lo que le permite probar el guard en un entorno aislado.
- **Simular ExecutionContext:** La función `createMockExecutionContext` simula diferentes escenarios de solicitud manipulando los encabezados de la solicitud. Esta función devuelve un objeto que imita la estructura de `ExecutionContext`, lo que le permite probar cómo reacciona el guard a varios tokens. Necesitamos crear esta función auxiliar para que el archivo de prueba pueda funcionar sin errores (sección del paso 3).
- **Aserción del comportamiento del guard:** Cada caso de prueba utiliza aserciones `expect` para comprobar si el guard concede o deniega correctamente el acceso en función del token proporcionado en los encabezados de la solicitud.

##### Paso 3: Implementar createMockExecutionContext

Ahora, cree un nuevo archivo en `src/common/helper/mock-execution-context.helper.ts` con el siguiente código:

```typescript
import { ExecutionContext } from '@nestjs/common';

export function createMockExecutionContext(token: string): ExecutionContext {
  return {
    switchToHttp: () => ({
      getRequest: () => ({
        headers: {
          authorization: token,
        },
      }),
    }),
  } as ExecutionContext;
}
```

En esta función auxiliar, simulamos lo que parece un contexto de ejecución con los campos que necesitamos que explore el archivo del guard. En este caso, ese es el campo `authorization`.

Con todo en su lugar, probemos nuestro archivo de guard con el comando `yarn test mock-auth.guard.spec.ts` y observemos que todo se ve bien.

Probar el `MockAuthGuard` proporciona confianza en el mecanismo de autenticación de su aplicación, asegurando que solo las solicitudes autorizadas procedan a rutas protegidas. Este enfoque para probar guards se puede extender a otros guards en su aplicación, garantizando una cobertura y seguridad integrales.

A continuación, trabajemos en los casos de prueba del interceptor.

#### Probar el interceptor de respuestas

El `ResponseLoggingInterceptor` está diseñado para registrar los detalles de la solicitud y modificar la estructura de respuesta enviada de vuelta al cliente. Probar este interceptor implica verificar que registre correctamente los detalles de la solicitud y formatee las respuestas como se espera.

##### Paso 1: Configurar el entorno de prueba

Cree un archivo de prueba llamado `response-logging.interceptor.spec.ts`. Este archivo contendrá sus pruebas para el interceptor, centrándose en su capacidad para modificar la respuesta y realizar el registro.

##### Paso 2: Escribir casos de prueba

Para probar eficazmente el interceptor, necesita simular el contexto de ejecución del interceptor y verificar su comportamiento.

Aquí hay un ejemplo de cómo podría abordar esto:

```typescript
import { CallHandler, ExecutionContext } from '@nestjs/common';
import { Test } from '@nestjs/testing';
import { Observable, of } from 'rxjs';
import { ResponseLoggingInterceptor } from './response-logging.interceptor';

describe('ResponseLoggingInterceptor', () => {
  let interceptor: ResponseLoggingInterceptor;

  beforeEach(async () => {
    const module = await Test.createTestingModule({
      providers: [ResponseLoggingInterceptor],
    }).compile();

    interceptor = module.get<ResponseLoggingInterceptor>(
      ResponseLoggingInterceptor
    );
  });

  it('should log and format the response', done => {
    const mockExecutionContext: ExecutionContext = createMockExecutionContext();
    const callHandler: CallHandler = {
      // Simulate a handler returning an observable with
      // data
      handle: () => of({ data: 'test' }),
    };

    interceptor.intercept(mockExecutionContext, callHandler).subscribe(response => {
      expect(response).toHaveProperty('success', true);
      expect(response).toHaveProperty('data', { data: 'test' });
      expect(response).toHaveProperty('timestamp');
      done();
    });
  });
});
```

En el fragmento de código anterior, aquí están los componentes clave:

- **Configuración del módulo de prueba:** Al igual que al probar guards, se crea un módulo de prueba de NestJS que incluye el interceptor. Este entorno aislado permite realizar pruebas focalizadas.
- **Simulación de CallHandler y ExecutionContext:** La prueba simula los objetos `CallHandler` y `ExecutionContext`. `CallHandler` se simula para devolver un observable, imitando la respuesta de un controlador. `ExecutionContext` se puede simular para proporcionar los detalles de solicitud necesarios. Sin embargo, en esta prueba específica, sus detalles no son tan cruciales.
- **Verificación de la lógica del interceptor:** La prueba se suscribe al `Observable` devuelto por el método `intercept` del interceptor. Luego afirma que el interceptor añade las propiedades `success` y `timestamp` a la respuesta, asegurando que el interceptor formatee las respuestas correctamente.

Ahora, ejecute sus pruebas una vez más para asegurarse de que el interceptor se comporte como se espera. Verifique que registre los detalles de la solicitud y modifique la estructura de la respuesta adecuadamente.

Probar `ResponseLoggingInterceptor` garantiza que la estructura de respuesta de su aplicación tenga un formato coherente y que los tiempos de procesamiento de solicitudes se registren correctamente. Este patrón se puede aplicar a otros interceptores dentro de su aplicación, garantizando que su lógica personalizada de manipulación de respuestas funcione a la perfección.

Ahora, probemos nuestro middleware.

#### Probar middleware

El middleware en NestJS desempeña un papel crucial en el procesamiento de solicitudes antes de que lleguen a los controladores de ruta. Probar estos middlewares asegura que se comporten como se espera, como modificar objetos de solicitud, administrar la autenticación o registrar solicitudes. Para nuestro sistema de gestión de usuarios, nos centraremos en probar dos piezas específicas de middleware: `VersionManagementMiddleware` y `LoggingMiddleware`.

##### Probar VersionManagementMiddleware

`VersionManagementMiddleware` ajusta dinámicamente la URL de la solicitud según la versión de la API. Es esencial probarlo para garantizar que las solicitudes se enruten correctamente a la versión adecuada de nuestra API.

###### Configuración del entorno de prueba

Para probar middlewares, normalmente simula la ejecución del middleware dentro del ciclo de vida de una solicitud. A continuación, se muestra cómo puede configurar una prueba para `VersionManagementMiddleware`:

```typescript
import { VersionManagementMiddleware } from './version-management.middleware';

describe('VersionManagementMiddleware', () => {
  let middleware: VersionManagementMiddleware;

  beforeEach(() => {
    middleware = new VersionManagementMiddleware();
  });

  it('should prepend /v1 to requests without a version', () => {
    const req: any = {
      originalUrl: '/users',
    };
    const res: any = {};
    const next = jest.fn();

    middleware.use(req, res, next);

    expect(req.originalUrl).toBe('/v1/users');
    expect(next).toHaveBeenCalled();
  });

  it('should correct invalid version to the latest supported version', () => {
    const req: any = {
      originalUrl: '/v3/users',
    };
    const res: any = {};
    const next = jest.fn();

    middleware.use(req, res, next);

    expect(req.originalUrl).toBe('/v2/users');
    expect(next).toHaveBeenCalled();
  });
});
```

Estos son los componentes clave del fragmento de código anterior:

- **Instanciación del middleware:** Instancie el middleware directamente, ya que no depende de DI para su funcionalidad.
- **Simulación de solicitudes:** Simule objetos de solicitud (`req`) para probar cómo el middleware modifica el `originalUrl` en función de las reglas de versionado.
- **Simulación de la función next (Mocking):** Simule el callback `next` para verificar que se llame, lo que indica que el middleware procesa con éxito la solicitud y pasa el control al siguiente middleware o controlador de ruta.

Ahora, veamos cómo podemos probar el otro middleware que tenemos en el proyecto: `LoggingMiddleware`.

##### Probar LoggingMiddleware

`LoggingMiddleware` registra detalles sobre las solicitudes entrantes. Probar este middleware verifica que registre correctamente la información sin interferir con el procesamiento de solicitudes.

Así es como podemos escribir pruebas para él:

```typescript
import { LoggingMiddleware } from './logging.middleware';

describe('LoggingMiddleware', () => {
  let middleware: LoggingMiddleware;

  beforeEach(() => {
    middleware = new LoggingMiddleware();
  });

  it('should log request details', () => {
    console.log = jest.fn(); // Mock console.log
    const req: any = {
      method: 'GET',
      originalUrl: '/users',
    };
    const res: any = {};
    const next = jest.fn();

    middleware.use(req, res, next);

    expect(console.log).toHaveBeenCalledWith(
      expect.stringContaining('GET /users')
    );
    expect(next).toHaveBeenCalled();
  });
});
```

En el fragmento de código anterior, estos son los componentes clave:

- **Mocking de consola:** Simule `console.log` para verificar que el middleware registre los detalles esperados de la solicitud.
- **Verificación:** Asegúrese de que se llame a la función `next`, lo que indica que el registro no detiene el ciclo de vida de la solicitud.

Probar middlewares en NestJS se trata de garantizar que modifiquen correctamente los objetos de solicitud/respuesta y realicen los efectos secundarios esperados, como el registro. Al simular solicitudes y usar mocks para funciones como `next`, puede validar eficazmente el comportamiento del middleware. Estas pruebas ayudan a mantener la fiabilidad del pipeline de procesamiento de solicitudes de su aplicación, lo cual es crucial tanto para la funcionalidad como para la seguridad.

Muy bien, hemos probado casi todas las piezas de código importantes de nuestro proyecto. Ahora, presentemos un concepto muy importante, que es la depuración de proyectos NestJS, en la siguiente sección.

---

### Depuración de APIs REST

La depuración es una parte integral del proceso de desarrollo, especialmente cuando se trabaja con sistemas complejos como APIs REST en NestJS. Una estrategia de depuración bien pensada no solo ayuda a identificar rápidamente las causas fundamentales de los problemas, sino que también garantiza que las APIs sigan siendo fiables y de alto rendimiento. En esta sección, exploraremos varias técnicas y herramientas que pueden mejorar significativamente su eficiencia de depuración.

#### Comprender el problema

El primer paso en la depuración es comprender con precisión el problema. Esto implica recopilar la mayor cantidad de información posible sobre el problema, incluidos los mensajes de error, los registros de solicitudes y las condiciones bajo las cuales ocurre el problema. NestJS proporciona soporte integrado para el registro (logging) que se puede aprovechar para capturar información detallada sobre cada solicitud y su resultado.

Si bien NestJS tiene su mecanismo de registro, puede resultarle beneficioso implementar un registro personalizado para obtener información más detallada, especialmente para capturar los cuerpos de solicitudes y respuestas.

Aquí hay un ejemplo simple de cómo configurar un registrador personalizado en NestJS:

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response } from 'express';

@Injectable()
export class LoggingMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: Function) {
    console.log(`Incoming Request: ${req.method} ${req.url}`, {
      body: req.body,
    });

    res.on('finish', () => {
      console.log(`Outgoing Response: ${res.statusCode}`, {
        response: res,
      });
    });

    next();
  }
}
```

En el middleware anterior, registramos los detalles de las solicitudes entrantes y las respuestas salientes, lo que proporciona visibilidad de los datos que fluyen a través de la API.

Ahora veamos las diferentes estrategias de depuración que nos brinda el framework en las siguientes secciones.

#### Aprovechar las herramientas de depuración

Varias herramientas pueden ayudar en la depuración de aplicaciones NestJS:

- **NestJS CLI:** El NestJS CLI ofrece comandos como `nest info` para brindarle una descripción general del entorno NestJS, lo que puede ser útil para garantizar que todas las dependencias estén instaladas correctamente y actualizadas.
- **Depurador de Visual Studio Code:** El depurador integrado de VS Code es increíblemente potente para depurar aplicaciones Node.js, incluidos los proyectos NestJS. Al establecer puntos de interrupción (breakpoints) e inspeccionar variables en tiempo de ejecución, puede obtener información detallada sobre el comportamiento de la aplicación.
- **Postman e Insomnia:** Estos entornos de desarrollo de APIs son invaluables para probar manualmente los endpoints de la API y observar sus respuestas. Pueden simular diferentes tipos de solicitudes, encabezados y cuerpos, lo que facilita la replicación y el diagnóstico de problemas.

Esta no es la única forma en que podemos depurar nuestra aplicación NestJS. En la siguiente sección, conozcamos una alternativa nativa que nos brinda NestJS: el uso de variables de entorno.

#### Mejorar la depuración con variables de entorno

Establecer la variable de entorno `NEST_DEBUG` en `true` es una forma sencilla pero potente de desbloquear capacidades de registro adicionales dentro de su aplicación NestJS. Esto puede proporcionar detalles más granulares sobre la ejecución de la aplicación, especialmente en lo que respecta a la resolución de módulos, la configuración de middleware y los procesos de manejo de solicitudes.

Para habilitar esto, ajuste su script de inicio en `package.json` de la siguiente manera:

```json
"scripts": {
  "start:debug": "NEST_DEBUG=true node dist/main"
}
```

Ejecutar su aplicación con `npm run start:debug` o incluir `NEST_DEBUG=true` en su archivo de variables de entorno (`.env`) puede iluminar problemas que podrían no ser evidentes a través del registro estándar, especialmente durante la fase de inicio de la aplicación.

#### Aprovechar NestJS DevTools para una experiencia de desarrollo optimizada

El paquete `@nestjs/devtools-integration` introduce una capa de integración para usar NestJS con herramientas de desarrollo populares, mejorando el proceso de depuración. Este paquete facilita una experiencia de depuración más interactiva, lo que permite a los desarrolladores inspeccionar la estructura, las dependencias y las operaciones en tiempo de ejecución de la aplicación en tiempo real. Sin embargo, acceder al panel de DevTools requiere una tarifa de pago con siete días de prueba gratuita.

Instalarlo y configurarlo es sencillo. Siga los pasos indicados aquí:

1. Ejecute el siguiente comando:

```bash
yarn add @nestjs/devtools-integration
```

2. Luego, integre el `DevtoolsModule` en su aplicación añadiendo la siguiente importación en el `AppModule`:

```typescript
@Module({
  imports: [
    DevtoolsModule.register({
      http: process.env.NODE_ENV !== 'production',
    }),
    UsersModule,
  ],
  // the rest remains the same
```

Esta configuración garantiza que las DevTools solo se activen en un entorno de desarrollo, preservando el rendimiento en producción. Una vez habilitadas, tendrá acceso a un conjunto de funciones de depuración directamente dentro de su IDE o una interfaz web, según la herramienta de desarrollo que esté utilizando.

El panel de DevTools se ve como en la Figura 7.5. Esto muestra una representación simplificada de lo que ofrece el panel de DevTools, omitiendo funciones como la medición del rendimiento de la aplicación, la inspección de módulos y más:

*Figura 7.5: Grafo de la aplicación desde Nest DevTools*

Jugar con el panel le otorgará algunos superpoderes de NestJS, así que no dude en tomarse un tiempo para hacerlo. En las siguientes secciones, veamos cómo es realmente la depuración en escenarios de desarrollo del mundo real.

#### Escenario de depuración del mundo real revisitado

Teniendo en cuenta estas herramientas adicionales, revisemos la depuración del endpoint `/api/users`. Después de habilitar `NEST_DEBUG` e integrar NestJS DevTools, podemos volver a examinar el comportamiento de la aplicación cuando se accede al endpoint. El registro adicional puede revelar configuraciones erróneas en las importaciones de módulos o en la inyección de dependencias que no eran evidentes antes.

Simultáneamente, la interfaz de DevTools puede proporcionar una representación visual de la estructura de la aplicación, destacando cualquier anomalía en las dependencias de los servicios o en las aplicaciones de middleware.

Al combinar la información granular de `NEST_DEBUG` y la descripción visual general proporcionada por `@nestjs/devtools-integration`, podemos identificar el problema de manera más rápida y precisa. Por ejemplo, si el error está relacionado con un servicio que no se inyecta correctamente en un controlador, estas herramientas pueden ayudar a identificar el eslabón perdido en la cadena de dependencias, guiándonos hacia una resolución rápida.

#### Errores comunes y cómo abordarlos

Aquí hay algunos errores comunes que debe considerar y que puede enfrentar en el futuro:

- **Rechazos de promesas no controlados (Unhandled promise rejections):** Estos pueden provocar fallos silenciosos en su aplicación. Asegúrese siempre de que las promesas se manejen correctamente con bloques try-catch o encadenando métodos `.catch()`.
- **Problemas de Inyección de Dependencias (DI):** Un problema común en las aplicaciones NestJS es una DI mal configurada, lo que genera errores como *Nest can’t resolve dependencies*. Revisar las importaciones de módulos y las configuraciones de los providers generalmente ayuda a resolver estos problemas.
- **Errores específicos del entorno:** A veces, los errores solo aparecen bajo condiciones ambientales específicas. El uso de contenedores Docker o herramientas de gestión de entorno consistentes como `dotenv` para gestionar variables de entorno puede ayudar a garantizar que su aplicación se ejecute sin problemas en diferentes entornos.
- **Dependencia circular:** Hablamos de dependencias circulares en el pasado. Una regla que debemos tener en cuenta aquí es evitar tener más de una exportación por archivo y aprovechar el uso de Madge cada vez que ocurra.

#### Depuración en acción: un ejemplo paso a paso

Para demostrar el proceso de depuración, considere un escenario donde un endpoint en particular, `/api/users`, devuelve un error `500 Internal Server Error`. Los siguientes pasos describen un enfoque sistemático para resolver el problema:

1. **Revisar registros:** Compruebe los registros de la aplicación en busca de mensajes de error relacionados con la solicitud.
2. **Aislar el problema:** Utilice Postman o Insomnia para replicar la solicitud y observar la respuesta.
3. **Identificar el componente defectuoso:** Al establecer puntos de interrupción en el depurador de VS Code alrededor del controlador del endpoint y los servicios relacionados, identifique dónde falla la aplicación.
4. **Abordar la causa raíz:** Una vez identificada, realice las correcciones necesarias, ya sea corregir un error lógico, manejar un caso faltante en un método de servicio o corregir un problema del modelo de datos.
5. **Probar y verificar:** Después de aplicar la corrección, vuelva a probar el endpoint para asegurarse de que el problema esté resuelto y no se hayan introducido nuevos problemas.

La depuración es tanto un arte como una ciencia. Al combinar una comprensión clara del problema con las herramientas adecuadas y un enfoque metódico, puede resolver eficientemente los problemas en sus APIs REST de NestJS, mejorando su fiabilidad y rendimiento.

Recuerde, el objetivo no es solo solucionar el problema inmediato, sino comprender su causa y aplicar ese conocimiento para evitar problemas similares en el futuro. DevTools de Nest es una herramienta muy potente que definitivamente debería probar, especialmente si está trabajando en un proyecto muy grande donde la depuración puede volverse compleja.

Este enfoque de depuración, centrado en la comprensión, las herramientas y la resolución metódica de problemas, complementa las estrategias de prueba discutidas anteriormente en el capítulo. Juntos, forman una estrategia integral para desarrollar, probar y mantener aplicaciones NestJS robustas.

---

### Resumen

Al concluir el Capítulo 7, reflexionamos sobre el camino crítico de fortalecer nuestras APIs, asegurándonos de que no solo sean operativas sino también robustas y tolerantes a fallos. Este capítulo nos equipó con las herramientas y conocimientos necesarios para elevar nuestras prácticas de prueba y depuración, que son elementos fundamentales para crear aplicaciones NestJS de alta calidad.

Nuestra exploración comenzó con un análisis en profundidad de la importancia de las pruebas. Destacamos cómo las pruebas trascienden la mera detección de errores, sirviendo como piedra angular para el aseguramiento de la calidad y la fiabilidad del software. A través de aplicaciones prácticas en nuestro sistema de gestión de usuarios, profundizamos en las metodologías de pruebas unitarias y de integración, demostrando cómo cada una juega un papel fundamental en la validación de los componentes de nuestra aplicación y sus interacciones.

El viaje hacia la depuración reveló estrategias y herramientas integrales para identificar y resolver problemas dentro de nuestras APIs REST. Enfatizamos la importancia de un enfoque metódico para la depuración, aprovechando las robustas funciones de NestJS, incluidas las variables de entorno y las herramientas de desarrollo, para diagnosticar y solucionar problemas de manera eficiente.

Al revisar el sistema de gestión de usuarios desarrollado en el [Capítulo 5](https://subscription.packtpub.com/book/web-development/9781835468609/5), aplicamos rigurosas estrategias de prueba, asegurando un proceso de verificación exhaustivo. Este enfoque práctico no solo consolidó nuestra comprensión de las capacidades de prueba de NestJS, sino que también destacó la adaptabilidad y eficiencia del framework para fomentar las mejores prácticas en el desarrollo de APIs.

Al sintetizar estos elementos, sentamos una base que prioriza la calidad y la eficiencia desde el inicio del proceso de desarrollo. Las metodologías y herramientas discutidas en este capítulo no son meros conceptos teóricos, sino recursos prácticos que mejorarán la fiabilidad y confiabilidad de sus aplicaciones NestJS.

Mirando hacia el futuro, las habilidades y el conocimiento adquiridos en este capítulo serán activos invaluables a medida que continuemos evolucionando y escalando nuestras aplicaciones. El énfasis en las pruebas y la depuración subraya un enfoque proactivo para el desarrollo de software, asegurando que nuestras APIs puedan soportar las complejidades y demandas de los ecosistemas digitales modernos.

Al concluir este capítulo, sigamos adelante con los principios de pruebas meticulosas y depuración diligente. Estas prácticas no son solo tareas que deben marcarse, sino que son parte integral del ciclo de vida del desarrollo de software robusto, escalable y fiable. Armados con estas capacidades, el próximo capítulo nos guiará en la prueba de nuestra API GraphQL que construimos en el [Capítulo 6](https://subscription.packtpub.com/book/web-development/9781835468609/6).
