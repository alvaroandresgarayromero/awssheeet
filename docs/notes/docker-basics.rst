.. meta::
    :description lang=en: Docker
    :keywords: Python, Python3, Docker, Containers, Flask, Docker-Compose

====================
Docker Containers
====================

.. contents:: Table of Contents
    :backlinks: none

Introduction
--------------

Docker allows us to run multiple isolated processes in parallel. A container is an isolated process that consists of the following items, all bundled into one package:

- the application code,
- the required dependencies (e.g. libraries, utilities, configuration files), and
- the necessary runtime environment to run the application.

Each container is an independent component that can run on its own and be moved from environment to environment.

Basic Docker Workflow
--------------------------

- Create an empty directory (docker will read all files unless explicitly ignored)

- Create DockerFile: Contains instructions to create Docker Image

    .. code-block:: bash

        #Dockefile does not have any file extension

        FROM python:3.7.2-slim

        COPY . /app
        WORKDIR /app

        RUN pip install --upgrade pip
        RUN pip install flask

        ENTRYPOINT ["python", "app.py"]

- Create simple flask-python app.py file

    .. code-block:: python

        from flask import Flask, jsonify

        app = Flask(__name__)


        @app.route('/')
        def index():
            return jsonify({'success': True})


        if __name__ == '__main__':
            app.run(host='0.0.0.0', port=8080, debug=True)


- Directory should appear like

    .. code-block:: bash

        ~/DockerExample1$ ls
        app.py  Dockerfile

- Build Docker image

- Create and run container

- Verify app is running:

    .. code-block:: bash

       $ curl http://0.0.0.0:80
        {
          "success": true
        }

Basic Docker-Compose Workflow
--------------------------------

Compose is a tool for defining and running multi-container Docker applications

- Create an empty directory (docker will read all files unless explicitly ignored)

- Create DockerFile: Contains instructions to create Docker Image

    .. code-block:: bash

        #Dockefile does not have any file extension

        FROM python:3.7-stretch

        WORKDIR /app

        COPY . /app

        RUN pip install --upgrade pip
        RUN pip install PyJWT==1.7.1
        RUN pip install flask==1.1.2
        RUN pip install gunicorn==20.0.4
        RUN pip install pytest==6.2.2

        ENTRYPOINT ["gunicorn", "-b", ":8080", "main:APP"]

- Create Docker-Compose.yml: Contains instructions to build image and container

    Note: Volumes key mounts the project directory (current directory) on the host to /app inside the container, allowing you to modify the code on the fly, without having to rebuild the image.

    .. code-block:: yaml

        version: "3"
        services:
          web:
            build: .
            image: myimage
            container_name: myContainer
            env_file:
              - .env_file
            ports:
              - "80:8080"
            volumes:
              - .:/app

- Create simple flask-python app.py file

    .. code-block:: python

        from flask import Flask, jsonify

        APP = Flask(__name__)


        @APP.route('/', methods=['POST', 'GET'])
        def health():
            return jsonify("Healthy")


        if __name__ == '__main__':
            APP.run(host='127.0.0.1', port=8080, debug=True)



- Build Docker image, create/run container

- Verify app is running:

    .. code-block:: bash

       $ curl http://0.0.0.0:80/
         "Healthy"

Image
------

- Build

    .. code-block:: bash

        $ sudo docker build -t myimage .


- Check the list of images

    .. code-block:: bash

        $ sudo docker image ls

- Remove specific image

    .. code-block:: bash

        $ sudo docker image rm <image_id>

- Remove all images

    .. code-block:: bash

        $ sudo docker image prune

- Check the list of images

    .. code-block:: bash

        $ sudo docker image ls

Container
-----------

- Run Container

    .. code-block:: bash

        $ docker run --name myContainer myimage

    .. code-block:: bash

        # -p mapping port 80 of your local machine to the port 8080 of the container when running web application.
        $ sudo docker run -p 80:8080 myimage

    .. code-block:: bash

        # add name to container
        $ sudo docker run --name myContainer -p 80:8080 test

    .. code-block:: bash

        # add environment files, if any
        $ sudo docker run --name myContainer --env-file=.env_file -p 80:8080 test

- List running containers

    .. code-block:: bash

        $ docker container ls
        $ docker ps

- Get all running and stopped container

    .. code-block:: bash

        $ docker ps -a

- Stop a container

    .. code-block:: bash

        $ sudo docker container stop <container_id>

- Remove a specific container

    .. code-block:: bash

        $ sudo docker container rm <container_id>

- Remove all containers

    .. code-block:: bash

        $ sudo docker container prune

- Enter Container:

    .. code-block:: bash

        $ sudo docker exec -it <container_id> /bin/bash

Docker-Compose
---------------

- Build Image

    New or overwrite existing image

    .. code-block:: bash

        $ sudo docker-compose build

- Run Container

    Build current (or new image if none), and run container

    .. code-block:: bash

        $ sudo docker-compose up

Enable TCP port 2375 (Linux)
--------------------------------

Enabling TCP port 2375 for external connection to Docker can allow third-party (IDEs like PyCharm) plugin tools interface with Docker

- Create daemon.json file in /etc/docker:

    .. code-block:: bash

        {"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]}

- Add /etc/systemd/system/docker.service.d/override.conf

    .. code-block:: bash

         [Service]
         ExecStart=
         ExecStart=/usr/bin/dockerd

- Reload the systemd daemon:

    .. code-block:: bash

        systemctl daemon-reload

- Restart docker:

    .. code-block:: bash

        systemctl restart docker.service
