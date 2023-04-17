Using Modules
=============

The user environment is controlled using the modules environment management system.
Modules may be loaded, unloaded, or swapped either on a command line or in 
your `$HOME/.modules7` startup file.

To generate a list of your currently loaded software modules:

.. code-block:: console

    module list

To get a list of currently available modules on Keeling:

.. code-block:: console

    module avail

To see a specific list of modules that you may be interested in,
for example GNU compilers and packages:

.. code-block:: console

    module avail gnu 

To see a list of modules that include a keyword to a package such as netcdf4:

.. code-block:: console

    module keyword netcdf4

To load a given module for your environment:

.. code-block:: console

    module load <name of module>

To unload a given module from your current environment:

.. code-block:: console

    module unload <name of module>

To unload all loaded modules from your current environment:

.. code-block:: console

    module purge
