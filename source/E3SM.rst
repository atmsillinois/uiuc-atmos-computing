Getting Started with E3SM
+++++++++++++++++++++++++
This page provides a guide to setting up E3SM on Keeling.

Downloading the model
=====================

Create a E3SM2_1 directory in your home directory 
-------------------------------------------------
``mkdir E3SM2_1/`` 

Next, create a E3SM folder in your /a/ directory. 
This will store the model outputs and data about the cases that are being run.

``cd a``

``mkdir E3SM`` 

Create a SSH key to your GitHub account to download E3SM
---------------------------------------------------------
Click `here`_ for a guide through the setup process.

.. _here: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?platform=linux


Downloading the model
---------------------
First, make sure to navigate to your E3SM folder from youd home directory.
``cd E3SM2_1/``

Use the following command to begin installing the required model files into this directory. 
``git clone -b maint-2.1 --recursive https://github.com/E3SM-Project/E3SM.git``

It is not unusual for the model to take a long time to download at first, since the model consists of several very large files.


Checking for a successful download
----------------------------------
Navigate to your E3SM directory:
``cd E3SM``
and list the files using ``ls``.

You should see the following files:

* AUTHORS	
* CONTRIBUTING.md	
* README.md	
* cime_config	
* components	
* driver-moab	
* run_e3sm.template.sh 
* CITATION.cff 
* LICENSE	
* cime	
* codemeta.json	
* driver-mct
* externals	
* share

Modules
-------
Navigate to your home directory, then edit the file ``.modules7``.

Add the following lines:

.. code-block:: console

 module load gnu/gnu-9.3.0
 module load gnu/netcdf4-4.7.4-gnu-9.3.0
 module load gnu/openmpi-4.1.2-gnu-9.3.0
  
After saving the file, you will either need to relogin to Keeling, or use the commands
``module purge``
``source .modules7``
to load the modules correctly.

Note that these modules can fail CESM builds - you will have to come back and delete those lines if you need to run a CESM model.


Edit XML Scripts for Keeling
============================

Machine Configuration
---------------------
Navigate to the cime_config/machines folder. 
``cd /data/keeling/a/<NetId>/E3SM2_1/E3SM/cime_config/machines/``

Add the following to the file ``config_machines.xml`` in ``/data/keeling/a/<NetID>/E3SM2_1/E3SM/cime_config/machines``
Don’t forget to change the netID from <NetID>. You can place the code block at the end of the code. Make sure you paste this before <default_run_suffix>!!:

.. code-block:: console

  <machine MACH="keeling">
    <DESC>UIUC CentOS 7.9, os is Linux, 16 pes/node, batch system is SLURM</DESC>
    <OS>LINUX</OS>
    <COMPILERS>gnu</COMPILERS>
    <MPILIBS>openmpi</MPILIBS>
    <CIME_OUTPUT_ROOT>/data/keeling/a/netID/a/E3SM/$CASE/run</CIME_OUTPUT_ROOT>
    <DIN_LOC_ROOT>/data/keeling/a/netID/a/E3SM/E3SM_input_data</DIN_LOC_ROOT>
    <DIN_LOC_ROOT_CLMFORC>$DIN_LOC_ROOT/atm/datm7</DIN_LOC_ROOT_CLMFORC>
    <DOUT_S_ROOT>/data/keeling/a/netID/a/E3SM/$CASE/E3SM_output_data</DOUT_S_ROOT>
    <BASELINE_ROOT>/data/keeling/a/netID/a/E3SM/CCSM_BASELINE</BASELINE_ROOT>
    <CCSM_CPRNC>/data/keeling/a/netID/E3SM2_1/E3SM/cime_config/tools/cprnc/cprnc</CCSM_CPRNC>
    <GMAKE_J>8</GMAKE_J>
    <BATCH_SYSTEM>slurm</BATCH_SYSTEM>
    <SUPPORTED_BY>e3sm</SUPPORTED_BY>
    <MAX_TASKS_PER_NODE>48</MAX_TASKS_PER_NODE>
    <MAX_MPITASKS_PER_NODE>48</MAX_MPITASKS_PER_NODE>
    <PROJECT_REQUIRED>FALSE</PROJECT_REQUIRED>
    <mpirun mpilib="openmpi">
      <executable>mpirun</executable>
        <arguments>
        <arg name="num_tasks">-n {{ total_tasks }}  </arg>
        
      </arguments>

    </mpirun>
    <module_system type="module">
    </module_system>

  </machine>


Checking the validity of ``config_machines.xml``
------------------------------------------------
Run the following command from any directory, making sure to replace <NetID> with your netID:

``xmllint --xinclude --noout --schema /data/keeling/a/<NetId>/E3SM2_1/E3SM/cime/CIME/data/config/xml_schemas/config_machines.xsd /data/keeling/a/<NetId>/E3SM2_1/E3SM/cime_config/machines/config_machines.xml``

If it successfully ports, you should see the following message:

.. code-block:: console

	/data/keeling/a/<NetId>/E3SM2_1/E3SM/cime_config/machines/config_machines.xml validates


Batch Configuration
-------------------
From the same directory, edit the file ``config_batch.xml``

Add the following lines:

.. code-block:: xml

 <batch_system type="slurm">
    <batch_query per_job_arg="-j">squeue</batch_query>
    <batch_submit>sbatch</batch_submit>
    <batch_cancel>scancel</batch_cancel>
    <batch_directive>#SBATCH</batch_directive>
    <jobid_pattern>(\d+)$</jobid_pattern>
    <depend_string>--dependency=afterok:jobid</depend_string>
    <depend_allow_string>--dependency=afterany:jobid</depend_allow_string>
    <depend_separator>:</depend_separator>
    <walltime_format>%H:%M:%S</walltime_format>
    <batch_mail_flag>--mail-user netID@illinois.edu</batch_mail_flag>
    <batch_mail_type_flag>--mail-type</batch_mail_type_flag>
    <batch_mail_type>none, all, begin, end, fail</batch_mail_type>
    <submit_args>
      <arg flag="--time" name="$JOB_WALLCLOCK_TIME"/>
      <arg flag="-q" name="$JOB_QUEUE"/>
    </submit_args>
    <directives>
      <directive> --job-name={{ job_id }}</directive>
      <directive> --partition=sesempi</directive>
      <directive> --nodes={{ num_nodes }}</directive>
      <directive> --ntasks-per-node={{ tasks_per_node }}</directive>
      <directive> --constraint=j48</directive>
      <directive> --output={{ job_id }}.%j </directive>
      <directive> --exclusive </directive>
      <directive> --mail-type=FAIL </directive>
      <directive> --mail-type=END </directive>
      <directive> --mail-user=NetId@illinois.edu </directive>

    </directives>
     <queues>
      <queue walltimemax="168:00:00">sesempi</queue>
     </queues>
  </batch_system>

**Important: You will find another batch system  <batch_system type="slurm" > in the middle of the config_batch.xml file. So you need to remove this block:**

.. code-block:: xml

  <batch_system type="slurm" >
    <batch_query per_job_arg="-j">squeue</batch_query>
    <batch_submit>sbatch</batch_submit>
    <batch_cancel>scancel</batch_cancel>
    <batch_directive>#SBATCH</batch_directive>
    <jobid_pattern>(\d+)$</jobid_pattern>
    <depend_string>--dependency=afterok:jobid</depend_string>
    <depend_allow_string>--dependency=afterany:jobid</depend_allow_string>
    <depend_separator>:</depend_separator>
    <walltime_format>%H:%M:%S</walltime_format>
    <batch_mail_flag>--mail-user</batch_mail_flag>
    <batch_mail_type_flag>--mail-type</batch_mail_type_flag>
    <batch_mail_type>none, all, begin, end, fail</batch_mail_type>
    <submit_args>
      <arg flag="--time" name="$JOB_WALLCLOCK_TIME"/>
      <arg flag="-p" name="$JOB_QUEUE"/>
      <arg flag="--account" name="$PROJECT"/>
    </submit_args>
    <directives>
      <directive> --job-name={{ job_id }}</directive>
      <directive> --nodes={{ num_nodes }}</directive>
      <directive> --output={{ job_id }}.%j </directive>
      <directive> --exclusive </directive>
    </directives>
  </batch_system>

Checking the validity of ``config_batch.xml``
----------------------------------------------
Run the following command, making sure to replace <NetID> with your netID:
``xmllint --xinclude --noout --schema /data/keeling/a/<NetId>/E3SM2_1/E3SM/cime/CIME/data/config/xml_schemas/config_batch.xsd /data/keeling/a/<NetId>/E3SM2_1/E3SM/cime_config/machines/config_batch.xml``


If it successfully ports, you should see the following message:

.. code-block:: console

	/data/keeling/a/<NetId>/E3SM2_1/E3SM/cime_config/machines/config_machines.xml validates


Editing the userdefined files
-----------------------------

Go to the ``cmake_macros`` folder:
``cd /data/keeling/a/<NetId>/E3SM2_1/E3SM/cime_config/machines/cmake_macros/``

Here, we will make a copy of userdefined.cmake in case of mistakes or for comparison.
``cp userdefined.cmake userdefined_copy.cmake``

First, rename ``userdefined.cmake`` to ``keeling.cmake``, as shown below:

``cp userdefined.cmake keeling.cmake``

Now, edit the ``keeling.cmake`` file. Change the contents to the coded provided below.

.. code-block:: console

  set(SUPPORTS_CXX "TRUE")
  if (COMP_NAME STREQUAL gptl)
    string(APPEND CPPDEFS " -DHAVE_VPRINTF -DHAVE_GETTIMEOFDAY -DHAVE_BACKTRACE")
  endif()
  set(NETCDF_PATH "/sw/netcdf4-4.7.4-gnu-9.3.0")
  if (NOT DEBUG)
    string(APPEND FFLAGS " -fno-unsafe-math-optimizations")
  endif()
  if (DEBUG)
    string(APPEND FFLAGS " -g -fbacktrace -fbounds-check -ffpe-trap=invalid,zero,overflow")
  endif()
  string(APPEND SLIBS " -L${NETCDF_PATH}/lib/ -lnetcdff -lnetcdf -lcurl -llapack -lblas")
  if (MPILIB STREQUAL mpi-serial)
    set(SCC "gcc")
  endif()
  if (MPILIB STREQUAL mpi-serial)
    set(SFC "gfortran")
  endif()
  string(APPEND CXX_LIBS " -lstdc++")


Building a Case
===============
Navigate to the directory
``cd /data/keeling/a/<NetId>/E3SM2_1/E3SM/cime/scripts/``

Create a new case with any name for the case, for example, "case1" with the following command.

``./create_newcase --case <case name> --compset A --res f45_g37 --mach keeling``

Setup and build your case
--------------------------

``cd <caseName>``

``./case.setup``

``./case.build``

If your case was built successfully, you will see the following message:

.. code-block:: console

	MODEL BUILD HAS FINISHED SUCCESSFULLY
   
Preview case run
----------------

Run ``./preview_run`` to see the case info, check the number of total tasks and how many nodes are required to run the case.
Make sure to change the nodes in your ``env_batch.xml`` file according to the case info displayed here, and change –ntasks  if required. 
**Never occupy more nodes than your case info says it needs, Keeling will still occupy those extra nodes for no reason.**

Note that
--ntasks=nodes*48
For example, if you require 3 nodes then --ntasks=144.


Running a test case 
-------------------
To run a standard test case, edit your ``env_batch.xml`` file using the xmlchange command:

From your case directory,
``./xmlchange NTHRDS=2``

We are only using 2 threads for this test case. For a real case, you will have to change this number to match your requirements.

To run the case after making this change, run the following commands to reset, clean, and rebuild your run.

``./case.setup --reset``

``./case.build --clean-all``

``./case.build``

Submit the case run 
-------------------
To run your test case, run the command:
``./case.submit``
If your batch configuration was correct, you should get updates by Email from slurm when your run begins and ends.
You can also type ``sqq`` to look at everything that is currently being run.


Output files from E3SM
======================

To find your output files, navigate to ``/data/keeling/a/<netID>/a/E3SM/<case_name>/run/<case_name>/run``

These files do not provide the same conventional gridded output as CESM - and instead provide data as vectors - so they cannot be analyzed in the same way. They need to be converted first.
