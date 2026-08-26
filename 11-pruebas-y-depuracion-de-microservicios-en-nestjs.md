# Parte 4: Escalando con Microservicios y NestJS

## Capítulo 11: Pruebas y Depuración de Microservicios en NestJS

¡Bienvenido al Capítulo 11, donde haremos la transición desde los bloques de construcción prácticos de la arquitectura de microservicios en NestJS hacia los aspectos críticos de las pruebas! Con el proyecto configurado en el [Capítulo 10](https://subscription.packtpub.com/book/web-development/9781835468609/10), este capítulo se centrará en mejorar la confiabilidad y el rendimiento de tus microservicios mediante pruebas exhaustivas.

Si bien todo lo aprendido en los Capítulos 7 y 8 sobre las pruebas de una aplicación NestJS seguirá siendo aplicable aquí, NestJS ofrece una plataforma versátil para el desarrollo y prueba de microservicios, y este capítulo profundizará en la escritura de pruebas para operaciones entre servicios y en garantizar la integridad de los datos. Abordaremos escenarios del mundo real, basándonos en los ejemplos presentados en capítulos anteriores para demostrar la importancia de las pruebas y la depuración en arquitecturas de microservicios escalables.

En este capítulo, esto es lo que exploraremos juntos:

- Pruebas unitarias para microservicios
- Pruebas de integración para microservicios
- Depuración de problemas comunes y soluciones en pruebas de microservicios
- Mejores prácticas para pruebas y depuración de microservicios

El proyecto del [Capítulo 10](https://subscription.packtpub.com/book/web-development/9781835468609/10) debería estar listo ahora mientras nos adentramos en sus pruebas. Al final de este capítulo, estarás equipado con las habilidades para escribir pruebas unitarias y de integración, solucionar problemas e implementar las mejores prácticas para probar y depurar microservicios en NestJS.

¿Listo para mejorar tus habilidades de prueba y depuración de microservicios? ¡Comencemos!

---

### Requisitos técnicos

Para este capítulo, asegúrate de tener listo tu entorno local siguiendo los pasos del Capítulo 3. También deberías tener listo el proyecto construido en el [Capítulo 10](https://subscription.packtpub.com/book/web-development/9781835468609/10) para evitar enfrentar cualquier problema al leer este. [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS).

---

### Pruebas unitarias para microservicios

En esta sección, nos centraremos en probar funcionalidades específicas de microservicios en NestJS, prestando especial atención a las operaciones entre servicios en lugar de las pruebas unitarias básicas de APIs. Si necesitas un repaso sobre los fundamentos de las pruebas unitarias, consulta los Capítulos 7 y 8. Aquí, asumimos que ya estás familiarizado con esos conceptos básicos y ahora estás profundizando en las pruebas para microservicios.

Las pruebas unitarias de microservicios implican probar unidades individuales de funcionalidad de forma aislada, incluida la comunicación entre servicios, que es un aspecto definitorio de la arquitectura de microservicios. En NestJS, los microservicios a menudo se comunican mediante eventos o mensajes transmitidos a través de un intermediario de mensajes (*message broker*) o un cliente proxy (*client proxy*). Esto significa que debemos centrarnos en simular (*mocking*) servicios externos y garantizar que las llamadas entre servicios se manejen adecuadamente y se prueben de forma aislada.

Comencemos probando el flujo de creación de órdenes.

#### Pruebas de createOrder en el servicio de órdenes

Para las pruebas unitarias, de manera similar a lo que hicimos en los Capítulos 7 y 8, nos enfocamos en probar cada pieza de lógica contenida en los archivos de servicio (que contienen la lógica de negocio de la aplicación).

Dado que estamos escribiendo pruebas para el proyecto construido en el capítulo anterior, toma el siguiente ejemplo de la clase `OrderService` (este código se puede encontrar en la carpeta del [Capítulo 10](https://subscription.packtpub.com/book/web-development/9781835468609/10) en el repositorio de GitHub del libro: [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch10](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch10)), donde se crea una orden y se emite un evento al `InventoryService`:

```typescript
// order.service.ts
createOrder(createOrderInput: CreateOrderInput): Order {
  const order = {
    ...createOrderInput,
    id: `${this.orders.length + 1}`,
    status: OrderStatus.PENDING,
  };
  this.orders.push(order);
  console.log('Order created:', order, this.orders);

  // emit event to the inventory service
  this.inventoryClient.emit(EVENTS.ORDER_CREATED, order);

  return order;
}
```

Aquí, el método `createOrder` hace dos cosas:

1. Crea y almacena una orden.
2. Emite un evento `ORDER_CREATED` al servicio de inventario.

Para probar unitariamente este método, debes hacer lo siguiente:

1. Asegurarte de que la orden se cree y almacene correctamente.
2. Asegurarte de que el evento se emita al servicio de inventario.

Dado que este método depende de `ClientProxy` (para emitir eventos), simularemos (*mock*) esta dependencia para aislar la funcionalidad.

Para poder probar el método `createOrder`, debemos seguir los siguientes pasos:

1. **Simulación de dependencias (Mocking dependencies):** Simularemos `ClientProxy` para emular la comunicación entre servicios sin enviar realmente el evento.
2. **Prueba de la lógica (Testing the logic):** Probaremos si la orden se crea y almacena adecuadamente.
3. **Verificación de la emisión de eventos (Verifying event emission):** Verificaremos que el evento `ORDER_CREATED` se emita con el payload correcto.

Utilizaremos Jest como nuestro framework de pruebas, ya que es totalmente compatible con NestJS mediante la creación de archivos `.spec.ts`.

En el directorio `apps/order/src/`, crea un archivo `order.service.spec.ts` y pega el siguiente código:

```typescript
// add necessary imports
describe('OrderService', () => {
  let orderService: OrderService;
  let inventoryClient: ClientProxy;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        OrderService,
        {
          provide: 'INVENTORY_SERVICE',
          useValue: { emit: jest.fn() }, // Mocking ClientProxy's emit function
        },
      ],
    }).compile();

    orderService = module.get<OrderService>(OrderService);
    inventoryClient = module.get<ClientProxy>('INVENTORY_SERVICE');
  });

  it('should create an order and emit an event to the inventory service', () => {
    const createOrderInput: CreateOrderInput = {
      product: 'Laptop',
      quantity: 2,
      userId: 'user123',
    };

    // Call the createOrder method
    const result = orderService.createOrder(createOrderInput);

    // Assertions
    expect(result).toEqual({
      ...createOrderInput,
      id: '1',
      status: OrderStatus.PENDING,
    });

    // Ensure the order was added to the orders array
    expect(orderService['orders'].length).toBe(1);

    // Verify that the emit function was called with the correct event and payload
    expect(inventoryClient.emit).toHaveBeenCalledWith(
      EVENTS.ORDER_CREATED,
      {
        ...createOrderInput,
        id: '1',
        status: OrderStatus.PENDING,
      }
    );
  });
});
```

En el código anterior, estos son los componentes principales:

- **Simulación de la comunicación entre servicios (*Mocking inter-service communication*):** Simulamos `ClientProxy` (que es responsable de comunicarse con `InventoryService`) utilizando `jest.fn()` de Jest para garantizar que el método `emit` se llame correctamente sin realizar la llamada de red real.
- **Aislamiento de la lógica (*Isolating logic*):** Dado que el objetivo de las pruebas unitarias es probar la funcionalidad de forma aislada, nos aseguramos de que solo se esté probando la lógica de `OrderService`, sin interacciones reales con `InventoryService`.
- **Aserciones (*Assertions*):** Comprobamos que la lógica de creación de la orden sea correcta y también verificamos que el evento se haya emitido con el payload adecuado.

De manera similar, otras operaciones entre servicios en microservicios se pueden probar simulando la capa de comunicación, como las siguientes:

- Probar el método `handleOrderProcessed`, que actualiza una orden según el resultado recibido de `InventoryService`.
- Probar escenarios de fallo, como cuando `InventoryService` está caído o responde con un error.

Las pruebas unitarias de microservicios implican simular dependencias para garantizar que cada servicio funcione como se espera de forma aislada. En el caso de NestJS, la comunicación entre servicios suele ocurrir a través de eventos o mensajes, por lo que simulamos esas capas de comunicación para verificar que los servicios interactúen correctamente. Siguiendo este patrón, puedes probar a fondo cada componente de microservicio y tener confianza en la confiabilidad de tu arquitectura de microservicios.

Para obtener una explicación más detallada de las prácticas básicas de pruebas unitarias, consulta los Capítulos 7 y 8. Aquí nos estamos centrando exclusivamente en estrategias de prueba específicas para microservicios.

A continuación, exploraremos las pruebas de integración, donde iremos más allá de las pruebas unitarias aisladas para probar cómo funcionan los servicios en conjunto dentro de una arquitectura de microservicios.

---

### Pruebas de integración para microservicios

Ahora que hemos cubierto las pruebas unitarias, pasemos a las pruebas de integración: un paso crítico para garantizar que los microservicios funcionen juntos como se espera en un entorno real. Mientras que las pruebas unitarias se centran en probar la lógica de servicios individuales de forma aislada, las pruebas de integración verifican que los diferentes servicios puedan comunicarse, compartir datos y realizar tareas coordinadas de manera eficaz.

En una arquitectura de microservicios, los servicios a menudo interactúan a través de colas de mensajes, peticiones HTTP o mecanismos orientados a eventos. Por lo tanto, las pruebas de integración van más allá de simplemente probar la lógica de servicios individuales; garantizan la confiabilidad de la comunicación entre servicios.

Usaremos el ejemplo de `OrderService` e `InventoryService` del proyecto para demostrar cómo se realizan las pruebas de integración en una configuración de microservicios. Dado que estos servicios interactúan mediante la emisión de eventos (por ejemplo, cuando se crea una orden en `OrderService`, se envía un evento a `InventoryService` para procesar la orden), nuestras pruebas de integración se centrarán en garantizar que estas interacciones funcionen según lo previsto.

Al escribir pruebas de integración para microservicios, por lo general necesitarás hacer lo siguiente:

1. **Ejecutar servicios juntos:** Iniciar los servicios para que puedan comunicarse entre sí.
2. **Utilizar protocolos de comunicación reales:** En lugar de simular las llamadas entre servicios como lo harías en las pruebas unitarias, se utilizarán intermediarios de mensajes reales (p. ej., RabbitMQ, Kafka) o capas de transporte (p. ej., Transmission Control Protocol (TCP), HTTP). En nuestro proyecto de ejemplo, recuerda que estábamos usando TCP, pero el principio es prácticamente el mismo.
3. **Probar el flujo de datos real:** Asegurarte de que los servicios envíen y reciban datos correctamente a través de estos canales de comunicación.

En nuestro ejemplo, nos centraremos en probar el siguiente flujo:

1. Cuando se crea una orden en `OrderService`, se emite un evento a `InventoryService`.
2. `InventoryService` maneja el evento, procesa la orden y emite un evento de vuelta a `OrderService` con el resultado (éxito o fallo).
3. `OrderService` actualiza el estado de la orden en función del resultado de `InventoryService`.

#### Pruebas de integración para la creación de órdenes y actualización de inventario

En este ejemplo, probaremos cómo interactúa `OrderService` con `InventoryService`. Esto incluye crear una orden, enviar un evento a `InventoryService` y asegurarse de que la orden se actualice en consecuencia.

#### Configuración e inicialización de pruebas

En esta subsección, configuraremos la aplicación NestJS, estableceremos la comunicación de microservicios sobre TCP e inicializaremos el entorno de prueba utilizando Jest. Para comenzar, en el archivo `apps/microservices-sample/test/app.e2e-spec.ts` (nuestro proyecto API gateway), pega el siguiente código:

```typescript
// add all the necessary imports
describe('Order and Inventory Services Integration Test', () => {
  let app: INestApplication;
  let orderClient: ClientProxy;
  let inventoryClient: ClientProxy;

  beforeAll(async () => {
    const module: TestingModule = await Test.createTestingModule({
      imports: [
        OrderModule,
        InventoryModule,
        ClientsModule.register([
          {
            name: 'ORDER_SERVICE',
            transport: Transport.TCP,
            options: { port: 8001 },
          },
          {
            name: 'INVENTORY_SERVICE',
            transport: Transport.TCP,
            options: { port: 8002 },
          },
        ]),
      ],
    }).compile();

    app = module.createNestApplication();
    app.connectMicroservice({
      transport: Transport.TCP,
      options: { port: 8001 },
    });
    app.connectMicroservice({
      transport: Transport.TCP,
      options: { port: 8002 },
    });

    await app.startAllMicroservices();
    await app.init();

    orderClient = app.get<ClientProxy>('ORDER_SERVICE');
    inventoryClient = app.get<ClientProxy>('INVENTORY_SERVICE');
  });

  afterAll(async () => {
    await app.close();
  });
});
```

En el código anterior, tenemos los siguientes componentes:

- **Creación del módulo:** Usamos `Test.createTestingModule` para configurar el entorno de prueba, importando tanto `OrderModule` como `InventoryModule`.
- **Conexiones de microservicios:** Conectamos los microservicios a través de TCP. `OrderService` escucha en el puerto 8001 e `InventoryService` en el puerto 8002.
- **Inicio de la aplicación:** Usamos `startAllMicroservices()` para inicializar los microservicios y comenzar la comunicación.

Ahora, trabajemos en la prueba en sí.

#### Creación de una orden y prueba de respuesta

A continuación, simulamos la creación de una orden utilizando `supertest` (usamos supertest en el [Capítulo 7](https://subscription.packtpub.com/book/web-development/9781835468609/7), así que asegúrate de echarle un vistazo para un repaso) y verificamos que `OrderService` la maneje adecuadamente:

```typescript
import * as request from 'supertest';
import { OrderStatus } from '@app/constants';

it('should create an order and update the inventory accordingly', async () => {
  // Step 1: Create an order by making a POST request to
  // the Order Service
  const createOrderResponse = await request(app.getHttpServer())
    .post('/orders')
    .send({ product: 'Laptop', quantity: 2, userId: 'user123' })
    .expect(201);

  const order = createOrderResponse.body;
  expect(order.status).toBe(OrderStatus.PENDING);
});
```

En el código anterior, usamos `supertest` para simular una petición POST a `OrderService` para crear una nueva orden. Después de que se crea la orden, verificamos que el estado de la orden esté configurado en `PENDING`. Este es el comportamiento esperado.

#### Emisión de eventos y prueba del procesamiento de inventario

Después de crear la orden, probamos la comunicación entre `OrderService` e `InventoryService`. Espiaremos la función `emit` de `InventoryService` para asegurarnos de que reciba la orden y la procese:

```typescript
import { EVENTS } from '@app/constants';

it('should create an order and update the inventory accordingly', async () => {
  // step 1 remains the same
  // ...
  // step 2: Verify that the Inventory Service processes
  // the order and emits a result
  const orderProcessedSpy = jest.spyOn(
    inventoryClient,
    'emit'
  );

  // Emit event to simulate InventoryService processing the
  // order
  await inventoryClient.emit(EVENTS.ORDER_CREATED, order);

  // Simulate response from Inventory Service
  const inventoryProcessedPayload = {
    success: true,
    message: 'Order processed successfully',
    orderId: order.id,
  };

  // Ensure the order was processed
  expect(orderProcessedSpy).toHaveBeenCalledWith(
    EVENTS.ORDER_PROCESSED,
    inventoryProcessedPayload
  );

  // next step below
});
```

En el código anterior, usamos `jest.spyOn` para monitorear cuándo se llama al método `emit` de `InventoryService`. Esto nos permite verificar que el evento `ORDER_CREATED` se emita correctamente. Luego, simulamos el procesamiento de inventario emitiendo el evento `ORDER_CREATED` con el payload de la orden. Finalmente confirmamos que `InventoryService` emita el evento `ORDER_PROCESSED` con el payload correcto.

Por último, para verificar que `OrderService` actualice el estado de la orden a `COMPLETED` después de recibir una respuesta de `InventoryService`, agregaremos el siguiente código en el archivo `app.e2e-spec.ts`:

```typescript
// Step 3: Verify that the Order Service updates the
// order status to COMPLETED
const updatedOrder = await request(app.getHttpServer())
  .get(`/orders/${order.id}`)
  .expect(200);

expect(updatedOrder.body.status).toBe(OrderStatus.COMPLETED);
```

En el código anterior, enviamos una petición GET para recuperar la orden por su ID. Después del procesamiento, verificamos que el estado de la orden se haya actualizado de `PENDING` a `COMPLETED`.

Ahora, probemos todo ejecutando el comando `yarn test:e2e` en la raíz del proyecto y asegurémonos de que la prueba se ejecute exitosamente, como se muestra en la Figura 11.1 a continuación:

![Figura 11.1: Pruebas de integración en el proyecto API Gateway](./images/figure-11-1.png)
*Figura 11.1: Pruebas de integración en el proyecto API Gateway*

En esta sección, demostramos cómo realizar pruebas de integración entre `OrderService` e `InventoryService`. La prueba simula peticiones HTTP reales y comunicación entre servicios a través de la emisión de eventos, asegurando que todo el flujo de procesamiento de órdenes funcione como se espera. Este enfoque de pruebas de integración garantiza que ambos servicios operen correctamente juntos en un entorno distribuido y que los datos fluyan sin problemas entre ellos.

Al dividir la prueba en pasos más pequeños (inicializar la aplicación, simular la creación de una orden, verificar la emisión de eventos y confirmar las actualizaciones de estado), puedes probar las interacciones de microservicios de manera eficaz manteniendo cada caso de prueba claro y enfocado.

En la siguiente sección, centrémonos en la depuración.

---

### Depuración de problemas comunes y soluciones en pruebas de microservicios

Al probar microservicios, especialmente en un entorno distribuido, la depuración puede volverse compleja rápidamente debido a la comunicación entre servicios, los procesos asíncronos y los posibles fallos en múltiples componentes que son difíciles de rastrear. La buena noticia es que NestJS proporciona funciones y herramientas potentes para ayudar a resolver estos problemas de manera sistemática y eficiente. En esta sección, repasaremos algunos problemas comunes y presentaremos técnicas sofisticadas para depurarlos de forma escalable.

#### Problema 1 – Microservicios que no logran conectarse

Un problema común en los microservicios es el fallo de conexión entre servicios. Esto puede deberse a lo siguiente:

- Configuraciones de puerto incorrectas
- Puntos de conexión de red (*endpoints*) inalcanzables
- Nombres de servicio que no coinciden

Simulemos y solucionemos este problema paso a paso.

Para reproducir este problema, sigue estos pasos:

1. Inicia tus microservicios asegurándote de que estén configurados para comunicarse a través de TCP.
2. Ejecuta tus pruebas de integración y verifica si hay errores de conexión, como `ECONNREFUSED` o `ECONNRESET`.
3. Puedes simular un escenario de puerto incorrecto en tu código de la siguiente manera:

```typescript
app.connectMicroservice({
  transport: Transport.TCP,
  options: { port: 8001 }, // Simulating an incorrect port
});
```

Aquí simulamos una situación donde la configuración del puerto es incorrecta, lo que provoca problemas de conexión entre `OrderService` e `InventoryService`.

Siempre que encuentres una situación de este tipo, tenemos algunas formas posibles de depurarla y solucionarla, como se muestra en la siguiente lista:

1. **Verificar configuraciones de puertos:** Asegúrate de que los puertos configurados en tu prueba coincidan con los puertos reales de los servicios en ejecución. Por ejemplo, verifica los puertos usando el siguiente comando en tu terminal:

```bash
$ netstat -an | grep 8001
```

Alternativamente, puedes usar `lsof` para comprobar qué puertos están en uso:

```bash
$ lsof -i :8001
```

Esto te ayuda a asegurarte de que los servicios no se estén ejecutando en puertos conflictivos. Así es como puedes configurar correctamente tu microservicio para evitar codificar puertos de forma fija (*hardcoding*):

```typescript
app.connectMicroservice({
  transport: Transport.TCP,
  options: { port: process.env.ORDER_SERVICE_PORT || 8001 }, // Avoid hardcoding ports
});
```

Al utilizar variables de entorno, permites flexibilidad para diferentes entornos (desarrollo local, *staging*, producción), lo que facilita la gestión de conflictos de puertos.

2. **Verificar nombres de servicios y puertos:** Si has registrado servicios con nombres específicos (p. ej., `ORDER_SERVICE`, `INVENTORY_SERVICE`), asegúrate de que estos nombres sean consistentes tanto en el entorno de prueba como en la configuración real del microservicio. La inconsistencia en los nombres puede provocar que los microservicios fallen durante el descubrimiento de servicios (*service discovery*).

Para verificarlo, puedes agregar registros de depuración (*logs*) para confirmar que los servicios se están iniciando con los nombres y puertos correctos:

```typescript
console.log(
  'Order Service running on port',
  process.env.ORDER_SERVICE_PORT || 8001
);
console.log(
  'Inventory Service running on port',
  process.env.INVENTORY_SERVICE_PORT || 8002
);
```

En aplicaciones distribuidas a gran escala, codificar puertos de forma fija puede generar problemas en entornos como tuberías de CI/CD o sistemas en contenedores, donde los puertos pueden necesitar asignarse dinámicamente. La mejor práctica es utilizar variables de entorno para gestionar las configuraciones de los servicios, incluidos los puertos.

Aquí tenemos un ejemplo de cómo hacerlo:

```typescript
// app.module.ts
@Module({
  imports: [
    ClientsModule.register([
      {
        name: 'ORDER_SERVICE',
        transport: Transport.TCP,
        options: { port: process.env.ORDER_SERVICE_PORT || 8001 }, // Flexible port assignment
      },
      {
        name: 'INVENTORY_SERVICE',
        transport: Transport.TCP,
        options: { port: process.env.INVENTORY_SERVICE_PORT || 8002 }, // Flexible port assignment
      },
    ]),
  ],
})
export class AppModule {}
```

En el fragmento de código anterior, la clase `AppModule` está decorada con el decorador `@Module()`, que es un bloque de construcción fundamental en NestJS para organizar y agrupar componentes relacionados. Dentro del módulo, importamos `ClientsModule` y registramos dos microservicios: `ORDER_SERVICE` e `INVENTORY_SERVICE`.

Ambos servicios utilizan la capa de transporte `Transport.TCP` para la comunicación entre servicios. En lugar de codificar de forma rígida los números de puerto, utilizamos variables de entorno (`process.env.ORDER_SERVICE_PORT` y `process.env.INVENTORY_SERVICE_PORT`) para asignar puertos dinámicamente. Si las variables de entorno no están configuradas, se utilizan los puertos predeterminados (8001 para `ORDER_SERVICE` y 8002 para `INVENTORY_SERVICE`).

Este enfoque garantiza flexibilidad y escalabilidad, permitiendo que los microservicios se ejecuten en diferentes entornos (desarrollo, *staging*, producción) sin requerir valores fijos en el código, lo que puede causar problemas como conflictos de puertos. Al aprovechar las variables de entorno, la aplicación se adapta mejor a entornos contenedorizados o de CI/CD, donde las asignaciones de puertos pueden variar dinámicamente.

#### Problema 2 – Emisión no confiable de eventos entre microservicios

Los microservicios a menudo se comunican a través de eventos, pero las emisiones de eventos pueden fallar debido a problemas de red, errores de configuración o problemas de descubrimiento de servicios. Esto puede dar como resultado que los eventos no se entreguen o no se procesen.

Estos son los pasos para reproducir el problema:

1. Simula un fallo en la emisión de eventos introduciendo un error de configuración deliberado o deteniendo el servicio receptor.
2. Emite un evento desde `OrderService` hacia `InventoryService` y observa el fallo en el procesamiento:

```typescript
// apps/inventory/inventory.service.ts
// ...
orderClient.emit(EVENTS.ORDER_CREATED, order); // Simulate event emission
```

En este caso, es posible que `InventoryService` no reciba el evento `ORDER_CREATED` debido a un fallo de comunicación.

Para manejar este escenario de forma adecuada, estos son los pasos sencillos que se pueden seguir:

1. **Verificar el registro del servicio:**
Asegúrate de que ambos servicios estén correctamente registrados y sean accesibles. Utiliza `nestjs-devtools` o registros para verificar que los servicios estén registrados correctamente y emitiendo/recibiendo eventos como se espera.

```typescript
console.log('Emitting ORDER_CREATED event for Order ID:', order.id);
```

2. **Habilitar el mecanismo de reintento:**
Configura un mecanismo de reintento para la emisión de eventos en caso de fallos transitorios. NestJS proporciona una forma sencilla de implementar reintentos para la comunicación basada en eventos:

```typescript
ClientsModule.register([
  {
    name: 'ORDER_SERVICE',
    transport: Transport.TCP,
    options: {
      port: process.env.ORDER_SERVICE_PORT || 8001,
      retryAttempts: 5, // Retry 5 times before failing
      retryDelay: 3000, // Delay of 3 seconds between retries
    },
  },
]);
```

Los reintentos son particularmente útiles para mitigar interrupciones temporales de la red o retrasos en el inicio del servicio.

3. **Utilizar colas de mensajes fallidos (Dead Letter Queues - DLQ):**
Para manejar emisiones de eventos fallidas en sistemas de producción, considera configurar una DLQ donde los mensajes no entregables se enruten para su posterior investigación. Esto ayuda a evitar la pérdida de datos y permite el análisis posterior de eventos fallidos.

#### Problema 3 – Datos inconsistentes entre microservicios

En las arquitecturas de microservicios, la inconsistencia de datos puede surgir si un servicio actualiza su estado pero otro servicio no lo hace (por ejemplo, si `OrderService` actualiza una orden pero `InventoryService` no ajusta el inventario).

Sigue estos pasos para reproducir este escenario:

1. Simula un fallo en `InventoryService` después de que una orden se crea exitosamente pero antes de que ocurra la actualización del inventario. Puedes hacer esto lanzando manualmente una excepción o devolviendo un error en `InventoryService` después de que `OrderService` complete su transacción. Por ejemplo, dentro del método `updateInventory`, introduce un bloque condicional como el siguiente código:

```typescript
updateInventory(productId: number, quantity: number): void {
  if (productId === 123) {
    throw new Error('Simulated inventory update failure');
  }
  // Proceed with inventory update logic
}
```

Como resultado, los datos en `OrderService` e `InventoryService` se vuelven inconsistentes.

Para depurar este escenario, sigue estos pasos:

1. **Utilizar transacciones distribuidas:** Aunque las transacciones distribuidas pueden ser complejas, una buena alternativa en una arquitectura de microservicios es usar patrones *saga* o *transactional outbox* para garantizar la consistencia entre servicios. NestJS cuenta con un excelente soporte para implementar sagas utilizando el módulo `@nestjs/cqrs`.

Aquí hay un ejemplo de implementación del patrón saga que puede ayudar a evitar el escenario anterior:

```typescript
// Saga example in OrderService
@Injectable()
export class OrderSaga {
  @Saga()
  orderCreated = (
    events$: Observable<OrderCreatedEvent>
  ): Observable<void> => {
    return events$.pipe(
      switchMap((event) => {
        // Handle transaction across services (Order
        // and Inventory)
        return of(
          inventoryService.updateInventory(event.orderId)
        );
      }),
      catchError((err) => {
        console.error('Error updating inventory:', err);
        return EMPTY;
      })
    );
  };
}
```

En el fragmento de código anterior, utilizamos el patrón saga para manejar transacciones distribuidas a través de múltiples servicios, como `OrderService` e `InventoryService`. Este patrón es particularmente útil para gestionar procesos de larga duración o de varios pasos. Contamos con los siguientes componentes:

- `Observable<OrderCreatedEvent>`: El método `orderCreated` escucha eventos relacionados con la creación de una orden. El flujo `events$` representa un observable que emite eventos cuando una orden se crea exitosamente.
- `switchMap`: Una vez creada una orden, el operador `switchMap` cambia la ejecución al método `inventoryService.updateInventory`, que actualiza el inventario para el `orderId` correspondiente. Esto ayuda a gestionar el flujo transaccional entre `OrderService` e `InventoryService`.
- **Manejo de errores (`catchError`):** Si la operación `inventoryService.updateInventory` falla, el error se captura mediante el operador `catchError`, que registra el error (`Error updating inventory:`) y devuelve un observable vacío (`EMPTY`) de RxJS. Esto evita que todo el proceso falle estrepitosamente y permite que el sistema reintente o aplique acciones compensatorias, dependiendo del diseño general de tu sistema.

Este patrón garantiza que incluso si `InventoryService` falla durante la actualización, el sistema pueda manejar la falla con elegancia, ya sea reintentando o tomando medidas alternativas. Evita el problema en el que una falla en un servicio (Inventario) hace que falle toda la operación (Orden).

2. **Utilizar consistencia eventual y patrones outbox:** Para garantizar la consistencia eventual, utiliza el patrón outbox, donde los eventos se almacenan en una tabla de base de datos (la "bandeja de salida" u *outbox*) y se procesan de forma asíncrona. Esto garantiza que incluso si falla la actualización del inventario, el evento se reintente hasta que tenga éxito:

```typescript
// Outbox pattern implementation
await this.outboxService.saveEvent(
  EVENTS.ORDER_CREATED,
  { orderId: order.id }
);
```

En el fragmento de código anterior, el método `saveEvent` almacena un evento `ORDER_CREATED` en la tabla de outbox, lo que permite que el evento se procese de forma asíncrona. Esto asegura que si un servicio como `InventoryService` falla, el evento permanezca en la outbox para reintentarse hasta que se procese con éxito, garantizando la consistencia eventual entre los servicios al reintentar operaciones fallidas sin perder datos importantes.

Con este patrón, tus servicios pueden recuperarse de fallos transitorios sin arriesgarse a estados de datos inconsistentes.

#### Problema 4 – Rendimiento lento en la comunicación entre servicios

Los cuellos de botella en el rendimiento de los microservicios pueden surgir por una comunicación de red lenta o por un manejo ineficiente de los mensajes entre servicios.

Esta es la forma de reproducir este problema:

1. Simula un tráfico alto creando una gran cantidad de órdenes en `OrderService` y enviándolas a `InventoryService` para su procesamiento. Para simular un tráfico elevado, puedes crear una prueba de estrés generando una gran cantidad de órdenes en `OrderService` y enviándolas a `InventoryService` para su procesamiento. Esto ayudará a identificar cuellos de botella en la comunicación entre servicios.

Aquí tienes un ejemplo de cómo simular un tráfico elevado en tu servicio:

```typescript
// Simulate high traffic by creating multiple orders
for (let i = 0; i < 1000; i++) {
  await this.orderService.createOrder({
    productId: i,
    quantity: 1,
  });
}
```

En el fragmento de código anterior, se utiliza un bucle para crear 1000 órdenes de forma asíncrona. Cada orden es procesada por `OrderService`, que envía los mensajes pertinentes a `InventoryService`. Este método ayuda a simular un tráfico elevado y puede resaltar posibles problemas de rendimiento, como latencia de red o cuellos de botella en el manejo de mensajes entre servicios.

2. Observa retrasos en el procesamiento y una posible sobrecarga del servicio.

Sigue estos pasos para depurarlo:

1. **Utilizar procesamiento de mensajes por lotes (*message batching*):** En lugar de procesar eventos uno por uno, implementa el procesamiento por lotes donde se procesan múltiples eventos en una sola petición. Esto reduce la cantidad de llamadas de red y mejora el rendimiento general (*throughput*).

```typescript
// Batch processing in InventoryService
inventoryClient.emit(
  EVENTS.ORDER_BATCH_CREATED,
  ordersBatch
); // Emit batch instead of single order
```

El código anterior emite un lote de eventos, lo que reduce la carga en lugar de emitir eventos uno tras otro.

2. **Utilizar comunicación asíncrona:** Asegúrate de que las tareas de larga duración se deleguen a trabajadores en segundo plano (*background workers*) o colas. Puedes utilizar colas de NestJS (mediante `@nestjs/bull`, por ejemplo) para manejar el procesamiento pesado en segundo plano.

El siguiente ejemplo muestra cómo puedes utilizar Bull en la cola de órdenes:

```typescript
// Example of queueing with Bull
@Processor('order_queue')
export class OrderProcessor {
  @Process('process_order')
  async handleOrder(job: Job<OrderData>) {
    // Process order asynchronously
  }
}
```

En el fragmento de código anterior, la clase `OrderProcessor` escucha a `order_queue`. Cuando se agrega un trabajo a la cola con el tipo `process_order`, el método `handleOrder` procesa la orden en segundo plano. Esto asegura que las tareas de larga duración no bloqueen el flujo principal de ejecución, mejorando el rendimiento general del sistema.

3. **Monitorear con NestJS DevTools y perfiladores (*profilers*):** Utiliza NestJS DevTools o soluciones APM integradas, como Jaeger o Zipkin, para monitorear el rendimiento de tus servicios. Estas herramientas pueden proporcionar información sobre la comunicación entre servicios, tiempos de respuesta y cuellos de botella:

```bash
$ npm install --save @nestjs/terminus @nestjs/devtools
```

Luego puedes visualizar métricas de rendimiento en tiempo real e identificar áreas donde se producen retrasos en la comunicación. Se pueden encontrar más detalles sobre NestJS DevTools aquí: [https://docs.nestjs.com/devtools/overview](https://docs.nestjs.com/devtools/overview).

Al abordar estos problemas comunes de los microservicios e implementar las soluciones sugeridas, deberías ver mejoras tanto en la confiabilidad como en la escalabilidad de tus sistemas distribuidos. Por supuesto, cada arquitectura de microservicios es única, y aunque aquí hemos cubierto una variedad de técnicas, el proceso de depuración es a menudo iterativo. A medida que pruebes y perfecciones tu sistema, es posible que encuentres otros desafíos, como configuraciones de tiempo de espera (*timeout*) de servicios, pérdida de mensajes o inquietudes de seguridad en la comunicación entre servicios.

Existen muchas herramientas y técnicas adicionales, como el rastreo distribuido (*distributed tracing*) con OpenTelemetry, pruebas de carga con herramientas como Apache JMeter o el uso de estrategias de orquestación más avanzadas con Kubernetes, que pueden mejorar aún más tus procesos de desarrollo y depuración de microservicios. Siéntete libre de explorar estas alternativas y adaptarlas a tu arquitectura para asegurarte de que tus microservicios funcionen sin problemas en todos los entornos.

Ahora que hemos abordado algunos de los desafíos de depuración más comunes, profundicemos en las mejores prácticas para probar y depurar microservicios, lo que consolidará aún más tu arquitectura de microservicios tanto para las necesidades de escalabilidad actuales como futuras.

---

### Mejores prácticas para pruebas y depuración de microservicios

Probar y depurar microservicios de manera eficaz puede resultar un desafío debido a la naturaleza distribuida de su arquitectura. Sin embargo, seguir ciertas mejores prácticas puede agilizar el proceso, hacer que tus servicios sean más confiables y ayudarte a detectar posibles problemas antes de que afecten a producción. En esta sección, nos centraremos en estrategias prácticas utilizando el código de muestra y los conceptos que hemos explorado anteriormente.

#### Aislar microservicios para pruebas unitarias

Si bien las pruebas de integración entre servicios son esenciales, probar unitariamente cada microservicio de forma independiente garantiza que los servicios individuales se comporten como se espera. En las pruebas de microservicios, el aislamiento es crucial porque cada servicio debe probarse sin dependencias de otros servicios.

Puedes lograr esto simulando o creando sustitutos (*stubs*) para las dependencias, como llamadas a bases de datos, APIs externas u otros servicios. Aquí tienes un ejemplo de cómo simular una dependencia al realizar pruebas unitarias de `OrderService`:

```typescript
// order.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { OrderService } from './order.service';
import { ClientsModule, Transport } from '@nestjs/microservices';

describe('OrderService', () => {
  let service: OrderService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [OrderService],
      imports: [
        ClientsModule.register([
          {
            name: 'INVENTORY_SERVICE',
            transport: Transport.TCP,
            options: { port: 8002 }, // Stubbed Inventory service
          },
        ]),
      ],
    }).compile();

    service = module.get<OrderService>(OrderService);
  });

  it('should create an order and emit an event', async () => {
    // Mock the Inventory service's response
    const order = { id: 1, items: ['item1', 'item2'] };
    const spy = jest.spyOn(service, 'createOrder');

    await service.createOrder(order);

    expect(spy).toHaveBeenCalledWith(order);
  });
});
```

En este código, estamos probando `OrderService` y su capacidad para crear una orden y emitir un evento. La prueba utiliza `ClientsModule` para registrar un `INVENTORY_SERVICE` simulado, emulando la comunicación a través de TCP. Se espía el método `createOrder` utilizando Jest para verificar que se llame con los argumentos correctos.

##### ¿Por qué es importante?

Aislar los servicios mediante simulaciones (*mocks*) o sustitutos (*stubs*) ayuda a detectar problemas dentro de un solo servicio sin depender de que otros servicios estén disponibles. Esta práctica permite una retroalimentación más rápida durante las pruebas.

#### Uso de variables de entorno para configuraciones

Ya hemos analizado las dificultades de codificar valores de forma fija (*hardcoding*), como los puertos, en tus servicios en la sección *Depuración de problemas comunes y soluciones en pruebas de microservicios*. El uso de variables de entorno es una mejor práctica que hace que tus microservicios sean más configurables y fáciles de probar en múltiples entornos (local, CI/CD, producción).

Así es como puedes configurar microservicios para usar variables de entorno de manera efectiva:

```typescript
// app.module.ts
@Module({
  imports: [
    ClientsModule.register([
      {
        name: 'ORDER_SERVICE',
        transport: Transport.TCP,
        options: { port: process.env.ORDER_SERVICE_PORT || 8001 }, // Use environment variables
      },
    ]),
  ],
})
export class AppModule {}
```

En este código, estamos definiendo `AppModule`, registrando el microservicio `ORDER_SERVICE` y utilizando `ClientsModule`. El transporte está configurado en TCP y el puerto se asigna dinámicamente mediante una variable de entorno (`ORDER_SERVICE_PORT`) con un valor predeterminado de 8001. Esta configuración permite flexibilidad en diferentes entornos.

##### ¿Por qué es importante?

Las variables de entorno hacen que tu aplicación sea más flexible. Puedes ejecutar el mismo servicio en diferentes entornos sin necesidad de cambiar el código, lo que reduce la posibilidad de errores de configuración.

#### Aprovechar las pruebas de integración para la comunicación entre servicios

En los microservicios, los servicios a menudo interactúan entre sí. Es esencial asegurarse de que estas interacciones funcionen como se espera. Las pruebas de integración deben verificar que los servicios puedan comunicarse, intercambiar datos y manejar respuestas correctamente.

Aquí tienes un ejemplo simplificado de cómo escribir una prueba de integración que verifique la comunicación entre `OrderService` e `InventoryService`:

```typescript
// order.integration.spec.ts
import { Test } from '@nestjs/testing';
import { OrderService } from './order.service';
import { ClientsModule, Transport } from '@nestjs/microservices';

describe('OrderService Integration Test', () => {
  let service: OrderService;

  beforeAll(async () => {
    const module = await Test.createTestingModule({
      providers: [OrderService],
      imports: [
        ClientsModule.register([
          {
            name: 'INVENTORY_SERVICE',
            transport: Transport.TCP,
            options: {
              port: process.env.INVENTORY_SERVICE_PORT || 8002,
            }, // Simulating Inventory service
          },
        ]),
      ],
    }).compile();

    service = module.get<OrderService>(OrderService);
  });

  it('should create an order and communicate with InventoryService', async () => {
    const order = { id: 1, items: ['item1', 'item2'] };

    await service.createOrder(order);

    // Verify that communication with InventoryService was
    // successful
  });
});
```

En este código, estamos configurando una prueba de integración para `OrderService` utilizando el `TestingModule` de NestJS. El `ClientsModule` registra el `INVENTORY_SERVICE` con una capa de transporte TCP, simulando el servicio al asignar un puerto mediante variables de entorno. La prueba garantiza que cuando se crea una orden, se comunique adecuadamente con `InventoryService`.

##### ¿Por qué es importante?

Las pruebas de integración detectan problemas que las pruebas unitarias podrían pasar por alto, como fallos de comunicación entre servicios o discordancias en los formatos de datos.

#### Habilitar mecanismos de reintento para mayor robustez

Los microservicios a menudo se comunican a través de redes poco confiables y, por lo tanto, pueden ocurrir fallos transitorios. Es fundamental incorporar mecanismos de reintento para manejar interrupciones temporales del servicio o retrasos. NestJS facilita la configuración de reintentos. Observa el siguiente código, por ejemplo:

```typescript
ClientsModule.register([
  {
    name: 'ORDER_SERVICE',
    transport: Transport.TCP,
    options: {
      port: process.env.ORDER_SERVICE_PORT || 8001,
      retryAttempts: 5, // Retry up to 5 times
      retryDelay: 3000, // 3-second delay between retries
    },
  },
]);
```

En este código, la clase `OrderSaga` maneja el evento `orderCreated` utilizando el decorador `@Saga()`. La saga `orderCreated` escucha eventos `OrderCreatedEvent` y los procesa utilizando operadores de RxJS. Cuando se crea una orden, intenta actualizar el inventario a través de `inventoryService`. Si se produce un error durante la actualización, se captura y se registra, y el flujo devuelve `EMPTY` para evitar una mayor propagación del error. Esta configuración garantiza un manejo fluido de las transacciones entre servicios.

##### ¿Por qué es importante?

Sin reintentos, tus microservicios pueden fallar cuando se encuentren con problemas de red temporales. Los mecanismos de reintento garantizan que tus servicios sean resilientes y puedan recuperarse de fallos transitorios.

#### Uso de NestJS DevTools para depuración en tiempo real

Para obtener información en tiempo real sobre tus servicios durante el desarrollo, NestJS proporciona una potente herramienta de desarrollo (*DevTool*) que te permite visualizar e interactuar con tu aplicación. Ayuda a monitorear el rendimiento y depurar problemas a través de los microservicios.

Ejecuta el siguiente comando en la raíz del proyecto para instalar las herramientas de depuración necesarias:

```bash
$ npm install --save @nestjs/terminus @nestjs/devtools
```

Después de la instalación, puedes monitorear fácilmente las interacciones de tus servicios, medir latencias e identificar cuellos de botella.

##### ¿Por qué es importante?

Las herramientas de monitoreo en tiempo real como NestJS DevTools te brindan retroalimentación inmediata sobre cómo se están desempeñando tus servicios, lo que facilita el diagnóstico y la resolución de problemas antes de que se intensifiquen.

#### Monitoreo y rastreo de sistemas distribuidos

En una arquitectura de microservicios, es crucial rastrear las peticiones a medida que fluyen a través de los diferentes servicios. Herramientas como Jaeger o Zipkin pueden ayudarte a rastrear peticiones y diagnosticar problemas como un rendimiento lento o fallos de servicio.

Integrar el rastreo distribuido es relativamente sencillo y te ayuda a visualizar cómo se mueven los datos entre los servicios. Así es como podrías integrar un sistema de rastreo en una aplicación NestJS.

En nuestro proyecto, podemos configurar Jaeger instalando primero todas las dependencias requeridas, ejecutando el siguiente comando en la raíz del proyecto:

```bash
$ yarn add @opentelemetry/sdk-trace-base @opentelemetry/sdk-node @opentelemetry/instrumentation-http @opentelemetry/instrumentation-express @opentelemetry/instrumentation-nestjs-core @opentelemetry/resources @opentelemetry/semantic-conventions
```

A continuación, debemos crear un archivo `tracing.ts` en la raíz del proyecto con el siguiente contenido:

```typescript
import {
  ConsoleSpanExporter,
  SimpleSpanProcessor,
} from '@opentelemetry/sdk-trace-base';
import { NodeSDK } from '@opentelemetry/sdk-node';
import * as process from 'process';
import { HttpInstrumentation } from '@opentelemetry/instrumentation-http';
import { ExpressInstrumentation } from '@opentelemetry/instrumentation-express';
import { NestInstrumentation } from '@opentelemetry/instrumentation-nestjs-core';
import { Resource } from '@opentelemetry/resources';
import { SemanticResourceAttributes } from '@opentelemetry/semantic-conventions';

const traceExporter = new ConsoleSpanExporter();

export const otel = (serviceName: string) => {
  const otelSDK = new NodeSDK({
    resource: new Resource({
      [SemanticResourceAttributes.SERVICE_NAME]: `${serviceName}-service`,
    }),
    spanProcessor: new SimpleSpanProcessor(traceExporter),
    instrumentations: [
      new HttpInstrumentation(),
      new ExpressInstrumentation(),
      new NestInstrumentation(),
    ],
  });

  // gracefully shut down the SDK on process exit
  process.on('SIGTERM', () => {
    otelSDK
      .shutdown()
      .then(
        () => console.log('SDK shut down successfully', serviceName),
        (err) => console.log('Error shutting down SDK', err),
      )
      .finally(() => process.exit(0));
  });

  return otelSDK;
};
```

En el fragmento de código anterior, configuramos el rastreo distribuido utilizando OpenTelemetry con Jaeger en un microservicio de NestJS, estructurado para un monorepositorio (*monorepo*). Aquí hay un desglose simple:

- **Exportador de tramos (*Span Exporter*):** Usamos `ConsoleSpanExporter()` para imprimir datos de traza en la consola, lo cual es útil para probar y verificar la configuración. En producción, reemplazarías esto con un exportador que envíe trazas a Jaeger u otra herramienta de recolección de trazas.
- **OpenTelemetry NodeSDK:** La configuración de `NodeSDK` inicializa el rastreo reuniendo todas las configuraciones necesarias. Los aspectos clave incluyen los siguientes:
  - **Recurso (*Resource*):** Definimos un `serviceName` para cada servicio con `SemanticResourceAttributes.SERVICE_NAME`. Por ejemplo, configurarlo en `order-service` ayuda a identificar el microservicio específico en Jaeger, facilitando el seguimiento de peticiones a través del sistema.
  - **Instrumentación (*Instrumentation*):** Se agregan instrumentaciones para capturar automáticamente datos de traza para HTTP, Express y NestJS.
    - **HTTP** rastrea peticiones salientes y entrantes, esencial para monitorear la comunicación entre servicios.
    - **Express** captura middleware y manejo de rutas, lo cual puede ser útil si tienes lógica de Express personalizada.
    - **NestJS** captura operaciones internas de NestJS, lo que permite visibilidad en las interacciones a nivel de controlador.
  - **Cierre ordenado (*Graceful shutdown*):** El SDK maneja cualquier traza pendiente antes de que la aplicación se cierre. Cuando el servicio recibe una señal de terminación (`SIGTERM`), se cierra de forma ordenada, asegurando que no se pierdan datos de traza.
  - **Uso del SDK:** Cada microservicio puede invocar esta configuración de rastreo pasando un `serviceName` único a `otel(serviceName)`. Esto permite un rastreo consistente a través de los diversos servicios del monorepo, facilitando el monitoreo y diagnóstico de problemas mediante la consola o el panel de control (*dashboard*) de Jaeger.

Esta configuración permite un seguimiento detallado de las peticiones a medida que viajan a través de la aplicación NestJS, ofreciendo visibilidad completa de los cuellos de botella o fallos, con cada servicio identificable dentro de una interfaz de usuario de rastreo distribuido. Para una visualización adicional, configura tu punto de conexión de Jaeger (p. ej., `OTEL_EXPORTER_JAEGER_ENDPOINT`) para enrutar los datos de traza a tu backend de rastreo.

Para ver el rastreo en acción, actualiza todo el código de `main.ts`, agregando la siguiente línea en su función `bootstrap`, antes de cualquier otro código:

```typescript
// apps/inventory/main.ts
const otelStd = otel('inventory');
await otelStd.start();

// apps/order/src/main.ts
const otelStd = otel('order');
await otelStd.start();

// apps/main/src/main.ts
const otelStd = otel('main');
await otelStd.start();
```

Ahora, iniciemos las tres aplicaciones una vez más, ejecutando los siguientes comandos en diferentes terminales para poder ver el rastreo en acción:

```bash
# to start the gateway
$ yarn start:dev

# to start the order service
$ yarn start:dev order

# to start the inventory service
$ yarn start:dev inventory
```

Con todos los microservicios iniciados, realicemos una petición POST en el endpoint `create-order` y observemos la consola. La Figura 11.1 muestra lo que obtenemos en la consola, informándonos sobre la petición recibida y los servicios involucrados.

![Figura 11.2: Rastreo de transacciones distribuidas](./images/figure-11-2.png)
*Figura 11.2: Rastreo de transacciones distribuidas*

Si bien esto puede ser útil al depurar microservicios, Jaeger nos brinda la capacidad de mostrar la misma información en un panel de control, y esto puede requerir una configuración adicional; se puede encontrar más información en la documentación oficial aquí: [https://opentelemetry.io/docs/demo/collector-data-flow-dashboard/](https://opentelemetry.io/docs/demo/collector-data-flow-dashboard/).

Para patrones de comunicación más complejos, como intermediarios de mensajes como RabbitMQ o Kafka, estos sistemas a menudo vienen con sus propios paneles de control nativos. Estas herramientas muestran los flujos de mensajes y la comunicación entre servicios, brindando información valiosa sobre las interacciones del sistema. Sin embargo, para simplificar, mantendremos nuestro enfoque aquí en el rastreo basado en HTTP con Jaeger.

##### ¿Por qué es importante?

El rastreo distribuido te permite identificar dónde ocurren los cuellos de botella o fallos a medida que las peticiones se mueven a través de tu sistema. Esto es esencial para depurar problemas de rendimiento en una arquitectura de microservicios distribuidos.

#### Uso de sagas para transacciones distribuidas

Pueden ocurrir datos inconsistentes entre servicios si un servicio actualiza su estado pero otro no lo hace. Implementar un patrón saga asegura que las operaciones entre servicios se completen exitosamente o se reviertan (*rollback*).

Así es como puedes implementar una saga en NestJS para manejar una transacción distribuida:

```typescript
@Injectable()
export class OrderSaga {
  @Saga()
  orderCreated = (events$: Observable<OrderCreatedEvent>): Observable<void> => {
    return events$.pipe(
      switchMap((event) => {
        // Handle transaction across services (Order and
        // Inventory)
        return of(inventoryService.updateInventory(event.orderId));
      }),
      catchError((err) => {
        console.error('Error updating inventory:', err);
        return EMPTY;
      })
    );
  };
}
```

En este código, la clase `OrderSaga` maneja eventos en un patrón saga para garantizar un procesamiento consistente entre servicios. El método `orderCreated` escucha un flujo de eventos `OrderCreatedEvent` y, para cada evento, intenta actualizar el inventario mediante el método `inventoryService.updateInventory()`. Si ocurre un error durante este proceso, captura el error, lo registra y devuelve un observable vacío para evitar una mayor propagación. Esto garantiza que las actualizaciones de inventario se gestionen de manera transaccional, permitiendo reintentos o acciones compensatorias en caso de fallos.

##### ¿Por qué es importante?

Las sagas ayudan a garantizar la consistencia entre servicios, especialmente en escenarios de fallo. Esto evita discordancias en los datos y asegura que tus microservicios sean resilientes.

Siguiendo estas mejores prácticas, puedes asegurarte de que tus microservicios estén bien probados, sean escalables y resilientes ante los desafíos comunes de los sistemas distribuidos. Las pruebas y la depuración en microservicios requieren un enfoque proactivo, pero con las herramientas y técnicas adecuadas, puedes construir y escalar con confianza tus servicios con NestJS.

---

### Resumen

En este capítulo, exploramos las mejores prácticas para construir microservicios escalables con NestJS, centrándonos en técnicas efectivas de prueba y depuración.

Comenzamos identificando problemas comunes que surgen en la comunicación de microservicios, como fallos de conexión, emisiones de eventos no confiables y datos inconsistentes. Luego repasamos soluciones prácticas para depurar estos problemas utilizando herramientas como variables de entorno, mecanismos de reintento y colas DLQ, garantizando una comunicación entre servicios más robusta y resiliente.

A continuación, analizamos la importancia de garantizar la consistencia de los datos en todos los servicios distribuidos, presentando patrones como las sagas y el outbox transaccional para mantener la consistencia ante fallos. Estas técnicas son esenciales para manejar la comunicación entre servicios en aplicaciones del mundo real.

También abordamos los desafíos de los cuellos de botella en el rendimiento de los microservicios al demostrar formas de optimizar la comunicación entre servicios mediante el procesamiento por lotes (*batching*), el procesamiento asíncrono y las colas de mensajes para mejorar el rendimiento bajo escenarios de alto tráfico.

Posteriormente, exploramos técnicas avanzadas de prueba para microservicios, proporcionando ejemplos del mundo real de pruebas de integración, simulación de servicios y aseguramiento de que los servicios interactúen como se espera. Al integrar herramientas como NestJS DevTools e implementar sofisticados mecanismos de reintento, desarrollamos una arquitectura de microservicios escalable y comprobable.

Por último, descubrimos algunas de las mejores prácticas a la hora de depurar una aplicación de microservicios, las cuales te ayudarán más adelante a depurar con confianza en tu viaje con NestJS.

En los tres próximos capítulos, nos sumergiremos en estudios de caso y trabajaremos en proyectos de la vida real, lo que te permitirá aplicar estos principios en escenarios prácticos. Comenzando con una API REST de la vida real en el próximo capítulo, estos proyectos te ayudarán a consolidar tu comprensión y te equiparán con las habilidades para manejar arquitecturas complejas de microservicios en entornos de producción. ¡Prepárate para dar vida a tu experiencia con NestJS!
