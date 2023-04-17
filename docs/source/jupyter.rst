Utilizing Jupyter notebooks on Keeling
======================================

Start an interactive session

    qlogin -p node -n 4 --mem-per-cpu=16G -time 24:00:00

The node that you are on is important information this should be
presented to you

or can be acquired in general at any point by::

    hostname

Start a jupyter notebook

    jupyter notebook --port=XXXX --no-browser --ip=127.0.0.1

or if you prefer to use jupyter-lab (https://jupyterlab.readthedocs.io/en/stable/)

   jupyter-lab --port=XXXX --no-browser --ip=127.0.0.1 

where the XXXX is a port selected by you. It is important that
you select and use a port unique to yourself and not a port that will
conflict with other users.

.. todo:: What is the best way to connect to the notebook from your local machine...
