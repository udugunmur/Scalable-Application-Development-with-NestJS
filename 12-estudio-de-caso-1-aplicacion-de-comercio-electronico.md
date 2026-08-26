# Parte 5: Ejemplos de Aplicaciones del Mundo Real y Estudios de Caso

## Capítulo 12: Estudio de Caso 1 - Aplicación de Comercio Electrónico

¿Sientes la confianza para construir aplicaciones de la vida real a escala? Este es el lugar donde haremos un buen uso de esa confianza. Bienvenido a nuestro primer estudio de caso, donde construiremos una aplicación de comercio electrónico (*e-commerce*) con NestJS.

En este capítulo, aprovecharemos todo lo que ya sabemos sobre APIs REST, pruebas y la aplicación de restricciones de modelado de datos en nuestras rutas, para que podamos construir una aplicación muy cercana a cualquier plataforma de comercio electrónico que utilizamos a diario.

También aprovecharemos esta oportunidad para integrar bases de datos reales, en lugar de nuestras bases de datos en memoria, añadiendo desafíos más prácticos a medida que construimos. Haremos esto para poder implementar mecanismos reales de autenticación y autorización en nuestra aplicación, trasladando todas las consideraciones teóricas de seguridad a un escenario del mundo real.

Para este capítulo, esto es lo que tenemos en nuestra agenda:

- Comprensión de los requisitos de una aplicación de comercio electrónico
- Diseño de la arquitectura de la aplicación y modelado de datos
- Implementación de APIs REST para productos y pedidos
- Autenticación y autorización de usuarios en el contexto del comercio electrónico

Al final de este capítulo, consolidarás tu comprensión de las APIs REST y sus consideraciones de arquitectura en el mundo real, autenticación y autorización, e integración de bases de datos, entre otros aspectos. Prepara tu entorno antes de continuar, ya que utilizaremos PostgreSQL además de otras herramientas que hemos empleado hasta ahora.

Adentrémonos en este estudio de caso para comprender el contexto y los requisitos de la aplicación que vamos a construir.

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Comprensión de los requisitos de una aplicación de comercio electrónico

Antes de sumergirnos en el código, es crucial comprender los requisitos de una aplicación de comercio electrónico. Esta comprensión guiará cada decisión que tomemos, desde la arquitectura hasta los detalles específicos de la implementación. Asegurará que la aplicación que construyamos satisfaga las necesidades de los usuarios y los estándares de la industria.

#### Identificación de las necesidades de los usuarios

Cuando se trata de una aplicación de comercio electrónico, las necesidades de los usuarios pueden ser bastante diversas y específicas. Sin embargo, ciertos requisitos prevalecen en un amplio espectro demográfico de usuarios:

- **Facilidad de navegación:** Los usuarios deben poder navegar fácilmente a través de la aplicación. Un diseño intuitivo y bien estructurado es clave. Por ejemplo, Amazon cuenta con un menú de navegación claro y simple que facilita a los usuarios encontrar lo que están buscando. Aunque esta es una necesidad de usuario muy importante, en nuestro contexto de aplicación del lado del servidor, nuestro enfoque no estará tan orientado hacia este aspecto. Un framework de interfaz de usuario como AngularJS o Next.js se tomaría esto muy en serio. No obstante, como aplicación del lado del servidor, nuestra API debe asegurarse de enviar al cliente una estructura de datos que no requiera demasiada manipulación antes de mostrar la información a los usuarios.
- **Información del producto:** La información detallada y precisa del producto es primordial. Los usuarios quieren saber exactamente qué están comprando. Imágenes de alta calidad, descripciones detalladas y reseñas reales de clientes son aspectos importantes de esto. En el contexto de nuestra API, debemos asegurarnos de que la descripción del producto describa el artículo de una manera poco ambigua en la fase de validación de datos del endpoint de creación de productos. ¿Por qué no añadir restricciones como la longitud de la descripción y luego acoplarla a una herramienta de Inteligencia Artificial (IA) que nos ayude a validar la descripción para hacerla aún más atractiva?
- **Opciones de búsqueda y filtrado:** Los usuarios deben poder encontrar rápidamente el producto que buscan. Capacidades de búsqueda efectivas y diversas opciones de filtrado son fundamentales. Por ejemplo, eBay permite a los usuarios filtrar los resultados de búsqueda según varios criterios como precio, condición y ubicación. Nuestra API debe ser lo suficientemente sólida para admitir todos estos tipos de filtrado de datos; tenemos múltiples opciones que exploraremos más adelante mientras construimos.
- **Seguridad:** Los usuarios necesitan sentirse seguros al realizar transacciones. Pasarelas de pago seguras y políticas de privacidad claras son necesarias. Aquí es donde todas las consideraciones de seguridad se pondrán en práctica.
- **Servicio de atención al cliente:** Un servicio de atención al cliente rápido y eficaz es imprescindible. Los usuarios deben poder comunicarse fácilmente para recibir soporte y obtener respuestas rápidas a sus consultas. Muchas plataformas exitosas de comercio electrónico, como Zappos, destacan por brindar un servicio al cliente de primer nivel.
- **Proceso de pago sencillo (*Easy checkout process*):** Un proceso de tramitación de pedidos sencillo y fluido es esencial para evitar el abandono del carrito. Los usuarios prefieren un proceso de pago que sea rápido, simple y que permita varios métodos de pago. Esta será una consideración clave en la parte de arquitectura, permitiendo a los usuarios pasar fácilmente de su carrito al pedido en sí.

Comprender estas necesidades del usuario es el primer paso para crear una aplicación de comercio electrónico que sea fácil de usar y cumpla con los estándares de la industria.

#### Definición de requisitos funcionales

Los requisitos funcionales definen las acciones fundamentales que una aplicación debe realizar para tener éxito. Son los servicios que el sistema debe proporcionar, cómo debe reaccionar ante entradas particulares y cómo debe comportarse bajo condiciones específicas. Son fundamentales para la utilidad y eficacia de un sistema, ya que describen comportamientos o funciones específicas del mismo.

Habiendo identificado las necesidades de los usuarios, ahora dirigimos nuestra atención a los requisitos funcionales de nuestra aplicación de comercio electrónico. Es importante tener en cuenta que, en el contexto de NestJS, nuestro enfoque se centrará principalmente en los aspectos del lado del servidor de estos requisitos. Estos son algunos de los requisitos funcionales más importantes:

- **Gestión de productos:** La aplicación debe proporcionar un conjunto estándar de operaciones para administrar productos. Esto incluye crear nuevos productos, leer detalles de productos, actualizar información de productos y eliminar productos. Estas operaciones deben encapsularse en una API bien estructurada que pueda integrarse fácilmente con un framework frontend.
- **Gestión de pedidos:** De manera similar, la aplicación debe admitir una serie de operaciones para gestionar pedidos. Esto incluye crear nuevos pedidos, actualizar estados de pedidos y eliminar pedidos. La API también debe admitir operaciones para gestionar los artículos dentro de un pedido.
- **Gestión de usuarios:** La aplicación debe proporcionar operaciones para administrar usuarios. Esto incluye crear nuevos usuarios, leer detalles de usuarios, actualizar información de usuarios y eliminar usuarios. También debe proporcionar operaciones para gestionar la autenticación y autorización de usuarios.
- **Gestión de pagos:** La aplicación debe proporcionar operaciones para gestionar pagos. Esto incluye crear nuevos pagos, actualizar estados de pagos y eliminar pagos. La API debe admitir una variedad de métodos de pago para satisfacer las diversas necesidades de los usuarios.

Dado que cada vez nos sentimos más cómodos con nuestro framework, estos requisitos probablemente se encapsularán en módulos separados, o dividiremos un módulo grande en una cantidad razonable de módulos y (si es necesario) añadiremos módulos relevantes para permitir la escalabilidad de nuestra aplicación.

En las siguientes secciones, profundizaremos en estos requisitos con más detalle y discutiremos cómo se pueden implementar utilizando NestJS. Recuerda que estos requisitos funcionales son solo un punto de partida. A medida que construyamos la aplicación, es probable que identifiquemos requisitos adicionales que deban abordarse.

A medida que avancemos, veremos cómo estos requisitos funcionales guían el desarrollo de nuestra aplicación de comercio electrónico.

#### Establecimiento de requisitos de rendimiento

Los requisitos de rendimiento son un tipo de requisito no funcional, fundamentales para la funcionalidad general y la experiencia de usuario de nuestra aplicación de comercio electrónico. Los requisitos no funcionales son los criterios que juzgan el funcionamiento de un sistema, en lugar de comportamientos específicos, tales como el rendimiento del sistema, la fiabilidad y la mantenibilidad. A menudo están diseñados para mejorar el rendimiento del sistema, haciéndolo más confiable, más fácil de usar y más eficiente. En nuestra aplicación de comercio electrónico, los requisitos de rendimiento pueden incluir tiempos de carga rápidos de las páginas, consultas eficientes a la base de datos y la capacidad de manejar muchos usuarios simultáneos. A medida que avancemos en el diseño de nuestra aplicación, consideraremos estos requisitos no funcionales para garantizar que nuestra aplicación no solo cumpla con sus objetivos funcionales, sino que también ofrezca una experiencia de usuario de alta calidad.

En resumen, comprender los requisitos de una aplicación de comercio electrónico sienta las bases de todo el proceso de desarrollo. Hemos explorado las necesidades de los usuarios, los requisitos funcionales y los requisitos de rendimiento, los cuales influirán en las decisiones que tomemos a medida que diseñamos e implementamos nuestra aplicación. Con una comprensión sólida de estos requisitos, ahora podemos avanzar hacia el diseño de la arquitectura de la aplicación, asegurando que nuestras decisiones de diseño se alineen con las necesidades y requisitos identificados y, en última instancia, den como resultado una plataforma de comercio electrónico eficaz, eficiente y fácil de usar.

---

### Diseño de la arquitectura de la aplicación y modelado de datos

En esta sección, nos centraremos en el diseño de la arquitectura de nuestra aplicación de comercio electrónico. Una arquitectura eficaz es crucial para el funcionamiento exitoso de cualquier aplicación y, en este caso, formará la columna vertebral de nuestra plataforma de comercio electrónico. Consideraremos cómo estructurar nuestra aplicación, cómo interactúan los diferentes componentes entre sí y cómo fluyen los datos dentro del sistema. Nuestras decisiones de diseño se guiarán por los requisitos que hemos identificado y la necesidad de escalabilidad, mantenibilidad y rendimiento. Este proceso es clave para garantizar que nuestra aplicación sea robusta, eficiente y capaz de ofrecer una experiencia de usuario fluida.

Al final de esta sección, tendremos un conjunto claro de especificaciones y comportamientos para nuestra API. Este es el resultado de adoptar el enfoque de documentación primero (*documentation-first approach*), mediante el cual diseñamos la API de manera flexible antes de pasar a la implementación. Esto contrasta con el enfoque de implementación primero (*implementation-first approach*), donde el diseño está fuertemente influenciado por lo que ya se ha implementado. Los ingenieros prefieren cada vez más el enfoque de documentación primero en los últimos años debido a la mayor flexibilidad que ofrece durante la fase de implementación.

#### Pasos para diseñar una API REST

Al diseñar una API REST, primero debemos identificar los objetos que se presentarán al cliente como recursos. En el contexto de una API REST, un recurso puede verse como un objeto individual o una colección de objetos sobre los cuales la API puede proporcionar información. Es una pieza específica de datos a la que se puede acceder y manipular a través de la API. Un recurso puede ser tan simple como una sola entidad de datos o puede ser una estructura compleja con múltiples campos de datos. Ejemplos de recursos pueden incluir usuarios, productos o pedidos en una aplicación de comercio electrónico. Cada recurso en una API REST se identifica mediante un Identificador Uniforme de Recursos (*Uniform Resource Identifier* - URI) único.

En el contexto de una aplicación de comercio electrónico, estos son algunos recursos importantes a los que prestar atención:

- **Productos:** Son los artículos disponibles para su compra en la aplicación de comercio electrónico. Cada producto normalmente tiene atributos como nombre, descripción, precio, imagen, especificaciones, etc.
- **Usuarios:** Son las personas que utilizan la aplicación como clientes, administradores o personal. Los datos del usuario pueden incluir información como nombre, correo electrónico, contraseña, dirección de envío, etc.
- **Pedidos:** Representan las compras realizadas por los usuarios. Un pedido normalmente incluye detalles como los productos adquiridos, cantidades, precio total, detalles del cliente e información de pago.
- **Pagos:** Son las transacciones realizadas por los usuarios para pagar sus pedidos. La información de pago puede incluir el método de pago utilizado, el monto pagado, la fecha y hora de la transacción y el estado del pago.
- **Reseñas:** Son evaluaciones o comentarios proporcionados por los clientes sobre los productos que han comprado. Las reseñas pueden incluir una calificación, un comentario en texto y el nombre del usuario que proporcionó la reseña.

#### Creación de URIs de recursos

Ahora que hemos identificado los objetos que nuestra API presentará a los clientes, podemos pasar al segundo paso, que es crear las URIs de los recursos. Este paso tiene como objetivo crear una lista de endpoints que potencialmente tendremos en nuestra aplicación, sin especificar sus métodos HTTP. Cada recurso tendrá un conjunto específico de URIs, como se detalla aquí:

```text
# products
/products
/products/{id}
/products/{id}/orders
/products/{id}/orders/{orderId}
/products/{id}/reviews
/products/search

# users
/users
/users/{id}
/users/{id}/orders
/users/{id}/orders/{orderId}
/users/{id}/reviews
/users/search

# orders
/orders
/orders/{id}
/orders/{id}/reviews

# payments
/payments
/payments/{id}
/payments/{paymentMethod}/orders

# reviews
/reviews
/reviews/{id}
/reviews/search
```

Con el fin de mantener las cosas directas y sencillas, aquí solo tenemos una lista de URIs que se implementarán en nuestra API. Tenemos, por ejemplo, el endpoint `/products/{id}/orders` que dará acceso a los clientes a los pedidos realizados para un `orderId` específico (el parámetro de ruta `id`) o el endpoint `/payments/{paymentMethod}/orders` que muy probablemente listará los pedidos realizados para un método de pago específico.

#### Representación de recursos

Ahora, el siguiente paso es trabajar en la presentación del recurso. Necesitamos contar con un contrato común entre el servidor y el cliente sobre cómo se presentarán los datos. En nuestra API, utilizaremos el formato JSON dado que es un formato ampliamente adoptado en la era moderna, aunque otra opción podría ser utilizar XML en su lugar.

Para nuestra representación de datos, obligaremos a nuestra API a enviar al cliente este formato único en JSON:

```typescript
class APIResponse {
  success: boolean;
  message: string;
  error?: HttpException;
  data: any;
}
```

La clase anterior establece la expectativa de cómo se presentarán los datos a los clientes. A pesar de ser una clase de TypeScript, esperamos un objeto JSON con la estructura definida por la clase `APIResponse`, y que `HttpException` se importe desde el paquete `@nestjs/common`. Puede ser reemplazada por cualquiera de sus excepciones heredadas, tales como `BadRequestException`, `UnauthorizedException`, etc.

#### Asignación de métodos HTTP

Ahora que hemos definido la representación de recursos, podemos asignar métodos HTTP a nuestra lista de endpoints. Se agrupan en tres categorías: *setters* (establecedores), *getters* (obtentores) y *mutators* (mutadores):

- **Establecedores (*Setters*):** Los *setters* son todas las operaciones que nos ayudarán a crear un recurso. En el mundo RESTful, usualmente definimos *setters* con el verbo POST, ya que esperamos que las peticiones POST ESTABLEZCAN (*SET*) datos en nuestra base de datos. Estos son los *setters* más importantes para nuestra API:

```text
# product
HTTP POST /products

# users
HTTP POST /users

# orders
HTTP POST /orders

# payments
HTTP POST /payments

# reviews
HTTP POST /reviews
```

Estos son todos los *setters* que necesitamos tener en la aplicación para persistir (*set*) información en la base de datos.

- **Obtentores (*Getters*):** Después de establecer datos de forma persistente en nuestra base de datos, necesitamos poder recuperarlos de allí. Aquí es donde los *getters* son importantes. Obtenerlos es bueno, pero obtenerlos de una manera significativa es mejor. Con base en la lista de URIs que definimos en el primer paso, estos son los *getters* más importantes para nuestra API:

```text
# products
HTTP GET /products?page=1&limit=100
HTTP GET /products/{id}
HTTP GET /products/{id}/orders?page=1&limit=10
HTTP GET /prodcuts/{id}/orders/{oderId}
HTTP GET /products/{id}/reviews?page=1&limit=100
HTTP GET /products/search?page=1&limit=100&keyword=sample&minPrice=100&maxPrice=1000&color=red,black&specs=ram:8Go,ram:16Go

# orders
HTTP GET /orders?page=1&limit=100
HTTP GET /orders/{id}
HTTP GET /orders/{id}/reviews?page=1&limit=100

# users
HTTP GET /users?page=1&limit=100
HTTP GET /users/{id}
HTTP GET /users/{id}/orders?page=1&limit=10
HTTP GET /users/{id}/reviews?page=1&limit=100
HTTP GET /users/search?page=1&limit=10&keyword=John&verified=true

# payments
HTTP GET /payments?page=1&limit=100
HTTP GET /payments/{id}
HTTP GET /payments/{paymentId}/orders?page=1&limit=100

# reviews
HTTP GET /reviews?page=1&limit=100
HTTP GET /reviews/{id}
HTTP GET /reviews/search?page=1&limit=100&keyword=Excellent&startDate=1716794723000
```

Aquí tenemos una larga lista de *getters* que podemos necesitar utilizar en nuestra API. Ten en cuenta que algunos de ellos vienen con consultas de muestra (*queries*), tales como `page` y `limit` para propósitos de paginación; `keyword` en los endpoints de búsqueda; las consultas `minPrice`, `maxPrice` y `specs` en la búsqueda de productos; o `verified` en el endpoint de búsqueda de usuarios para mejorar la capacidad de filtrado de nuestra API.

- **Mutadores (*Mutators*):** Entre los mutadores, tenemos operaciones que pueden actualizar el estado de un solo registro en nuestra base de datos, o incluso eliminar un solo registro de la base de datos. Para realizar un seguimiento del registro de la base de datos, los desarrolladores de software tienden a priorizar el enfoque de borrado suave (*soft-delete*), mediante el cual los datos no se eliminan por completo de la base de datos, sino que se "ocultan" con una etiqueta o marca específica. Nuestra API debe ser lo suficientemente resistente para permitir el *soft-delete* mediante operaciones PUT (o PATCH), y una operación de eliminación adecuada mediante operaciones DELETE en registros eliminados por *soft-delete* después de un cierto período de tiempo si ya no se necesitan.

Esta es la lista de los mutadores que podemos necesitar en nuestra API:

```text
# products
HTTP PUT /products/{id}
HTTP DELETE /product/{id}

# users
HTTP PUT /users/{id}
HTTP DELETE /users/{id]

# orders
HTTP PUT /orders/{id}
HTTP DELETE /orders/{id}

# reviews
HTTP PUT /reviews/{id}
HTTP DELETE /reviews/{id}
```

Con estos mutadores, es importante señalar que nuestra API no permite mutar la información de pagos. Una vez que se ha procesado un pago, nadie debería poder mutarlo. Por otro lado, las reseñas pueden mutarse por diferentes razones, como por parte del autor cuando necesita actualizar cualquier información, o por el administrador del sistema si la reseña infringe las políticas de nuestro proyecto. Esto también puede conducir a la eliminación de la cuenta de un usuario.

¡Bien hecho!; hemos diseñado las operaciones principales de nuestra API. Poniéndolo todo junto, necesitamos idear una especificación de API que sea legible tanto para máquinas como para desarrolladores humanos que van a trabajar en la implementación, o clientes humanos que van a consumir nuestra API en el futuro. Existen varias herramientas que pueden ayudarnos a escribir la documentación de la API, incluidos productos GUI como Postman, Spotlight o Mulesoft. Sin embargo, para este caso de uso, utilizaremos una herramienta gratuita: OpenAPI (anteriormente conocida como la especificación Swagger), que es simplemente un archivo `.yaml` que describe las diferentes operaciones admitidas por tu API. OpenAPI también te brinda la capacidad de probar tu archivo `.yaml`, compartirlo con tu equipo y simular acciones de la API en la plataforma SwaggerHub ([https://swagger.io/](https://swagger.io/)).

#### Escritura de APIs

Sigamos adelante y, con todo lo que hemos discutido hasta ahora, escribamos la documentación de nuestra API. Esto también nos ayudará a definir el esquema en el que los objetos del cuerpo (*body*) deben llegar al servidor para fines de validación más adelante en la fase de implementación.

Inicia sesión en el panel de SwaggerHub (o crea una nueva cuenta si es necesario) y haz clic en el botón **Create New | Create New API** en la esquina superior izquierda, como se muestra en la Figura 12.1:

![Figura 12.1: Menú de control del panel de SwaggerHub](/images/Figure_12.1.png)

*Figura 12.1: Menú de control del panel de SwaggerHub*

Se te pedirá que elijas la plantilla. Simplemente usa la de **Simple API**, asigna un nombre y deja las opciones predeterminadas tal como están (*visibility* y *auto mock api*), luego haz clic en el botón **Create API**.

Serás redirigido a una interfaz de usuario que se parece a la Figura 12.2:

![Figura 12.2: Panel de SwaggerHub para la API de comercio electrónico](/images/Figure_12.2.png)

*Figura 12.2: Panel de SwaggerHub para la API de comercio electrónico*

Puedes copiar y pegar el contexto de mi especificación pública de SwaggerHub para este proyecto aquí:

[https://app.swaggerhub.com/apis/pacyL2K19/e-commerce/1.0.0](https://app.swaggerhub.com/apis/pacyL2K19/e-commerce/1.0.0)

La especificación OpenAPI pretende ser una documentación legible por humanos y máquinas que proporciona un conjunto de endpoints vinculados a sus métodos HTTP, así como posibles respuestas de la API.

Por ejemplo, en la especificación proporcionada (sigue el enlace anterior), tenemos la operación `HTTP GET /products` con un conjunto de parámetros y un formato de respuesta exitosa adjunto. Para obtener más información sobre las especificaciones de OpenAPI v3, consulta su documentación oficial en [https://swagger.io/docs/specification/about/](https://swagger.io/docs/specification/about/).

Bien hecho; tenemos nuestro diseño de API listo para ser implementado. Ten en cuenta que diseñarlo no es un proceso de una sola vez. Este archivo se revisará en el futuro a medida que evolucione el proyecto.

Ahora, para la implementación, escribamos algo de código en la siguiente sección.

---

### Implementación de APIs REST para productos y pedidos

Ahora que tenemos listo el diseño de la arquitectura de nuestra aplicación de comercio electrónico y hemos definido las operaciones principales de nuestra API, es hora de dirigir nuestra atención a la implementación real de nuestras APIs REST. En las siguientes secciones, nos centraremos en construir los aspectos funcionales de nuestra aplicación de comercio electrónico: productos, pedidos y pagos. Esto implicará codificar nuestros endpoints, configurar nuestra base de datos y asegurarnos de que nuestra aplicación se ejecute sin problemas. Utilizaremos nuestro diseño de API previamente definido como guía para esta implementación. Sumerjámonos y comencemos a construir nuestra aplicación de comercio electrónico.

#### Configuración del entorno de desarrollo

Aunque se asume que ya estás familiarizado con la configuración de un proyecto NestJS, vale la pena mencionar que este proceso implica crear una nueva aplicación NestJS usando la CLI de Nest, instalar las dependencias necesarias y configurar el archivo principal del servidor. Utilizaremos TypeScript como nuestro lenguaje de programación principal y seguiremos una estructura modular para nuestro código. Esto nos proporcionará una base de código limpia y organizada, facilitando la administración y escalabilidad de nuestra aplicación de comercio electrónico. En la siguiente sección, discutiremos en detalle la configuración y puesta a punto de nuestra base de datos usando PostgreSQL.

Ahora, continúa y crea un nuevo proyecto NestJS con el siguiente comando:

```bash
$ nest new e-commerce-app
```

Estoy usando el nombre `e-commerce-app` en el comando para generar el proyecto, pero siéntete libre de usar el que más te guste. Esto generará un nuevo proyecto NestJS, con el que ya estamos familiarizados. Ahora trabajemos en la configuración de nuestra base de datos.

#### Configuración y puesta a punto de la base de datos

Para configurar nuestra base de datos PostgreSQL, primero debemos descargar e instalar PostgreSQL. Se puede descargar de forma gratuita desde el sitio web oficial de PostgreSQL ([https://www.postgresql.org/download/](https://www.postgresql.org/download/)). Durante el proceso de instalación, se te pedirá que configures una contraseña para el superusuario de PostgreSQL (`postgres`), la cual debes recordar ya que será necesaria más adelante.

Una vez instalado, podemos crear nuestra base de datos. Para ello, debemos abrir la herramienta pgAdmin ([https://www.pgadmin.org/](https://www.pgadmin.org/)) que se instaló junto con PostgreSQL. Esta herramienta proporciona una interfaz gráfica para administrar bases de datos PostgreSQL.

En pgAdmin, expande la sección **Servers** en la barra lateral izquierda, luego **PostgreSQL**. Aquí, se te solicitará la contraseña que configuraste durante la instalación. Después de ingresar la contraseña, expande la sección **Databases** y haz clic derecho sobre ella. Selecciona **Create | Database...**. Se abrirá una nueva ventana donde puedes ingresar el nombre de tu nueva base de datos, por ejemplo, `ecommerce_db`. Haz clic en el botón **Save** para crear la base de datos.

Ahora que nuestra base de datos ha sido creada, estamos listos para conectarla con nuestra aplicación NestJS. Usaremos el módulo TypeORM ([https://typeorm.io/](https://typeorm.io/)) para esto, que es un potente módulo ORM que puede trabajar con varios tipos de bases de datos, incluida PostgreSQL.

Primero, necesitamos instalar las dependencias necesarias en nuestro proyecto. En la terminal, navega a la carpeta raíz de nuestro proyecto NestJS y ejecuta el siguiente comando:

```bash
$ yarn add @nestjs/typeorm typeorm pg
```

Esto instalará el módulo `@nestjs/typeorm`, que es un módulo de NestJS que integra la biblioteca TypeORM en sí, y `pg`, que es un cliente de PostgreSQL para Node.js.

Ahora, podemos configurar el módulo TypeORM en nuestra aplicación. Haremos esto en el archivo `app.module.ts`. Aquí, importaremos la clase `TypeOrmModule` y la añadiremos al arreglo `imports` de nuestro módulo. Usaremos el método `forRoot()` de `TypeOrmModule` para configurarlo. Pasaremos un objeto a `forRoot` con las siguientes propiedades:

- **`type`:** El tipo de nuestra base de datos; en este caso, es `postgres`
- **`host`:** El host de nuestra base de datos; usualmente, es `localhost` para fines de desarrollo.
- **`port`:** El puerto de nuestra base de datos; por defecto, es `5432`
- **`username`:** El nombre de usuario para conectarse a nuestra base de datos; por defecto, es `postgres`
- **`password`:** La contraseña para conectarse a nuestra base de datos; es la contraseña que configuraste durante la instalación de PostgreSQL
- **`database`:** El nombre de nuestra base de datos; es el nombre que le dimos a nuestra base de datos cuando la creamos en pgAdmin
- **`entities`:** Un arreglo con las rutas a nuestras entidades; las entidades en TypeORM son clases de JavaScript que se asignan a tablas en nuestra base de datos
- **`synchronize`:** Un valor booleano que indica si la base de datos debe crearse automáticamente en cada lanzamiento de la aplicación; durante el desarrollo, es útil establecerlo en `true`

Siguiendo la guía anterior, el archivo `app.module.ts` debería verse como el siguiente código:

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: 'localhost',
      port: 5432,
      username: 'postgres',
      password: 'your_password',
      database: 'ecommerce_db',
      entities: [],
      synchronize: true,
    }),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

Inicia el servidor una vez más en modo de desarrollo y asegúrate de que arranque sin ningún problema.

##### Pruébalo

Si bien esta base de datos minimalista funciona perfectamente bien por ahora, hay margen para mejorarla aprovechando el poder de la clase `ConfigModule` de NestJS siguiendo estos sencillos pasos:

1. Instala el paquete `@nestjs/config` usando Yarn.
2. Importa la clase `ConfigModule` y configúrala como global en el arreglo `imports` de la clase `AppModule`.
3. En lugar de usar el método `forRoot()` de `TypeOrmModule`, usa `forRootAsync()`, lo que te permitirá inyectar variables de entorno de forma asíncrona.
4. Inyecta la clase `ConfigService` en la propiedad `inject` de `forRootAsync`.
5. Reemplaza los valores codificados en duro (*hard-coded*) por sus equivalentes en el archivo `.env` que tendrás que crear; por ejemplo, en lugar de codificar en duro la contraseña, puedes hacer algo como establecer la contraseña en `configService.get<string>('POSTGRES_PASSWORD')` o como decidas nombrarla en tu archivo `.env`.
6. Prueba la aplicación una vez más y asegúrate de que se inicie con éxito.

El código completo está disponible en el repositorio de GitHub de este capítulo ([https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch12](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch12)). Puedes echar un vistazo si tienes problemas al seguir los pasos anteriores.

Con esto, nuestra base de datos está lista y conectada con nuestra aplicación, y podemos comenzar a implementar las operaciones de base de datos en nuestros endpoints de API.

#### Implementación de los endpoints de la API de productos

Tenemos nuestro diseño listo; tenemos la base de datos configurada; ahora, es hora de implementar la lógica central en diferentes módulos.

Para garantizar que la estructura del proyecto siga reflejando el modelo de datos que definimos en la primera sección, nuestros módulos se definirán alrededor de los modelos de negocio de los que ya hablamos, que son productos, usuarios, pedidos, pagos y reseñas.

##### Creación del módulo de productos

Primero, generemos el recurso de productos utilizando la CLI de Nest, ejecutando el siguiente comando en la raíz del proyecto; asegúrate de seleccionar REST API (primera opción) cuando se te solicite:

```bash
$ nest generate resource products
```

El script anterior generará los archivos de recursos de productos y actualizará el archivo `app.module.ts` para importar el módulo recién creado: `ProductsModule`. Ya dominamos el proceso de crearlos a mano. Sin embargo, para ganar productividad, siempre se recomienda aprovechar el uso de la CLI.

##### Creación de la entidad Product

A continuación, definiremos la entidad `Product`. En el archivo generado `product/entities/product.entity.ts`, pega el siguiente contenido:

```typescript
// src/products/entities/product.entity.ts
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';

@Entity('products')
export class Product {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column('text')
  name: string;

  @Column('text')
  description: string;

  @Column('decimal', { default: 0.0 })
  price: number;

  @Column('text', { default: 'no-image.png' })
  image: string;

  @Column('simple-json', {
    default: {},
    comment: "a key-value pair that represents the product's specs",
  })
  specs: Record<string, string>;

  @Column({ type: 'timestamp', default: () => 'CURRENT_TIMESTAMP' })
  createdAt: Date;
}
```

El fragmento de código anterior es una definición de entidad para la tabla `product` en nuestra base de datos. Como ya no estamos usando una base de datos en memoria, esta tabla se almacenará en la base de datos que creamos en la sección anterior gracias a los decoradores específicos de TypeORM, tales como `@Entity()` y `@Column()`. El primero indica que se trata de una tabla de base de datos real (en el contexto de bases de datos SQL). Acepta dos parámetros opcionales: el primero es el nombre de la tabla (cuando no se especifica, TypeORM usará el nombre de la clase de entidad en minúsculas) y el segundo es un objeto de opciones que se puede utilizar para completar propiedades como comentarios, nombre de la base de datos (si se usa una base de datos diferente a la predeterminada), etc. El segundo decorador, `@Column()`, indica que este campo es una columna en la tabla principal. Este decorador también toma dos parámetros opcionales; el primero impone el tipo de datos que se almacenarán en esa columna. Si no se indica, Postgres asignará un tipo según el tipo asociado con el campo en la clase. El segundo es también un objeto de opciones, que puede agregar múltiples propiedades a la columna, tales como las siguientes:

- Está la propiedad booleana `nullable` que le indica a la base de datos si el campo puede ser nulo o no. Por defecto, cuando no se establece, la propiedad `nullable` se establece en `false`.
- La propiedad `default` indica el valor predeterminado que se establecerá para este campo, en caso de una omisión cuando se establezca en la base de datos.
- Dependiendo del tipo de columna, se pueden añadir propiedades adicionales como `precision`, para campos decimales. Por ejemplo, para nuestra columna `price`, podemos hacer algo como lo siguiente:

```typescript
@Column('decimal', { precision: 5, default: 0 })
price: number;
```

Ten en cuenta que cada entidad debe tener una columna primaria (para bases de datos basadas en SQL) o un `ObjectId` (para bases de datos basadas en NoSQL) decorada con un decorador específico. En nuestro caso, estamos usando el decorador `@PrimaryGeneratedColumn()`, que es de tipo numérico por defecto. Dado que pasamos el parámetro `'uuid'`, nuestra columna primaria será una cadena de Identificador Único Universal (*Universal Unique Identifier* - UUID). Puedes aprender sobre los parámetros uuid y por qué se prefieren como identificadores primarios en [https://www.techtarget.com/searchapparchitecture/definition/UUID-Universal-Unique-Identifier](https://www.techtarget.com/searchapparchitecture/definition/UUID-Universal-Unique-Identifier).

Ahora, utilizando una herramienta gráfica como pgAdmin ([https://www.pgadmin.org/](https://www.pgadmin.org/)) para la visualización de la base de datos, comprueba que tienes la tabla `products` con sus columnas correspondientes. Tu tabla debería verse como en la Figura 12.3:

![Figura 12.3: pgAdmin mostrando la tabla recién creada](/images/Figure_12.3.png)

*Figura 12.3: pgAdmin mostrando la tabla recién creada*

Ten en cuenta que esta tabla no está completa, ya que necesitaremos agregar una relación entre el producto y otros recursos como pedidos o reseñas. Esto se discutirá en las próximas secciones.

##### Servicio de productos

En el archivo `products/products.service.ts`, tenemos las operaciones generadas de Crear, Leer, Actualizar y Eliminar (*Create, Read, Update, and Delete* - CRUD) que la CLI de Nest creó para nosotros. Vamos a mejorarlas.

Siéntete libre de eliminar cualquier código irrelevante que puedas encontrar allí y procedamos paso a paso.

Primero, implementemos la creación de productos, como se decidió en la fase de arquitectura y diseño. Esto solo lo pueden realizar usuarios conectados con el rol de administrador. Para mantener la implementación fluida, ignoraremos esta restricción por ahora y nos centraremos en la lógica de negocio. La última sección se centrará en proteger nuestros endpoints, así como en aplicar mecanismos de autorización y autenticación; no te preocupes por eso por ahora.

Para poder crear un nuevo producto, necesitamos el nombre del producto, la descripción, el precio y la URL de la imagen que cumplan con los siguientes requisitos comerciales y de usuario:

- El campo de **descripción** debe ser lo suficientemente significativo como para ayudar a los usuarios a comprender qué es el producto. Primero debemos considerar la longitud de la descripción y establecerla en un mínimo de 25 caracteres por ahora, y luego agregar una herramienta de IA en el pipeline de validación para asegurarnos de que no estamos agregando descripciones largas que no signifiquen nada. Para agregar la herramienta de IA, podemos usar algunas de las herramientas de IA gratuitas o de pago disponibles. Con fines de demostración, utilizaremos Gemini aquí debido a su corta curva de aprendizaje y su reputación en la industria.
- El campo de **nombre** no debe ser largo. Debemos permitir que las aplicaciones cliente muestren el nombre con un enfoque *mobile-first*. Es preferible un nombre que pueda adaptarse fácilmente a la pantalla del usuario. Aquí adoptaremos una restricción de 5 a 25 caracteres, pero esto puede variar según los clientes objetivo.
- El **precio** debe ser mayor que cero.
- La **URL de la imagen**, si está presente, debe ser una URL válida.
- Las **especificaciones del producto** deben ser un objeto de pares clave-valor siguiendo el formato `<nombre_especificación>:<valor_especificación>`. Por ejemplo, un arreglo válido de especificaciones de producto podría tomar la forma de `specs: {'color':'red', 'size:XL'}`.

Nuestro validador de entrada debe ser lo suficientemente robusto para verificar todo esto antes de persistir los datos en la base de datos.

En el archivo de servicio generado, localiza la función `create` (siéntete libre de eliminar el resto del código). Verás algo como lo siguiente:

```typescript
// src/products/products.service.ts
@Injectable()
export class ProductsService {
  constructor(
    @InjectRepository(Product)
    private productsRepository: Repository<Product>,
  ) {}

  create(createProductDto: CreateProductDto) {
    console.log('see dto', createProductDto);
  }
}
```

Ten en cuenta que simplemente imprimí el objeto `createProductDto`. No queremos persistir datos en este momento. La validación ocurre en la clase `CreateProductDto` ubicada en `src/products/dto/create-product.dto.ts`. Completemos ahora algunas validaciones. Reemplaza la clase vacía generada con el siguiente código (asegúrate de instalar los paquetes requeridos para este paso mediante el comando `$ yarn add class-validator class-transformer`):

```typescript
// src/products/dto/create-product.dto.ts
import {
  IsDecimal,
  IsObject,
  IsString,
  IsUrl,
  Length,
  Validate,
} from 'class-validator';
import { ProductDescription } from '../custom-validators/ProductDescription';
import { ProductSpecs } from '../custom-validators/ProductSpecs';

export class CreateProductDto {
  @IsString({
    message: 'name must be a string',
  })
  @Length(5, 25, {
    message: 'name must be between 5 and 25 characters',
  })
  name: string;

  @IsString({
    message: 'description must be a string',
  })
  @Length(25, 255, {
    message: 'description must be between 25 and 255 characters',
  })
  @Validate(ProductDescription)
  description: string;

  @IsDecimal(
    {
      decimal_digits: '2',
    },
    {
      message: 'price must be a decimal number',
    },
  )
  price: number;

  @IsObject({
    message: 'specs must be a valid object',
  })
  specs: Record<string, string>;

  @IsString({
    message: 'image must be a string',
  })
  @IsUrl(
    {
      require_protocol: true,
    },
    {
      message: 'image must be a valid URL',
    },
  )
  image: string;
}
```

En el fragmento de código anterior, diseñamos cómo debería ser nuestra entrada de creación de producto. Aunque se trata de una validación básica, es un buen punto de partida. Estamos validando aspectos como la longitud de los textos (descripción del producto y nombre del producto), la presencia de una URL correcta (en el campo `image`) o la presencia de una entrada de objeto en el campo `specs`.

Según los requisitos de nuestro producto, este no es el final del pipeline de validación. Necesitamos hacer dos cosas más para completarlo:

1. Validar el campo `specs` del producto para asegurarse de que la entrada siga el formato correcto
2. Validar la descripción del producto de acuerdo con los requisitos de nuestros usuarios

Comencemos con lo más fácil: validar el formato del campo `specs`. Para lograrlo, el paquete `class-validator` nos permite crear proveedores personalizados listos para usar, aplicando nuestras reglas. Creemos una carpeta dentro del módulo `products` llamada `custom-validators`. En la carpeta recién creada, creemos un archivo `ProductSpecs.ts` y peguemos el siguiente código:

```typescript
// src/products/custom-validators/ProductSpecs.ts
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
} from 'class-validator';

@ValidatorConstraint({ name: 'ProductSpecs', async: false })
export class ProductSpecs implements ValidatorConstraintInterface {
  accepetedSpecs = [
    'ram',
    'processor',
    'ssd',
    'hdd',
    'brand',
    'model',
    'color',
    'weight',
    'dimensions',
    'material',
    'capacity',
    'power',
    'voltage',
    'warranty',
    'condition',
    'chip',
    'year',
    'other_features', // we can support more specs as the application grows
  ];

  validate(specs: Record<string, string>) {
    const keys = Object.keys(specs);
    if (keys.length === 0) return true; // no specs provided
    return keys.every(
      (key) => this.accepetedSpecs.includes(key) && specs[key].trim() !== '',
    );
  }

  defaultMessage() {
    return 'Product specs must be a valid object with supported specs';
  }
}
```

En el fragmento de código anterior, hemos creado la clase `ProductSpecs` que implementa la interfaz `ValidatorConstraintInterface`, la cual nos obliga a implementar al menos un método. Ese sería el método `validate()`, decorado con el decorador `@ValidatorConstraint`, que nos permite establecer un nombre para nuestro decorador personalizado. En este caso, estamos validando el formato del objeto `specs`, que se pasó como parámetro en la implementación del método `validate`. Luego aplicamos algunas comprobaciones en el objeto, como verificar que si los elementos de `specs` no siguen el formato deseado, devolvemos `false`, y en caso contrario, devolvemos `true`. Hemos codificado en duro el parámetro `accepetedSpecs` en un arreglo por ahora que admite un conjunto de especificaciones como RAM, SSD, garantía, etc. A medida que el alcance de nuestra aplicación de comercio electrónico evolucione, también lo hará el arreglo. En lugar de tener un conjunto de especificaciones codificado en duro, más adelante podemos decidir crear una tabla dedicada para almacenar especificaciones personalizadas. Sin embargo, por ahora, mantengamos las cosas simples, con tu permiso, por supuesto :).

Esta validación es sincrónica. En la mayoría de los escenarios, necesitamos validar de forma sincrónica, pero como veremos en breve, a veces la validación asíncrona es imprescindible. La implementación será un poco diferente.

Ten en cuenta que también implementamos el método `defaultMessage()` que nos permite establecer un mensaje predeterminado en caso de que llamemos a este validador personalizado sin un mensaje de error.

Para consumir este validador personalizado, tendremos que agregar esta restricción al campo `specs` como en el siguiente fragmento de código:

```typescript
// add this import
import {
  // everything else we had
  Validate,
} from 'class-validator';
import { ProductSpecs } from '../custom-validators/ProductSpecs'; // the newly created class

export class CreateProductDto {
  // everything else remains the same
  @IsObject({
    message: 'specs must be a valid object',
  })
  @Validate(ProductSpecs) // we added this line
  specs: Record<string, string>;
}
```

En las actualizaciones de código anteriores, agregamos la restricción `ProductSpecs` a nuestro campo `specs`. Ahora, el decorador `IsObject()` (o `IsArray()`) por sí solo no es suficiente; las especificaciones deben seguir nuestras reglas personalizadas. Usamos el decorador `@Validate()` de `class-validator` para inyectar nuestras reglas. Si esto no está claro, te animo a que te tomes un tiempo para consultar la documentación sobre validadores personalizados en [https://github.com/typestack/class-validator?tab=readme-ov-file#custom-validation-classes](https://github.com/typestack/class-validator?tab=readme-ov-file#custom-validation-classes).

Probaremos todo en unos minutos. Sin embargo, antes de probarlo todo, implementemos nuestro segundo validador personalizado, `ProductDescription`. Para continuar con este paso, te sugiero que obtengas tu clave de API de Gemini AI studio en [https://ai.google.dev/gemini-api/docs/api-key](https://ai.google.dev/gemini-api/docs/api-key). Sigue los pasos en el tutorial de Google y guarda tu clave en tu archivo `.env`; nombrémosla `GEMINI_API_KEY`. Puedes usar cualquier otro modelo de IA, como GPT-3.5 o 4. La idea aquí es agregar un validador asíncrono personalizado gracias a la IA.

Necesitamos hacer lo siguiente:

1. Definir claramente el prompt de entrada para darle al modelo de IA. Recuerda que estas herramientas pueden ser engañosas, especialmente si el prompt en sí no es claro.
2. Definir claramente qué salida esperamos obtener del prompt. Definir el formato nos ayudará a brindar comentarios personalizables a los clientes.
3. También es importante descubrir cómo enviar los comentarios que recibimos del modelo de IA como respuesta a nuestros clientes en lugar de un mensaje predeterminado codificado en duro, como hicimos en el ejemplo anterior.

Implementemos ahora esta lógica. Crea un nuevo archivo en la carpeta `custom-validators` y nómbralo `ProductDescription.ts`, luego pega el siguiente código:

```typescript
// src/products/custom-validators/ProductDescription.ts
import {
  ValidatorConstraint,
  ValidatorConstraintInterface,
} from 'class-validator';
import { GoogleGenerativeAI } from '@google/generative-ai';
import * as dotenv from 'dotenv';

@ValidatorConstraint({ name: 'ProductDescription', async: true })
export class ProductDescription implements ValidatorConstraintInterface {
  message: string = '';

  async validate(description: string) {
    dotenv.config();
    const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
    const model = genAI.getGenerativeModel({
      model: 'gemini-1.5-pro',
    });
    const prompt = `Given the description provided below, check if it means something to a user perspective and that it doesn't contain any offensive content or vague informations 
 
 the description: "${description}" 
 if you think the description is valid, please return "valid" otherwise type "invalid" + the reason why you think it's invalid 
 the response should be sent in a human-readable format, since it will be used to send feedback to the client`;

    const result = await model.generateContent(prompt);
    const response = result.response;
    const isValid = !response.text().toLowerCase().includes('invalid');
    if (!isValid) this.message = response.text();
    return isValid;
  }

  defaultMessage() {
    return this.message;
  }
}
```

Como puedes ver en el código anterior, debemos asegurarnos de instalar el paquete requerido con el comando `$ yarn add @google/generative-ai`. La implementación se parece casi a la clase `ProductSpecs`, excepto por el hecho de que ahora tenemos un validador asíncrono (establecemos la propiedad `async` en `true` en el decorador `ValidatorConstraint`). Por lo tanto, el método `validate()` ahora realiza una llamada a la API asíncrona a la API de GenAI.

El prompt le pide claramente al modelo que revise la descripción proporcionada y compruebe si la información proporcionada no es engañosa o no contiene expresiones ofensivas. Espera que el modelo devuelva `valid` o `invalid`, más detalles del motivo si la descripción no es válida. Luego declaramos una variable de clase llamada `message` para permitirnos pegar los comentarios del modelo de IA como un mensaje de error para enviar al cliente en lugar de un valor codificado en duro.

¡Bien hecho!; asegúrate de consumir este validador de la misma manera que lo hicimos para la clase `ProductSpecs` antes de probar las cosas.

Ahora, es el momento de probar nuestra validación. Recuerda que en el archivo de servicio simplemente imprimimos el objeto `createProductDto` entrante. Sin embargo, nunca alcanzaremos esta línea si la entrada no es válida porque la validación ocurre a nivel del controlador.

Para que las cosas funcionen, no olvides añadir la siguiente línea en el archivo `main.ts`, solo para asegurarte de que el tipo de instancia `ValidationPipe` global se aplique a nuestro controlador de productos y a cualquier otro controlador que podamos tener en el futuro:

```typescript
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  // <- add this line
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

Ahora, iniciemos el proyecto en modo de desarrollo una vez más y probémoslo con una entrada válida para comenzar. Aquí está mi entrada, que también puedes usar para probar por tu parte:

```json
{
  "name": "Macbook Pro M3",
  "description": "This product is a Macbook pro with an M3 chip, 16Go of Ram and 1To of SSD, no scratches on it. We also offer 3 months guarantee for this product.",
  "price": "1999.99",
  "image": "https://www.apple.com/newsroom/images/2023/09/apple-unveils-iphone-15-pro-and-iphone-15-pro-max/article/Apple-iPhone-15-Pro-lineup-hero-230912_Full-Bleed-Image.jpg.large.jpg",
  "specs": {
    "ram": "16Go",
    "ssd": "1To",
    "chip": "M3",
    "condition": "like new",
    "brand": "Apple",
    "model": "Macbook Pro",
    "color": "grey",
    "year": "2023"
  }
}
```

Cuando se le proporciona la entrada anterior, la herramienta de IA no se queja y valida la descripción del producto. Puedo ver el registro en mi terminal, una señal de que llegó al archivo de servicio y la entrada está lista para ser persistida en la base de datos.

Intentemos hacer algo mal y ver cómo responden nuestros validadores. Usemos la siguiente entrada:

```json
{
  "name": "Macbook Pro M3",
  "description": "This product is a Macbook pro with an M3 Pro Ultra Max chip, 32To of Ram and 100To of SSD, no scratches on it. We also offer 3 months guarantee for this product. We also give you a possibility to upgrade to newer models in the future",
  "price": "1999.99",
  "image": "https://www.apple.com/newsroom/images/2023/09/apple-unveils-iphone-15-pro-and-iphone-15-pro-max/article/Apple-iPhone-15-Pro-lineup-hero-230912_Full-Bleed-Image.jpg.large.jpg",
  "specs": {
    "something": "16Go",
    "ssd": "1To",
    "chip": "M3",
    "condition": "like new",
    "this_is_an_incorrect_spec": "Apple",
    "model": "Macbook Pro",
    "color": "grey",
    "year": "2023"
  }
}
```

Como todos podemos coincidir, a día de hoy, M3 Pro Ultra Max no es un chip de Apple válido. 32 To de RAM y 100 To de SSD parecen engañosos y no existen. Veamos la respuesta que obtenemos. La Figura 12.4 muestra lo que obtengo en mi aplicación cliente Postman:

![Figura 12.4: Error de validación](https://raw.githubusercontent.com/PacktPublishing/Scalable-Application-Development-with-NestJS/main/images/Figure%2012.4.png)

*Figura 12.4: Error de validación*

Estamos obteniendo el resultado que esperábamos. Nuestra herramienta de IA ha podido identificar información engañosa y rechazar la entrada. Además, el arreglo de especificaciones no sigue el formato deseado. Por ejemplo, el elemento `"this_is_an_incorrect_spec":"Apple"` es incorrecto, por lo que todo funciona como se esperaba.

También puedes intentar agregar algunas palabras de moda (*buzzwords*) o contenido ofensivo en la descripción y ver cómo reacciona el modelo de IA. Siéntete libre de jugar con Gemini y probar su precisión.

##### Tu turno

Si bien este es un punto de partida sólido, todavía hay margen de mejora. Imagina un escenario en el que la descripción menciona 24 Go de RAM, pero en las especificaciones del producto dice 8 Go. Alternativamente, imagina a un cliente describiendo el producto como más barato que un cargador de teléfono cuando el campo de precio dice $2,000. Ambos ejemplos incluyen información claramente engañosa. En la implementación actual, solo tomamos la descripción en sí como entrada, pero podría ser interesante ver cómo reacciona el modelo de IA cuando analizamos más de una entrada para verificar la descripción combinada con los detalles del producto, como las especificaciones, el precio, etc. Actualiza el validador personalizado de descripción de producto para que la herramienta de IA pueda validar la descripción combinada con los detalles del producto. Puedes encontrar la solución en el repositorio de GitHub de este capítulo.

Para completar el trabajo en el servicio de productos, solo debemos persistir datos válidos en nuestra tabla de productos. Acabamos de ver que solo las entradas válidas llegan al archivo de servicio, lo cual es el comportamiento preferido.

Antes de comenzar a interactuar con la base de datos a través de la clase `ProductEntity`, debemos informarle a TypeORM de su existencia. Recuerda que en la configuración de TypeORM teníamos este arreglo:

```typescript
// app.module.ts
// ...
TypeOrmModule.forRoot({
  entities: [],
  // ...
})
// ...
```

A partir de ahora, el arreglo `entities` está vacío. Necesitamos especificar qué entidades tenemos en todo el sistema proporcionando una lista de entidades en el arreglo `entities`. TypeORM nos da la posibilidad de colocar clases de entidades en el arreglo, o rutas de directorio desde donde cargarlas (usando el patrón glob, sobre el cual puedes aprender en [https://developers.tetrascience.com/docs/common-glob-pattern](https://developers.tetrascience.com/docs/common-glob-pattern)).

En nuestro caso, dado que seguimos una convención de nomenclatura que nos obliga a nombrar todas las entidades en el formato `<module_name>.entity.ts`, podemos actualizar nuestro arreglo de entidades de la siguiente manera:

```typescript
// app.module.ts
// ...
entities: ['/**/*.entity{.ts,.js}'], // we could list entities one by one using entities: [ProductEntity],
// ...
```

Es importante tener en cuenta que una vez que el módulo TypeORM ha sido configurado en la clase `AppModule` usando `forRoot()` o `forRootAsync()`, nos da acceso a un objeto `DataSource` que contiene toda la información sobre la configuración de TypeORM, incluyendo que en cualquier parte del proyecto podemos inyectar una instancia de la clase `DataSource` sin necesidad de importarla. Vale la pena saber esto, ya que necesitaremos tener acceso a la clase de entidad en la clase de servicio a través del objeto `dataSource`.

Actualicemos ahora el método `create` de `ProductsService` usando el siguiente código:

```typescript
// src/products/products.service.ts
import { Injectable } from '@nestjs/common';
import { CreateProductDto } from './dto/create-product.dto';
import { InjectRepository } from '@nestjs/typeorm';
import { Product } from './entities/product.entity';
import { Repository } from 'typeorm';

@Injectable()
export class ProductsService {
  constructor(
    @InjectRepository(Product)
    private productRepository: Repository<Product>,
  ) {}

  async create(createProductDto: CreateProductDto) {
    const product = this.productRepository.create(createProductDto);
    await this.productRepository.save(product);
    return {
      message: 'Product created successfully',
      product,
    };
  }
}
```

En el fragmento de código anterior, en comparación con la versión anterior, hemos realizado algunos cambios:

- Inyectamos el `productRepository` de tipo `Repository<Product>` usando el decorador `@InjectRepository()`.
- Convertimos la función `create` en una operación asíncrona (`async`), ya que ahora estamos realizando una llamada asíncrona con el método `save()` del repositorio.
- Usando la instancia de `Repository`, estamos guardando el objeto `createProductDto` siguiendo dos pasos. El primero consiste en crear una nueva instancia de la entidad `Product` a partir del DTO recibido, y el segundo es guardar esa entidad creada en la base de datos usando el método `save()`.
- Luego devolvemos un objeto con un mensaje para mostrar al cliente y el producto que acabamos de guardar. Además, envolví la llamada a la base de datos en un bloque `try ... catch` para capturar cualquier excepción que pueda ocurrir durante la operación.

Antes de sumergirnos en probar nuestra operación de creación de productos, hagamos una cosa más. Necesitamos especificar el repositorio de productos en el módulo de productos (`ProductsModule`) con el siguiente código:

```typescript
// products/products.module.ts
@Module({
  // <- this line
  imports: [TypeOrmModule.forFeature([Product])],
  controllers: [ProductsController],
  providers: [ProductsService],
})
export class ProductsModule {}
```

Al hacerlo, le estamos diciendo al módulo `ProductsModule` qué repositorio necesitamos para este ámbito. NestJS se encargará de crear una instancia de ese repositorio y la pondrá a disposición de cualquier proveedor declarado en ese módulo.

Estamos listos. Probemos este proceso una vez más, iniciando el proyecto en modo de desarrollo usando `$ yarn start:dev`.

Usemos la entrada correcta que usamos anteriormente para probar los validadores y veamos la respuesta que obtenemos en Postman:

![Figura 12.5: Respuesta de la API con un cuerpo de producto válido](/images/Figure_12.5.png)

*Figura 12.5: Respuesta de la API con un cuerpo de producto válido*

Como sugiere la Figura 12.5, la base de datos aceptó mis cambios y ha persistido los datos en la base de datos.

Sin embargo, es posible que necesitemos verificar que el producto se haya persistido. Para hacerlo, trabajemos en nuestros *getters* de productos en el servicio de productos.

El objetivo aquí es hacer lo siguiente:

- Obtener una lista paginada de productos a través del endpoint `HTTP GET /products`
- Obtener un solo producto con su ID a través del endpoint `HTTP GET /products/{id}`

Para lograr estos objetivos, agreguemos dos funciones más en nuestros archivos de servicio como en el siguiente fragmento de código:

```typescript
// ... the rest of the code remains the same
async getProducts(page = 1, limit = 10) {
  const products = await this.productRepository.find({
    skip: (page - 1) * limit,
    take: limit,
  });
  return products;
}

async getProductById(id: string) {
  const product = await this.productRepository.findOne({
    where: { id },
  });
  return product;
}
```

En el fragmento de código anterior, hemos añadido dos métodos más. El primero busca una lista de productos paginada usando las opciones `skip` y `take` de TypeORM, y el segundo busca un solo producto por su ID usando la opción `where`.

##### Tu turno

Necesitamos consumir estos dos métodos, creando sus manejadores correspondientes en la clase del controlador. Como recordatorio, usamos el decorador `@Get()` para definir un endpoint GET, `@Param('id')` para extraer el parámetro de ruta ID y `@Query('page')` para extraer los parámetros de consulta de la solicitud. En el controlador de productos, crea los manejadores de ruta correspondientes y pruébalos.

Con los manejadores en su lugar, prueba las cosas una vez más. Las Figuras 12.6 y 12.7 muestran lo que deberías obtener como respuesta para los endpoints `/products` y `/products/{id}`, respectivamente:

![Figura 12.6: Salida para el endpoint /products](/images/Figure_12.6.png)

*Figura 12.6: Salida para el endpoint /products*

![Figura 12.7: Salida para el endpoint /product/{id}](/images/Figure_12.7.png)

*Figura 12.7: Salida para el endpoint /product/{id}*

Bien hecho, hemos logrado un gran hito. El servicio de productos parece responder perfectamente. Sin embargo, no hemos terminado. Pasemos a los endpoints de pedidos en la siguiente sección.

#### Tómate un café

Hemos explorado muchos conceptos nuevos y muchos repasos en nuestra base de código. ¡Eso vale un café! Tómate un breve descanso antes de pasar a la siguiente sección. ¡Nos vemos en unos minutos!

#### Implementación de los endpoints de la API de pedidos

Para implementar los endpoints de la API de pedidos, seguiremos una estructura similar a la de nuestra API de productos. Primero, generemos el recurso de pedidos usando la CLI de Nest ejecutando el siguiente comando:

```bash
$ nest generate resource orders
```

El comando anterior generará una nueva carpeta bajo el directorio `src/`, con la conocida estructura de carpetas de NestJS (un controlador, un servicio, un módulo, DTOs y carpetas de entidades).

Ahora, trabajemos en la entidad de pedidos en la siguiente sección.

##### La entidad Order

El archivo de entidad se puede encontrar en la carpeta generada `src/orders/entities/`. Personalicémoslo siguiendo las necesidades del negocio.

Antes de profundizar más, debemos recordar que la entidad `Product` en realidad no tiene ninguna relación con otras entidades en nuestra base de datos. Sin embargo, en el mundo real, un producto puede pertenecer a una o más órdenes, y una orden puede tener uno o más productos (en una relación de muchos a muchos) o varios productos pueden pertenecer a una sola orden. Para simplificar las cosas, asumiremos que cada pedido contiene un solo producto por ahora (una relación de muchos a uno).

- **Actualizar la entidad Order:** En la clase de entidad generada, pega el siguiente contenido:

```typescript
// src/orders/entities/order.entity.ts
import { Product } from 'src/products/entities/product.entity';
import {
  Column,
  Entity,
  ManyToOne,
  PrimaryGeneratedColumn,
} from 'typeorm';

export enum OrderStatus {
  PENDING = 'pending',
  PAID = 'paid',
  CANCELLED = 'cancelled',
}

@Entity('orders')
export class Order {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @ManyToOne(() => Product, (product) => product.orders)
  product: Product;

  // @ManyToOne(() => User, (user) => user.orders)
  // customer: User;

  @Column({ default: 1 })
  quantity: number;

  @Column('decimal', { default: 0.0 })
  totalPrice: number;

  @Column({
    type: 'enum',
    enum: OrderStatus,
    default: OrderStatus.PENDING,
  })
  status: OrderStatus;

  @Column({ type: 'timestamp', default: () => 'CURRENT_TIMESTAMP' })
  createdAt: Date;
}
```

Como puedes ver en el fragmento de código anterior, tenemos decoradores normales que ya conocemos (`@Column()`, `@PrimaryGeneratedColumn()` y `@Entity()`). Además, hemos añadido el decorador `@ManyToOne()` que define una relación de muchos a uno entre las entidades `Order` y `Product`. Esto significa que muchos pedidos pueden estar asociados con un solo producto.

Además, ten en cuenta que tenemos una relación comentada entre pedidos y usuarios que es el campo `customer`. Como aún no hemos implementado el módulo de usuarios, mantengámoslo comentado por ahora. Volveremos a él más adelante.

También tenemos `OrderStatus`, que es un objeto enum de los diferentes estados que puede tener un pedido. Los diferentes estados admitidos son `pending` (pendiente), `paid` (pagado) y `cancelled` (cancelado).

La otra cosa a tener en cuenta antes de seguir adelante y actualizar la entidad del producto es que esto creará una relación bidireccional entre las tablas de productos y pedidos. Esto significa que podemos acceder a los pedidos de un producto desde la entidad del producto, y viceversa.

- **Actualizar la entidad Product:** Para reflejar lo que acabamos de indicarle a la entidad de pedidos (que la entidad de productos tiene un campo llamado `orders`), necesitamos agregar el campo `orders` a la entidad `Product` de la siguiente manera:

```typescript
// src/products/entities/product.entity.ts
// ...
@OneToMany(() => Order, (order) => order.product)
orders: Order[];
// ...
```

Por este lado, hemos creado un campo `orders` anotado con el decorador `@OneToMany()` en su lugar. Esto no es obligatorio; podríamos haber dejado la relación solo en un lado (en la entidad `Order`) como en el siguiente fragmento de código:

```typescript
@ManyToOne(() => Product) // <- without the product.orders reference
product: Product;
```

En nuestro caso, dado que tenemos operaciones como `HTTP GET /product/{id}/orders`, para una mejor separación de conceptos, es mejor tener un campo `orders` dentro de la entidad `Product`.

Bien hecho, este es un buen punto de partida. Ahora inicia la aplicación una vez más y observa la tabla generada con sus columnas en pgAdmin.

Ahora que la entidad de pedidos se ve bien, trabajemos en el archivo de servicio y creemos operaciones simples como crear un pedido, obtener todos los pedidos y obtener un solo pedido por su ID.

##### Creación de la lógica de negocio: el archivo de servicio de pedidos

La CLI de Nest generó un archivo de servicio para nosotros. Usémoslo y actualicemos su contenido actual siguiendo los requisitos de nuestra aplicación.

Primero, necesitamos actualizar el constructor para poder inyectar tanto la instancia de `productRepository` como la de `orderRepository` con el siguiente fragmento de código:

```typescript
// order.entity.ts
// the constructor
constructor(
  @InjectRepository(Order)
  private orderRepository: Repository<Order>,
  @InjectRepository(Product)
  private productRepository: Repository<Product>,
) {}
```

Con estas inyecciones en su lugar, tenemos la capacidad de acceder a ellas en cualquier lugar de la clase. Consumámoslas, implementando el método de servicio de creación de pedidos.

Luego debemos actualizar la clase `CreateOrderDto` generada. La versión actual es una clase vacía, así que actualicémosla con el siguiente contenido:

```typescript
// src/orders/dto/create-order-dto.ts
import { IsDecimal, IsPositive, IsString, IsUUID } from 'class-validator';

export class CreateOrderDto {
  @IsString({ message: 'customerId must be a string' })
  // @IsUUID('all', { message: 'customerId must be a // valid UUID' })
  customerId: string;

  @IsString({ message: 'productId must be a string' })
  @IsUUID(
    'all',
    { message: 'productId must be a valid UUID' }
  )
  productId: string;

  @IsPositive({ message: 'quantity must be a positive number' })
  quantity: number;

  @IsDecimal(
    { decimal_digits: '2' },
    { message: 'totalPrice must be a decimal number' },
  )
  totalPrice: number;
}
```

Este es un archivo de validación simple. Observa cómo comenté la validación de `customerId`. Como aún no tenemos un UUID válido para los clientes, podemos usar texto plano para simular la petición por ahora.

Luego, regresemos a la implementación del método de creación de pedidos. Necesitaremos ambos repositorios para asegurarnos de que el producto exista, gracias al repositorio de productos, antes de guardar el objeto de pedido en la base de datos, gracias al repositorio de pedidos. El método de creación de pedidos se verá como el siguiente código:

```typescript
// orders.service.ts > the create method
async create(createOrderDto: CreateOrderDto) {
  try {
    const { productId, quantity, totalPrice } = createOrderDto;
    const product = await this.productRepository.findOne({
      where: {
        id: productId,
      },
    });

    if (!product) {
      return {
        message: 'Product not found!',
      };
    }

    const order = this.orderRepository.create({
      product,
      quantity,
      totalPrice,
    });

    await this.orderRepository.save(order);

    return {
      message: 'Order created successfully',
      order,
    };
  } catch (error) {
    return {
      message: 'An error occurred!',
      error,
    };
  }
}
```

En la implementación anterior, primero verificamos si el producto existe antes de crear el pedido. Luego ponemos todo en un bloque `try ... catch` por si acaso sucediera algo durante la ejecución del código, como por ejemplo si la base de datos no estuviera disponible por cualquier motivo. Luego enviamos la respuesta al cliente, dependiendo de cómo vaya la petición (en el bloque `try` o en el `catch`).

Bien hecho. Antes de probar las cosas, implementemos algunos *getters*.

Implementemos los *getters* de pedidos de la misma manera que implementamos los de productos. Para esto, necesitaremos dos métodos más. En el archivo de servicio, añade el siguiente código:

```typescript
async getOrders(page = 1, limit = 10) {
  const orders = await this.orderRepository.find({
    skip: (page - 1) * limit,
    take: limit,
    relations: [
      'product',
      // 'customer', <-- Uncomment this line after // creating the User entity
    ],
  });
  return {
    orders,
  };
}

async getOrderById(id: string) {
  const order = await this.orderRepository.findOne({
    where: { id },
    relations: [
      'product',
      // 'customer', <-- Uncomment this line after // creating the User entity
    ],
  });
  return {
    order,
  };
}
```

El primero es `getOrders`, que devuelve una lista paginada de pedidos. Luego, con el parámetro `relations` en las opciones del método `find()`, le pedimos al método que obtenga el producto correspondiente a cada pedido por nosotros. Ten en cuenta que esto devolverá todos los campos del producto. Puedes elegir los que consideres más relevantes para el cliente usando la opción `select`, como en el siguiente código:

```typescript
// select product's fields you need to send
const orders = await this.orderRepository.find({
  skip: (page - 1) * limit,
  take: limit,
  relations: [
    'product',
    // 'customer', <-- Uncomment this line after // creating the User entity
  ],
  select: {
    product: {
      name: true,
      price: true,
      image: true,
    },
  },
});
```

Al especificar estos campos, solo obtendrás productos con los campos que deseas. Esto puede reducir el ancho de banda de la petición.

Dado que estamos utilizando tanto `productRepository` como `orderRepository`, debemos informarle a `OrdersModule` que necesitaremos las características de estas entidades, tal como hicimos para `ProductsModule`. Actualicemos `OrdersModule` con el siguiente contenido:

```typescript
// order.module.ts
@Module({
  imports: [TypeOrmModule.forFeature([Order, Product])],
  controllers: [OrdersController],
  providers: [OrdersService],
})
export class OrdersModule {}
```

Esto realmente hará que el repositorio correspondiente esté disponible en todas partes dentro de los proveedores registrados del módulo `OrdersModule`.

Nos estamos acercando. Ahora trabajemos en consumir esos archivos de servicio en el archivo `OrdersController` antes de que podamos probarlo todo.

##### Controlador de pedidos

En el archivo generado `orders.controller.ts`, implementemos el archivo de servicio que creamos en la sección anterior.

Necesitamos un controlador `@Post()` para crear un pedido y un controlador `@Get()` para obtener uno o varios pedidos. Sigamos adelante y reemplacemos el código generado con lo siguiente:

```typescript
// src/orders/orders.controller.ts
import { Controller, Post, Body, Get, Param, Query } from '@nestjs/common';
import { OrdersService } from './orders.service';
import { CreateOrderDto } from './dto/create-order.dto';

@Controller('orders')
export class OrdersController {
  constructor(
    private readonly ordersService: OrdersService
  ) {}

  @Post()
  create(@Body() createOrderDto: CreateOrderDto) {
    return this.ordersService.create(createOrderDto);
  }

  @Get()
  getOrders(
    @Query('page') page: number,
    @Query('limit') limit: number
  ) {
    return this.ordersService.getOrders(page, limit);
  }

  @Get(':id')
  getOrderById(@Param('id') id: string) {
    return this.ordersService.getOrderById(id);
  }
}
```

Esto se parece casi exactamente a lo que hicimos para los productos, ¿verdad? Esto es bueno; nuestro módulo de pedidos está listo para ser probado.

Asegúrate de iniciar el servidor en modo de desarrollo una vez más, luego usa el siguiente objeto en el cuerpo (*body*) para probar la llamada a la API de creación de pedidos:

```json
{
  "productId": "043ba968-ea49-487e-a8c0-3cf8a8ffca19",
  "customerId": "fake-customer-id",
  "quantity": 4,
  "totalPrice": "124.53"
}
```

Ten en cuenta que estamos usando un ID de cliente simulado por ahora (comentamos la validación para este campo), pero enviar una petición usando cualquier aplicación cliente debería funcionar.

Para verificar que hemos persistido con éxito el pedido en la base de datos, prueba tanto `HTTP GET /orders` como `HTTP GET /orders/{id}` y asegúrate de obtener los resultados esperados.

Bien hecho, tenemos un servicio de pedidos que funciona muy bien. Sin embargo, observa cómo tenemos algunos tipos repetitivos, como los parámetros que pasamos en las peticiones paginadas. Además, la respuesta que enviamos al cliente no es uniforme. Cuando estábamos diseñando, decidimos usar una estructura de objeto única para devolver datos a los clientes. Esto creará confianza entre el cliente y el servidor de modo que, independientemente de las actualizaciones que podamos implementar en el futuro, reflejará exactamente la misma estructura. Esto evitará que el cliente falle cada vez que realicemos actualizaciones en el lado del servidor. Trabajemos en ello en la siguiente sección.

#### Lógica común e interceptores

Esta sección nos ayudará a identificar qué lógica común y compartida tenemos actualmente en la aplicación, así como qué posibles lógicas compartidas futuras podríamos tener. Luego, nos centraremos en cómo deberíamos estructurarlas en el proyecto. Esto nos presentará algo que ya conocemos: los interceptores.

##### Módulo común

Para nuestra lógica compartida, coloquémosla en `CommonModule`. Crea un nuevo módulo usando la CLI de Nest mediante `$ nest generate module common`. Este comando generará un nuevo módulo y lo vinculará a la clase `AppModule`.

Actualmente, cuando enviamos una respuesta paginada al cliente, simplemente enviamos el arreglo de pedidos o productos, lo cual es bueno pero no óptimo. En la mayoría de los casos, el cliente necesita tener acceso a más información, como el número total de páginas (para que un cliente frontend pueda decidir cuántas páginas mostrar en su componente de paginación), la página actual y el límite actual. Recuerda que nuestra aplicación usa valores predeterminados para el límite y las páginas.

Dado que esta lógica se repetirá en múltiples módulos, creemos una clase `PaginationService` dentro del módulo común siguiendo estos pasos:

1. En el archivo `src/common/pagination/pagination.service.ts`, pega el siguiente contenido:

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class PaginationService {
  getPaginationMeta(
    page: number,
    limit: number,
    totalItems: number
  ) {
    const totalPages = Math.ceil(totalItems / limit);
    return {
      page,
      limit,
      totalPages,
    };
  }
}
```

En el código anterior, calculamos el número total de páginas en función del límite y el número total de elementos que hemos proporcionado.

2. Ahora, necesitamos exportar este servicio desde el módulo común para que esté disponible fuera de su ámbito. Para hacerlo, necesitamos actualizar el módulo generado `CommonModule` con el siguiente código:

```typescript
import { Module } from '@nestjs/common';
import { PaginationService } from './pagination/pagination.service';

@Module({
  providers: [PaginationService],
  exports: [PaginationService],
})
export class CommonModule {}
```

Aquí, simplemente exportamos la clase `PaginationService` y la declaramos como un proveedor para que pueda inyectarse en el archivo de servicio de otro módulo.

3. A continuación, debemos importarlo desde los módulos `OrdersModule` y `ProductsModule` antes de usarlo en sus respectivos archivos de servicio. Esto es lo que debemos hacer:

```typescript
// orders.module.ts
@Module({
  imports: [
    TypeOrmModule.forFeature([Order, Product]),
    CommonModule
  ],
  controllers: [OrdersController],
  providers: [OrdersService, PaginationService],
})
export class OrdersModule {}

// orders.service.ts
constructor(
  @InjectRepository(Order)
  private orderRepository: Repository<Order>,
  @InjectRepository(Product)
  private productRepository: Repository<Product>,
  private readonly paginationService: PaginationService,
) {}
```

Aquí, importamos la clase `CommonModule` en el módulo `OrdersModule`, y luego añadimos la clase `PaginationService` como proveedor. Esto nos permitió inyectar su instancia en el servicio de pedidos y asegurarnos de que estuviera lista para ser consumida allí. Haz lo mismo para el servicio de productos antes del último paso.

4. Finalmente, lo consumimos actualizando el objeto que devolvemos. En lugar de devolver un arreglo de pedidos, devolvamos un objeto con un arreglo de pedidos más un objeto `meta` devuelto por el servicio de paginación, como sugiere el siguiente código:

```typescript
// ... the rest remains the same
const totalItems = await this.orderRepository.count();
const meta = this.paginationService.getPaginationMeta(
  page,
  limit,
  totalItems,
);
return {
  orders,
  meta,
};
```

Ahora que tenemos un objeto diferente devuelto, regresemos a nuestra aplicación cliente Postman, probemos el endpoint de obtención de pedidos una vez más y observemos que ahora se devuelve un objeto `meta`.

Esto se ve bien, pero podemos mejorarlo aún más. Formalicemos la respuesta que enviamos al cliente en la siguiente sección.

##### Interceptor de respuesta de la API

El objetivo ahora es devolver siempre una respuesta en una estructura única, pase lo que pase. Necesitamos que nuestra respuesta tenga siempre la misma estructura que la clase `APIResponse` que definimos en la discusión sobre arquitectura.

Para lograr este objetivo, sigamos estos pasos:

1. **Crear una clase `APIResponse`:** Primero lo primero, definamos cómo debería verse la respuesta. En el archivo `src/common/types/APIResponse.ts` (este debe crearse primero), pega el siguiente contenido:

```typescript
export class APIResponse {
  success: boolean;
  data: any;
  error: any;
  message: string | string[];
}
```

Como puedes ver en la clase anterior, estamos definiendo cómo necesitamos que se vea nuestra respuesta. Este será un contrato entre nuestro servidor y nuestros posibles clientes. En la documentación en la que trabajamos, ya se especifica a los clientes que van a recibir respuestas con esa estructura. Cumplamos ese deseo ahora.

2. **Crear una clase `APIResponseInterceptor`:** Ahora, creemos nuestro archivo de interceptor, ubicado en `common/interceptors/api-response-interceptor.ts`, y peguemos el siguiente contenido:

```typescript
// src/common/interceptors/api-response-interceptor.ts
import {
  CallHandler,
  ExecutionContext,
  Injectable,
  NestInterceptor,
} from '@nestjs/common';
import { Observable, map } from 'rxjs';
import { APIResponse } from '../types/APIResponse';

@Injectable()
export class APIResponseInterceptor implements NestInterceptor {
  intercept(
    context: ExecutionContext,
    handler: CallHandler<any>,
  ): Observable<any> | Promise<Observable<any>> {
    return handler.handle().pipe(
      map((data): APIResponse => {
        const message =
          data && data.message ? data.message : 'Request successful';
        if (data?.message) delete data.message;
        const data_ =
          data instanceof Error ? null : data?.data ? data.data : data;
        const success = !(data instanceof Error) && data_ !== null;
        return {
          success,
          data: data_,
          error: data instanceof Error ? data : null,
          message,
        };
      }),
    );
  }
}
```

En el código anterior, implementamos el método del interceptor `NestInterceptor`, cuyo objetivo es interceptar la petición antes de que llegue al manejador de ruta, y la respuesta justo después del manejador. El manejador que implementamos es tan básico que simplemente necesitamos actualizar la respuesta para que se devuelva al final de la misma, colocando los campos deseados en la respuesta, como `message`, `error`, `data`, etc. También eliminamos el campo `message` de los objetos que ya tienen uno y lo devolvemos como parte del objeto de respuesta en su lugar.

3. Ahora, dado que necesitamos que este interceptor intercepte todas las respuestas entrantes, actualicemos el archivo `main.ts` agregando la siguiente línea:

```typescript
// ... the rest remains the same
app.useGlobalInterceptors(
  new APIResponseInterceptor()
); // <- add this line
```

Ten en cuenta que puedes tener un interceptor de ámbito restringido. Supongamos que queremos que solo intercepte peticiones de productos; para hacerlo, podemos actualizar el módulo añadiendo el siguiente decorador:

```typescript
@Controller('products')
@UseInterceptors(new APIResponseInterceptor())
export class ProductsController {}
```

##### Pruébalo

Con nuestro interceptor implementado, intenta realizar algunas peticiones a la API y observa el comportamiento. ¿Estás obteniendo la respuesta esperada? Si no es así, asegúrate de revisar los pasos anteriores.

Al interactuar con la API, es posible que hayas notado algo. A veces obtenemos las respuestas integradas de Nest, por ejemplo, cuando falla la validación o cuando tenemos un error interno del servidor. La razón es que la petición no llega al manejador de ruta y nuestro interceptor no es informado sobre la respuesta hasta que llegamos allí. Este no es nuestro comportamiento esperado, ¿verdad?

Podemos usar el operador `catchError()` de RxJS en el observable que teníamos y devolver una respuesta adecuada. Afortunadamente, NestJS proporciona una forma elegante de manejar esto mediante la implementación de filtros de excepciones personalizados. Implementemos uno para nosotros en la siguiente sección.

##### Filtros de excepciones

El objetivo aquí es interceptar cualquier excepción generada a nivel de API y seguir enviando la misma estructura de datos en la respuesta (siguiendo nuestra clase `APIResponse`).

Para lograr esto, creemos otro archivo ubicado en `common/exception-filters/http-exception.filter.ts` con el siguiente contenido:

```typescript
import {
  ArgumentsHost,
  Catch,
  ExceptionFilter,
  HttpException,
} from '@nestjs/common';
import { Response } from 'express';
import { APIResponse } from '../types/APIResponse';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const status = exception.getStatus();
    const errorMessage = exception.getResponse()['message']
      ? exception.getResponse()['message']
      : exception.message || 'Internal server error';
    const error: any =
      exception.getResponse() || exception.getResponse()['error'];
    delete error['message'];
    const body: APIResponse = {
      success: false,
      message: errorMessage,
      data: null,
      error,
    };
    response.status(status).json(body);
  }
}
```

Aquí, hemos usado el decorador `@Catch()` con la clase `HttpException` como parámetro. Esto le indica a nuestra excepción que solo busque hasta `HttpException` (y sus derivados) e inyecte los metadatos requeridos en esta clase, como el contexto de ejecución.

Luego, implementamos el método `catch()` con algo de lógica en su interior. La lógica tiene acceso al contexto y al host (inyectado por el decorador `@Catch()`) y, con algunas manipulaciones, podemos actualizar el cuerpo de la respuesta sin cambiar su estado HTTP.

Esto capturará cualquier excepción `HttpException` y, en lugar de obtener la respuesta integrada, obtendremos la respuesta que esperamos.

Para aplicar este filtro de forma global, podemos seguir las mismas técnicas que usamos para el interceptor, añadiendo esta línea en el archivo `main.ts`:

```typescript
app.useGlobalFilters(new HttpExceptionFilter());
```

Esto aplicará el filtro en cada controlador y manejador de ruta que tengamos en el proyecto.

Probemos las cosas enviando un objeto incorrecto a la creación de productos. Aquí simplemente agregaré una especificación `some_spec` en el objeto `specs`, lo cual es incorrecto. La Figura 12.8 muestra lo que obtuve como respuesta del cliente Postman:

![Figura 12.8: Respuesta manejada por el filtro de excepciones](https://raw.githubusercontent.com/PacktPublishing/Scalable-Application-Development-with-NestJS/main/images/Figure%2012.8.png)

*Figura 12.8: Respuesta manejada por el filtro de excepciones*

Como puedes ver en esta figura, la respuesta ya no es la integrada, sino nuestra instancia de `APIResponse`.

Bien hecho, nos estamos acercando. Antes de continuar con la autenticación y la autorización, veamos un ejercicio.

##### Algunos desafíos para ti

De la misma manera que hicimos para los servicios de pedidos y productos, implementemos la lógica básica para el servicio de reseñas (*Review*), siguiendo el mismo camino. Siempre puedes volver al repositorio y ver la implementación. En primer lugar, te recomendamos que lo intentes tú mismo. Recuerda, la práctica hace al maestro :).

Avanzando, es hora de proteger nuestros endpoints de clientes no autorizados. Esos clientes serán usuarios como administradores, clientes, comerciantes o desarrolladores que interactúan con nuestras APIs. En la siguiente sección, construyamos las capas de autorización y autenticación de nuestro sistema.

---

### Autenticación y autorización de usuarios en el contexto del comercio electrónico

En una aplicación de comercio electrónico, la autenticación y autorización de usuarios son críticas por varias razones.

Esta es una lista de algunos ejemplos de la importancia de implementar la autenticación y la autorización en tu API. Algunas de estas razones ya nos resultan familiares, ya que exploramos el tema en los Capítulos 5 y 6:

- **Seguridad:** La autenticación garantiza que solo los usuarios registrados puedan acceder a ciertas funciones de la aplicación, como realizar pedidos o ver el historial de pedidos. La autorización garantiza que los usuarios solo puedan acceder a los recursos a los que tienen permiso de acceso, evitando acciones no autorizadas.
- **Experiencia de usuario:** Los mecanismos adecuados de autenticación y autorización mejoran la experiencia de usuario al permitirles administrar sus cuentas, ver contenido personalizado y realizar transacciones seguras.
- **Integridad de datos:** Al restringir el acceso a operaciones y datos sensibles, estos mecanismos ayudan a mantener la integridad de los datos de los usuarios, evitando modificaciones no autorizadas o violaciones de seguridad de datos.
- **Cumplimiento normativo (*Compliance*):** Muchas regulaciones y estándares, como el Reglamento General de Protección de Datos (*General Data Protection Regulation* - GDPR) y los Estándares de Seguridad de Datos de la Industria de Tarjetas de Pago (*Payment Card Industry Data Security Standards* - PCI DSS), requieren mecanismos robustos de autenticación y autorización para proteger los datos y la privacidad del usuario.

Tenemos todo para ganar al implementar la autenticación y la autorización en nuestro sistema. Además, dado que no hemos trabajado en el módulo de usuarios, es momento de hacerlo ahora.

Implementaremos la autenticación y autorización de usuarios utilizando JSON Web Tokens (JWT) para una autenticación segura y sin estado (*stateless*).

#### Configuración del módulo de usuarios

Primero, necesitamos crear el módulo, servicio y controlador de usuario. Usando la CLI de Nest, generemos el recurso de usuario con el siguiente comando:

```bash
$ nest generate resource users
```

Este comando generará archivos de plantilla (*boilerplate*) para nosotros en `src/users`, como de costumbre.

#### Creación de la entidad User

Actualiza el archivo de entidad generado con el siguiente código:

```typescript
import { Order } from 'src/orders/entities/order.entity';
import { Column, Entity, OneToMany, PrimaryGeneratedColumn } from 'typeorm';

export enum UserRole {
  ADMIN = 'admin',
  USER = 'user',
  MERCHANT = 'merchant',
}

@Entity()
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ unique: true })
  email: string;

  @Column({ unique: true })
  username: string;

  @Column()
  password: string;

  @OneToMany(() => Order, (order) => order.customer)
  orders: Order[];

  // @OneToMany(() => Review, (review) => review.user)
  // reviews: Review[];

  @Column({ type: 'enum', enum: UserRole, default: UserRole.USER })
  role: UserRole;

  @Column({ type: 'timestamp', default: () => 'CURRENT_TIMESTAMP' })
  createdAt: Date;
}
```

El archivo anterior es un archivo de entidad normal con columnas y una relación `OneToMany` con el servicio de pedidos (un usuario puede tener varios pedidos). El resto del contenido es muy similar a lo que ya conocemos. Como tenemos una relación `OneToMany` en el modelo de usuario, debemos crear un `@ManyToOne()` asociado en la entidad de pedidos. Sin embargo, recuerda que ya lo tenemos. Solo asegúrate de descomentar esta línea y actualizar la importación en el archivo `order.entity.ts`:

```typescript
// orders/entities/order.entity.ts
@ManyToOne(() => User, (user) => user.orders)
customer: User;
```

Bien hecho, nuestra entidad está lista. Antes de continuar, recuerda que debemos registrarla en el contexto del módulo `UserModule` para poder tener acceso a su repositorio.

##### Pruébalo

Registra la clase `User` en `UserModule` utilizando la clase `TypeOrmModule` para que podamos tener acceso a su repositorio. Siempre puedes consultar la rama `feat/auth` para ver el resultado.

Ahora, trabajemos en la lógica de negocio.

#### Implementación del archivo de servicio

Necesitamos implementar las características principales del módulo de usuarios. Para esto, ignoremos las consideraciones de seguridad y contraseñas por ahora; nos enfocaremos en eso en la sección de autenticación.

Ahora, en el archivo `users/users.service.ts`, pega el siguiente contenido:

```typescript
// src/users/users.service.ts
@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>,
  ) {}

  async create(user: CreateUserDto) {
    try {
      const newUser = this.userRepository.create(user);
      await this.userRepository.save(newUser);
      return newUser;
    } catch (error) {
      throw new Error(error);
    }
  }

  async getUserById(id: string) {
    return await this.userRepository.findOne({
      where: { id },
      relations: ['orders'],
    });
  }

  async getUserByUsername(username: string) {
    return await this.userRepository.findOne({
      where: { username },
      relations: ['orders'],
    });
  }
}
```

En el fragmento de código anterior, tenemos un archivo de servicio con cierta interacción con la base de datos a través de `userRepository`. Hemos definido tres métodos: `create` para crear un nuevo usuario, `getUserById` para recuperar un usuario por su ID (incluyendo sus pedidos relacionados) y `getUserByUsername` para recuperar un usuario por su nombre de usuario.

Con el archivo de servicio listo, trabajemos en el manejador de rutas que va a consumir esta lógica de negocio.

#### Manejadores de rutas y el archivo del controlador

Cada método de servicio debería asociarse idealmente con su manejador de ruta; mientras trabajamos en el controlador, necesitaremos crear un objeto de Transferencia de Datos (*Data Transfer Object* - DTO) para validar los datos que entran a nuestros manejadores de rutas.

Necesitamos validar la entrada que recibimos del cliente tanto en las peticiones de registro (*signup*) como de inicio de sesión (*login*). En la carpeta `src/users/dto`, crea un archivo `create-user.dto.ts` y pega el siguiente código:

```typescript
// src/users/dto/create-user.dto.ts
import { IsEmail, IsString, IsStrongPassword, Length } from 'class-validator';

export class CreateUserDto {
  @IsString()
  username: string;

  @IsString()
  @Length(8, 20)
  @IsStrongPassword(
    {
      minLength: 8,
      minLowercase: 1,
      minUppercase: 1,
      minNumbers: 1,
      minSymbols: 1,
    },
    {
      message:
        'password should contain at least one lowercase letter, one uppercase letter, one number, and one symbol',
    },
  )
  password: string;

  @IsEmail()
  email: string;
}
```

El archivo anterior le indica al cliente qué estructura de datos esperamos y establece restricciones. Para la contraseña, por ejemplo, esperamos que tenga al menos 8 caracteres de longitud, contenga al menos una letra minúscula, una letra mayúscula, un número y un símbolo.

Teniendo listo el archivo DTO, concentrémonos ahora en el archivo del controlador y peguemos el siguiente contenido:

```typescript
// src/users/user.controller.ts
import { Controller, Get, Post, Body, Param } from '@nestjs/common';
import { UsersService } from './users.service';
import { CreateUserDto } from './dto/create-user.dto';
import { LoginUserDto } from './dto/login-user.dto';

@Controller('users')
export class UsersController {
  constructor(
    private readonly usersService: UsersService
  ) {}

  @Post('create')
  create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.usersService.getUserById(id);
  }

  @Get('username/:username')
  findOneByUsername(@Param('username') username: string) {
    return this.usersService.getUserByUsername(username);
  }
}
```

En el archivo del controlador anterior, hemos creado algunos manejadores de rutas. Cada uno de ellos está asociado con un método de servicio. Siendo los métodos de registro y de inicio de sesión peticiones POST, añadimos el objeto `@Body()` requerido en su DTO esperado.

Bien hecho, ahora es el momento de probar el flujo. Desde cualquier cliente (como la aplicación cliente Postman), realicemos estas acciones y veamos qué obtenemos como respuestas después de iniciar el servidor en modo de desarrollo una vez más.

Por ejemplo, usando la siguiente carga útil (*payload*), la Figura 12.9 muestra lo que obtengo de Postman como respuesta del servidor:

```json
{
  "username": "tester1",
  "password": "newPassword123!",
  "email": "tester1@email.com"
}
```

El fragmento anterior es la carga útil para las pruebas. Aquí está la respuesta:

![Figura 12.9: Petición de registro (signup) con datos válidos](/images/Figure_12.9.png)

*Figura 12.9: Petición de registro (signup) con datos válidos*

Cuando intento crear el mismo nombre de usuario dos veces, dado que se supone que es único, la Figura 12.10 muestra la respuesta que obtengo:

![Figura 12.10: Respuesta del servidor ante un intento de duplicar un nombre de usuario](/images/Figure_12.10.png)

*Figura 12.10: Respuesta del servidor ante un intento de duplicar un nombre de usuario*

Cuando los datos no se ajustan a las restricciones que establecemos en nuestros DTOs, como una contraseña débil, la Figura 12.11 muestra lo que obtenemos del servidor:

![Figura 12.11: Cuerpo de petición erróneo en el registro](https://raw.githubusercontent.com/PacktPublishing/Scalable-Application-Development-with-NestJS/main/images/Figure%2012.11.png)

*Figura 12.11: Cuerpo de petición erróneo en el registro*

Observa cómo siempre obtenemos la misma estructura de datos, ya sea para peticiones exitosas o fallidas. Esto significa que nuestro interceptor y manejadores de excepciones siguen haciendo un gran trabajo y, por lo tanto, marca otra victoria.

Tenemos nuestro módulo de usuario, pero todavía nos queda un poco de trabajo por hacer en él: hacer el hash de la contraseña y crear un flujo adecuado de inicio de sesión/registro. Estos son aspectos muy importantes cuando se trata de datos secretos de las personas. Nunca debemos conocer las contraseñas de los usuarios y, si el sistema es atacado, los atacantes nunca deben tener acceso a los datos secretos de los usuarios. Aunque están surgiendo mecanismos de autenticación más robustos, todavía es común ver a los usuarios usando una sola contraseña o patrón en todas las plataformas que usan a diario. Por lo tanto, nuestro sistema no debería ser el punto de fallo. Como habrás notado, ignoramos la contraseña que obtuvimos de la petición para usar un valor codificado en duro, bueno, hasta ahora. Protejamos a nuestros usuarios y asegurémonos de darnos prisa…

#### Módulo de autenticación

Si bien es común usar el módulo de usuario para manejar la autenticación y la autorización, la lógica detrás de esto puede volverse tan compleja que mantenerla junto con el módulo de usuario puede convertirse en una pesadilla. Siempre es preferible manejarlo en un módulo separado para que pueda escalarse y mantenerse por separado, o incluso trasladarse a un nuevo microservicio si decides hacerlo en el futuro, manteniendo los módulos de usuarios y autenticación débilmente acoplados y evitando un único punto de fallo en nuestro sistema.

Para comenzar, generemos un nuevo módulo y servicio de autenticación. En lugar de generar todo el módulo con controladores que no necesitaremos, usemos los siguientes comandos:

```bash
# generate the module
$ nest generate module auth

# generate the service file
$ nest generate service auth
```

Estos comandos crearán una nueva carpeta de módulo llamada `auth`, registrarán el nuevo módulo en `AppModule` y registrarán el archivo de servicio como un proveedor en la clase `AuthModule`, tal como lo haría el comando `generate resource`. La diferencia es que estos comandos no agregarán archivos DTO o de entidad que no necesitaremos, al menos por ahora.

Antes de continuar, configuremos la clase `AuthModule`. En el archivo `src/auth/auth.module.ts`, pega el siguiente contenido:

```typescript
@Module({
  providers: [AuthService],
  imports: [
    UsersModule,
    JwtModule.registerAsync({
      inject: [ConfigService],
      useFactory: async (configService: ConfigService) => ({
        secret: configService.get<string>('JWT_SECRET'),
        signOptions: { expiresIn: '24h' },
        global: true,
      }),
    }),
  ],
  controllers: [AuthController],
})
export class AuthModule {}
```

Aquí, nos aseguramos de importar todos los módulos necesarios, como `UsersModule` y `JwtModule`. `JwtModule` se importa desde el paquete nativo `@nestjs/jwt`. Nos ayudará a generar el `accessToken` y verificar su validez en rutas protegidas (hablaremos de esto en detalle más adelante).

Pasando al archivo de servicio de autenticación, primero instalemos los paquetes requeridos. Usaremos `bcrypt` ([https://www.npmjs.com/package/bcrypt](https://www.npmjs.com/package/bcrypt)), una herramienta popular, para cifrar las contraseñas de los usuarios:

```bash
$ yarn add bcrypt
$ yarn add @types/bcrypt --save-dev
```

Con el paquete instalado, actualicemos el archivo `auth.service.ts` con el siguiente contenido:

```typescript
@Injectable()
export class AuthService {
  constructor(
    private readonly userService: UsersService,
    private jwtService: JwtService,
  ) {}

  async login(username: string, password: string) {
    try {
      const user = await this.userService.getUserByUsername(username);
      if (
        await this.verifyPassword(
          user,
          password,
          user.password
        )
      ) {
        delete user.password;
        const accessToken = await this.jwtService.signAsync({
          sub: user.id,
          username: user.username,
        });
        return {
          message: 'Login successful',
          data: {
            ...user,
            accessToken,
          },
        };
      }
      return {
        message: 'Invalid username or password',
        data: null,
      };
    } catch (error) {
      return error;
    }
  }

  async signup(createUserDto: CreateUserDto) {
    try {
      const hashedPassword = this.hashPassword(createUserDto.password);
      const newUser: CreateUserDto = {
        ...createUserDto,
        password: hashedPassword,
      };
      return await this.userService.create(newUser);
    } catch (error) {
      return error;
    }
  }

  hashPassword(password: string) {
    return bcrypt.hashSync(password, 10);
  }

  async verifyPassword(
    user: User,
    password: string,
    hashedPassword: string
  ) {
    return user && (await bcrypt.compare(password, hashedPassword));
  }
}
```

En el código anterior, hemos implementado algunos métodos útiles. Uno de ellos es `verifyPassword()`, que toma la instancia de usuario, la contraseña y la contraseña con hash, y luego verifica que el usuario proporcionado no sea nulo. Luego comparamos la contraseña con hash almacenada con la contraseña en texto plano pasada en la solicitud utilizando la función `compare` de `bcrypt`. Si la contraseña coincide con el hash almacenado, devolverá `true`. De lo contrario, devolverá `false`. Esto debe usarse en la función `login` del servicio de autenticación. También es importante usar la función `hashPassword()`, que toma una contraseña en cadena de texto plano y devuelve su valor con hash usando la función `hashSync()` de `bcrypt`. Esto se usará en la función `signup` para almacenar un valor con hash en lugar de la contraseña con hash codificada en duro que estamos almacenando actualmente. Ten en cuenta que también movimos las funciones de inicio de sesión y registro de `userService` a `authService`, lo cual tiene más sentido.

Luego tenemos los métodos `login()` y `signup()`. Para el primero, dado un nombre de usuario y una contraseña en texto plano, primero verifica si el usuario existe en la base de datos. Luego compara la contraseña del usuario, almacenada como un hash en la base de datos, con la contraseña en texto plano proporcionada y verifica si hay una coincidencia gracias al método `verifyPassword()`. El método de inicio de sesión también adjunta un `accessToken` al objeto de datos devuelto, gracias al método `signAsync` de `jwtService`. El segundo recibe un objeto `createUserDto`, luego llama al método `create()` de la clase `UsersService` para persistir la información del nuevo usuario en la base de datos. Ten en cuenta que a partir de ahora, cuando creamos un nuevo usuario, ya no guardamos la contraseña como texto plano. En su lugar, la guardamos como una cadena con hash gracias al método `hashPassword()`.

Además, ten en cuenta que estamos inyectando una entidad `userService` en el constructor de la clase `AuthService`. Esto significa que se cumple lo siguiente:

- Necesitamos importar la clase `UserModule` en el arreglo `imports` de `AuthModule`.
- Luego necesitamos exportar el proveedor `UserService` desde la configuración del módulo `UserModule` o usarlo en el arreglo `providers` en `AuthModule`.

Ambos métodos funcionan. Siéntete libre de elegir según tus preferencias.

##### ¿Tienes algún problema hasta ahora?

Asegúrate de revisar la rama `feat/auth` del proyecto del repositorio y compararla con tu implementación.

#### Controladores de autenticación

Ahora que tenemos los métodos adecuados de inicio de sesión y registro en el archivo de servicio, implementemos sus manejadores de rutas. Podemos hacer eso pegando el siguiente contenido en el archivo `src/auth/auth.controller.ts`:

```typescript
@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Post('signup')
  signup(@Body() createUserDto: CreateUserDto) {
    return this.authService.signup(createUserDto);
  }

  @Post('login')
  login(@Body() loginUserDto: LoginUserDto) {
    const { username, password } = loginUserDto;
    return this.authService.login(username, password);
  }
}
```

Con la implementación anterior (no olvides agregar las importaciones necesarias al archivo), hemos creado dos manejadores de rutas de autenticación principales: el de registro (*signup*) y el de inicio de sesión (*login*). Ambos son peticiones POST. Esperan los objetos `createUserDto` y `loginUserDto`, respectivamente. Para el primero, usaremos el mismo objeto ubicado en el archivo `src/users/dto/create-user.dto.ts`. Para el segundo, creemos un nuevo objeto. Dado que el inicio de sesión está explícitamente relacionado con la función de autenticación, creemos un archivo `login-user.dto.ts` en la carpeta `src/auth/dto` con el siguiente contenido:

```typescript
import { PickType } from '@nestjs/mapped-types';
import { CreateUserDto } from 'src/users/dto/create-user.dto';

export class LoginUserDto extends PickType(CreateUserDto, [
  'username',
  'password',
] as const) {}
```

En el código anterior, usamos el tipo de utilidad `PickType` de `@nestjs/mapped-types` para crear una clase con solo el nombre de usuario y la contraseña esperados del objeto `CreateUserDto`. Ahora, siéntete libre de cambiar la firma de la función de inicio de sesión de `async login(username: string, password: string)` a `async login(loginUser: LoginUserDto)`.

Ahora que tenemos todos los DTOs que queríamos y los manejadores de rutas en su lugar, probemos todo el flujo de registro/inicio de sesión. Comenzaremos creando un nuevo usuario y luego iniciando sesión en el sistema usando su nombre de usuario y contraseña. Ten en cuenta que ambos endpoints de inicio de sesión y registro son `/auth/login` y `/auth/signup`. Ahora podemos eliminar el endpoint `user/create` de `UserController`, ya que no trata las contraseñas como esperábamos.

Siguiendo la guía anterior, deberías poder crear un nuevo usuario e iniciar sesión en el sistema. Debes notar que las contraseñas que provienen de la base de datos ya no son textos planos y que, al iniciar sesión, adjuntamos una cadena `accessToken` a la respuesta. Esto debería verse como en la Figura 12.12:

![Figura 12.12: Inicio de sesión de usuario con un accessToken](https://raw.githubusercontent.com/PacktPublishing/Scalable-Application-Development-with-NestJS/main/images/Figure%2012.12.png)

*Figura 12.12: Inicio de sesión de usuario con un accessToken*

La cadena `accessToken` nos ayudará a verificar si el usuario que realiza una solicitud está autenticado en rutas protegidas.

#### Guard de autenticación

Ahora que el usuario autenticado está obteniendo su `accessToken`, es hora de mejorar la seguridad de la aplicación protegiendo los endpoints sensibles. Por ejemplo, no todos deberían poder crear un producto. Los usuarios deberían estar al menos conectados con un `accessToken` válido, y probablemente tener el rol de `ADMIN`. Además, un usuario no autenticado no debería poder realizar un pedido o actualizar su estado.

Para lograr esto, debemos crear un Guard y vincularlo a los manejadores de rutas deseados. En el archivo `src/auth/auth.guard.ts`, pega el siguiente contenido:

```typescript
@Injectable()
export class AuthGuard implements CanActivate {
  constructor(
    private jwtService: JwtService,
    private readonly configService: ConfigService,
  ) {}

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const request = context.switchToHttp().getRequest();
    const token = this.extractTokenFromHeader(request);

    if (!token) {
      throw new UnauthorizedException();
    }

    try {
      const payload = await this.jwtService.verifyAsync(token, {
        secret: this.configService.get<string>('JWT_SECRET'),
      });
      request['user'] = payload;
    } catch {
      throw new UnauthorizedException();
    }
    return true;
  }

  private extractTokenFromHeader(request: Request): string | undefined {
    const [type, token] = request.headers.authorization?.split(' ') ?? [];
    return type === 'Bearer' ? token : undefined;
  }
}
```

Este Guard implementa la interfaz `CanActivate`. Su método `canActivate()` toma el contexto de ejecución que tiene acceso al objeto de petición y comprueba si se pasa algún token en el encabezado de la petición.

Si no es así, lanzamos directamente una excepción `UnauthorizedException`. De lo contrario, comprobamos si el token sigue siendo válido mediante el método `verifyAsync()` de JWT. Luego, si todo está según lo esperado, devolvemos `true` para que la petición pueda ser procesada por el manejador.

##### Tu turno

Ahora que tenemos el guard de autenticación, usa el decorador `@UseGuards()` para proteger los endpoints enumerados anteriormente. Ve más allá, implementando la comprobación de autorización para asegurar que solo los usuarios conectados con el rol `ADMIN` puedan crear un producto. El código se puede encontrar en el repositorio del capítulo.

Siguiendo la guía anterior, podemos asegurarnos de que las funciones básicas se implementen de forma segura. Siéntete libre de implementar la función de reseñas como un ejercicio práctico.

---

### Resumen

En este capítulo, nos embarcamos en la construcción de una aplicación de comercio electrónico con NestJS, reforzando nuestras habilidades en el desarrollo de APIs REST, pruebas, modelado de datos e integración de bases de datos reales. Abordamos las siguientes áreas clave:

- Profundizamos en las características y funcionalidades esenciales requeridas para una plataforma de comercio electrónico robusta, asegurando una comprensión integral de las necesidades de los usuarios y del negocio.
- Luego diseñamos una arquitectura escalable y definimos nuestros modelos de datos, sentando las bases para una aplicación bien estructurada.
- Desarrollamos endpoints RESTful para la gestión de productos, pedidos y pagos, garantizando un flujo continuo de datos y operaciones.
- Finalmente, implementamos mecanismos seguros de autenticación y autorización de usuarios utilizando JWT, mejorando la seguridad de la aplicación y las capacidades de gestión de usuarios.

A lo largo de este capítulo, integramos PostgreSQL para la gestión de bases de datos, yendo más allá de las bases de datos en memoria para abordar desafíos del mundo real. Al aplicar consideraciones teóricas de seguridad en un escenario práctico, consolidamos nuestra comprensión sobre la creación de aplicaciones seguras y eficientes.

Con una sólida comprensión del desarrollo de APIs REST y la arquitectura de aplicaciones del mundo real, ahora estás listo para asumir un nuevo desafío. En el próximo capítulo, exploraremos la construcción de un caso de uso del mundo real utilizando la API GraphQL. Esto ampliará aún más tus habilidades en el desarrollo de APIs, brindándote una comprensión más profunda de las tecnologías web modernas.

Prepárate para sumergirte en el mundo de GraphQL y explorar su potencial para crear APIs eficientes y flexibles. ¡Nos vemos en el próximo capítulo!
