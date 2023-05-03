PartMC
======

PartMC: Particle-resolved Monte Carlo code for atmospheric aerosol simulation

**Source:** <https://github.com/compdyn/partmc>

**Homepage:** <http://lagrange.mechse.illinois.edu/partmc/>


Installing PartMC
-----------------

#. To build PartMC on Keeling, first load the required modules:

   .. code-block:: console

       module load gnu/gnu-9.3.0
       module load gnu/netcdf4-4.7.4-gnu-9.3.0
       module load gnu/openmpi-3.1.6-gnu-9.3.0

#. Set the following flags so Keeling finds the right compilers:

   .. code-block:: console

       export FC=gfortran
       export CC=gcc

   or for MPI enabled code:

   .. code-block:: console

       export FC=mpif90
       export CC=mpicc

#. Get the PartMC code from github:

   .. code-block:: console

       git clone https://github.com/compdyn/partmc.git

#. Make the build directory:

   .. code-block:: console

       mkdir build

#. Change into the build directory:

   .. code-block:: console

       cd build

#. To set the NetCDF path variable ``NETCDF_HOME``:

   .. code-block:: console

       export NETCDF_HOME=`nc-config --prefix`

#. If you compiled MOSAIC, set the ``MOSAIC_HOME`` as:

   .. code-block:: console

       export MOSAIC_HOME=<where you installed it>

   If you compiled CAMP, set the ``CAMP_HOME`` as:

   .. code-block:: console

       export CAMP_HOME=<where you installed it>

   If you compile SUNDIALS, set the ``SUNDIALS_HOME`` as:

   .. code-block:: console

       export SUNDIALS_HOME=<where you installed it>

#. PartMC can be easily configured using the graphical interface supplied by ccmake:

   .. code-block:: console

       ccmake3 ..

   When in the GUI menu, press ``[c]`` to configure.
   At this point, turn on MOSAIC (or other options) as desired.
   When done configuring, press ``[c]`` to configure again.
   Finally, press ``[g]`` to generate.

#. To build PartMC:

   .. code-block:: console

       make

   Upon completion of the build process, PartMC test suite may be executed by:

   .. code-block:: console

       make test

Installing chemistry via Chemistry Across Multiple Phases (CAMP)
----------------------------------------------------------------

CAMP is available at `open-atmos/camp <https://github.com/open-atmos/camp>`_.
It can be cloned by:

.. code-block:: console

   git clone https://github.com/open-atmos/camp.git

CAMP has the following library dependencies:

   * SuiteSparse
   * JSON-Fortran
   * CVODE

Building SuiteSparse
^^^^^^^^^^^^^^^^^^^^

#. Download SuiteSparse:

   .. code-block:: console

      curl -kLO http://faculty.cse.tamu.edu/davis/SuiteSparse/SuiteSparse-5.1.0.tar.gz

#. Untar the tar file:

   .. code-block:: console

      tar -zxvf SuiteSparse-5.1.0.tar.gz

#. Set some environmental variables

   .. code-block:: console
 
      export CXX=g++
      export SUITE_SPARSE_HOME=<where you like to install it>

#. Build and install the library:

   .. code-block:: console

      make install INSTALL=$SUITE_SPARSE_HOME BLAS="-L/lib64 -lopenblas"

Building JSON-Fortran
^^^^^^^^^^^^^^^^^^^^^

#. Download JSON-Fortran:

   .. code-block:: console

      curl -LO https://github.com/jacobwilliams/json-fortran/archive/6.1.0.tar.gz

#. Untar the tar file:

   .. code-block:: console

      tar -zxvf 6.1.0.tar.gz

#. Change into the untarred directory:

   .. code-block:: console

      cd json-fortran-6.1.0

#. Set some environmental variables:

   .. code-block:: console

      export JSON_FORTRAN_INSTALL=<where you want it>
      export FC=gfortran

#. Create and change into a build directory:

   .. code-block:: console

      mkdir build
      cd build

#. Configure, compile and install:

   .. code-block:: console

      cmake -D CMAKE_INSTALL_PREFIX=$JSON_FORTRAN_INSTALL  -D SKIP_DOC_GEN:BOOL=TRUE  ..
      make install

#. Set the environment variable ``JSON_FORTRAN_HOME`` so CAMP will locate it as:

   .. code-block:: console

      export JSON_FORTRAN_HOME=$JSON_FORTRAN_INSTALL/jsonfortran-gnu-6.1.0/

Building CVODE
^^^^^^^^^^^^^^

#. Change back to CAMP directory and untar CVODE:

   .. code-block:: console

      tar -zxvf cvode-3.4-alpha.tar.gz

#. Change into the `cvode-3.4-alpha` directory and create a build directory and change into it:

   .. code-block:: console

      cd cvode-3.4-alpha
      mkdir build
      cd build

#. Set the environmental variable for the install location and so CAMP will locate it as:

   .. code-block:: console

      export SUNDIALS_HOME=<where you want to install it>

#. Configure by:

   .. code-block:: console

      cmake -D CMAKE_BUILD_TYPE=release -D KLU_ENABLE:BOOL=TRUE -D KLU_LIBRARY_DIR=$SUITE_SPARSE_HOME/lib -D KLU_INCLUDE_DIR=$SUITE_SPARSE_HOME/include -D EXAMPLES_INSTALL:BOOL=FALSE -D CMAKE_INSTALL_PREFIX=$SUNDIALS_HOME ..

#. Install it to ``SUNDIALS_HOME``:

   .. code-block:: console

      make install

Building CAMP
^^^^^^^^^^^^^

#. Move back to the main CAMP directory where the README is located.
   Make a directory called build and change into it:

   .. code-block:: console

      mkdir build
      cd build

#. Configure CAMP:

   .. code-block:: console

      ccmake3 ..

   Inside ccmake press ``[c]`` to configure, edit the values as needed, press ``[c]`` again, then ``[g]`` to generate. Optional libraries can be activated by setting the ENABLE variable to ON.

#. Compile CAMP and test it as follows. Some tests may fail due to bad random initial conditions, so re-run the tests a few times to see if failures persist.

   .. code-block:: console

      make
      make test

Installing SUNDIALS for cloud parcel
------------------------------------

SUNDIALS (SUNDIALS is a SUite of Nonlinear and DIfferential/ALgebraic equation Solvers)
is available for download `from LLNL <https://computing.llnl.gov/projects/sundials/sundials-software>`_.

