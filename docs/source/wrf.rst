Weather Research and Forecast (WRF) model
=========================================

This document is intended to serve as a reference for new users in building and running the WRF and WPS on Keeling. This assumes Versions 3.9.1 and greater.

WRF Versions 4.0 and greater are available on GitHub `here <https://github.com/wrf-model/WRF>`__

Older versions are available for download from `here <https://www2.mmm.ucar.edu/wrf/users/download/get_source.html>`__

It is recommended that you fork the WRF repository through GitHub and then acquire the code by:

.. code-block:: console

    git clone git@github.com:<your github username>/WRF.git

or acquire a specific version from the Release page for wrf-model on GitHub
`here <https://github.com/wrf-model/WRF/releases>`__. For example, to acquire a
tar file of version 4.4.2:

.. code-block:: console

    wget https://github.com/wrf-model/WRF/releases/download/v4.4.2/v4.4.2.tar.gz

and you can unpack by:

.. code-block:: console

    tar -xvzf v4.4.2.tar.gz 

Building WRF with GNU compilers
-------------------------------

Configuring environment
^^^^^^^^^^^^^^^^^^^^^^^

Load the following modules:

.. code-block:: console

    module load gnu/gnu-9.3.0
    module load gnu/netcdf4-4.7.4-gnu-9.3.0
    module load gnu/openmpi-3.1.6-gnu-9.3.0

Set the path to NetCDF:

.. code-block:: console

    export NETCDF=`nc-config --prefix`

If you are interested in compiling WRF with chemistry options, WRF-Chem can be
enabled on by:

.. code-block:: console

    export WRF_CHEM=1

Configuring and compiling WRF
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To configure WRF:

.. code-block:: console
  
    ./configure

Select the option that is distributed memory (DM) with GNU (option 34 as of 2/6/2023)

Select 1 for nesting.

Upon completion of the configure process a file `configure.wrf` will be generated
that contains all the settings for building WRF. This is the file that one
may be required to modify in event of a problem or to modify compiler options.

To compile the real model and send the output to a log file:

.. code-block:: console

    ./compile em_real >& compile_WRF_GNU.log

WRF also has various idealized cases. These cases are found in the `test` directory and
all available cases can be seen by

.. code-block:: console

    ./compile -h

with further information regarding each case found in the README files within each case.
As an example, if you wanted to compile the LES scenario found in `test/em_les`

.. code-block:: console

    ./compile em_les >& compile_WRF_les_GNU.log

Building WRF Pre-Processing System (WPS)
----------------------------------------

WPS is available `here <https://github.com/wrf-model/WPS>`_.

To configure:

.. code-block:: console

    ./configure

Select option YY.

Then to compile:

.. code-block:: console

    ./compile >& compile_WPS.log

Running WPS
-----------

Main programs: geogrid.exe, ungrib.exe, metgrid.exe

Run `geogrid.exe` to create the geography data.

`ungrib.exe` decodes the data using tables and creates an intermediate format

`metgrid.exe` ingests the data and interpolates the fields to the model domain

Running WRF
-----------
