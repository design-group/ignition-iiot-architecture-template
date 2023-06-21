# IIOT Docker Project Template

___

## Prerequisite

Understand the process of creating docker containers using the [docker-image](https://github.com/design-group/ignition-docker).

This project assumes you have a local Traefik reverse proxy running, if not, you can set one up using this repository [traefik-proxy](https://github.com/design-group/traefik-proxy)

___

## Setup

1. Follow [this guide from Github](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template) to create a new repository from the template.
2. Create a new directory for your project and clone the repository into it.

    ```sh
    mkdir <project-name>
    cd <project-name>
    git clone https://github.com/design-group/ignition-iiot-architecture-template.git .
    ```

3. Rename the vscode workspace file to match your project name.

    ```sh
    mv ignition-project-template.code-workspace <project-name>.code-workspace
    ```

4. Review the `docker-compose.yml` file to verify the container structure is correct
5. If using a reverse proxy, go through the `docker-compose.traefik.yml` file and change all instances of `ignition-template` to your project name.
6. Review the `.gitignore` file to add any
   additional directories and contents to ignore.
7. To name the compose project that will be built, edit the `.env` file and set the `COMPOSE_PROJECT_NAME` variable to the name of 	your project.

	```sh
	COMPOSE_PROJECT_NAME=<project-name>
	```

	or if you are using traefik as a reverse proxy, set the `.env` file to:

	```sh
	COMPOSE_PATH_SEPARATOR=:
	COMPOSE_FILE=docker-compose.yml:docker-compose.traefik.yml
	COMPOSE_PROJECT_NAME=<project-name>
	```
 
8. If mounting the `workdir` volume on a non-MacOS device, make sure to create the directory first so that it is owned by the user running the container.

	```sh
	mkdir subscriber-data publisher-data
	```

9. Pull any changes to the docker image and start the container.
      
    On Mac:
    
	```sh
    docker compose pull && docker compose up -d
    ```
    
	On WSL
    
	```sh
    docker-compose pull && docker-compose up -d
    ```

10. In a web browser, access the different gateways at the following ports:	
	- Subscriber (Engine): `http://localhost:80`
	- Publisher (Transmission): `http://localhost:81`
	- Broker (Distributor): `http://localhost:82`

11. If using traefik as a proxy, access the different gateways at the following ports:
	- Subscriber (Engine): `http://subscriber-template.localtest.me`
	- Publisher (Transmission): `http://publisher-template.localtest.me`
	- Broker (Distributor): `http://broker-template.localtest.me`

___

## Gateway Config

The gateway configuration is stored in stripped gateway backups, that have all projects removed. In order to get these backups run the following command:

```sh
bash download-gateway-backups.sh
```

The backups are stored in the `backups` directory, and if configured in the compose file will automatically restore when the container is initially created. 

This repository includes the gateway backups necessary to auto-configure MQTT between the different gateways. Each backup only contains what is necessary to setup a transmitter from the `MQTT Tags` folder, and publish it through the broker and make it visible to the engine.
