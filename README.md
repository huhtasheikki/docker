# docker
Hive project to understand and think through containers, docker + shell scripts.

# Mandatory part

## V.1 How to Docker
In this first part you will be introduced to Docker and its main options. In your repository,
create a 00_how_to_docker folder in which you will push the solutions for all exercises.
If you don’t have it already, install docker, docker-machine and virtualbox. Docker
and docker-machine can be installed via Brew, and virtualbox via the Managed Software Center.
Check that docker version prints out your current docker version, and that you have
at least the 1.12 docker (Editor’s Note: as I write, a new versionning notation is being
used, and the most recent version is 17.03.0-ce-mac2).
You might be tempted to use Docker for Mac, and I sympathize. Don’t worry, you’ll
use it for the next Docker projects. But for this project, you will need to implement
clustering locally, and that can be done quite easily with docker-machine!
You will push your work in the 00_how_to_docker folder: one file per exercise, named
after the exercise number, with the command(s) requested for this exercise.
$> ls 00_how_to_docker
01 02 03 04 05 06
[...]
31 32 33 34
$> cat 00_how_to_docker/01
docker-machine [...]
7
Docker-1 Now, you’re thinking with containers...

## V.2 Exercises
For each exercise, we will ask you to give the shell command(s) to:
1. Create a virtual machine with docker-machine using the virtualbox driver, and
named Char.
2. Get the IP address of the Char virtual machine.
3. Define the variables needed by your virtual machine Char in the general env of your
terminal, so that you can run the docker ps command without errors. You have
to fix all four environment variables with one command, and you are not allowed
to use your shell’s builtin to set these variables by hand.
4. Get the hello-world container from the Docker Hub, where it’s available.
5. Launch the hello-world container, and make sure that it prints its welcome message, then leaves it.
6. Launch an nginx container, available on Docker Hub, as a background task. It
should be named overlord, be able to restart on its own, and have its 80 port
attached to the 5000 port of Char. You can check that your container functions
properly by visiting
http://<ip-de-char>:5000 on your web browser.
7. Get the internal IP address of the overlord container without starting its shell and
in one command.
8. Launch a shell from an alpine container, and make sure that you can interact
directly with the container via your terminal, and that the container deletes itself
once the shell’s execution is done.
9. From the shell of a debian container, install via the container’s package manager
everything you need to compile C source code and push it onto a git repo (of
course, make sure before that the package manager and the packages already in the
container are updated). For this exercise, you should only specify the commands
to be run directly in the container.
10. Create a volume named hatchery.
11. List all the Docker volumes created on the machine. Remember. VOLUMES.
8
Docker-1 Now, you’re thinking with containers...
12. Launch a mysql container as a background task. It should be able to restart on its
own in case of error, and the root password of the database should be Kerrigan.
You will also make sure that the database is stored in the hatchery volume, that
the container directly creates a database named zerglings, and that the container
itself is named spawning-pool.
13. Print the environment variables of the spawning-pool container in one command,
to be sure that you have configured your container properly.
14. Launch a wordpress container as a background task, just for fun. The container
should be named lair, its 80 port should be bound to the 8080 port of the virtual
machine, and it should be able to use the spawning-pool container as a database
service. You can try to access lair on your machine via a web browser, with the
IP address of the virtual machine as a URL.
Congratulations, you just deployed a functional Wordpress website in two commands!
15. Launch a phpmyadmin container as a background task. It should be named roach-warden,
its 80 port should be bound to the 8081 port of the virtual machine and it should
be able to explore the database stored in the spawning-pool container.
16. Look up the spawning-pool container’s logs in real time without running its shell.
17. Display all the currently active containers on the Char virtual machine.
18. Relaunch the overlord container.
19. Launch a container name Abathur. It will be a Python container, 2-slim version,
its /root folder will be bound to a HOME folder on your host, and its 3000 port
will be bound to the 3000 port of your virtual machine.
You will personalize this container so that you can use the Flask micro-framework
in its latest version. You will make sure that an html page displaying "Hello World"
with < h1 > tags can be served by Flask. You will test that your container is
properly set up by accessing, via curl or a web browser, the IP address of your
virtual machine on the 3000 port.
You will also list all the necessary commands in your repository.
20. Create a local swarm, the Char virtual machine should be its manager.
21. Create another virtual machine with docker-machine using the virtualbox driver,
and name it Aiur.
22. Turn Aiur into a slave of the local swarm in which Char is leader (the command to
take control of Aiur is not requested).
23. Create an overlay-type internal network that you will name overmind.
9
Docker-1 Now, you’re thinking with containers...
24. Launch a rabbitmq SERVICE that will be named orbital-command. You should
define a specific user and password for the RabbitMQ service, they can be whatever
you want. This service will be on the overmind network.
25. List all the services of the local swarm.
26. Launch a 42school/engineering-bay service in two replicas and make sure that
the service works properly (see the documentation provided at hub.docker.com).
This service will be named engineering-bay and will be on the overmind network.
27. Get the real-time logs of one the tasks of the engineering-bay service.
28. ... Damn it, a group of zergs is attacking orbital-command, and shutting down
the engineering-bay service won’t help at all... You must send a troup of Marines
to eliminate the intruders. Launch a 42school/marine-squad in two replicas,
and make sure that the service works properly (see the documentation provided
at hub.docker.com). This service will be named... marines and will be on the
overmind network.
29. Display all the tasks of the marines service.
30. Increase the number of copies of the marines service up to twenty, because there’s
never enough Marines to eliminate Zergs. (Remember to take a look at the tasks
and logs of the service, you’ll see, it’s fun.)
31. Force quit and delete all the services on the local swarm, in one command.
32. Force quit and delete all the containers (whatever their status), in one command.
33. Delete all the container images stored on the Char virtual machine, in one command
as well.
34. Delete the Aiur virtual machine without using rm -rf.
10

# Chapter VI
Dockerfiles
So, that was fun wasn’t it?
Now, it’s time get down to business. Docker makes it possible to create yourself
OWN containers for your OWN applications! That’s what this chapter is about. Luckily,
Docker lets you program Dockerfiles (aka a Makerfile for Docker... I think you get the
nuance). Dockerfiles use a specific syntax that reuses a base image or an existing container to add your own dependencies and your own files.
You will also notice that every command built by your Dockerfiles generates a layer
that’s reusable in other Dockerfiles or other versions of the same Dockerfile, in order to
avoid data duplication. Awesome, isn’t it?
But before you start making your own containers, make sure that your virtual machine
is cleared of all images and residual active containers. We’ll start from the beginning and
build up more and more complex applications.
Create a 01_dockerfiles folder in which you’ll store all the Dockerfiles that you’ll need
later, in separate folders (ex00 / ex01 ...).
Check out this link to design high quality Dockerfiles: Dockerfile Best Practices
11
Docker-1 Now, you’re thinking with containers...

### VI.1 Exercise 00: vim/emacs
Exercise 00
Exercise 00: vim/emacs
Turn-in directory : ex00/
Files to turn in : Dockerfile
Allowed functions : -
Notes : n/a
From an alpine image you’ll add to your container your favorite text editor, vim or
emacs, that will launch along with your container.

### VI.2 Exercise 01: BYOTSS
Exercise 01
Exercise 01: BYOTSS
Turn-in directory : ex01/
Files to turn in : Dockerfile + Scripts (if applicable)
Allowed functions : -
Notes : n/a
From a debian image you will add the appropriate sources to create a TeamSpeak
server, that will launch along with your container. It will be deemed valid if at least
one user can connect to it and engage in a normal discussion (no far-fetched setup), so
be sure to create your Dockerfile with the right options. Your program should get the
sources when it builds, they cannot be in your repository.
For the smarty-pants using the official docker image of TeamSpeak is
strictly FORBIDDEN, and will get you -42.
12
Docker-1 Now, you’re thinking with containers...

### VI.3 Exercise 02: Dockerfile in a Dockerfile... in a
Dockerfile ?
Exercise 02
Exercise 02: Dockerfile in a Dockerfile... in a Dockerfile ?
Turn-in directory : ex02/
Files to turn in : Dockerfile
Allowed functions : -
Notes : n/a
You are going to create your first Dockerfile to containerize Rails applications. That’s
a special configuration: this particular Dockerfile will be generic, and called in another
Dockerfile, that will look something like this:
FROM ft-rails:on-build
EXPOSE 3000
CMD ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
Your generic container should install, via a ruby container, all the necessary dependencies and gems, then copy your rails application in the /opt/app folder of your
container. Docker has to install the approtiate gems when it builds, but also launch
the migrations and the db population for your application. The child Dockerfile should
launch the rails server (see example below). If you don’t know what commands to use,
it’s high time to look at the Ruby on Rails documentation.
13
Docker-1 Now, you’re thinking with containers...

### VI.4 Exercise 03: ... and bacon strips ... and bacon
strips ...
Exercise 03
Exercise 03: ... and bacon strips ... and bacon strips ...
Turn-in directory : ex03/
Files to turn in : Dockerfile
Allowed functions : -
Notes : n/a
Docker can be useful to test an application that’s still being developed without polluting your libraries. You will have to design a Dockerfile that gets the development
version of Gitlab - Community Edition installs it with all the dependencies and the necessary configurations, and launches the application, all as it builds. The container will be
deemed valid if you can access the web client, create users and interact via GIT with this
container (HTTPS and SSH). Obviously, you are not allowed to use the official container
from Gitlab, it would be a shame...
