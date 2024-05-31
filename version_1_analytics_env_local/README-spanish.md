
# Analytics env local

Funciona únicamente de forma local ya que se crea un archivo dockerfile y también un archivo docker-compose (el cual se puede utilizar únicamente localmente) para crear el ambiente analítico. Tiene la ventaja que generar archivos persistentes tanto en el contenedor como localmente (Permite que los archivos modificados en el ambiente analítico del contenedor también se vean reflejados en la carpeta local. Así si llegara a ocurrir un problema con el contenedor los archivos no se perderían)



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
Run the command in a console  to create the docker container with the analytics environment

`docker-compose up`

**Obs: If it is the first time that you run the codes, internally docker creates the image and then mounth the container.** Create the images take time, but already you have created the image, create the container is fast

#### - Start Docker in your local machine
You will see in your terminal the address of the jupyter lab environment. Click it or paste it in the browser to access to jupyter Lab. `http://127.0.0.1:8888/`


## Importan considerations in docker environment

- **It is neccesary copy/paste a series of files to create the analytics environment (dockerfile, dockercompose, dockerignore, requirements) and the docker analytics environment works only in the repo with this files are located and the container was created**. So, for each project that need a different repository it is necessary build the environment.

    - It is usefull because it helps to have a controllated environment for each project

    - An also, it should be a problem is the team have always worked with other environments for example anaconda where it is not necesary have this kind of files in the repository

    - It is important considering this to define a polytics of repositorys that will fit to the team