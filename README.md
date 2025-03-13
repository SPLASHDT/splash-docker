# Docker containers for SPLASH Digital Twin

This repository contains a Docker Compose file to build docker containers for both the front and backend of the SPLASH Digital Twin. 
It uses git sudmodules to bring in the code (which includes the Dockerfiles) of both components.

To build the containers run the following:

~~~
git clone --recurse-submodules git@github.com:SPLASHDT/splash-docker.git
docker-compose build
~~~

To run them in the foreground run:

~~~
docker-compose up
~~~

or in the background:

~~~
docker-compose up -d
~~~

and to stop them:

~~~
docker-compose stop
~~~
