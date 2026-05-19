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

These instructions assume you are using using the newest versions of CM1.
For older versions, you may need to edit the ``Makefile`` to set the correct
paths for NetCDF and for the correct architecture options you want.
However, those paths and settings should be similar to the instructions below.

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

    For using Intel compilers and MPI, you can load the following modules:

    .. code-block:: console

      module load intel/intel-oneapi
      module load intel/intel-mpi
      module load intel/netcdf4-4.9.2-intel-oneapi

#. To set the compiler, you can set the ``FC`` environment variable before running the ``make`` command:

    * For GNU:

     .. code-block:: console

      export FC=gfortran

    * For Intel:

     .. code-block:: console

      export FC=ifx

     Two additional changes are required to the ``Makefile``, which are:
        - Change the line which sets the compilers flags for ``ifort`` to be for ``ifx`` so that flags are set correctly for the Intel Fortran compiler.
        - Change the line which sets the compiler to ``mpif90`` to be ``mpiifx`` so that the correct MPI is called.

#. Build the model and enable MPI by running the Makefile:

    .. code-block:: console

      make USE_MPI=true

   If you prefer NetCDF output file support, you can run:

    .. code-block:: console
     
      make USE_MPI=true USE_NETCDF=true

   This will enable NetCDF support, which is useful for outputting data in a format that can be easily analyzed with various tools
   (albeit it may be less efficient in terms of performance). To specify where the NetCDF library is located,
   you can set the ``NETCDFBASE`` environment variable before running the ``make`` command:

    * For GNU:

     .. code-block:: console
    
      export NETCDFBASE=/usr

     Additionally for GNU, you must edit the ``Makefile`` to set the correct paths for NetCDF include files:

      .. code-block:: makefile

        OUTPUTINC   += -I$(NETCDFBASE)/include
        OUTPUTINC   += -I/usr/lib64/gfortran/modules

     Note that we are adding an extra include path for the Fortran modules, which is necessary for the NetCDF library to work correctly with Fortran.

    * For Intel:

     .. code-block:: console
    
       export NETCDFBASE=/sw/netcdf-4.9.2-intel-oneapi/

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

Previous versions
=================

Versions predating vXX.X may require the modication of the src/Makefile to 
set the correct paths for NetCDF and architecture options.

For example, you may need to set the following variables in the Makefile:

For GNU:

  .. code-block:: makefile

    #  multiple processors, distributed memory (MPI), gnu compiler
    FC = mpif90
    OPTS = -ffree-form -ffree-line-length-none -O2 -finline-functions
    CPP  = cpp -C -P -traditional -Wno-invalid-pp-token -ffreestanding
    DM = -DMPI

For Intel:

  .. code-block:: makefile

    #  multiple processors, distributed memory (MPI), Intel compiler 
    #      (eg, NCAR's yellowstone/cheyenne)
    FC   = mpiifx
    OPTS = -O3 -assume byterecl -fp-model precise -ftz -no-fma
    CPP  = cpp -C -P -traditional -Wno-invalid-pp-token -ffreestanding
    DM   = -DMPI

Note that ``-Xhost`` has been removed as an OPT as there are possibly some architecture
compatitbility issues with compiling on login node and running on compute nodes.

For the NetCDF section, edit the following so that the library is found:

  .. code-block:: makefile

    #OUTPUTINC = -I$(NETCDF)/include
    #OUTPUTLIB = -L$(NETCDF)/lib
    #OUTPUTOPT = -DNETCDF -DNCFPLUS
    #LINKOPTS  = -lnetcdf -lnetcdff 

For GNU:

  .. code-block:: makefile

    OUTPUTINC = -I/usr/lib64/gfortran/modules
    OUTPUTLIB = -L/usr/lib64
    OUTPUTOPT = -DNETCDF -DNCFPLUS
    LINKOPTS  = -lnetcdf -lnetcdff

For Intel:

  .. code-block:: makefile

    OUTPUTINC = -I/sw/netcdf-4.9.2-intel-oneapi/include
    OUTPUTLIB = -L/sw/netcdf-4.9.2-intel-oneapi/lib
    OUTPUTOPT = -DNETCDF -DNCFPLUS
    LINKOPTS  = -lnetcdf -lnetcdff
