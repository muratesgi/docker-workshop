# docker-workshop

Managing Docker Containers

These basic commands are outlined as follows:
• docker pull: This command downloads a container image to the local cache
• docker stop: This command stops a running container instance
• docker start: This command starts a container instance that is no longer in a running state
• docker restart: This command restarts a running container
• docker attach: This command allows users to gain access (or attach) to the primary process of a running Docker container
  instance
• docker exec: This command executes a command inside a running container
• docker rm: This command deletes a stopped container
• docker rmi: This command deletes a container image
• docker inspect: This command shows verbose details about the state of a container

$ docker run -d ubuntu:18.04

Notice that this time we are using the docker run command with the -d flag.
This tells Docker to run the container in daemon mode (or in the background).
If we omit the -d flag, the container will take over our current terminal until the
primary process inside the container terminates.

$ docker run -i -t -d --name ubuntu1 ubuntu:18.04

Run the docker run command again, passing in the -i flag to make
the session interactive (expecting user input), and the -t flag to allocate a
pseudo-tty handler to the container. pseudo-tty handler will essentially link
the user's terminal to the interactive Bash shell running inside the container.
This will allow Bash to run properly since it will instruct the container to run in an
interactive mode, expecting user input. You can also give the container a humanreadable
name by passing in the --name flag.

$docker exec -it ubuntu1 /bin/bash

You now have an Ubuntu container up and running. You can run commands
inside this container using the docker exec command. Run the exec
command to access a Bash shell, which will allow us to run commands inside the
container. Similar to docker run, pass in the -i and -t flags to make it an
interactive session. Also pass in the name or ID of the container, so that Docker
knows which container you are targeting. The final argument of docker exec
is always the command you wish to execute. In this case, it will be /bin/bash
to start a Bash shell inside the container instance

Note
To streamline the process of cleaning up your environment, Docker provides
a prune command that will automatically remove old containers and
base images:
$ docker system prune -fa
Executing this command will remove any container images that are not tied
to an existing running container, along with any other resources in your
Docker environment.

# Getting Started with Dockerfiles

A Docker image consists of multiple layers, each layer representing the commands
provided in the Dockerfile. These read-only layers are stacked on top of one
another to create the final Docker image. Docker images can be stored in a Docker
registry, such as Docker Hub, which is a place where you can store and distribute
Docker images.
A Docker container is a running instance of the Docker image. One or more Docker
containers can be created from a single Docker image using the docker run (or
docker container run) command. Once a Docker container is created from the
Docker image, a new writeable layer will be added on top of the read-only layers from
the Docker image.

What Is a Dockerfile?

A Dockerfile is a text file that contains instructions on how to create a Docker
image. These commands are known as directives. A Dockerfile is a mechanism
that we use to create a custom Docker image as per our requirements.
The format of a Dockerfile is as follows:

# This is a comment

DIRECTIVE argument

A Dockerfile can contain multiple lines of comments and directives. These lines
will be executed in order by the Docker Engine while building the Docker image.
Like programming languages, a Dockerfile can also contain comments.

All statements starting with the # symbol will be treated as a comment. Currently,
Dockerfiles only support single-line comments. If you wish you write a multi-line
comment, you need to add the # symbol at the beginning of each line.
However, unlike most programming languages, instructions within the Dockerfile
are not case-sensitive. Even though the DIRECTIVE is case-insensitive, it is a best
practice to write all directives in uppercase to distinguish them from arguments.

>>>Common Directives in Dockerfiles
As discussed in the previous section, a directive is a command that is used to
create a Docker image. In this section, we will be discussing the following five
Dockerfile directives:

1. The FROM directive
2. The LABEL directive
3. The RUN directive
4. The CMD directive
5. The ENTRYPOINT directive

>>>The FROM Directive
A Dockerfile usually starts with the FROM directive. This is used to specify the
parent image of our custom Docker image. The parent image is the starting point of
our custom Docker image. All the customization that we do will be applied on top
of the parent image. The parent image can be an image from Docker Hub, such as
Ubuntu, CentOS, Nginx, and MySQL. The FROM directive takes a valid image name
and a tag as arguments. If the tag is not specified, the latest tag will be used.

A FROM directive has the following format:
FROM <image>:<tag>

Additionally, we can use the base image if we need to build a Docker image from
scratch. The base image, known as the scratch image, is an empty image mostly
used to build other parent images.
In the following FROM directive, we are using the scratch image to build our custom
Docker image from scratch:

FROM scratch

>>>The LABEL Directive
A LABEL is a key-value pair that can be used to add metadata to a Docker image.
These labels can be used to organize the Docker images properly. An example
would be to add the name of the author of the Dockerfile or the version of
the Dockerfile.
A LABEL directive has the following format:

LABEL <key>=<value>

A Dockerfile can have multiple labels, adhering to the preceding key-value format:

LABEL maintainer=sathsara@mydomain.com
LABEL version=1.0
LABEL environment=dev

Or these labels can be included on a single line separated by spaces:

LABEL maintainer=sathsara@mydomain.com version=1.0 environment=dev

Labels on an existing Docker image can be viewed with the docker image
inspect command.

The output should be like the following on running the docker image inspect
<image>:<tag> command:
...
...
"Labels": {
    "environment": "dev",
    "maintainer": "sathsara@mydomain.com",
    "version": "1.0"
}
...
...
As shown here, the docker image inspect command will output the key-value pairs
configured in the Dockerfile using the LABEL directive.

>>>The RUN Directive
The RUN directive is used to execute commands during the image build time. This will
create a new layer on top of the existing layer, execute the specified command, and
commit the results to the newly created layer. The RUN directive can be used to install
the required packages, update the packages, create users and groups, and so on.
The RUN directive takes the following format:

RUN <command>

<command> specifies the shell command you want to execute as part of the image
build process. A Dockerfile can have multiple RUN directives adhering to the
preceding format.

In the following example, we are running two commands on top of the parent image.
The apt-get update is used to update the package repositories, and apt-get
install nginx -y is used to install the Nginx package:

RUN apt-get update
RUN apt-get install nginx -y

Alternatively, you can add multiple shell commands to a single RUN directive by
separating them with the && symbol. In the following example, we have used the
same two commands, but this time in a single RUN directive, separated by an
&& symbol:

RUN apt-get update && apt-get install nginx -y

>>>The CMD Directive
A Docker container is normally expected to run one process. A CMD directive is used
to provide this default initialization command that will be executed when a container
is created from the Docker image. A Dockerfile can execute only one CMD
directive. If there is more than one CMD directive in the Dockerfile, Docker
will execute only the last one.

The CMD directive has the following format:

CMD ["executable","param1","param2","param3", ...]

For example, use the following command to echo "Hello World" as the output of a
Docker container:

CMD ["echo","Hello World"]

The preceding CMD directive will produce the following output when we run the
Docker container with the docker container run <image> command
(replace <image> with the name of the Docker image):

$ docker container run <image>

Hello World

However, if we send any command-line arguments with docker container
run <image>, these arguments will take precedence over the CMD command that
we defined. For example, if we execute the following command (replace <image>
with the name of the Docker image), the default "Hello World" output defined
with the CMD directive will be ignored. Instead, the container will output "Hello
Docker !!!":

$ docker container run <image> echo "Hello Docker !!!"

As we discussed, both the RUN and CMD directives can be used to execute a shell
command. The main difference between these two directives is that the command
provided with the RUN directive will be executed during the image build process,
while the command provided with the CMD directive will be executed once a
container is launched from the built image.

Another notable difference between the RUN and CMD directives is that there can be
multiple RUN directives in a Dockerfile, but there can be only one CMD directive
(if there are multiple CMD directives, all others except the last one will be ignored).
As an example, we can use the RUN directive to install a software package during the
Docker image build process and the CMD directive to start the software package once
a container is launched from the built image.

>>>The ENTRYPOINT Directive
Similar to the CMD directive, the ENTRYPOINT directive is also used to provide this
default initialization command that will be executed when a container is created from
the Docker image. The difference between the CMD directive and the ENTRYPOINT
directive is that, unlike the CMD directive, we cannot override the ENTRYPOINT
command using the command-line parameters sent with the docker container
run command.

Note

The --entrypoint flag can be sent with the docker container
run command to override the default ENTRYPOINT of the image.
The ENTRYPOINT directive has the following format:

ENTRYPOINT ["executable","param1","param2","param3", ...]

Similar to the CMD directive, the ENTRYPOINT directive also allows us to provide
the default executable and the parameters. We can use the CMD directive with the
ENTRYPOINT directive to provide additional arguments to the executable.

In the following example, we have used "echo" as the default command and
"Hello" as the default parameter using the ENTRYPOINT directive. We have
also provided "World" as the additional parameter using the CMD directive:

ENTRYPOINT ["echo","Hello"]
CMD ["World"]

The output of the echo command will differ based on how we execute the docker
container run command.

If we launch the Docker image without any command-line parameters, it will output
the message as Hello World:

$ docker container run <image>
Hello World

But if we launch the Docker image with additional command-line parameters
(for example, Docker), the output message will be Hello Docker:

$ docker container run <image> "Docker"
Hello Docker

#Exercise 2.01: Creating Our First Dockerfile

In this exercise, you will create a Docker image that can print the arguments you
pass to the Docker image, preceded by the text You are reading. For example,
if you pass hello world, it will output You are reading hello world as
the output. If no argument is provided, The Docker Workshop will be used as the
standard value:

1. Create a new directory named custom-docker-image using the mkdir
command. This directory will be the context for your Docker image. Context
is the directory that contains all the files needed to successfully build an image:

$ mkdir custom-docker-image

2. Navigate to the newly created custom-docker-image directory using the cd
command as we will be creating all the files required during the build process
(including the Dockerfile) within this directory:

$ cd custom-docker-image

3. Within the custom-docker-image directory, create a file named
Dockerfile using the touch command:

$ touch Dockerfile

4. Now, open the Dockerfile using your favorite text editor:
5. 
$ vim Dockerfile

5. Add the following content to the Dockerfile, save it, and exit from
the Dockerfile:

# This is my first Docker image
FROM ubuntu
LABEL maintainer=sathsara@mydomain.com
RUN apt-get update
CMD ["The Docker Workshop"]
ENTRYPOINT ["echo", "You are reading"]

The Docker image will be based on the Ubuntu parent image. You then
use the LABEL directive to provide the email address of the author of the
Dockerfile. The next line executes the apt-get update command to
update the package list of Debian to the latest available version. Finally, you will
use the ENTRYPOINT and CMD directives to define the default executable and
parameters of the container.

We have provided echo as the default executable and You are reading
as the default parameter that cannot be overridden with command-line
parameters. Also, we have provided The Docker Workshop as an additional
parameter that can be overridden with command-line parameters with a
docker container run command.

#Building Docker Images

A Docker image is the template used to build Docker containers. This is analogous
to how a house plan can be used to create multiple houses from the same design. If
you are familiar with object-oriented programming concepts, a Docker image and
a Docker container have the same relationship as a class and an object. A class in
object-oriented programming can be used to create multiple objects.
A Docker image is a binary file consisting of multiple layers based on the instructions
provided in the Dockerfile. These layers are stacked on top of one another, and
each layer is dependent on the previous layer. Each of the layers is a result of the
changes from the layer below it. All the layers of the Docker image are read-only.

This image build process is initiated by the Docker CLI and executed by the Docker
daemon. To generate a Docker image, the Docker daemon needs access to the
Dockerfile, source code (for example, index.html), and other files (for example,
properties files) that are referenced in the Dockerfile. These files are typically
stored in a directory that is known as the build context. This context will be specified
while executing the docker image build command. The entire context will be sent to
the Docker daemon during the image build process.
The docker image build command takes the following format:
$ docker image build <context>
We can execute the docker image build command from the folder that contains the
Dockerfile and the other files, as shown in the following example. Note that the
dot (.) at the end of the command is used to denote the current directory:

$ docker image build .

Note that there was no name for our custom Docker image. This was because we
did not specify any repository or tag during the build process. We can tag an existing
image with the docker image tag command.
Let's tag our image with IMAGE ID dc3d4fd77861 as my-taggedimage:
v1.0:

$ docker image tag dc3d4fd77861 my-tagged-image:v1.0

We can also tag an image during the build process by specifying the -t flag:

$ docker image build -t my-tagged-image:v2.0 .

>>>Exercise 2.02: Creating Our First Docker Image

In this exercise, you will build the Docker image from the Dockerfile that you
created in Exercise 2.01: Creating Our First Dockerfile and run a Docker container
from the newly built image. First, you will run the Docker image without passing any
arguments, expecting You are reading The Docker Workshop as the output. Next, you
will run the Docker image with Docker Beginner's Guide as the argument and
expect You are reading Docker Beginner's Guide as the output:

1. First, make sure you are in the custom-docker-image directory created in
Exercise 2.01: Creating Our First Dockerfile. Confirm that the directory contains
the following Dockerfile created in Exercise 2.01: Creating Our First Dockerfile:

# This is my first Docker image
FROM ubuntu
LABEL maintainer=sathsara@mydomain.com
RUN apt-get update
CMD ["The Docker Workshop"]
ENTRYPOINT ["echo", "You are reading"]

2. Build the Docker image with the docker image build command. This
command has the optional -t flag to specify the tag of the image. Tag your
image as welcome:1.0:

$ docker image build -t welcome:1.0 .

Note
Do not forget the dot (.) at the end of the preceding command, which is
used to denote the current directory as the build context.

3. Build this image again without changing the Dockerfile content:
$ docker image build -t welcome:2.0 .

4. Use the docker image list command to list all the Docker images available on your computer:
   
$ docker image list

5. Execute the docker container run command to start a new container
from the Docker image that you built in step 1 (welcome:1.0):

$ docker container run welcome:1.0

6. Finally, execute the docker container run command again, this time with
command-line arguments:

$ docker container run welcome:1.0 "Docker Beginner's Guide"

# Other Dockerfile Directives
In the section Common Directives in Dockerfile, we discussed the common directives
available for a Dockerfile. In that section, we discussed FROM, LABEL, RUN, CMD,
and ENTRYPOINT directives and how to use them to create a simple Dockerfile.
In this section, we will be discussing more advanced Dockerfile directives. These
directives can be used to create more advanced Docker images. For example, we
can use the VOLUME directive to bind the filesystem of the host machine to a Docker
container. This will allow us to persist the data generated and used by the Docker
container. Another example is the HEALTHCHECK directive, which allows us to define
health checks to evaluate the health status of Docker containers. We will look into the
following directives in this section:
1. The ENV directive
2. The ARG directive
3. The WORKDIR directive
4. The COPY directive
5. The ADD directive
6. The USER directive
7. The VOLUME directive
8. The EXPOSE directive
9. The HEALTHCHECK directive
10. The ONBUILD directive

>>>The ENV Directive
The ENV directive in Dockerfile is used to set environment variables.
Environment variables are used by applications and processes to get information
about the environment in which a process runs. One example would be the PATH
environment variable, which lists the directories to search for executable files.

Environment variables are defined as key-value pairs as per the following format:

ENV <key> <value>

The PATH environment variable is set with the following value:

$PATH:/usr/local/myapp/bin/

Hence, it can be set using the ENV directive as follows:

ENV PATH $PATH:/usr/local/myapp/bin/

We can set multiple environment variables in the same line separated by spaces.
However, in this form, the key and value should be separated by the equal to
(=) symbol:

ENV <key>=<value> <key>=<value> ...

In the following example, there are two environment variables configured. The PATH
environment variable is configured with the value of $PATH:/usr/local/myapp/
bin/, and the VERSION environment variable is configured with the value of 1.0.0:

ENV PATH=$PATH:/usr/local/myapp/bin/ VERSION=1.0.0

Once an environment variable is set with the ENV directive in the Dockerfile,
this variable is available in all subsequent Docker image layers. This variable is
even available in the Docker containers launched from this Docker image.

>>>The ARG Directive
The ARG directive is used to define variables that the user can pass at build time.
ARG is the only directive that can precede the FROM directive in the Dockerfile.
Users can pass values using --build-arg <varname>=<value>, as shown here,
while building the Docker image:

$ docker image build -t <image>:<tag> --build-arg <varname>=<value> .

The ARG directive has the following format:

ARG <varname>

There can be multiple ARG directives in a Dockerfile, as follows:

ARG USER
ARG VERSION

The ARG directive can also have an optional default value defined. This default value
will be used if no value is passed at build time:

ARG USER=TestUser
ARG VERSION=1.0.0

Unlike the ENV variables, ARG variables are not accessible from the running
container. They are only available during the build process.

>>>Exercise 2.03: Using ENV and ARG Directives in a Dockerfile
Your manager has asked you to create a Dockerfile that will use ubuntu as the
parent image, but you should be able to change the ubuntu version at build time.
You will also need to specify the publisher's name and application directory as the
environment variables of the Docker image. You will use the ENV and ARG directives
in the Dockerfile to perform this exercise:
1. Create a new directory named env-arg-exercise using the
mkdir command:
mkdir env-arg-exercise
2. Navigate to the newly created env-arg-exercise directory using
the cd command:
cd env-arg-exercise

3. Within the env-arg-exercise directory, create a file named Dockerfile:
touch Dockerfile
4. Now, open the Dockerfile using your favorite text editor:
vim Dockerfile
5. Add the following content to the Dockerfile. Then, save and exit from
the Dockerfile:
# ENV and ARG example
ARG TAG=latest
FROM ubuntu:$TAG
LABEL maintainer=sathsara@mydomain.com
ENV PUBLISHER=packt APP_DIR=/usr/local/app/bin
CMD ["env"]
This Dockerfile first defined an argument named TAG with the default value
of the latest. The next line is the FROM directive, which will use the ubuntu parent
image with the TAG variable value sent with the build command (or the default
value if no value is sent with the build command). Then, the LABEL directive
sets the value for the maintainer. Next is the ENV directive, which defines the
environment variable of PUBLISHER with the value packt, and APP_DIR with
the value of /usr/local/app/bin. Finally, use the CMD directive to execute
the env command, which will print all the environment variables.
6. Now, build the Docker image:
7. 
$ docker image build -t env-arg --build-arg TAG=19.04 .

7. Now, execute the docker container run command to start a new
container from the Docker image that you built in the last step:
$ docker container run env-arg

>>>The WORKDIR Directive
The WORKDIR directive is used to specify the current working directory of the Docker
container. Any subsequent ADD, CMD, COPY, ENTRYPOINT, and RUN directives will be
executed in this directory. The WORKDIR directive has the following format:

WORKDIR /path/to/workdir

If the specified directory does not exist, Docker will create this directory and make it
the current working directory, which means this directive executes both mkdir and
cd commands implicitly.

There can be multiple WORKDIR directives in the Dockerfile. If a relative path
is provided in a subsequent WORKDIR directive, that will be relative to the working
directory set by the previous WORKDIR directive:

WORKDIR /one
WORKDIR two
WORKDIR three
RUN pwd

In the preceding example, we are using the pwd command at the end of the
Dockerfile to print the current working directory. The output of the pwd
command will be /one/two/three.

>>The COPY Directive
During the Docker image build process, we may need to copy files from our local
filesystem to the Docker image filesystem. These files can be source code files
(for example, JavaScript files), configuration files (for example, properties files), or
artifacts (for example, JAR files). The COPY directive can be used to copy files and
folders from the local filesystem to the Docker image during the build process.

This directive takes two arguments. The first one is the source path from the local
filesystem, and the second one is the destination path on the image filesystem:

COPY <source> <destination>

In the following example, we are using the COPY directive to copy the index.html
file from the local filesystem to the /var/www/html/ directory of the Docker image:

COPY index.html /var/www/html/index.html

Wildcards can also be specified to copy all files that match the given pattern.
The following example will copy all files with the .html extension from the
current directory to the /var/www/html/ directory of the Docker image:

COPY *.html /var/www/html/

In addition to copying files, the --chown flag can be used with the COPY directive to
specify the user and group ownership of the files:

COPY --chown=myuser:mygroup *.html /var/www/html/

In the preceding example, in addition to copying all the HTML files from the current
directory to the /var/www/html/ directory, the --chown flag is used to set file
ownership, with the user as myuser and group as mygroup:

Note
The --chown flag is only supported from Docker version 17.09 and above.
For Docker versions below 17.09, you need to run the chown command
after the COPY command to change file ownership.

>>>The ADD Directive
The ADD directive is also similar to the COPY directive, and has the following format:

ADD <source> <destination>

However, in addition to the functionality provided by the COPY directive, the ADD
directive also allows us to use a URL as the <source> parameter:

ADD http://sample.com/test.txt /tmp/test.txt

In the preceding example, the ADD directive will download the test.txt file from
http://sample.com and copy the file to the /tmp directory of the Docker
image filesystem.

Another feature of the ADD directive is automatically extracting the compressed
files. If we add a compressed file (gzip, bzip2, tar, and so on) to the <source>
parameter, the ADD directive will extract the archive and copy the content to the
image filesystem.

Imagine we have a compressed file named html.tar.gz that contains index.
html and contact.html files. The following command will extract the html.
tar.gz file and copy the index.html and contact.html files to the /var/
www/html directory:

ADD html.tar.gz /var/www/html

Since the COPY and ADD directives provide almost the same functionality, it is
recommended to always use the COPY directive unless you need the additional
functionality (add from a URL or extract a compressed file) provided by the ADD
directive. This is because the ADD directive provides additional functionality that can
behave unpredictably if used incorrectly (for example, copying files when you want to
extract, or extracting files when you want to copy).

#Exercise 2.04: Using the WORKDIR, COPY, and ADD Directives in the Dockerfile
In this exercise, you will deploy your custom HTML file to the Apache web server. You
will use Ubuntu as the base image and install Apache on top of it. Then, you will copy
your custom index.html file to the Docker image and download the Docker logo (from
the https://www.docker.com website) to be used with the custom index.html file:
1. Create a new directory named workdir-copy-add-exercise using the
mkdir command:
mkdir workdir-copy-add-exercise
2. Navigate to the newly created workdir-copy-add-exercise directory:
cd workdir-copy-add-exercise
3. Within the workdir-copy-add-exercise directory, create a file named
index.html. This file will be copied to the Docker image during build time:
touch index.html
4. Now, open index.html using your favorite text editor:
vim index.html
5. Add the following content to the index.html file, save it, and exit from
index.html:
<html>
    <body>
        <h1>Welcome to The Docker Workshop</h1>
        <img src="logo.png" height="350" width="500"/>
    </body>
</html>
This HTML file will output Welcome to The Docker Workshop as the
header of the page and logo.png (which we will download during the Docker
image build process) as an image. You have defined the size of the logo.png
image as a height of 350 and a width of 500.
62 | Getting Started with Dockerfiles
6. Within the workdir-copy-add-exercise directory, create a file named
Dockerfile:
touch Dockerfile
7. Now, open the Dockerfile using your favorite text editor:
vim Dockerfile
8. Add the following content to the Dockerfile, save it, and exit from the
Dockerfile:
# WORKDIR, COPY and ADD example
FROM ubuntu:latest
RUN apt-get update && apt-get install apache2 -y
WORKDIR /var/www/html/
COPY index.html .
ADD https://www.docker.com/sites/default/files/d8/2019-07/
Moby-logo.png ./logo.png
CMD ["ls"]
This Dockerfile first defines the ubuntu image as the parent image. The next
line is the RUN directive, which will execute apt-get update to update the
package list, and apt-get install apache2 -y to install the Apache HTTP
server. Then, you will set /var/www/html/ as the working directory. Next,
copy the index.html file that we created in step 3 to the Docker image. Then,
use the ADD directive to download the Docker logo from https://www.docker.com/
sites/default/files/d8/2019-07/Moby-logo.png to the Docker image. The final step is to
use the ls command to print the content of the /var/www/html/ directory.
9. Now, build the Docker image with the tag of workdir-copy-add:
$ docker image build -t workdir-copy-add .

10. Execute the docker container run command to start a new container
from the Docker image that you built in the previous step:
$ docker container run workdir-copy-add
As we can see from the output, both the index.html and logo.png files are
available in the /var/www/html/ directory:
index.html
logo.png

The USER Directive
Docker will use the root user as the default user of a Docker container. We can use
the USER directive to change this default behavior and specify a non-root user as the
default user of a Docker container. This is a great way to improve security by running
the Docker container as a non-privileged user. The username specified with the USER
directive will be used to run all subsequent RUN, CMD, and ENTRYPOINT directives in
the Dockerfile.
The USER directive takes the following format:
USER <user>
In addition to the username, we can also specify the optional group name to run the
Docker container:
USER <user>:<group>
We need to make sure that the <user> and <group> values are valid user and
group names. Otherwise, the Docker daemon will throw an error while trying to run
the container:
docker: Error response from daemon: unable to find user my_user:
no matching entries in passwd file.

>>>Exercise 2.05: Using USER Directive in the Dockerfile
Your manager has asked you to create a Docker image to run the Apache web server.
He has specifically requested that you use a non-root user while running the Docker
container due to security reasons. In this exercise, you will use the USER directive in
the Dockerfile to set the default user. You will be installing the Apache web server
and changing the user to www-data. Finally, you will execute the whoami command
to verify the current user by printing the username:

Note
The www-data user is the default user for the Apache web server
on Ubuntu.

1. Create a new directory named user-exercise for this exercise:
mkdir user-exercise
2. Navigate to the newly created user-exercise directory:
cd user-exercise
3. Within the user-exercise directory, create a file named Dockerfile:
touch Dockerfile
4. Now, open the Dockerfile using your favorite text editor:
vim Dockerfile
5. Add the following content to the Dockerfile, save it, and exit from
the Dockerfile:
# USER example
FROM ubuntu
RUN apt-get update && apt-get install apache2 -y
USER www-data
CMD ["whoami"]
This Dockerfile first defines the Ubuntu image as the parent image. The
next line is the RUN directive, which will execute apt-get update to update
the package list, and apt-get install apache2 -y to install the Apache
HTTP server. Next, you use the USER directive to change the current user to the
www-data user. Finally, you have the CMD directive, which executes the whoami
command, which will print the username of the current user.
6. Build the Docker image:
$ docker image build -t user .

7. Now, execute the docker container run command to start a new container
from the Docker image that we built in the previous step:
$ docker container run user
As you can see from the following output, www-data is the current user
associated with the Docker container:
www-data