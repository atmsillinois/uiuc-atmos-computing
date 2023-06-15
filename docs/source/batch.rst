Accessing compute nodes
=======================

The login node of Keeling is useful for lightweight computing that requires not a lot of memory
and not a large number of cores. Programs that require large amounts of memory or CPUs will be
killed if run on the head node.

To avoid this issue, users must access the compute nodes. This can be done in two ways,
interactively or using the batch system which uses `SLURM <https://slurm.schedmd.com/>`__.

Interactive computing
---------------------

To request an interactive job, the ``qlogin`` command is used. An example of a request:

.. code-block:: console

    qlogin -n 20 --mem-per-cpu=4096 --time=12:00:00 --job-name=interactive

will utilize 20 cores with 4 GB of memory per core, a wall clock limit of 12 hours.
Note: Interactive job launched by ``qlogin`` are only allowed one node.

To finalize your interactive compute job you can do ``exit`` to release
the node to be available to others.

Alternatively you can use ``srun`` to access other partitions to request more resources.

To interactively compute on a GPU node:

.. code-block:: console

    qlogin -p gpu -n 20 --gres=gpu:K40m:1

Batch computing
---------------

The login node of Keeling is useful for lightweight computing that requires not a lot of memory
and not a large number of cores. Programs that require large amounts of memory or CPUs will be
killed if run on the head node.

It can also be used to launch interactive or non-interactive batch jobs using the cluster’s scheduler slurm. Keeling is a shared resource and is monitored for abuse. CPU or memory intensive jobs will be automatically killed - these should be run on compute nodes to maintain system stability.

Example scripts

A simple script requesting a single node consisting of 12 cores for 24 hours:

.. code-block:: slurm

    #!/bin/bash

    #SBATCH --job-name=PARTMC3D
    #SBATCH -p node
    #SBATCH -n 12 
    #SBATCH --time=24:00:00
    #SBATCH --mail-type=FAIL
    #SBATCH --mail-type=END
    #SBATCH --mail-user=jcurtis2@illinois.edu

A simple script requesting many nodes consisting of 100 cores for 12 hours 
without caring about node configuration:

.. code-block:: slurm

    #!/bin/bash

    #SBATCH --job-name=PARTMC3D
    #SBATCH -p sesempi 
    #SBATCH -n 100 
    #SBATCH --time=12:00:00
    #SBATCH --mail-type=FAIL
    #SBATCH --mail-type=END
    #SBATCH --mail-user=jcurtis2@illinois.edu

A simple script for requesting a handful of cores but a large amount of memory per
core (16 GB) for 6 hours:

.. code-block:: slurm

    #!/bin/bash

    #SBATCH --job-name=PARTMC3D
    #SBATCH -p node 
    #SBATCH -n 4
    #SBATCH --time=12:00:00
    #SBATCH --mem-per-cpu=16g

A more advanced job script requesting 100 cores, 4 GB of memory per core, 12 hours of
wall clock time and wanting a node configuration of 2 sockets with 10 cores each:

.. code-block:: slurm

    #!/bin/bash

    #SBATCH --job-name=PARTMC3D
    #SBATCH -p sesempi
    #SBATCH -n 100 
    #SBATCH --sockets-per-node=2
    #SBATCH --cores-per-socket=10
    #SBATCH --time=12:00:00
    #SBATCH --mem-per-cpu=4096
    #SBATCH --mail-type=FAIL
    #SBATCH --mail-type=END
    #SBATCH --mail-user=jcurtis2@illinois.edu

    cd WRFV3/test/em_real/
    mpirun -np $SLURM_NTASKS ./wrf.exe

Helpful SLURM command line options:

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

Partitions
______________

Keeling consists of many different partitions that you may submit your job script
to depending on your computing needs.

+-------------+------------------------+--------------------+
| Option      | Description            | Max wall clock time| 
+-------------+------------------------+--------------------+
| node        | Shared single node     | 10 days            |
+-------------+------------------------+--------------------+
| seseml      | Exclusive single node  | 7 days             |
+-------------+------------------------+--------------------+
| sesempi     | MinNodes=2 MaxNodes=8  | 7 days             |
+-------------+------------------------+--------------------+
| sesebig     | MinNodes=9 MaxNodes=32 | 2 days             |
+-------------+------------------------+--------------------+
| gpu         | Access to GPU (K40m)   | 7 days             |
+-------------+------------------------+--------------------+

Keeling consists of the following types of nodes with the following features

+-------------+------------------+---------------+
| Name        | Cores per node   | Real memory   |
+-------------+------------------+---------------+
| a           | 4                | 12000         |
+-------------+------------------+---------------+
| b           | 8                | 16000 - 32000 |
+-------------+------------------+---------------+
| c           | 12               | 32160 or 72480|
+-------------+------------------+---------------+
| d           | 12               | 64300 or 80500|
+-------------+------------------+---------------+
| e           | 12               | 64300         |
+-------------+------------------+---------------+
| f           | 8                | 24000         |
+-------------+------------------+---------------+
| g           | 20               | 128800        |
+-------------+------------------+---------------+
| gpu         | 20               | 256200        |
+-------------+------------------+---------------+
| h           | 20 or 24         | 256200        |
+-------------+------------------+---------------+
| i           | 32               | 192000        |
+-------------+------------------+---------------+
| j           | 48               | 253000        |
+-------------+------------------+---------------+

Access compute nodes directly using SSH
---------------------------------------

This is not allowed except for monitoring already running jobs. However if you
need to monitor a job, you may access the specific compute node by first
identifying the node your job is running on by:

.. code-block:: console

    squeue -u $USER

which will list information regarding the your running and queued jobs with ``NODELIST``
denoting the nodes of your running jobs.
You may then ``ssh`` directly into that node by the following:

.. code-block:: console 

    ssh keeling-<node letter and number>
