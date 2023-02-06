Accessing compute nodes
=====

The login node of Keeling is useful for lightweight computing that requires not a lot of memory
and not a large number of cores. Programs that require large amounts of memory or CPUs will be
killed if run on the head node.

To avoid this issue, users must access the compute nodes. This can be done in two ways,
interactively or using the batch system which uses SLURM. https://slurm.schedmd.com/

Interactive computing
---------------

Compute

Batch computing
---------------

The login node of Keeling is useful for lightweight computing that requires not a lot of memory
and not a large number of cores. Programs that require large amounts of memory or CPUs will be
killed if run on the head node.



It can also be used to launch interactive or non-interactive batch jobs using the cluster’s scheduler slurm. Keeling is a shared resource and is monitored for abuse. CPU or memory intensive jobs will be automatically killed - these should be run on compute nodes to maintain system stability.

Example scripts:::

    Script here

TODO: Specifics of what options mean

| Option      | Description |
| :---        |    :----:   |
| Header      | Title       |
| Paragraph   | Text        |


Helpful SLURM command line options:

| Command      | Description |
| :---        |    :----:   |
| sinfo       |             |
| squeue      |             |
| scancel     |             |
| sshare      |             |
| sacct       |             |

Partitions
______________

Keeling consists of different partitions that you may submit your job to depending on your computing needs.

+-------------+------------------------+--------------------+
| Option      | Description            | Max wall clock time| 
+-------------+------------------------+--------------------+
| node        | Single node            | 10 days            |
+-------------+------------------------+--------------------+
| sesempi     | MinNodes=2 MaxNodes=8  | 7 days             |
+-------------+------------------------+--------------------+
| sesebig     | MinNodes=9 MaxNodes=32 | 2 days             |
+-------------+------------------------+--------------------+
| gpu         |        Access GPU      | 7 days             | 
+-------------+------------------------+--------------------+

Compute node details:

| Name        | Description |
| :---        |    :----:   |
| a           |           |
| b           |           |
| c           |           |
| d           |           |
| e           |           |
| f           |           |
| g           |           |

Access compute nodes directly using SSH
---------------

This is not allowed except for monitoring already running jobs. However if you
need to monitor a job, you may access the specific compute node by first
identifying the node your job is running on by::

    squeue -u $USER

which will list all your current running and queued jobs.
You may then `ssh` directly into that node by the following::

    ssh keeling-<node letter and number>
