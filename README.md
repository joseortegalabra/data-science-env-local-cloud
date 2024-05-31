
# Analytics environment local cloud
Repo with instructions to create a analytics enviroment to develop data science projects collaborative locally and cloud (Docker and GCP). Using JupyterLab.


## Objective
This repo shows how it can be done to have a stable and transversal analytical environment where a team of data scientists can work together and there are no version dependency problems or other compatibility problems that they may have when each one works in their own computers. In addition to being an analytical environment that is scalable in size according to the needs of the project to be addressed. In order to achieve this objective, an analytical environment is developed using Docker and JupyterLab.

Advantages:
- By having an analytical environment in a docker image, each data scientist can set up a completely isolated environment on their computer identical to the one the rest of the team has and then if they want to pass data, models, codes there will not be any compatibility problem

- Furthermore, since it is a docker image, it is possible to mount it in the cloud and be able to use that environment in a cloud virtual machine that allows it to scale according to the needs of the trained models and/or data used. This example shows how to mount it in GCP using the Compute Engine service as a virtual machine and also in a cloud run as a serveless service (with the handicap that an always-on instance would be needed to work without problems)

- As it is a docker image, the difficulty and time lies in creating a Dockerfile with stable versions of the different packages, but it is very easy to mount the image and container both locally and in the cloud

## Objective and Advantages Summary
- Collaborative work easily in identical environments
- Completely isolated and stable environments independent of the PC and whether you work locally and cloud
- Locally scalable environments such as cloud according to the needs of the project


## Content
For this project, 2 cases were developed, one focused on developing an analytical environment to work only locally and another to work both locally and in the cloud with certain advantages and disadvantages for each approach:

- **version_1_analytics_env_local**: It works only locally as a dockerfile and also a docker-compose file (which can only be used locally) is created to create the analytical environment. It has the advantage of generating persistent files both in the container and locally (It allows files modified in the container's analytical environment to also be reflected in the local folder. This way, if a problem were to occur with the container, the files would not be lost)

- **version_2_analytics_env_local_cloud**: It works both locally and in the cloud since only a dockerfile is used to create the analytical environment. This dockerfile can be saved in the cloud and with it create different virtual environments of the size required by the project. It presents a single problem in the persistence of the files, for example when working locally, the modified files in the analytical environment of the container are not reflected in the local environment and if there were a problem with the container these files would be lost (the same thing happens with the work in cloud). Therefore, working in this way makes it completely mandatory to use version control, for example git, constantly


## Extra: What is Docker - how it works

### What is Docker
Docker is a container virtualization platform that allows you to package, distribute and run applications in a consistent and portable way, facilitating the development, testing and deployment of software in different environments quickly and efficiently.

### How Docker works - key concepts
- **Imágenes de Docker**: A Docker image is a lightweight, self-contained package that includes everything needed to run an application: the code, libraries, system tools, configurations, and anything else needed to make the application work. Images are read-only and are used as templates to create containers

- **Dockerfile**: A dockerfile is a text file that contains a series of instructions that Docker uses to build a Docker image. These instructions include commands to configure the execution environment within the container, such as installing software, setting environment variables, copying files and directories, exposing ports, etc.

- **Contenedores Docker**: A Docker container is a running instance of a Docker image. It is a way of packaging and distributing applications along with all their dependencies in an isolated environment. Containers share the underlying operating system kernel and run as isolated processes, making them lightweight and portable

### How Docker works - other important concepts
- **requirements.txt**: packages to be installed

- **docker-compose.yml**: This file specifies the configuration of the application, including which containers will run, what images will be used, how they will connect to each other, etc. As mentioned above it is used only in the first example

- **dockerignore**: similar to .gitigore in git. Tells Docker which files and directories to ignore when building a Docker image. This is useful to prevent unnecessary or sensitive files from being included in the image, which can reduce image size and improve security.

### General process of working with Docker
- A Docker image is defined using a Dockerfile
- This image is built using the docker build command
- Once the image is built, it can be used to create and run containers using the docker run command
- Running containers can communicate with each other and the outside world through exposed ports and networks defined in Docker

### Docker secondary concepts
- **Docker Hub**: It is a Docker image registry that allows developers to share and distribute their Docker images. It is also a repository where you can find public Docker images created by the community, as well as private images for internal team or company use.

- **Docker Hub / jupyter images**: is a Docker image that contains the environment needed to run Jupyter Notebooks and Jupyerlab. These images include the dependencies necessary to run Data Science libraries and tools.


### Persistent disks

Using volumes in a docker-compose.yml file allows files to be synchronized between your local machine and the container, meaning that any changes you make to files on your local machine will be reflected in the container and vice versa.

For example, the volumes you configure are synchronizing specific directories and files between your local machine and the container. For example, the volume - ./src:/app/src syncs the src folder on your local machine with the src folder inside the container at the /app/src path.

Extract docker-compose:
    volumes:
      - ./src:/app/src
      - ./transversal:/app/transversal
      - ./application_default_credentials.json:/root/.config/gcloud/application_default_credentials.json
      - ./requirements.txt:/app/requirements.txt



## Transversal instructions to create analytical environment

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

- Register the dockerfile developed in the GCP service - artifact registry focused on being a repository of both docker images as well as python packages among others

- By having the dockerfile in the artifact registry, choose the option to deploy in a compute engine
     - In the firewall option, choose the option to enable http and https traffic
     - You also need to enable ports in the GCP firewall-vpc service

- Also, in the dockerfile saved in the artifact registry there are other deploy options, for example in a cloud run (serveless service that by default is not on all the time). There is also the option to deploy in kubernetes


## Future content (not currently in this repo)
- Interesting codes will be included for the work of a data scientist using GCP, such as versioning of experiments or sending training jobs
- Codes will be included to work with git, so a completely transversal environment can be created and the user clones the repo they want to work on
- Currently there are only the codes for the 2 forms of creating the analytical environment mentioned above


## Documentation
Interest links:
- Juptyter lab in docker locally: https://medium.com/technology-hits/starting-with-python-jupyter-in-docker-7d3ead8940cb

- Jupyter lab in a google cloud engine: https://medium.com/@squarefish75/host-your-jupyter-lab-using-docker-on-a-google-cloud-virtual-machine-513c3e6ec5f8

- How to see a computer engine app in my browser: https://stackoverflow.com/questions/56362196/how-to-see-my-compute-engine-app-in-my-browser

- Tutorial GCP compute engine: https://cloud.google.com/python/docs/getting-started/getting-started-on-compute-engine?hl=es-419

- (extra, maybe is useful) Redes VPC: https://cloud.google.com/vpc/docs/vpc?hl=es-419#externalhttpconnection