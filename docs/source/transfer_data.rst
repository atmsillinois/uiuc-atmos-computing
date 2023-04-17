Transferring files to/from Keeling 
==================================

There are several secure options for transferring files to and from your
local machine to remote systems such as Keeling and from Keeling to another
remote machine.

+---------+--------------------------------------------------------------------+
| Methods | Recommended use                                                    |
+---------+--------------------------------------------------------------------+
|sftp     | For interactive                                                    |
+---------+--------------------------------------------------------------------+
|scp      | For use in scripting                                               |
+---------+--------------------------------------------------------------------+
|rsync    | For ensuring two directories are synced when little has changed    |
+---------+--------------------------------------------------------------------+ 
|Globus   | For really large files                                             |
+---------+--------------------------------------------------------------------+


sftp
----

Secure File Transfer Protocol (SFTP) is a network protocol for securely accessing,
transferring and managing large files and sensitive data. It is best used for
interactively moving files between systems.

You can interactively connect to Keeling by:

.. code-block:: console

    sftp <netID>@keeling.earth.illinois.edu

To put a file from your laptop named ``local_file`` on Keeling:

.. code-block:: console

    put local_file file_from_laptop

which will rename this file ``file_from_laptop``. If you do not wish to rename the file,
the last argument can be omitted.

To get a file from Keeling named ``model_output.nc``:

.. code-block:: console

    get model_output.nc

scp
---

SCP (secure copy protocol) is a network file transfer protocol that enables easy
and secure file transfers between a remote system and a local host (or two remote locations).

To copy local file to Keeling:

.. code-block:: console

    scp <options> <local_filename> <HPC_username>@<HPC_hostname>:<HPC_filename>

As an example to copy a single file called ``local_file`` to Keeling to your home directory:

.. code-block:: console

    scp local_file <netID>@keeling.earth.illinois.edu:

As an example to copy a whole directory for a specific location on Keeling:

.. code-block:: console

    scp -r local_dir <netID>@keeling.earth.illinois.edu:data/

copies the local directory ``local_dir`` to be found in the directory ``$HOME/data`` on Keeling. 

The general form to copy a file from a remote machine to your local system:

.. code-block:: console
 
    scp <options> <HPC_username>@<HPC_hostname>:<HPC_filename> <local_filename>

To copy a single file `model_output/output.nc` from Keeling to your local system 
to your current directory for example:

.. code-block:: console

    scp <netID>@keeling.earth.illinois.edu:model_output/output.nc . 

To copy the entire ``model_output`` directory from Keeling to your local system:

.. code-block:: console

    scp -r netID@keeling.earth.illinois.edu:model_output model_output_from_keeling

which copies the directory `model_output` from Keeling to your local system and
places it in the directory `model_output_from_keeling`. The file paths are relative
to your `$HOME` directory unless the full file path is specied.

rsync
-----

Rsync (remote sync) is a remote and local file synchronization tool.
It relies on an algorithm to minimize the amount of data that must be transfered
between the systems based on only copying the portions of files that have changed.
This makes it greatly efficient when synchronizing a step of files or directories
that have minor changes. This in in contrast to transferring all files as methods
such as sftp and scp.


You can sync a local directory `data` to Keeling by:

.. code-block:: console

    rsync -a ~/data <netID>@keeling.earth.illinois.edu:destination_directory

You can sync a remote directory at `/home/username/data` to your local machine by:

.. code-block:: console

    rsync -a <netID>@keeling.earth.illinois.edu:/home/username/data place_to_sync_on_local_machine

.. todo:: Add script example

Globus
------

Globus Online is a service that automates moving files between different systems. 
It allows users queue file transfers which are then done asynchronously in the background.
Globus offers a number of advantages such as monitoring performance, retrying on failures
and recover from failures automatically when possible.
Globus Online transfers files between "endpoints" which are systems that are
registered with the Globus Online service.

To transfer files from your local machine to a Globus Endpoint, you can use
Globus Connect.

.. todo:: Expand

Globus Connect Personal
^^^^^^^^^^^^^^^^^^^^^^^

Globus Connect Personal turns your laptop or other personal computer into a
Globus endpoint. With Globus Connect Personal you can share and transfer files
to/from a local machine to a campus cluster (such as Keeling).

.. todo:: Include some step by step directions of how to do this.
