Utilizing Python on Keeling
===========================

If you are starting out, it may be easiest to use Conda.
Conda is an open source package management system and environment management system.
It easily installs, runs and updates packages and their dependencies.
Conda easily creates, saves, loads and switches between environments such that
different environments can be used without breaking the other. 

#. To acquire Conda:

   .. code-block:: console

    wget https://repo.anaconda.com/archive/Anaconda3-2023.07-0-Linux-x86_64.sh

   .. note::

     To get the latest version of Conda for Linux-x86_64, find the newest release `here <https://repo.anaconda.com/archive/>`__.

   For a lightweight experience, you can use Miniconda, which consists of less initial packages:

   .. code-block:: console

    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

#. Run the script to install it by:

   .. code-block:: console

    bash Miniconda3-latest-Linux-x86_64.sh

   or

   .. code-block:: console

    bash Anaconda3-2023.07-0-Linux-x86_64.sh

#. Accept the policy by responding ``yes`` when prompted.

#. Specify where you want to install it. The default path will typically put it in your ``$HOME``
   directory under the directory ``anaconda3``. After this, packages will be downloaded and installed.

#. Activate your conda installation.
   You may have to ``source .bashrc`` to get it to work initially.

#. You will then be in the ``base`` environment.

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

   .. hint::

    If you always are doing the same parameters in your command, consider
    setting up an alias in your ``.bashrc`` such as the following:

    .. code-block:: console

     alias qlognb="qlogin -p node -n 4 --mem-per-cpu=16G -time 24:00:00"

    which shortens the effort for issue a compute job for your notebooks by simply

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

   where the ``XXXX`` is a port selected by you. It is important that
   you select and use a port unique to yourself and not a port that will
   conflict with other users.

   .. hint::

    Similar to before, this command may be shortened as an alias if you find yourself
    using the same parameters. Example:

    .. code-block:: console

     alias nb="jupyter-lab --port=XXXX --no-browser --ip=127.0.0.1"

    which would then be simply invoked by

    .. code-block:: console

     nb

#. Using a terminal, open a second ssh session to keeling, with the following command to
   access the compute node that is running your notebook server:

   .. code-block:: console

    ssh -L XXXX:127.0.0.1:XXXX netID@keeling.earth.illinois.edu ssh -L XXXX:127.0.0.1:XXXX hostname

   where ``hostname`` is the Keeling compute node (eg: keeling-d01, keeling-g20), ``XXXX`` is your
   unique port and ``netID`` is your netID.
