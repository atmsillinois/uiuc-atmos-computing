=========================================
Weather Research and Forecast (WRF) model
=========================================

This document is intended to serve as a reference for new users in building and running the WRF and WPS on Keeling. This guide assumes WRF version 4.0.0 and greater.

Acquiring WRF source code
=========================

WRF Versions 4.0 and greater are available at the `WRF GitHub repository <https://github.com/wrf-model/WRF>`__

To acquire the WRF model there are various options:

* You can clone the WRF repository directly by:

  .. code-block:: console

      git clone https://github.com/wrf-model/WRF.git

* Create your own fork the WRF repository through GitHub (or by `clicking here <https://github.com/wrf-model/WRF/fork>`__) and then acquire the code by:

  .. code-block:: console

    git clone https://github.com/<your GitHub username>/WRF.git

* Acquire a specific version as a tarball from the `WRF GitHub Release page <https://github.com/wrf-model/WRF/releases>`__

  .. note::
    For example, to acquire a tar file of version 4.4.2:

    .. code-block:: console

        wget https://github.com/wrf-model/WRF/releases/download/v4.4.2/v4.4.2.tar.gz

    and you can unpack by:

    .. code-block:: console

        tar -xvzf v4.4.2.tar.gz 

* Older versions (before v4.0) are available for download from the `WRF Users Page <https://www2.mmm.ucar.edu/wrf/users/download/get_source.html>`__

Building WRF
============

Setting up the build environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. There are two compiler options on Keeling to compile WRF: GNU and Intel.

   * If building with GNU compilers, load the following modules:

    .. code-block:: console

       module load mpi/openmpi-x86_64 

   * If building with Intel compilers, load the following modules:

    .. code-block:: console

       module load intel/intel-oneapi
       module load intel/intel-mpi
       module load intel/netcdf4-4.9.2-intel-oneapi

#. Set the path to NetCDF:

   .. code-block:: console

      export NETCDF=`nc-config --prefix`

#. If you are interested in compiling WRF with chemistry options, WRF-Chem can be
   enabled on by:

   .. code-block:: console

      export WRF_CHEM=1

Configuring and compiling WRF
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. To configure WRF, run the configure process by:

   .. code-block:: console
  
    ./configure

   For GNU compilers, select the ``GNU (gfortran/gcc)`` option that is distributed memory (DM) (option ``[34]``)

   For Intel compileres, select the option that says ``INTEL (ifx/icx)`` (option ``[78]`` as of version 4.6.1)

   .. note::

     For older version of WRF, you may have to select ``INTEL (ifort/icc)`` option and edit the ``configure.wrf``
     file to change the compiler to ifx/icx from ifort/icc.

#. Unless you require moving nesting, select ``[1]`` for default nesting.

#. Upon completion of the configure process a file ``configure.wrf`` will be generated
   that contains all the settings for building WRF. This is the file that one may be required to modify in event of a problem or to further modify compiler options/flags.

#. To compile WRF to do a real case and send the output to a log file, run the following:

.. code-block:: console

    ./compile em_real >& compile_WRF.log

.. note::

   WRF also has various idealized cases. These cases are found in the ``test`` directory and
   all available cases can be seen the following command:

   .. code-block:: console

    ./compile -h

   with further information regarding each case found in the README files within each case directory 
   within the ``test`` directory. As an example, if you wanted to compile the LES scenario found in ``test/em_les``

   .. code-block:: console

     ./compile em_les >& compile_WRF_les.log

Building WRF Pre-Processing System (WPS)
========================================

The WRF Pre-Processing System (WPS) is a collection programs that provides data used as
input to the real.exe program. WPS is available on `GitHub <https://github.com/wrf-model/WPS>`_.

#. Change to the directory where your WRF directory can be found. WPS will need a compiled version of WRF to compile
   and will be expecting it in this specific location (``../`` relative to the WPS directory).
   If you do not wish to do this, you can set the ``WRF_DIR`` environment variable to the location of the WRF directory.

#. Clone WPS repository:

   .. code-block:: console

      git clone https://github.com/wrf-model/WPS.git

#. Checkout the major version that matches your version of WRF. For example, if you have compield WRF v4.6.2, checkout the v4.6 branch:

   .. code-block:: console

      git checkout -b v4.6 v4.6

#. To configure:

   .. code-block:: console

    ./configure

   Select the option that matches your WRF compiler choice: option ``[1]`` if you used GNU compilers or ``[17]`` if you used Intel.
   
#. To compile WPS:

   .. code-block:: console

    ./compile >& compile_WPS.log

#. Verify that the directory now contains the following executables:

   * ``geogrid.exe``: creates the geography data
   * ``ungrib.exe``:  decodes the data using tables and creates an intermediate format
   * ``metgrid.exe``: ingests the data and interpolates the fields to the model domain