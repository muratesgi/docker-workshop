>>>You are assigned to design and deploy a blog with its database as microservices
in Docker. You will be using WordPress since it is the most popular Content
Management System (CMS), used by more than one-third of all the websites on the
internet. Also, the development and testing teams require the installation of both
WordPress and the database multiple times on different platforms with isolation.
Therefore, you are required to design it as a Docker Compose application and
manage it with the docker-compose CLI.

Perform the following steps to complete this activity:
1. Start by creating a directory for your docker-compose.yaml file.
2. Create a service for the database using MySQL and a volume defined in the
docker-compose.yaml file. Ensure that the MYSQL_ROOT_PASSWORD,
MYSQL_DATABASE, MYSQL_USER, and MYSQL_PASSWORD environment
variables are set.
3. Create a service for WordPress defined in the docker-compose.yaml
file. Ensure that the WordPress containers start after the database. For the
configuration of WordPress, do not forget to set the WORDPRESS_DB_HOST,
WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD, and WORDPRESS_DB_
NAME environment variables in accordance with step 2. In addition, you need to
publish its port to be able to reach it from the browser.
4. Start the Docker Compose application in detached mode.