Usage
=====

.. _installation:

Installation
------------

The PyflightCoach packages can be installed using pip or built from source

.. code-block:: console

   (.venv) $ pip install ardupilot-log-reader 
   (.venv) $ pip install pfc-geometry
   (.venv) $ pip install flightdata
   (.venv) $ pip install flightplotting
   (.venv) $ pip install flightanalysis



Clone the source code:

.. code-block:: console
   $ git clone https://github.com/PyFlightCoach/PyFlightCoach.git
   $ git submudules update --init --recursive


To setup a conda environment with all the developer dependencies, run the following command:

.. code-block:: console
   $ source make_conda_env.sh


To setup manually install the requirements in requirements.txt, then cd to each submodule and do: python setup.py develop. 

