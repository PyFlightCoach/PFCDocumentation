.. _fcscore:

FCScore
=======

FCScore is a tool for automatic judging of precision aerobatics. You can use FCScore from the following link:

https://pyflightcoach.github.io/FCScoreClient/

The program consists of this web interface and an analysis server. We have a public analysis server running in the UK, but it could get very slow especially at busy times. For a better experience and to reduce the load on our server you can run the analysis server on your computer. To do this first install Docker and start it running, then run the following command in a terminal:

.. code-block:: console
    
    docker run --rm -p 5000:5000 thomasdavid/fcs-server:latest

This will download the latest version of the server and start it running. You can then select the local analysis server under the Options menu.


.. toctree::

   installation
   usage
   scoring

