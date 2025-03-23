PartMC
======

PartMC: Particle-resolved Monte Carlo code for atmospheric aerosol simulation

**Source code:** https://github.com/compdyn/partmc

**Homepage:** http://lagrange.mechse.illinois.edu/partmc/


Installing PartMC
-----------------


#. There are two compiler options (GNU and Intel) to compile PartMC on Keeling.
   To build PartMC on Keeling, first load the required modules:

   * GNU Compilers, the default environment will work (GNU 11.5.0)

   * Intel compilers:

      .. code-block:: console

       module load intel/intel-oneapi
       module load intel/netcdf4-4.9.2-intel-oneapi

#. Set the following environment variables so PartMC finds the desired compilers:

   * GNU compilers:

     .. code-block:: console

       export FC=gfortran
       export CC=gcc

   * Intel compilers:

     .. code-block:: console

       export FC=ifx
       export CC=ifc

#. Acquire the PartMC code from Github:

   .. code-block:: console

       git clone https://github.com/compdyn/partmc.git

#. Set the NetCDF path variable ``NETCDF_HOME`` to the location of the NetCDF library:

   .. code-block:: console

       export NETCDF_HOME=`nc-config --prefix`

#. Configure the environment variables for the optional libraries.

   * If you compiled MOSAIC (see instructions on :ref:`MOSAIC`), set the ``MOSAIC_HOME`` environmental variable as:

     .. code-block:: console

       export MOSAIC_HOME=<where you installed it>

   * If you compiled CAMP (see instructions on :ref:`CAMP`), set the ``CAMP_HOME`` environmental variable as:

     .. code-block:: console

       export CAMP_HOME=<where you installed it>

   * If you compiled SUNDIALS (see instructions on :ref:`SUNDIALS`), set the ``SUNDIALS_HOME`` environmental variable  as:

     .. code-block:: console

       export SUNDIALS_HOME=<where you installed it>


#. Set the build type to ``release`` by setting the ``CMAKE_BUILD_TYPE`` variable to ``release``:

   .. code-block:: console

       export CMAKE_BUILD_TYPE=release

#. Make the ``build`` directory:

   .. code-block:: console

       mkdir build

#. Change into the ``build`` directory:

   .. code-block:: console

       cd build

#. PartMC can be easily configured using the graphical interface supplied by ccmake, which can be called by:

   .. code-block:: console

       ccmake ..

#. Press ``[c]`` to do the initial configure.

#. At this point turn on different options as desired. For example:

     * ``USE_MOSAIC``: turns MOSAIC on for chemistry.
     * ``USE_CAMP``: turns CAMP on for chemistry.
     * ``USE_SUNDIALS``: turns SUNDIALS on for cloud parcel condensation model.
     * ``USE_GSL``: turns on the GNU Scientific Library for random number generation.

   When done configuring your options, press ``[c]`` to configure again.
   Finally, press ``[g]`` to generate the files to build PartMC.

#. To build PartMC, call the following command:

   .. code-block:: console

       make

#. Upon completion of the build process to verify the correctness of the build process, the
   PartMC test suite may be executed by:

   .. code-block:: console

       make test

.. _MOSAIC:

Installing chemistry via MOSAIC
-------------------------------

#. MOSAIC is currently available only by request. Please contact the relevant
   developers for access to the code.

#. Copy ``Makefile.local.gfortran`` to ``Makefile.local``:

   .. code-block:: console

      cp Makefile.local.gfortran Makefile.local

#. Edit ``Makefile.local`` depending on your compiler choice:

   * For GNU:

       .. code-block:: console

         FC = gfortran
         FFLAGS = -g -Idatamodules -Jdatamodules -fallow-argument-mismatch

   * For Intel:

      .. code-block:: console

         FC = ifx
         FFLAGS = -Idatamodules -module datamodules

#. Build MOSAIC

   .. code-block:: console

      make

#. Verify the build by checking that the ``libmosaic.a`` library file was created
and that ``*.mod`` files were created in the ``datamodules`` directory.

.. _CAMP:

Installing chemistry via Chemistry Across Multiple Phases (CAMP)
----------------------------------------------------------------

CAMP is on Github available at `open-atmos/camp <https://github.com/open-atmos/camp>`__.
It can be cloned by:

.. code-block:: console

   git clone https://github.com/open-atmos/camp.git

Or you can fork it and similarly clone your fork.

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

      cmake -D CMAKE_BUILD_TYPE=release -D KLU_ENABLE:BOOL=TRUE \
            -D KLU_LIBRARY_DIR=$SUITE_SPARSE_HOME/lib -D KLU_INCLUDE_DIR=$SUITE_SPARSE_HOME/include \
            -D EXAMPLES_INSTALL:BOOL=FALSE -D CMAKE_INSTALL_PREFIX=$SUNDIALS_HOME ..

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

      ccmake ..

   Inside ccmake press ``[c]`` to configure, edit the values as needed, press ``[c]`` again, then ``[g]`` to generate.
   Optional libraries can be activated by setting the respective ``ENABLE`` variable to ON.

#. Compile CAMP and test it as follows. Some tests may fail due to bad random initial conditions, so re-run the tests a few times to see if failures persist.

   .. code-block:: console

      make
      make test

.. _SUNDIALS:

Installing SUNDIALS
-------------------

SUNDIALS is a SUite of Nonlinear and DIfferential/ALgebraic equation Solvers.

#. Acquire the code `from LLNL <https://computing.llnl.gov/projects/sundials/sundials-software>`__.

#. Untar the tar file:

   .. code-block:: console

      tar -zxvf sundials-*.tar.gz

#. Change into untarred directory:

   .. code-block:: console

      cd sundials-*/

   and make a build directory to change into:

   .. code-block:: console

      mkdir build
      cd build

#. Set the install location for SUNDIALS to place libraries and for PartMC to find it:

   .. code-block:: console

      export SUNDIALS_HOME=<where you want to install it>

#. Configure:

   .. code-block:: console

      cmake -D CMAKE_BUILD_TYPE=release -D EXAMPLES_INSTALL:BOOL=FALSE \
            -D CMAKE_INSTALL_PREFIX=$SUNDIALS_HOME ..

#. Compile, test and install:

   .. code-block:: console

      make
      make test
      make install

.. _TChem:

Installing TChem
----------------

#. Acquire the code from Github:

   .. code-block:: console

      git clone --recursive https://github.com/PCLAeroParams/TChem-atm.git

#. Launch an interactive session to a GPU node

   .. code-block:: console

      qlogin -p l40s -n 96 --gres=gpu:L40S:1 --mem=250000

#. Load the required modules:

   .. code-block:: console

      module purge
      module load L40S

#. Create a build directory and change into it:

   .. code-block:: console

      cd TChem-atm/
      mkdir build
      cd build

#. Copy the build scripts from the TChem-atm repository

   .. code-block:: console

      cp ../scripts/tpl_build.sh
      cp ../scripts/build_script.sh
   
#. Edit the third party build script ``tpl_build.sh`` to include the
   path of the TChem-atm repository:

   .. code-block:: console

      TCHEM_REPOSITORY_PATH=<path to TChem-atm directory>

   Disable building OpenBLAS by setting:

   .. code-block:: console

      INSTALL_OPENBLAS="OFF"

#. Run the third party build script:

   .. code-block:: console

      ./tpl_build.sh >& tpl_host.log

#. Edit the third party build script to enable CUDA:

   .. code-block:: console

      CUDA="ON"

#. Run ``tpl_build.sh`` to build the CUDA version of the libraries:

   .. code-block:: console

      ./tpl_build.sh >& tpl_gpu.log

#. Edit the TChem build script ``build_script.sh``:

   .. code-block:: console

      TCHEM_REPOSITORY_PATH=<path to TChem-atm directory>
      BUILD_PATH=$(pwd)

   If you want to build the Release version, change ``BUILD_TYPE`` from DEBUG to RELEASE:

   .. code-block:: console

      BUILD_TYPE=RELEASE

#. Run the TChem build script:

   .. code-block:: console

      ./build_script.sh >& build_host.log

#. Change the build script to build the CUDA versions:

   .. code-block:: console

      CUDA="ON"

#. Run the TChem build script:

   .. code-block:: console

      ./build_script.sh >& build_gpu.log

#. Run the tests, depending on the build type you set. 

   ``DEBUG`` version:

   .. code-block:: console

      ctest --test-dir HOST/DEBUG/build/tchem_atm/
      ctest --test-dir CUDA/DEBUG/build/tchem_atm/

   ``RELEASE`` version:

   .. code-block:: console

      ctest --test-dir HOST/RELEASE/build/tchem_atm/
      ctest --test-dir CUDA/RELEASE/build/tchem_atm/