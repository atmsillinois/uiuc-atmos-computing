Utilizing Python on Keeling
===========================

If you are starting out, it may be easiest to use Conda

    wget https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh

For a lightweight experience, you can use Miniconda

    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

To run the script::

    bash Miniconda3-latest-Linux-x86_64.sh

    bash Anaconda-latest-Linux-x86_64.sh

Accep the policy

Specify where to install it

And activate it.

You may have to source .bashrc to get it to work initially.

You will be in the base environment.



Managing environments
---------------------

With conda, you can create, export, list, remove, and update environments that
have different versions of Python and/or packages installed in them. 
Isolating different purposes to different environments ensures that
changes do not break previously existing code due to dependencies.

To create a new environment::

    conda create --name myenv

where ``myenv`` is the name you wish to use for the environment.



Switching or moving between environments is called activating the environment.
To actiavte this new environment::

    conda activate myenv

For more information about environments see

https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html

Utilizing Jupyter notebooks on Keeling
--------------------------------------

#. Start an interactive session that meets your compute requirements. For example:

   .. code-block:: console

    qlogin -p node -n 4 --mem-per-cpu=16G -time 24:00:00

   would request 4 cores with 16 GB of memory each for 24 hours.


   Suggestion: If you always are doing the same thing, consider setting up an alias in your
   ``.modules7`` such as the following:

   .. code-block:: console

    alias qlognb="qlogin -p node -n 4 --mem-per-cpu=16G -time 24:00:00"

   which shortens invoking a compute job for your notebooks by simply

   .. code-block:: console

    qlognb

#. Note the node that your job is on at that is important.
   This should be presented to you on job start up with the displayed information
   regarding your job request (under "connecting to node") 
   or can be acquired in general at any point by typing:

   .. code-block:: console

    hostname

#. Start a jupyter notebook:

   .. code-block:: console

    jupyter notebook --port=XXXX --no-browser --ip=127.0.0.1

   or if you prefer to use jupyter-lab (https://jupyterlab.readthedocs.io/en/stable/)

   .. code-block:: console

    jupyter-lab --port=XXXX --no-browser --ip=127.0.0.1 

   where the XXXX is a port selected by you. It is important that
   you select and use a port unique to yourself and not a port that will
   conflict with other users.

   Suggestion: Once again, this may be shortened as an alias if you find yourself
   using the same parameters. Example:

   .. code-block:: console

    alias nb="jupyter-lab --port=XXXX --no-browser --ip=127.0.0.1"

#. Using a terminal, open a second ssh session to keeling, with the following command to
   access the compute node that is running your notebook server:

   .. code-block:: console

    ssh -L XXXX:127.0.0.1:XXXX netID@keeling.earth.illinois.edu ssh -L XXXX:127.0.0.1:XXXX hostname

   where hostname is the Keeling compute node (eg: keeling-d01, keeling-g20), XXXX is your
   unique port and netID is your netID.
