# Parte 1: Introducción a NestJS y Arquitectura de Aplicaciones Escalables

## Capítulo 2: Comprensión de los Principios de Arquitectura de Aplicaciones Escalables y Patrones de Diseño

Bienvenido al Capítulo 2, donde profundizaremos en los principios arquitectónicos y los patrones de diseño que te permitirán construir aplicaciones escalables con NestJS. NestJS es un framework increíblemente potente, pero manejar ese poder de manera efectiva requiere una comprensión sólida de conceptos clave como el almacenamiento en caché (*caching*), los patrones de diseño y la programación asíncrona. Si alguna vez te has preguntado cómo gestionar elegantemente el estado global, optimizar el rendimiento o escribir código que escale con fluidez, este capítulo será el lugar para obtener tus respuestas.

Al final de este capítulo, tendrás una comprensión teórica de varias estrategias de almacenamiento en caché para impulsar el rendimiento de tu aplicación. También te volverás competente en el uso de patrones de diseño que contribuyen a la escalabilidad y mantenibilidad de tu aplicación. Sin embargo, no se detiene ahí; aprenderás por qué NestJS prefiere ciertos patrones sobre otros y cómo aplicar la programación asíncrona de manera efectiva para aplicaciones a gran escala. Estas lecciones te equiparán con las habilidades y el conocimiento para construir aplicaciones que no solo funcionen bien, sino que también resistan la prueba de la escalabilidad.

En este capítulo, vamos a cubrir los siguientes temas principales:

- Principios de aplicaciones escalables
- Estrategias de almacenamiento en caché para la optimización del rendimiento
- Patrones de diseño para construir aplicaciones NestJS escalables
- Comprensión de la programación asíncrona para la escalabilidad
- Mejores prácticas para construir aplicaciones escalables

Navegar por estos temas te proporcionará un enfoque holístico de la arquitectura de aplicaciones, asegurando que estés bien equipado para afrontar los desafíos de construir aplicaciones escalables de nivel empresarial con NestJS.

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Principios de Arquitectura de Aplicaciones Escalables

¿Alguna vez has soñado con que tu aplicación se vuelva tan popular que sea la comidilla del mundo tecnológico? Todos lo hemos hecho, pero los sueños por sí solos no escalan. Tu arquitectura sí lo hace. Entonces, ¿qué es esta cosa mágica llamada "arquitectura escalable"? En términos técnicos, la escalabilidad es la capacidad de un sistema para manejar una cantidad creciente de trabajo, o su potencial para adaptarse al crecimiento.

#### Tu aplicación como una ciudad en crecimiento

Imagínate esto: tu aplicación es una ciudad pequeña y acogedora. Tiene servicios básicos: una tienda de comestibles, un parque y algunas casas. Ahora, ¿qué pasaría si esta ciudad de repente tuviera que albergar una afluencia de recién llegados? Se te ofrecen dos opciones:

- **Opción A:** Te apresuras a construir más casas, pero están por todas partes. Ahora la ciudad es un laberinto desordenado.
- **Opción B:** Tenías un plano bien planificado desde el principio. Cada nueva adición encaja perfectamente en su lugar designado. *Voilà*: ¡tu pequeña ciudad se transforma con gracia en una metrópolis bulliciosa!

La arquitectura escalable es como ese plano bien planificado para tu aplicación. Garantiza que a medida que tu aplicación gana terreno, no se convierta en un caos desordenado. En su lugar, escala sin problemas, como una ciudad bien planificada:

*Figura 2.1: ¿En qué ciudad preferirías vivir? La arquitectura de tu aplicación funciona de la misma manera*

#### Las tres S de la escalabilidad

Profundicemos un poco más en las tres "S" de la escalabilidad (*Speed, Storage, Simplicity*):

- **Velocidad (*Speed*):** ¿Puede tu aplicación manejar más peticiones por minuto a medida que crece la base de usuarios? ¡A nadie le gusta una aplicación lenta!
- **Almacenamiento (*Storage*):** ¿Puede almacenar más datos sin asfixiarse? Más usuarios significan más datos.
- **Simplicidad (*Simplicity*):** ¿Puedes añadir fácilmente nuevas características sin romper las existentes? ¡Tus usuarios siempre querrán algo nuevo y brillante!

#### La importancia del diseño modular: un arma de doble filo

Cuando escuchas "diseño modular", podrías pensar: "¡Ah, sí, dividir mi aplicación en partes más pequeñas y manejables, fácil!". Bueno, espera un poco. Si bien la modularización es generalmente una buena idea, también puede ser un arma de doble filo. Exploremos los pros y los contras.

#### ¿Por qué optar por lo modular?

En primer lugar, ¿por qué molestarse con los módulos? He aquí por qué: le dan estructura a tu código, haciéndolo más fácil de gestionar. Los módulos separan lógicamente tu base de código, facilitando las pruebas, la depuración y la extensión. Piensa en los módulos como departamentos en una empresa; cada uno tiene su propio conjunto de responsabilidades.

**Fragmento de código 1:**

```typescript
// In a NestJS app, creating a module for handling user // authentication @Module({ imports: [], controllers: [AuthController], providers: [AuthService], }) export class AuthModule {}
```

En el fragmento de código anterior, tenemos una clase de módulo simple llamada `AuthModule`, y estamos utilizando el decorador `@Module` para convertirla en una clase de NestJS. En la esencia del pensamiento modular, esta solo gestionará la lógica de autenticación, y la parte de implementación dependerá de la lógica de negocio.

#### Los riesgos: cuando los módulos se descontrolan

Aquí está el dilema: tener demasiados módulos puede generar una serie de problemas. Imagina que una empresa tuviera un departamento para cada pequeña tarea; ¡se desataría el caos! De manera similar, una modularización excesiva puede hacer que tu código sea más difícil de seguir y depurar, e incluso puede afectar el rendimiento. La Figura 2.2 muestra cómo debemos decidir si crear un módulo o no:

*Figura 2.2: A medida que tu aplicación escala, decidir cuándo optar por la modularidad o no puede ayudar a tu equipo a mantener el proyecto fácilmente*

#### El proceso de toma de decisiones: modularizar o no modularizar

Entonces, ¿cómo decides cuándo crear un módulo? Hazte las siguientes preguntas:

- **¿Es la funcionalidad independiente?** —¿Puede funcionar independientemente de otras características?
- **¿Es compleja?** —¿Involucra múltiples componentes, proveedores, etc.?
- **¿Se reutilizará?** —¿Es esta una funcionalidad que necesitarás en otras partes de tu aplicación o en proyectos futuros?
- **¿Es una responsabilidad separada?** —¿Representa un área única de responsabilidad dentro de tu aplicación?

Si respondes "sí" a la mayoría de estas preguntas, es un buen indicador de que un módulo separado podría ser beneficioso.

**Fragmento de código 2:**

```typescript
// Example of when NOT to create a new module in NestJS @Injectable() export class LoggingService { log(message: string): void { console.log(message); } }
```

En el fragmento de código anterior, tenemos una clase simple `LoggingService` que solo imprime en la consola. Este servicio es simple y probablemente se usará en varios módulos, por lo que podría no necesitar su propio módulo.

> **Ejercicio rápido**  
> Revisa algunas de tus aplicaciones Node.js recientes o el último programa en el que trabajaste. Identifica una parte del código que creas que debería haber sido un módulo separado y otra parte que no.

#### Sin estado (*Statelessness*): la piedra angular del escalado horizontal

Aquí hay una pregunta rápida: ¿recuerdas lo que comiste en el almuerzo hace tres miércoles? ¿No? Está bien; ¡tu servidor tampoco debería recordar cosas como esa! En el mundo de la tecnología, a menudo se aprecia una memoria corta, especialmente cuando se trata de servidores.

#### ¿Por qué ser sin estado (*stateless*)?

Imaginemos que tu servidor es como un *food truck* súper eficiente. Sirve pedidos sabrosos (datos) a personas (clientes) durante todo el día. Sin embargo, imagina si el camión comenzara a recordar los pedidos anteriores de cada persona, sus alergias y si era del tipo de persona que pide "sin pepinillos". Es mucho con lo que hacer malabares, ¿verdad?

Ser sin estado significa que a tu *food truck* —es decir, a tu servidor— no le importa quién eres ni lo que pediste la última vez. Pides una hamburguesa, te da una hamburguesa, fin de la transacción. Es este olvido lo que hace que sea fácil replicar este *food truck* por toda la ciudad (escalado horizontal) sin preocuparse por sincronizar a quién le gusta qué.

**Fragmento de código 3:**

```typescript
// A simple example to bring home the point. import { Controller, Get } from '@nestjs/common'; @Controller('stateless') export class StatelessController { @Get() statelessEndpoint(): string { return 'Hello, anonymous human! Enjoy your stateless interaction.'; } }
```

En el fragmento de código anterior, implementamos un controlador simple sin estado. Dado que este controlador no se preocupa por el emisor de la petición, solo devuelve una cadena de texto, sin importar quién hizo la petición, siendo este un ejemplo de diseño sin estado.

#### Escalabilidad de la base de datos: *sharding*, replicación y particionamiento

Las bases de datos son como los magos detrás de la cortina. Sin embargo, incluso los magos necesitan escalar su magia a veces, a través de técnicas como el *sharding* (fragmentación), la replicación y el particionamiento, que se analizarán más adelante en el libro.

#### Microservicios y escalabilidad

¿Alguna vez has oído el dicho "divide y vencerás"? Ese es el espíritu detrás de los microservicios. Cada microservicio tiene su propio rol y se puede escalar de forma independiente. Tenemos un capítulo dedicado especialmente a los microservicios, y llegaremos a discutir esta útil arquitectura que nos permite escalar aplicaciones más adelante, en la Parte 4 del libro.

#### La red social de los microservicios: arquitectura orientada a eventos (*Event-Driven Architecture*)

Imagina un concurrido cóctel, donde todos se mezclan y tienen excelentes conversaciones. Ahora imagina si, en lugar de acercarte a hablar con alguien directamente, tuvieras que anunciar tus noticias en voz alta para que todos las escucharan (incómodo, ¿verdad?). Esa es la esencia de la arquitectura orientada a eventos.

#### ¿Por qué eventos en lugar de llamadas directas?

En configuraciones tradicionales, un servicio puede realizar una llamada de API directa a otro servicio para hacer algo, un poco como acercarse y tocar a alguien en el hombro para hablar. Sin embargo, ¿qué sucede si esa persona ya está inmersa en una conversación profunda? No pueden realizar múltiples tareas y tu mensaje importante se retrasa.

Ahora imagina en cambio que simplemente anuncias: "¡Oigan, tengo pizza gratis aquí!". Acabas de emitir un "evento". Cualquiera interesado en la pizza (y seamos sinceros, ¿quién no?) se acercará a ti. No necesitas ir de persona en persona; reaccionan al evento que emitiste.

#### Desglosándolo: eventos y escuchadores (*events and listeners*)

En una arquitectura orientada a eventos, una parte de tu aplicación desencadena o emite un "evento" y otras partes lo escuchan. Cuando lo escuchan, ejecutan algún código, como un grupo de entusiastas de la pizza acudiendo a la mesa de aperitivos.

**Fragmento de código 4:**

```typescript
// Let's see this social event in action with this NestJS // code snippet import { Injectable } from '@nestjs/common'; import { EventEmitter2 } from '@nestjs/event-emitter'; @Injectable() export class EventsService { constructor(private eventEmitter: EventEmitter2) {} triggerEvent(): void { this.eventEmitter.emit('user.created', { /* payload */ }); } }
```

En el fragmento de código anterior, estamos utilizando la clase `EventEmitter` de `@nestjs/event-emitter` para crear un evento simple. Este evento luego será procesado por un escuchador (*listener*) para su posterior procesamiento. En el capítulo dedicado a microservicios, aprenderemos cómo procesar estos eventos y asegurarnos de que los datos se mantengan consistentes, sin importar el orden de los eventos.

La Figura 2.3 muestra una ilustración de una arquitectura orientada a eventos donde un evento puede desencadenar una serie de eventos y asegurarse de que el usuario reciba la actualización correcta sobre el flujo de los eventos mientras se procesan:

*Figura 2.3: Ilustración de una arquitectura orientada a eventos (derechos de autor: [https://www.confluent.io/blog/journey-to-event-driven-part-1-why-event-first-thinking-changes-everything/](https://www.confluent.io/blog/journey-to-event-driven-part-1-why-event-first-thinking-changes-everyt))*

Con una arquitectura orientada a eventos, puedes crear un sistema más dinámico, receptivo y escalable, un poco como asegurarse de que todos en tu fiesta estén bien alimentados y entretenidos sin tener que consultar a cada persona individualmente.

Toma un respiro profundo. Hemos cubierto mucho terreno. Estos principios no son solo teóricos; hemos compartido un par de fragmentos de código para ilustración y profundizaremos en ejercicios más prácticos más adelante en este libro.

En esta sección, hemos descubierto los principios de la arquitectura escalable, incluido lo que significa diseñar una arquitectura orientada a eventos y su importancia. En la siguiente sección, veremos cómo gestionar el crecimiento de la aplicación y cómo lidiar con procesos altamente demandantes mediante un mecanismo de balanceo de carga.

#### Balanceo de carga y escalado horizontal

¿Alguna vez has estado en un concierto o en un gran evento deportivo? Piensa en esos agentes de tránsito o personal del evento, dirigiendo eficientemente a una multitud de fanáticos entusiasmados a sus respectivos asientos o salidas sin causar caos. ¡En nuestra arena digital, ese es el papel de un balanceador de carga (*load balancer*)!

#### ¿Por qué tanto revuelo con los balanceadores de carga?

Imagina que tu aplicación está organizando un gran concierto virtual. Miles de usuarios (fanáticos) se apresuran a entrar para obtener la mejor experiencia. Si todos fueran a la misma entrada (servidor), las cosas se volverían caóticas rápidamente, ¿verdad?

Un balanceador de carga actúa como ese personal de eventos súper eficiente, asegurando que los fanáticos (usuarios) se distribuyan en múltiples entradas (servidores) para que todos obtengan una experiencia fluida. Ya sean 100 o 100,000 fanáticos, ¡el balanceador de carga garantiza que todos tengan una noche memorable sin colapsar la fiesta!

#### El balanceo es un arte y una ciencia

No se trata solo de distribuir uniformemente la carga. Un buen balanceador de carga es inteligente. Sabe qué entradas del servidor están menos concurridas, cuáles están más cerca de los fanáticos y, a veces, incluso qué entradas ofrecen el acceso más rápido a los bocadillos (datos importantes).

#### ¿Escalado horizontal?

¿Captaste ese nuevo término? El "escalado horizontal" es como clonar soldados para los servidores. En lugar de hacer que un solo servidor sea súper robusto y potente (eso es escalado vertical), lo clonas. Ahora tienes múltiples servidores, todos igualmente capaces de manejar peticiones sin tener que conocer los asuntos de los demás. ¡Magia!

> **Ejercicio rápido**  
> ¡Muy bien, momento de una prueba sorpresa! 📝 Imagina tu experiencia de concierto perfecta. ¿Qué tres cualidades desearías del personal del evento que dirige el tráfico? Anótalas. Ahora, traduce esas cualidades a un balanceador de carga. ¿Dimos en la nota correcta?

Ahora que hemos descubierto qué es el balanceo de carga y su importancia en una arquitectura escalable, hablemos de algo que a menudo se subestima: las estrategias de almacenamiento en caché. ¿Por qué alguien necesitaría esto para mejorar el rendimiento de su aplicación? La respuesta está en la siguiente sección.

---

### Estrategias de almacenamiento en caché para la optimización del rendimiento

¿Alguna vez entraste en una habitación y olvidaste por qué estás allí, solo para regresar y recordarlo al instante? Es frustrante hacer lo mismo dos veces sin ninguna razón, ¿verdad? Las computadoras también pueden sentirse así (metafóricamente hablando). El almacenamiento en caché es como la nota en el bloc de notas que te ayuda a recordar por qué entraste en esa habitación.

En términos técnicos, el almacenamiento en caché (*caching*) es el proceso de almacenar copias de datos a los que se accede con frecuencia en una "caché" para que las peticiones futuras puedan atenderse más rápidamente. Con el almacenamiento en caché, esencialmente ahorras tiempo y recursos, haciendo que tu aplicación sea más rápida y eficiente.

#### Tipos de almacenamiento en caché: conociendo tus herramientas

Al igual que tienes diferentes tipos de blocs de notas (notas adhesivas, notas digitales, cuadernos de bolsillo), también existen diferentes tipos de cachés. Exploremos algunos de ellos:

- **Caché en memoria (*In-memory caching*):** Como notas adhesivas en la pantalla de tu computadora, es rápida de escribir y leer, pero desaparece cuando la apagas.
- **Caché de base de datos (*Database caching*):** Más como un cuaderno de bolsillo, este tipo de caché se almacena en la base de datos y es un poco más permanente.
- **Caché de contenido (*Content caching*):** Piensa en esto como tomar una captura de pantalla de una página web para verla rápidamente más tarde sin recargar todo.

#### El ABC de las estrategias de almacenamiento en caché

Recordar todo no es útil; quieres recordar las cosas correctas. Aquí está el ABC de las diferentes estrategias de almacenamiento en caché:

- **Menos utilizado recientemente (*Least Recently Used* - LRU):** Si tu bloc de notas está lleno, borrarás la nota que no hayas mirado durante más tiempo.
- **Tiempo de vida (*Time to Live* - TTL):** Notas adhesivas con fechas de caducidad. ¡Después de un período establecido, se autodestruyen!
- **Invalidación de caché (*Cache invalidation*):** A veces, tus notas quedan desactualizadas o incorrectas. Necesitarás reemplazarlas con información nueva y precisa.

#### Cuando el almacenamiento en caché sale mal: dificultades y cómo evitarlas

Imagina escribir tu lista de compras y luego olvidarte de actualizarla cuando otra persona compra la leche. ¡Terminas con demasiada leche y sin café! El almacenamiento en caché no es infalible. Aquí hay algunas dificultades:

- **Obsolescencia de la caché (*Cache staleness*):** Cuando la caché está desactualizada y sirve datos viejos.
- **Saturación/sacudida de la caché (*Cache thrashing*):** Cuando los datos se expulsan continuamente antes de usarse, lo que básicamente frustra el propósito del almacenamiento en caché.
- **Complejidad de la caché (*Cache complexity*):** Cuando te excedes y terminas con una infraestructura de caché complicada que es difícil de gestionar.

> **Ejercicio rápido: momento de reflexionar sobre tu caché**  
> Tómate un momento para considerar la última aplicación que construiste. ¿Puedes identificar lugares donde la implementación de una caché habría mejorado el rendimiento? ¿Qué tal lugares donde el almacenamiento en caché podría introducir problemas? Anota estos puntos.

#### El almacenamiento en caché en el universo de NestJS

Dado que este libro tiene un enfoque especial en NestJS, veamos un fragmento de código rápido para implementar un almacenamiento en caché simple en una aplicación NestJS.

**Fragmento de código 5:**

```typescript
// Caching a response in a NestJS service import { Injectable, CACHE_MANAGER, Inject, CacheStore } from '@nestjs/common'; @Injectable() export class AppService { constructor( @Inject(CACHE_MANAGER) private cacheManager: CacheStore ) {} async cacheThis(): Promise<string> { let value = await this.cacheManager.get('my-key'); if (value) { return `From cache: ${value}`; } value = 'some expensive operation result'; await this.cacheManager.set( 'my-key', value, { ttl: 600 } ); return `Processed: ${value}`; } }
```

En el fragmento de código anterior, almacenamos en caché una operación costosa en un servicio dentro de una aplicación NestJS para mejorar el rendimiento al reducir la necesidad de ejecutar repetidamente un proceso costoso. Aquí está el desglose del resto del código:

- **Sentencias de importación:** El código importa `Injectable`, `CACHE_MANAGER`, `Inject` y `CacheStore` de `@nestjs/common`. Estos son proporcionados por NestJS para facilitar la inyección de dependencias y la funcionalidad de almacenamiento en caché.
- **Decoración del servicio:** El decorador `@Injectable()` marca la clase `AppService` como un proveedor que puede ser gestionado por el sistema de inyección de dependencias de NestJS. Esto permite inyectarlo en controladores u otros servicios.
- **Clase de servicio:** `export class AppService` define una nueva clase de servicio llamada `AppService` que contendrá lógica que se puede compartir en toda la aplicación.
- **Constructor e inyección de dependencias:** El constructor de la clase `AppService` utiliza el decorador `@Inject` para inyectar el administrador de caché. El token `CACHE_MANAGER` se utiliza para indicarle al sistema de inyección de dependencias de NestJS que proporcione una instancia de `CacheStore`.
- **Método de operación de caché:** `async cacheThis(): Promise<string>`: Este método es una función asíncrona que promete devolver una cadena una vez que se complete la operación.
  - Dentro del método, primero intenta recuperar un valor de la caché utilizando la clave `my-key` a través de `this.cacheManager.get('my-key')`.
  - Si se encuentra un valor (`if (value)`), significa que el resultado de la operación costosa ya está en caché. Luego, el método devuelve ese valor con el prefijo `From cache:`, lo que indica que la respuesta se entregó desde la caché.
- **Lógica de almacenamiento en caché:** Si la caché no tiene un valor para `my-key`, el código procede a simular una operación costosa asignando `'some expensive operation result'` al valor.
- **Devolución del valor procesado:** Después de almacenar en caché el valor, el método devuelve una cadena con el prefijo `Processed:`, indicando que la operación tuvo que ser procesada y no fue entregada desde la caché.

Esta estrategia de almacenamiento en caché puede mejorar enormemente el rendimiento de tu aplicación al evitar la necesidad de realizar la operación costosa cada vez que se llama al método `cacheThis`. En su lugar, entrega el resultado precalculado de la caché siempre que esté disponible, ahorrando así tiempo y recursos.

Hemos cubierto el qué, el por qué y el cómo del almacenamiento en caché, e incluso lo hemos visto en acción con algo de código de NestJS. Comprender el almacenamiento en caché es crucial para construir sistemas escalables, lo que se relaciona con nuestra discusión sobre los principios de arquitectura escalable y balanceo de carga.

A continuación, nos sumergiremos en los patrones de diseño que blindarán aún más tus aplicaciones NestJS para escalar.

---

### Patrones de diseño para la construcción de aplicaciones NestJS escalables

Los patrones de diseño, en el ámbito del desarrollo de software, son soluciones comprobadas para problemas comunes. Son como planos arquitectónicos para construir software. Cuando se trata del framework NestJS, ciertos patrones de diseño emergen como particularmente efectivos para garantizar la escalabilidad y la mantenibilidad. Profundicemos en estos patrones y en el razonamiento detrás de las elecciones arquitectónicas de NestJS.

#### Por qué importan los patrones de diseño

Imagina construir un edificio sin un plano. No solo el proceso sería caótico, sino que también el resultado podría ser inestable. De manera similar, descuidar los patrones de diseño en el software puede conducir a código desordenado, sistemas no escalables y una serie de problemas imprevistos.

En un framework como NestJS, diseñado para la escalabilidad y la flexibilidad, seguir patrones de diseño reconocidos no es solo una recomendación: es una piedra angular de su filosofía de diseño.

#### El patrón Singleton para gestionar el estado global

En esencia, el patrón Singleton garantiza que una clase tenga solo una instancia y proporciona un punto de acceso a esa instancia desde cualquier otra clase.

#### Por qué NestJS prefiere el patrón Singleton

NestJS utiliza intensamente el patrón Singleton, particularmente para los servicios. ¿Por qué? Porque los servicios a menudo gestionan tareas como conexiones a bases de datos, almacenamiento en caché y configuración de datos, operaciones con las que no deseas que interfieran múltiples instancias.

#### Un vistazo rápido al patrón Singleton en NestJS

**Fragmento de código 6:**

```typescript
@Injectable() export class MyService { // This service will be instantiated once and shared // across modules }
```

Con `@Injectable()`, NestJS garantiza que el servicio permanezca como un Singleton en todos los módulos, promoviendo la consistencia y reduciendo posibles conflictos de datos.

#### El patrón Factory para la creación de objetos

Este patrón aborda el problema de crear objetos sin especificar la clase exacta del objeto que se creará.

#### Por qué NestJS utiliza el patrón Factory

En NestJS, los proveedores no se limitan únicamente a servicios. Pueden ser cualquier cosa: objetos, cadenas de texto, números, etc. Para ofrecer esta flexibilidad sin obligar al desarrollador a codificar de forma rígida la creación de objetos, NestJS emplea el patrón Factory.

**Fragmento de código 7:**

```typescript
{ provide: 'MY_FACTORY_TOKEN', useFactory: (connection: Connection) => { return new MyCustomClass(connection); }, inject: [DbConnectionToken], }
```

Aquí, `useFactory` es un ejemplo del patrón Factory en acción, permitiendo la creación dinámica de objetos basada en dependencias.

#### Inyección de dependencias para el bajo acoplamiento

La inyección de dependencias (*Dependency Injection* - DI) es una técnica mediante la cual un objeto suministra las dependencias de otro objeto, en lugar de hacer que se construyan internamente. Es fundamental para lograr la inversión de control y promueve el bajo acoplamiento.

#### Por qué NestJS confía ciegamente en la DI

Todo el ecosistema de NestJS gira en torno a la DI. Al adoptar la DI, NestJS garantiza que los sistemas sean modulares, comprobables y escalables. Los componentes, ya sean servicios o controladores, no están estrechamente vinculados sino interconectados a través de un sistema bien definido.

#### Presenciando la DI en NestJS

El siguiente código es un ejemplo de DI comúnmente utilizada en aplicaciones NestJS:

```typescript
constructor(private myService: MyService) {}
```

Aquí, en lugar de crear una instancia de `MyService` dentro del componente, NestJS la inyecta, asegurando que el componente no dependa directamente de `MyService`.

La Figura 2.4 muestra una ilustración de módulos débilmente acoplados trabajando juntos para lograr una tarea determinada:

*Figura 2.4: Módulos débilmente acoplados trabajando en sincronía*

#### El patrón Decorator para extender la funcionalidad en NestJS

En el ámbito de los patrones de diseño, el patrón Decorator tiene un propósito único: permite a los desarrolladores agregar nuevas funcionalidades a los objetos sin alterar su estructura. Este tipo de patrón de diseño se clasifica dentro de los patrones estructurales, ya que actúa como un contenedor (*wrapper*) para las clases existentes.

#### Por qué NestJS se basa en el patrón Decorator

NestJS utiliza decoradores para recopilar metadatos sobre tus clases, funciones y propiedades. Estos metadatos se pueden utilizar en tiempo de ejecución para realizar una variedad de tareas, ya sea configurar una ruta HTTP, inyectar dependencias o establecer una puerta de enlace de WebSockets. La reflexión de metadatos es una piedra angular de NestJS y es lo que impulsa la extensibilidad del framework.

#### Modularidad y código limpio

Al usar decoradores, NestJS permite a los desarrolladores escribir código más limpio y modular. Los decoradores abstraen gran parte del código repetitivo, lo que permite a los desarrolladores concentrarse en escribir la lógica de negocio. Esto mejora la legibilidad y la mantenibilidad, haciendo que la base de código sea más manejable a medida que escala.

#### Funcionalidad mejorada con intrusión mínima

Los decoradores en NestJS agregan capas de funcionalidad a tu código sin requerir que alteres la lógica central. Por ejemplo, el decorador `@Roles()` puede agregar rápidamente control de acceso basado en roles a un método sin requerir que modifiques la implementación del método. La naturaleza no intrusiva de los decoradores los hace ideales para aplicaciones escalables.

#### Sistema fuertemente tipado

El uso de decoradores a menudo va de la mano con la verificación de tipos de TypeScript, mejorando la seguridad de tipos en tiempo de desarrollo. Por ejemplo, el decorador `@Body()` no solo extrae propiedades del cuerpo de la petición, sino que también puede imponer restricciones de tipos si se utiliza en conjunto con clases de Objetos de Transferencia de Datos (*Data Transfer Object* - DTO).

#### Los decoradores de NestJS en acción

En una API RESTful, los decoradores `@Controller` y `@Get`/`@Post`/`@Put`/`@Delete` configuran rutas, transformando clases y métodos en manejadores de peticiones casi por arte de magia.

**Fragmento de código 8:**

```typescript
@Controller('cats') export class CatsController { @Get() findAll() { return 'Returns all cats'; } }
```

Este fragmento de código simple muestra un ejemplo de un controlador que maneja una petición GET; gracias al decorador `@Get`, no hay necesidad de especificar qué metadatos deben formar parte de la petición, ya que la función ya lo sabe.

#### Extracción de la carga útil de la petición (*Request payload extraction*)

Los decoradores de petición como `@Query`, `@Param` y `@Body` agilizan el proceso de extracción de parámetros, cadenas de consulta y cuerpos de peticiones, haciendo que los métodos del controlador sean más limpios y enfocados.

**Fragmento de código 9:**

```typescript
@Get('find') findCat(@Query('name') name: string) { return `Returns cat with name ${name}`; }
```

#### Decoradores personalizados

NestJS también permite la creación de decoradores personalizados, ofreciendo infinitas opciones de extensibilidad. Por ejemplo, podrías crear un decorador `@Sanitize` que limpie automáticamente los datos entrantes para evitar ataques de inyección de SQL (*Structured Query Language*).

**Fragmento de código 10:**

```typescript
export const Sanitize = createParamDecorator( (data, req) => { return sanitize(req.body); // fictional sanitize // function }, );
```

#### Inyección de dependencias

Con `@Injectable`, `@Inject` y los decoradores de proveedores personalizados, NestJS convierte la DI en un proceso elegante y sin complicaciones.

**Fragmento de código 11:**

```typescript
@Injectable() export class CatsService { constructor( @Inject('DATABASE_CONNECTION') private dbConnection: Connection ) {} }
```

#### NestJS sin decoradores frente a con decoradores

Aquí observaremos cómo se verían las cosas en un mundo hipotético de NestJS sin decoradores y lo compararemos con cómo los decoradores simplifican el proceso de desarrollo.

#### Enrutamiento y controladores

En un mundo sin decoradores, configurar una ruta simple podría implicar una configuración menos elegante, utilizando llamadas directas a métodos y mapeo manual de rutas.

**Fragmento de código 12:**

```typescript
export class CatsController { constructor( private readonly expressApp: Express.Application ) { this.expressApp.get('/cats', this.findAll.bind(this)); } findAll(req: Request, res: Response) { res.send('Returns all cats'); } }
```

Usando decoradores, el proceso se simplifica, mejorando la legibilidad del código, como se muestra en el fragmento de código 13 a continuación:

**Fragmento de código 13:**

```typescript
@Controller('cats') export class CatsController { @Get() findAll() { return 'Returns all cats'; }}
```

#### Extracción de la carga útil de la petición

Sin decoradores de petición, tendrías que extraer manualmente los parámetros de la petición, lo que puede resultar engorroso.

**Fragmento de código 14:**

```typescript
export class CatsController { constructor( private readonly expressApp: Express.Application ) { this.expressApp.get('/find', this.findCat.bind(this)); } findCat(req: Request, res: Response) { const name = req.query.name; res.send(`Returns cat with name ${name}`); } }
```

Ahora con un decorador como `@Query`, simplificamos el proceso de extracción de parámetros, como se muestra en el fragmento de código 15 a continuación:

**Fragmento de código 15:**

```typescript
@Get('find') findCat(@Query('name') name: string) { return `Returns cat with name ${name}`; }
```

#### Inyección de dependencias

Gestionar las dependencias manualmente sin decoradores podría verse engorroso y requeriría código de inicialización adicional.

**Fragmento de código 16:**

```typescript
export class CatsService { private dbConnection: Connection; constructor() { // some singleton pattern this.dbConnection = DatabaseConnection.getInstance(); } }
```

#### Con decoradores

Al usar `@Injectable` y `@Inject`, la DI se vuelve elegante y sin fricciones.

**Fragmento de código 17:**

```typescript
@Injectable() export class CatsService { constructor( @Inject('DATABASE_CONNECTION') private dbConnection: Connection ) {} }
```

#### Por qué los decoradores hacen que NestJS sea sobresaliente

El uso extensivo de decoradores en NestJS no es una simple elección de diseño, sino una decisión calculada para facilitar el desarrollo en varios dominios, ya sean aplicaciones web, móviles o de escritorio. Los decoradores ofrecen una combinación armoniosa de elegancia, funcionalidad y extensibilidad, lo que convierte a NestJS en un framework robusto para crear aplicaciones escalables, mantenibles y de nivel empresarial.

Al utilizar el patrón Decorator, NestJS permite a los desarrolladores concentrarse en lo que realmente importa: resolver problemas complejos sin preocuparse por la fontanería interna. Proporciona una capa de abstracción que enriquece el desarrollo, desde la configuración inicial hasta la implementación de funcionalidades a gran escala.

En conclusión, los patrones de diseño son la columna vertebral de cualquier aplicación bien estructurada, y NestJS defiende esta filosofía. Al comprender y aprovechar estos patrones, los desarrolladores pueden aprovechar el verdadero poder de NestJS, garantizando aplicaciones escalables, mantenibles y eficientes.

En nuestras próximas secciones, profundizaremos en la programación asíncrona, que es otra piedra angular de la construcción de sistemas escalables, especialmente en un entorno de Node.js como NestJS.

---

### Comprensión de la programación asíncrona para la escalabilidad

La programación asíncrona puede sonar como un término complejo sacado directamente de un libro de texto de ciencias de la computación, pero entenderla es crucial para construir aplicaciones escalables, particularmente en un framework basado en JavaScript como NestJS. Como dice el famoso refrán: "La escalabilidad es al rendimiento lo que ágil es al desarrollo". Desmitifiquemos este concepto y analicemos por qué adoptar la programación asíncrona puede cambiar las reglas del juego para tus aplicaciones NestJS.

#### ¿Qué es la programación asíncrona?

En los términos más simples, la programación asíncrona permite que múltiples operaciones se ejecuten simultáneamente, pero no necesariamente en el mismo instante. En un modelo síncrono, las operaciones se realizan una tras otra, bloqueando las acciones posteriores hasta que se completa la actual. La programación asíncrona, por otro lado, permite descargar operaciones como tareas vinculadas a I/O, liberando el hilo principal para manejar otras tareas.

#### Por qué la asincronía es importante para la escalabilidad

En una aplicación del lado del servidor, no puedes permitirte tener recursos inactivos mientras esperas a que se completen consultas a la base de datos, llamadas a la API u otras operaciones vinculadas a I/O. La asincronía garantiza que tus recursos se utilicen de manera óptima.

#### Experiencia de usuario mejorada

A los usuarios no les gusta esperar. La programación asíncrona garantiza que tu aplicación siga respondiendo incluso cuando realiza tareas que toman tiempo. Esta capacidad de respuesta se traduce directamente en una mejor experiencia de usuario.

#### Más fácil de escalar

Las aplicaciones que hacen un uso efectivo de la programación asíncrona son generalmente más fáciles de escalar horizontalmente. Al utilizar eficientemente los recursos disponibles, puedes manejar más usuarios sin agregar más hardware.

#### Programación asíncrona en NestJS

NestJS, al estar construido sobre Node.js, admite de forma natural patrones asíncronos. Echemos un vistazo a cómo NestJS aprovecha este poder.

#### Uso de Promises y async/await

NestJS promueve el uso de Promises y la sintaxis `async/await` para manejar operaciones asíncronas de manera elegante. Por ejemplo, supongamos que tienes un servicio que obtiene datos de usuario de una base de datos. El fragmento de código 18 muestra cómo debería verse la implementación sin la sintaxis `async/await`.

**Fragmento de código 18:**

```typescript
getUserById(id: string) { return this.database .query(`SELECT * FROM users WHERE id = ${id}`) .then(user => user) .catch(err => { throw new Error(err); }); }
```

Ahora, utilizando la sintaxis `async/await`, obtenemos la versión que se muestra en el fragmento de código 19 a continuación:

**Fragmento de código 19:**

```typescript
async getUserById(id: string) { try { const user = await this.database.query( `SELECT * FROM users WHERE id = ${id}` ); return user; } catch (err) { throw new Error(err); } }
```

La versión con `async/await` no solo es más legible, sino también más fácil de razonar y depurar.

#### Bucle de eventos e I/O no bloqueante (*Event loop and non-blocking I/O*)

NestJS aprovecha la arquitectura orientada a eventos de Node.js, que es inherentemente no bloqueante y adecuada para aplicaciones escalables. El bucle de eventos maneja todas las tareas asíncronas y asegura una ejecución fluida.

#### Programación reactiva con RxJS

NestJS también se integra a la perfección con RxJS, una librería para programación reactiva que facilita la composición de código asíncrono o basado en retornos de llamada (*callbacks*). Esto se suma a las capacidades de construir servicios reactivos y escalables.

En conclusión, la programación asíncrona no es solo una característica; es un elemento fundamental para construir aplicaciones escalables. NestJS aprovecha la programación asíncrona en toda su extensión, proporcionando a los desarrolladores las herramientas necesarias para crear aplicaciones que sean tanto escalables como de alto rendimiento.

Comprender y emplear prácticas de programación asíncrona en NestJS te pondrá en el camino hacia la creación de aplicaciones que sean receptivas, eficientes y listas para crecer.

Así que, la próxima vez que te encuentres contemplando la escalabilidad de tu aplicación NestJS, recuerda que entender y utilizar la programación asíncrona es como equipar tu aplicación con un turbocompresor. Se trata de hacer más cosas, más rápido y de manera más eficiente.

En la siguiente sección, reuniremos todo y discutiremos las mejores prácticas para pensar en aplicaciones escalables.

---

### Mejores prácticas para construir aplicaciones escalables

A medida que nos acercamos al final de este capítulo, es hora de unirlo todo. Has explorado la arquitectura y los patrones de diseño, e incluso has profundizado en los detalles de la programación asíncrona en NestJS. Sin embargo, conocer todos estos componentes individuales es como tener todos los ingredientes para un plato gourmet: todavía necesitas la receta. Entonces, vayamos a las mejores prácticas prácticas que servirán como tu modelo para construir aplicaciones escalables en NestJS.

#### La organización del código es clave

Considera las siguientes buenas prácticas para una mejor organización del código:

- **Diseño modular:** Divide tu aplicación en módulos bien definidos. NestJS facilita esto con su sistema de módulos. Mantener juntas las funcionalidades relacionadas simplifica tanto el desarrollo como el escalado.
- **Código limpio:** La importancia de escribir código limpio y mantenible no se puede subestimar. Cíñete a las convenciones, sé consistente en los nombres y, sobre todo, haz que tu código sea autoexplicativo.

#### Optimización del almacenamiento y recuperación de datos

Al tratar con un conjunto de datos grande, considera incluir las siguientes características:

- **Indexación de bases de datos:** Asegúrate de que tus bases de datos estén indexadas adecuadamente. La indexación acelera el proceso de recuperación de datos, haciendo que tu aplicación sea más rápida y escalable.
- **Almacenamiento en caché:** Como se discutió anteriormente, el almacenamiento en caché es esencial para la optimización del rendimiento. Utiliza estrategias de almacenamiento en caché para servir datos a los que se accede con frecuencia, reduciendo significativamente la carga de la base de datos.

#### Adoptar microservicios para el desacoplamiento

La arquitectura de microservicios no es solo una frase de moda; es una mejor práctica para aplicaciones a gran escala. Al desacoplar varias partes de tu aplicación, los microservicios permiten que los componentes individuales escalen de forma independiente.

#### Automatización de pruebas y despliegue

Considera agregar las siguientes capacidades a tus futuras aplicaciones:

- **Integración continua:** Configurar una canalización de integración continua/entrega continua (CI/CD) automatiza los procesos de prueba y despliegue, asegurando que tu aplicación esté siempre en un estado desplegable.
- **Pruebas automatizadas:** Invierte en pruebas automatizadas. No solo mejora la calidad de tu código, sino que también garantiza que la escalabilidad no introduzca nuevos errores.

#### Hacer uso de middleware e interceptores

El middleware y los interceptores en NestJS actúan como los puntos de control perfectos para registrar, medir o modificar peticiones y respuestas, que a menudo son actividades cruciales al escalar una aplicación.

#### No descuides la seguridad

Entre varias consideraciones de seguridad, aquí están las más comunes a considerar:

- **Limitación de velocidad (*Rate limiting*):** Protege tu aplicación del abuso implementando una limitación de velocidad en tus APIs.
- **Validación y saneamiento (*Validation and sanitization*):** Siempre valida y sanea los datos. Una aplicación que es segura es más fácil de escalar, ya que no tienes que apagar incendios de seguridad constantemente.

#### Aprovechar el balanceo de carga

Como aprendiste anteriormente, el balanceo de carga distribuye eficazmente el tráfico de red entrante entre múltiples servidores. Esto garantiza una alta disponibilidad y confiabilidad, haciendo que tu aplicación sea inherentemente más escalable.

#### Uso de métricas de aplicación para obtener información

Considera las siguientes métricas para obtener información útil sobre el estado de tu aplicación:

- **Registro y monitoreo (*Logging and monitoring*):** Vigila las métricas de tu aplicación. Herramientas como Grafana y Prometheus pueden brindarte información crucial para escalar.
- **Métricas de rendimiento:** Los tiempos de respuesta, las tasas de error y otros indicadores clave de rendimiento (KPI) deben monitorearse para comprender qué tan bien está escalando tu aplicación.

---

### Resumen

La escalabilidad no es una característica aislada ni un elemento que simplemente se marca en una lista de verificación; es una interacción compleja de mejores prácticas, patrones de diseño y elecciones arquitectónicas. Estos elementos deben trabajar en armonía para crear una aplicación escalable.

Y ahí lo tienes: las prácticas fundamentales que transforman tu comprensión teórica de la escalabilidad en conocimientos prácticos en el contexto de NestJS. Ahora eres más que un simple seguidor de las tendencias de la industria; estás equipado con el conjunto de herramientas conceptuales que sienta las bases para construir aplicaciones verdaderamente escalables. Estas no son solo mejores prácticas; son tu Estrella Polar en el a veces brumoso panorama del desarrollo de aplicaciones.

Sin embargo, la comprensión es solo el primer paso; la implementación es la clave. ¿Estás listo para ensuciarte las manos con algo de código real? Prepárate, porque nuestro próximo capítulo te llevará de cero a héroe en la aplicación práctica de estas mejores prácticas. Configuraremos tu entorno de desarrollo y nos sumergiremos directamente en la construcción de una aplicación robusta y de alto rendimiento con NestJS.

Por lo tanto, mantén listo tu Entorno de Desarrollo Integrado (IDE) y tu mente despierta; estás a punto de traducir las mejores prácticas abstractas en acciones concretas. El próximo capítulo promete ser una inmersión práctica en el lado técnico de las cosas. Mantengamos el impulso y comencemos a construir esa maravilla de aplicación que ahora estás más que preparado para desarrollar.
