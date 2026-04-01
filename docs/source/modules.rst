Using Modules
=============

The user environment is controlled using the modules environment management system.
Modules may be loaded, unloaded, or swapped either on a command line or in 
your `$HOME/.modules9` startup file.

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

To display information about a module (description, paths, environment variables) before loading it:

.. code-block:: console

    module show <name of module>

To load a given module for your environment:

.. code-block:: console

    module load <name of module>

To unload a given module from your current environment:

.. code-block:: console

    module unload <name of module>

To swap one module for another, for example when switching compiler versions:

.. code-block:: console

    module swap <old module> <new module>

To unload all loaded modules from your current environment:

.. code-block:: console

    module purge

Compilers
---------

Keeling has three available compiler options:

.. list-table::
   :header-rows: 1

   * - Compiler version
     - Module
     - C
     - C++
     - Fortran
   * - gcc 11.5.0
     - default
     - gcc
     - g++
     - gfortran
   * - intel-15.0
     - ``module load intel/intel-15.0.3``
     - icc
     - icpc
     - ifort
   * - intel-api
     - ``module load intel/intel-oneapi``
     - icx
     - icpx
     - ifx

Note that ``intel-api`` uses Intel's newer oneAPI LLVM-based compilers (``icx``, ``icpx``, ``ifx``),
which differ from the classic Intel compilers (``icc``, ``icpc``, ``ifort``) used in ``intel-15.0``.
Code that builds with one may require changes to build with the other.

For MPI:

.. list-table::
   :header-rows: 1

   * - Compiler version
     - Module
     - C
     - C++
     - Fortran
   * - gcc 11.5.0
     - ``module load mpi/openmpi-x86_64``
     - mpicc
     - mpic++
     - mpif90
   * - intel-15.0
     - ``module load intel/openmpi-4.1.8-intel-15.0.3``
     - mpicc
     - mpic++
     - mpif90
   * - intel-api
     - ``module load intel/intel-mpi``
     - mpiicx
     - mpiicpx
     - mpiifx

GPU (CUDA) Environment
---------------

The ``L40S`` module configures the GPU environment, including loading ``nvcc`` (the CUDA compiler)
and various other CUDA toolkit utilities:

.. code-block:: console

    module load L40S