# Parte 6: Despliegue, DevOps y Más Allá

## Capítulo 16: Adoptando DevOps: Integración Continua y Despliegue Continuo para NestJS

Has desplegado con éxito tu aplicación NestJS y la has hecho accesible en todo el mundo. Sin embargo, en un entorno de desarrollo dinámico, donde los desarrolladores de software necesitan escribir, probar y depurar código en tiempo real para cumplir con los requisitos cambiantes del proyecto, el despliegue es solo el comienzo. ¿Cómo te aseguras de que tu código esté siempre listo para producción? ¿Cómo evitas las trampas de los despliegues manuales que pueden ralentizar tu proceso de desarrollo e introducir errores? Bienvenido al capítulo donde integramos la integración continua (*Continuous Integration* - CI) y el despliegue continuo (*Continuous Deployment* - CD) en tu flujo de trabajo, automatizando y optimizando el camino desde el código hasta el despliegue.

En este capítulo, exploraremos cómo las prácticas de CI/CD pueden transformar tu proceso de desarrollo al automatizar las pruebas, la compilación y el despliegue de tu aplicación. Aprenderás sobre las herramientas para configurar pipelines de CI/CD y cómo implementarlas específicamente para tus proyectos de NestJS. Ya sea que trabajes en un equipo pequeño o en una gran empresa, la adopción de CI/CD te permitirá entregar actualizaciones más rápidamente y con mayor confianza.

Esto es lo que cubriremos en este capítulo:

- Comprensión de la integración continua y el despliegue continuo
- Elección de tus herramientas de CI/CD
- Configuración de un pipeline de CI/CD para NestJS
- Mejores prácticas de CI/CD

Al final de este capítulo, tendrás una sólida comprensión de cómo incorporar CI/CD en tus aplicaciones NestJS, asegurando que cada cambio que realices en tu base de código se pueda desplegar de forma segura y automática. Prepárate para llevar tu proceso de despliegue al siguiente nivel mientras adoptamos DevOps y construimos un flujo de trabajo optimizado y eficiente que pueda manejar la entrega continua con facilidad.

---

### Requisitos técnicos

Los archivos de código para el capítulo se pueden encontrar en [https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS](https://github.com/PacktPublishing/Scalable-Application-Development-with-NestJS)

---

### Comprendiendo la integración continua y el despliegue continuo

En esta sección, te introduciremos en el mundo de CI y CD, dos prácticas fundamentales en DevOps que ayudan a automatizar y optimizar el proceso de desarrollo. DevOps es un acrónimo de *Development* (Desarrollo) y *Operations* (Operaciones). Representa un conjunto de prácticas que unen el desarrollo de software (Dev) y las operaciones de TI (Ops) para acortar el ciclo de vida del desarrollo y proporcionar una entrega continua con alta calidad de software. En esencia, DevOps trata de derribar los silos entre los equipos de desarrollo y operaciones para mejorar la colaboración y la productividad.

Aunque este libro no se centra en DevOps, es fundamental comprender cómo CI/CD puede beneficiar a tus aplicaciones NestJS, especialmente después de haberlas desplegado en el capítulo anterior.

#### Integración continua

La integración continua (CI) se refiere al proceso automatizado de fusionar con frecuencia los cambios de código de varios colaboradores en un repositorio central a lo largo del día. El objetivo de CI es detectar errores rápidamente, lo que permite a los equipos abordar los problemas tan pronto como surgen. En el contexto de tu aplicación NestJS, CI garantiza que cualquier código nuevo añadido al proyecto se pruebe y valide automáticamente, reduciendo la probabilidad de que los errores lleguen a producción.

#### Despliegue continuo

El despliegue continuo (CD) lleva el concepto de CI un paso más allá al desplegar automáticamente en producción cada cambio que supere el proceso de CI. Esto significa que cada vez que se realiza un commit de una nueva característica o corrección de errores, se envía automáticamente a la aplicación en vivo después de pasar las pruebas. Este enfoque garantiza que la última versión de tu aplicación esté siempre disponible para los usuarios, minimizando la intervención manual.

#### ¿Por qué es importante CI/CD para tu aplicación NestJS?

En el capítulo anterior, desplegamos con éxito una aplicación NestJS. Sin embargo, realizar despliegues manualmente puede llevar mucho tiempo y ser propenso a errores. CI/CD automatiza este proceso, asegurando que tu aplicación esté siempre actualizada, probada y lista para los usuarios. Implementar CI/CD en tu proyecto conduce a ciclos de desarrollo más rápidos, lanzamientos más confiables y menos estrés para el equipo de desarrollo.

La Figura 16.1 ilustra cómo debería ser un pipeline básico de CI/CD:

*Figura 16.1 – Ejemplo de pipeline CI/CD*

El diagrama de la Figura 16.1 muestra un pipeline básico de CI/CD, donde los desarrolladores generalmente planifican primero. La planificación suele realizarse a través de reuniones de equipo o reuniones con el cliente. Luego, un responsable asigna tareas a los desarrolladores antes de que comiencen a escribir código y a probar todo localmente. A continuación, los desarrolladores deben asegurarse de que los cambios realizados no rompan ninguna funcionalidad existente; esto se hace compilando el bundle del proyecto y ejecutando una serie de pruebas. En este paso, si algo falla, ya sea que el bundle arroje un error o las pruebas (unitarias y/o de integración y/o de extremo a extremo), los desarrolladores deben corregir cualquier error que haya introducido el código anterior. Una vez que el desarrollador está seguro de sus compilaciones, necesitamos publicar los cambios para que los usuarios finales puedan comenzar a consumir la funcionalidad (si era una nueva característica) o ver el error resuelto (si era una corrección de errores). Luego debemos desplegar los cambios, generalmente en diferentes entornos, antes de comenzar a monitorizar para verificar si el cambio impacta al usuario final.

Ahora que tienes una comprensión básica de CI/CD y cómo encaja en DevOps, estás listo para explorar las herramientas que hacen posibles estas prácticas. En la siguiente sección, veremos algunas de las herramientas de CI/CD más populares disponibles y cómo se pueden integrar en tu flujo de trabajo de NestJS. ¡Sumerjámonos en las herramientas que te ayudarán a automatizar y optimizar tu proceso de despliegue!

---

### Eligiendo tus herramientas de CI/CD

Con una comprensión sólida de CI/CD, el siguiente paso es explorar las herramientas que hacen posibles estas prácticas. Las herramientas de CI/CD son aplicaciones de software que automatizan los procesos de integración y despliegue en tu flujo de trabajo de desarrollo. Estas herramientas ayudan a garantizar que tu código esté siempre en un estado desplegable, con cambios probados y desplegados automáticamente en entornos de producción. Al utilizar estas herramientas, puedes reducir los errores manuales, mejorar la colaboración y aumentar la eficiencia general de tu equipo de desarrollo.

En esta sección, te presentaremos algunas de las herramientas de CI/CD más populares que se pueden integrar en tus proyectos NestJS. Aunque este libro no se centra en una capacitación profunda en DevOps, conocer las herramientas adecuadas te ayudará a automatizar y optimizar tu proceso de despliegue, dejando más tiempo para el desarrollo real.

Pero antes de presentarte las herramientas populares de CI/CD, te daremos algunas pautas que pueden ayudarte a fundamentar tu elección de herramientas de CI/CD.

#### Criterios a considerar al seleccionar herramientas de CI/CD

Cuando se trata de elegir la herramienta de CI/CD adecuada para tu aplicación NestJS, hay varios factores a considerar. La decisión depende en gran medida de las necesidades específicas de tu proyecto, la familiaridad de tu equipo con ciertas herramientas y la escala a la que planeas operar. Aquí hay algunos criterios que pueden guiar tu elección:

- **Integración con herramientas existentes:** Si tu equipo ya está utilizando ciertos sistemas de control de versiones, como GitHub o GitLab, tiene sentido elegir herramientas de CI/CD que se integren perfectamente con ellos, como GitHub Actions o GitLab CI/CD. Esto reduce la curva de aprendizaje y permite un flujo de trabajo más optimizado.
- **Facilidad de configuración y mantenimiento:** Considera cuánto tiempo puede dedicar tu equipo a configurar y mantener el pipeline de CI/CD. Herramientas como Travis CI y GitHub Actions son conocidas por su simplicidad y facilidad de uso, lo que las hace ideales para equipos que desean comenzar rápidamente sin necesidad de gestionar configuraciones complejas.
- **Escalabilidad:** Si se espera que tu proyecto crezca significativamente en tamaño y complejidad, elegir una herramienta como Jenkins o CircleCI, que ofrece una escalabilidad robusta y características avanzadas, podría ser más apropiado. Estas herramientas pueden manejar flujos de trabajo complejos y equipos grandes, proporcionando la infraestructura necesaria para soportar el crecimiento.
- **Personalización y flexibilidad:** Algunos proyectos pueden requerir flujos de trabajo personalizados o integraciones que solo ciertas herramientas de CI/CD pueden admitir. Jenkins, por ejemplo, es altamente personalizable gracias a su extenso ecosistema de plugins, lo que lo hace adecuado para proyectos que necesitan soluciones a medida.
- **Costo y recursos:** Dependiendo de tu presupuesto, algunas herramientas pueden ser más adecuadas que otras. Para proyectos de código abierto o aquellos con un presupuesto ajustado, Travis CI ofrece servicios gratuitos, mientras que herramientas como GitHub Actions y GitLab CI/CD ofrecen niveles gratuitos con limitaciones. Es importante evaluar las implicaciones de costo de cada herramienta, especialmente si planeas escalar.
- **Soporte de la comunidad y documentación:** Una herramienta con un fuerte soporte de la comunidad y documentación exhaustiva puede ser invaluable, especialmente al solucionar problemas. Herramientas populares como Jenkins y GitHub Actions cuentan con grandes comunidades y amplios recursos, lo que puede ayudar a resolver problemas de manera rápida y eficiente.

Al evaluar estos criterios, puedes tomar una decisión informada sobre qué herramienta de CI/CD se adapta mejor a tu aplicación NestJS. Recuerda que la herramienta ideal puede variar según las particularidades de tu proyecto y las necesidades de tu equipo, así que tómate el tiempo para evaluar cada opción detenidamente.

#### Herramientas populares de CI/CD

A continuación se presentan las herramientas de CI/CD más populares que se pueden utilizar para tu aplicación NestJS:

- **Jenkins ([https://www.jenkins.io/solutions/pipeline/](https://www.jenkins.io/solutions/pipeline/)):** Jenkins es una de las herramientas de CI/CD de código abierto más utilizadas. Ofrece un amplio soporte de plugins, lo que lo hace altamente personalizable para diversos proyectos. Jenkins automatiza el proceso de compilación, prueba y despliegue de tu aplicación, asegurando que cada cambio sea validado adecuadamente antes de entrar en producción. Sin embargo, Jenkins requiere cierta configuración y mantenimiento, lo que puede ser ideal para equipos con necesidades más complejas.
- **GitHub Actions ([https://github.com/marketplace?type=actions](https://github.com/marketplace?type=actions)):** GitHub Actions es una herramienta de CI/CD integrada directamente en GitHub, lo que te permite automatizar tus flujos de trabajo desde tu propio repositorio. Es especialmente beneficiosa para proyectos ya alojados en GitHub, ya que ofrece una integración fluida y una configuración simple basada en YAML. Con GitHub Actions, puedes crear fácilmente flujos de trabajo para compilar, probar y desplegar tus aplicaciones NestJS, lo que la convierte en una excelente opción para equipos que buscan facilidad de uso.
- **GitLab CI/CD ([https://docs.gitlab.com/ee/ci/](https://docs.gitlab.com/ee/ci/)):** GitLab ofrece un pipeline de CI/CD integrado que viene con su plataforma, lo que la convierte en una herramienta potente para quienes usan GitLab para el control de versiones. GitLab CI/CD te permite crear pipelines que compilan, prueban y despliegan automáticamente tu código. Es particularmente fuerte en su capacidad para manejar proyectos a gran escala y ofrece un conjunto robusto de características para CD.
- **CircleCI ([https://circleci.com/](https://circleci.com/)):** CircleCI es otra herramienta popular de CI/CD conocida por su velocidad y escalabilidad. Ofrece una plataforma alojada en la nube que simplifica la configuración de pipelines, especialmente para equipos pequeños o startups. CircleCI es conocida por sus potentes integraciones con varios proveedores y servicios en la nube, lo que facilita el despliegue de tu aplicación NestJS en una amplia variedad de entornos.
- **Travis CI ([https://www.travis-ci.com/](https://www.travis-ci.com/)):** Travis CI es una herramienta de CI/CD fácil de usar que se integra perfectamente con GitHub. Es una excelente opción para proyectos de código abierto, ya que les ofrece servicios de CI/CD gratuitos. Travis CI es conocida por su simplicidad y facilidad de configuración, lo que la convierte en una opción excelente para proyectos más pequeños o equipos que desean ponerse en marcha rápidamente.

Aunque esta sección te presentó algunas de las herramientas de CI/CD más populares, dominar estas herramientas requiere práctica e investigación adicional. Cada herramienta tiene sus puntos fuertes y casos de uso, así que considera experimentar con ellas para ver cuál se adapta mejor a tu flujo de trabajo. La documentación oficial, los foros comunitarios y los tutoriales son excelentes recursos para profundizar tu comprensión y desarrollar tu experiencia en CI/CD.

Ahora que te has familiarizado con varias herramientas de CI/CD, es hora de poner en práctica este conocimiento. En la siguiente sección, te guiaremos a través de la configuración de un pipeline de CI/CD para tu aplicación NestJS. Esto reunirá todo lo que hemos discutido hasta ahora, ayudándote a automatizar y optimizar tu proceso de despliegue. ¡Avancemos y pongámonos manos a la obra con la configuración de tu primer pipeline de CI/CD!

---

### Configuración de un pipeline de CI/CD para NestJS

En esta sección, te guiaremos paso a paso a través del proceso de configuración de un pipeline de CI/CD adaptado para una aplicación NestJS. En concreto, utilizaremos GitHub Actions, una potente herramienta que se integra directamente con tu repositorio de GitHub para automatizar las pruebas, la compilación y el despliegue de tu aplicación. Ya sea que estés gestionando un proyecto pequeño o desplegando en un entorno de staging, la automatización de estos pasos garantiza despliegues consistentes y sin errores, así como ciclos de desarrollo más rápidos.

El pipeline de CI/CD consta de varias fases. Recorramos los pasos específicos involucrados en la configuración del pipeline:

1. **Cambios de código y push de código:** Los desarrolladores hacen push de sus cambios de código a una rama específica (por ejemplo, `feature/*`, `bugfix/*` o `hotfix/*`).
2. **Pull request y pruebas automáticas:** Cuando se crea una pull request (una propuesta para fusionar un conjunto de cambios de una rama a otra), activamos dos acciones clave:
   - **Compilación del código:** Usamos `yarn build` para compilar la aplicación.
   - **Ejecución de pruebas:** Se ejecutan tanto las pruebas unitarias como las de extremo a extremo para detectar cualquier error de forma temprana.
3. **Retroalimentación automática:** Si la compilación o las pruebas fallan, se notifica inmediatamente a los desarrolladores, permitiéndoles corregir los problemas antes de que se realice la revisión de código.
4. **Despliegue en staging:** Después de superar todas las verificaciones, el código se fusiona en la rama de desarrollo. Esto desencadena automáticamente un despliegue en un entorno de staging; para fines de demostración, utilizaremos Render ([https://render.com/](https://render.com/)), una plataforma gratuita de aplicaciones en la nube. Esto permite que el equipo de Aseguramiento de Calidad (*Quality Assurance* - QA) pruebe la aplicación antes de pasarla a producción.
5. **Despliegue en producción:** Una vez que los cambios se aprueban en staging, el pipeline despliega el código en el entorno de producción automáticamente.

Ahora que tienes una visión general de cómo es un pipeline de CI/CD, escribamos uno en la siguiente sección.

#### Escribiendo acciones de CI/CD en GitHub

Entremos ahora en los detalles de implementación. Utilizaremos GitHub Actions, que es una herramienta gratuita y fácil de comenzar a usar.

#### Configuración de GitHub Actions

Para comenzar, necesitamos definir las acciones que manejan la compilación y las pruebas. Colocaremos estas configuraciones en un directorio `.github/workflows/` y crearemos un archivo `build.yaml` de la siguiente manera:

```yaml
name: Build and Test Pipeline
on:
  pull_request:
    branches: ['develop']
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '14'
      - run: yarn install
      - run: yarn build
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '14'
      - run: yarn install
      - run: yarn test
```

Esta configuración hace lo siguiente:

- **Se activa en pull request:** El pipeline se ejecuta cuando se crea una pull request en la rama `develop`.
- **Trabajo de compilación (*build*):** El paso de compilación garantiza que el proyecto compile ejecutando `yarn build`.
- **Trabajo de pruebas (*test*):** El paso de prueba ejecuta las pruebas unitarias y de extremo a extremo del proyecto mediante `yarn test`.

#### Manejo de variables de entorno

En la mayoría de las aplicaciones, las variables de entorno (por ejemplo, conexiones a bases de datos y claves API) desempeñan un papel crucial tanto en las pruebas como en el despliegue. Para evitar fallos debidos a la falta de variables de entorno, puedes hacer cualquiera de las siguientes opciones:

1. Inyectar variables de entorno directamente en los secretos del repositorio de GitHub (en **Settings | Secrets and variables | Actions**). Luego, haz clic en el botón **New repository secret**, como se muestra en la Figura 16.2, y completa el formulario con el nombre del secreto y su valor.

*Figura 16.2 – Añadiendo un nuevo secreto de repositorio*

2. Usar valores predeterminados de respaldo (*fallback*) en tu código para entornos de prueba, como se muestra aquí:

```typescript
{ secret: this.configService.get<string>('JWT_SECRET') || 'testSecret', }
```

Esto garantiza que, incluso sin variables de entorno inyectadas, las pruebas puedan ejecutarse con éxito.

Dado que estas variables secretas no están codificadas de forma rígida (*hardcoded*) en la base de código, cuando se ejecuta el pipeline de CI/CD, utilizará los valores inyectados directamente en la sección de secretos del repositorio, que es el comportamiento deseado (en lugar de exponerlos en la base de código). Ahora, veamos cómo podemos desplegar nuestros cambios de código en la siguiente sección.

#### Despliegue en staging

Una vez que pasan las acciones de CI, el siguiente paso es desplegar la aplicación en un entorno de staging. Aquí tienes un ejemplo de cómo puedes configurar el despliegue en Render creando un archivo `deploy.yaml`:

```yaml
name: Deploy to Render
on:
  push:
    branches: ['develop']
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Render Deployment
        uses: sws2apps/render-deployment@main
        with:
          serviceId: ${{ secrets.RENDER_SERVICE_ID }}
          apiKey: ${{ secrets.RENDER_API_KEY }}
```

Aquí, estamos apuntando a las ramas `develop` y `main` para los entornos de staging y producción, respectivamente. Deberás inyectar tus claves API de Render y el ID de servicio como secretos en el repositorio, tal como hicimos en la sección anterior.

Para un entorno de producción, aquí tenemos una versión más completa del archivo YAML que podemos activar cada vez que se fusiona algún código en la rama `main`:

```yaml
name: Deploy on Production Server
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18.x'
      - name: Install dependencies
        run: npm ci --ignore-scripts
      - name: Build application
        run: npm run build
      - name: Copy files to Production server
        uses: appleboy/scp-action@v0.1.4
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USERNAME }}
          key: ${{ secrets.PROD_KEY }}
          port: ${{ secrets.PROD_PORT }}
          source: 'dist/, package.json, package-lock.json, ecosystem.config.json, .npmrc, .npmignore'
          target: ${{secrets.PROD_LOCATION}}
      - name: SSH into Production and install dependencies and restart app
        uses: appleboy/ssh-action@v0.1.10
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USERNAME }}
          key: ${{ secrets.PROD_KEY }}
          port: ${{ secrets.PROD_PORT }}
          script: |
            cd ${{secrets.PROD_LOCATION}}
            bash -ic 'npm ci --ignore-scripts --only=production && npm cache clean --force'
            bash -ic 'pm2 restart ecosystem.config.json'
```

Este flujo de trabajo de GitHub Actions automatiza el proceso de despliegue en un servidor de producción cada vez que se hace push de código a la rama `main`, optimizando las actualizaciones con una mínima intervención manual. Para configurar esto, guarda el siguiente archivo YAML bajo `.github/workflows` (por ejemplo, `deploy-production.yml`) en tu repositorio.

El flujo de trabajo realiza varias tareas críticas:

- Hace checkout del código más reciente.
- Configura Node.js para que coincida con el entorno especificado en tu proyecto.
- Instala dependencias, compila la aplicación y transfiere de forma segura los archivos de compilación al servidor de producción.
- Finalmente, se conecta por SSH (*Secure Shell*) al entorno de producción, instala las dependencias específicas de producción, limpia la caché y reinicia la aplicación mediante PM2.

Esta configuración garantiza que tu aplicación se despliegue de manera consistente y confiable, reduciendo los errores de despliegue y simplificando el proceso de lanzamiento.

---

### Mejores prácticas de CI/CD

A medida que comienzas a integrar CI/CD en tus proyectos NestJS, es crucial adherirse a las mejores prácticas que garanticen que tu pipeline sea eficiente, confiable y escalable. Implementar estas prácticas puede mejorar significativamente la calidad de tu software, reducir los tiempos de despliegue y minimizar el riesgo de que surjan problemas en producción.

Las siguientes son algunas de las mejores prácticas más importantes para tu pipeline de CI/CD:

- **Integración con control de versiones:** En el corazón de cualquier pipeline de CI/CD se encuentra un sistema de control de versiones (*Version Control System* - VCS) robusto, como Git. Asegúrate de que tus procesos de CI/CD estén estrechamente integrados con tu VCS. Esto permite la activación automática de los procesos de compilación y prueba cada vez que se hace commit de cambios en el código, lo que ayuda a detectar problemas en etapas tempranas del ciclo de desarrollo. Considera el uso de estrategias de ramificación como Gitflow o desarrollo basado en el tronco (*trunk-based development*) para gestionar la integración de funcionalidades y los lanzamientos sin contratiempos.
- **Pruebas automatizadas:** Las pruebas automatizadas son esenciales para mantener la calidad del código. Implementa una variedad de pruebas dentro de tu pipeline, incluyendo pruebas unitarias, pruebas de integración y pruebas de extremo a extremo. Cada cambio de código debe activar estas pruebas automáticamente y las compilaciones solo deben aprobarse si todas las pruebas tienen éxito. Además, considera usar herramientas de cobertura de pruebas para asegurarte de que una parte significativa de tu base de código esté probada.
- **Verificaciones continuas de calidad de código:** Incorpora herramientas para el análisis estático de código, como ESLint para JavaScript/TypeScript y SonarQube para evaluaciones más amplias de calidad de código. Estas herramientas pueden comprobar automáticamente tu código en busca de posibles problemas, *code smells* y vulnerabilidades de seguridad. Integrar estas comprobaciones en tu pipeline de CI garantiza que la calidad del código se mantenga alta y consistente en todo el proyecto.
- **Paridad de entornos:** Para evitar el problema de "en mi máquina funciona", busca la paridad de entornos en el desarrollo, staging y producción. Herramientas como Docker y Kubernetes pueden ayudar a lograr esto al asegurar que tu aplicación se ejecute de manera consistente en diferentes entornos. Tu pipeline de CI/CD debe desplegar en un entorno de staging que refleje la producción lo más fielmente posible, lo que te permite detectar posibles problemas de despliegue antes de que lleguen a producción.
- **Despliegues incrementales y frecuentes:** Los despliegues frecuentes y más pequeños son generalmente más seguros y fáciles de gestionar que los despliegues grandes e infrecuentes. Al integrar y desplegar código continuamente, reduces el riesgo de conflictos y facilitas la identificación del origen de cualquier problema que surja. La implementación de *feature toggles* (conmutadores de funcionalidad) también puede ayudarte a desplegar nuevas características de forma segura sin exponerlas a los usuarios de inmediato.
- **Monitorización y registro (*logging*):** Una vez que tu aplicación está desplegada, la monitorización y el registro continuos son fundamentales. Integra herramientas de monitorización como Prometheus, Grafana o New Relic en tu pipeline de CI/CD para rastrear el estado de tu aplicación y de la infraestructura. Los logs deben agregarse y monitorizarse en tiempo real, con alertas configuradas para notificar al equipo sobre cualquier problema crítico. Este enfoque proactivo garantiza que puedas responder rápidamente a cualquier problema en producción.
- **Estrategias de reversión (*rollback*):** No importa cuán bien pruebes tu código, aún pueden surgir problemas en producción. Tu pipeline de CI/CD debe incluir una estrategia de *rollback* que te permita revertir rápidamente a una versión estable anterior de tu aplicación si un despliegue falla. Esto podría implicar mantener los últimos artefactos de despliegue en buen estado conocidos o usar el control de versiones para revertir fácilmente los cambios de código.
- **Consideraciones de seguridad:** La seguridad debe ser una parte integral de tu pipeline de CI/CD. Incorpora herramientas de escaneo de seguridad que verifiquen vulnerabilidades en tu código, dependencias e imágenes de contenedores. Además, asegúrate de que tus procesos de despliegue sean seguros gestionando adecuadamente los secretos, utilizando protocolos de comunicación seguros y revisando periódicamente los controles de acceso.
- **Documentación y transparencia:** Mantener una documentación clara y actualizada de tus procesos de CI/CD es importante para la colaboración del equipo y la incorporación de nuevos miembros (*onboarding*). Esto incluye documentar la configuración del pipeline, los procesos de despliegue y cualquier script o configuración personalizada utilizada. Además, asegúrate de que los registros de compilación y despliegue sean fácilmente accesibles y transparentes para todo el equipo, promoviendo una cultura de responsabilidad compartida.
- **Mejora continua:** CI/CD no es un proceso que se configura y se olvida. Revisa y mejora continuamente tu pipeline en función de la retroalimentación, nuevas herramientas y los requisitos cambiantes del proyecto. Recopila periódicamente comentarios de tu equipo, analiza las métricas de despliegue y mantente al día con las mejores prácticas de la industria para garantizar que tus procesos de CI/CD sigan siendo eficientes y efectivos.

Al seguir estas mejores prácticas, puedes construir un pipeline de CI/CD que no solo acelere tu proceso de desarrollo, sino que también garantice que tus aplicaciones NestJS sean de la más alta calidad, seguras y estén listas para producción. A medida que avances, recuerda que la clave para una implementación exitosa de CI/CD es el aprendizaje y la adaptación continuos.

---

### Resumen

En este capítulo, has dado los primeros pasos en el mundo de DevOps explorando CI/CD y cómo se aplican a tus aplicaciones NestJS. Comenzamos comprendiendo los conceptos centrales de CI/CD y por qué son críticos en el desarrollo de software moderno. Aprendiste sobre las diversas herramientas de CI/CD, desde Jenkins y GitLab CI/CD hasta CircleCI, cada una ofreciendo características únicas para ayudar a optimizar tu flujo de trabajo de desarrollo.

Luego pasamos a configurar un pipeline de CI/CD adaptado para NestJS, centrándonos en automatizar las pruebas, la compilación y el despliegue de tus aplicaciones. Se enfatizaron las mejores prácticas, incluida la integración con el control de versiones, las pruebas automatizadas y la paridad de entornos, para garantizar que tu pipeline de CI/CD sea robusto y confiable.

A medida que avances, recuerda que dominar CI/CD es un proceso continuo. Las prácticas y herramientas presentadas aquí son solo el comienzo. La mejora continua y la adaptación son clave para mantener tu pipeline alineado con las necesidades cambiantes de tus proyectos.

En el próximo capítulo, profundizaremos en técnicas avanzadas de CI/CD, incluyendo la optimización del rendimiento del pipeline, la integración con plataformas DevOps basadas en la nube y la implementación de sistemas de monitorización y alerta a gran escala. ¡Prepárate para elevar tus habilidades de DevOps al siguiente nivel!
