# Parte 6: Despliegue, DevOps y Más Allá

## Capítulo 17: Optimización del Rendimiento en NestJS

Has construido, desplegado y automatizado tu aplicación NestJS, haciéndola robusta y escalable. Sin embargo, queda un paso crucial para garantizar que tu aplicación prospere en el mundo real: la optimización del rendimiento. ¿Con qué rapidez responde tu aplicación? ¿Con qué eficiencia maneja el aumento de tráfico? ¿Cómo minimizas el uso de recursos al tiempo que maximizas el rendimiento? Te damos la bienvenida al capítulo donde nos enfocamos en exprimir cada gota de rendimiento de tu aplicación NestJS.

En este capítulo, exploraremos una variedad de técnicas y estrategias para optimizar el rendimiento de tus aplicaciones NestJS. Desde mejoras a nivel de código hasta optimizaciones de bases de datos y estrategias eficaces de almacenamiento en caché (*caching*), aprenderás cómo hacer que tus aplicaciones sean más rápidas, eficientes y confiables. Ya sea que te estés preparando para un aumento repentino en el tráfico de usuarios o simplemente busques brindar una mejor experiencia de usuario, la optimización del rendimiento es fundamental.

Esto es lo que cubriremos en este capítulo:

- Comprensión de la optimización del rendimiento
- Técnicas de optimización de código
- Técnicas de optimización de bases de datos
- Estrategias de almacenamiento en caché para NestJS
- Perfilado y pruebas de carga
- Mejores prácticas para la optimización del rendimiento

Al final de este capítulo, tendrás un conjunto de herramientas y estrategias para garantizar que tus aplicaciones NestJS funcionen al máximo rendimiento y estén listas para satisfacer las demandas de los entornos de producción. Prepárate para sumergirte en el arte de hacer que tu aplicación no solo sea funcional, sino también excepcionalmente rápida y eficiente.

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Comprensión de la optimización del rendimiento

Antes de sumergirnos en los detalles técnicos de la optimización del rendimiento, es importante comprender por qué es relevante. La optimización del rendimiento no consiste únicamente en hacer que tu aplicación sea más rápida: se trata de crear una experiencia de usuario fluida, reducir costes y garantizar que tu aplicación pueda escalar a medida que crece tu base de usuarios.

En el contexto de una aplicación NestJS, la optimización del rendimiento involucra varios aspectos clave: reducir los tiempos de respuesta, minimizar el uso de recursos, garantizar interacciones eficientes con la base de datos y utilizar mecanismos de almacenamiento en caché de manera efectiva. También abarca escribir código limpio y eficiente que se ejecute con rapidez, así como utilizar las herramientas y técnicas adecuadas para monitorizar y optimizar tu aplicación de forma continua.

Al pensar en la optimización del rendimiento, considera las siguientes áreas clave:

- **Eficiencia de la aplicación (*Application efficiency*):** ¿Qué tan bien realiza sus tareas tu código? ¿Existen procesos redundantes que puedan simplificarse?
- **Escalabilidad (*Scalability*):** ¿Puede tu aplicación gestionar un incremento de usuarios sin que disminuya el rendimiento?
- **Utilización de recursos (*Resource utilization*):** ¿Estás utilizando los recursos del sistema como la CPU, la memoria y el ancho de banda de manera eficiente?
- **Experiencia de usuario (*User experience*):** ¿Con qué rapidez responde tu aplicación a las peticiones de los usuarios? ¿Es la experiencia fluida y consistente?

Comprender estos aspectos te ayuda a tomar decisiones informadas sobre dónde enfocar tus esfuerzos de optimización.

Al final de este capítulo, no solo comprenderás la importancia de la optimización del rendimiento, sino que también estarás equipado con estrategias para mejorar la velocidad, la eficiencia y la confiabilidad de tus aplicaciones NestJS.

Ahora que tienes una comprensión fundamental de la optimización del rendimiento, es momento de profundizar en las técnicas específicas que harán que tu aplicación NestJS funcione de manera más eficiente. En la siguiente sección, comenzaremos con las técnicas de optimización de código, donde aprenderás a escribir código más limpio, rápido y eficiente.

---

### Técnicas de optimización de código

Ahora que entendemos la importancia de la optimización del rendimiento, comencemos con una de las áreas más críticas: la optimización del código. Escribir código eficiente es fundamental para mejorar el rendimiento general de tu aplicación NestJS. Incluso pequeñas mejoras en cómo se estructura y ejecuta tu código pueden generar ganancias significativas en velocidad y uso de recursos.

En esta sección, exploraremos varias técnicas que te ayudarán a escribir código más limpio, rápido y fácil de mantener. Las siguientes técnicas no solo mejoran el rendimiento, sino que también aumentan la legibilidad y la escalabilidad de tu aplicación.

#### Manejo eficiente de datos

Para gestionar los datos de manera eficiente, es importante hacer lo siguiente:

- **Evitar cálculos innecesarios:** Minimiza el uso de operaciones costosas dentro de bucles o funciones llamadas con frecuencia. Calcula los valores una vez y reutilízalos siempre que sea posible.
- **Optimizar bucles e iteraciones:** Asegúrate de que los bucles sean lo más eficientes posible. Considera usar métodos como `.map()`, `.filter()` y `.reduce()` para manejar arreglos con un estilo de programación funcional, lo cual puede ser más eficiente y legible.
- **Limitar la transferencia de datos:** Reduce la cantidad de datos que se envían entre el servidor y el cliente. Utiliza paginación, filtrado y ordenación para garantizar que solo se recuperen y envíen los datos necesarios.
- **Programación asíncrona:** Al escribir código asíncrono, debemos evitar caer en trampas comunes con los siguientes consejos:
  - **Usa async/await con prudencia:** Implementa adecuadamente las funciones asíncronas utilizando `async`/`await` para evitar bloquear el bucle de eventos (*event loop*). NestJS, construido sobre Node.js, está diseñado para manejar operaciones asíncronas de manera eficiente, así que aprovecha esto para mejorar el rendimiento.
  - **Paralelizar tareas independientes:** Siempre que sea posible, ejecuta las tareas independientes en paralelo en lugar de secuencialmente. Esto puede reducir significativamente el tiempo de ejecución para ciertas operaciones.

#### Reducción de la complejidad de middlewares y rutas

Puedes reducir la complejidad de las rutas y del middleware con los siguientes consejos:

- **Racionalizar el middleware:** Incluye únicamente el middleware que sea estrictamente necesario para una ruta específica. El middleware innecesario puede añadir sobrecarga (*overhead*) a tus peticiones.
- **Simplificar las rutas:** Evita rutas excesivamente complejas que requieran un procesamiento significativo. Simplifica la lógica donde sea posible y divide las rutas complejas en fragmentos más pequeños y manejables.

#### Optimización de la gestión de dependencias

Al tratar con la inyección de dependencias, es importante hacer lo siguiente:

- **Minimizar las dependencias:** Incluye únicamente los paquetes y librerías necesarios. Cada dependencia se suma al tiempo de inicio y al consumo de memoria de tu aplicación.
- **Usar tree shaking:** Para el código frontend, utiliza *tree shaking* para eliminar el código no utilizado de tus paquetes (*bundles*). Esto reduce el tamaño de la aplicación y acelera los tiempos de carga.

#### Gestión de memoria

La memoria es uno de los grandes desafíos al construir aplicaciones del lado del servidor. Es importante tener en cuenta lo siguiente:

- **Evitar fugas de memoria:** Presta atención a la gestión de la memoria, especialmente en procesos de larga duración. Identifica y corrige fugas de memoria (*memory leaks*) que puedan degradar el rendimiento con el tiempo.
- **Utilizar buffers y streams:** Para procesar grandes volúmenes de datos, utiliza los *buffers* y *streams* de Node.js para procesar la información de manera más eficiente y reducir el uso de memoria.

Estas técnicas proporcionan una base sólida para optimizar el rendimiento de tu código. Recuerda que el objetivo no es solo hacer que tu código sea más rápido, sino también más limpio y mantenible, lo cual dará frutos a largo plazo a medida que tu aplicación crezca.

Con un mejor dominio de las técnicas de optimización de código, ahora estás equipado para escribir código más rápido y eficiente. Sin embargo, optimizar el rendimiento de tu aplicación no se detiene aquí. En la siguiente sección, exploraremos cómo optimizar tus interacciones con la base de datos, lo que puede tener un impacto aún más significativo en el rendimiento general de tu aplicación. Sumerjámonos en las técnicas de optimización de bases de datos y veamos cómo puedes reducir los tiempos de consulta y mejorar el manejo de datos en tu aplicación NestJS.

---

### Técnicas de optimización de bases de datos

A medida que tu aplicación NestJS escala, la eficiencia de tus interacciones con la base de datos se vuelve cada vez más crucial. Las consultas y estructuras de bases de datos mal optimizadas pueden ralentizar significativamente tu aplicación, generando cuellos de botella que afectan la experiencia del usuario. En esta sección, exploraremos varias técnicas para optimizar tus interacciones con la base de datos, garantizando que tu aplicación siga siendo rápida y receptiva a medida que crece.

La optimización de bases de datos no consiste solo en escribir consultas eficientes: se trata de comprender las estructuras de datos, la indexación y las estrategias de almacenamiento en caché que pueden reducir drásticamente los tiempos de consulta y mejorar el rendimiento general. Cubriremos una gama de técnicas, desde la indexación hasta la optimización de consultas, cada una orientada a hacer que tus interacciones con la base de datos sean lo más eficientes posible.

#### Optimización de consultas de bases de datos

Uno de los primeros pasos para optimizar tu base de datos es revisar y refinar tus consultas. Las consultas ineficientes pueden generar tiempos de respuesta lentos, especialmente cuando se trabaja con grandes conjuntos de datos. Aquí tienes algunas estrategias para optimizar tus consultas:

- **Evitar selects innecesarios (*Avoiding select \*):** Seleccionar todas las columnas de una tabla puede consumir muchos recursos, especialmente si solo necesitas unos pocos campos. Especifica solo las columnas que requieres para reducir la cantidad de datos transferidos y procesados.
- **Optimizar los joins:** Al realizar *joins* entre tablas, asegúrate de que las condiciones del *join* estén indexadas y de que las consultas estén escritas para minimizar la cantidad de datos combinados. Considera utilizar subconsultas o reestructurar el esquema de tu base de datos para reducir la necesidad de *joins* complejos.

#### Indexación

Los índices son una de las herramientas más potentes para mejorar el rendimiento de las consultas. Al crear índices en columnas que se utilizan con frecuencia en cláusulas `WHERE`, condiciones `JOIN` o sentencias `ORDER BY`, puedes reducir significativamente el tiempo que lleva recuperar los datos:

- **Claves primarias y foráneas:** Asegúrate de que las columnas de clave primaria y foránea estén indexadas, ya que se usan a menudo en comandos de *joins* y búsquedas.
- **Índices compuestos:** Si sueles consultar varias columnas juntas con frecuencia, considera el uso de índices compuestos, los cuales cubren múltiples columnas en un único índice. Esto puede optimizar aún más tus consultas.

#### Diseño del esquema de base de datos

La forma en que está estructurado el esquema de tu base de datos puede tener un profundo impacto en el rendimiento. Un esquema bien diseñado minimizará la redundancia y optimizará la recuperación de datos:

- **Normalización frente a desnormalización:** La normalización reduce la redundancia de datos al dividir tu información en tablas más pequeñas y relacionadas, lo que puede mejorar la integridad de los datos y reducir los costos de almacenamiento. Sin embargo, por razones de rendimiento, la desnormalización —donde los datos se duplican en varias tablas— a veces puede ser más eficiente al reducir el número de *joins* necesarios para consultas complejas.
- **Particionamiento:** Para tablas de gran tamaño, considera utilizar el particionamiento, el cual divide la tabla en piezas más pequeñas y manejables. Esto puede mejorar el rendimiento al limitar la cantidad de datos que deben escanearse para responder a las consultas.

#### Estrategias de almacenamiento en caché

Almacenar en caché los datos a los que se accede con frecuencia puede reducir significativamente la carga en tu base de datos. Al almacenar los resultados de consultas costosas en una caché, puedes responder a solicitudes posteriores directamente desde la caché, mejorando enormemente los tiempos de respuesta:

- **Caché de consultas (*Query caching*):** Implementa el almacenamiento en caché de consultas para guardar los resultados de las consultas utilizadas habitualmente. Herramientas como Redis se pueden integrar con tu aplicación NestJS para gestionar eficazmente los datos en caché.
- **Caché a nivel de aplicación (*Application-level caching*):** Considera almacenar datos en caché a nivel de aplicación, especialmente para datos que no cambian con frecuencia. Esto puede reducir la cantidad de llamadas a la base de datos y acelerar tu aplicación.

Al aplicar estas técnicas de optimización de base de datos, puedes asegurarte de que tu aplicación NestJS escale de manera eficiente y mantenga un alto rendimiento, incluso a medida que crece el volumen de datos. Optimizar las interacciones con tu base de datos generará tiempos de carga más rápidos, mejores experiencias de usuario y una aplicación más robusta en general.

Con una sólida comprensión de las técnicas de optimización de bases de datos, ahora estás equipado para hacer que tus operaciones de datos sean más eficientes. Sin embargo, nuestro camino hacia la excelencia en el rendimiento no termina aquí. En la siguiente sección, exploraremos las estrategias de almacenamiento en caché, las cuales pueden mejorar aún más la velocidad y la capacidad de respuesta de tu aplicación NestJS al reducir la carga sobre tu base de datos. ¡Sigamos adelante y sumerjámonos en el mundo de la caché!

---

### Estrategias de almacenamiento en caché para NestJS

En cualquier aplicación de alto rendimiento, el almacenamiento en caché es un componente crítico para optimizar la velocidad y la eficiencia. El almacenamiento en caché te permite guardar datos a los que se accede con frecuencia en memoria, reduciendo la necesidad de recuperarlos repetidamente de medios de almacenamiento más lentos, como las bases de datos. En una aplicación NestJS, aprovechar la caché de manera efectiva puede reducir significativamente los tiempos de respuesta, disminuir la carga de la base de datos y mejorar la experiencia general del usuario.

En esta sección, exploraremos diferentes estrategias de almacenamiento en caché que puedes implementar en tus aplicaciones NestJS. Desde un simple almacenamiento en caché en memoria hasta soluciones más complejas de caché distribuida, aprenderás cómo almacenar datos en caché en varios niveles de tu aplicación para maximizar el rendimiento.

#### Almacenamiento en caché en memoria con cache-manager

NestJS proporciona una integración fluida con el paquete `cache-manager`, lo que te permite implementar almacenamiento en caché en memoria de forma rápida y eficiente. Este tipo de almacenamiento en caché se guarda directamente en la memoria de la aplicación y es ideal para almacenar datos pequeños y consultados con frecuencia que cambian rara vez.

Aquí tienes cómo puedes configurar el almacenamiento en caché en memoria en una aplicación NestJS:

```typescript
import { CacheModule, Module } from '@nestjs/common';
import { APP_INTERCEPTOR } from '@nestjs/core';
import { CacheInterceptor } from '@nestjs/common';

@Module({
  imports: [
    CacheModule.register({
      ttl: 5, // seconds
      max: 100, // maximum number of items in cache
    }),
  ],
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: CacheInterceptor,
    },
  ],
})
export class AppModule {}
```

En el código anterior, tenemos lo siguiente:

- **Time-to-Live (TTL):** Establece la duración (en segundos) que un elemento en caché permanecerá en memoria antes de ser expulsado automáticamente. Ajusta el TTL según la frecuencia con la que cambian los datos.
- **Max:** Limita el número de elementos que se pueden almacenar en la caché al mismo tiempo. Esto ayuda a gestionar el uso de la memoria.

Con esta configuración, cualquier método en tu aplicación puede almacenarse en caché automáticamente simplemente añadiendo el decorador `@Cacheable()`, reduciendo drásticamente los tiempos de carga para los datos solicitados con frecuencia.

#### Almacenamiento en caché distribuido con Redis

Para aplicaciones más grandes o aquellas desplegadas en múltiples servidores, el almacenamiento en caché distribuido utilizando un servicio como Redis suele ser más apropiado. Redis es un almacén de datos en memoria que se puede utilizar como caché distribuida, garantizando que los datos almacenados en caché sean consistentes y estén disponibles en todas las instancias de tu aplicación.

Aquí tienes un ejemplo de cómo configurar Redis como almacén de caché en NestJS:

```typescript
import { CacheModule, Module } from '@nestjs/common';
import * as redisStore from 'cache-manager-redis-store';

@Module({
  imports: [
    CacheModule.register({
      store: redisStore,
      host: 'localhost',
      port: 6379,
      ttl: 600, // 10 minutes
    }),
  ],
})
export class AppModule {}
```

En el fragmento de código anterior, estos son los componentes clave:

- **Store:** Especifica el almacén de caché; en este caso, Redis.
- **Host y Port:** Configura los detalles de conexión para tu instancia de Redis.
- **TTL:** Determina cuánto tiempo persistirán los datos en caché en Redis antes de ser eliminados; ajusta esto según la naturaleza de los datos y la frecuencia de las actualizaciones.

Con Redis, tu caché se puede compartir entre múltiples instancias de tu aplicación, lo que garantiza un rendimiento constante independientemente de la infraestructura subyacente.

#### Almacenamiento en caché HTTP con @CacheKey y @CacheTTL

NestJS también proporciona decoradores como `@CacheKey()` y `@CacheTTL()` que ofrecen un control más granular sobre el almacenamiento en caché a nivel de método. Estos pueden ser particularmente útiles para almacenar en caché los resultados de llamadas a la API específicas.

Aquí tienes un ejemplo:

```typescript
import { Controller, Get } from '@nestjs/common';
import { CacheKey, CacheTTL } from '@nestjs/common';

@Controller('data')
export class DataController {
  @Get()
  @CacheKey('my-custom-key')
  @CacheTTL(300)
  findAll() {
    return this.dataService.findAll();
  }
}
```

En este fragmento de código, tenemos lo siguiente:

- **`@CacheKey`:** Asigna una clave personalizada a la respuesta almacenada en caché, lo cual resulta útil cuando necesitas almacenar datos similares bajo diferentes contextos.
- **`@CacheTTL`:** Sobrescribe el TTL por defecto, lo que te permite especificar durante cuánto tiempo debe almacenarse en caché el resultado de este método específico.

Este nivel de control garantiza que solo se almacenen en caché los datos más críticos, reduciendo la polución innecesaria de la caché y asegurando que tu caché sea lo más efectiva posible.

#### Invalidación de caché (*Cache busting*)

El almacenamiento en caché es muy potente, pero los datos obsoletos pueden ser un inconveniente importante si no se gestionan adecuadamente. La invalidación de caché (*cache busting*) se refiere al proceso de invalidar o actualizar la caché cuando los datos subyacentes cambian. En NestJS, puedes limpiar manualmente la caché cuando sea necesario utilizando el servicio `CacheManager`:

```typescript
import { CacheService } from '@nestjs/common';

@Injectable()
export class MyService {
  constructor(private cacheManager: CacheService) {}

  async updateData() {
    // Update data logic
    await this.cacheManager.del('my-custom-key');
  }
}
```

Este método elimina los datos en caché asociados con la clave especificada, garantizando que la siguiente petición recupere datos actualizados.

El uso de estrategias de invalidación de caché garantiza que tus usuarios siempre reciban información actualizada sin sacrificar los beneficios de rendimiento del almacenamiento en caché.

El almacenamiento en caché es una técnica potente que puede mejorar drásticamente el rendimiento de tu aplicación NestJS. Al utilizar caché en memoria, caché distribuida con Redis y controles de almacenamiento en caché a nivel de método, puedes asegurarte de que tu aplicación responda rápidamente, incluso bajo una gran carga. Sin embargo, recuerda gestionar tu caché de manera efectiva, utilizando técnicas como la invalidación de caché para evitar que los datos obsoletos afecten la experiencia de tus usuarios. Con estas estrategias, tu aplicación NestJS estará bien equipada para manejar un mayor tráfico y brindar una experiencia de usuario fluida y rápida.

Ahora que la caché está optimizada, sumerjámonos en el perfilado y las pruebas de carga para asegurarnos aún más de que tu aplicación pueda funcionar de manera confiable bajo diferentes niveles de demanda.

---

### Perfilado y pruebas de carga

Al proponerte optimizar el rendimiento, el primer paso es comprender dónde puede mejorar tu aplicación NestJS. El perfilado (*profiling*) y las pruebas de carga (*load testing*) te ayudan a identificar cuellos de botella, procesos ineficientes y áreas donde los esfuerzos de optimización tendrán el mayor impacto. Al examinar tu aplicación bajo diferentes cargas, puedes obtener información valiosa sobre los tiempos de respuesta, el uso de recursos y los límites de escalabilidad.

En esta sección, exploraremos herramientas y técnicas para el perfilado y las pruebas de carga, lo que te permitirá evaluar con precisión el rendimiento de tu aplicación. Aprenderás a configurar pruebas de carga para simular tráfico del mundo real, analizar métricas de rendimiento y utilizar herramientas de perfilado para profundizar en áreas específicas de tu código. Con esta base, podrás tomar decisiones fundamentadas que mejoren la velocidad, confiabilidad y escalabilidad de tu aplicación.

#### Perfilado de una aplicación NestJS

Antes de sumergirte en la optimización, es fundamental identificar qué partes de tu aplicación pueden estar ralentizando las cosas. El perfilado nos ayuda a rastrear dónde se gastan el tiempo y los recursos dentro de una aplicación, lo que nos permite realizar mejoras focalizadas. En esta sección, utilizarás el proyecto de estudio de caso de comercio electrónico del [Capítulo 12](https://subscription.packtpub.com/book/web-development/9781835468609/12) como base práctica para el perfilado. Las herramientas de perfilado te ayudarán a explorar el rendimiento de tu aplicación NestJS, especialmente cuando maneja tareas más complejas como la validación asíncrona y el *hashing* de contraseñas.

Al final de esta sección, sabrás cómo configurar herramientas de perfilado, comprender las métricas que proporcionan e identificar áreas para su optimización. Comencemos configurando un perfilador simple, analizando la ejecución del código y mejorando el rendimiento paso a paso.

##### Paso 1 – Configuración de herramientas de perfilado de Node.js

NestJS se ejecuta sobre Node.js, el cual cuenta con herramientas de perfilado integradas perfectas para inspeccionar el rendimiento de tu aplicación. Comencemos con el perfilador integrado de Node.js y exploremos cómo se puede configurar para nuestro proyecto de comercio electrónico:

```bash
$ node --prof dist/main.js
```

Este comando inicia tu aplicación NestJS con el perfilado habilitado. Genera un archivo `.log` que contiene detalles sobre las llamadas a funciones, el tiempo de ejecución y la asignación de memoria. Una vez que hayas generado el archivo `.log`, puedes utilizar la herramienta `node --prof-process` para analizarlo:

```bash
$ node --prof-process isolate-0xNNNN.log > processed.txt
```

Esta salida proporciona un desglose del tiempo de ejecución, destacando las áreas del código donde se invierte la mayor cantidad de tiempo de procesamiento. Al inspeccionar el archivo `processed.txt`, presta especial atención a las llamadas a funciones de validación personalizadas y a las operaciones de *hashing* de contraseñas, ya que a menudo consumen muchos recursos.

Al realizar el perfilado con las herramientas integradas de Node, puedes obtener una visión amplia de cómo se comporta tu aplicación en tiempo de ejecución. El archivo `.log` muestra la frecuencia de llamadas a funciones y el tiempo de ejecución, lo que ayuda a identificar "puntos calientes" (*hot spots*): áreas del código que pueden beneficiarse de la optimización.

##### Paso 2 – Perfilado de consultas de base de datos con PostgreSQL

Nuestra aplicación de comercio electrónico utiliza PostgreSQL, y las consultas ineficientes pueden provocar ralentizaciones significativas. Comienza habilitando el módulo `auto_explain` de PostgreSQL para obtener detalles sobre la ejecución de las consultas.

En tu archivo de configuración de PostgreSQL, habilita `auto_explain` y establece el parámetro `log_min_duration_statement` para especificar el umbral de tiempo a partir del cual se registran las consultas lentas (por ejemplo, 100 ms):

```text
# Enable the auto_explain module
shared_preload_libraries = 'auto_explain'
auto_explain.log_min_duration = '100ms'
```

Reinicia PostgreSQL y luego revisa los registros tras ejecutar tu aplicación, especialmente durante rutinas de validación complejas y procesos que manejan grandes volúmenes de datos.

Los registros de `auto_explain` revelarán los tiempos de consulta y sugerirán qué consultas SQL están tardando más. Dado que la aplicación de comercio electrónico tiene un alto rendimiento de datos (*throughput*), optimizar las consultas que tardan más de 100 ms puede mejorar significativamente la capacidad de respuesta de la aplicación.

##### Paso 3 – Uso de la suite Clinic.js para perfilado avanzado

Clinic.js ([https://clinicjs.org/](https://clinicjs.org/)) de NearForm proporciona información detallada sobre las aplicaciones Node.js. Incluye herramientas como Doctor, Bubbleprof y Flame para examinar cuellos de botella, las cuales pueden ser sumamente efectivas para comprender lógicas complejas. A continuación se muestra cómo comenzar a usarla:

Instala Clinic.js:

```bash
$ npm install -g clinic
```

Ejecuta Clinic Doctor:

```bash
$ clinic doctor -- node dist/main.js
```

Abre el informe `.html` generado en un navegador. Proporciona pistas visuales sobre el uso de la memoria, el tiempo de CPU y las operaciones asíncronas, lo que lo hace especialmente útil en áreas como el *hashing* de contraseñas y la validación.

Los informes visuales muestran dónde se pueden estar acumulando las tareas asíncronas, lo que podría causar retrasos. En un sistema con validación asíncrona, es posible que observes que estas funciones consumen recursos significativos, lo que indica la necesidad de simplificar o descargar trabajo.

##### Paso 4 – Análisis de puntos calientes del código

A medida que sigas los informes del perfilador, identificarás "puntos calientes" (*hot spots*) donde se puede mejorar el rendimiento. Por ejemplo, si el *hashing* de contraseñas aparece con frecuencia, considera ajustar las rondas de *hashing* para bcrypt:

```typescript
// Use lower hash rounds for bcrypt in development
const saltRounds = process.env.NODE_ENV === 'production' ? 12 : 8;
const hashedPassword = await bcrypt.hash(
  password,
  saltRounds
);
```

Este enfoque mantiene la seguridad en producción mientras ahorra recursos en los entornos de desarrollo.

El perfilado a menudo muestra áreas de alto uso donde ajustes menores pueden reducir el tiempo de procesamiento. Al ajustar las rondas de *hash* de bcrypt según el entorno, conservamos la seguridad en producción mientras aceleramos las pruebas en el desarrollo local.

Con una visión más clara del rendimiento de la aplicación a través del perfilado, el siguiente paso es probar qué tan bien puede soportar una carga creciente. Las pruebas de carga simulan el tráfico de los usuarios, ayudándonos a evaluar la solidez de las optimizaciones bajo estrés. En la siguiente sección, explicaremos cómo configurar pruebas de carga para garantizar que tu aplicación NestJS se mantenga receptiva y confiable a medida que crece la demanda.

#### Pruebas de carga de una aplicación NestJS

Tras identificar los cuellos de botella con el perfilado, el siguiente paso es evaluar la resiliencia de tu aplicación bajo un tráfico elevado mediante pruebas de carga. Las pruebas de carga simulan interacciones de usuarios del mundo real y miden qué tan bien maneja tu aplicación NestJS el tráfico concurrente pesado, ayudándote a identificar áreas de mejora en el rendimiento. Al final de esta sección, podrás configurar y analizar pruebas de carga, utilizando estos conocimientos para mejorar la capacidad y la confiabilidad de tu aplicación.

Para mantener las cosas prácticas, continuaremos utilizando el proyecto de comercio electrónico del [Capítulo 12](https://subscription.packtpub.com/book/web-development/9781835468609/12), con PostgreSQL, validación personalizada y *hashing* de contraseñas. Esta configuración es ideal para demostrar pruebas de carga bajo operaciones complejas.

##### Paso 1 – Configuración de una herramienta de pruebas de carga

Una herramienta popular para realizar pruebas de carga en aplicaciones Node.js es Artillery. Se integra perfectamente con JavaScript, ofreciendo configuraciones sencillas para definir escenarios de alta concurrencia. A continuación se explica cómo configurarla:

Instala Artillery con el siguiente comando:

```bash
$ npm install -g artillery
```

Crea un archivo de configuración de Artillery. En la raíz de tu proyecto, crea un archivo `artillery-config.yml` para configurar la duración de la prueba, la tasa de tráfico y las peticiones:

```yaml
config:
  target: "http://localhost:3000"
  phases:
    - duration: 60
      arrivalRate: 20
scenarios:
  - flow:
      - get:
          url: "/api/products"
      - post:
          url: "/api/auth/register"
          json:
            username: "user-{{ $randomString }}"
            password: "Test@1234"
```

Esta configuración envía 20 peticiones por segundo durante 60 segundos, dirigiéndose a los endpoints `/api/products` y `/api/auth/register` para simular la recuperación de productos y el registro de usuarios, probando tanto operaciones de lectura como de escritura.

##### Paso 2 – Ejecución y análisis de la prueba de carga

Ejecuta la prueba con lo siguiente:

```bash
$ artillery run artillery-config.yml
```

Los resultados en tiempo real mostrarán métricas como el tiempo de respuesta, el rendimiento (*throughput*) y las tasas de error. Aquí tienes una estructura JSON de ejemplo como referencia:

```json
{
  "scenariosCreated": 1200,
  "scenariosCompleted": 1180,
  "requestsCompleted": 2400,
  "latency": {
    "min": 50,
    "max": 800,
    "p95": 500
  },
  "errors": {
    "ECONNRESET": 5
  }
}
```

En la salida anterior tenemos:

- **Tiempo de respuesta (*Response time*):** Muestra la capacidad de respuesta del endpoint.
- **Rendimiento (*Throughput*):** Mide las peticiones exitosas por segundo.
- **Tasa de errores (*Error rate*):** Rastrea las peticiones fallidas (por ejemplo, errores `ECONNRESET` debido a sobrecarga).

Al revisar estas métricas, puedes detectar optimizaciones clave, como agregar indexación a la base de datos, almacenar en caché validaciones repetitivas o aumentar el tamaño del grupo de hilos (*thread pool size*) para tareas vinculadas a la CPU.

Tras el perfilado y las pruebas de carga, has obtenido información valiosa sobre la eficiencia de tu aplicación y las posibles áreas de mejora. Sin embargo, ¿cómo puedes asegurarte de que tu aplicación se optimice constantemente a medida que crece? En la siguiente sección, exploraremos las mejores prácticas para la optimización del rendimiento, centrándonos en principios esenciales para mantener la velocidad, la confiabilidad y la escalabilidad en entornos de producción.

---

### Mejores prácticas para la optimización del rendimiento

La optimización del rendimiento no es una tarea de una sola vez; es un proceso continuo que requiere un enfoque estratégico y una cuidadosa consideración de múltiples factores. A medida que continúas refinando y mejorando tu aplicación NestJS, adherirte a las mejores prácticas puede ayudarte a mantener una aplicación de alto rendimiento que escale eficazmente y ofrezca una experiencia de usuario superior.

En esta sección, cubriremos las mejores prácticas clave para la optimización del rendimiento en NestJS. Estas prácticas están diseñadas para garantizar que tu aplicación siga siendo rápida, eficiente y confiable a medida que evoluciona.

#### Monitorizar y realizar pruebas comparativas (*benchmarking*) con regularidad

La monitorización continua y las pruebas comparativas (*benchmarking*) son esenciales para identificar cuellos de botella en el rendimiento y garantizar que tu aplicación cumpla con los objetivos establecidos. Utiliza herramientas de monitorización como Prometheus, Grafana o Datadog para realizar un seguimiento de métricas clave de rendimiento como los tiempos de respuesta, el uso de CPU, el consumo de memoria y las tasas de peticiones.

Aquí tienes un ejemplo:

```typescript
import { Injectable, OnModuleInit } from '@nestjs/common';
import { register as promRegister, Counter } from 'prom-client';

@Injectable()
export class MetricsService implements OnModuleInit {
  private readonly httpRequestsTotal: Counter<string>;

  onModuleInit() {
    this.httpRequestsTotal = new Counter({
      name: 'http_requests_total',
      help: 'Total number of HTTP requests',
      labelNames: ['method', 'status'],
    });

    promRegister.registerMetric(this.httpRequestsTotal);
  }

  incrementHttpRequestCount(method: string, status: string) {
    this.httpRequestsTotal.inc({ method, status });
  }
}
```

Este ejemplo muestra cómo puedes utilizar Prometheus con NestJS para monitorizar el número total de peticiones HTTP, segmentadas por método y estado. Al integrar la monitorización directamente en tu aplicación, puedes realizar un seguimiento continuo de las métricas de rendimiento e identificar tendencias o problemas a tiempo.

Las pruebas comparativas periódicas deben complementar la monitorización. Herramientas como Artillery o Apache JMeter pueden simular carga en tu aplicación, ayudándote a comprender cómo se comporta bajo estrés. Esto te permite abordar de manera proactiva los problemas de rendimiento antes de que afecten a tus usuarios.

#### Optimizar la asignación de recursos

La asignación eficiente de recursos es fundamental para mantener un rendimiento óptimo. Esto implica ajustar el uso de los recursos de CPU, memoria y E/S de tu aplicación para satisfacer las demandas de tu carga de trabajo:

- **Contenedorización (*Containerization*):** Si estás utilizando Docker o Kubernetes, asegúrate de que tus contenedores tengan el tamaño adecuado y no estés comprometiendo recursos en exceso. Aprovecha herramientas como el escalador automático horizontal de pods (*Horizontal Pod Autoscaler* - HPA) de Kubernetes para escalar automáticamente tu aplicación en función de métricas en tiempo real.
- **Gestión de procesos (*Process management*):** Para aplicaciones basadas en Node.js, el uso de administradores de procesos como PM2 puede ayudarte a gestionar el uso de recursos de manera más efectiva. PM2 te permite agrupar en clústeres (*cluster*) tu aplicación a través de múltiples núcleos de CPU, equilibrando la carga y mejorando la capacidad de respuesta.

Aquí tienes un ejemplo:

```bash
$ pm2 start dist/main.js -i max --max-memory-restart 200M
```

Este comando de PM2 inicia tu aplicación NestJS en modo clúster, utilizando todos los núcleos de CPU disponibles (`-i max`). La opción `--max-memory-restart` asegura que si un proceso supera los 200 MB de memoria, se reiniciará, evitando que las fugas de memoria degraden el rendimiento.

#### Aprovechar la programación asíncrona y la E/S no bloqueante

NestJS, construido sobre Node.js, sobresale en el manejo de operaciones asíncronas y E/S no bloqueante. Maximizar estas características puede mejorar enormemente el rendimiento de tu aplicación, especialmente bajo una alta concurrencia:

- **Async/await:** Prefiere siempre `async`/`await` para manejar código asíncrono, ya que hace que tu código sea más limpio y fácil de gestionar en comparación con el uso de *callbacks* o cadenas `.then()`.
- **Operaciones no bloqueantes (*Non-blocking operations*):** Asegúrate de que las operaciones, especialmente aquellas vinculadas a E/S como leer archivos o consultar bases de datos, sean no bloqueantes. Utiliza librerías o módulos que admitan operaciones no bloqueantes para evitar que tu aplicación se ralentice por estas tareas.

Aquí tienes un ejemplo:

```typescript
import { Injectable } from '@nestjs/common';
import { readFile } from 'fs/promises';

@Injectable()
export class FileService {
  async readFileContent(filePath: string): Promise<string> {
    return await readFile(filePath, 'utf-8');
  }
}
```

Este ejemplo muestra cómo utilizar el módulo `fs/promises` para realizar una operación de lectura de archivos no bloqueante. Al evitar las operaciones sincrónicas de archivos, puedes mantener tu aplicación receptiva incluso cuando manejes archivos grandes o almacenamiento lento.

#### Minimizar el tamaño de las cargas útiles (*payloads*)

Reducir el tamaño de los datos intercambiados entre el cliente y el servidor puede mejorar significativamente los tiempos de respuesta y reducir el uso de ancho de banda. Esto implica optimizar tanto las cargas útiles de las peticiones como de las respuestas:

- **Compresión (*Compression*):** Implementa la compresión de respuestas utilizando middleware como `compression`. Esto reduce el tamaño de tus respuestas HTTP, haciendo que se transfieran más rápido a través de la red.
- **Serialización (*Serialization*):** Utiliza formatos de serialización eficientes como Protocol Buffers o MessagePack en lugar de JSON para estructuras de datos complejas, especialmente en entornos de alto rendimiento o con limitaciones de ancho de banda.

Aquí tienes un ejemplo:

```typescript
import { Module, MiddlewareConsumer } from '@nestjs/common';
import * as compression from 'compression';

@Module({
  imports: [],
})
export class AppModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(compression())
      .forRoutes('*');
  }
}
```

El ejemplo demuestra cómo utilizar el middleware `compression` en una aplicación NestJS. Esto comprime automáticamente tus respuestas HTTP, reduciendo su tamaño y mejorando los tiempos de carga.

#### Implementar un registro de eventos (*logging*) eficiente

El registro de eventos (*logging*) es esencial para la depuración y la monitorización, pero una configuración deficiente del *logging* puede afectar el rendimiento. Las prácticas de registro eficientes garantizan que captures la información necesaria sin sobrecargar tu aplicación ni perjudicar los tiempos de respuesta.

##### Niveles de registro (*Log levels*)

Establece niveles de registro adecuados (por ejemplo, `DEBUG`, `INFO`, `WARN` o `ERROR`) para controlar la verbosidad. En producción, `WARN` o `ERROR` pueden reducir el ruido y mejorar el rendimiento, mientras que `DEBUG` se puede utilizar en desarrollo para obtener una mayor visibilidad sobre el comportamiento de la aplicación.

##### Registro asíncrono (*Asynchronous logging*)

Para evitar bloquear el hilo principal, configura tus registros para que sean asíncronos. Esto se puede hacer utilizando librerías de registro de alto rendimiento como `pino`, que es ligera y adecuada para aplicaciones a gran escala.

##### Configuración de Pino en un entorno listo para producción

El siguiente ejemplo demuestra una configuración de Pino lista para producción dentro de una aplicación NestJS. Incluye un método para cambiar a `pino-pretty` para mejorar la legibilidad en entornos que no son de producción.

##### Configurar Pino en el archivo AppConfig

Añade una función `getLoggerConfig` en tu archivo `AppConfig` para configurar Pino con opciones específicas del entorno:

```typescript
import { Params } from 'nestjs-pino';
import * as crypto from 'crypto';

export class AppConfig {
  public static getLoggerConfig(): Params {
    const { NODE_ENV, LOG_LEVEL, CLUSTERING } = process.env;
    return {
      exclude: [], // Exclude specific paths from logging
                   // if needed
      pinoHttp: {
        genReqId: () => `${crypto.randomUUID()}/${
          Date.now().toString(36)
        }`, // Generate unique IDs for each request
        autoLogging: true, // Auto log HTTP
                           // requests/responses
        base: CLUSTERING === 'true' ? { pid: process.pid } : {}, // Include PID in logs if clustering
        customAttributeKeys: { responseTime: 'timeSpent' }, // Rename responseTime for clarity
        level: LOG_LEVEL || (NODE_ENV === 'production' ? 'info' : 'trace'),
        serializers: {
          req(request) {
            return {
              method: request.method,
              url: request.url,
              id: request.id,
            };
          },
          res(reply) {
            return { statusCode: reply.statusCode };
          },
        },
        transport:
          NODE_ENV !== 'production'
            ? {
                target: 'pino-pretty',
                options: {
                  translateTime: 'SYS:yyyy-mm-dd HH:MM:ss',
                },
              }
            : null,
      },
    };
  }
}
```

Esta configuración realiza lo siguiente:

- Establece los niveles de log en función del entorno (la producción usa `info` y el desarrollo usa `trace`).
- Utiliza un transporte `pino-pretty` en entornos que no son de producción para facilitar la lectura.
- Añade serializadores personalizados para `req` y `res` para controlar los atributos registrados de las peticiones y respuestas.

##### Integrar el logger en tu clase AppModule

En tu clase `AppModule`, cambia el registrador (*logger*) por defecto a Pino utilizando la configuración de `AppConfig`:

```typescript
import { Module } from '@nestjs/common';
import { LoggerModule } from 'nestjs-pino';
import { AppConfig } from './config/app.config';

@Module({
  imports: [
    LoggerModule.forRoot(AppConfig.getLoggerConfig()),
    // other modules
  ],
})
export class AppModule {}
```

Esta integración garantiza que todas las peticiones y respuestas HTTP en tu aplicación se registren de manera eficiente.

##### Uso de Logger en los servicios

Dentro de cualquier servicio, utiliza la clase `Logger` de Nest o Pino directamente para un control de registro granular:

```typescript
import { Injectable, Logger } from '@nestjs/common';

@Injectable()
export class MyService {
  private readonly logger = new Logger(MyService.name);

  logSomethingImportant() {
    this.logger.log('Important operation executed');
  }
}
```

Adherirse a las mejores prácticas para la optimización del rendimiento es fundamental para mantener una aplicación NestJS de alto rendimiento. Al monitorizar y realizar evaluaciones comparativas continuamente, optimizar la asignación de recursos, aprovechar la programación asíncrona, minimizar los tamaños de carga útil e implementar un registro eficiente, puedes garantizar que tu aplicación se mantenga rápida, escalable y resiliente. Estas prácticas te ayudarán a crear aplicaciones que no solo cumplan con los requisitos de rendimiento, sino que también brinden una experiencia de usuario excepcional a medida que crecen y evolucionan.

---

### Resumen

En este capítulo, exploramos los aspectos cruciales para optimizar tu aplicación NestJS con el fin de alcanzar el máximo rendimiento. Comenzando con la comprensión de la optimización del rendimiento, profundizamos en varias técnicas para hacer que tu aplicación sea más rápida, eficiente y escalable. Desde mejoras a nivel de código como el manejo eficiente de datos y la programación asíncrona, hasta optimizaciones de bases de datos y estrategias eficaces de almacenamiento en caché, has aprendido a mejorar la capacidad de respuesta y la confiabilidad de tu aplicación. Además, cubrimos las mejores prácticas que garantizan que tu aplicación se mantenga robusta a medida que escala, incluyendo la monitorización regular, la optimización de recursos y el registro eficiente.

Con estas herramientas y estrategias, tu aplicación NestJS ahora está mejor equipada para satisfacer las exigencias de los entornos de producción, brindando una experiencia de usuario fluida y rápida. A medida que continúes perfeccionando tu aplicación, recuerda que la optimización del rendimiento es un proceso continuo: uno que requiere atención y adaptación constantes.

En el siguiente capítulo, cambiaremos nuestro enfoque hacia otra área crítica: la seguridad en NestJS. Si bien el rendimiento garantiza que tu aplicación funcione de manera eficiente, la seguridad es fundamental para proteger tu aplicación y a sus usuarios de posibles amenazas. Prepárate para sumergirte en las mejores prácticas, técnicas y herramientas para proteger tus aplicaciones NestJS en el panorama digital.
