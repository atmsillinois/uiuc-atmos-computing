Port Validation
++++++++++++++++
These are tests meant to check the validity of the Keeling port, and should be done after implementing out-of-the-box capability.

Functionality tests
===================
Try running these following tests:

* ERS_D.T31_g37_rx1.A
* ERI.ne30_g16.X
* ERI.T31_g37_rx1.A
* ERS.T62_g16.C

You can create the tests as per the following (put the name of any test in place of ERS_D.T31_g37_rx1.A):

``./create_test -mach keeling -testname ERS_D.T31_g37_rx1.A -testid t01 -compiler intel``

This creates a directory called ERS_D.T31_g37_rx1.A.

``cd ERS_D.T31_g37_rx1.A.keeling.intel.t01``

``./ERS_D.T31_g37_rx1.A.keeling_intel.t01.test_build``

``./ERS_D.T31_g37_rx1.A.keeling_intel.t01.submit``

When the job is done running, check TestStatus to see if the test ran correctly. The first word should be PASS.

Reproducibility 
===============
Try running a compset for a year twice. We suggest doing the control for your own experiment twice, like how we do below:

This is the CAM 5 Aquaplanet with a resolution of 1.9x1.9.
``./create_newcase -case cam5aquaFixSST -compset FC5AQUAP -res f19_f19 -mach keeling``

To set it to run for a year, set the following in ``env_run.xml``: ``<entry id="STOP_N"   value="365"  />``

Run the first test normally for a year.

For the second test, try setting it so it restarts every so often to check for reproducibility between restarts. 

This can be done so in ``env_run.xml``:

.. code-block:: xml

   <entry id="STOP_N"   value="365"  />
   <entry id="REST_N"   value="2"  />
   <entry id="REST_OPTION"   value="nmonths"  />
   <entry id="DOUT_S_ROOT"   value="$HOME/a/CESM_DATA/$CASE/outputdata_restart"  />

To check if the two tests are bit-for-bit the same, use the following Python code.

.. code-block:: python
  
   import xarray as xr
   data = xr.open_dataset("cam5aquaFixSST.cam.h0.0001-12.nc") #End of year for first run
   data2 = xr.open_dataset("cam5aquaFixSST_restart.cam.h0.0001-12.nc") #End of year for second run
   for variable in data.data_vars:
       print(variable, data[variable].equals(data2[variable]))

Check everything is the same between the two except for the date and time written!
