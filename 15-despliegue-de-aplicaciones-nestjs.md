# Parte 6: Despliegue, DevOps y Más Allá

## Capítulo 15: Despliegue de Aplicaciones NestJS

Has construido tu aplicación NestJS, la has probado y te has asegurado de que funcione sin problemas. Ahora es el momento de dar el siguiente gran paso: lanzarla al mundo. Bienvenido al capítulo donde transformamos tu desarrollo local en un servicio en vivo y accesible.

En este capítulo, te guiaremos a través de los pasos esenciales para desplegar aplicaciones NestJS en diversos entornos. Ya sea que busques configurar un servidor local o desplegar en plataformas en la nube, cubriremos las configuraciones necesarias y las mejores prácticas. Desde garantizar la seguridad y la optimización del rendimiento hasta el manejo de configuraciones específicas del entorno y la monitorización, obtendrás el conocimiento necesario para lanzar tu aplicación a los usuarios finales con total confianza.

Exploraremos diferentes estrategias de despliegue, incluyendo el uso de Docker para la contenedorización y diversas plataformas en la nube, como AWS, Google Cloud y más. Además, analizaremos prácticas importantes posteriores al despliegue, como la monitorización, el escalado y la seguridad de tu aplicación en producción.

Para este capítulo, esto es lo que tenemos en agenda:

- Configuración de un servidor local para pruebas similares a producción
- Despliegue mediante Docker
- Despliegue en plataformas en la nube
- Buenas prácticas posteriores al despliegue

Al final de este capítulo, no solo serás capaz de desplegar tus aplicaciones NestJS, sino también de asegurar que sean seguras, eficientes y estén listas para el mundo real. Asegúrate de tener configuradas tus herramientas de despliegue preferidas y tus cuentas en la nube mientras nos sumergimos en el mundo del despliegue.

Comencemos este viaje para hacer que tu aplicación esté disponible para los usuarios en todas partes.

---

### Requisitos técnicos

Para este capítulo, asegúrate de tener tu entorno preparado siguiendo el [Capítulo 3](https://subscription.packtpub.com/book/web-development/9781835468609/3).

También debes consultar el [Capítulo 12](https://subscription.packtpub.com/book/web-development/9781835468609/12) para configurar tu proyecto.

---

### Configuración de un servidor local para pruebas similares a producción

Antes de pasar a la configuración real, asegurémonos de tener lista nuestra aplicación NestJS.

#### Preparación de tu aplicación NestJS para el despliegue

Antes de sumergirse en el proceso de despliegue, es fundamental contar con un proyecto concreto con el que trabajar. Para este capítulo, utilizaremos la aplicación de comercio electrónico (*e-commerce*) desarrollada en el [Capítulo 12](https://subscription.packtpub.com/book/web-development/9781835468609/12) como nuestro caso de uso de referencia. Este proyecto de API REST proporciona una base integral que se puede aplicar a otros proyectos en el futuro, lo que lo convierte en un punto de partida ideal.

Para seguir el proceso, necesitarás hacer lo siguiente:

- **Utilizar el proyecto del [Capítulo 12](https://subscription.packtpub.com/book/web-development/9781835468609/12):** Recomendamos utilizar la aplicación de comercio electrónico del [Capítulo 12](https://subscription.packtpub.com/book/web-development/9781835468609/12), ya que cubre una amplia gama de funcionalidades, incluidos endpoints de API REST, integración con bases de datos y mecanismos de autenticación. Si ya has trabajado en el [Capítulo 12](https://subscription.packtpub.com/book/web-development/9781835468609/12), asegúrate de que tu proyecto esté en funcionamiento localmente.
- **Clonar el repositorio:** Si no tienes el proyecto listo o prefieres empezar desde cero, puedes clonar el proyecto desde el repositorio de GitHub de nuestro libro. Visita el siguiente enlace para acceder al repositorio: [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch12](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS/tree/main/ch12). Asegúrate de tener todas las dependencias instaladas y de que la aplicación se esté ejecutando localmente.

Tener el proyecto configurado es crucial antes de configurarlo y prepararlo para el despliegue. Cubriremos tareas esenciales como la gestión de variables de entorno, los procesos de compilación de producción y la optimización de la aplicación para el despliegue. Si encuentras algún inconveniente al iniciar el proyecto localmente, asegúrate de revisar el [Capítulo 7](https://subscription.packtpub.com/book/web-development/9781835468609/7), donde nos equipamos con conocimientos sobre depuración y pruebas.

Una vez que estés listo con el proyecto, podemos proceder a prepararlo para las etapas de despliegue. ¡Empecemos!

Ahora, examinaremos más de cerca la configuración de un entorno de servidor local que imite un entorno de producción. Este paso es crucial para detectar problemas potenciales a tiempo y garantizar una transición fluida al desplegar en servidores de producción reales.

En esta sección, te guiaremos a través de lo siguiente:

- Configuración de un servidor local (NGINX, Apache, etc.)
- Configuración de SSL para HTTPS
- Monitorización y registro (*logging*) básicos

Al final de esta sección, dispondrás de un entorno local más robusto que reflejará de cerca una configuración de producción, mejorando la confiabilidad y el rendimiento de tu aplicación. Sumerjámonos y comencemos configurando un servidor local.

#### Configuración de un servidor local (NGINX, Apache, etc.)

Para crear un entorno similar a producción, configuraremos un servidor local utilizando NGINX. NGINX es una opción popular para servir aplicaciones, actuar como proxy inverso (*reverse proxy*) y gestionar contenido estático de manera eficiente. Comencemos:

1. **Instalar NGINX:** Sigue estos pasos para instalar NGINX localmente:

   En Ubuntu/Debian, utiliza estos comandos:
   ```bash
   sudo apt update
   sudo apt install NGINX
   ```

   En macOS, utiliza Homebrew:
   ```bash
   brew install NGINX
   ```

   En Windows, sigue esta guía para instalarlo: [https://NGINX.org/en/docs/windows.html](https://nginx.org/en/docs/windows.html)

2. **Configurar NGINX:** Con NGINX instalado en tu máquina local, es momento de configurarlo siguiendo estos sencillos pasos:

   **Abrir el archivo de configuración de NGINX**

   Utiliza el siguiente comando para abrir tu archivo de configuración de NGINX:
   ```bash
   sudo nano /etc/NGINX/sites-available/e-commerce-sample
   ```

   Para los usuarios de macOS, el archivo de configuración de NGINX se encuentra en `/usr/local/etc/NGINX/NGINX.conf` para usuarios con un procesador basado en Intel y en `/opt/homebrew/etc/NGINX/NGINX.conf` para aquellos con uno de los chips Apple Silicon. Asegúrate de estar editando el archivo correcto según tu hardware. Ten en cuenta que la configuración puede variar según tu sistema operativo. Para un sistema operativo diferente a macOS, asegúrate de seguir la guía oficial de NGINX aquí: [https://NGINX.org/en/docs/beginners_guide.html](https://nginx.org/en/docs/beginners_guide.html).

   El comando anterior abrirá el archivo de configuración en un editor. Ahora, agrega la siguiente configuración para servir tu aplicación NestJS y asegúrate de que este bloque `server` se agregue dentro del bloque `http` para usuarios de Apple:

   ```nginx
   server {
       listen 80;
       server_name localhost;

       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

   El archivo de configuración anterior le indica a nuestro servidor proxy NGINX qué puerto debe escuchar (`80`) y el bloque `location /` le indica a NGINX qué hacer con cualquier petición entrante, lo que nos permite apuntar el puerto del proxy hacia nuestra aplicación real mediante el parámetro `proxy_pass`.

   Una vez completadas la instalación y la configuración, comencemos a utilizar NGINX en los siguientes pasos.

3. **Iniciar NGINX:**

   En Ubuntu/Debian, utiliza el siguiente comando:
   ```bash
   sudo systemctl start NGINX
   ```

   En macOS, utiliza el siguiente comando:
   ```bash
   sudo NGINX
   ```

4. **Probar la configuración de NGINX:** Abre un navegador web y navega hasta [http://localhost](http://localhost/). Deberías ver tu aplicación NestJS ejecutándose con la respuesta esperada, como se muestra en la Figura 15.1.

*Figura 15.1: Servidor NGINX sirviendo nuestra aplicación NestJS*

Con NGINX configurado y tu aplicación ejecutándose en un servidor local, ahora dispones de un entorno básico similar a producción. Sin embargo, en escenarios de producción del mundo real, proteger la comunicación entre tu servidor y los usuarios es crucial. Ahí es donde entra en juego SSL.

A continuación, daremos un paso más configurando SSL para HTTPS con el fin de garantizar una comunicación cifrada y segura. Esto es particularmente importante para el manejo de datos sensibles, como información de usuarios o transacciones de pago.

#### Configuración de SSL para HTTPS

Proteger tu aplicación con HTTPS es crucial para resguardar los datos en tránsito y generar confianza en el usuario. Configuraremos un certificado SSL autofirmado para pruebas locales. Comencemos:

1. **Generar un certificado SSL autofirmado:** Para hacerlo, ejecuta los siguientes comandos en tu terminal:
   ```bash
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/NGINX-selfsigned.key -out /etc/ssl/certs/NGINX-selfsigned.crt
   ```

2. **Configurar NGINX para HTTPS:** Para hacerlo, modifica el archivo de configuración de NGINX para incluir los ajustes de SSL:
   ```nginx
   server {
       listen 80;
       listen 443 ssl;
       server_name localhost;

       ssl_certificate /etc/ssl/certs/NGINX-selfsigned.crt;
       ssl_certificate_key /etc/ssl/private/NGINX-selfsigned.key;

       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

   Para los usuarios de macOS, es posible que debas crear nuevas carpetas para almacenar tu clave; seguir este artículo te resultará de ayuda: [https://arjav-dave.medium.com/self-signed-ssl-NGINX-on-mac-part-3-ed484e7b6911](https://arjav-dave.medium.com/self-signed-ssl-NGINX-on-mac-part-3-ed484e7b6911)

3. **Reiniciar NGINX:** Reinicia NGINX para aplicar los cambios:
   ```bash
   sudo systemctl restart NGINX
   # or
   sudo NGINX -s stop && sudo NGINX
   ```

4. **Probar la configuración HTTPS:** Para hacerlo, abre un navegador web y navega hasta [https://localhost](https://localhost/). Deberías ver una advertencia sobre el certificado autofirmado, como se muestra en la Figura 15.2, la cual puedes omitir para pruebas locales.

*Figura 15.2: Inicio del servidor local con SSL habilitado*

Hemos podido iniciar nuestra aplicación utilizando NGINX localmente y configurar una conexión SSL. Ahora veamos cómo podemos monitorizar nuestra aplicación localmente, reflejando el comportamiento esperado en un entorno similar a producción.

#### Monitorización y registro básicos

La monitorización y el registro (*logging*) son esenciales para diagnosticar problemas y comprender el comportamiento de tu aplicación. Ahora configuraremos una monitorización y un registro básicos utilizando herramientas como PM2 y los registros integrados de NGINX:

1. **Instalar PM2:** PM2 es un gestor de procesos para aplicaciones Node.js que incluye funciones integradas de monitorización y registro. En tu terminal, utiliza el siguiente comando para instalar PM2 globalmente:
   ```bash
   npm install pm2 -g
   ```

2. **Iniciar tu aplicación con PM2:** Inicia tu aplicación NestJS utilizando PM2 ejecutando el siguiente comando en la raíz de tu aplicación:
   ```bash
   pm2 start dist/main.js --name nest-app
   ```
   Este comando iniciará tu aplicación NestJS, como se muestra en la Figura 15.3.

*Figura 15.3: Uso de PM2 para iniciar nuestra aplicación NestJS*

3. **Ver registros (*logs*):** PM2 proporciona registros para tu aplicación. Ejecuta el siguiente comando en la raíz de tu aplicación:
   ```bash
   pm2 logs nest-app
   ```

4. **Registros de NGINX:** NGINX registra peticiones y errores. Puedes ver estos registros ejecutando el siguiente comando en cualquier parte de tu terminal:
   ```bash
   sudo tail -f /var/log/NGINX/access.log /var/log/NGINX/error.log
   ```

5. **Monitorizar el rendimiento de la aplicación:** PM2 proporciona monitorización de rendimiento integrada, lo que te permite supervisar el estado de tu aplicación, el uso de memoria, el consumo de CPU y más en tiempo real. Esta característica es especialmente útil al administrar sistemas de producción, ayudándote a mantener la salud del sistema e identificar problemas de forma temprana.

   Para monitorizar tu aplicación, puedes utilizar el siguiente comando:
   ```bash
   pm2 monit
   ```

Has creado un entorno local robusto similar a producción configurando un servidor local con NGINX, habilitando SSL para HTTPS e implementando monitorización y registro básicos. Esta configuración te ayudará a detectar problemas potenciales a tiempo y garantizará que tu aplicación esté lista para un despliegue fluido en servidores de producción.

A continuación, pasaremos a desplegar tu aplicación en servicios en la nube, donde exploraremos diferentes plataformas y sus configuraciones específicas. ¡Sigue atento!

---

### Despliegue mediante Docker

En las secciones anteriores, configuramos un servidor local para imitar un entorno de producción. Ahora adoptaremos un enfoque más versátil al contenedorizar nuestra aplicación NestJS mediante Docker. Docker nos permite empaquetar nuestra aplicación y sus dependencias en un contenedor portátil, garantizando un comportamiento consistente en diferentes entornos, ya sea tu máquina local, un servidor de staging o producción.

En esta sección, nos centraremos en desplegar nuestra aplicación de ejemplo utilizando Docker. Recorreremos el proceso de creación de un `Dockerfile`, la construcción de una imagen Docker y su ejecución como contenedor. Si bien no cubriremos los conceptos básicos de Docker en detalle, te guiaremos hacia la documentación oficial de Docker para cualquier configuración o conocimiento previo que puedas necesitar.

Esto es lo que cubriremos:

- Creación de un Dockerfile para tu aplicación NestJS
- Construcción y ejecución de tu contenedor Docker
- Optimización de tu imagen Docker para producción

Al final de esta sección, tendrás una versión dockerizada de tu aplicación NestJS que se puede desplegar fácilmente en cualquier entorno. Comencemos con la creación del Dockerfile.

#### Creación de un Dockerfile para tu aplicación NestJS

Un Dockerfile es esencialmente un conjunto de comandos que definen cómo construir una imagen Docker. Describe la imagen base, incluye las dependencias necesarias y proporciona instrucciones para configurar y ejecutar la aplicación dentro del contenedor.

Para crear un Dockerfile, en el directorio raíz de tu proyecto NestJS, crea un nuevo archivo llamado `Dockerfile` con el siguiente contenido:

```dockerfile
# Use an official Node.js runtime as a parent image
FROM node:16-alpine

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the package.json and package-lock.json files
COPY package*.json ./

# Install dependencies
RUN npm install --production

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Expose the port the app runs on
EXPOSE 3000

# Define the command to run the application
CMD ["node", "dist/main.js"]
```

En la configuración del Dockerfile anterior, tenemos lo siguiente:

- **Imagen base:** Utilizamos la imagen `node:16-alpine`, que es una versión ligera de Node.js, lo que garantiza un tamaño de imagen Docker más pequeño
- **Directorio de trabajo:** El comando `WORKDIR` establece el directorio dentro del contenedor donde se ejecutarán los comandos
- **Copia de archivos:** `COPY package*.json ./` copia los archivos de paquetes, y `COPY . .` copia todo el proyecto al contenedor
- **Instalación de dependencias:** `RUN npm install --production` instala únicamente las dependencias de producción
- **Compilación de la aplicación:** `RUN npm run build` compila el código TypeScript a JavaScript
- **Exposición de puertos:** El comando `EXPOSE 3000` hace que la aplicación sea accesible en el puerto 3000
- **Comando de inicio:** `CMD ["node", "dist/main.js"]` define el comando para iniciar la aplicación dentro del contenedor.

Ahora, ejecutemos el contenedor.

#### Construcción y ejecución de tu contenedor Docker

Una vez que hayas creado tu Dockerfile, el siguiente paso es construir la imagen Docker y ejecutarla como un contenedor. Hagámoslo:

1. **Construir la imagen Docker:** Abre tu terminal, navega al directorio raíz de tu proyecto y ejecuta el siguiente comando:
   ```bash
   docker build -t nestjs-app .
   ```
   Este comando construye la imagen Docker y la etiqueta como `nestjs-app`.

2. **Ejecutar el contenedor Docker:** Después de construir la imagen, puedes ejecutarla utilizando lo siguiente:
   ```bash
   docker run -p 3000:3000 nestjs-app
   ```
   El flag `-p 3000:3000` mapea el puerto 3000 del contenedor al puerto 3000 de tu máquina local, haciendo que la aplicación sea accesible en [http://localhost:3000](http://localhost:3000/).

3. **Probar la aplicación:** Abre un navegador y navega a [http://localhost:3000](http://localhost:3000/) para verificar que tu aplicación se esté ejecutando dentro del contenedor Docker.

Con tu contenedor Docker ejecutándose con éxito, el siguiente paso es optimizarlo para producción, centrándose en reducir el tamaño de la imagen, mejorar la seguridad y optimizar el rendimiento. En la siguiente sección, profundizaremos en las mejores prácticas para crear una imagen Docker más ligera y eficiente, adecuada para el despliegue en entornos del mundo real.

#### Optimización de tu imagen Docker para producción

Aunque el Dockerfile que creamos funciona bien para pruebas locales, existen algunas optimizaciones que podemos realizar para un entorno de producción:

- **Utilizar construcciones multietapa (*multi-stage builds*):** Las compilaciones multietapa te permiten usar una imagen base diferente para los entornos de construcción y de tiempo de ejecución, lo que reduce el tamaño de la imagen final:

  ```dockerfile
  # Build stage
  FROM node:16-alpine AS builder
  WORKDIR /usr/src/app
  COPY package*.json ./
  RUN npm install --production
  COPY . .
  RUN npm run build

  # Production stage
  FROM node:16-alpine
  WORKDIR /usr/src/app
  COPY --from=builder /usr/src/app .
  EXPOSE 3000
  CMD ["node", "dist/main.js"]
  ```

  En el código proporcionado, primero utilizamos una etapa de construcción (`FROM node:16-alpine AS builder`) para compilar la aplicación. Esta etapa instala dependencias, copia los archivos del proyecto y ejecuta el comando de compilación. En la etapa de producción (`FROM node:16-alpine`), solo la aplicación compilada de la etapa de construcción se copia a una nueva imagen, omitiendo archivos de construcción y dependencias innecesarios, lo que optimiza el tamaño de la imagen para producción.

- **Minimizar capas:** Combina múltiples comandos `RUN` en uno solo para minimizar la cantidad de capas en la imagen, reduciendo su tamaño.
- **Consideraciones de seguridad:** Asegúrate de que la información sensible, como las variables de entorno, se gestione de forma segura utilizando secretos de Docker (*Docker secrets*) u otros métodos seguros.

Al seguir la lista anterior, ahora dispones de una aplicación NestJS dockerizada que está optimizada para el despliegue. Este contenedor se puede desplegar fácilmente en varios entornos, desde máquinas locales hasta servicios en la nube, garantizando consistencia y confiabilidad en todos los niveles.

En la siguiente sección, exploraremos el despliegue de tu aplicación dockerizada en plataformas en la nube, donde revisaremos configuraciones específicas y mejores prácticas para diversos entornos de nube. ¡Prepárate para llevar tus habilidades de despliegue al siguiente nivel!

---

### Despliegue en plataformas en la nube

Con tu aplicación NestJS ahora contenedorizada mediante Docker, el siguiente paso lógico es desplegarla en una plataforma en la nube. Las plataformas en la nube ofrecen infraestructura escalable, lo que garantiza que tu aplicación pueda manejar diferentes niveles de tráfico sin necesidad de administrar servidores físicos. En esta sección, recorreremos el despliegue de nuestra aplicación de ejemplo dockerizada en plataformas en la nube populares como AWS, Google Cloud Platform (GCP) y Microsoft Azure.

Nos centraremos en los pasos esenciales para poner tu aplicación en marcha en estas plataformas. Si bien no profundizaremos en detalles específicos de cada nube, proporcionaremos enlaces a la documentación oficial para profundizar en el aprendizaje.

Esto es lo que cubriremos:

- Despliegue en AWS Elastic Beanstalk
- Despliegue en Google Cloud Run
- Despliegue en Azure App Service

Al final de esta sección, tendrás una comprensión clara de cómo desplegar tu aplicación NestJS en múltiples entornos de nube, lo que te permitirá elegir la mejor plataforma para tus necesidades.

#### Despliegue en AWS Elastic Beanstalk

AWS Elastic Beanstalk es una plataforma como servicio (*Platform-as-a-Service* o PaaS) que simplifica el proceso de despliegue y escalado de aplicaciones web. Gestiona automáticamente los detalles de infraestructura, como el balanceo de carga, el escalado y la monitorización.

Sigue estos pasos para desplegar en AWS:

1. **Preparar tu aplicación para el despliegue:** Asegúrate de que tu aplicación dockerizada funcione localmente. Si funciona correctamente, está lista para la nube.
   Comprime en un archivo ZIP todo el directorio del proyecto, incluyendo el `Dockerfile` y otros archivos necesarios.

2. **Crear un entorno de Elastic Beanstalk:** Inicia sesión en la consola de administración de AWS ([https://aws.amazon.com/console/](https://aws.amazon.com/console/)) y navega hasta el servicio Elastic Beanstalk.
   Crea un nuevo entorno, selecciona **Web Server Environment** (Entorno de servidor web) y elige **Docker** como plataforma.
   Sube el archivo ZIP de tu aplicación cuando se te solicite.

3. **Desplegar tu aplicación:** AWS detectará automáticamente el `Dockerfile` y construirá la imagen Docker como parte del proceso de despliegue.
   Después del despliegue, AWS Elastic Beanstalk proporcionará una URL donde tu aplicación estará accesible.

#### Despliegue en Google Cloud Run

Google Cloud Run es una plataforma de procesamiento totalmente administrada que escala automáticamente tu aplicación contenedorizada. Abstrae la infraestructura, permitiéndote concentrarte únicamente en tu aplicación.

Sigue estos pasos para desplegar tu aplicación en GCP:

1. **Contenedorizar tu aplicación:** Asegúrate de que tu imagen Docker esté lista. Puedes probarla localmente con `docker run -p 3000:3000 nestjs-app`.

2. **Desplegar en Cloud Run:** Instala y configura el SDK de Google Cloud si aún no lo has hecho.

   Empuja (*push*) tu imagen Docker a Google Container Registry:
   ```bash
   gcloud auth configure-docker
   docker tag nestjs-app gcr.io/your-project-id/nestjs-app
   docker push gcr.io/your-project-id/nestjs-app
   ```

   Despliega la imagen en Cloud Run:
   ```bash
   gcloud run deploy --image gcr.io/your-project-id/nestjs-app --platform managed
   ```

   Durante el despliegue, Cloud Run proporcionará una URL donde se podrá acceder a tu aplicación.

#### Despliegue en Azure App Service

Azure App Service es una oferta PaaS de Microsoft Azure que te permite desplegar aplicaciones web sin gestionar infraestructura. Admite varios lenguajes de programación y frameworks, incluido Docker.

Sigue estas instrucciones para desplegar aplicaciones web en Azure:

1. **Preparar tu imagen Docker:** Asegúrate de que tu imagen Docker esté etiquetada y subida a un registro de contenedores como Docker Hub o Azure Container Registry.

2. **Desplegar en Azure App Service:** Inicia sesión en el portal de Azure ([https://portal.azure.com/#home](https://portal.azure.com/#home)) y crea un nuevo recurso de **Web App** (Aplicación web).
   Selecciona **Docker** como origen de despliegue y proporciona los detalles de la imagen Docker.
   Azure App Service descargará (*pull*) la imagen Docker del registro especificado y la desplegará.

3. **Acceder a tu aplicación:** Después del despliegue, Azure proporciona una URL donde tu aplicación es accesible.

---

Ahora que has visto cómo desplegar tu aplicación NestJS en diferentes plataformas en la nube, tienes la flexibilidad de elegir la que mejor se adapte a las necesidades de tu proyecto. Cada plataforma tiene sus propios puntos fuertes, así que considera factores como el costo, la escalabilidad y la facilidad de uso al tomar tu decisión. En la siguiente sección, discutiremos las mejores prácticas para administrar tus aplicaciones desplegadas, incluyendo estrategias de monitorización, registro y escalado. ¡Continuemos optimizando nuestros despliegues en la nube!

---

### Buenas prácticas posteriores al despliegue

¡Felicitaciones por desplegar tu aplicación NestJS! Sin embargo, el trabajo no se detiene aquí. Asegurar que tu aplicación continúe funcionando bien y se mantenga segura es un proceso continuo. Esta sección cubrirá las mejores prácticas esenciales posteriores al despliegue que te ayudarán a mantener, monitorizar y optimizar tu aplicación en un entorno de producción.

Esto es lo que cubriremos:

- Monitorización y registro
- Seguridad y actualizaciones
- Copias de seguridad y recuperación ante desastres
- Escalado y balanceo de carga

Al final de esta sección, tendrás una comprensión clara de los pasos necesarios para mantener tu aplicación funcionando sin problemas en producción, minimizando el tiempo de inactividad y protegiendo tus datos.

#### Monitorización y registro

La monitorización y el registro son fundamentales para comprender la salud y el rendimiento de tu aplicación en producción. Te permiten detectar problemas antes de que afecten a los usuarios y proporcionan información sobre cómo se está utilizando tu aplicación.

En un entorno similar a producción, sigue esta guía para implementar la monitorización en tu aplicación:

- **Configurar la monitorización de la aplicación:** Utiliza herramientas como Prometheus y Grafana para monitorizar métricas como el uso de CPU, el consumo de memoria y las tasas de peticiones.
  Integra soluciones de Monitorización del Rendimiento de Aplicaciones (*Application Performance Monitoring* o APM) como New Relic o Datadog para rastrear el rendimiento de la aplicación e identificar cuellos de botella.
- **Implementar el registro (*logging*):** Configura un registro estructurado utilizando librerías como Winston o Pino en tu aplicación NestJS. Asegúrate de que los registros se emitan en un formato compatible con las herramientas de gestión de logs.
  Utiliza soluciones de registro centralizado como ELK Stack (también conocido como Elasticsearch, Logstash y Kibana) o Splunk para agregar y analizar registros.

#### Seguridad y actualizaciones

Mantener la seguridad de tu aplicación es crucial en un entorno de producción. Aplicar actualizaciones y parches con regularidad, así como seguir las mejores prácticas de seguridad, puede prevenir vulnerabilidades.

- **Actualizar dependencias periódicamente:** Utiliza herramientas como `npm audit` para identificar y corregir vulnerabilidades en las dependencias de tu proyecto.
  Programa actualizaciones periódicas para tus imágenes base de Docker para garantizar que incluyan los últimos parches de seguridad.
- **Implementar mejores prácticas de seguridad:** Utiliza HTTPS para cifrar los datos en tránsito, garantizando que la información sensible, como las credenciales de los usuarios, esté protegida.
  Implementa limitación de tasa (*rate limiting*) y validación de entrada para prevenir ataques comunes como DDoS e inyección SQL.
- **Utilizar variables de entorno de forma segura:** Almacena información sensible, como claves de API y credenciales de bases de datos, en variables de entorno. Asegúrate de que estas variables no estén codificadas de forma rígida (*hardcoded*) ni expuestas en el código fuente de tu aplicación.

#### Copias de seguridad y recuperación ante desastres

Ningún sistema es inmune a los fallos, por lo que es esencial contar con un plan de copias de seguridad y recuperación ante desastres. Estos planes garantizan que tu aplicación pueda recuperarse rápidamente de problemas inesperados como la pérdida de datos, fallos del sistema o ciberataques.

Puedes hacer lo siguiente:

- **Implementar copias de seguridad periódicas:** Utiliza herramientas como `pg_dump` para PostgreSQL para crear copias de seguridad periódicas de la base de datos de tu aplicación.
  Almacena las copias de seguridad en una ubicación externa y segura utilizando servicios como Amazon S3 o Google Cloud Storage.
- **Desarrollar un plan de recuperación ante desastres:** Define Objetivos de Tiempo de Recuperación (*Recovery Time Objectives* o RTO) y Objetivos de Punto de Recuperación (*Recovery Point Objectives* o RPO) para establecer metas claras de recuperación del sistema ante un desastre.
  Prueba tu plan de recuperación ante desastres con regularidad para garantizar que las copias de seguridad se puedan restaurar de forma rápida y precisa.

#### Escalado y balanceo de carga

A medida que tu aplicación crece, es importante asegurarse de que pueda manejar el aumento de tráfico y mantener una alta disponibilidad. Las estrategias de escalado y balanceo de carga ayudan a distribuir el tráfico entre múltiples instancias de tu aplicación, evitando la sobrecarga en un solo servidor.

- **Para implementar autoescalado:** Utiliza servicios en la nube como AWS Auto Scaling, Google Cloud Autoscaler o Azure Scale Sets para ajustar automáticamente la cantidad de instancias de la aplicación según la demanda de tráfico.
- **Para configurar el balanceo de carga:** Haz lo siguiente:
  - Configura un balanceador de carga para distribuir las peticiones entrantes entre múltiples instancias de la aplicación. Las opciones populares incluyen AWS Elastic Load Balancing (ELB), NGINX y HAProxy.
  - Asegúrate de que tu balanceador de carga esté configurado para comprobaciones de estado (*health checks*) para redirigir automáticamente el tráfico lejos de las instancias no saludables.

Al implementar estas mejores prácticas posteriores al despliegue, puedes asegurarte de que tu aplicación se mantenga confiable, segura y con un alto rendimiento en un entorno de producción. La monitorización regular, las actualizaciones de seguridad y la planificación de la recuperación ante desastres son claves para mantener la salud de tu aplicación. Con tu aplicación ahora desplegada y administrada adecuadamente, estás bien equipado para manejar las exigencias de un entorno de producción.

---

### Resumen

En este capítulo, hemos dado pasos cruciales para garantizar que tu aplicación NestJS no solo esté desplegada, sino también bien preparada para las exigencias de un entorno de producción. Al configurar un servidor local para pruebas similares a producción, desplegar utilizando Docker y aprovechar las plataformas en la nube, has aprendido a desplegar tu aplicación en diferentes entornos con facilidad. También hemos cubierto las mejores prácticas esenciales posteriores al despliegue, incluyendo monitorización, registro, seguridad, copias de seguridad y escalado, para mantener y optimizar tu aplicación una vez que esté en vivo.

A medida que avanzamos, debemos centrarnos en automatizar estos procesos. En el próximo capítulo, nos sumergiremos en los pipelines de Integración Continua y Despliegue Continuo (CI/CD), lo que te permitirá optimizar tus flujos de trabajo de despliegue, garantizar la calidad del código y hacer que tus despliegues sean más rápidos y confiables. Sigue atento mientras llevamos el proceso de desarrollo de tu aplicación al siguiente nivel.

---

### Lecturas complementarias

Para profundizar en el material utilizado y mencionado en este capítulo, tómate tu tiempo para explorar la documentación y los recursos oficiales aquí:

- Estrategias de copia de seguridad en Amazon S3: [https://aws.amazon.com/backup-storage/](https://aws.amazon.com/backup-storage/)
- Documentación de copias de seguridad de PostgreSQL: [https://www.postgresql.org/docs/current/backup-dump.html](https://www.postgresql.org/docs/current/backup-dump.html)
- Documentación de npm audit: [https://docs.npmjs.com/cli/v7/commands/npm-audit](https://docs.npmjs.com/cli/v7/commands/npm-audit)
- Documentación de Prometheus: [https://prometheus.io/docs/introduction/overview/](https://prometheus.io/docs/introduction/overview/)
- Documentación de ELK Stack: [https://www.elastic.co/what-is/elk-stack](https://www.elastic.co/what-is/elk-stack)
- Documentación de Azure App Service: [https://docs.microsoft.com/en-us/azure/app-service/](https://docs.microsoft.com/en-us/azure/app-service/)
- Documentación de Google Cloud Run: [https://cloud.google.com/run/docs](https://cloud.google.com/run/docs)
- Documentación de AWS Elastic Beanstalk: [https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)
- Documentación de balanceo de carga en NGINX: [https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)
- Documentación de AWS Auto Scaling: [https://docs.aws.amazon.com/autoscaling/](https://docs.aws.amazon.com/autoscaling/)
