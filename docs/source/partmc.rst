PartMC
======

PartMC: Particle-resolved Monte Carlo code for atmospheric aerosol simulation

**Source:** <https://github.com/compdyn/partmc>

**Homepage:** <http://lagrange.mechse.illinois.edu/partmc/>


Installing PartMC
-----------------

1. To build PartMC on Keeling, first load the required modules:

   .. code-block:: console

       module load gnu/gnu-9.3.0
       module load gnu/netcdf4-4.7.4-gnu-9.3.0
       module load gnu/openmpi-3.1.6-gnu-9.3.0

2. Set the following flags so Keeling finds the right compilers:

   .. code-block:: console

       export FC=gfortran
       export CC=gcc

   or for MPI enabled code:

   .. code-block:: console

       export FC=mpif90
       export CC=mpicc

3. Get the PartMC code from github:

   .. code-block:: console

       git clone https://github.com/compdyn/partmc.git

4. Make the build directory:

   .. code-block:: console

       mkdir build

5. Change into the build directory:

   .. code-block:: console

       cd build

6. To set the NetCDF path variable ``NETCDF_HOME``:

   .. code-block:: console

       export NETCDF_HOME=`nc-config --prefix`

7. If you compiled MOSAIC, set the ``MOSAIC_HOME`` as:

   .. code-block:: console

       export MOSAIC_HOME=<where ever you installed it>

   If you compiled CAMP, set the ``CAMP_HOME`` as:

   .. code-block:: console

       export CAMP_HOME=<where ever you installed it>    

   If you compile SUNDIALS, set the ``SUNDIALS_HOME`` as:

   .. code-block:: console

       export SUNDIALS_HOME=<where ever you installed it>

8. PartMC can be easily configured using the graphical interface supplied by ccmake:

   .. code-block:: console

       ccmake3 ..

   When in the GUI menu, press ``[c]`` to configure.

   At this point, turn on MOSAIC (or other options) as desired.

   When done configuring, press ``[c]`` to configure again.

   Finally, press ``[g]`` to generate.

9. To build PartMC:

   .. code-block:: console

       make

   Upon completion of the build process, PartMC test suite may be executed by:

   .. code-block:: console

       make test

Installing chemistry via Chemistry Across Multiple Phases (CAMP)
----------------------------------------------------------------

CAMP is available `here <https://github.com/open-atmos/camp>`_

Installing SUNDIALS for cloud parcel
------------------------------------

SUNDIALS (SUNDIALS is a SUite of Nonlinear and DIfferential/ALgebraic equation Solvers)
is available for download `here <https://computing.llnl.gov/projects/sundials/sundials-software>`_.
