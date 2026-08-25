# Parte 1: Introducción a NestJS y Arquitectura de Aplicaciones Escalables

## Capítulo 1: Visión General de NestJS

A medida que el mundo del desarrollo del lado del servidor continúa evolucionando, la demanda de frameworks robustos, escalables y fáciles de mantener crece cada vez más. Aquí entra **NestJS**: un framework progresivo de Node.js que combina elementos de Programación Orientada a Objetos (*Object-Oriented Programming* - OOP), Programación Funcional (*Functional Programming* - FP) y Programación Reactiva Funcional (*Functional Reactive Programming* - FRP) para ofrecer una experiencia de desarrollo única. Al adoptar TypeScript y proporcionar una arquitectura altamente modular, NestJS abre la puerta a aplicaciones del lado del servidor limpias, eficientes y seguras.

En este capítulo, te sumergirás en el intrincado mundo de NestJS. Te guiaremos a través de los aspectos fundamentales, comenzando por qué es NestJS y los problemas que busca resolver. Aprenderás sobre su filosofía y patrones de diseño, su robusto ecosistema y cómo TypeScript amplifica la experiencia de desarrollo. Al final de este capítulo, habrás adquirido una comprensión integral de por qué NestJS se ha convertido en el favorito de los desarrolladores para crear aplicaciones del lado del servidor escalables y mantenibles.

En este capítulo, vamos a cubrir los siguientes temas principales:

- Introducción a NestJS
- El poder de TypeScript
- Características clave de NestJS
- El ecosistema de NestJS
- ¿Por qué elegir NestJS?

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Introducción a NestJS

En el vertiginoso mundo del desarrollo web, los desarrolladores buscan continuamente herramientas y frameworks que proporcionen eficiencia, estructura y escalabilidad. A medida que las aplicaciones se vuelven más complejas, un framework robusto y adaptable se vuelve vital. NestJS, un framework progresivo de Node.js, se destaca como una solución prominente para construir aplicaciones del lado del servidor que sean mantenibles y escalables.

#### Contexto histórico

El panorama del desarrollo backend ha experimentado cambios sísmicos desde los primeros días de la web. Comenzando desde simples scripts CGI hasta arquitecturas monolíticas y evolucionando hacia servicios RESTful y microservicios, cada fase ha traído su propio conjunto de desafíos y soluciones. NestJS surge como un framework diseñado para abordar las complejidades de las aplicaciones modernas, proporcionando una forma estructurada de construir aplicaciones del lado del servidor escalables, eficientes y robustas.

NestJS fue creado por Kamil Myśliwiec y lanzado oficialmente en 2017. Fue diseñado con el objetivo de empoderar a los desarrolladores con un conjunto de herramientas versátil que combina las capacidades de OOP, FP y FRP. Inspirado en Angular, NestJS se basa en los sólidos cimientos de TypeScript, lo que permite a los desarrolladores escribir código más robusto y libre de errores.

Myśliwiec trabajaba como ingeniero de software en una empresa que utilizaba Angular para el desarrollo frontend. Quedó impresionado con la arquitectura de Angular y quiso crear un framework similar para el desarrollo backend.

NestJS está construido sobre Express, un popular framework de Node.js. Añade una serie de características que facilitan el desarrollo de aplicaciones escalables y mantenibles, como inyección de dependencias, enrutamiento y middleware.

NestJS se ha convertido rápidamente en uno de los frameworks de Node.js más populares. Es utilizado por varias empresas grandes, como Adidas, GitLab y Decathlon.

Aquí hay una cronología de los eventos clave en la historia de NestJS:

- **2016:** NestJS es creado por Kamil Myśliwiec
- **2017:** Se lanza NestJS 1.0
- **2018:** Se lanza NestJS 5.0, que introduce una serie de nuevas características, como soporte para TypeScript 3.0 y enrutamiento asíncrono
- **2019:** Se lanza NestJS 6.0, que introduce una serie de nuevas características, como soporte para WebSockets y GraphQL
- **2020:** Se lanza NestJS 7.0, que introduce una serie de nuevas características, como soporte para TypeScript 4.0 y despliegues serverless
- **2021:** Se lanza NestJS 8.0, que introduce una serie de nuevas características, como soporte para TypeScript 4.3 y emisores de eventos asíncronos
- **2022:** Se lanza NestJS 9.0, que introduce una serie de nuevas características, como soporte para TypeScript 4.5 y pipes asíncronos
- **2023:** Se lanza NestJS 10.0 con soporte para TypeScript 4.8

NestJS es un framework en rápida evolución que se actualiza constantemente con nuevas características, con una comunidad de rápido crecimiento que sigue manteniendo el framework, el cual madura día a día. Es una opción popular para construir aplicaciones de Node.js escalables y mantenibles.

#### Planteamiento del problema – la necesidad de NestJS

El panorama del lado del servidor está plagado de una infinidad de frameworks y librerías. Sin embargo, muchos de ellos ofrecen una estructura limitada para el diseño de aplicaciones, lo que genera pesadillas de mantenimiento a medida que los proyectos escalan. Además, con la creciente demanda de procesamiento en tiempo real y arquitecturas de microservicios, una solución bien estructurada y escalable es más necesaria que nunca. Aquí entra NestJS, un framework que aporta modularidad, escalabilidad y tipado fuerte al panorama.

Node.js es una plataforma popular para construir aplicaciones web escalables y eficientes. Sin embargo, puede resultar difícil desarrollar aplicaciones grandes y complejas utilizando únicamente Node.js.

NestJS es un framework de TypeScript que proporciona una serie de características que facilitan el desarrollo de aplicaciones Node.js grandes y complejas. Estas características incluyen lo siguiente:

- **Inyección de dependencias:** La inyección de dependencias es un patrón de diseño que te permite desacoplar tu código de sus dependencias. Esto hace que tu código sea más modular y más fácil de probar.
- **Enrutamiento:** NestJS proporciona un potente sistema de enrutamiento que facilita la definición de los endpoints para tu aplicación.
- **Middleware:** El middleware es código que se ejecuta antes o después de que tu aplicación maneje una petición. NestJS proporciona una variedad de middlewares que puedes usar para agregar funcionalidades comunes a tu aplicación, como autenticación y registro de eventos (*logging*).
- **Pruebas:** NestJS proporciona una serie de características que facilitan las pruebas de tu aplicación. Estas características incluyen un framework de pruebas integrado y soporte para simulación (*mocking*) de dependencias.

NestJS es un potente framework que puede ayudarte a desarrollar aplicaciones Node.js grandes y complejas más fácilmente. Si estás buscando un framework que te ayude a construir aplicaciones Node.js escalables y mantenibles, entonces NestJS es una gran opción.

#### NestJS frente a otros frameworks – un análisis comparativo

Express proporciona lo mínimo indispensable para construir aplicaciones; deja la arquitectura en gran medida en tus manos, incluida la elección de si usar NestJS o competidores como Koa, Sails.js y hapi. NestJS va un paso más allá al proporcionar una estructura bien definida para tu aplicación. No es simplemente una colección de librerías, sino un sistema integrado que ofrece una metodología de desarrollo consistente.

La siguiente tabla proporciona una comparación simplificada de NestJS y estos otros frameworks:

| Característica | NestJS | Express | hapi | Fastify | Koa |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Lenguaje de programación nativo | TypeScript | JavaScript | JavaScript | JavaScript | JavaScript |
| Inyección de dependencias | Sí – integrada | Sí – necesita ser implementada | Sí – necesita ser implementada | Sí – necesita ser implementada | Sí – algunas utilidades integradas |
| Enrutamiento | Sí | Sí | Sí | Sí | Sí |
| Middleware | Sí | Sí | Sí | Sí | Sí |
| Manejo de errores | Sí | Sí | Sí | Sí | Sí |
| Pruebas | Sí – abundantes herramientas integradas | Sí | Sí | Sí | Sí |
| Soporte para GraphQL | Sí – abundantes herramientas integradas | Sí | Sí | Sí | Sí |
| Soporte para microservicios | Sí – abundantes herramientas integradas | Sí | Sí | Sí | Sí |
| Comunidad | Grande y activa | Grande y activa | Grande y activa | Activa | Activa |
| Documentación | Excelente | Buena | Buena | Buena | Buena |
| Rendimiento | Excelente | No está mal | Bueno | Excelente | Bueno |
| Escalabilidad | Excelente | Buena | Buena | Buena | Buena |
| Mantenibilidad | Buena | Buena | Buena | Buena | Buena |

*Tabla 1.1: NestJS frente a otros frameworks*

Como puedes ver, NestJS es un framework rico en características comparable con otros frameworks populares de Node.js. Ofrece una serie de ventajas, como el uso de TypeScript, su comunidad grande y activa, y su excelente documentación.

Sin embargo, es importante tener en cuenta que no existe el "mejor" framework para todas las aplicaciones. El mejor framework para ti dependerá de tus necesidades y requisitos específicos.

Aquí hay algunas cosas adicionales a considerar al elegir un framework para tu próximo proyecto:

- **El tamaño y la complejidad de tu proyecto:** Si estás construyendo un proyecto pequeño, un framework simple como Express puede ser suficiente. Sin embargo, si estás construyendo un proyecto grande o complejo, es posible que necesites un framework más rico en características como NestJS.
- **La experiencia y habilidades de tu equipo:** Si tu equipo ya está familiarizado con un framework en particular, esa puede ser la mejor opción para tu proyecto. Sin embargo, si tu equipo es nuevo en Node.js, es posible que desees elegir un framework conocido por su facilidad de uso.
- **Los requisitos de tu proyecto:** Algunos frameworks son más adecuados para ciertos tipos de proyectos que otros. Por ejemplo, si necesitas construir una API RESTful, necesitarás un framework que admita enrutamiento. Si necesitas construir una aplicación en tiempo real, necesitarás un framework que admita WebSocket.

En última instancia, la mejor manera de elegir un framework es investigar tus opciones y elegir el que mejor se adapte a tus necesidades específicas.

#### NestJS en cifras

A fecha de octubre de 2024, NestJS cuenta con una comunidad vibrante con más de 67k estrellas en GitHub, 2 millones de descargas semanales en npm y más de 400 colaboradores. Estos números son un testimonio de su creciente popularidad y adopción en la comunidad de desarrolladores.

#### La filosofía de NestJS

La filosofía de NestJS se basa en los siguientes principios:

- **Convención sobre configuración:** NestJS utiliza un enfoque de convención sobre configuración, lo que significa que muchas de las configuraciones y opciones están predefinidas y se pueden sobrescribir según sea necesario. Esto hace que sea más fácil comenzar con NestJS y construir aplicaciones consistentes.
- **Inyección de dependencias:** NestJS utiliza la inyección de dependencias para gestionar las dependencias entre componentes. Esto facilita la prueba y el mantenimiento de las aplicaciones, y también ayuda a promover el bajo acoplamiento.
- **Arquitectura modular:** Las aplicaciones NestJS se organizan en módulos, lo que facilita la búsqueda y comprensión del código. Los módulos se pueden importar y exportar según sea necesario, lo que facilita la reutilización del código y la creación de aplicaciones escalables.
- **Programación reactiva:** NestJS admite la programación reactiva, lo que facilita la creación de aplicaciones orientadas a eventos. La programación reactiva se basa en la idea de flujos (*streams*), que son secuencias de datos que se pueden procesar de forma asíncrona.
- **TypeScript:** NestJS está construido sobre TypeScript, que es un superconjunto de JavaScript que agrega seguridad de tipos. La seguridad de tipos ayuda a prevenir errores y a hacer que el código sea más legible y mantenible.

La filosofía de NestJS está diseñada para facilitar la construcción de aplicaciones Node.js escalables, mantenibles y comprobables.

#### Ventajas clave de usar NestJS

En esta sección, descubriremos las ventajas fundamentales que hacen de NestJS una opción atractiva para el desarrollo del lado del servidor. Ya sea que te preocupe reducir el código repetitivo (*boilerplate*), mantener una aplicación escalable o aprovechar una comunidad activa de desarrolladores, NestJS te respalda. Profundicemos en las características que no solo simplifican el desarrollo, sino que también lo elevan.

- **Convención sobre configuración:** Una de las características que definen a NestJS es su arquitectura obstinada (*opinionated*), que aboga por un paradigma de "convención sobre configuración". Esto reduce el código repetitivo y la fatiga por toma de decisiones, ayudándote a concentrarte más en la lógica de negocio.
- **Tipado fuerte con TypeScript:** NestJS fomenta fuertemente el uso de TypeScript, ofreciendo verificación estática de tipos y código autodocumentado, reduciendo así los errores y haciendo que la base de código sea más mantenible.
- **Escalabilidad: más que solo manejar peticiones:** La escalabilidad en NestJS no se trata solo de manejar un mayor número de peticiones. También se refiere a la modularidad y la facilidad de adición de funciones, lo que lo convierte en la opción predilecta para proyectos en crecimiento.
- **Un enfoque impulsado por la comunidad:** NestJS está respaldado por una comunidad activa, como lo demuestra su próspero ecosistema, actualizaciones frecuentes y documentación extensa. Esto asegura que el framework se mantenga relevante y evolucione continuamente para enfrentar los desafíos emergentes.

Ahora que hemos explorado los elementos fundamentales, las filosofías y las ventajas de usar NestJS para el desarrollo escalable del lado del servidor, es hora de profundizar en una de sus tecnologías angulares. A continuación, discutiremos el poder y los beneficios de usar TypeScript en el framework NestJS, aclarando por qué se considera más que una simple característica "deseable".

---

### El poder de TypeScript

En un panorama donde JavaScript domina el desarrollo del lado del servidor, TypeScript ofrece una ventaja única con su sólida verificación de tipos y capacidades orientadas a objetos. Esta sección arrojará luz sobre por qué TypeScript no es solo una opción beneficiosa, sino casi esencial cuando se trabaja con NestJS.

#### ¿Por qué TypeScript?

JavaScript, aunque omnipresente y versátil, deja mucho que desear en términos de seguridad de tipos y código autodocumentado. TypeScript, un superconjunto de JavaScript, fue desarrollado por Microsoft para abordar estos problemas. Cuando se utiliza en el contexto de un framework del lado del servidor como NestJS, las características de TypeScript brillan aún más.

#### La propuesta de valor: ¿qué ofrece TypeScript?

TypeScript aporta tipado estático a JavaScript, lo que te permite definir tipos de datos en tiempo de compilación. Esta característica proporciona varias ventajas clave:

- **Reducción de errores:** Detectar errores relacionados con tipos en tiempo de compilación, en lugar de en tiempo de ejecución, puede ahorrar un tiempo significativo de depuración y prevenir posibles fallos del sistema.
- **Calidad del código:** El tipado estático a menudo conduce a un código más legible y autodocumentado, facilitando a los miembros del equipo la comprensión de la base de código.
- **Soporte de IDE:** Los IDEs modernos proporcionan mejores características de IntelliSense y autocompletado cuando se utiliza TypeScript, haciendo que el desarrollo sea más rápido y eficiente.

La Figura 1.1 explica cómo funciona TypeScript. Primero compila los archivos `.ts` a través de su compilador, `ts-loader`, y luego escribe su código equivalente antes de que todo el código `.js` se empaquete en un único punto de entrada a través de un empaquetador (*bundler*).

*Figura 1.1: Diagrama de cómo TypeScript se compila en JavaScript plano y el proceso de verificación de tipos*

#### Fragmento de código – TypeScript en acción

Considera este código JavaScript que contiene un error:

```javascript
// javascript code function add(a, b) { return a + b; } console.log(add(5, "10")); // Output: 510
```

Ahora, veamos cómo TypeScript puede ayudarnos:

```typescript
// typescript code function add(a: number, b: number): number { return a + b; } console.log(add(5, "10")); // Error: Argument of type // 'string' is not assignable // to parameter of type // 'number'.
```

Observa cómo TypeScript detecta la discrepancia de tipos en tiempo de compilación, reduciendo las posibilidades de que los errores lleguen al código de producción.

> **⚒ ¡Pruébalo!**  
> ¿Qué sucede cuando intentas compilar el código TypeScript anterior con la discrepancia de tipos? Experimenta y anota tus observaciones.

#### TypeScript y NestJS – una sinergia perfecta

Las características de TypeScript se alinean a la perfección con los principios de NestJS. He aquí cómo:

- **Decoradores:** Tanto NestJS como TypeScript utilizan decoradores para la reflexión de metadatos, lo que conduce a un código más limpio y organizado.
- **Tipado fuerte:** Esto garantiza que tus controladores, servicios y otras clases de NestJS sean robustos y mantenibles.
- **Modularidad:** El sistema de espacios de nombres y módulos de TypeScript ayuda a organizar el código, haciéndolo escalable y alineándose así con la arquitectura modular de NestJS.

#### Fragmento de código – NestJS con TypeScript

Creemos un servicio simple de NestJS con TypeScript:

```typescript
// typescript code import { Injectable } from '@nestjs/common'; @Injectable() export class AppService { getData(): string { return 'Hello, NestJS!'; } }
```

En este fragmento, el decorador `@Injectable()` y el tipo de retorno `: string` son ejemplos de cómo TypeScript mejora el desarrollo en NestJS.

Al integrar TypeScript, NestJS ofrece un entorno de desarrollo que es robusto, eficiente y menos propenso a errores. El tipado estático de TypeScript, el soporte mejorado de IDE y la compatibilidad con los paradigmas de programación modernos lo convierten en una parte crucial del ecosistema NestJS.

Ahora que hemos visto cómo TypeScript eleva la experiencia de desarrollo en NestJS, profundicemos en el framework en sí. En la siguiente sección, exploraremos las características clave que hacen de NestJS una opción poderosa para construir aplicaciones escalables.

---

### Características clave de NestJS

NestJS no es simplemente otro framework; es un ecosistema completo para construir aplicaciones del lado del servidor escalables, mantenibles y robustas. Incorpora una variedad de características que facilitan el desarrollo rápido sin sacrificar la calidad. Exploremos estas características clave.

#### Modularidad

NestJS es un framework modular, lo que significa que las aplicaciones se organizan en módulos. Los módulos son unidades de código autónomas que se pueden importar y exportar según sea necesario. Esto facilita encontrar y entender el código, reutilizarlo y construir aplicaciones escalables.

Hay dos tipos de módulos en NestJS:

- **Módulos raíz (*Root modules*):** El módulo raíz es el módulo principal de tu aplicación. Importa todos los demás módulos de tu aplicación e inicializa (*bootstraps*) tu aplicación.
- **Módulos de características (*Feature modules*):** Los módulos de características son módulos que contienen código para una funcionalidad específica de tu aplicación. Se pueden importar y exportar según sea necesario, lo que facilita la reutilización del código y la creación de aplicaciones escalables.

Los módulos de NestJS se organizan en una estructura de árbol. El módulo raíz está en la parte superior del árbol, y los módulos de características están anidados debajo del módulo raíz. Esto facilita encontrar y entender el código en tu aplicación.

Los módulos de NestJS también están débilmente acoplados (*loosely coupled*). Esto significa que los módulos no dependen fuertemente entre sí. Esto facilita probar y mantener aplicaciones, y también facilita cambiar o actualizar módulos sin afectar a otros módulos en tu aplicación.

La modularidad de NestJS es una característica poderosa que facilita la creación de aplicaciones escalables, mantenibles y comprobables.

Los siguientes son los beneficios de la modularidad de NestJS:

- **Código desacoplado:** Los módulos fomentan la separación de responsabilidades (*separation of concerns*), lo que conduce a un código más mantenible.
- **Reutilización:** Construye una vez y úsalo en múltiples partes de tu aplicación o incluso en diferentes proyectos.
- **Escalabilidad mejorada:** Los módulos se pueden escalar de forma independiente, lo que facilita escalar tu aplicación según sea necesario.
- **Comprobabilidad mejorada:** Los módulos están débilmente acoplados, lo que los hace más fáciles de probar.
- **Mantenibilidad mejorada:** Los módulos facilitan la realización de cambios en tu aplicación sin afectar a otras partes de la misma.

Aquí hay una definición típica de módulo en una aplicación NestJS. Esta es una parte integral del framework que permite la modularización de funcionalidades. Veamos el código y luego desglosémoslo:

```typescript
// typescript code @Module({ imports: [UsersModule, OrdersModule], controllers: [AppController], providers: [AppService], }) export class AppModule {}
```

Aquí hay una explicación paso a paso del código anterior:

- **Decorador `@Module({...})`:** En NestJS, los decoradores son funciones especiales que proporcionan metadatos sobre la clase que decoran. Aquí, `@Module` es un decorador que le indica a NestJS que `AppModule` es un módulo. Las siguientes son las propiedades dentro del decorador `@Module`:
  - **`imports`:** Este es un arreglo donde listas otros módulos que este módulo necesitará. En el fragmento, `AppModule` requiere `UsersModule` y `OrdersModule`. Estos podrían ser módulos que manejan datos de usuarios y de órdenes, respectivamente.
  - **`controllers`:** Los controladores manejan las peticiones entrantes y devuelven respuestas al cliente. En este fragmento, se lista `AppController`, indicando que este módulo lo utiliza para manejar ciertas rutas y acciones.
  - **`providers`:** Los proveedores pueden ser muchas cosas en NestJS: servicios, repositorios, fábricas, etc. A menudo manejan la lógica y la manipulación de datos entre bastidores. Aquí, `AppService` se lista como un proveedor, que podría ser responsable de alguna lógica de negocio o de la obtención de datos.

Examinaremos esto más a fondo en el [Capítulo 3](https://subscription.packtpub.com/book/web-development/9781835468609/3).

- **`export class AppModule {}`:** Esta es la declaración de la clase `AppModule`. Por sí sola, esta clase no hace mucho. Sin embargo, combinada con el decorador `@Module`, proporciona a NestJS la información que necesita para comprender la estructura y las dependencias de la aplicación.

Los módulos en NestJS te permiten organizar tu aplicación en secciones lógicas y funcionales. A medida que tu aplicación crezca, tendrás más controladores, proveedores e incluso otros módulos. Organizarlos mediante módulos garantiza que puedas realizar un seguimiento de todas estas partes, haciendo que tu aplicación sea más mantenible y escalable.

> **❓ Tarea interactiva**  
> Intenta crear un nuevo módulo para manejar productos en una aplicación de comercio electrónico. ¿Qué controladores, proveedores y servicios podrías necesitar?

#### Inyección de dependencias

La inyección de dependencias es un patrón de diseño que nos permite crear código débilmente acoplado. El código débilmente acoplado es código que no depende estrechamente de otras piezas de código. Esto hace que el código sea más fácil de probar, mantener y reutilizar.

En NestJS, la inyección de dependencias se implementa utilizando el decorador `@Injectable()`. El decorador `@Injectable()` marca una clase como inyectable. Esto significa que la clase se puede inyectar en otras clases mediante la inyección de dependencias.

Para inyectar una dependencia en una clase, usamos el decorador `@Inject()`. El decorador `@Inject()` toma el nombre de la dependencia como argumento. La dependencia se inyectará en el constructor de la clase cuando esta sea instanciada.

**Beneficios:**

- **Comprobabilidad (*Testability*):** Facilita las pruebas unitarias ya que las dependencias se pueden simular (*mock*) fácilmente.
- **Bajo acoplamiento (*Loose coupling*):** Las clases no están cableadas de forma fija, sino que se proporcionan según sea necesario.

Aquí hay un ejemplo común de un fragmento de código de inyección de dependencias utilizado en NestJS. El siguiente servicio se inyectará en un módulo. Exploraremos y entenderemos cómo en el [Capítulo 3](https://subscription.packtpub.com/book/web-development/9781835468609/3):

```typescript
// typescript code @Injectable() export class UsersService { constructor( private readonly userRepository: UserRepository ) {} }
```

> **❓ Reto rápido**  
> Refactoriza el código anterior para inyectar otra dependencia llamada `OrdersRepository`. ¿Cómo modificarías el constructor?

#### Decoradores

Los decoradores son una característica poderosa de NestJS que se puede utilizar para modificar el comportamiento de clases, métodos y propiedades. Los decoradores se declaran utilizando el símbolo `@`, seguido del nombre del decorador.

Hay muchos tipos diferentes de decoradores disponibles en NestJS, cada uno con su propio propósito específico. Algunos de los decoradores más comunes incluyen los siguientes:

- **`@Injectable`:** Este decorador se utiliza para marcar una clase como inyectable. Esto significa que la clase se puede inyectar en otras clases mediante inyección de dependencias.
- **`@Controller`:** Este decorador se utiliza para marcar una clase como controlador. Los controladores son responsables de manejar las peticiones y respuestas HTTP.
- **`@Get`:** Este decorador se utiliza para marcar un método como un endpoint GET. Los endpoints GET se utilizan para recuperar datos del servidor.
- **`@Post`:** Este decorador se utiliza para marcar un método como un endpoint POST. Los endpoints POST se utilizan para crear datos en el servidor.
- **`@Put`:** Este decorador se utiliza para marcar un método como un endpoint PUT. Los endpoints PUT se utilizan para actualizar datos en el servidor.
- **`@Delete`:** Este decorador se utiliza para marcar un método como un endpoint DELETE. Los endpoints DELETE se utilizan para eliminar datos del servidor.

Tendremos más tiempo para revisarlos todos de forma práctica a medida que profundicemos en las secciones prácticas del libro.

**Beneficios:**

Estos son los beneficios más importantes que obtenemos al usar decoradores:

- **Reflexión de metadatos:** Adjunta metadatos a clases/funciones fácilmente.
- **Código más limpio:** Menos código repetitivo (*boilerplate*) y código más declarativo.
- **Legibilidad del código mejorada:** Los decoradores facilitan la comprensión del propósito de una clase, método o propiedad.
- **Mayor flexibilidad del código:** Los decoradores se pueden utilizar para modificar el comportamiento de clases, métodos y propiedades en tiempo de ejecución.
- **Mantenibilidad del código mejorada:** Los decoradores se pueden utilizar para centralizar cambios en el comportamiento de clases, métodos y propiedades.
- **Reducción de duplicación de código:** Los decoradores se pueden utilizar para evitar duplicar código que modifica el comportamiento de clases, métodos y propiedades.

**Fragmento de código:**

En el siguiente ejemplo, hemos utilizado algunos decoradores integrados de NestJS:

```typescript
// typescript code @Get('profile') async getProfile(@Req() request: Request) { return this.usersService.find(request.userId); }
```

#### Guards

En NestJS, los guards desempeñan un papel importante. Al construir aplicaciones profesionales, determinan si la petición será manejada por la ruta, en función de ciertos criterios o condiciones. Estas condiciones pueden basarse en roles, niveles de permisos, etc., a menudo denominado autorización.

Los guards, en NestJS, son clases anotadas con el decorador `@Injectable()`. Deben implementar la interfaz `CanActivate` y tener acceso al contexto de ejecución de la petición, lo que los hace más inteligentes que el middleware habitual en aplicaciones tradicionales de Express.

**Beneficios:**

Con el uso de Guards, obtenemos:

- **Lógica de autorización centralizada:** Los guards te permiten definir la lógica de autorización en un solo lugar, facilitando la gestión del control de acceso a través de múltiples rutas.
- **Políticas de seguridad reutilizables:** Una vez creados, los guards se pueden reutilizar en diferentes controladores o servicios, promoviendo la reutilización de código. Esto es especialmente útil cuando tienes políticas de seguridad recurrentes, como garantizar que un usuario esté autenticado o tenga ciertos roles, que se pueden aplicar a múltiples rutas sin reescribir la lógica.
- **Seguridad mejorada:** Los guards ayudan a fortalecer la seguridad al controlar el acceso al principio del ciclo de vida de la petición. Interceptan las peticiones entrantes antes de que lleguen al manejador de ruta, asegurando que solo procedan las peticiones autorizadas, evitando así el acceso no autorizado a recursos sensibles.
- **Autorización sensible al contexto:** Con acceso al contexto de ejecución (que incluye la petición, respuesta y otros datos relevantes), los guards pueden realizar comprobaciones de autorización complejas y sensibles al contexto. Por ejemplo, puedes examinar encabezados de peticiones, tokens o incluso el rol del usuario para tomar decisiones dinámicas sobre permitir o denegar el acceso.
- **Integración con otras características:** Los guards se integran a la perfección con otras características de NestJS, como la inyección de dependencias y los middlewares. Esto permite arquitecturas altamente flexibles y escalables donde los guards pueden trabajar junto con servicios u otros componentes, haciendo que tu capa de seguridad sea más mantenible y adaptable al cambio.

**Fragmento de código:**

El siguiente es un fragmento de código de ejemplo:

```typescript
@Injectable() export class AuthGuard implements CanActivate { canActivate( context: ExecutionContext, ): boolean | Promise<boolean> | Observable<boolean> { const request = context.switchToHttp().getRequest(); return !!request; } }
```

El fragmento de código anterior es una muestra de un guard llamado `AuthGuard`, y como puedes ver, el método `canActivate()` toma el parámetro `context`. Luego devolvemos `true` si la petición está presente; de lo contrario, devolvemos `false`. En un guard típico, la función `canActivate()` debe devolver un booleano. Cuando devuelve `false`, la petición será rechazada; de lo contrario, se pasa al manejador de ruta.

#### Filtros de excepciones (*Exception filters*)

NestJS proporciona una capa para manejar excepciones en toda tu aplicación de manera uniforme.

**Beneficios:**

Al usar filtros de excepciones, obtenemos:

- **Manejo de errores centralizado:** Gestiona errores en un solo lugar.
- **Respuestas personalizadas:** Elabora mensajes de error fáciles de usar para el usuario.

**Fragmento de código:**

Combinando las características de inyección de dependencias y decoradores que hemos visto antes, aquí hay un ejemplo de un filtro de excepciones en una aplicación NestJS. Estos filtros se pueden importar desde una librería integrada de NestJS o ser personalizados por el desarrollador:

```typescript
// typescript code @Catch(NotFoundException) export class NotFoundExceptionFilter implements ExceptionFilter { catch( exception: NotFoundException, host: ArgumentsHost ) { const ctx = host.switchToHttp(); const response = ctx.getResponse(); const status = exception.getStatus(); response.status(status).json({ statusCode: status, message: "Not Found!", }); } }
```

NestJS tiene una capa integrada para manejar excepciones, lo que permite a los desarrolladores capturar y gestionar excepciones de manera uniforme en toda la aplicación. Este código específico demuestra cómo usar filtros de excepciones para este propósito.

Aquí hay un desglose del código:

- **Anotación – `@Catch(NotFoundException)`:** Este es un decorador proporcionado por NestJS. Indica que el filtro está diseñado específicamente para capturar excepciones del tipo `NotFoundException`. Siempre que se lance una `NotFoundException` dentro de la aplicación, este filtro la capturará y ejecutará la lógica dentro del método `catch`.
- **Declaración de clase – `NotFoundExceptionFilter`:** Esta clase implementa la interfaz `ExceptionFilter` de NestJS. Al implementar esta interfaz, prometemos que esta clase tendrá un método llamado `catch` que sigue una estructura determinada.
- **El método `catch`:** Aquí es donde ocurre el manejo real de la excepción. Toma dos parámetros:
  - `exception`: La excepción que se captura, en este caso, una instancia de `NotFoundException`.
  - `host`: Una instancia de `ArgumentsHost`, que proporciona acceso a los argumentos del método que lanzó la excepción.
- **Cambiando contextos – `host.switchToHttp()`:** Esta línea se utiliza para recuperar el contexto HTTP en el que se lanzó la excepción. Con esto, puedes acceder a los objetos request y response del ciclo HTTP.
- **Obteniendo la respuesta – `ctx.getResponse()`:** Esto recupera el objeto de respuesta HTTP, permitiendo la manipulación de la respuesta que se envía al cliente.
- **Recuperando el estado de la excepción – `exception.getStatus()`:** Dado que `NotFoundException` es una excepción HTTP, lleva consigo un código de estado. Este método recupera ese código de estado.
- **Configurando la respuesta:** El método `status` del objeto de respuesta establece el código de estado HTTP para la respuesta. El método `json` luego envía una respuesta JSON al cliente con el código de estado dado y el mensaje de error `Not Found!`.

Este código demuestra un filtro de excepciones personalizado en NestJS que captura `NotFoundException` y devuelve un mensaje de error JSON consistente y fácil de usar al cliente. Esto no solo mejora el manejo de errores en la aplicación, sino que también garantiza una mejor experiencia de usuario.

> **❓ Cuestionario**  
> ¿Cómo modificarías el filtro para manejar `BadRequestException`?

#### Pipes

Los pipes son una característica poderosa de NestJS que se puede utilizar para transformar datos a medida que fluyen a través de tu aplicación. Los pipes se declaran utilizando el decorador `@Pipe()`.

Hay muchos tipos diferentes de pipes disponibles en NestJS, cada uno con su propio propósito específico. Algunos de los pipes más comunes incluyen los siguientes:

- **Pipes de validación (*Validation pipes*):** Los pipes de validación se pueden utilizar para validar datos antes de que sean procesados por tu aplicación.
- **Pipes de transformación (*Transformation pipes*):** Los pipes de transformación se pueden utilizar para transformar datos a un formato diferente.
- **Pipes de registro (*Logging pipes*):** Los pipes de registro se pueden utilizar para registrar datos a medida que fluyen a través de tu aplicación.
- **Pipes de manejo de errores (*Error-handling pipes*):** Los pipes de manejo de errores se pueden utilizar para manejar los errores que ocurren a medida que los datos fluyen a través de tu aplicación.

Los pipes se pueden encadenar para crear un comportamiento más complejo. Por ejemplo, la siguiente cadena de pipes valida los datos, los transforma en un formato diferente y los registra:

```typescript
// typescript code @Pipe({name: 'myPipe'}) export class MyPipe { constructor(private readonly logger: Logger) {} transform(value: any) { this.logger.log('The value is: ' + value); return value; } }
```

Los pipes son una herramienta poderosa que se puede utilizar para mejorar la flexibilidad y el mantenimiento de tus aplicaciones NestJS.

Aquí hay algunos de los beneficios de usar pipes en NestJS:

- **Flexibilidad:** Los pipes se pueden utilizar para transformar datos de diversas formas, facilitando la adaptación de tu aplicación a diferentes necesidades.
- **Mantenibilidad:** Los pipes se pueden reutilizar y actualizar fácilmente, facilitando el mantenimiento de tu aplicación a medida que evoluciona.
- **Rendimiento:** Los pipes se ejecutan de forma asíncrona, lo que ayuda a mejorar el rendimiento de tu aplicación.

#### Interceptores (*Interceptors*)

Los interceptores te permiten inspeccionar y transformar peticiones/respuestas HTTP en tu aplicación.

Los interceptores son una característica poderosa de NestJS que se puede utilizar para interceptar peticiones y respuestas a medida que fluyen a través de tu aplicación. Los interceptores se declaran utilizando el decorador `@Interceptor()`.

Hay muchos tipos diferentes de interceptores disponibles en NestJS, cada uno con su propio propósito específico. Algunos de los interceptores más comunes incluyen los siguientes:

- **Interceptores de registro (*Logging interceptors*):** Se pueden utilizar para registrar peticiones y respuestas a medida que fluyen a través de tu aplicación.
- **Interceptores de manejo de errores (*Error-handling interceptors*):** Se pueden utilizar para manejar errores que ocurren a medida que las peticiones y respuestas fluyen a través de tu aplicación.
- **Interceptores de seguridad (*Security interceptors*):** Se pueden utilizar para proteger peticiones y respuestas a medida que fluyen a través de tu aplicación.
- **Interceptores de almacenamiento en caché (*Caching interceptors*):** Se pueden utilizar para almacenar en caché peticiones y respuestas a medida que fluyen a través de tu aplicación.

**Beneficios:**

Al usar Interceptores, obtienes:

- **Programación Orientada a Aspectos (*Aspect-Oriented Programming*):** Separa aspectos transversales (*cross-cutting concerns*) como el registro o la transformación de respuestas.
- **Flexibilidad:** Modifica peticiones o respuestas según sea necesario.

**Fragmento de código:**

Profundicemos en una implementación práctica de un interceptor. El siguiente fragmento de código muestra un interceptor básico que transforma la estructura de respuesta:

```typescript
// typescript code @Injectable() export class TransformInterceptor implements NestInterceptor { intercept(context: ExecutionContext, next: CallHandler): Observable<any> { return next.handle().pipe( map(data => ({ result: data })) ); } }
```

En este interceptor, llamado `TransformInterceptor`, los datos de respuesta se envuelven en un objeto con una clave llamada `result`. Esto permite estructuras de respuesta consistentes en toda la aplicación.

> **⚒ Pruébalo**  
> ¿Puedes modificar el interceptor para registrar la duración de cada petición? Pista: Usa `Date.now()` para obtener la marca de tiempo actual.

> **❓ Preguntas de repaso**  
> ¿Por qué es crucial la modularidad en NestJS? ¿Cómo mejoran los pipes la validación de datos en NestJS? ¿Cuál es el propósito central de los interceptores en NestJS?

Acabamos de explorar las características y funcionalidades principales que hacen de NestJS un framework destacado. A medida que avanzamos, nuestro siguiente enfoque estará en el vasto ecosistema de NestJS, sumergiéndonos en las librerías y herramientas que mejoran aún más sus capacidades.

---

### El ecosistema de NestJS

En el desarrollo de software, un ecosistema se refiere al entorno en el que reside una tecnología o framework. Esto incluye todo, desde el soporte de la comunidad hasta librerías de terceros, herramientas y servicios que complementan la oferta principal. Un ecosistema rico suele ser un signo de un framework maduro y sostenible, y NestJS no es una excepción.

#### Módulos de terceros

La modularidad de NestJS permite una integración fluida con diversas librerías de terceros. Los siguientes son algunos ejemplos:

- **TypeORM:** Para la interacción con bases de datos
- **GraphQL:** Para la construcción de APIs GraphQL
- **Passport:** Para la autenticación
- **Swagger:** Para la documentación de APIs

#### Nest CLI

NestJS proporciona una potente herramienta de Interfaz de Línea de Comandos (CLI) que agiliza el proceso de desarrollo.

**Beneficios:**

Al aprovechar la Nest CLI, estos son los beneficios que obtienes:

- **Andamiaje de código (*Code scaffolding*):** Genera fácilmente módulos, servicios y controladores.
- **Compilación y ejecución (*Build and serve*):** Compila y ejecuta tu aplicación con comandos sencillos.

**Fragmento de código:**

Usando la Nest CLI, aquí hay algunos comandos que puedes usar para comenzar con un proyecto simple de Nest:

```bash
# bash code $ nest new my-new-project $ nest generate module users
```

El primer comando te ayudará a configurar un proyecto NestJS básico con el nombre `my-new-project`. El segundo generará un nuevo módulo bajo el proyecto creado. Antes de ejecutarlo, debes estar en un directorio de NestJS.

Estos son solo dos comandos proporcionados por la Nest CLI. Exploraremos esto más a fondo en el [Capítulo 3](https://subscription.packtpub.com/book/web-development/9781835468609/3) del libro.

#### Soporte de la comunidad

Una comunidad sólida garantiza la sostenibilidad de un framework. NestJS cuenta con una comunidad activa que contribuye a lo siguiente:

- Documentación
- Tutoriales
- Módulos de terceros

**Actividad de participación:**

Busca un foro de la comunidad de NestJS o un canal de Discord/Slack. Participa en una discusión y comparte tus ideas allí.  
El canal oficial de Discord de NestJS se puede encontrar aquí: [https://discord.gg/G7Qnnhy](https://discord.gg/G7Qnnhy)

> **❓ Preguntas de repaso**  
> ¿Qué módulos de terceros complementan comúnmente a NestJS? ¿Cómo mejora la Nest CLI el flujo de trabajo de desarrollo? ¿Por qué?

El ecosistema que rodea a NestJS es tanto rico como creciente, lo que lo convierte en una excelente opción para aplicaciones robustas y escalables. Con el soporte de la comunidad, módulos de terceros y una CLI intuitiva, estás bien equipado para afrontar cualquier desafío. Envolvamos ahora todo explorando las razones principales por las que NestJS es una opción ideal para construir aplicaciones escalables y mantenibles en la siguiente sección.

---

### ¿Por qué elegir NestJS?

Llegados a este punto, has recopilado suficiente información sobre NestJS, pero la pregunta persistente permanece: ¿por qué elegir NestJS sobre otros frameworks backend?

- **Arquitectura sólida:** NestJS impone un patrón arquitectónico sólido que garantiza que tu aplicación sea escalable y mantenible.
- **Soporte de TypeScript:** La seguridad de tipos y el soporte de IntelliSense a través de la integración de TypeScript ofrecen una experiencia de desarrollo superior.
- **Flexibilidad:** NestJS es flexible y no dogmático (*unopinionated* en cuanto a librerías), lo que te permite elegir tus propias librerías para bases de datos, autenticación, etc.
- **Ecosistema sólido:** Como hemos visto, el ecosistema que rodea a NestJS es robusto, proporcionándote todas las herramientas necesarias para aplicaciones de nivel empresarial.
- **Soporte empresarial y de la comunidad:** Una comunidad activa y el respaldo de soluciones empresariales garantizan la sostenibilidad del framework.

> **⚒ Ejercicio de toma de decisiones**  
> Enumera los requisitos de tu proyecto actual y observa cuántos de ellos pueden abordarse directamente con las características de NestJS y su ecosistema. Comparte tus conclusiones.

Elegir un framework backend es una decisión importante que afectará la futura escalabilidad, mantenibilidad y el éxito general de tu proyecto. NestJS ofrece un conjunto convincente de características, respaldado por un sólido ecosistema y comunidad, lo que lo convierte en una excelente opción para varios tipos de proyectos, desde startups hasta grandes empresas.

> **❓ Preguntas de repaso**  
> ¿Cómo garantiza NestJS que tu aplicación sea mantenible? ¿Qué papel juega TypeScript en NestJS? ¿Cómo contribuye el ecosistema de NestJS a su viabilidad como framework de elección?

---

### Resumen

En este capítulo, nos hemos embarcado en un emocionante viaje explorando NestJS, un framework robusto diseñado para hacer que el desarrollo del lado del servidor sea fluido y escalable. Comenzamos presentándote el framework, discutiendo sus orígenes y describiendo sus filosofías centrales. Luego profundizamos en el poder transformador de TypeScript en un entorno NestJS, aclarando cómo el tipado fuerte puede mejorar la mantenibilidad del código y reducir errores. Además, examinamos las características clave de NestJS que lo convierten en una opción altamente modular y escalable para el desarrollo web moderno, como su arquitectura bien estructurada y su enfoque impulsado por la comunidad.
