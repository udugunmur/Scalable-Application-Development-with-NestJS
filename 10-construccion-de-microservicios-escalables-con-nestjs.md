# Parte 4: Escalando con Microservicios y NestJS

## Capítulo 10: Construcción de Microservicios Escalables con NestJS

¡Bienvenido al Capítulo 10, donde pasaremos de los aspectos teóricos aprendidos en el capítulo anterior a la práctica! Construiremos una aplicación de microservicios robusta utilizando un monorepo (*mono repo*) para aprovechar la forma en que NestJS maneja con elegancia las arquitecturas de microservicios.

NestJS ofrece una amplia gama de posibilidades a la hora de construir microservicios; las exploraremos con una serie de ejemplos y desafíos de la vida real a los que puedes enfrentarte al diseñar tu aplicación escalable.

En este capítulo, también aprenderemos a construir operaciones entre servicios utilizando varias capas de transporte de comunicación, todas aplicadas a casos de uso prácticos con los que los desarrolladores suelen lidiar al abordar la integridad de datos en arquitecturas de microservicios.

Esto es lo que cubriremos en este capítulo:

- Definición e implementación de microservicios en NestJS
- Visión general del paquete de microservicios en NestJS
- Comunicación entre servicios e integridad de datos en NestJS
- Mejores prácticas para construir microservicios con NestJS
- Resolución de problemas y depuración de microservicios

Para continuar con el capítulo, asegúrate de preparar tu entorno local mientras nos preparamos para una sesión de programación interactiva. Al final de este capítulo, no solo comprenderás cómo implementar microservicios utilizando NestJS, sino que también estarás preparado para superar los errores comunes.

¿Listo para poner la teoría en práctica? ¡Empecemos a programar!

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Definición e implementación de microservicios en NestJS

Basándonos en los fundamentos de NestJS que hemos establecido en capítulos anteriores, en esta sección nos centraremos en definir e implementar microservicios dentro de nuestro entorno NestJS existente. Al hacer la transición de una arquitectura monolítica a una arquitectura orientada a microservicios, aprovecharemos las sólidas capacidades de NestJS para gestionar sistemas complejos y escalables.

Exploraremos cómo NestJS respalda las arquitecturas de microservicios a través de su enfoque modular y características integradas que facilitan la independencia del servicio y la comunicación entre servicios. La transición de una arquitectura monolítica a una orientada a microservicios es vital para los desarrolladores que buscan crear sistemas que sean escalables, más fáciles de mantener y capaces de evolucionar con el tiempo.

A lo largo de esta sección, profundizaremos en los pasos prácticos necesarios para reestructurar tus aplicaciones NestJS en microservicios.

Mejoremos tus aplicaciones NestJS avanzando desde diseños monolíticos hacia sistemas de microservicios dinámicos y distribuidos.

#### Configuración de un nuevo proyecto

Comenzaremos creando un nuevo proyecto de NestJS utilizando el siguiente comando:

```bash
$ nest new microservices-sample
```

Puedes utilizar cualquier otro nombre de tu elección. Este comando generará un nuevo proyecto de NestJS, listo para ser transformado en uno de microservicios.

En NestJS, un microservicio es una aplicación que utiliza una capa de transporte diferente a HTTP.

El siguiente paso será instalar los paquetes necesarios mediante el siguiente comando:

```bash
$ yarn add @nestjs/microservices
```

Ahora, nuestro proyecto está listo para convertirse en un microservicio.

#### Configuración de un nuevo proyecto de microservicio

Para transformar la aplicación recién creada en una de microservicios, simplemente necesitamos actualizar el archivo `main.ts` con el siguiente código:

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { MicroserviceOptions, Transport } from '@nestjs/microservices';

async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(
    AppModule,
    {
      transport: Transport.TCP,
    },
  );
  await app.listen();
}
bootstrap();
```

El fragmento de código anterior se parece casi al generado por Nest-CLI. Sin embargo, se pueden notar claramente algunas diferencias:

- La aplicación ahora se crea con el método `createMicroservice()` del tipo de instancia `NestFactory` en lugar del método `create()`.
- La aplicación ya no es una instancia de `INestApplication`, sino una instancia de `INestMicroservice`.
- La función `listen` de la aplicación ya no recibe un `PORT` como parámetro.
- Estamos utilizando la capa de transporte TCP (que es diferente de HTTP, y en el contexto de NestJS, la aplicación es ahora un microservicio). El enum `Transport` tiene múltiples opciones. Dependiendo del caso de uso de la aplicación, podemos usar `REDIS`, `KAFKA`, `NATS`, `RabbitMQ`, etcétera. Con fines demostrativos, en este capítulo utilizaremos la opción por defecto, que es el Protocolo de Control de Transmisión (*Transmission Control Protocol* - TCP).

NestJS también admite aplicaciones híbridas. Las aplicaciones híbridas, en el contexto de NestJS, son aplicaciones que escuchan peticiones procedentes de dos o más fuentes; esto puede combinar un servidor HTTP con uno o más oyentes (*listeners*) de microservicios.

Como vimos en el código anterior, el método `createMicroservice()` devuelve una instancia que no nos permite configurar un servidor HTTP (no admite el parámetro de puerto). Por lo tanto, necesitamos otro método para las aplicaciones híbridas.

Podemos crear una aplicación híbrida en NestJS actualizando el archivo `main.ts` con el siguiente código:

```typescript
const app = await NestFactory.create(AppModule);
const microservice = app.connectMicroservice<MicroserviceOptions>({
  transport: Transport.TCP,
});
await app.startAllMicroservices();
await app.listen(3001);
```

El método `app.listen(3001)` inicia un servidor HTTP en el puerto especificado y permite que tu aplicación escuche tanto peticiones HTTP como TCP, convirtiéndola en una aplicación híbrida de NestJS.

¡Bien hecho! Con estos sencillos cambios, tenemos una aplicación de microservicios gracias a NestJS. Antes de avanzar con más implementaciones, hablemos sobre el paquete `@nestjs/microservices` en la siguiente sección.

---

### Visión general del paquete de microservicios en NestJS

Con tu proyecto NestJS ahora configurado para operar como un microservicio utilizando TCP, es esencial comprender las capacidades y características proporcionadas por el paquete `@nestjs/microservices`. Este paquete es la columna vertebral de la arquitectura de microservicios en NestJS, facilitando opciones robustas de comunicación y configuración que pueden escalar con la complejidad de tu aplicación.

#### Introducción a @nestjs/microservices

El módulo `@nestjs/microservices` está diseñado para integrarse a la perfección con el framework central de NestJS, permitiendo el desarrollo de aplicaciones de microservicios ligeras y altamente escalables. Admite una variedad de capas de transporte como TCP, Redis, NATS, MQTT, RabbitMQ y Kafka, lo que significa que tus microservicios de NestJS pueden comunicarse a través de diferentes canales, según los requisitos y restricciones específicos de tu aplicación.

#### Características principales y ventajas

El paquete `@nestjs/microservices` ofrece varias características clave que son fundamentales para desarrollar microservicios resilientes:

- **Independencia del transporte (*Transport independence*):** La capacidad de abstraer los detalles de la capa de transporte subyacente permite a los desarrolladores cambiar entre diferentes estrategias de comunicación con cambios mínimos en el código de la aplicación.
- **Manejo de mensajes y eventos (*Message and event handling*):** Proporciona decoradores y asistentes (*helpers*) para manejar mensajes y eventos entrantes de manera efectiva según el patrón utilizado, permitiendo una configuración clara y concisa para rutas y manejadores de mensajes.
- **Comunicación resiliente (*Resilient communication*):** Estrategias integradas para lidiar con caídas del servicio, como mecanismos de reintentos (*retries*) y retroceso (*back-off*), aseguran que tus servicios sigan siendo robustos bajo condiciones de fallo.
- **Integración con otros módulos (*Integration with other modules*):** `@nestjs/microservices` está diseñado para trabajar armoniosamente con otros módulos de NestJS, garantizando que aspectos como la seguridad y la configuración se gestionen de manera uniforme en todos tus servicios.

Habiendo descubierto los conceptos centrales y los beneficios del módulo `@nestjs/microservices`, profundicemos ahora en un ejemplo práctico para ver cómo se puede utilizar este módulo para implementar la comunicación entre microservicios.

#### Implementación de un ejemplo de comunicación simple

Implementemos un ejemplo básico para ilustrar cómo un microservicio NestJS puede enviar y recibir mensajes. Supongamos que tenemos un servicio que necesita procesar datos de usuario de forma asíncrona:

```typescript
import { Controller } from '@nestjs/common';
import { MessagePattern, Payload } from '@nestjs/microservices';

@Controller()
export class UserDataProcessor {
  @MessagePattern({ cmd: 'process_user_data' })
  async processUserData(@Payload() data: any) {
    // Logic to process user data
    console.log('Processing user data:', data);
  }
}
```

En este ejemplo, `@MessagePattern` decora el método `processUserData` para escuchar mensajes con el patrón `{ cmd: 'process_user_data' }`. Cada vez que se recibe un mensaje con este tema (*topic*), el método se activa con el payload del mensaje.

El paquete `@nestjs/microservices` extiende la arquitectura potente y escalable de NestJS al ámbito de los microservicios, proporcionando herramientas y técnicas esenciales para los sistemas backend modernos. Comprender y utilizar este paquete te permitirá construir microservicios eficientes, fáciles de mantener y robustos.

Al dominar estas herramientas, estarás bien equipado para manejar diversos desafíos que surgen en sistemas distribuidos, asegurando que tus aplicaciones puedan escalar y evolucionar junto con las necesidades de tu negocio.

En la siguiente sección, profundizaremos en la configuración de patrones de comunicación entre servicios y la gestión de la consistencia de datos entre ellos para mejorar aún más tu arquitectura de microservicios.

---

### Comunicación entre servicios e integridad de datos en NestJS

Ahora que entendemos cómo NestJS admite la arquitectura de microservicios, veamos cómo los microservicios se comunican entre sí. Antes de continuar, necesitamos transformar nuestra estructura actual en un monorepo (*mono repo*). Ten en cuenta que, según los requisitos de la aplicación, es posible que necesites tener múltiples repositorios. Mantendremos las cosas simples por ahora y utilizaremos una estructura de monorepo.

Para hacer esta transición —del modo estándar a una estructura de monorepo— podemos usar el poder de la CLI de Nest (`nest-cli`) y generar múltiples aplicaciones fácilmente. Necesitamos tener un proyecto de API gateway único, que expondrá el punto de entrada a nuestros clientes, y dos servicios más, por ejemplo, el servicio de pedidos (*order service*) y el servicio de inventario (*inventory service*). Siéntete libre de utilizar cualquier ejemplo que resuene mejor contigo.

Para generar esos servicios, utiliza el siguiente comando en la raíz del proyecto:

```bash
$ nest generate app order
# then
$ nest generate app inventory
```

Ejecutar estos comandos actualizará la estructura de tu proyecto a algo similar a la Figura 10.1:

![Figura 10.1: Estructura de aplicaciones en el monorepo](/images/Figure_10.1.png)
*Figura 10.1: Estructura de aplicaciones en el monorepo*

A partir de nuestra nueva estructura, es posible que hayas notado algunos cambios:

- Ahora tenemos una carpeta `apps/` en la raíz del proyecto.
- El proyecto existente ahora se encuentra bajo `/apps/original-application-name`, donde `original-application-name` es `microservices-sample` en mi caso.
- En el archivo `package.json`, podemos ver que se han actualizado algunos comandos. Por ejemplo, ahora el comando `start:prod` se ve así: `"start:prod": "node dist/apps/microservices-sample/main"`. Esto significa que el proyecto `microservices-sample` será tratado como nuestro punto de entrada, o en palabras comunes, nuestro API gateway.
- El archivo `nest-cli.json` se ha actualizado con lo siguiente:

```json
{
  "$schema": "https://json.schemastore.org/nest-cli",
  "collection": "@nestjs/schematics",
  "sourceRoot": "apps/microservices-sample/src",
  "compilerOptions": {
    "deleteOutDir": true,
    "webpack": true,
    "tsConfigPath": "apps/microservices-sample/tsconfig.app.json"
  },
  "monorepo": true,
  "root": "apps/microservices-sample",
  "projects": {
    "microservices-sample": {
      "type": "application",
      "root": "apps/microservices-sample",
      "entryFile": "main",
      "sourceRoot": "apps/microservices-sample/src",
      "compilerOptions": {
        "tsConfigPath": "apps/microservices-sample/tsconfig.app.json"
      }
    },
    "order": {
      "type": "application",
      "root": "apps/order",
      "entryFile": "main",
      "sourceRoot": "apps/order/src",
      "compilerOptions": {
        "tsConfigPath": "apps/order/tsconfig.app.json"
      }
    },
    "inventory": {
      "type": "application",
      "root": "apps/inventory",
      "entryFile": "main",
      "sourceRoot": "apps/inventory/src",
      "compilerOptions": {
        "tsConfigPath": "apps/inventory/tsconfig.app.json"
      }
    }
  }
}
```

En el archivo de configuración anterior, puedes ver que nuestro proyecto ahora reconoce tres proyectos diferentes, con la raíz principal del código fuente establecida en nuestra aplicación API gateway, y ambos microservicios siendo del tipo aplicación (`application`).

¡Bien hecho! Tienes un monorepo listo para ser cocinado.

Dependiendo de los requisitos de la aplicación, es posible que necesitemos exponer puertos adicionales para cada microservicio. Estos se considerarán como aplicaciones híbridas, ya que se comunicarán utilizando tanto HTTP como otra capa de comunicación (TCP, Kafka, etc.).

Nuestro proyecto está listo, así que hablemos un poco sobre lo que construiremos ahora.

#### Características clave de la aplicación

Tratando de mantener las cosas simples por ahora, aquí está la lista de las características más importantes que construiremos en este proyecto:

- Cuando alguien realiza un pedido, enviamos una petición POST al servicio de pedidos (*order service*) con el payload que contiene información sobre el pedido, como el usuario que quiere comprar, el producto y el precio total del pedido.
- Cuando el servicio de pedidos recibe la petición, inicia la creación de un pedido —aunque no tengamos ninguna base de datos implementada aquí, considéralo como una transacción de base de datos iniciada—, lo que emite un evento con el tema `order_created` que será consumido por el servicio de inventario (*inventory service*).
- Cuando el servicio de inventario recibe el evento `order_created`, comprueba si hay suficientes productos en el inventario. Si es así, actualiza el nivel de inventario y el estado del pedido a través de otro evento con el tema `order_processed`; si no, rechaza la creación del pedido mediante un evento del mismo tema `order_processed` con un payload diferente que será manejado por el servicio de pedidos para actualizar el estado del pedido en consecuencia.

Ten en cuenta que esta es una versión muy simplificada de un sistema de gestión de pedidos: el objetivo es demostrar cómo dos aplicaciones pueden comunicarse en NestJS.

Con el contexto establecido, construyamos nuestros microservicios.

#### El servicio de pedidos (*Order Service*)

Con nuestro monorepo configurado y nuestra comprensión del alcance del proyecto consolidada, trabajemos ahora en el servicio de pedidos.

Estos son los requisitos técnicos más importantes para nuestro servicio de pedidos:

- El servicio de pedidos escuchará peticiones HTTP entrantes para crear un nuevo pedido, es decir, una petición POST en el endpoint `/orders` (o `/create-order`).
- Luego, emitirá un evento cuando se haya creado el pedido para que sea manejado por el servicio de inventario.
- Luego, escuchará los eventos provenientes del servicio de inventario y actualizará el estado del pedido en consecuencia. Por ejemplo, si el nivel de inventario para el producto en el pedido no es suficiente para satisfacer la solicitud del pedido, el inventario enviará un evento `order_processed` al servicio de pedidos. Luego, el servicio de pedidos actualizará el estado del pedido de `PENDING` a `CANCELLED`.
- El hecho de que nuestro servicio de pedidos pueda escuchar tanto peticiones HTTP como TCP lo convierte en una aplicación híbrida, en el contexto de NestJS.

Esto suena minimalista por ahora, pero demostrará cómo dos aplicaciones de microservicios pueden comunicarse e intercambiar información. A continuación, construyamos nuestro servicio de pedidos de acuerdo con nuestros requisitos técnicos.

#### Construcción de la lógica del servicio de pedidos

Dentro de nuestra aplicación de pedidos, reemplacemos el código generado en el archivo `main.ts` con el siguiente contenido:

```typescript
// apps/order/src/main.ts
import { NestFactory } from '@nestjs/core';
import { OrderModule } from './order.module';

async function bootstrap() {
  const app = await NestFactory.create(OrderModule);
  // to connect to the microservices linked to the order
  // service
  app.startAllMicroservices();
  await app.listen(3001);
}
bootstrap();
```

En el código anterior, hemos actualizado el código inicial para que nuestro servicio de pedidos pueda iniciar todos los microservicios vinculados a él, esperando eventos entrantes o emitiendo nuevos eventos a esos servicios. En nuestro ejemplo, simplemente esperamos eventos provenientes del servicio de inventario mientras podemos enviarle eventos también. También estamos escuchando peticiones HTTP entrantes en el puerto 3001.

#### Modelado de los datos

A continuación, necesitamos modelar nuestros datos. Manteniéndonos minimalistas, creemos un archivo `apps/order/entities/order.entity.ts` y peguemos el siguiente contenido:

```typescript
export enum OrderStatus {
  PENDING = 'Pending',
  COMPLETED = 'Completed',
  CANCELLED = 'Cancelled',
}

export class Order {
  id: string;
  name: string;
  product: string;
  price: number;
  status: OrderStatus;
  quantity: number;
}
```

Por ahora, exportemos dos entidades del mismo archivo. Mejoraremos la arquitectura más adelante, siguiendo muy buenas prácticas.

Ten en cuenta que en la clase `Order` del código anterior, tenemos campos como `product`, pero en escenarios de la vida real, en lugar de una cadena, esta podría ser otra entidad. Alternativamente, puedes crear una entidad de usuario y establecer una relación entre el usuario y los pedidos. Por ahora, mantengamos las cosas sencillas. Tenemos casos de estudio en los próximos capítulos donde construiremos aplicaciones reales y consideraremos todas las restricciones de arquitectura, ¡no te preocupes! :).

#### Registro del servicio de inventario

Ahora, registremos el servicio de inventario en el `OrderModule` para que podamos establecer un contrato entre nuestros servicios con el siguiente fragmento de código:

```typescript
@Module({
  imports: [
    ClientsModule.register([
      {
        name: 'INVENTORY_SERVICE',
        transport: Transport.TCP,
        options: {
          port: 8002,
        },
      },
    ]),
  ],
  controllers: [OrderController],
  providers: [OrderService],
})
export class OrderModule {}
```

Con `ClientsModule` y `Transport` siendo importados de nuestro buen amigo, `@nestjs/microservices`, simplemente informamos al `OrderModule` que podemos estar enviando y recibiendo eventos TCP desde un módulo llamado `INVENTORY_SERVICE` usando el puerto 8002. No hay necesidad de asegurarse de que el puerto esté activo, es un contrato "TAL VEZ" (*MAYBE contract*). Si el servicio está fuera de servicio, ese no es problema del servicio de pedidos, pero sabemos que deberíamos poder comunicarnos con el servicio de inventario a partir de esta configuración.

A continuación, implementemos la lógica de negocio en el proceso de creación de pedidos.

#### Implementación de la lógica de negocio para la creación de pedidos

En el archivo `order/order.service.ts`, agrega el siguiente método `createOrder`. Asegúrate de crear e importar el Data Transfer Object (DTO) `CreateOrderInput` bajo `order/dto/create-order.dto.ts`:

```typescript
// order/order.service.ts
constructor(
  @Inject('INVENTORY_SERVICE') private inventoryClient: ClientProxy,
) {}

// in memory storage - for demo purposes
private orders: Order[] = [];

createOrder(createOrderInput: CreateOrderInput): Order {
  const order = {
    ...createOrderInput,
    id: `${this.orders.length + 1}`,
    status: OrderStatus.PENDING,
  };
  this.orders.push(order);
  console.log('Order created:', order, this.orders);

  // emit event to the inventory service
  this.inventoryClient.emit('order_created', order);

  return order;
}

// order/dto/create-order.dto.ts
export class CreateOrderInput {
  name: string;
  product: string;
  price: number;
}
```

En el fragmento de código anterior, implementamos el método `createOrder` en el archivo `order.service`, que hace lo siguiente:

- **Inyecta el cliente del servicio de inventario:** El constructor utiliza inyección de dependencias para incorporar el proxy del cliente de inventario, que se utiliza para comunicarse con el servicio de inventario, utilizando el token `INVENTORY_SERVICE`.
- **Mantiene una lista de pedidos en memoria:** Se utiliza un arreglo simple de pedidos para almacenar los datos de los pedidos en memoria con fines de demostración.
- **Genera un nuevo pedido:** El método `createOrder` toma una instancia de `CreateOrderInput`, genera un nuevo ID de pedido basado en el número actual de pedidos y asigna un estado inicial de `PENDING`.
- **Almacena el pedido:** El pedido recién creado se agrega al arreglo `orders`, simulando el almacenamiento.
- **Registra la creación del pedido:** Para visibilidad durante las pruebas y la depuración, imprime el pedido creado y la lista completa de pedidos en la consola.
- **Se comunica con el servicio de inventario:** El método `emit()` en el cliente de inventario se utiliza para enviar un evento `order_created` al servicio de inventario, pasando el pedido recién creado como payload. En NestJS, el método `emit()` devuelve un *hot Observable*, lo que significa que los oyentes (*listeners*) no tienen que suscribirse al flujo para recibir ese evento. Así es como los servicios se comunican y comparten datos entre sí de manera desacoplada.

El DTO `CreateOrderInput`, definido en `order/dto/create-order.dto.ts`, sirve como una plantilla estructurada para los datos de pedidos entrantes, asegurando que cada pedido tenga un nombre, producto y precio.

Esta configuración demuestra los aspectos fundamentales de la comunicación entre servicios, lo que te permite desacoplar los servicios mientras garantizas que permanezcan coordinados en el procesamiento de la lógica de negocio, especialmente cuando escalan de forma independiente.

#### Adición de un método en el controlador

Para completar el proceso y permitir que el servicio de pedidos reciba y procese esta solicitud, agreguemos un método en el controlador con el decorador `@Post()` y especifiquemos una ruta para esta operación. En el archivo `order/order.controller.ts`, agrega el siguiente método:

```typescript
// order/order.controller.ts
@Post('create-order')
createOrder(@Body() createOrderInput: CreateOrderInput): Order {
  return this.orderService.createOrder(createOrderInput);
}
```

Este método simplemente escucha peticiones POST entrantes en el endpoint `create-order` y llama al servicio dedicado del que hablamos anteriormente.

¿Es esto suficiente? Bueno, si solo estamos hablando de la operación de creación de pedidos del servicio de pedidos, entonces sí.

#### Ejecución del servicio de pedidos

Cuando se realiza el pedido, el servicio de pedidos toma la solicitud entrante e informa al servicio de inventario que se ha realizado un pedido. El evento puede ser recibido o no, ese ya no es asunto del servicio de pedidos; esto significa que podemos escalar este servicio sin preocuparnos por los demás que están vinculados a él: esa es la magia de los microservicios.

Probemos las cosas ejecutando el servicio de pedidos con el siguiente comando:

```bash
$ yarn start:dev order
```

Esto solo iniciará el servicio de pedidos y comenzará a escuchar peticiones HTTP en el puerto 3001, de acuerdo con nuestra configuración en el archivo `main.ts`.

Envía una solicitud de ejemplo que inicie la creación de un pedido usando `curl` (o cualquier otra plataforma con interfaz gráfica como Postman) con el siguiente comando:

```bash
$ curl -H 'Content-Type: application/json' \
  -d '{
    "name": "Joseph Diakese",
    "product": "Laptop",
    "price": 2000,
    "quantity": 10
  }' \
  -X POST \
  http://localhost:3001/create-order
```

Este comando realizará una petición POST en la URL especificada. La opción `-H` contiene las cabeceras de la petición, la opción `-d` el cuerpo (*body*), y `-X` el verbo. Iniciará una creación de pedido sin ningún problema, sin cuestionar al receptor de ese evento.

Mirando la terminal, la Figura 10.2 muestra cómo debería verse, imprimiendo el pedido recién creado y la lista actualizada en la base de datos en memoria que creamos, tal como pretendía el archivo de servicio:

![Figura 10.2: Servicio de pedidos – salida al iniciar la creación de un pedido](/images/Figure_10.2.png)
*Figura 10.2: Servicio de pedidos – salida al iniciar la creación de un pedido*

¡Bien hecho! Acabamos de enviar una operación de creación de pedidos. Sin embargo, nuestro trabajo apenas comienza. ¿Qué pasa si mientras procesamos el pedido no tenemos suficientes productos en stock? ¿Cómo confirmamos al usuario que su pedido se ha completado cuando tenemos suficientes productos en stock, o cancelado debido a muchos problemas probables? Por ejemplo, tal vez el servicio de inventario no esté disponible en este momento, tal vez nos hayamos quedado sin existencias para ese producto específico, etcétera. Aquí es donde entra en juego el servicio de inventario. Trabajemos en él.

#### Construcción de la lógica del servicio de inventario

La responsabilidad del servicio de inventario es simple por ahora: escuchar los eventos entrantes `order_created` que provienen del servicio de pedidos. Luego, dependiendo del nivel de inventario del producto especificado, envía de vuelta otro evento con el patrón `order_processed` con un payload que indica si el nivel de inventario fue suficiente.

Esto significa que no necesitamos una aplicación híbrida aquí, al menos por ahora. Comencemos actualizando el archivo `inventory/main.ts` con el siguiente código:

```typescript
// inventory/main.ts
import { NestFactory } from '@nestjs/core';
import { InventoryModule } from './inventory.module';
import { MicroserviceOptions, Transport } from '@nestjs/microservices';

async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(
    InventoryModule,
    {
      transport: Transport.TCP,
      options: {
        port: 8002,
        host: 'localhost',
      },
    },
  );
  await app.listen();
}
bootstrap();
```

Con el fragmento de código anterior, configuramos un microservicio utilizando un transporte TCP y comunicándose en el puerto 8002, siendo el host `localhost`, ya que no planeamos pasar a producción por ahora. Ten en cuenta que el segundo parámetro del método `createMicroservice()` se ve casi igual a las opciones con las que registramos el servicio de inventario en el `OrderModule`; si el puerto y el transporte no coinciden aquí, el evento nunca se recibirá ni se enviará de un servicio a otro porque los contratos no son iguales.

A continuación, necesitamos probar si el evento enviado desde el servicio de pedidos se está recibiendo aquí.

#### Comprobación de la recepción del evento

Para eso, creemos un manejador de eventos con el tema `order_created` en el archivo `inventory/inventory.controller.ts`, agregando el siguiente método `handleOrderCreated`:

```typescript
// inventory/inventory.controller.ts
@EventPattern('order_created')
async handleOrderCreated(data: any) {
  console.log('Order created:', data);
}
```

El método anterior, decorado con `@EventPattern`, se llamará cada vez que se emita un evento `order_created` desde uno de los productores. Realmente no nos importa la fuente. Simplemente necesitamos reaccionar tras la recepción del evento, y dado que la función `emit()` llamada en el servicio de pedidos devuelve un *hot Observable*, no hay necesidad de suscribirse a este tema, como vimos anteriormente.

Entonces, manteniendo el servicio de pedidos en ejecución, en otra terminal iniciemos el servicio de inventario con el siguiente comando:

```bash
$ yarn start:dev inventory
```

Ahora, iniciemos la misma petición HTTP desde el servicio de pedidos usando `curl` y veamos qué sucede en ambas terminales.

En la terminal del servicio de pedidos, las cosas siguen prácticamente igual, pero en el servicio de inventario, observa que tenemos impreso el payload del pedido, como si el servicio de inventario estuviera confirmando que ha recibido nuestra solicitud.

Esto es muy bueno. Ahora necesitamos implementar la lógica de negocio que manejará este evento en el servicio de inventario. Sin embargo, primero limpiemos un poco el proyecto porque esto se está volviendo un poco desordenado.

#### Tomemos un café

Antes de continuar, tómate un momento para repasar todo el proceso una vez más y asegurarte de que esté claro. Los siguientes pasos serán los mismos, con el productor convirtiéndose en consumidor esta vez.

#### Limpieza del proyecto

Por ahora, hemos codificado de forma fija (*hard-coded*) muchos valores. Imagina escribir `order_creted` por error en lugar de `order_created` en uno de esos servicios, o tener un puerto 3003 registrado en el `OrderModule` y el 3002 configurado en el servicio de inventario. Sería imposible que nuestros servicios se comunicaran, y a medida que la aplicación crece, se agregan más operaciones y constantes, y las cosas pueden salirse de control rápidamente.

Algunos valores, como los puertos o los nombres de los servicios, se pueden agregar como variables de entorno y tener diferentes valores según el entorno. Sin embargo, para las instancias de la aplicación o las piezas de código más utilizadas, se necesita otra solución. Recuerda el principio *Don’t Repeat Yourself* (DRY).

Aquí es donde la creación de bibliotecas (*libraries*) cobra importancia. En NestJS, cuando tienes un monorepo, puedes generar una biblioteca usando `$ nest generate lib library-name`. Esto actualizará los archivos `nest-cli.json` y `package.json` y luego creará un nuevo proyecto bajo la carpeta `libs/library-name`.

Las bibliotecas también son importantes cuando alguna lógica de negocio, constantes o funciones se pueden usar en múltiples lugares de la aplicación.

En escenarios más sofisticados, cuando tienes una arquitectura multirepo (*poly repo*), estas bibliotecas se pueden publicar como paquetes npm e importar a cualquier repositorio que las necesite.

Entonces, generemos nuestro primer módulo y llamémoslo `constants` usando el siguiente comando:

```bash
$ nest generate lib constants
# accept the default configuration
```

Este comando hará lo siguiente:

- Crear un nuevo proyecto bajo `libs/constants`.
- Actualizar `nest-cli.json`, agregando las siguientes propiedades:

```json
"constants": {
  "type": "library",
  "root": "libs/constants",
  "entryFile": "index",
  "sourceRoot": "libs/constants/src",
  "compilerOptions": {
    "tsConfigPath": "libs/constants/tsconfig.lib.json"
  }
}
```

Ten en cuenta que la propiedad `type` se establece en `library`, mientras que las otras se establecen en `application`.

- Actualizar el archivo `package.json`, agregando la siguiente propiedad:

```json
"moduleNameMapper": {
  "^@app/constants(|/.*)$": "<rootDir>/libs/constants/src/$1"
}
```

Ahora, elimina todos los archivos generados bajo `libs/constants/src` excepto el archivo `index.ts`, que es necesario para exportar todas las instancias de la biblioteca.

Ahora, en la carpeta `constants`, agreguemos un nuevo archivo (`constants.events.ts` o `events.ts`) y peguemos el siguiente código:

```typescript
// libs/constants/src/constants.events.ts
export const EVENTS = {
  ORDER_CREATED: 'order_created',
  ORDER_UPDATED: 'order_updated',
};

// update the constants/src/index.ts file
export * from './events';
```

En el código anterior, simplemente hemos exportado el objeto `EVENTS` con las propiedades necesarias y luego lo hemos exportado desde el archivo `index`.

Ahora, reemplaza el patrón de evento codificado de forma fija tanto en el servicio de pedidos como en el de inventario, importando el patrón desde `@app/constants` en su lugar.

De vuelta al proyecto con una nota: cada vez que comencemos a codificar cadenas fijas o a copiar y pegar un bloque de código, puede ser el momento de migrarlo a una biblioteca.

Ahora, manejemos la creación de pedidos en el servicio de inventario.

#### Manejo de la creación de pedidos en el servicio de inventario

Primero, debemos implementar la lógica de negocio actualizando el archivo `inventory.service.ts` con el siguiente código:

```typescript
import { EVENTS } from '@app/constants';
import { Inventory, Order, OrderProcessPayload } from '@app/shared';
import { Inject, Injectable } from '@nestjs/common';
import { ClientProxy } from '@nestjs/microservices';

@Injectable()
export class InventoryService {
  constructor(@Inject('ORDER_SERVICE') private orderClient: ClientProxy) {}

  // in memory storage of inventory - for demo purposes
  private inventory: Inventory[] = [
    { id: 1, name: 'Laptop', quantity: 100 },
    { id: 2, name: 'Mouse', quantity: 50 },
    { id: 3, name: 'Keyboard', quantity: 75 },
  ];

  handleOrderCreated(order: Order) {
    let success = false;
    let message = '';
    const item = this.inventory.find((i) => i.name === order.product);

    if (item) {
      if (item.quantity < order.quantity) {
        message = 'Insufficient quantity in inventory';
      } else {
        item.quantity -= order.quantity;
        success = true;
        message = 'Order processed successfully';
      }
    } else {
      message = `Product ${order.product} not found in inventory`;
    }

    const payload: OrderProcessPayload = {
      success,
      message,
      orderId: order.id,
    };
    console.log('Order processed with the payload:', payload);

    // emit event to the order service
    return this.orderClient.emit(EVENTS.ORDER_PROCESSED, payload);
  }

  // keep whatever we had before
}
```

Como puedes ver en el fragmento de código anterior, esto se parece a lo que hicimos en el archivo del servicio de pedidos. Tenemos un `orderClient` que emite eventos para ser manejados por el servicio de pedidos. También tenemos una base de datos en memoria para almacenar el inventario.

El método `handleOrderCreated()` espera una instancia de `Order` como parámetro que se pasará desde el manejador de eventos en el controlador, en lugar de importar la entidad `Order` desde el servicio de pedidos, que es una aplicación independiente. Es preferible crear una nueva biblioteca llamada `shared` y exportar entidades desde `libs/shared/entities/*.entities.ts`.

El método también devuelve un `Observable` para ser consumido por el servicio de pedidos. Espera un segundo: ¿puede el servicio de pedidos escuchar peticiones TCP? En este momento, no puede. También necesitamos configurar una conexión TCP, pero no de la misma manera que lo hicimos para el servicio de inventario. Recuerda que se supone que nuestro servicio de pedidos debe ser híbrido.

Para configurar una conexión TCP, actualicemos el archivo `order/main.ts`, agregando la siguiente llamada a método en la instancia de `INestApplication`:

```typescript
// apps/order/main.ts
// configure the microservice to listen on port 3001 using
// the TCP transport layer
app.connectMicroservice<MicroserviceOptions>({
  transport: Transport.TCP,
  options: {
    port: 8001,
    host: 'localhost',
  },
});
```

De acuerdo con la documentación oficial de Nest, el método `connectMicroservice()` hace lo siguiente:

> "Conecta el microservicio a la instancia de NestApplication. Transforma la aplicación en una instancia híbrida."

Esto significa que agregar este método en el archivo `main.ts` ahora convierte al servicio de pedidos en una aplicación híbrida real.

#### Registro del servicio de pedidos en el servicio de inventario

A continuación, necesitamos registrar el servicio de pedidos en el servicio de inventario de la misma manera que lo hicimos en el servicio de pedidos. Para hacerlo, agreguemos el siguiente código al arreglo `imports` de `InventoryModule`:

```typescript
// inventory/inventory.module.ts
ClientsModule.register([
  {
    name: 'ORDER_SERVICE',
    transport: Transport.TCP,
    options: {
      port: 8001,
    },
  },
]),
```

Ten en cuenta que el puerto es idéntico al que configuramos en el servicio de pedidos.

Reemplacemos el código en el archivo del controlador de inventario con el siguiente código para que consuma la lógica de negocio en su archivo de servicio en lugar de un `console.log`:

```typescript
@EventPattern(EVENTS.ORDER_CREATED)
async handleOrderCreated(data: Order) {
  // change this line
  this.inventoryService.handleOrderCreated(data);
}
```

¡Bien hecho! Registramos el servicio de pedidos en el servicio de inventario e invocamos la lógica de negocio desde el archivo del controlador, que envía un evento al cliente de pedidos con el payload correcto de acuerdo con el nivel de inventario de un producto específico.

Desde el momento en que el servicio de inventario envía un evento al servicio de pedidos, completa su parte del contrato. El manejo de esos eventos es responsabilidad del servicio de pedidos. Gestionémoslo simplemente agregando un manejador de eventos en el archivo `order.controller.ts` de la siguiente manera:

```typescript
// order.controller.ts
@EventPattern(EVENTS.ORDER_PROCESSED)
async handleOrderProcessed(data: OrderProcessPayload) {
  this.orderService.handleOrderProcessed(data);
}

// order.service.ts
handleOrderProcessed(data: OrderProcessPayload) {
  const order = this.orders.find((o) => o.id === data.orderId);
  if (order) {
    order.status = data.success ? OrderStatus.COMPLETED : OrderStatus.CANCELLED;
    console.log('Order status updated:', order, this.orders);
  } else {
    console.log('Order not found');
  }
}
```

En el fragmento de código anterior, implementamos un oyente (*listener*) para que cada vez que llegue un evento con el patrón `EVENTS.ORDER_PROCESSED` desde cualquier lugar al puerto especificado en el archivo `main.ts`, active una llamada a función en el archivo de servicio, que luego actualiza el estado del pedido si se encuentra.

¡Bien hecho! Lo has logrado. Ahora tenemos operaciones navegando a través de los servicios como una reina en un tablero de ajedrez (sin límites).

#### Ejecución de los servicios de pedidos e inventario

Probemos todo una vez más, iniciando ambos servicios desde terminales diferentes mediante lo siguiente:

```bash
$ nest start:dev order
$ nest start:dev inventory
```

Las Figuras 10.3 y 10.4 ilustran lo que deberías ver en ambas terminales cuando realices la misma solicitud `curl` si todo sale según lo planeado.

![Figura 10.3: Salida del servicio de pedidos](/images/Figure_10.3.png)
*Figura 10.3: Salida del servicio de pedidos*

![Figura 10.4: Salida del servicio de inventario](/images/Figure_10.4.png)
*Figura 10.4: Salida del servicio de inventario*

¡Bien hecho! Esto ha sido genial hasta ahora.

Antes de cerrar esta sección, debe haber una pregunta en tu mente: ¿cuál es entonces el papel del API gateway si nunca se involucra? Te entiendo, tienes razón, lo hemos ignorado hasta ahora. Demostremos cómo puede desempeñar un papel crucial en nuestra arquitectura en la siguiente sección.

---

### API gateway, nuestro guardián

Un API gateway es una parte esencial de la arquitectura de microservicios, ya que actúa como el único punto de entrada para que los clientes externos interactúen con el sistema. En esta arquitectura, el API gateway ofrece una interfaz unificada para varios servicios y maneja tareas como el enrutamiento de solicitudes, la composición y la traducción de protocolos.

#### Funciones clave de un API gateway

En una arquitectura de microservicios, estas son las funciones clave de un API gateway:

- **Enrutamiento centralizado (*Centralized routing*):** El API gateway dirige las solicitudes entrantes de los clientes al microservicio adecuado según la URL, el método HTTP u otra lógica de enrutamiento.
- **Autenticación y autorización (*Authentication and authorization*):** Garantiza que solo las solicitudes autenticadas y autorizadas lleguen a los servicios, gestionando la seguridad de forma centralizada.
- **Balanceo de carga (*Load balancing*):** Distribuye las solicitudes de manera uniforme entre múltiples instancias de un servicio para mejorar la confiabilidad y el rendimiento.
- **Transformación de solicitudes (*Request transformation*):** Modifica los formatos de solicitud y respuesta para alinearse con los requisitos específicos del servicio, abstrayendo la complejidad de los clientes.
- **Limitación de tasa y almacenamiento en caché (*Rate limiting and caching*):** Protege contra cargas excesivas limitando las tasas de solicitud y mejora el rendimiento almacenando en caché las respuestas.
- **Descubrimiento de servicios (*Service discovery*):** Descubre automáticamente instancias de servicio disponibles y enruta el tráfico en consecuencia, mejorando la flexibilidad y la escalabilidad.

Con una comprensión de las funciones críticas que realiza un API gateway dentro de una arquitectura de microservicios, ahora podemos explorar cómo implementar un API gateway en NestJS, llevando estos conceptos a la práctica.

#### Implementación de un API Gateway en NestJS

Implementemos nuestro API gateway siguiendo estos sencillos pasos:

1. **Configuración inicial:** Comienza configurando la aplicación API gateway como un punto de entrada para las solicitudes de los clientes. Actualiza el archivo `main.ts` de la aplicación API gateway para configurar la funcionalidad HTTP básica:

```typescript
// apps/microservice-sample/src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

2. **Reenvío (*proxying*) de solicitudes a microservicios:** Configura el gateway para redirigir las solicitudes de los clientes al microservicio correspondiente mediante HTTP u otra capa de transporte. En este caso, el servicio de pedidos escucha en el puerto 3001 y el servicio de inventario escucha en el puerto 8002.

A continuación, debes configurar el proxy HTTP en el gateway. Hay varias formas de manejar esto, como usar un módulo de proxy inverso o reenviar manualmente las solicitudes a los servicios apropiados.

#### Uso de @nestjs/terminus

El middleware de proxy inverso como `@nestjs/terminus` puede manejar el reenvío:

1. **Instalación de paquetes:** Instala el paquete requerido con el siguiente comando:

```bash
$ yarn add @nestjs/terminus
```

2. **Configuración de rutas en `app.module.ts`:** Actualiza el `AppModule` del gateway para incluir rutas que reenvíen solicitudes a los servicios de pedidos e inventario:

```typescript
// apps/api-gateway/src/app.module.ts
import { Module } from '@nestjs/common';
import { TerminusModule } from '@nestjs/terminus';
import { HttpModule } from '@nestjs/axios';

@Module({
  imports: [TerminusModule, HttpModule],
  controllers: [],
  providers: [],
})
export class AppModule {}
```

3. **Implementación de un controlador proxy:** Crea un controlador para gestionar el reenvío de solicitudes. Aquí tienes un ejemplo básico:

```typescript
// apps/api-gateway/src/proxy.controller.ts
import { Controller, Get, Post, Req, Res, HttpService } from '@nestjs/common';
import { Request, Response } from 'express';

@Controller('orders')
export class ProxyController {
  constructor(private httpService: HttpService) {}

  @Post()
  async forwardToOrderService(@Req() req: Request, @Res() res: Response) {
    const { data } = await this.httpService
      .post('http://localhost:3001/orders', req.body)
      .toPromise();
    res.json(data);
  }
}
```

4. **Adición del controlador proxy al módulo:** Registra el controlador en el módulo:

```typescript
// apps/microservice-sample/src/app.module.ts
import { Module } from '@nestjs/common';
import { ProxyController } from './proxy.controller';

@Module({
  imports: [TerminusModule, HttpModule],
  controllers: [ProxyController],
  providers: [],
})
export class AppModule {}
```

5. **Manejo de autenticación y autorización:** Integra middleware o módulos para gestionar la autenticación de usuarios de forma centralizada en el gateway. El gateway verifica las credenciales del cliente y garantiza el acceso seguro a los servicios subyacentes.

6. **Configuración avanzada:** Implementa limitación de tasa (*rate limiting*), almacenamiento en caché de respuestas y otras características avanzadas utilizando módulos de NestJS disponibles o bibliotecas de terceros.

7. **Descubrimiento de servicios y balanceo de carga:** Considera implementar enrutamiento dinámico y balanceo de carga integrando el descubrimiento de servicios con una herramienta como Consul o Eureka.

En una arquitectura de microservicios, el API gateway sirve como el punto de entrada coherente para las interacciones con los clientes, actuando como el guardián de la red interna de servicios. Al centralizar el enrutamiento, la seguridad y el procesamiento de solicitudes, simplifica las interacciones del cliente al tiempo que permite la escalabilidad y la independencia del servicio. Con NestJS, configurar un API gateway robusto garantiza que tu arquitectura de microservicios siga siendo modular, escalable y segura.

#### Ejercicio práctico

Al cerrar esta extensa sección, tómate un momento para transformar el servicio de inventario en una aplicación híbrida de modo que también pueda escuchar llamadas HTTP, como un POST en el endpoint `add-product`, para agregar productos al inventario.

En la siguiente sección, analicemos el descubrimiento de servicios y su importancia en la arquitectura de microservicios.

---

### Descubrimiento de servicios con Consul en NestJS

El descubrimiento de servicios (*service discovery*) es un aspecto crucial de la arquitectura de microservicios. Permite a los servicios registrarse dinámicamente y descubrir otros servicios en un entorno distribuido. En esta sección, integraremos Consul ([https://www.consul.io/](https://www.consul.io/)), una herramienta de configuración y descubrimiento de servicios ampliamente utilizada, con nuestra aplicación NestJS.

#### ¿Qué es Consul?

Consul es una solución de malla de servicios (*service mesh*) que proporciona varias funciones, incluidas las siguientes:

- **Descubrimiento de servicios (*Service discovery*):** Los servicios pueden registrarse en Consul, que actúa como un directorio para todos los servicios en ejecución.
- **Comprobación de estado (*Health checking*):** Consul puede monitorizar activamente el estado de los servicios y enrutar el tráfico únicamente a instancias saludables.
- **Almacén clave-valor (*Key-value store*):** Consul proporciona un almacén clave-valor distribuido para configuración y coordinación.

Con Consul, los microservicios ya no necesitan codificar de forma rígida las direcciones IP o los puertos de otros servicios. En su lugar, consultan a Consul para descubrir servicios dinámicamente, lo que garantiza una mayor flexibilidad y escalabilidad.

Implementemos el descubrimiento de servicios siguiendo estos sencillos pasos.

#### Paso 1 – Instalación del cliente de Consul para Node.js

Para interactuar con Consul en una aplicación NestJS, necesitamos instalar el cliente oficial de Consul para Node.js:

```bash
$ yarn add consul
```

#### Paso 2 – Creación del servicio Consul en NestJS

Para integrar Consul en nuestros microservicios de NestJS, crearemos un `ConsulService` responsable de registrar el servicio al iniciarse y desregistrarlo al apagarse. Este servicio será reutilizable en todos los microservicios y proporcionará los métodos de descubrimiento necesarios:

```typescript
// consul.service.ts
import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
import * as Consul from 'consul';

@Injectable()
export class ConsulService implements OnModuleInit, OnModuleDestroy {
  private consul: Consul.Consul;
  private serviceId: string;

  constructor() {
    // Initialize Consul client
    this.consul = new Consul({
      host: 'localhost', // Change to your Consul server
      // address
      port: 8500, // Default port for Consul
    });
    this.serviceId = 'inventory-service'; // Unique ID for
    // the service
  }

  // Register the service in Consul when the module
  // initializes
  async onModuleInit() {
    const serviceName = 'inventory-service';
    const serviceHost = 'localhost'; // Change to your
    // service's hostname
    const servicePort = 8002; // Change to your service's
    // port
    try {
      await this.consul.agent.service.register({
        id: this.serviceId,
        name: serviceName,
        address: serviceHost,
        port: servicePort,
        check: {
          http: `http://${serviceHost}:${servicePort}/health`,
          interval: '10s', // Health check every 10 seconds
          timeout: '5s', // Timeout for the health check
        },
      });
      console.log(`${serviceName} registered with Consul`);
    } catch (error) {
      console.error(
        'Error registering service with Consul:',
        error,
      );
    }
  }

  // Deregister the service in Consul when the module is
  // destroyed
  async onModuleDestroy() {
    try {
      await this.consul.agent.service
        .deregister(this.serviceId);
      console.log(
        `${this.serviceId} deregistered from Consul`,
      );
    } catch (error) {
      console.error(
        'Error deregistering service from Consul:',
        error,
      );
    }
  }

  // Discover other services using Consul
  async discoverService(serviceName: string) {
    try {
      const services = await this.consul.catalog
        .service.nodes(serviceName);
      return services;
    } catch (error) {
      console.error(
        'Error discovering service with Consul:',
        error,
      );
      return null;
    }
  }
}
```

Aquí hay algunos puntos a tener en cuenta sobre el código anterior:

- `onModuleInit`: Cuando se inicia el módulo `InventoryModule`, se registra a sí mismo en Consul. Proporciona el nombre del servicio, la dirección y el puerto, junto con un endpoint de comprobación de estado (*health check*).
- `onModuleDestroy`: Cuando el módulo se destruye (por ejemplo, al apagarse), desregistra el servicio de Consul.
- `discoverService`: Este método nos permite consultar a Consul por otros servicios por su nombre. Esto será particularmente útil para realizar llamadas de servicio a servicio sin codificar direcciones fijas.

#### Paso 3 – Adición de comprobaciones de estado para el servicio

Consul utiliza comprobaciones de estado (*health checks*) para garantizar la disponibilidad de los servicios. Podemos crear una ruta simple de comprobación de estado en nuestro `InventoryService`:

```typescript
// inventory.controller.ts
import { Controller, Get } from '@nestjs/common';

@Controller('health')
export class HealthController {
  @Get()
  checkHealth() {
    return { status: 'UP' }; // Indicate the service is
    // healthy
  }
}
```

Este endpoint responderá con un estado HTTP 200 y un mensaje JSON simple para indicar que el servicio está en buen estado (*healthy*).

#### Paso 4 – Registro de ConsulService en el módulo

Ahora, necesitamos registrar el `ConsulService` en el `InventoryModule` para que se inicie y se detenga correctamente:

```typescript
// inventory.module.ts
import { Module } from '@nestjs/common';
import { ConsulService } from './consul.service'; // Import the Consul service

@Module({
  providers: [ConsulService],
  exports: [ConsulService], // Export if other modules need
  // it
})
export class InventoryModule {}
```

#### Paso 5 – Descubrimiento de otros servicios

En la arquitectura de microservicios, es común que un servicio se comunique con otros. En lugar de codificar de forma fija la dirección de `OrderService`, podemos consultar a Consul para obtener la instancia correcta.

A continuación se muestra un ejemplo de cómo podemos descubrir el `OrderService` desde el servicio de inventario:

```typescript
@Injectable()
export class InventoryService {
  // Inject the Consul service
  constructor(
    private readonly consulService: ConsulService,
  ) {}

  async processOrder() {
    // Discover the Order service
    const services = await this.consulService
      .discoverService('order-service');
    if (services && services.length > 0) {
      const orderService = services[0]; // Choose one
      // instance of the
      // order service
      const orderUrl = `http://${orderService.ServiceAddress}:${orderService.ServicePort}`;
      console.log('Order service URL:', orderUrl);
      // Use the discovered URL to make a request to the
      // order service
      // e.g., using HttpService to place an order
    } else {
      console.error('Order service not found');
    }
  }
}
```

Aquí, en lugar de codificar de forma fija la dirección de `OrderService`, el `InventoryService` consulta a Consul en busca de instancias disponibles de `OrderService` y recupera su dirección de forma dinámica.

Al integrar Consul en nuestros microservicios, habilitamos el descubrimiento dinámico de servicios, mejorando la flexibilidad y la escalabilidad de nuestra arquitectura. Ahora, cada servicio puede registrarse de forma independiente y descubrir a otros, haciendo que el sistema sea más robusto y adaptable a los cambios en las instancias de servicio o en las condiciones de la red.

---

### Mejores prácticas para construir microservicios con NestJS

La construcción de microservicios es compleja, pero seguir las mejores prácticas puede mejorar significativamente la escalabilidad, confiabilidad y mantenibilidad del sistema. A continuación se presentan algunas mejores prácticas específicas para los microservicios de NestJS.

#### Estructura y modularización

Considera las siguientes mejores prácticas al estructurar tus microservicios:

- **Diseño modular (*Modular design*):** Organiza tus servicios en módulos para facilitar el mantenimiento y las pruebas. Cada módulo debe tener una única responsabilidad, facilitando la identificación y gestión de dependencias.
- **Separación de responsabilidades (*Separation of concerns*):** Asegúrate de que cada microservicio se enfoque en una capacidad de negocio específica para mantener claros los límites entre los servicios.

#### Gestión de la configuración

En términos de configuración, las siguientes mejores prácticas pueden ser útiles:

- **Variables de entorno (*Environment variables*):** Utiliza el paquete `@nestjs/config` para gestionar configuraciones específicas del entorno. Evita codificar valores fijos para garantizar un despliegue sencillo en distintos entornos.
- **Configuración centralizada (*Centralized configuration*):** Para configuraciones compartidas entre múltiples servicios, considera el uso de herramientas centralizadas de gestión de configuración.

#### Comunicación entre servicios

Cuando sea el momento de comunicarse entre microservicios, considera las siguientes mejores prácticas:

- **Mensajería asíncrona (*Asynchronous messaging*):** Prefiere la comunicación basada en eventos (utilizando Kafka, RabbitMQ u otros sistemas de mensajería) para servicios débilmente acoplados. Esto mejora la tolerancia a fallos y la escalabilidad.
- **Comunicación HTTP (*HTTP communication*):** Para la comunicación síncrona, aprovecha las características integradas del cliente HTTP de NestJS y el soporte de proxy para simplificar la comunicación.

#### Resiliencia y manejo de errores

Para asegurarte de que la arquitectura sea resiliente y maneje los errores con elegancia, considera las siguientes mejores prácticas:

- **Patrón Circuit Breaker (*Circuit breaker pattern*):** Implementa disyuntores (*circuit breakers*) para evitar fallos en cascada entre los servicios.
- **Reintentos y tiempos de espera (*Retries and timeouts*):** Utiliza reintentos y tiempos de espera para operaciones idempotentes a fin de gestionar eficazmente los errores transitorios.

#### Gestión de datos

Al tratar con un gran conjunto de datos (que probablemente sea la razón por la que migraste a una arquitectura de microservicios), considera las siguientes prácticas:

- **Patrón Saga (*Saga pattern*):** Para transacciones distribuidas, implementa el patrón Saga para mantener la consistencia de los datos entre servicios.
- **Event Sourcing:** Utiliza *event sourcing* para mantener el estado de los cambios de datos y garantizar una sincronización de estado confiable.

#### Registro y monitorización

Para un mejor sistema de registro, considera las siguientes mejores prácticas:

- **Registro centralizado (*Centralized logging*):** Utiliza el registro centralizado para monitorizar servicios distribuidos, lo que facilita el rastreo de incidencias.
- **Métricas y comprobaciones de estado (*Metrics and health checks*):** Implementa comprobaciones de estado y métricas utilizando paquetes como `@nestjs/terminus` para monitorizar el estado y el rendimiento de los servicios.

#### Pruebas y CI/CD

Las siguientes prácticas te ayudarán a construir una arquitectura robusta:

- **Pruebas unitarias y de integración (*Unit and integration tests*):** Escribe pruebas unitarias para componentes individuales y pruebas de integración para las interacciones entre servicios.
- **Integración y despliegue continuos (*Continuous Integration and Deployment* - CI/CD):** Configura canalizaciones de CI/CD para compilar, probar y desplegar automáticamente los servicios en cada commit.

#### API Gateway y seguridad

Para cuestiones de seguridad, asegúrate de considerar la incorporación de los siguientes componentes:

- **API Gateway:** Utiliza un API gateway para centralizar el acceso externo a los servicios e implementar aspectos transversales como la autenticación y la limitación de tasa (*rate limiting*).
- **Autenticación y autorización (*Authentication and authorization*):** Implementa las mejores prácticas de seguridad para proteger los servicios del acceso no autorizado.

Al adherirte a estas mejores prácticas para construir microservicios con NestJS, puedes crear un sistema que no solo sea escalable y confiable, sino también fácil de mantener y evolucionar con el tiempo. Desde el diseño modular hasta la gestión eficaz de la configuración y el manejo robusto de errores, estas estrategias te ayudarán a construir microservicios resilientes que puedan adaptarse a las cambiantes necesidades comerciales.

Con estas mejores prácticas en mente, la siguiente sección te guiará a través de los desafíos comunes que puedes enfrentar al desarrollar microservicios y te ofrecerá técnicas de resolución de problemas y depuración para superarlos.

---

### Resolución de problemas y depuración de microservicios

Las arquitecturas de microservicios a menudo involucran sistemas distribuidos complejos, lo que hace que la resolución de problemas y la depuración sean cruciales. A continuación se presentan estrategias y herramientas específicas para microservicios de NestJS que pueden ayudarte.

#### Registro centralizado

Para un sistema de registro centralizado, considera las siguientes prácticas:

- **Implementar registro estructurado (*Implement structured logging*):** Utiliza el registro estructurado para facilitar el análisis de los registros. Integra bibliotecas como `winston` o `bunyan` para estructurar los datos de log.
- **Agregación de registros (*Log aggregation*):** Agrega logs utilizando herramientas como el stack ELK (Elasticsearch, Logstash y Kibana) o plataformas de registro centralizado para obtener una vista unificada de tu sistema.

#### Rastreo y monitorización

Para una mejor monitorización, considera agregar los siguientes componentes:

- **Rastreo distribuido (*Distributed tracing*):** Integra herramientas de rastreo como OpenTelemetry o Jaeger para visualizar los flujos de solicitudes a través de los microservicios, lo que ayuda a identificar cuellos de botella y fallos.
- **Monitorización de métricas (*Metric monitoring*):** Utiliza herramientas de monitorización como Prometheus y Grafana para realizar un seguimiento de métricas críticas como la latencia, las tasas de error y el rendimiento (*throughput*).

#### Salud y resiliencia del servicio

Para asegurarte de que puedes evaluar la salud de tu sistema, considera agregar las siguientes capacidades a tu sistema:

- **Comprobaciones de estado (*Health checks*):** Implementa comprobaciones de estado mediante `@nestjs/terminus` para monitorizar la disponibilidad de los servicios y las dependencias.
- **Disyuntores y lógica de reintentos (*Circuit breakers and retry logic*):** Utiliza herramientas como `@nestjs/microservices` o `@nestjs/bull` para implementar disyuntores (*circuit breakers*) y lógica de reintentos para gestionar los fallos de los servicios.

#### Técnicas de depuración

A continuación se presentan algunas de las técnicas más comunes para una mejor depuración:

- **Depuración local (*Local debugging*):** Utiliza herramientas como el comando `start:debug` de Nest CLI y el depurador de VSCode para ejecutar el código paso a paso localmente.
- **Depuración remota (*Remote debugging*):** Habilita la depuración remota configurando tu aplicación NestJS con los flags adecuados para adjuntar depuradores a los servicios en ejecución.

#### Control de versiones y rollback

Para un mejor control de versiones, asegúrate de considerar las siguientes técnicas:

- **Versionado de servicios (*Versioning services*):** Mantén el control de versiones para cada microservicio de forma independiente para rastrear los cambios y su impacto.
- **Despliegues canary y rollbacks (*Canary deployments and rollbacks*):** Implementa despliegues *canary* para lanzar cambios gradualmente y revertirlos (*rollback*) en caso de problemas.

#### Gestión de dependencias

Para una mejor gestión de dependencias, considera las siguientes mejores prácticas:

- **Mantener las dependencias actualizadas (*Keep dependencies updated*):** Actualiza regularmente las dependencias para evitar vulnerabilidades de seguridad y beneficiarte de las últimas funciones y correcciones de errores.
- **Análisis de dependencias (*Dependency analysis*):** Utiliza herramientas como `npm audit` para analizar tus dependencias en busca de vulnerabilidades conocidas.

#### Aislamiento de servicios y pruebas

Para mejores pruebas en una arquitectura de microservicios, las siguientes prácticas pueden salvar tu día:

- **Stubs y mocks de servicios (*Service stubs and mocks*):** Utiliza *stubs* y *mocks* para aislar los servicios durante las pruebas, lo que facilita la identificación de las causas fundamentales de los fallos.
- **Pruebas de integración (*Integration Testing*):** Desarrolla pruebas de integración integrales que simulen las interacciones entre servicios en un entorno controlado.

#### Documentación e intercambio de conocimientos

Considera las siguientes prácticas para una mejor mantenibilidad:

- **Documentar los contratos de servicio (*Document service contracts*):** Documenta claramente los contratos de API y los patrones de eventos para ayudar a los desarrolladores a comprender las interacciones entre servicios.
- **Intercambio de conocimientos (*Knowledge sharing*):** Comparte guías de resolución de problemas y análisis post-mortem para fomentar una cultura de mejora continua.

Al comprender e implementar estas estrategias de resolución de problemas y depuración, puedes navegar eficazmente por las complejidades de la arquitectura de microservicios, especialmente cuando utilizas NestJS. Desde el registro centralizado hasta el rastreo distribuido y las comprobaciones de estado, estas técnicas ayudan a mantener la confiabilidad y el rendimiento de tus microservicios. Ya sea que estés depurando local o remotamente, gestionando dependencias o implementando actualizaciones, estas mejores prácticas garantizan que tus servicios sigan siendo resilientes y receptivos.

---

### Resumen

Este capítulo ofreció una guía completa para construir microservicios robustos con NestJS, transitando de arquitecturas monolíticas a escalables. Comenzamos definiendo e implementando microservicios, destacando las fortalezas del diseño modular de NestJS que facilita la comunicación entre servicios y la gestión de datos. Luego creamos una estructura de monorepo que aloja múltiples servicios, permitiéndoles aprovechar todo el potencial de la arquitectura de microservicios de NestJS.

Continuando, nos enfocamos en la comunicación entre servicios, demostrando cómo los servicios pueden interactuar a través de peticiones HTTP síncronas y patrones asíncronos dirigidos por eventos. Utilizando ejemplos prácticos como un sistema de gestión de pedidos, aprendimos a configurar manejadores de eventos, definir DTOs e implementar patrones de eventos para una interacción fluida entre servicios.

Además, exploramos las mejores prácticas para construir microservicios con NestJS, enfatizando la modularización, la gestión de la configuración específica del entorno y la resiliencia a través de disyuntores y reintentos. Aprendimos la importancia de un API gateway centralizado que enruta el tráfico externo a los servicios apropiados, maneja la autenticación y garantiza una comunicación segura y eficiente.

Finalmente, exploramos estrategias para la resolución de problemas y la depuración de microservicios, centrándonos en el registro estructurado, el rastreo distribuido y la monitorización del rendimiento para garantizar la confiabilidad y el mantenimiento del sistema. Aprendimos la importancia de técnicas como el registro centralizado, las comprobaciones de salud y los disyuntores para mejorar la resiliencia y proporcionar una detección temprana de fallos.

En el próximo capítulo, aprenderemos a probar una arquitectura de microservicios en NestJS, así que mantente atento.
