# Parte 1: Introducción a NestJS y Arquitectura de Aplicaciones Escalables

## Capítulo 4: Conceptos Avanzados – Módulos, Controladores, Proveedores, Filtros de Excepciones, Pipes, Guards y Decoradores

Bienvenido al Capítulo 4, donde nos embarcamos en una inmersión apasionante en el terreno avanzado de NestJS. Si alguna vez te has preguntado qué se encuentra bajo la superficie de este renombrado framework, guiándolo para ser tan extensible y adaptable, estás a punto de tener una experiencia esclarecedora. A medida que avancemos, descubriremos la intrincada maquinaria —desde módulos hasta decoradores— que permite a desarrolladores como tú crear aplicaciones que no solo son funcionales, sino también obras maestras de escalabilidad y eficiencia.

Ya has vislumbrado los pilares fundamentales de NestJS. Ahora, imagina esos conceptos fundacionales como las raíces de un árbol. En este capítulo, treparemos por sus ramas, explorando facetas avanzadas que permiten al árbol tocar el cielo, resistir tormentas y adaptarse a las estaciones cambiantes.

Aquí está la hoja de ruta para nuestro ascenso:

- Profundizando en los módulos
- Aspectos esenciales y prácticas avanzadas de controladores en NestJS
- Comprensión de los proveedores en profundidad
- Ciclo de vida de la petición (*Request lifecycle*)

Para cuando lleguemos al final de este capítulo, no solo tendrás una comprensión teórica profunda, sino también las habilidades prácticas para entrelazar estos conceptos avanzados en tus aplicaciones. Estarás equipado para diseñar aplicaciones NestJS que no solo sean eficientes y escalables, sino también un testimonio de una arquitectura de software sobresaliente. Entonces, ¿estás listo para ascender a nuevas alturas? ¡Comencemos esta emocionante escalada!

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Profundizando en los módulos

En el intrincado tapiz del diseño de software, la noción de modularidad siempre se ha destacado como un emblema tanto de elegancia como de eficiencia. Dentro de los paradigmas de NestJS, los módulos emergen como la pieza clave, unificando elementos dispares en un todo cohesivo y poderoso. Pero para apreciar verdaderamente su papel, debemos embarcarnos en una exploración más profunda, sumergiéndonos en la filosofía y el pragmatismo del pensamiento modular. ¿Listo para descubrir las capas? Comencemos.

#### La esencia del pensamiento modular

Antes de la era de la programación moderna, el software a menudo se escribía como estructuras monolíticas: bloques colosales de código donde las funcionalidades estaban entrelazadas. Funcionaba, pero a medida que las aplicaciones crecían, también lo hacía el caos. Aquí entra la programación modular. Nacida de la necesidad de orden en medio de la creciente complejidad, la programación modular defendió la idea de dividir el software en partes distintas y manejables, o "módulos". Este no fue solo un cambio técnico; fue un enfoque transformador para la resolución de problemas, instando a los desarrolladores a ver las aplicaciones no solo como código, sino como una sinfonía de piezas interconectadas pero independientes.

La Figura 4.1 ilustra la diferencia entre una estructura monolítica que solo tiene un bloque, el cual se vuelve más grande cuanto más funciones le agregamos, frente a una estructura modular que tiene múltiples bloques conectados entre sí y trabajando en armonía:

*Figura 4.1: Estructura monolítica frente a estructura modular – derechos de autor coinmarketcap.com*

#### Por qué es más que una simple organización

Aunque a primera vista el pensamiento modular puede parecer una mera organización, es un paradigma que va más allá del orden. Se trata de crear límites claros, fomentar la reutilización y garantizar que cada módulo pueda evolucionar sin causar repercusiones en toda la aplicación. En el mundo de NestJS, esto se vuelve aún más primordial. Con cada módulo actuando como piedra angular para funcionalidades específicas, comprender su papel significa apreciar el intrincado equilibrio entre independencia e interdependencia.

Los módulos en NestJS no son solo un elemento estructural; son el reflejo de una filosofía más profunda. A medida que avancemos, exploraremos cómo esta filosofía se manifiesta en el diseño y la funcionalidad de las aplicaciones Nest. A continuación, profundizaremos en los aspectos tangibles de los módulos en NestJS, desglosando sus funciones y responsabilidades.

#### El papel fundamental de los módulos en NestJS

A medida que nos adentramos en las complejidades de NestJS, se hace evidente que los módulos no son solo un subproducto del diseño del framework; son su corazón palpitante. Los módulos desempeñan un papel fundamental en la formación de la estructura subyacente y en garantizar la orquestación fluida de la infinidad de elementos dentro de una aplicación. Para discernir su importancia, descubramos sus capas.

#### El aglutinante cohesivo de una aplicación

Los módulos en NestJS actúan como el pegamento que une los diversos componentes de una aplicación. En esencia, sirven como unidades organizativas (OU), lo que permite agrupar funcionalidades relacionadas, lo que da como resultado una base de código más intuitiva y estructurada. Cuando pienses en módulos, imagina una biblioteca bien organizada donde cada sección o estante alberga un género específico de libros. En el reino de NestJS, estos "géneros" son tus controladores, proveedores y otros elementos de la aplicación, como se ilustra en la Figura 4.2:

*Figura 4.2: Ilustración de los módulos de NestJS*

#### Un conducto para la escalabilidad y la mantenibilidad

El papel de los módulos no se limita únicamente a la organización. A medida que las aplicaciones crecen, también lo hace su complejidad. Los módulos actúan como un conducto, asegurando que este crecimiento sea sostenible. Facilitan la escalabilidad al permitir a los desarrolladores agregar, modificar o eliminar funcionalidades sin perturbar el equilibrio de toda la aplicación. Este diseño modular respalda inherentemente la mantenibilidad. Cuando se requieren actualizaciones o correcciones, los desarrolladores pueden señalar el módulo exacto responsable y realizar cambios sin tener que navegar por un mar de código no relacionado. Piensa en ello como actualizar una sección de nuestra biblioteca anterior sin causar revuelo en las otras secciones.

Los módulos, en su esencia, son los héroes anónimos de NestJS, que aseguran de manera silenciosa pero efectiva que las aplicaciones sigan siendo estructuradas, escalables y mantenibles. A medida que avancemos, profundizaremos en la mecánica de creación e inicialización de estos módulos, brindándote una visión práctica de su configuración y despliegue.

#### Creación e inicialización de un módulo

Profundizando en el reino de los módulos de NestJS, es crucial comprender no solo su papel sino también su anatomía. Crear e inicializar un módulo no se trata solo de invocar un decorador o agregar unas pocas líneas de código. Se trata de comprender los diversos componentes que constituyen un módulo y cómo interactúan para garantizar que el módulo funcione de manera óptima. Desentrañemos esta intrincada danza.

#### La anatomía de un módulo

Cada módulo de NestJS es una clase caracterizada por un conjunto de propiedades, cada una con un propósito diferenciado. Estas se definen utilizando el decorador `@Module()`. Diseccionemos este decorador para comprender sus propiedades clave:

- **`controllers`:** Aquí listas un conjunto de controladores definidos en el módulo. Los controladores son responsables de manejar las peticiones entrantes y devolver respuestas al cliente. En el siguiente fragmento de código, tenemos un decorador de módulo con un solo controlador; ten en cuenta que este arreglo puede tomar múltiples controladores que manejan diferentes peticiones HTTP, por sus rutas o verbos:

```typescript
@Module({ controllers: [AppController], })
```

- **`providers`:** Estas son entidades que se pueden inyectar en constructores debido a la inyección de dependencias (DI) de Nest. Los proveedores pueden ser servicios, repositorios, fábricas y más. En el siguiente fragmento de código, tenemos un ejemplo de un `AppModule` con un solo proveedor, `AppService`:

```typescript
@Module({ providers: [AppService], })
```

- **`imports`:** Esta propiedad permite que los módulos importen otros módulos, habilitando así funcionalidades compartidas. Asegura que los proveedores del módulo importado se conviertan en parte del módulo en el que se importa, como se muestra en el siguiente fragmento de código:

```typescript
@Module({ imports: [SharedModule], })
```

- **`exports`:** Cuando deseas que los proveedores de un módulo (o los proveedores del módulo importado) sean accesibles y compartidos entre múltiples módulos, deben exportarse utilizando la propiedad `exports` del decorador `@Module`, como en el siguiente fragmento de código:

```typescript
@Module({ exports: [SharedService], })
```

> **Una advertencia:** Si bien la flexibilidad que ofrecen estas propiedades es enriquecedora, el uso indebido puede generar problemas. Por ejemplo, no exportar un servicio necesario puede provocar errores o no tener las funcionalidades necesarias en otro módulo que depende de él. Por lo general, Nest detectará tales errores y se lo notificará al desarrollador a través de registros (*logs*).

#### Primeros pasos: tu primer módulo personalizado

Dar ese primer paso para crear tu módulo personalizado es a la vez emocionante y fundamental en tu aventura con NestJS. Comencemos con una plantilla básica:

```typescript
import { Module } from '@nestjs/common'; import { CustomController } from './custom.controller'; import { CustomService } from './custom.service'; @Module({ imports: [], controllers: [CustomController], providers: [CustomService], }) export class CustomModule {}
```

Aquí, nuestra clase `CustomModule` engloba un módulo `CustomController` y un módulo `CustomService`. Tal como está, no hay importaciones: este módulo es autónomo, no depende de ningún otro módulo y, del mismo modo, no se exporta nada, lo que garantiza que sus servicios y controladores sean exclusivos de este módulo.

¿Pero qué sucede cuando nuestra clase `CustomModule` necesita interactuar con otros módulos? Examinemos un escenario en el que depende de otros dos módulos: `UserModule` y `AuthModule`:

```typescript
import { Module } from '@nestjs/common'; import { CustomController } from './custom.controller'; import { CustomService } from './custom.service'; import { UserModule } from './user.module'; import { AuthModule } from './auth.module'; @Module({ imports: [UserModule, AuthModule], controllers: [CustomController], providers: [CustomService], }) export class CustomModule {}
```

En esta versión evolucionada, `CustomModule` importa `UserModule` y `AuthModule`. Esto significa que ahora puede utilizar cualquier servicio o componente de estos módulos importados que hayan sido exportados por ellos. Si, por ejemplo, `UserModule` tuviera un servicio que `CustomModule` necesitara, primero tendría que ser exportado desde `UserModule` para ser accesible aquí.

> **Idea clave**  
> Al importar otros módulos, no solo los estamos vinculando, estamos tejiendo una red de dependencias. Esta estructura entrelazada debe elaborarse con cuidado para evitar dependencias circulares y garantizar que la aplicación siga siendo escalable y mantenible.

A medida que continúa nuestra exploración de la arquitectura modular de NestJS, es evidente que los módulos no son solo bloques de construcción. Son componentes intrincadamente diseñados que asumen la responsabilidad de definir y gestionar los límites y las interacciones de las funcionalidades de nuestra aplicación.

#### El grafo de módulos de la aplicación (*Application module graph*)

Cada aplicación NestJS, en su corazón, es fundamentalmente una colección de módulos. Estos módulos encapsulan distintas funcionalidades e interactúan para formar una aplicación coherente. Piensa en ellos como piezas de un rompecabezas; si bien cada pieza tiene su forma e imagen únicas, es el entrelazamiento de estas piezas lo que completa la imagen general.

#### Visualización de las interacciones entre módulos

En el centro mismo de cada aplicación NestJS se encuentra el módulo raíz, normalmente conocido como `AppModule`. Es el punto de entrada de tu aplicación, el centro neurálgico que une a todos los demás módulos. Cada proyecto NestJS tiene este módulo principal, que actúa como la base sobre la que se agregan otros módulos.

Para comprender completamente la arquitectura de la aplicación, uno debe visualizar su grafo de módulos, una representación de cómo los módulos se interconectan y dependen unos de otros. Si te has preguntado acerca de la estructura de datos que utiliza NestJS para representar estas conexiones, es un grafo dirigido. ¿Por qué un grafo? Porque permite una representación clara de dependencias y relaciones, facilitando la comprensión del flujo de datos y las interacciones entre módulos.

El sistema de módulos de Nest garantiza que cada módulo sea una unidad única de responsabilidad. A medida que los módulos se vuelven dependientes entre sí, forman un grafo acíclico dirigido (*Directed Acyclic Graph* - DAG) que dibuja una imagen clara de la arquitectura de tu aplicación. Esta visualización ayuda en los siguientes aspectos:

- **Diagnóstico de problemas:** Identificar fácilmente qué módulos podrían verse afectados cuando un solo módulo encuentra un problema.
- **Refactorización optimizada:** Reconocer qué módulos se pueden refactorizar de forma independiente sin alterar la funcionalidad general de la aplicación.
- **Escalabilidad mejorada:** Agregar estratégicamente nuevos módulos o expandir los existentes en función del grafo de módulos actual.

#### Comprensión de las dependencias entre módulos

Conectar módulos es sencillo: un módulo declara sus dependencias de otros a través del arreglo `imports` en el decorador `@Module`. Por ejemplo, si `ModuleA` necesitara funcionalidades de `ModuleB`, simplemente importaría `ModuleB` como se muestra en el siguiente código:

```typescript
@Module({ imports: [ModuleB], }) export class ModuleA {}
```

Sin embargo, el simple hecho de importar no hace que las funcionalidades estén disponibles. `ModuleB` debe exportar explícitamente los servicios o controladores requeridos, haciéndolos accesibles para otros módulos, como lo siguiente:

```typescript
@Module({ providers: [ServiceFromModuleB], exports: [ServiceFromModuleB] }) export class ModuleB {}
```

Aquí es donde entran en juego las complejidades:

- **Aislamiento de módulos:** Uno podría preguntarse si es posible tener un módulo independiente, desconectado del módulo raíz o de cualquier otro módulo. En la práctica, esto no es factible. Cada módulo en una aplicación NestJS se conecta invariablemente, ya sea directa o indirectamente, al módulo raíz. Los módulos aislados permanecerían inertes y no contribuirían a la funcionalidad de la aplicación.
- **Exportación de funcionalidades:** Recuerda: el simple hecho de importar un módulo no garantiza el acceso a sus servicios o controladores. El módulo proveedor debe exportar explícitamente las funcionalidades requeridas. Si `ModuleB` proporciona un servicio que `ModuleA` necesita, esto plantea una pregunta esencial: ¿Se puede acceder directamente a un módulo sin ser exportado? La respuesta tiene matices. Si bien puedes utilizar las funcionalidades dentro del módulo donde están declaradas, para compartirlas entre múltiples módulos, deben ser exportadas. Piensa en las exportaciones como dar "permiso" a otros módulos para utilizar servicios o controladores específicos.
- **Número óptimo de módulos:** No existe un número estricto de módulos que deba tener una aplicación. Sin embargo, cada módulo debe encapsular una funcionalidad o dominio específico de tu aplicación. Tener módulos claramente definidos ayuda a la mantenibilidad y escalabilidad.
- **La decisión de exportar:** No todos los módulos necesitan ser exportados. La decisión depende de si prevés que otros módulos necesitarán las funcionalidades contenidas dentro de un módulo específico. Si un módulo es únicamente de apoyo y no ofrece servicios o controladores que otros puedan requerir, no hay necesidad de exportarlo.

Comprender las complejidades del grafo de módulos y la lógica detrás de las dependencias entre módulos proporciona a los arquitectos y desarrolladores los conocimientos necesarios para crear aplicaciones escalables y mantenibles. Es la armonía entre los módulos, cada uno desempeñando su parte, lo que produce una aplicación NestJS cohesiva y eficiente.

En las secciones siguientes, exploraremos posibles inconvenientes, como las dependencias circulares, y estrategias para garantizar una interacción de módulos fluida y sin errores.

#### Inyección de dependencias (DI): el orquestador silencioso

En el desarrollo de software, no se puede exagerar el valor de gestionar bien las dependencias. La disposición, accesibilidad y ciclo de vida de los componentes de una aplicación influyen significativamente en su adaptabilidad, mantenibilidad y comprobabilidad. Aquí entra la DI: un patrón de diseño que aporta a NestJS toda su destreza. Pero, ¿y si NestJS decidiera no emplear este patrón de diseño? Las consecuencias remodelarían la esencia del framework, con repercusiones que afectarían la escalabilidad, la mantenibilidad y la facilidad de prueba.

#### Principios y poder de la DI

Para comprender el poder de la DI en un framework como NestJS, he aquí cómo podrían verse las cosas sin ella:

- **Acoplamiento más fuerte:** Imagina un mundo donde cada clase instancia sus propias dependencias. Este diseño no solo conduciría a un acoplamiento más estrecho entre los componentes, sino que también restringiría la modularidad. Las pruebas serían un desafío, ya que simular dependencias para pruebas aisladas sería engorroso.
- **Menor reutilización:** Sin DI, cambiar el comportamiento de un componente reemplazando una dependencia requeriría modificar el componente en sí, disminuyendo en gran medida la reutilización del código.
- **Configurabilidad comprometida:** La flexibilidad al configurar aplicaciones, especialmente en tiempo de ejecución, se vería afectada. Estarías atado a dependencias codificadas de forma rígida, lo que haría que el sistema fuera rígido.

Con la DI, se puede lograr lo siguiente:

- **Separación de responsabilidades (*Separation of Concerns* - SoC):** La DI defiende el principio de SoC. En lugar de que las clases sean responsables de obtener sus dependencias, un sistema externo (el inyector) las suministra. Esta separación impulsa la modularidad y hace que los componentes sean más intercambiables.
- **Comprobabilidad mejorada:** Con DI, las pruebas se vuelven más sencillas. Dado que las dependencias se pueden inyectar, puedes sustituir fácilmente las implementaciones reales con mocks o stubs, facilitando las pruebas unitarias aisladas.
- **Resolución dinámica de dependencias:** La DI capacita a las aplicaciones con resolución dinámica de dependencias, lo que permite a los desarrolladores modificar el comportamiento del sistema alterando las dependencias suministradas sin cambiar las clases que las usan.

En esencia, la DI agiliza el flujo arquitectónico, permitiendo a los desarrolladores concentrarse en crear la lógica sin verse abrumados por las complejidades de la gestión de dependencias.

#### El inyector jerárquico de Nest

NestJS no solo aprovecha la DI, sino que también la eleva con su sistema de inyector jerárquico. Este sistema funciona en capas, asegurando que cada módulo y sus componentes obtengan sus dependencias del inyector más cercano, ya sea específico del módulo o global.

Por ejemplo, cuando `ServiceA` solicita una dependencia, el inyector primero verifica dentro de su módulo. Si no la encuentra allí, sube por la jerarquía, examinando los módulos padre, hasta llegar al módulo raíz o al ámbito global.

Este enfoque jerárquico ofrece varias ventajas:

- **Instancias con ámbito (*Scoped instances*):** Permite instancias de proveedores con ámbito delimitado. Un servicio puede tener diferentes instancias en diferentes módulos, lo que permite configuraciones y comportamientos específicos del módulo.
- **Modularidad mejorada:** El sistema jerárquico fomenta límites claros entre módulos, reforzando la modularidad.
- **Mecanismo de reserva (*Fallback mechanism*):** La búsqueda basada en capas proporciona un mecanismo de respaldo integrado. Si un módulo local no proporciona una dependencia, el sistema puede recurrir a un módulo padre o al ámbito global.

En conclusión, la decisión de NestJS de aprovechar la DI, complementada por su inyector jerárquico, es estratégica. Ofrece a los desarrolladores una experiencia fluida, garantiza una estructura óptima de la aplicación e impulsa inherentemente la capacidad de prueba y la modularidad. Es difícil imaginar el NestJS que amamos sin su orquestador silencioso.

#### Dependencias circulares: el bucle que no deseas

Las dependencias circulares son un desafío común, especialmente en un sistema modular como NestJS. Cuando no se resuelven, pueden dar lugar a comportamientos inesperados y pueden complicar el proceso de inicialización de los módulos y servicios de tu aplicación. Identificarlas y resolverlas se vuelve crucial para garantizar la robustez de tu aplicación.

#### Identificación de posibles dependencias circulares

El primer paso para gestionar las dependencias circulares es detectarlas. Por lo general, se manifiestan cuando dos clases, servicios o módulos son interdependientes. He aquí un ejemplo clásico:

**Ejemplo 1: Interdependencia de servicios**

```typescript
// cat.service.ts import { Injectable } from '@nestjs/common'; import { DogService } from './dog.service'; @Injectable() export class CatService { constructor(private dogService: DogService) {} } // dog.service.ts import { Injectable } from '@nestjs/common'; import { CatService } from './cat.service'; @Injectable() export class DogService { constructor(private catService: CatService) {} }
```

Aquí, la clase `CatService` inyecta `DogService` en su constructor, lo que significa que depende de `DogService`. Paralelamente, `DogService` inyecta `CatService`, dependiendo así de él. Esta dependencia mutua crea una referencia cíclica. Para que el framework Nest cree una instancia de `CatService`, requiere una instancia de `DogService`, pero `DogService` no se puede instanciar sin una instancia de `CatService`. Esta naturaleza cíclica hace que no esté claro cuál debe inicializarse primero, lo que lleva a un bloqueo (*deadlock*) en el proceso de inicialización.

Otro escenario común es cuando los módulos dependen unos de otros, como en el siguiente ejemplo:

**Ejemplo 2: Interdependencia de módulos**

```typescript
// cat.module.ts import { Module } from '@nestjs/common'; import { DogModule } from './dog.module'; @Module({ imports: [DogModule], }) export class CatModule {} // dog.module.ts import { Module } from '@nestjs/common'; import { CatModule } from './cat.module'; @Module({ imports: [CatModule], }) export class DogModule {}
```

La situación es análoga a nivel de módulo en este ejemplo. La clase `CatModule` importa `DogModule`, lo que significa que depende de algunas funcionalidades proporcionadas por este último. Por el contrario, `DogModule` importa `CatModule`, indicando su dependencia del primero. Esta importación mutua genera una dependencia circular.

Durante el proceso de inicialización, el framework NestJS tendrá dificultades para determinar qué módulo debe inicializarse primero, ya que cada módulo está esperando al otro. Esta dependencia cíclica, al igual que en el ejemplo del servicio, conduce a la ambigüedad en el orden de inicialización.

> **Consejo**  
> Puedes usar la herramienta Madge ([https://github.com/pahen/madge](https://github.com/pahen/madge)) para comprobar si estás encontrando un error de dependencia circular o para saber exactamente dónde está sucediendo usando los siguientes comandos:

```bash
$ npx madge dist/main.js --circular $ npx madge dist/main.js --image graph.png
```

El primer comando imprimirá `No circular dependency found!` si no tienes una dependencia circular en tu proyecto, como se muestra en la Figura 4.3; y el segundo generará un archivo con el nombre especificado en la raíz del proyecto que se parece a la Figura 4.4:

*Figura 4.4: Auditoría de dependencia circular por Madge*

*Figura 4.3: No se encontraron dependencias circulares*

*Figura 4.4: Grafo de aplicación impreso por Madge*

#### Estrategias para evitar y resolver dependencias circulares en NestJS

Aquí hay algunas estrategias que NestJS sugiere para evitar dependencias circulares:

- **Referencia hacia adelante (*Forward reference*):** NestJS proporciona un mecanismo llamado `forwardRef()` que te permite hacer referencia a clases antes de que se definan, resolviendo así el problema de la dependencia cíclica. Usando nuestras clases `CatService` y `DogService` del ejemplo anterior, así es como puedes resolver la dependencia circular usando `forwardRef()`:

```typescript
// cat.service.ts import { Injectable, forwardRef, Inject } from '@nestjs/common'; import { DogService } from './dog.service'; @Injectable() export class CatService { constructor( @Inject(forwardRef(() => DogService)) private dogService: DogService ) {} }
```

Con esto, `DogService` no se importa directamente en el constructor, sino que se referencia a través de la función `forwardRef()`, posponiendo así su instanciación y resolviendo la dependencia cíclica.

- **Refactorización del servicio:** A veces, las dependencias circulares señalan un problema de diseño. Considera refactorizar tus servicios o módulos para cumplir con el Principio de Responsabilidad Única (SRP). Esto puede implicar dividir un servicio o módulo en unidades más pequeñas y enfocadas o reconsiderar las relaciones entre ellos.
- **Abstracción de dependencias:** Introduce un servicio o módulo intermediario del que puedan depender ambos servicios o módulos en conflicto. Este intermediario puede abstraer las funcionalidades comunes que ambos necesitan, eliminando la referencia cíclica directa.

Al comprender las fuentes y los desafíos potenciales de las dependencias circulares y aprovechar las herramientas y los principios de diseño proporcionados por NestJS, los desarrolladores pueden garantizar un proceso de inicialización de aplicaciones más fluido y eficiente.

#### Compartir módulos en toda la aplicación

Construir aplicaciones escalables no se trata solo de dividir tu aplicación en módulos, sino también de compartir funcionalidades de manera eficiente entre diferentes partes de tu aplicación. Aquí entra el poder de `exports` y los módulos globales en NestJS.

#### Por qué importan las exportaciones

Cada módulo tiene un contexto privado. Los servicios, controladores o cualquier otro componente definido en un módulo están encapsulados en su interior. Sin embargo, puede haber situaciones en las que un servicio o componente deba compartirse con otras partes de la aplicación.

Consideremos un servicio de registro (*logging*). Este servicio podría ser requerido en numerosos otros módulos para mecanismos de registro consistentes.

Sin la capacidad de exportar este servicio, terminaríamos duplicando el servicio (malo para la mantenibilidad y el uso de memoria) o moviendo todo a un mega módulo (malo para la escalabilidad).

En NestJS, el arreglo `exports` en los metadatos de un módulo especifica qué proveedores de este módulo deben compartirse y pueden usarse en otros módulos:

```typescript
@Module({ providers: [LoggingService], exports: [LoggingService], }) export class SharedModule {}
```

Con esta configuración, cualquier módulo que importe `SharedModule` también tendrá acceso a `LoggingService`, promoviendo los principios de "No te repitas" (*Don't Repeat Yourself* - DRY) y la gestión centralizada de la lógica.

#### Módulos globales: disponibles en todas partes

Si bien exportar módulos es poderoso, NestJS lo lleva un paso más allá con los módulos globales. Una vez definido como global, un módulo no necesita ser importado en otros módulos; sus proveedores y controladores se vuelven accesibles al instante en todas partes:

```typescript
import { Global, Module } from '@nestjs/common'; @Global() @Module({ providers: [ConfigService], exports: [ConfigService], }) export class ConfigModule {}
```

Estos módulos globales son particularmente útiles para servicios esenciales como la gestión de configuración, el registro o las conexiones a bases de datos que son omnipresentes en toda la aplicación.

#### Organización modular para aplicaciones a gran escala

A medida que las aplicaciones crecen, también lo hace la complejidad. Una organización modular adecuada garantiza que este crecimiento siga siendo manejable y no convierta tu base de código en un caos entrelazado.

#### Estructuración para el crecimiento: módulos de características (*Feature modules*)

Es aconsejable pensar en los módulos en términos de características o dominios de negocio. Cada funcionalidad o dominio puede tener su propio módulo, lo que facilita razonar sobre él, desarrollarlo y probarlo.

Por ejemplo, en una aplicación de comercio electrónico, podríamos tener lo siguiente:

- `UserManagement` podría ser un módulo separado
- `ProductManagement` es otro módulo
- `OrderandCheckout` podría ser otro módulo

Tal configuración no solo modulariza el código sino también el equipo. Diferentes equipos pueden trabajar en diferentes módulos sin pisarse los talones unos a otros.

Con el tiempo, ciertos módulos pueden sobrecargarse. Se deben realizar evaluaciones periódicas para asegurar que los módulos estén enfocados y no asuman demasiadas responsabilidades. Una regla general es que si un módulo está haciendo demasiado, podría ser el momento de dividirlo.

Recuerda: el objetivo principal es garantizar que cada módulo tenga una responsabilidad única y sea una unidad cohesiva. Este enfoque garantiza que los módulos sigan siendo mantenibles, comprensibles y, cuando sea necesario, reutilizables.

#### Reflexiones finales sobre la filosofía modular de Nest

El enfoque de Nest hacia la modularidad no es una ocurrencia tardía. Está en el centro mismo de su filosofía de diseño.

La modularidad en NestJS se inspira en principios de diseño de software consagrados, combinados con las necesidades de las aplicaciones de hoy en día. El objetivo es ofrecer a los desarrolladores una forma de crear aplicaciones estructuradas, escalables y mantenibles sin sentirse limitados.

Los módulos en NestJS son como bloques de construcción: cada bloque tiene un propósito y juntos forman la aplicación completa. Esta visión anima a los desarrolladores a pensar en términos de límites, responsabilidades y colaboraciones.

El enfoque modular garantiza que, a medida que tu aplicación crece, no crezca fuera de control. En cambio, crece de manera estructurada. Cada módulo que agregas, cada servicio que creas y cada controlador que defines encaja en un rompecabezas más grande y bien definido.

Cuando adoptas este pensamiento modular, no solo estás escribiendo código para el presente. Estás diseñando la arquitectura de tu aplicación para el futuro, asegurando que permanezca ágil, adaptable y, sobre todo, un placer para trabajar.

Muy bien: hemos cubierto los aspectos más importantes de los módulos en NestJS; con mucha práctica en los próximos capítulos, nos aseguraremos de que todo este conocimiento teórico sea útil para construir en el mundo real. Ahora, hablemos de otro concepto fundamental importante de NestJS en la siguiente sección.

---

### Aspectos esenciales y prácticas avanzadas de controladores en NestJS

En el ámbito de las aplicaciones web, los controladores desempeñan un papel fundamental en la gestión de las peticiones entrantes y la producción de respuestas relevantes. NestJS, con sus patrones bien diseñados, eleva este papel a una sinfonía de código organizado. En esta sección, vamos a desvelar los misterios de los controladores en NestJS, desde sus aspectos fundamentales hasta la creación de tu primer controlador.

En el corazón de cada aplicación NestJS se encuentra un componente crítico: el controlador. Actuando como nexo entre el cliente y el servidor, los controladores manejan las peticiones entrantes, las enrutan a los servicios apropiados y envían respuestas. Forman el puente que conecta la funcionalidad de tu aplicación con el mundo exterior.

Imagina tu aplicación NestJS como una concurrida estación de trenes. Los controladores son como los jefes de estación, que guían los trenes (o peticiones) al andén correcto (o servicio) y se aseguran de que todo funcione sin problemas. ¡Sin ellos, la estación caería en el caos!

#### Creando tu primer controlador

Configurar un controlador en NestJS es sencillo. La CLI del framework proporciona una forma conveniente de generar uno. He aquí cómo:

```bash
$ nest generate controller users # or use its shortcut $ nest g co users
```

Este comando genera un controlador de usuarios, creando un archivo `users.controller.ts` dentro del directorio `users/`. Si este directorio no existe, la CLI de Nest creará uno para ti. Aquí hay una vista simplificada de lo que contiene el archivo:

```typescript
import { Controller } from '@nestjs/common'; @Controller('users') export class UsersController {}
```

El decorador `@Controller()` le indica a Nest que `UsersController` es un controlador y puede manejar peticiones HTTP. El argumento `users` especifica que este controlador manejará peticiones al endpoint `/users`.

> **Nota importante**  
> En la forma en que se ha diseñado la arquitectura de NestJS, un controlador no debe existir sin un módulo; se requiere un módulo `UsersModule`, que luego se conectará al módulo raíz para que nuestras rutas creadas se registren en el contexto de la aplicación.

#### Enrutamiento básico en controladores

El enrutamiento es fundamental para los controladores. Es el mecanismo que le indica a la aplicación qué método de controlador debe manejar qué petición HTTP. NestJS utiliza decoradores para hacer que este enrutamiento sea declarativo y fácil de entender.

En el centro del enrutamiento se encuentran los manejadores de ruta (*route handlers*). Estos son métodos dentro de un controlador que manejan rutas específicas. NestJS proporciona un conjunto de decoradores —como `@Get()`, `@Post()` y otros— que corresponden a los métodos de petición HTTP.

Considera el siguiente código:

```typescript
import { Controller, Get } from '@nestjs/common'; @Controller('users') export class CatsController { @Get() findAll() { return 'This action returns all users'; } }
```

En el ejemplo anterior, el decorador `@Get()` sin ningún argumento especifica que el método `findAll()` debe manejar las peticiones GET al endpoint `/users`.

¿Pero qué pasa si quieres más granularidad? ¿Qué pasa si deseas tener rutas separadas bajo el endpoint `'users'`? Ahí es donde entran en juego las rutas hijas:

```typescript
import { Controller, Get } from '@nestjs/common'; @Controller('users') export class UsersController { @Get('details') findDetails() { return 'Details about users'; } @Get('images') findImages() { return 'Images of various users'; } }
```

En este ejemplo actualizado, el decorador `@Get('details')` dirige las peticiones GET del endpoint `/users/details` al método `findDetails()`, mientras que el decorador `@Get('images')` dirige las peticiones GET del endpoint `/users/images` al método `findImages()`.

Al usar rutas con decoradores de ruta, puedes organizar y estructurar las rutas de tu aplicación de manera más eficiente, lo que permite una mayor flexibilidad y escalabilidad a medida que tu aplicación crece.

#### Rutas parametrizadas

Las aplicaciones web a menudo tratan con contenido dinámico, donde los datos que deseas pueden depender de alguna variable, como el ID de un elemento. Aquí es donde entran en juego las rutas parametrizadas. En lugar de tener endpoints estáticos como `/users`, tendrías algo como `/users/:id` donde `:id` puede ser cualquier valor que represente el identificador único (*UID*) de un usuario.

Para manejar tales rutas dinámicas, NestJS introduce el decorador `@Param`. Este decorador te permite extraer parámetros de la URL, haciéndolos disponibles para los métodos del controlador.

#### Presentación del decorador `@Param`

El decorador `@Param` en NestJS se utiliza para extraer y vincular los parámetros de ruta a los parámetros del método del controlador. Cuando un cliente envía una petición a una URL que contiene parámetros de ruta, NestJS vincula los valores de la URL a los parámetros del método utilizando este decorador.

Por ejemplo, en la ruta `/users/:id`, `id` es un parámetro de ruta. Cuando un cliente envía una petición a `/users/123`, `123` es el valor del parámetro `id`.

Veamos esto en acción:

```typescript
import { Controller, Get, Param } from '@nestjs/common'; @Controller('users') export class UsersController { @Get(':id') findOne(@Param('id') id: string) { return `This action returns a user with ID ${id}`; } }
```

En este ejemplo ocurre lo siguiente:

- La ruta se define como `@Get(':id')`, lo que indica que espera un parámetro `id` en la URL.
- Dentro del método `findOne()`, se utiliza el decorador `@Param('id')` para extraer el parámetro `id` de la URL.
- El valor del parámetro `id` se vincula luego al argumento `id` del método `findOne()`.
- El método devuelve una cadena que incluye el valor de `id`, mostrando el ID que se está recuperando.

Este mecanismo permite que tu aplicación responda a una multitud de URLs potenciales con una sola ruta, extrayendo los datos requeridos de la propia URL.

#### Manejo de múltiples parámetros

NestJS permite la definición de múltiples parámetros dentro de una sola ruta. Supongamos que deseas filtrar usuarios por `id`, `sex` y `minAge`, con la posibilidad de un parámetro de consulta opcional, `salary`. La ruta podría verse así: `/users/:id/:sex/:minAge`.

Así es como la definirías y manejarías:

```typescript
import { Controller, Get, Param, Query } from '@nestjs/common'; @Controller('users') export class UsersController { @Get(':id/:sex/:minAge') filterUsers( @Param('id') id: string, @Param('sex') sex: string, @Param('minAge') minAge: number, @Query('salary') salary?: number ) { return `Fetching users with ID: ${id}, Sex: ${sex}, Minimum Age: ${minAge}, and Salary: ${salary || 'Not Specified'}`; } }
```

En el ejemplo anterior ocurre lo siguiente:

- La ruta `@Get(':id/:sex/:minAge')` especifica tres parámetros.
- El decorador `@Param()` se utiliza tres veces, una para cada parámetro, para extraer sus valores de la URL. Nest vinculará automáticamente estos valores del cuerpo de la petición; omitirlos dará como resultado variables indefinidas y puede romper la lógica de tu controlador.
- Se maneja un parámetro de consulta opcional de salario utilizando el decorador `@Query('salary')`. Si el cliente no proporciona este parámetro de consulta, el valor tendrá como valor predeterminado `undefined`.

Es importante tener en cuenta que en la ruta `/users/123/male/2?salary=10000`, tenemos lo siguiente:

- `123` corresponde a `id`
- `male` corresponde a `sex`
- `2` corresponde a `minAge`
- El parámetro de consulta `salary=10000` corresponde a `salary`

Esta versatilidad permite que las aplicaciones NestJS atiendan diversas necesidades de filtrado, clasificación y consulta en escenarios del mundo real.

Con rutas parametrizadas, múltiples parámetros y la potente combinación de los decoradores `@Param` y `@Query`, NestJS te equipa con la flexibilidad para diseñar rutas dinámicas para una gama más amplia de peticiones en tu aplicación.

#### Escalando tus controladores

En el ámbito de NestJS, como con cualquier framework backend estructurado, mantener los controladores delgados, enfocados y mantenibles se vuelve esencial, especialmente a medida que tu aplicación escala. A continuación se presentan estrategias para garantizar que tus controladores gestionen eficazmente el tráfico HTTP sin sobrecargarse ni complicarse:

- **Separar responsabilidades:** Cumplir con el SRP puede marcar una gran diferencia. La función principal de un controlador debe ser manejar peticiones HTTP y delegar la lógica adicional a otra parte. Si encuentras que tu controlador administra tanto usuarios como sus órdenes, por ejemplo, considera dividirlos en `UsersController` y `OrdersController`.
- **Utilizar módulos:** Piensa en los módulos como contenedores individuales de funcionalidad relacionada. A medida que introduces más controladores, servicios u otros proveedores en tu aplicación, considera agruparlos utilizando módulos. Por ejemplo, un módulo `UsersModule` puede abarcar todo lo relacionado con los usuarios, haciendo que la estructura de la aplicación sea más clara y modular.
- **Middleware e interceptores:** Antes de profundizar en funciones más avanzadas de NestJS, es bueno saber que hay herramientas disponibles para manejar tareas comunes (como registro, transformación de datos o validaciones menores) antes de que la petición llegue al controlador o justo después de que salga. Los middlewares e interceptores desempeñan un papel fundamental aquí, asegurando que el controlador no se sobrecargue con responsabilidades ajenas al manejo del ciclo de petición-respuesta.
- **Aprovechar las clases de servicio:** El verdadero poder de mantener delgados los controladores proviene de las clases de servicio. Supongamos que estás creando funcionalidades en torno a los usuarios. En lugar de colocar la lógica para obtener, modificar o eliminar un usuario directamente en el controlador, delegas estas tareas a una clase de servicio.

Aquí hay un ejemplo simplificado:

```typescript
@Controller('users') export class UsersController { constructor(private usersService: UsersService) {} @Get(':id') findUserById(@Param('id') id: string) { return this.usersService.findById(id); } // ... other methods ... }
```

En este fragmento, `UsersController` simplemente dirige la tarea de obtener un usuario por ID a `UsersService`. El servicio contiene la lógica real, tal vez consultando una base de datos o realizando llamadas a APIs. Esta separación garantiza que los controladores solo manejen el tráfico, delegando la lógica de negocio a los servicios.

En resumen, el objetivo es mantener tus controladores lo más optimizados posible. Las estrategias antes mencionadas proporcionan una hoja de ruta para ese fin, asegurando que a medida que tu aplicación crezca, su estructura siga siendo coherente, organizada y eficiente. Los controladores deben actuar como administradores de tráfico, canalizando las peticiones a los servicios o proveedores apropiados que manejan la lógica de negocio.

#### Manejo refinado de peticiones

En NestJS, a medida que tu aplicación se vuelve más sofisticada, simplemente manejar peticiones HTTP no es suficiente. Necesitas técnicas para refinar este proceso, garantizando un manejo de peticiones eficiente, seguro y escalable. Esta sección desvela los secretos detrás de tales técnicas refinadas, asegurando que tus controladores no sean solo guardianes sino también administradores de tráfico efectivos.

En las aplicaciones web, comprender la petición entrante es fundamental para un manejo eficaz de la respuesta. El objeto `Request` en NestJS actúa como una ventana a esta información esencial, otorgándote información sobre varios detalles de la petición entrante.

#### La anatomía del objeto `Request`

El objeto `Request` en NestJS es una rica fuente de información. Procedente de la instancia subyacente de Express o Fastify, contiene datos sobre el cliente, la petición HTTP, los encabezados y más:

```typescript
import { Controller, Get, Req } from '@nestjs/common'; import { Request } from 'express'; // if you use Fastify, // import from 'fastify' @Controller('info') export class InfoController { @Get() extractReqInfo(@Req() request: Request) { return { method: request.method, url: request.url, headers: request.headers }; } }
```

En el fragmento anterior ocurre lo siguiente:

- Estamos importando el tipo de objeto `Request` de Express.
- El decorador `@Req()` proporciona acceso al objeto `Request` subyacente. Extraemos atributos como `method`, `url` y `headers` de este objeto.

#### Extracción de metadatos

Uno de los principales casos de uso del objeto `Request` en una aplicación web es acceder a los metadatos asociados con la petición entrante. Estos metadatos proporcionan información valiosa, que va desde encabezados y cookies hasta detalles del cliente y más.

En NestJS, extraer esta información es sencillo, y el decorador `@Req()` es fundamental para lograrlo.

Los encabezados se utilizan para enviar información adicional del cliente al servidor. Esto puede abarcar tokens de autenticación, especificaciones de tipo de contenido, información sobre el tipo de respuesta que el cliente puede manejar, y así sucesivamente.

Echemos un vistazo a cómo podemos acceder a estos encabezados:

```typescript
@Get('headers') showHeaders(@Req() request: Request) { return request.headers; }
```

En el código anterior ocurre lo siguiente:

- Usamos el decorador `@Get('headers')` para especificar que este método debe manejar peticiones GET al endpoint `/headers`.
- El decorador `@Req()` se emplea para inyectar el objeto `Request` completo en el método `showHeaders()`.
- La instancia `request.headers` es un objeto que contiene todos los encabezados entrantes. Simplemente lo devolvemos, haciendo posible inspeccionar cada encabezado enviado con la petición.

Al aprovechar el poder del objeto `Request` y de decoradores como `@Req()`, puedes obtener y utilizar metadatos sin esfuerzo, mejorando la capacidad de respuesta y la adaptabilidad de tu aplicación.

#### Manipulación avanzada de peticiones

Las aplicaciones web a menudo requieren un manejo intrincado de peticiones. Si bien a veces se trata de leer de una petición, hay escenarios en los que necesitarás agregar, modificar o incluso eliminar ciertos aspectos de la petición entrante antes de pasarla a los servicios u otras partes de tu aplicación. Esto te permite hacer cumplir la lógica de negocio, administrar metadatos personalizados o incluso mejorar los datos de la petición para un mejor procesamiento posterior.

Considera un escenario en el que te gustaría anotar peticiones entrantes con metadatos adicionales, tal vez para registro, análisis o cualquier necesidad de negocio personalizada. He aquí cómo puedes lograrlo:

```typescript
@Get('annotate') annotateRequest(@Req() request: Request) { // Adding custom metadata request['customMetadata'] = { timestamp: new Date(), userAgent: request.headers['user-agent'], route: request.route.path }; // Further processing... }
```

Desglosemos el fragmento de código anterior:

- El decorador `@Get('annotate')` especifica que este método debe manejar peticiones GET realizadas al endpoint `/annotate`.
- Usamos el decorador `@Req()` para inyectar el objeto `Request` completo.
- Se agrega una nueva propiedad, `customMetadata`, al objeto `Request`. Esta propiedad es un objeto que contiene lo siguiente:
  - La marca de tiempo actual, que muestra cuándo se recibió la petición.
  - El agente de usuario (*User-Agent*) de los encabezados, que indica el software y la versión del cliente (por ejemplo, navegador o aplicación).
  - La ruta de la ruta accedida.

Ahora que has enriquecido el objeto `Request` con metadatos personalizados, se puede utilizar en varias partes de tu aplicación, como servicios o middlewares.

Imagina que tienes un servicio de registro que rastrea la actividad del usuario:

```typescript
@Injectable() export class LoggingService { logActivity(request: Request) { const metadata = request['customMetadata']; console.log( `Accessed route: ${metadata.route} at ${metadata.timestamp} via ${metadata.userAgent}`); } }
```

En tu controlador, después de agregar metadatos a la petición, puedes pasar la petición a `LoggingService` para una acción adicional:

```typescript
constructor( private readonly loggingService: LoggingService ) {} @Get('annotate') annotateRequest(@Req() request: Request) { // ... [Metadata addition code] this.loggingService.logActivity(request); }
```

Al emplear técnicas avanzadas de manipulación de peticiones, los desarrolladores pueden imbuir a sus aplicaciones de una mayor funcionalidad, asegurando que los datos se procesen de manera eficiente e inteligente.

Para obtener información como la dirección IP del cliente o incluso la geolocalización, puedes confiar en el objeto `Request`:

```typescript
@Get('client-info') clientInfo(@Req() request: Request) { return { ip: request.ip, // you could use a service to derive geolocation from // IP, etc. }; }
```

En este fragmento, extraemos la dirección IP del cliente. Usando esta IP, se podrían integrar aún más servicios de geolocalización si fuera necesario.

#### Objetos de Transferencia de Datos (DTOs) avanzados y canalizaciones de validación

A medida que tratas con datos más complejos en una aplicación en crecimiento, los Objetos de Transferencia de Datos (*Data Transfer Objects* - DTOs) se convierten en una herramienta crucial. Definen la forma y las expectativas de los datos con los que trabajarás, asegurando la seguridad de tipos y la validación.

Hay escenarios donde la estructura de datos con la que estás trabajando no es plana. Un usuario, por ejemplo, podría tener una dirección y esa dirección podría tener múltiples propiedades. Para manejar esto, puedes crear DTOs anidados:

```typescript
import { IsString, IsNotEmpty, IsNumber, ValidateNested } from 'class-validator'; class AddressDTO { @IsString() @IsNotEmpty() street: string; @IsNumber() houseNumber: number; } export class UserDTO { @IsString() @IsNotEmpty() name: string; @ValidateNested() address: AddressDTO; }
```

En el fragmento de código proporcionado, se aplica lo siguiente:

- Comenzamos importando los validadores necesarios del paquete `class-validator`.
- La clase `AddressDTO` representa la dirección de un usuario.
- El atributo `street` debe ser una cadena y no puede estar vacío, garantizado por `@IsString()` e `@IsNotEmpty()`, respectivamente.
- El atributo `houseNumber` debe ser un número, verificado por `@IsNumber()`.
- La clase `UserDTO` describe a un usuario con un nombre y una dirección.
- El atributo `name` debe ser una cadena y no puede estar vacío.
- El decorador `@ValidateNested()` garantiza que la propiedad `address` cumpla con la estructura de `AddressDTO`.

#### Validación condicional con grupos de validación

Tu aplicación puede encontrar situaciones en las que la validación de datos deba variar según la operación que se esté realizando. Por ejemplo, al actualizar los datos de un usuario, es posible que no todos los campos sean obligatorios. Los grupos de validación te permiten aplicar validaciones condicionalmente. Puedes ver un ejemplo de esto en el siguiente fragmento de código:

```typescript
import { IsString, IsNotEmpty, IsOptional } from 'class-validator'; export class UpdateUserDTO { @IsString() @IsNotEmpty({ groups: ['create'] }) name: string; @IsString() @IsOptional() bio?: string; }
```

En el fragmento de código anterior, se aplica lo siguiente:

- La clase `UpdateUserDTO` se importa junto con los validadores necesarios.
- El atributo `name` debe ser una cadena, garantizado por el decorador `@IsString()`.
- Al crear un usuario, el nombre es obligatorio, representado por `@IsNotEmpty({ groups: ['create'] })`. Sin embargo, para otras operaciones como actualizaciones, el nombre podría no ser necesario.
- El atributo `bio` es opcional y puede ser una cadena. El decorador `@IsOptional()` indica esta opcionalidad y `@IsString()` confirma que, si está presente, debe ser una cadena.

Al integrar estructuras DTO avanzadas y técnicas de validación, refuerzas la integridad de los datos de tu aplicación, asegurando que las operaciones se lleven a cabo en estructuras de datos limpias, validadas y esperadas.

#### Decoradores de validación personalizados

Si bien `class-validator` proporciona un conjunto integral de decoradores de validación, habrá casos en los que necesitarás un validador personalizado específico para los requisitos de tu aplicación.

Considera que necesitas validar si una cadena dada es un palíndromo. En lugar de verificar esto manualmente en múltiples lugares, puedes encapsular esta lógica en un decorador de validación personalizado:

```typescript
import { registerDecorator, ValidationOptions } from 'class-validator'; export function IsPalindrome( validationOptions?: ValidationOptions) { return function (object: Object, propertyName: string) { registerDecorator({ name: 'IsPalindrome', target: object.constructor, propertyName: propertyName, options: validationOptions, validator: { validate(value: any) { return typeof value === 'string' && value === value.split('').reverse().join(''); } } }); }; }
```

Aquí hay una explicación del código anterior:

- Comenzamos importando `registerDecorator` y `ValidationOptions` de `class-validator`.
- Luego definimos una función `IsPalindrome`, que recibe `validationOptions`. Estas opciones nos permiten personalizar mensajes de error de validación o especificar grupos, entre otras configuraciones.
- Dentro de esta función, se devuelve otra función que utiliza el método `registerDecorator()`. Este método es fundamental para crear decoradores personalizados en `class-validator`.
- El atributo `name` especifica el nombre del decorador.
- Los atributos `target` y `propertyName` ayudan a señalar la ubicación exacta donde se utiliza nuestro decorador.
- La clave `validator` contiene un objeto con un método `validate()`. Este método contiene nuestra lógica personalizada: en este caso, verificar si un valor es un palíndromo.

Con este validador personalizado en su lugar, ahora puedes decorar propiedades en tus DTOs con `@IsPalindrome()`. Cuando se ejecuta la canalización de validación, comprueba si el valor de la propiedad decorada es un palíndromo.

Después de definir nuestro decorador de validación personalizado, el siguiente paso lógico es incorporarlo a nuestros DTOs. Esto asegura que los datos entrantes o salientes se adhieran a nuestra lógica de validación personalizada.

Imagina un DTO para un servicio lingüístico que estudia palíndromos. Un requisito para uno de los endpoints es que una palabra o frase proporcionada deba ser de hecho un palíndromo. He aquí cómo podrías estructurar dicho DTO:

```typescript
import { IsNotEmpty } from 'class-validator'; import { IsPalindrome } from './path-to-your-decorator-file'; export class PalindromeDTO { @IsNotEmpty() @IsPalindrome({ message: 'The provided word or phrase is not a palindrome.' }) word: string; }
```

En el fragmento de código anterior, se aplica lo siguiente:

- Comenzamos importando los validadores necesarios, incluido nuestro decorador personalizado `@IsPalindrome`.
- En la clase `PalindromeDTO`, tenemos una propiedad llamada `word` que contendrá la cadena que deseamos validar.
- Primero nos aseguramos de que el atributo `word` no esté vacío con `@IsNotEmpty()`.
- A continuación, usamos el decorador `@IsPalindrome()`. Además, hemos proporcionado un mensaje de error personalizado utilizando el atributo `message` en caso de que la validación falle.

Con este DTO en su lugar, cuando un usuario envía datos a un endpoint que espera una instancia de `PalindromeDTO`, la canalización de validación de NestJS verificará que el atributo `word` enviado no esté vacío y sea un palíndromo. Si no es un palíndromo, la canalización devolverá un error con el mensaje `"The provided word or phrase is not a palindrome."`.

Al integrar decoradores de validación personalizados dentro de los DTOs, centralizas tu lógica de validación y mantienes controladores y servicios más limpios e intuitivos.

#### Transformación de datos

Tras la validación, puede ser necesario transformar los datos validados. El paquete `class-transformer` ayuda con esto:

```typescript
import { Transform } from 'class-transformer'; export class UserDTO { @IsString() name: string; @Transform(value => value.toUpperCase()) favoriteColor: string; }
```

Aquí, el decorador `@Transform()` asegura que el valor de `favoriteColor` se almacene en mayúsculas.

#### Transformación de respuestas: interceptores en acción

Los interceptores desempeñan un papel importante en la arquitectura de NestJS. A un nivel alto, los interceptores permiten a los desarrolladores inspeccionar y transformar las peticiones y respuestas HTTP. Pueden actuar antes o después de que se ejecute un manejador de ruta (método de controlador), lo que los convierte en herramientas versátiles para tareas como el registro, la transformación o la medición del rendimiento.

El trabajo fundamental de un interceptor es permitir operaciones tanto antes como después de la ejecución de un método. Profundicemos en un ejemplo:

```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common'; import { Observable } from 'rxjs'; import { map } from 'rxjs/operators'; @Injectable() export class UppercaseInterceptor implements NestInterceptor { intercept( context: ExecutionContext, next: CallHandler): Observable<any> { return next.handle().pipe( map(data => (typeof data === 'string' ? data.toUpperCase() : Data ) ) ); } }
```

En el fragmento de código anterior, se aplica lo siguiente:

- `@Injectable()` declara que `UppercaseInterceptor` puede ser administrado por el sistema DI de Nest.
- El método `intercept()` es donde ocurre la magia:
  - La instancia `context: ExecutionContext` proporciona detalles del ciclo de petición actual, lo que te permite acceder a la petición, respuesta o incluso metadatos sobre la ruta que se está ejecutando actualmente.
  - La instancia `next: CallHandler` representa el siguiente procesador en el flujo de peticiones. Al llamar a `next.handle()`, pasas el control al siguiente interceptor o al manejador de ruta en sí.
  - Dentro del operador `map`, verificamos si los datos de respuesta son una cadena. Si lo son, los convertimos a mayúsculas. De lo contrario, devolvemos los datos tal cual.

#### Mapeo de respuestas

Más allá de las transformaciones simples, los interceptores pueden reestructurar todo el cuerpo de la respuesta. Considera este interceptor, que envuelve las respuestas:

```typescript
@Injectable() export class ResponseInterceptor implements NestInterceptor { intercept( context: ExecutionContext, next: CallHandler): Observable<any> { return next.handle().pipe( map(data => ({ status: 'success', data }) ) ); } }
```

Ahora, independientemente de lo que devuelvan tus manejadores de ruta, cada respuesta será un objeto que contiene un campo `status` con el valor `'success'` y un campo `data` que contiene la respuesta original.

En la siguiente sección, exploraremos cómo interceptar peticiones y asegurarnos de que los usuarios cumplan ciertos criterios antes de que una petición pueda acceder a endpoints específicos. ¡Así que abróchate el cinturón y profundicemos en el mundo de NestJS!

---

### Middleware: los compañeros no celebrados del controlador

En la arquitectura de las aplicaciones web modernas, las funciones de middleware son los héroes anónimos que trabajan entre bastidores. Tienen la capacidad única de interceptar y procesar peticiones entrantes antes de que lleguen a tus manejadores de ruta (controladores) y después de que tu aplicación haya enviado una respuesta. En NestJS, el middleware adopta la forma de una función que tiene acceso a los objetos `Request` y `Response`, así como a la función `next`, que forma parte del ciclo de petición-respuesta.

Las funciones de middleware pueden realizar una variedad de tareas, como las siguientes:

- Modificar objetos `Request` y `Response`
- Finalizar el ciclo de petición-respuesta
- Llamar al siguiente middleware en la pila
- Registro (*logging*), autenticación y mucho más

En NestJS, las funciones de middleware están construidas para funcionar en armonía con la estructura modular del framework, ofreciendo así un enfoque robusto y organizado para manejar tareas secundarias.

Uno de los usos más fundamentales del middleware es el registro (*logging*). Al capturar cada petición entrante, un middleware de registro puede ayudar a realizar un seguimiento de cómo se está utilizando el sistema, así como ayudar en la depuración.

Por ejemplo, he aquí cómo podrías escribir un middleware de registro simple en NestJS:

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common'; import { Request, Response, NextFunction } from 'express'; @Injectable() export class LoggingMiddleware implements NestMiddleware { use(req: Request, res: Response, next: NextFunction) { console.log( `[${new Date().toISOString()}] Request made to: ${req.path}`); next(); } }
```

En este fragmento ocurre lo siguiente:

- Creamos una clase `LoggingMiddleware` que implementa la interfaz `NestMiddleware`; esto transforma automáticamente esta clase en un middleware además del decorador `@Injectable`.
- El método `use()` es donde definimos lo que hace el middleware. Aquí registra la fecha actual y la ruta solicitada.
- Se llama a la función `next()` para pasar la petición al siguiente middleware (o al manejador de ruta si es el último middleware en la pila).

Después de la ejecución de tu middleware, se extrae una breve conclusión sobre la petición, lo que brinda información valiosa sobre el tráfico que recibe tu aplicación.

#### Construcción de middlewares personalizados en NestJS

Para construir un middleware personalizado en NestJS, primero definimos una clase que implementa la interfaz `NestMiddleware`. Repasemos la creación de un middleware que verifica un encabezado específico en la petición:

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common'; import { Request, Response } from 'express'; @Injectable() export class HeaderCheckMiddleware implements NestMiddleware { use(req: Request, res: Response, next: NextFunction) { if(req.headers['x-custom-header']) { console.log('Header is present'); next(); } else { res.status(400).send('Missing custom header'); } } }
```

Desglosemos el código:

- Nuestra clase `HeaderCheckMiddleware` busca `x-custom-header` en los encabezados de la petición.
- Si el encabezado existe, registramos un mensaje y procedemos llamando a `next()`.
- Si falta el encabezado, finalizamos la petición enviando una respuesta 400.

Este middleware es un gran ejemplo de cómo interceptar y validar peticiones para requisitos específicos antes de que lleguen a tus controladores.

#### Configuración mejorada de middleware

Para aprovechar el concepto de middleware en NestJS, aquí hay algunos puntos importantes a considerar:

#### Aplicación de middleware global

En NestJS, el middleware global se aplica a cada petición entrante, independientemente de la ruta. Esto es especialmente útil para funciones que deben ejecutarse en toda la aplicación, como el manejo del intercambio de recursos de origen cruzado (*CORS*) o el registro general.

He aquí cómo configurar el middleware global en el archivo principal de tu aplicación (normalmente `main.ts`):

```typescript
import { NestFactory } from '@nestjs/core'; import { AppModule } from './app.module'; import { LoggingMiddleware } from './logging.middleware'; async function bootstrap() { const app = await NestFactory.create(AppModule); app.use(new LoggingMiddleware().use); await app.listen(3000); } bootstrap();
```

Aquí, `app.use(new LoggingMiddleware().use)` registra `LoggingMiddleware` como un middleware global.

#### Encadenamiento de múltiples middlewares

NestJS permite encadenar múltiples middlewares en la misma ruta. Esto puede ser útil cuando deseas realizar varias operaciones en secuencia, como validar una petición, luego registrarla y quizás modificarla antes de que llegue al controlador.

Agreguemos otro middleware a nuestra cadena:

```typescript
consumer .apply( FirstMiddleware, SecondMiddleware, LoggingMiddleware ) .forRoutes('*');
```

En la función `apply`, se pueden pasar múltiples middlewares en el orden en que deben ejecutarse.

#### Utilización de middleware funcional

Si bien el middleware basado en clases es común en NestJS, a veces es preferible un enfoque más simple que utilice middleware funcional, particularmente para tareas ligeras.

Un middleware funcional es simplemente una función que no necesita un decorador `@Injectable()` y se puede usar directamente:

```typescript
export function SimpleLogger(req, res, next) { console.log('Simple log:', req.path); next(); }
```

Se aplica de la siguiente manera:

```typescript
consumer.apply(SimpleLogger).forRoutes('*');
```

La clase `MiddlewareConsumer` ofrece métodos para ajustar con precisión las rutas a las que se aplica el middleware:

- **`exclude()`:** Este método te permite especificar rutas a las que no se debe aplicar el middleware:

```typescript
consumer .apply(LoggingMiddleware) .exclude( { path: 'health', method: RequestMethod.GET }, { path: 'metrics', method: RequestMethod.ALL } ) .forRoutes('*');
```

Aquí, `LoggingMiddleware` se aplica globalmente excepto para las peticiones GET `/health` y todas las peticiones `/metrics`. La clase `RequestMethod` se puede importar del paquete `@nestjs/common`.

- **`forRoutes()`:** Este método se utiliza para aplicar middleware a rutas específicas. Puede tomar cadenas, rutas de acceso, controladores e incluso métodos de controladores:

```typescript
consumer .apply(AuthenticationMiddleware) .forRoutes( { path: 'users', method: RequestMethod.ALL } );
```

`AuthenticationMiddleware` ahora solo se aplica a las rutas bajo `/users`.

#### Secuencia de middleware y flujo de ejecución

Los middlewares en NestJS se ejecutan en el orden en que están configurados. Esta secuencia es crucial porque determina cómo se procesa y potencialmente se transforma la petición antes de llegar al controlador. Comprender esta secuencia es esencial para evitar comportamientos inesperados en la lógica de tu aplicación.

Para garantizar que los middlewares no creen cuellos de botella, es importante mantenerlos delgados y eficientes, delegando cualquier procesamiento pesado a servicios o tareas en segundo plano cuando sea posible.

Aunque es potente, el middleware no siempre es la herramienta adecuada para cada trabajo. Por ejemplo, cuando necesitas proteger rutas basadas en autenticación o autorización, sería mejor usar guards. El middleware es excelente para tareas que no involucran la toma de decisiones sobre la continuación del ciclo de petición-respuesta basadas en la lógica de negocio.

Como pensamiento final sobre el middleware, recuerda que, aunque no siempre están al frente de las conversaciones de desarrollo, desempeñan un papel vital en la solidez y confiabilidad de tu aplicación.

Al concluir esta sección sobre middleware, queda claro que estos componentes sirven como puntos de control críticos dentro del ciclo de vida de la petición, facilitando tareas que son ortogonales a tu lógica de negocio. Su uso correcto garantiza una base de código limpia y mantenible.

Antes de sumergirnos en nuestro próximo tema, visualicemos un flujo de middleware como se muestra en la Figura 4.5:

*Figura 4.5: Flujo de petición*

En la Figura 4.5 podemos ver lo siguiente:

1. Cuando un cliente realiza una petición HTTP (A), la petición es primero interceptada por el middleware, que puede realizar algunos cálculos y comprobaciones, como registrar, verificar el token, verificar la presencia de cierta información en la petición o incluso modificar el objeto `Request` antes de pasarlo al manejador.
2. Si la petición no satisface una cierta cantidad de criterios (B), el propio middleware responderá al cliente, con un mensaje de error adecuado.
3. Si la petición es correcta (C), el middleware la pasará al siguiente paso llamando a la función `next()`; para entonces, la petición puede ser manejada por otro middleware en la cola o por el manejador de la petición, que es, en el contexto de Nest, el controlador.
4. Luego, el controlador enviará una respuesta al cliente (D).

El middleware en NestJS es una característica poderosa que permite un enfoque granular para el manejo de peticiones. Desde la configuración del middleware global hasta el encadenamiento y la personalización de rutas con los métodos de `MiddlewareConsumer`, tenemos las herramientas para controlar el flujo de nuestra aplicación de una manera sofisticada. A medida que aplicamos estos principios, nos aseguramos de que nuestra aplicación no solo realice sus tareas principales de manera efectiva, sino que también maneje las operaciones auxiliares con precisión y elegancia.

Con el panorama de middlewares detallado, pasaremos a profundizar en los guards de ruta, basándonos en nuestro conocimiento del manejo de peticiones. Los guards ofrecen una perspectiva diferente sobre la gestión del tráfico, con un enfoque en autorizar el acceso a las rutas, garantizando que nuestra aplicación permanezca segura y que el acceso de los usuarios se gestione correctamente.

---

### Inmersión profunda en los guards de ruta (*Route guards*)

Los guards de ruta son una parte fundamental del framework NestJS, cruciales para garantizar que se cumpla un determinado conjunto de lógica antes de que se ejecute un manejador de ruta. Estos guards son particularmente vitales para implementar la lógica de autorización y autenticación en una aplicación.

En su núcleo, un guard de ruta en NestJS es una clase anotada con `@Injectable()` que implementa la interfaz `CanActivate`. Esta interfaz requiere la función `canActivate()`, a la que llama NestJS para determinar si la petición actual tiene permiso para continuar.

La Figura 4.6 ilustra cómo funcionan los guards en una aplicación NestJS:

*Figura 4.6: Guards en NestJS – crédito https://docs.nestjs.com/guards*

#### Principios de funcionamiento de los guards

Los guards tienen el poder de controlar la canalización de peticiones. Pueden decidir pasar la petición a la siguiente etapa (devolviendo `true`) o bloquearla (devolviendo `false`). Los guards operan examinando la clase `ExecutionContext`, que proporciona detalles sobre la petición, la respuesta y el contexto de ejecución.

Comencemos con un ejemplo simple. Aquí hay un guard que bloquea cada petición:

```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common'; @Injectable() export class BlockGuard implements CanActivate { canActivate(context: ExecutionContext): boolean { // In a real scenario, you'd implement some logic here return false; } }
```

En este fragmento de código, el método `canActivate()` siempre devuelve `false`, lo que significa que no se podrá acceder a ninguna ruta cuando este guard esté activo. El argumento `ExecutionContext` te da acceso a los detalles de la petición, aunque no se usa aquí.

#### Uso de guards para autenticación y autorización

La autenticación es el proceso de verificar quién es un usuario, mientras que la autorización consiste en determinar qué se le permite hacer. Los guards son un lugar excelente para realizar estas comprobaciones.

Aquí hay un ejemplo de un guard de autenticación:

```typescript
@Injectable() export class AuthGuard implements CanActivate { canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean> { const request = context.switchToHttp().getRequest(); return validateRequest(request); } } function validateRequest(request: Request): boolean { // Authentication logic here return request.headers.authorization ? true : false; }
```

Este guard busca la presencia de un encabezado de autorización en la petición, una forma común de implementar la autenticación basada en tokens.

#### Control de acceso basado en roles (RBAC) con guards

Para aplicaciones con diferentes roles de usuario, los guards pueden hacer cumplir el control de acceso basado en roles (*Role-Based Access Control* - RBAC). Aquí hay un ejemplo de una implementación de guard RBAC:

```typescript
@Injectable() export class RolesGuard implements CanActivate { canActivate(context: ExecutionContext): boolean { const request = context.switchToHttp().getRequest(); const user = request.user; return user && user.role === 'admin'; } }
```

Este guard verifica si el objeto de usuario en la petición tiene el rol de `'admin'`. Solo los usuarios con un rol de `'admin'` pueden acceder a la ruta.

#### Guards dinámicos: ejecución de guards basada en contexto

Los guards dinámicos son adaptables y pueden modificar su comportamiento según la petición o el contexto. Por ejemplo, es posible que tengas un guard que permita el acceso durante ciertas horas del día:

```typescript
@Injectable() export class TimeBasedGuard implements CanActivate { canActivate(context: ExecutionContext): boolean { const currentHour = new Date().getHours(); // Allow access only during office hours return currentHour >= 9 && currentHour <= 17; } }
```

Este guard otorga acceso a una ruta solo si la hora actual está dentro del rango especificado.

#### Composición de guards: combinación de múltiples guards

NestJS permite aplicar múltiples guards a una ruta, y se resolverán de manera lógica AND. Esto significa que todos los guards deben devolver `true` para que la petición continúe:

```typescript
@UseGuards(AuthGuard, RolesGuard) @Get('admin-panel') findAdminPanel() { // Admin panel data }
```

En este caso, una petición debe pasar tanto `AuthGuard` como `RolesGuard` para acceder a la ruta `admin-panel`.

#### Guards globales

Un guard global se aplica a todas las rutas de la aplicación. Puedes configurar un guard global en el archivo principal de la aplicación:

```typescript
async function bootstrap() { const app = await NestFactory.create(AppModule); app.useGlobalGuards(new AuthGuard()); await app.listen(3000); } bootstrap();
```

En esta configuración, `AuthGuard` se aplica globalmente, por lo que todas las rutas requieren autenticación.

#### Nota importante sobre el orden de ejecución

Antes de concluir nuestra exploración de los guards en NestJS, es crucial comprender su lugar dentro de la canalización de procesamiento de peticiones. En el flujo típico de una aplicación NestJS, los middlewares siempre se ejecutan antes que los guards. Esta secuencia es por diseño, lo que permite que el middleware realice sus tareas, como el registro de peticiones, el análisis del cuerpo o incluso la autenticación, antes de que cualquier guard decida si permite o deniega que una petición continúe hacia un manejador de ruta. Recuerda: el trabajo del middleware es preparar y calificar las peticiones para un manejo posterior, mientras que los guards son los guardianes que toman la decisión final sobre el control de acceso según el trabajo preparatorio realizado por los middlewares.

Al tener en cuenta este orden, los desarrolladores pueden diseñar sus aplicaciones con claridad, asegurándose de que cada componente de la canalización de peticiones haga su trabajo de manera efectiva y en la etapa correcta. Esta clara delimitación de responsabilidades es una de las fortalezas de NestJS, proporcionando una estructura sólida para desarrollar aplicaciones del lado del servidor escalables y mantenibles.

Los guards son indispensables en NestJS, actuando como guardianes de las rutas y asegurando que solo procedan las peticiones válidas. Al aprovechar los guards, los desarrolladores pueden hacer cumplir la autenticación, la autorización y una gran cantidad de políticas de control de acceso personalizadas en sus aplicaciones.

Pasando de esta comprensión de los guards, ahora exploraremos cómo NestJS facilita la persistencia de datos con técnicas robustas de integración de bases de datos, asegurando que la capa de datos de tu aplicación sea tan confiable como su manejo de errores.

---

### Transmisión de datos con controladores NestJS (*Streaming data*)

La transmisión de datos (*data streaming*) es un método para transferir datos continuamente de un emisor a un receptor, lo que permite al receptor comenzar a procesar los datos antes de que haya llegado toda la carga útil. Este enfoque contrasta con los métodos tradicionales de transferencia de datos, que requieren que todos los datos estén disponibles antes de que pueda comenzar cualquier procesamiento.

En las aplicaciones web, el streaming se puede utilizar para manejar archivos grandes, video en vivo o fuentes de datos en tiempo real. Es beneficioso para mejorar la experiencia del usuario al reducir los tiempos de espera y mejorar el rendimiento al disminuir el uso de memoria en el servidor.

Las principales ventajas del streaming en aplicaciones web incluyen las siguientes:

- **Eficiencia:** Procesa datos sobre la marcha sin necesidad de almacenar todo el contenido en memoria.
- **Escalabilidad:** Maneja fuentes de datos grandes o infinitas, como transmisiones de medios en vivo.
- **Capacidad de respuesta:** Proporciona a los usuarios comentarios inmediatos, lo cual es crucial para la transmisión de video y los juegos en línea.

#### El papel de los streams en NestJS

NestJS aprovecha los streams de Node.js para manejar de manera eficiente las transferencias de datos. Esta integración garantiza que las aplicaciones NestJS puedan administrar grandes cantidades de datos o datos que se producen continuamente sin abrumar los recursos del servidor.

Node.js proporciona cuatro tipos de streams:

- **Readable:** Streams de los cuales se pueden leer datos (por ejemplo, `fs.createReadStream`).
- **Writable:** Streams a los cuales se pueden escribir datos (por ejemplo, `fs.createWriteStream`).
- **Duplex:** Streams que son tanto legibles como escribibles.
- **Transform:** Streams dúplex que pueden modificar o transformar los datos a medida que se escriben y leen.

#### Implementación de un stream de datos básico

Antes de sumergirnos en el código, preparemos el escenario. Imagina un escenario en el que queremos transmitir un gran conjunto de datos desde el servidor a un cliente, como enviar un archivo CSV grande que contiene datos de usuarios.

Comenzamos creando un endpoint en un controlador NestJS que transmitirá los datos:

```typescript
import { Controller, Get, Res } from '@nestjs/common'; import { Response } from 'express'; import { createReadStream } from 'fs'; import { join } from 'path'; @Controller('data') export class DataController { @Get('stream') streamData(@Res() res: Response) { const filePath = join(__dirname, 'large-dataset.csv'); const stream = createReadStream(filePath); stream.pipe(res); } }
```

En este fragmento de código ocurre lo siguiente:

- Importamos los módulos y decoradores necesarios.
- Creamos una clase `DataController` con un método `streamData()`.
- Dentro del método, usamos la función `createReadStream` de Node.js para leer un archivo del sistema de archivos.
- Luego canalizamos este stream de lectura directamente al objeto `Response`, que es un stream de escritura, usando `stream.pipe(res)`.

Este método transmite el archivo al cliente, enviando datos en fragmentos (*chunks*) tan pronto como están disponibles, en lugar de esperar a que todo el archivo se lea en la memoria.

El método `.pipe()` se utiliza para tomar los datos del stream legible y enviarlos al objeto `Response`, que es un stream escribible. Este método es eficiente porque solo procesa pequeñas piezas del archivo a la vez.

#### Uso de Observables de RxJS para streaming

NestJS está construido con RxJS, una librería para programación reactiva que utiliza Observables. Para implementar streaming con Observables, podemos hacer lo siguiente:

```typescript
import { Controller, Get } from '@nestjs/common'; import { Observable } from 'rxjs'; import { createReadStream } from 'fs'; import { join } from 'path'; import { map } from 'rxjs/operators'; @Controller('data') export class DataController { @Get('observable-stream') streamWithObservable(): Observable<any> { const filePath = join(__dirname, 'large-dataset.csv'); const stream = createReadStream(filePath); return new Observable(observer => { stream.on('data', (chunk) => observer.next(chunk)); stream.on('error', (err) => observer.error(err)); stream.on('end', () => observer.complete()); }).pipe( map(chunk => ({ data: chunk.toString() })), ); } }
```

Aquí, hemos envuelto el stream en una instancia de `Observable` y hemos mapeado cada fragmento a un objeto con una propiedad `data`. Este enfoque nos brinda más control sobre el proceso de transmisión, lo que permite operaciones complejas como filtrado y manejo de errores.

#### Mejores prácticas para el streaming de datos

Al implementar streams en NestJS, aquí hay un par de mejores prácticas a seguir:

- **Manejo de errores en streams:** Escucha siempre el evento `error` en los streams para evitar caídas y manejar adecuadamente las excepciones.
- **Consideraciones de rendimiento:** Cuando trabajes con streams grandes, monitorea el uso de recursos y ajusta el tamaño del fragmento (*chunk size*) del stream para optimizar la memoria y la velocidad.

La transmisión de datos es una característica poderosa en NestJS que permite el manejo eficiente de datos en tiempo real para aplicaciones. Al comprender e implementar los streams adecuadamente, los desarrolladores pueden crear aplicaciones web receptivas y escalables que manejen datos de manera elegante y eficiente.

En la siguiente sección, exploraremos otras características avanzadas del controlador NestJS que mejoran aún más las capacidades de tus aplicaciones.

---

### Patrones avanzados de WebSocket

En el mundo de las aplicaciones en tiempo real, los WebSockets desempeñan un papel integral para facilitar la comunicación inmediata y persistente entre el cliente y el servidor. NestJS, con su arquitectura escalable, ofrece patrones sofisticados para aprovechar la tecnología WebSocket. En esta sección, profundizamos en los patrones avanzados de WebSocket, explorando cómo se pueden implementar dentro de NestJS para construir aplicaciones altamente interactivas y en tiempo real.

Antes de profundizar en los patrones, establezcamos una comprensión de los WebSockets. Los WebSockets permiten un canal de comunicación dúplex completo (*full-duplex*) que permanece abierto durante el tiempo que sea necesario, lo que permite que los mensajes se transmitan de un lado a otro mientras se mantiene viva la conexión. Esto es particularmente útil para cualquier aplicación donde las actualizaciones en tiempo real son cruciales.

#### Beneficios del uso de WebSockets en aplicaciones en tiempo real

Las aplicaciones en tiempo real, como los sistemas de chat, las fuentes de noticias en vivo o las plataformas de juegos en línea, se benefician enormemente de los WebSockets debido a su capacidad para facilitar la transferencia instantánea de datos sin el modelo típico de petición-respuesta de HTTP, lo que hace que la experiencia del usuario sea fluida y receptiva.

Con este conocimiento fundamental, veamos cómo NestJS nos equipa con las herramientas para implementar servidores WebSocket.

#### Configuración de WebSockets en NestJS

Profundicemos en la configuración de WebSockets en tu aplicación NestJS.

NestJS simplifica la complejidad de los backends en tiempo real con su módulo `@nestjs/websockets`, que está construido sobre librerías robustas como Socket.IO o ws, proporcionando una integración perfecta para las funcionalidades de WebSocket.

Para iniciar nuestro viaje con WebSocket, comenzamos configurando un servidor básico. NestJS utiliza el concepto de una puerta de enlace (*gateway*) para manejar las conexiones WebSocket. Este gateway actúa como un puente entre tus clientes y el servidor, administrando los mensajes entrantes y salientes.

Echa un vistazo al siguiente fragmento de código:

```typescript
import { WebSocketGateway, SubscribeMessage, WebSocketServer } from '@nestjs/websockets'; import { Server } from 'socket.io'; @WebSocketGateway() export class AppGateway { @WebSocketServer() server: Server; @SubscribeMessage('message') handleMessage(client: any, payload: any): string { return 'Hello world!'; } }
```

En este fragmento ocurre lo siguiente:

- Declaramos una clase `AppGateway` con el decorador `@WebSocketGateway()`, que inicializa un servidor WebSocket.
- El decorador `@WebSocketServer()` inyecta la instancia del servidor subyacente, lo que permite el acceso directo a la API nativa del servidor.
- El decorador `@SubscribeMessage('message')` escucha los mensajes entrantes con el nombre de evento `'message'` y proporciona un manejador simple que responde con `'Hello world!'`.

Esta configuración es el punto de partida para habilitar la comunicación WebSocket en una aplicación NestJS.

A continuación, analizaremos los patrones que se pueden utilizar para organizar el flujo de datos y manejar escenarios más complejos en la comunicación WebSocket.

#### Organización del flujo de datos con espacios de nombres (*namespaces*) y salas (*rooms*)

Los espacios de nombres en Socket.IO, que NestJS puede utilizar, te permiten segmentar la comunicación WebSocket en diferentes rutas o contextos, lo que es similar a tener múltiples servidores WebSocket y es útil para dividir la aplicación en características o funcionalidades.

Las salas son una función dentro de los espacios de nombres que te permite organizar aún más las conexiones WebSocket. Esto es especialmente útil para transmitir mensajes a un subconjunto de clientes, como en una sala de chat o en el lobby de un juego multijugador.

Echa un vistazo al siguiente fragmento de código:

```typescript
@WebSocketGateway() export class ChatGateway { @SubscribeMessage('joinRoom') handleRoomJoin(client: any, room: string) { client.join(room); client.emit('joinedRoom', room); } @SubscribeMessage('leaveRoom') handleRoomLeave(client: any, room: string) { client.leave(room); client.emit('leftRoom', room); } }
```

Aquí ocurre lo siguiente:

- El método `handleRoomJoin()` permite a un cliente unirse a una sala específica.
- El método `handleRoomLeave()` es la contraparte para salir de la sala.
- Se notifica al cliente al unirse o salir de la sala con un evento.

Con estos métodos, puedes administrar el flujo de comunicación de manera granular, asegurando que los mensajes se entreguen a los destinatarios correctos.

A continuación, exploraremos cómo proteger nuestros canales de WebSocket con prácticas de autenticación y autorización.

#### Autenticación y autorización en WebSockets

Si bien los WebSockets son excelentes para datos en tiempo real, también abren una línea directa de comunicación que, si no se protege, podría explotarse. Por lo tanto, es crucial implementar autenticación y autorización para verificar la identidad de los usuarios y controlar el acceso a varios canales de WebSocket.

NestJS permite la integración de estrategias de autenticación estándar, como JSON Web Token (JWT) u Open Authorization (OAuth), con WebSockets. Esto se puede lograr a través de middlewares o guards, que pueden inspeccionar el protocolo de enlace (*handshake*) o el proceso de conexión para garantizar la validez de las credenciales de un cliente.

Echa un vistazo al siguiente fragmento de código:

```typescript
@WebSocketGateway() export class AuthGateway { @SubscribeMessage('authenticate') handleAuthentication(client: any, token: string) { try { // Logic to validate the token const user = validateToken(token); client.user = user; client.emit('authenticated'); } catch (e) { client.error('Authentication failed'); } } }
```

En este ejemplo ocurre lo siguiente:

- Se utiliza un escuchador de mensajes para `'authenticate'` para procesar el token del cliente.
- La función `validateToken` es una función hipotética que validaría el token y devolvería la información del usuario.
- Tras una autenticación exitosa, el objeto de usuario se adjunta al cliente y se le notifica.

Este patrón garantiza que solo los usuarios autenticados puedan acceder e interactuar con el servidor WebSocket.

Para concluir esta sección, resumiremos los puntos clave y el papel de los patrones avanzados de WebSocket en las aplicaciones NestJS.

Los WebSockets ofrecen un medio poderoso para habilitar la comunicación bidireccional en tiempo real entre clientes y servidores. Al dominar los patrones avanzados proporcionados por NestJS, los desarrolladores pueden crear aplicaciones complejas en tiempo real con una gestión eficiente del flujo de datos, conexiones seguras y arquitecturas escalables. Esta exploración de los patrones avanzados de WebSocket en NestJS demuestra el compromiso del framework de proporcionar a los desarrolladores las herramientas necesarias para manejar la comunicación en tiempo real a escala.

Al concluir esta sección, recuerda que la comunicación WebSocket es solo una de las muchas funciones avanzadas que ofrece NestJS. Al combinar estas prácticas con los otros aspectos de NestJS, puedes crear aplicaciones robustas, eficientes y altamente interactivas.

---

### Comprensión de los proveedores en profundidad

Los proveedores son la piedra angular de la arquitectura de NestJS. Están en el corazón del sistema DI de NestJS, sirviendo como un patrón poderoso para gestionar las dependencias de clases. Esencialmente, los proveedores pueden ser cualquier cosa que pueda devolver un valor, como un servicio, una fábrica, un valor o una clase. NestJS trata a estos proveedores como ciudadanos de primera clase, lo que significa que son fundamentales para la estructura y el funcionamiento de cualquier aplicación NestJS.

En NestJS, los proveedores son responsables de varias tareas, como las siguientes:

- Definir un conjunto de funciones que pueden necesitar varios componentes de una aplicación
- Encapsular lógica compleja
- Acceder a datos de una base de datos
- Realizar operaciones computacionales

La belleza de los proveedores radica en su versatilidad y reutilización, lo que conduce a un código más limpio y eficiente.

#### Breve resumen: proveedores y módulos

Antes de profundizar en los proveedores, recapitulemos rápidamente su relación con los módulos. En NestJS, los módulos son el tapiz donde se declaran y ensamblan los proveedores. Cada módulo encapsula proveedores, creando una estructura de aplicación clara y organizada.

Los módulos en NestJS sirven como un marco organizativo que nos permite agrupar funcionalidades relacionadas. Cuando definimos un proveedor, debe estar asociado con un módulo. Esta asociación le indica al sistema DI de NestJS sobre el ámbito del proveedor y su disponibilidad para ser inyectado en otros componentes, como controladores y servicios.

#### ¿Qué constituye un proveedor en NestJS?

Los proveedores en NestJS se definen ampliamente como clases que encapsulan la lógica de negocio y están anotadas con un decorador, normalmente `@Injectable()`. Estas clases se pueden inyectar en otras clases a través del constructor, lo que permite un enfoque de desarrollo modular y comprobable.

Hay varios tipos de proveedores en NestJS:

- **Servicios (*Services*):** Clases que manejan la lógica de negocio y la recuperación de datos
- **Fábricas (*Factories*):** Funciones que devuelven una instancia de proveedor dinámicamente
- **Valores (*Values*):** Valores codificados de forma rígida que se pueden inyectar
- **Clases (*Classes*):** Cuando una clase en sí misma puede ser un proveedor

Esta flexibilidad permite a los desarrolladores elegir la herramienta adecuada para el trabajo, asegurando que los componentes de la aplicación estén débilmente acoplados y sean altamente modulares.

#### Servicios, fábricas, valores y más

El proveedor más común es un servicio. Echemos un vistazo a un proveedor de servicios simple:

```typescript
import { Injectable } from '@nestjs/common'; @Injectable() export class CatsService { findAll(): string[] { return ['Whiskers', 'Tom', 'Felix']; } }
```

En este fragmento, definimos un servicio llamado `CatsService`. Al usar el decorador `@Injectable()`, NestJS comprende que esta clase es un proveedor y se puede usar en el sistema DI. Este servicio tiene un solo método, `findAll`, que devuelve un arreglo de nombres de gatos.

#### Cómo funciona la DI en NestJS

La DI es un patrón de diseño que NestJS utiliza para proporcionar a los componentes sus dependencias en lugar de crearlas internamente. La DI permite una mayor modularidad y facilidad de prueba. En NestJS, la DI se logra mediante el uso de decoradores para "inyectar" proveedores en las clases que los requieren.

Aquí hay un ejemplo de DI en acción:

```typescript
import { Controller } from '@nestjs/common'; import { CatsService } from './cats.service'; @Controller('cats') export class CatsController { constructor(private catsService: CatsService) {} findAll() { return this.catsService.findAll(); } }
```

En la clase `CatsController`, `CatsService` se inyecta a través del constructor. El sistema en tiempo de ejecución de NestJS se encarga de crear una instancia de `CatsService` y pasarla a la clase `CatsController` cuando se instancia.

#### Inyección de proveedores en módulos, controladores y otros proveedores

Los proveedores se pueden inyectar en cualquier clase que gestione NestJS, incluidos otros proveedores, módulos y controladores. Cuando se inyecta un proveedor, NestJS busca en el árbol de proveedores para encontrar la instancia más cercana que coincida con la dependencia requerida.

#### Ámbito y ciclo de vida de los proveedores (*Scope and lifetime of providers*)

Por defecto, los proveedores en NestJS son Singletons. Esto significa que se comparte una única instancia del proveedor en toda la aplicación, lo que garantiza un uso eficiente de la memoria y una gestión de datos coherente. Aquí hay un ejemplo de esto:

```typescript
// Singleton provider, the default in NestJS @Injectable() export class SingletonService { // Service logic here }
```

Este proveedor Singleton se instancia una vez y se utiliza la misma instancia dondequiera que se inyecte el proveedor.

#### Ámbitos de petición y transitorios (*Request and transient scopes*)

Aparte del ámbito Singleton predeterminado, NestJS también ofrece ámbitos de petición (*request*) y transitorios (*transient*) para los proveedores:

- **Ámbito de petición (*Request scope*):** Se crea una nueva instancia del proveedor exclusivamente para cada petición entrante.
- **Ámbito transitorio (*Transient scope*):** Se crea una nueva instancia cada vez que se inyecta un proveedor, lo cual es útil para servicios con estado que no se comparten.

Puedes ver un ejemplo de ambos en el siguiente fragmento de código:

```typescript
// Request scoped provider @Injectable({ scope: Scope.REQUEST }) export class RequestScopedService { // Service logic here } // Transient scoped provider @Injectable({ scope: Scope.TRANSIENT }) export class TransientService { // Service logic here }
```

Los proveedores en NestJS son componentes potentes y flexibles que sirven como columna vertebral del sistema DI del framework. Promueven una estructura de aplicación limpia y modular, asegurando que los componentes sean fácilmente comprobables y mantenibles. A medida que avancemos, exploraremos los proveedores personalizados y sus configuraciones, ampliando aún más las capacidades del sistema DI de NestJS. Veremos cómo los proveedores personalizados se pueden adaptar a las necesidades específicas de la aplicación, mejorando la versatilidad de tus aplicaciones NestJS.

#### Proveedores personalizados

NestJS permite una variedad de tipos de proveedores personalizados, lo que permite a los desarrolladores adaptar la DI a sus necesidades específicas. Estos se definen en el módulo donde se utilizarán.

La clave `useFactory` te permite crear un proveedor dinámicamente. Esto es particularmente útil cuando el proceso de creación involucra cierta lógica.

Echa un vistazo al siguiente ejemplo de código:

```typescript
{ provide: 'ASYNC_CONNECTION', useFactory: async () => { const connection = await createConnection(); return connection; }, }
```

En este ejemplo, se requiere una operación asíncrona para crear una conexión a la base de datos. La función de fábrica también puede inyectar dependencias proporcionando un arreglo `inject`.

Cuando desees proporcionar una clase que se pueda instanciar, utiliza la sintaxis `useClass`.

Aquí hay un ejemplo de esto:

```typescript
{ provide: 'Connection', useClass: DatabaseConnection, }
```

Aquí, `DatabaseConnection` sería una clase que proporciona la lógica necesaria para gestionar una conexión a la base de datos.

Puedes proporcionar un valor constante o un conjunto de configuraciones utilizando `useValue`.

Aquí hay un ejemplo de esto:

```typescript
{ provide: 'CONFIG', useValue: { host: process.env.DB_HOST, }, }
```

Esto es útil para inyectar objetos de configuración o constantes que no requieren ninguna lógica de instanciación.

Con `useExisting`, puedes mapear proveedores para usar la instancia de otro proveedor ya creado.

Aquí hay un ejemplo de esto:

```typescript
{ provide: 'AliasConnection', useExisting: DatabaseConnection, }
```

Esto crea un alias `AliasConnection` que apunta a la misma instancia que `DatabaseConnection`, asegurando que solo se mantenga una instancia.

#### Proveedores asíncronos

Los proveedores asíncronos son útiles cuando algunos pasos de inicialización son asíncronos, como conexiones de base de datos o llamadas a APIs.

Echa un vistazo al siguiente ejemplo:

```typescript
{ provide: 'ASYNC_RESOURCE', useFactory: async (): Promise<ResourceType> => { const resource = await someAsyncFunction(); return resource; }, }
```

Aquí, el proveedor esperará a que `someAsyncFunction` se resuelva antes de estar disponible para la inyección.

Los escenarios prácticos incluyen la configuración de conexiones de bases de datos, la inicialización de servicios de terceros o la obtención de ajustes de configuración de un servidor remoto durante el inicio de la aplicación.

En NestJS, los proveedores pueden ser privados para un módulo a menos que se exporten explícitamente. Los proveedores privados solo se pueden inyectar en componentes que también estén dentro del mismo módulo.

#### Exportación de proveedores desde módulos

Para hacer que un proveedor esté disponible fuera de su módulo de declaración, debes exportarlo.

Aquí hay un ejemplo de esto en acción:

```typescript
@Module({ providers: [CatsService], exports: [CatsService], }) export class CatsModule {}
```

`CatsService` ahora es accesible para cualquier módulo que importe `CatsModule`.

#### Potenciadores (*Enhancers*) como un tipo especial de proveedor

Los potenciadores (*enhancers*), al igual que los interceptores, filtros, guards y pipes, se tratan como proveedores en NestJS, pero se utilizan para extender el comportamiento del framework en lugar de proporcionar datos o servicios.

Echa un vistazo al siguiente ejemplo:

```typescript
@Injectable() export class LoggingInterceptor implements NestInterceptor { intercept(context: ExecutionContext, next: CallHandler): Observable<any> { console.log('Logging...'); return next.handle(); } }
```

Aquí, `LoggingInterceptor` actúa como un proveedor que intercepta el manejo de peticiones y registra un mensaje.

Los potenciadores se registran de manera similar a los proveedores y pueden inyectar dependencias a través de sus constructores.

#### Técnicas avanzadas y mejores prácticas

Al construir tus proveedores, aquí hay un par de mejores prácticas a seguir:

- **Módulos y proveedores dinámicos:** NestJS admite módulos dinámicos que pueden crear proveedores basados en parámetros externos. Echa un vistazo al siguiente ejemplo:

```typescript
@Module({}) export class DynamicModule { static forRoot(options: DynamicModuleOptions): DynamicModule { return { module: DynamicModule, providers: createProviders(options), }; } }
```

Este módulo se puede personalizar en función de las opciones pasadas a `forRoot`.

- **Patrones de diseño:** Los desarrolladores pueden crear decoradores personalizados para simplificar la inyección de proveedores o asociar tokens con proveedores para una identificación más fácil.

Dado que hemos obtenido una comprensión profunda de qué son los proveedores, es hora de concluir todo explorando el ciclo de vida de la petición en NestJS en la siguiente sección.

---

### Comprensión del ciclo de vida de la petición (*Request lifecycle*)

Ha sido un largo viaje y hemos aprendido mucho. Antes de concluir, consolidemos nuestra comprensión del ciclo de vida de la petición. Ya hemos visto cómo una aplicación NestJS consta de varios componentes como módulos, controladores y proveedores. Pero, ¿cómo navega una petición a través de estos componentes para generar una respuesta del servidor? Para aclarar esto, hemos incluido un diagrama que lo explica de manera más efectiva.

La Figura 4.6 resume todo lo que necesitas saber sobre el ciclo de vida de la petición, desde la petición inicial del cliente hasta la respuesta del servidor. El diagrama utiliza líneas verdes para mostrar el flujo de petición normal —si todo sale según lo planeado— y líneas rojas para indicar qué sucede si ocurre algo inesperado, como un middleware que bloquea una petición o un guard que deniega el acceso. Antes de que la respuesta se envíe de vuelta al cliente, los interceptores procesan el objeto `Response` para garantizar que el cliente reciba la respuesta correcta. Dependiendo de los detalles de tu aplicación, algunos componentes pueden omitirse o pueden incluirse elementos adicionales como un filtro de excepciones o un método `catchError()` en el interceptor. El gráfico puede variar ligeramente, pero la lógica subyacente sigue siendo la misma:

*Figura 4.7: Ciclo de vida de la petición*

---

### Resumen

Al concluir el Capítulo 4, hemos viajado por el corazón de NestJS, desvelando los secretos detrás de sus características avanzadas. Hemos ascendido desde los módulos fundacionales que estructuran nuestras aplicaciones con gracia y eficiencia a través del matizado mundo de los controladores y proveedores que dan vida a nuestro código. En el camino, hemos navegado por guards y pipes esenciales que mantienen nuestras aplicaciones seguras y nuestros datos validados, y hemos adornado nuestras clases con decoradores poderosos para extender sus capacidades sin esfuerzo.

Esta inmersión profunda no solo ha ampliado nuestra comprensión teórica, sino que también nos ha armado con herramientas prácticas para esculpir aplicaciones que sean robustas, escalables y bellamente diseñadas. Con este conocimiento, nos encontramos al borde de la aplicación práctica: construir una API RESTful que aproveche todo el potencial de NestJS.

A medida que avanzamos hacia el próximo capítulo, estamos listos para aplicar estos conceptos avanzados a los desafíos del mundo real, convirtiendo nuestras aplicaciones NestJS en obras de arte técnico. ¿Listo para arremangarte y poner la teoría en acción? ¡Demos este salto juntos!
