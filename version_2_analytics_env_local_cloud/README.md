
# Analytics env local and cloud

It works both locally and in the cloud since only a dockerfile is used to create the analytical environment. This dockerfile can be saved in the cloud and with it create different virtual environments of the size required by the project. It presents a single problem in the persistence of the files, for example when working locally, the modified files in the analytical environment of the container are not reflected in the local environment and if there were a problem with the container these files would be lost (the same thing happens with the work in cloud). Therefore, working in this way makes it completely mandatory to use version control, for example git, constantly



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

### Observations when creating the docker image and docker container
- By running these codes and according to the dockerfile, an image is created with all the contents of the folder where the dockerfile is. Also, there is a dockerignore, so content created in an image ignores everything in the docker ignore

- Creating the image from the dockerfile takes time, but creating the container is very fast


### In the docker run command, the -d and -p options have the following meanings

-d: This option is used to run the container in the background ("detached" mode). This means that the container will run in the background and free up the current terminal so you can continue using the command line while the container is running.

-p: This option is used to map ports between the container and the host. You specify the host port followed by : and the container port. For example, if a service in the container is listening on port 8888 and you want to access it from port 5000 on your local machine, you can use: docker run -p 5000:8888 name_of_the_image


### Persistent disks - synchronized container and local
In theory, there is an option to have local files synchronized with those of the docker container. However this applies only to files that are not copied from the dockerfile. This is done by adding volumes in the container run command `-v /ruta/local/src:/app/src`

For example:
`docker run -d -p 8888:8888 -v /ruta/local/src:/app/src -v /ruta/local/transversal:/app/transversal -v /ruta/local/requirements.txt:/app/requirements.txt --name name_of_container name_of_the_image`


### Use VOLUME in dockerfile
In Docker, it is not recommended to use the VOLUME command inside the Dockerfile in that way to mount volumes from the host. The reason is that when VOLUME is used in the Dockerfile, Docker creates a new anonymous volume at runtime and mounts the specified folder on that volume. This means that existing data in the folder inside the container is copied to the anonymous volume and hidden, so changes to files on the anonymous volume will not be reflected in the host folder.