PartMC
======

PartMC: Particle-resolved Monte Carlo code for atmospheric aerosol simulation

**Source:** <https://github.com/compdyn/partmc>

**Homepage:** <http://lagrange.mechse.illinois.edu/partmc/>


To build PartMC on Keeling, first load the required modules::

    module load gnu/gnu-9.3.0
    module load gnu/netcdf4-4.7.4-gnu-9.3.0
    module load gnu/openmpi-3.1.6-gnu-9.3.0

So Keeling finds the right compilers::

   export FC=gfortran
   export CC=gcc

Get the PartMC code from github::

    git clone https://github.com/compdyn/partmc.git

Make the build directory::

    mkdir build

Change into the build directory::

    cd build

To set the NetCDF path::

    nc-config --prefix

And set ``NETCDF_HOME`` to that value with the following::

    export NETCDF_HOME=/sw/netcdf4-4.7.4-gnu-9.3.0

If you compiled MOSAIC, set the ``MOSAIC_HOME`` as::

    export MOSAIC_HOME=<where ever you installed it>

If you compiled CAMP, set the ``CAMP_HOME`` as::

    export CAMP_HOME=<where ever you installed it>    

ccmake is used to configure the code::

    ccmake3 ..

When in the menu, Press ``[c]`` to configure.

Turn on MOSAIC (or other options) as desired.

When done configuring, Press ``[c]`` to configure again.

Press ``[g]`` to generate.

Then::

    make
    make test
