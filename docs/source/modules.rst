Using Modules
=============

The user environment is controlled using the modules environment management system.
Modules may be loaded, unloaded, or swapped either on a command line or in 
your `$HOME/.modules7` startup file.

To generate a list of your currently loaded software modules::

    module list

To get a list of currently available modules on Keeling::

    module avail

To see a specific list of modules that you may be interested in,
for example GNU compilers and packages::

    module avail gnu 

To see a list of modules that include a keyword to a package such as netcdf4::

    module keyword netcdf4

To load a given module for your environment::

    module load <name of module>

To unload a given module from your current environment::

    module unload <name of module>

To unload all loaded modules from your current environment::

    module purge
