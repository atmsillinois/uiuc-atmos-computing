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
