Known Issues
+++++++++++++

This is for known CESM issues that may pop up.

B Compset Missing Ice Data
==========================
The B compset is known to be missing ice input data, so it needs to be downloaded separately.

Go into your input data directory, and do the following:

.. code-block:: console

   mailes2@keeling CESM_INPUT_DATA: $ mkdir ice
   mailes2@keeling CESM_INPUT_DATA: $ cd ice
   mailes2@keeling ice: $ mkdir cice
   mailes2@keeling ice: $ cd cice
   mailes2@keeling cice: $ svn export https://svn-ccsm-inputdata.cgd.ucar.edu/trunk/inputdata/ice/cice/iced.0001-01-01.gx3v7_20080212
