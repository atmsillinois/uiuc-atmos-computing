netCDF Kitchen Sink
===================

To reduce file size of stored data or for data transfer, it may be helpful to
reduce the number of variables in a netCDF file, reduce the number of time steps

To extract a variable from a file and save it as a new file:

.. code-block:: console

   ncks -v variable_name in.nc out.nc

As an example from WRF output

.. code-block:: console

   ncks -v XLAT,XLONG,co,so2 wrfout_d01_2022-01-01_00:00:00 wrfout_modified

To extract time, for example the second through second to last time

.. code-block:: console

    ncks -d Time,1,-2 in.nc out.nc

Or the second through last time 

.. code-block:: console

    ncks -d Time,1,-1 in.nc out.nc

This can be applied to other dimensions to select part of the domain:

or even a certain point

If you ever want to rename a variable

.. code-block:: console

    ncrename -h -O -v old_variable_name,new_variable_name filename.nc

