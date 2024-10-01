.. _installation:

Installation
------------

These instructions are to run both the client and server locally on your computer. This might be useful if you want to analyse flights without internet access. For general use you can just use the online version here. https://pyflightcoach.github.io/FCScoreClient/

Setup with Docker (Linux):

.. code-block:: console

    $ git clone git@github.com:PyFlightCoach/FCScore.git
    $ cd FCScore/project
    $ docker compose up

Once it is running, open a web browser to: localhost/5173 

To stop the server:

.. code-block:: console

    $ docker compose down


Windows Setup for non-developers:
---------------------------------

1 - make a new folder on your computer.

2 - download this file: https://github.com/PyFlightCoach/FCScore/blob/main/project/docker-compose.yml

3 - save it in the folder

4 - Follow these instructions to install docker and docker compose: https://docs.docker.com/compose/install/>

5 - open the folder in file explorer, in the top bar type cmd, then press return

6 - it should open a command window, type 'docker compose up' then press return

7 - Hopefully it will tell you something like this:

.. code-block:: console

    >> VITE v4.5.1  ready in 1194 ms
    >> ➜  Local:   http://localhost:5173/
    >> ➜  Network: use --host to expose
    >> ➜  press h to show help


8 - open a webrowser (chrome or firefox, I havent tried others)
9 - go to http://localhost:5173/
