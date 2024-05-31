
# Analytics env local and cloud

Funciona tanto localmente como cloud ya que se utiliza únicamente una archivo dockerfile para crear el ambiente analítico. Este dockerfile se pude guardar en cloud y con él crear diferentes ambiente virtuales del tamaño que requiera el proyecto. Presenta un único problema en la persistencia de los archivos, por ejemplo trabajando localmente los archivos modificados en el ambiente analítico del contenedor no se ven reflejados en el local y si hubiera un problema con el contenedor estos archivos se perderían (lo mismo pasa con el trabajo en cloud). Por lo tanto, trabajar de esta forma vuelve completamente obligatorio utilizar un control de versiones, por ejemplo git, de forma constante



## Instrucciones para crear ambiente analítico

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

#### - Create docker image and docker container
Run the command in a console to create a docker image using the dockerfile

`docker build -t name_of_the_image .`

For example:
`docker build -t analytics-env-ds .`

#### - Create docker container
Run the command in a console to create a docker container using the docker image

`docker run -d -p 8888:8888 --name name_of_container name_of_the_image`

For example:
`docker run -d -p 8888:8888 --name version_2_analytics_env_local_cloud analytics-env-ds`

#### - Start Docker in your local machine
You will see in your terminal the address of the jupyter lab environment. Click it or paste it in the browser to access to jupyter Lab. `http://127.0.0.1:8888/`



## Observations

### Observaciones al crear el docker image y docker container:
- Al correr estos códigos y de acuerdo al dockerfile se crea una imagen con todo el contenido de la carpeta donde está el dockerfile. Además, existe un dockerignore, por lo que el contenido que se crea en una imagen omite todo lo que está en el docker ignore

- Crear la imagen a partir del dockerfile toma tiempo, pero crear el container es muy rápido


### En el comando docker run, las opciones -d y -p tienen los siguientes significados:

-d: Esta opción se utiliza para ejecutar el contenedor en segundo plano (modo "detached"). Esto significa que el contenedor se ejecutará en segundo plano y liberará la terminal actual para que puedas seguir utilizando la línea de comandos mientras el contenedor está en ejecución. 

-p: Esta opción se utiliza para mapear puertos entre el contenedor y el host. Especificas el puerto del host seguido por : y el puerto del contenedor. Por ejemplo, si un servicio en el contenedor está escuchando en el puerto 8888 y quieres acceder a él desde el puerto 5000 en tu máquina local, puedes usar: docker run -p 5000:8888 name_of_the_image


### Discos persistentes - sincronizados container y local
Existe en teoría una opción para tener sincronizados los archivos locales con los del docker container. Sin embargo esto aplica solo a archivos que no se copien desde el dockerfile. Eso se hace agregando volumenes en el comando de ejecución del container `-v /ruta/local/src:/app/src`

Por ejemplo:
`docker run -d -p 8888:8888 -v /ruta/local/src:/app/src -v /ruta/local/transversal:/app/transversal -v /ruta/local/requirements.txt:/app/requirements.txt --name name_of_container name_of_the_image`


### Utilizar VOLUME en dockerfile
En Docker, no se recomienda utilizar el comando VOLUME dentro del Dockerfile de esa manera para montar volúmenes desde el host. El motivo es que cuando se utiliza VOLUME en el Dockerfile, Docker crea un nuevo volumen anónimo en tiempo de ejecución y monta la carpeta especificada en ese volumen. Esto significa que los datos existentes en la carpeta dentro del contenedor se copian en el volumen anónimo y se ocultan, por lo que los cambios en los archivos en el volumen anónimo no se reflejarán en la carpeta del host.