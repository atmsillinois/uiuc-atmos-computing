Accessing compute nodes
=======================

The login node of Keeling is useful for lightweight computing that requires not a lot of memory
and not a large number of cores. Programs that require large amounts of memory or CPUs will be
killed if run on the head node.

To avoid this issue, users must access the compute nodes. This can be done in two ways,
interactively or using the batch system which uses `SLURM <https://slurm.schedmd.com/>`__.

Available compute resources
---------------------------

Keeling consists of different partitions that you may submit your job script
to depending on your computing needs. Partitions vary on number of nodes required,
amount of wall time allowed, and availability of special resources
(GPUs or nodes with higher core counts),

+---------+--------------------------------------------+------------------+
| Option  | Description                                | Wall clock limit |
+---------+--------------------------------------------+------------------+
| node    | Shared single node                         | 10 days          |
+---------+--------------------------------------------+------------------+
| seseml  | Exclusive single node                      | 7 days           |
+---------+--------------------------------------------+------------------+
| sesempi | Multiple node jobs (MinNodes=2 MaxNodes=8) | 7 days           |
+---------+--------------------------------------------+------------------+
| sesebig | Larger jobs (MinNodes=9 MaxNodes=32)       | 2 days           |
+---------+--------------------------------------------+------------------+
| gpu     | Access to GPU (Titan Titan RTX), MaxNodes=1      | 2 days           |
+---------+--------------------------------------------+------------------+
| L40S    | Access to GPU (L40S), MaxNodes=2           | 2 days           |
+---------+--------------------------------------------+------------------+

Keeling consists of the following types of nodes with the following features

+------------------------------+----------------+----------------------+----------+
| Name                         | Cores per node | Real memory (MB)     | GPU VRAM |
+------------------------------+----------------+----------------------+----------+
| a                            | 4              | 12000                |          |
+------------------------------+----------------+----------------------+----------+
| b                            | 8              | 16000, 32000         |          |
+------------------------------+----------------+----------------------+----------+
| c                            | 12             | 32160, 72480, 118000 |          |
+------------------------------+----------------+----------------------+----------+
| d                            | 12             | 64300, 80500         |          |
+------------------------------+----------------+----------------------+----------+
| e                            | 12             | 64300                |          |
+------------------------------+----------------+----------------------+----------+
| f                            | 8              | 24000                |          |
+------------------------------+----------------+----------------------+----------+
| g                            | 20             | 128800               |          |
+------------------------------+----------------+----------------------+----------+
| gpu (Titan RTX)              | 20             | 253000               | 24 GB    |
+------------------------------+----------------+----------------------+----------+
| gpu (L40S)                   | 96             | 253000               | 46 GB    |
+------------------------------+----------------+----------------------+----------+
| h                            | 20 or 24       | 256200               |          |
+------------------------------+----------------+----------------------+----------+
| i                            | 32             | 192000               |          |
+------------------------------+----------------+----------------------+----------+
| j                            | 48             | 253000               |          |
+------------------------------+----------------+----------------------+----------+

Interactive computing
---------------------

An interactive session reserves dedicated resources on compute nodes allowing you to use them
interactively as you would the login node.
To request an interactive job, the ``qlogin`` command is used. An example of a request:

.. code-block:: console

    qlogin -n 20 --mem-per-cpu=4096 --time=12:00:00 --job-name=interactive

will utilize 20 cores with 4 GB of memory per core (specified as 4096 MB),
a wall clock limit of 12 hours.

.. note::

   Interactive job launched by ``qlogin`` are only allowed one node.

To finalize your interactive compute job may do:

.. code-block:: console

    exit

which will end you job and release the node for others to use.

Alternatively you can use ``srun`` to access other partitions to request more resources.

Accessing GPU nodes
^^^^^^^^^^^^^^^^^^^

Keeling has 1 Titan RTX GPU node and 8 L40S GPUs. To access these resources, you must specify the
``gpu`` or ``L40S`` partition when requesting an interactive session.

To interactively compute on the Titan RTX GPU node (24 GB VRAM):

.. code-block:: console

    qlogin -p gpu -n 20 --gres=gpu:RTX:1 --mem=253000

To interactively compute on a newer L40S GPU node (46 GB VRAM):

.. code-block:: console

    qlogin -p l40s -n 96 --gres=gpu:L40S:1 --mem=253000

.. note::

    Once in the interactive sessions, you must run the following
    to load the neccessary modules to use the L40S GPU:

    .. code-block:: console

        module purge
        module load L40S

Batch computing
---------------

A batch job is controlled by a script written by the user who submits the job to the batch system (Slurm).
The batch system then selects the resources for the job given the parameters from the user
and decides where and when to run the job. This method allows you to have access to more resources.

Jobs are submitted by:

.. code-block:: console

    sbatch <job script>

The job script is a shell script that contains the necessary directives for the batch system to
allocate resources for the job. The job script should typically contain the following directives:

+---------------------+-----------------+
| Directive           | Description     |
+---------------------+-----------------+
| #SBATCH --job-name  | Name of the job |
+---------------------+-----------------+
| #SBATCH -p          | Partition       |
+---------------------+-----------------+
| #SBATCH -n          | Number of cores |
+---------------------+-----------------+
| #SBATCH --time      | Wall clock time |
+---------------------+-----------------+
| #SBATCH --mem       | Memory          |
+---------------------+-----------------+

For GPU batch computing:

+----------------+----------------------------------------------------+
| Directive      | Description                                        |
+----------------+----------------------------------------------------+
| #SBATCH --gres | Resources request (GPUS). For L40S, ``gpu:L40S:1`` |
+----------------+----------------------------------------------------+


Additional useful directives:

+-----------------------+----------------------------------------------+
| Directive             | Description                                  |
+-----------------------+----------------------------------------------+
| #SBATCH --mail-user   | Email address to receive notifications       |
+-----------------------+----------------------------------------------+
| #SBATCH --mail-type   | Reasons to notify (e.g. BEGIN, END, FAIL)    |
+-----------------------+----------------------------------------------+
| #SBATCH --constraint  | Node constraint (i.e. required node feature) |
+-----------------------+----------------------------------------------+
| #SBATCH --sockets     | Number of sockets per node                   |
+-----------------------+----------------------------------------------+
| #SBATCH --cores       | Number of cores per socket                   |
+-----------------------+----------------------------------------------+
| #SBATCH --mem-per-cpu | Memory per core. Alternative to --mem        |
+-----------------------+----------------------------------------------+


Example scripts
^^^^^^^^^^^^^^^

A simple script requesting a single node consisting of 12 cores for 24 hours:

.. code-block:: slurm

    #!/bin/bash

    #SBATCH --job-name=<job name>
    #SBATCH -p node
    #SBATCH -n 12
    #SBATCH --time=24:00:00
    #SBATCH --mail-type=FAIL
    #SBATCH --mail-type=END
    #SBATCH --mail-user=<netID>@illinois.edu

A simple script requesting many nodes consisting of 100 cores for 12 hours
without caring about node configuration:

.. code-block:: slurm

    #!/bin/bash

    #SBATCH --job-name=<job name>
    #SBATCH -p sesempi
    #SBATCH -n 100
    #SBATCH --time=12:00:00
    #SBATCH --mail-type=FAIL
    #SBATCH --mail-type=END
    #SBATCH --mail-user=<netID>@illinois.edu

A simple script for requesting a handful of cores but a large amount of memory per
core (16 GB) for 6 hours:

.. code-block:: slurm

    #!/bin/bash

    #SBATCH --job-name=<job name>
    #SBATCH -p node
    #SBATCH -n 4
    #SBATCH --time=12:00:00
    #SBATCH --mem-per-cpu=16g

A more advanced job script requesting 100 cores, 4 GB of memory per core, 12 hours of
wall clock time and wanting a node configuration of 2 sockets with 10 cores each
(i.e. using exclusively keeling-g nodes) and running WRF:

.. code-block:: slurm

    #!/bin/bash

    #SBATCH --job-name=<job name>
    #SBATCH -p sesempi
    #SBATCH -n 100
    #SBATCH --sockets-per-node=2
    #SBATCH --cores-per-socket=10
    #SBATCH --time=12:00:00
    #SBATCH --mem-per-cpu=4096
    #SBATCH --mail-type=FAIL
    #SBATCH --mail-type=END
    #SBATCH --mail-user=<netID>@illinois.edu

    mpirun -np $SLURM_NTASKS ./wrf.exe

A script for accessing a L40S GPU, requesting 96 cores, 2 days of wall clock time, and 253 GB of memory:

.. code-block:: slurm

   #SBATCH -p l40s
   #SBATCH -N 1
   #SBATCH -n 96
   #SBATCH --gres=gpu:L40S:1
   #SBATCH --constraint=tl40s
   #SBATCH --mem=253000
   #SBATCH --time=48:00:00
   #SBATCH --output=batchout
   #SBATCH --mail-user=<netID>@illinois.edu

   module purge
   module load L40S

Helpful SLURM command line options
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+-------------+-------------------------------------------------------------------+
| Command     | Description                                                       |
+-------------+-------------------------------------------------------------------+
| sinfo       | View partition and node information for a system.                 |
|             | Helpful viewing node availability                                 |
+-------------+-------------------------------------------------------------------+
| sbatch      | Submit a job script                                               |
+-------------+-------------------------------------------------------------------+
| squeue      | View information about jobs located in the Slurm scheduling queue |
+-------------+-------------------------------------------------------------------+
| scancel     | Signal job to quit                                                |
+-------------+-------------------------------------------------------------------+
| sshare      | View listing the shares of associations on the system             |
+-------------+-------------------------------------------------------------------+
| sacct       | View accounting data for all jobs                                 |
+-------------+-------------------------------------------------------------------+
| sview       | Graphical interface of the Slurm state                            |
+-------------+-------------------------------------------------------------------+

Information regarding each command may be found `here <https://slurm.schedmd.com/sinfo.html>`_
or by viewing each command's ``man`` page.

Access compute nodes directly using SSH
---------------------------------------

This is not allowed except for monitoring already running jobs. However if you
need to monitor a job (or set up SSH tunnel), you may access the specific compute node by first
identifying the node your job is running on by:

.. code-block:: console

    squeue -u $USER

which will list information regarding the your running and queued jobs with ``NODELIST``
denoting the nodes of your running jobs.
You may then ``ssh`` directly into that node by the following:

.. code-block:: console

    ssh keeling-<node letter and number>
