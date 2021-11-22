#Activity 5.02: Installing the Panoramic Trekking App Using Docker Compose

You are tasked with creating a deployment of the Panoramic Trekking App using
Docker Compose. You will take advantage of the three-tier architecture of the
Panoramic Trekking App and create a three-container Docker application, with
containers for the database, the web backend, and nginx. Therefore, you will design
it as a Docker Compose application and manage it with the docker-compose CLI.
Perform the following steps to complete this activity:
1. Create a directory for your docker-compose.yaml file.
2. Create a service for the database using PostgreSQL and a volume defined in
the docker-compose.yaml file. Ensure that the POSTGRES_PASSWORD
environment variable is set to docker. In addition, you need to create a db_
data volume in docker-compose.yaml and mount it to the /var/lib/
postgresql/data/ to store the database files.
3. Create a service for the Panoramic Trekking App defined in the dockercompose.
yaml file. Ensure that you are using the packtworkshops/
the-docker-workshop:chapter5-pta-web Docker image, which is
prebuilt and ready to use from the registry. In addition, since the application
is dependent on the database, you should configure the container to start
after the database. To store the static files, create a static_data volume
in docker-compose.yaml and mount it to /service/static/.
Finally, create a service for nginx and ensure that you are using the
packtworkshops/the-docker-workshop:chapter5-pta-nginx
Docker image from the registry. Ensure that the nginx container starts after
the Panoramic Trekking App container. You also need to mount the same
static_data volume to the /service/static/ location. Do not forget
to publish nginx port 80 to 8000 to reach from the browser.
4. Start the Docker Compose application in detached mode.
5. 5. Go to the administration section of the Panoramic Trekking App in the browser
with the address http://0.0.0.0:8000/admin:

6. Access the Panoramic Trekking App at the address
http://0.0.0.0:8000/photo_viewer in the browser:

>>>
In this activity, you have created a three-tier application using Docker Compose,
with tiers for a PostgreSQL database, a backend, and a proxy service. All services
are configured and connected using Docker Compose with its networking and
storage capabilities. 
>>>