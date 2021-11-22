docker-compose up: This command creates and starts the containers
defined in the configuration.

docker-compose ps: This command lists the containers and their
status information.

docker-compose down: This command stops and removes all the resources,
including containers, networks, images, and volumes.

Docker Compose File
Multi-container applications are run and defined using the docker-compose CLI.
By convention, the default name of these files is docker-compose.yaml.

Docker Compose works with the docker-compose.yaml and
docker-compose.yml file extensions by default.

<<<<<docker-compose.yaml>>>>>
version: 3
services:

-.....

networks:

-...

volumes:

-....


• version: This section defines the syntax version for the docker-compose
file, and currently, the latest syntax version is 3.
• services: This section describes the Docker containers that will be built if
needed and will be started by docker-compose.
• networks: This section describes the networks that will be used by the services.
• volumes: This section describes the data volumes that will be mounted to the
containers in services.

The official reference documentation for Docker Compose files is available
at https://docs.docker.com/compose/compose-file/.

>>>>Configuration of Services
Cloud-native applications are expected to store their configuration in environment
variables. Environment variables are easy to change between different platforms
without source code changes. Environment variables are dynamic values that are
stored in Linux-based systems and used by applications. In other words, the
variables can be used to configure applications by changing their values.
For instance, assume your application uses a LOG_LEVEL environment variable
to configure what is logged. If you change the LOG_LEVEL environment variable
from INFO to DEBUG and restart your application, you would see more logs and be
able to troubleshoot problems more easily. In addition, you can deploy the same
application with different sets of environment variables to staging, testing, and
production. Likewise, the method of configuring services in Docker Compose is
to set environment variables for the containers.

***There are three methods of defining environment variables in Docker Compose,
with the following priority:
1. Using the Compose file
2. Using shell environment variables
3. Using the environment file

* If the environment variables do not change very often but are required by the
containers, it is better to store them in docker-compose.yaml files. 
* If there are sensitive environment variables, such as passwords, it is recommended to pass them
via shell environment variables before calling the docker-compose CLI. 
* However, if the number of the variables is high and varies between the testing, staging, or
production systems, it is easier to collect them in .env files and pass them into
docker-compose.yaml files.

In the services part of the docker-compose.yaml file, environment
variables can be defined for each service. For example, the LOG_LEVEL and
METRICS_PORT environment variables are set in the Docker Compose file
as follows for the server service:

server:
  environment:
    - LOG_LEVEL=DEBUG
    - METRICS_PORT=8444

When the values are not set for the environment variables in the
docker-compose.yaml file, it is possible to get the values from the shell
by running a docker-compose command. For instance, the HOSTNAME
environment variable for the server service will be set straight from the shell:

  server:
    environment:
      - HOSTNAME

When the shell running the docker-compose command has no value for
the HOSTNAME environment variable, the container will start with an empty
environment variable.


