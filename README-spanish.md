
# Analytics environment local cloud
Repo with instructions to create a analytics enviroment to develop data science projects collaborative locally and cloud (Docker and GCP). Using JupyterLab.


## Objetivo
En esta repo se muestra cómo se puede hacer para tener un ambiente analítico estable y transversal donde puedan trabajar en conjunto un equipo de data scientist y no existan problemas de dependencias en las versiones u otros problemas de compatibilidad que puedan al tener al trabajar cada uno en sus propios computadores. Además de ser un ambiente analítico escalable en tamaño de acuerdo a las necesidades del proyecto a abordar. Para poder conseguir este objetivo se desarrolla un ambiente analítico utilizando Docker y JupyterLab.

Ventajas: 
- Al tener un ambiente analítico en una imagen de docker cada data scientist puede montar un ambiente completamente aislado en su computador idéntico al que tiene el resto del equipo y luego si se quieren pasar datos, modelos, códigos no va a haber ningun problema de compatibilidad

- Además al ser una imagen de docker, es posible montarla en cloud y poder utilizar ese ambiente en una máquina virtual de cloud que permite escalar de acuerdo a las necesidad de los modelos entrenados y/o datos utilizados. Para este ejemplo se muestra cómo montarla en GCP utilizando el servicio de Compute Engine como máquina virtual y además en un cloud run como servicio serveless (con el handicap que se necesitaría una instancia siempre encendida para trabajar sin problemas)

- Como es una imagen docker la dificultad y tiempo radica en crear un Dockerfile con versiones estables de los diferentes packages pero es muy fácil de montar la imagen y contenedor tanto localmente como en cloud

## Objetivo y Ventajas Resumen
- Trabajo colaborativo de forma fácil en ambientes idénticos
- Entornos completamente aislados y estables independiente el pc y si se trabaja local y cloud
- Entornos escalables localmente como cloud de acuerdo a las necesidad del proyecto


## Cotenido
Para este proyecto se desarrollaron 2 casos uno enfocado a desarrollar un ambiente analítico para trabajar únicamente de forma local y otro para trabajar tanto localmente como en cloud con ciertas ventajas y desventajas para cada enfoque:

- **version_1_analytics_env_local**: Funciona únicamente de forma local ya que se crea un archivo dockerfile y también un archivo docker-compose (el cual se puede utilizar únicamente localmente) para crear el ambiente analítico. Tiene la ventaja que generar archivos persistentes tanto en el contenedor como localmente (Permite que los archivos modificados en el ambiente analítico del contenedor también se vean reflejados en la carpeta local. Así si llegara a ocurrir un problema con el contenedor los archivos no se perderían)

- **version_2_analytics_env_local_cloud**: Funciona tanto localmente como cloud ya que se utiliza únicamente una archivo dockerfile para crear el ambiente analítico. Este dockerfile se pude guardar en cloud y con él crear diferentes ambiente virtuales del tamaño que requiera el proyecto. Presenta un único problema en la persistencia de los archivos, por ejemplo trabajando localmente los archivos modificados en el ambiente analítico del contenedor no se ven reflejados en el local y si hubiera un problema con el contenedor estos archivos se perderían (lo mismo pasa con el trabajo en cloud). Por lo tanto, trabajar de esta forma vuelve completamente obligatorio utilizar un control de versiones, por ejemplo git, de forma constante



## Extra: Qué es Docker - cómo funciona

### Qué es Docker
Docker es una plataforma de virtualización de contenedores que permite empaquetar, distribuir y ejecutar aplicaciones de forma consistente y portátil, facilitando el desarrollo, la prueba y la implementación de software en diferentes entornos de manera rápida y eficiente

### Cómo funciona Docker - conceptos claves
Conceptos claves:
- **Imágenes de Docker**: una imagen de Docker es un paquete ligero y autónomo que incluye todo lo necesario para ejecutar una aplicación: el código, las bibliotecas, las herramientas del sistema, las configuraciones y cualquier otra cosa que se necesite para que la aplicación funcione. Las imágenes son de solo lectura y se utilizan como plantillas para crear contenedores
- **Dockerfile**: Un dockerfile es un archivo de texto que contiene una serie de instrucciones que Docker utiliza para construir una imagen Docker. Estas instrucciones incluyen comandos para configurar el entorno de ejecución dentro del contenedor, como la instalación de software, configuración de variables de entorno, copia de archivos y directorios, exposición de puertos, etc
- **Contenedores Docker**: Un contenedor Docker es una instancia en ejecución de una imagen Docker. Es una forma de empaquetar y distribuir aplicaciones junto con todas sus dependencias en un entorno aislado. Los contenedores comparten el kernel del sistema operativo subyacente y se ejecutan como procesos aislados, lo que los hace ligeros y portátiles

### Cómo funciona Docker - otros conceptos importantes
- **requirements.txt**: packages que se van a instalar

- **docker-compose.yml**: Este archivo especifica la configuración de la aplicación, incluyendo qué contenedores se ejecutarán, qué imágenes se utilizaran, cómo se conectarán entre sí, etc. Como se menciona anteriormente se utiliza solo en el primer ejemplo

- **dockerignore**: similar a .gitigore en git. Indica a Docker qué archivos y directorios debe ignorar al construir una imagen Docker. Esto es útil para evitar que archivos innecesarios o sensibles se incluyan en la imagen, lo que puede reducir el tamaño de la imagen y mejorar la seguridad.

### Proceso general de trabajo con Docker:
- Se define una imagen Docker utilizando un Dockerfile
- Esta imagen se construye utilizando el comando docker build
- Una vez que la imagen está construida, se puede utilizar para crear y ejecutar contenedores utilizando el comando docker run
- Los contenedores en ejecución pueden comunicarse entre sí y con el mundo exterior a través de puertos expuestos y redes definidas en Docker

### Conceptos secundatios Docker
- **Docker Hub**: Es un registro de imágenes de Docker que permite a los desarrolladores compartir y distribuir sus imágenes de Docker. También es un repositorio donde se pueden encontrar imágenes de Docker públicas creadas por la comunidad, así como imágenes privadas para uso interno de equipo o empresas

- **Docker Hub / jupyter images**: es una imagen Docker que contiene el entorno necesario para ejecutar Jupyter Notebooks y Jupyerlab. Estas imágenes incluyen las dependencias necesarias para ejecutar bibliotecas y herramientas de Data Science.


### Discos persistentes

El uso de volúmenes en un archivo docker-compose.yml permite sincronizar los archivos entre tu máquina local y el contenedor, lo que significa que cualquier cambio que realices en los archivos en tu máquina local se reflejará en el contenedor y viceversa.

Porr ejemplo los volúmenes que configurados están sincronizando directorios y archivos específicos entre tu máquina local y el contenedor. Por ejemplo, el volumen - ./src:/app/src sincroniza la carpeta src de tu máquina local con la carpeta src dentro del contenedor en la ruta /app/src.

Extracto docker-compose:
    volumes:
      - ./src:/app/src
      - ./transversal:/app/transversal
      - ./application_default_credentials.json:/root/.config/gcloud/application_default_credentials.json
      - ./requirements.txt:/app/requirements.txt



## Instrucciones tranversales para crear ambiente analítico

### 1. Get Personal Service Account to use the GCP services
Note that it is your personal service account and the access to the cloud services depending of the permissions given to your account

#### - Install gcloud command line application
Follow the instructions for [installing gcloud sdk](https://cloud.google.com/sdk/docs/install) in your laptop.

#### - Open console google
Search the program "Google Cloud SDK Shell" and open it

#### - Login in gcloud
Execute `gcloud auth application-default login` and follow the browser instructions.

#### - Add default gcloud credentials to the root of this project
Copy the credentials file to the root of this project, depending of your environent, that file could be in: 
- `C:\Users\<your user>\AppData\Roaming\gcloud\application_default_credentials.json` (Windows)
- `~/.config/gcloud/legacy_credentials/<YOUR GOOGLE USERNAME>/adc.json` (Unix - Legacy)
- `~/.config/gcloud/application_default_credentials.json`
- `~/.config/gcloud/legacy_credentials/<YOUR GOOGLE USERNAME>/adc.json`

Make sure the name of your credentials json file added to the root of this project is **application_default_credentials.json**

Advice: in windows to locate the folder "appdata" you can write `%%appdata%%`


### 2. Start Docker in your local machine
Run the following instrucctions to create the Container in Docker with the analytics Environment

#### - Make sure you have the right environment variables
Save environment variables in the file `.env`

#### Open a console and navigate into the location of the files
Open a console (for example windows powershell) and navigate into the location of the files (dockerfile, docker-compose, requirements, etc)
Ex:
cd d:
cd D:\github-mi-repo\analytics-environment-local-cloud

#### Follow differents instrucctions to create the docker image and docker container
There differents instrucctions in the 2 examples developed. In a Readme.md file located in each example you can see the specific instructions. READ!!.



## Instructions to run analytics environment in your local machine
After install the docker container, it is possible run the analytics enviroment to do the data science work. Steps:

- Go to menu docker desktop in the menu `containers/apps` and select `start` in the container built

- Then go the dropdown menu and you see the specific container with the analytics environment and you see an option `Open in browser`, if you click it you will open a localhost with jupyterlab and the repo where the image was built, click it or paste it in the browser to access to jupyter Lab. `http://127.0.0.1:8888/`

- And you environment is full available to use it



## Instructions to install analytics environment in cloud - compute engine - cloud run - gcp

- Registrar el dockerfile desarrollado en servicio de GCP - artifact registry enfocado en ser un repositorio tanto de imágenes docker así como de packages de python entre otros

- Al tener el dockerfile en artifact registry elegir la opción de deploy en un compute engine
    - En la opción firewall elegir la opción de habilitar tráfico http y https
    - También es necesario habilitar los puertos en el servicio de GCP firewall-vpc

- También, en el dockerfile guardado en artifact registry existen otras opciones de deploy, por ejemplo en un cloud run (servicio serveless que por defecto no está encendido todo el tiempo). Además está la opción de deploy en kubernetes


## Contenido futuro (actualmente no está en el repo)
- Se incluirán códigos interesantes para el trabajo de un data scientist utilizando GCP, como por ejemplo versionamiento de experimentos o envio de jobs de entrenamiento
- Se incluirá códigos para trabajar con git, así se puede crear un ambiente completamente transversal y el usuario clona el repo en el que quiere trabajar
- Actualmente solo están los códigos para las 2 formas de creación de ambiente analítico mencionado anteriormente


## Documentation
Interest links:
- Juptyter lab in docker locally: https://medium.com/technology-hits/starting-with-python-jupyter-in-docker-7d3ead8940cb

- Jupyter lab in a google cloud engine: https://medium.com/@squarefish75/host-your-jupyter-lab-using-docker-on-a-google-cloud-virtual-machine-513c3e6ec5f8

- How to see a computer engine app in my browser: https://stackoverflow.com/questions/56362196/how-to-see-my-compute-engine-app-in-my-browser

- Tutorial GCP compute engine: https://cloud.google.com/python/docs/getting-started/getting-started-on-compute-engine?hl=es-419

- (extra, maybe is useful) Redes VPC: https://cloud.google.com/vpc/docs/vpc?hl=es-419#externalhttpconnection