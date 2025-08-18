===
CM1
===

CM1 is a three-dimensional, time-dependent, non-hydrostatic numerical model that has been developed
primarily by George Bryan at The Pennsylvania State University (PSU) 
and at the National Center for Atmospheric Research (NCAR).
CM1 is designed primarily for simulating atmospheric convection and
is widely used in the atmospheric science community.

Compiling CM1 on Keeling
========================

#. Clone the CM1 repository:

    .. code-block:: console

      git clone https://github.com/NCAR/CM1.git

   or acquire the source code from the NCAR website or other sources if you prefer to
   use a specific version not available from the repository. All versions can be specifically 
   found `here <https://github.com/NCAR/CM1/tree/main/releases>`_.
   For example, you can download a specific release version by copying the link to the desired
   version from the releases page and using ``wget``:

    .. code-block:: console
    
      wget https://www2.mmm.ucar.edu/people/bryan/cm1/cm1r21.0.tar.gz


   And you can extract it using:

    .. code-block:: console
    
      tar -xzf cm1r21.0.tar.gz

#. Change into the cloned directory:

    .. code-block:: console
    
      cd CM1

   or the directory where you extracted the source code if you downloaded a specific version.

   If you cloned the repository and want a specific tag or release version,
   you can identify the tag using the following commands:

    .. code-block:: console

      git tag

   You can then check out a specific version as a branch (here called ``my_version``) by using the tag name:

    .. code-block:: console

      git checkout -b my_version <version_tag>

#. Change into the ``src`` directory:

    .. code-block:: console
    
      cd src

#. Configure the module environment by loading the necessary modules. 
   For using GNU compilers and MPI, you can load the following modules:

    .. code-block:: console

      module load mpi/openmpi-x86_64

#. Build the model and enable MPI by running the Makefile:

    .. code-block:: console

      make USE_MPI=true

   If you prefer NetCDF output file support, you can run:

    .. code-block:: console
     
      make USE_MPI=true USE_NETCDF=true

   This will enable NetCDF support, which is useful for outputting data in a format that can be easily analyzed with various tools
   (albeit it may be less efficient in terms of performance). To specify where the NetCDF library is located,
   you can set the ``NETCDFBASE`` environment variable before running the ``make`` command:

    .. code-block:: console
    
      export NETCDFBASE=/usr

   Additionally, you must edit the ``Makefile`` to set the correct paths for NetCDF include files:

    .. code-block:: makefile

       OUTPUTINC   += -I$(NETCDFBASE)/include
       OUTPUTINC   += -I/usr/lib64/gfortran/modules

   Note that we are adding an extra include path for the Fortran modules, which is necessary for the NetCDF library to work correctly with Fortran.

#. After building, the executable will be located in the ``run`` directory:

    .. code-block:: console
      cm1.exe

Running a testcase on Keeling
=============================

#. Change into the ``run`` directory:

    .. code-block:: console

      cd ../run

#. Run the model:

    .. code-block:: console

      mpirun -np 1 ./cm1.exe

#. If NetCDF is enabled, check output files:

    .. code-block:: console
    
      ncview cm1out.nc
    
Example script for running CM1
==============================

