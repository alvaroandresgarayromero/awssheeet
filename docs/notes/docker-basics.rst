.. meta::
    :description lang=en: Docker
    :keywords: Python, Python3, Docker, Containers, Flask

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

Build image
-----------

.. code-block:: bash

    $ sudo docker build -t myimage .


Check the list of images
-------------------------

.. code-block:: bash

    $ sudo docker image ls

Remove any image
-----------------

.. code-block:: bash

    $sudo docker image rm <image_id>

Check the list of images
-------------------------

.. code-block:: bash

    $ sudo docker image ls

Run Container
--------------

.. code-block:: bash

    $ docker run --name myContainer myimage


List running containers
-------------------------

.. code-block:: bash

    $ docker container ls
    $ docker ps

Stop a container
-----------------

.. code-block:: bash

    $ sudo docker container stop <container_id>

Remove a container
-------------------

.. code-block:: bash

    $ sudo docker container rm <container_id>

Python
--------

- Create an empty directory (docker will read all files unless explicitly ignored)

- Create DockerFile: contains instructions to create Docker Image

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

.. code-block:: bash

    # cd to DockerExample1
    # -t test creates a repository tagged as "test"
    $ sudo docker build -t test .

- Create and run container

.. code-block:: bash

    # -p mapping port 80 of your local machine to the port 8080 of the container running the flask application.
    $ sudo docker run -p 80:8080 test

- output:

.. code-block:: bash

   $ curl http://0.0.0.0:80
    {
      "success": true
    }

- Check active running Docker Containers

.. code-block:: bash

    $ sudo docker ps
    CONTAINER ID   IMAGE     COMMAND           CREATED          STATUS          PORTS                  NAMES
    9b8b83f994ee   test      "python app.py"   25 minutes ago   Up 25 minutes   0.0.0.0:80->8080/tcp   kind_mendel

- Stop Specific Docker Container

.. code-block:: bash

    $ sudo docker stop 9b8b83f994ee
