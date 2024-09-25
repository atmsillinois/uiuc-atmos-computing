Options for editing files on Keeling
=====================================


Traditional text editors
------------------------

Keeling is equipped with the usual assortment of text editors for editing
scripts and code.

* emacs

* nano

* vim

Visual Studio Code
------------------

Visual Studio Code, also commonly referred to as VS Code, is a source-code editor
made by Microsoft and can be used on your local machine to connect to Keeling and
edit code.

The steps to do this are as follows:

#. Acquire the code from https://code.visualstudio.com/

#. Install

#. Open VS Code

#. You will need to get the ssh extension Remote-SSH

#. After installing the extension, in VS Code, select Remote-SSH: Connect to Host
   for the Command Palette (F1, ⇧⌘P). Enter the following::

     ssh <your netID>@keeling.earth.illinois.edu

Connect Python Jupyter Notebook in VS Code
------------------------------------------

Open terminal and connect to keeling via ``ssh`` and make sure you have the Python 
and Jupyter extension installed in VS Code.

.. code-block:: console

    ssh <your netID>@keeling.earth.illinois.edu

You could use ``sinfo`` to check the available partitions and nodes, 
e.g. keeling-gpu09, and specify the port number ``xxxx``, e.g. 7777. 

Now you could use the following command to login to the node,

.. code-block:: console

    ssh -L 127.0.0.1:xxxx:127.0.0.1:xxxx node_name

You will be required to input the password. After that, activate the Python environment
and start the Jupyter Notebook server. Make sure using
the same port number ``xxxx``.

.. code-block:: console

    conda activate <env_name>
    jupyter notebook --port=xxxx --ip=127.0.0.1

You will see the links like below,

.. code-block:: console

    To access the server, open this file in a browser:
        file:///data/keeling/a/<your netID>/.local/share/jupyter/runtime/jpserver-44396-open.html
    Or copy and paste one of these URLs:
        http://127.0.0.1:7777/tree?token=1c188a2ddb454ee362005aa556b5cbe189f1012c85139b3a
        http://127.0.0.1:7777/tree?token=1c188a2ddb454ee362005aa556b5cbe189f1012c85139b3a

choose one and copy the link.

Open an ``ipynb`` file in VS Code. You will see ``Select Kernel`` on the right top
of the window, click it and there will be a pop-up shows ``Existing Jupter Server``. 
Click it and choose ``Enter the URL of the running Jupyter Server``, paste the aboved 
link here.

Press ``Enter`` and choose ``Python 3 (ipykernel)``. You will see previous 
``Select Kernel`` is changed to ``Python 3 (ipykernel)``. 

Try running following command in a cell to check if it is working.

.. code-block:: console

    !hostname

The output should be the node name you are using.