Out of the box capability
++++++++++++++++++++++++++

If you would like to be able to run many cases without going into xml files each time to
set up machine related conditions, you can create some files and add some changes to
machine related files for quick and easy case setup each time.

Machine Configuration
======================
``cd CESM/cesm1_2_1/scripts/ccsm_utils/Machines``

Here, specifications, compilers, and other configurations for various machines at places
like NCAR are located. We will need to replicate these conditions for keeling.

Try ``cp /data/keeling/a/mailes2/CESM/cesm1_2_1/scripts/ccsm_utils/Machines/config_machines.xml .`` and edit $HOME to your home directory in the following entries:

* RUNDIR
* EXEROOT
* DIN_LOC_ROOT
* DIN_LOC_ROOT_CLMFORC
* DOUT_S_ROOT
* DOUT_L_MSROOT
* CCSM_BASELINE
* CCSM_CPRNC
* SUPPORTED_BY

Format should look like below.

.. code-block:: xml

   <machine MACH="keeling">
           <DESC>UIUC CentOS, os is Linux, 16 pes/node, batch system is SLURM</DESC>
           <OS>LINUX</OS>
           <COMPILERS>intel</COMPILERS>
           <MPILIBS>openmpi</MPILIBS>
           <RUNDIR>$HOME/a/CESM_DATA/$CASE/run</RUNDIR>
           <EXEROOT>$HOME/a/CESM_DATA/$CASE/bld</EXEROOT>
           <DIN_LOC_ROOT>$HOME/a/CESM_DATA/CESM_INPUT_DATA</DIN_LOC_ROOT>
           <DIN_LOC_ROOT_CLMFORC>$HOME/a/CESM_DATA/CLMFORC</DIN_LOC_ROOT_CLMFORC>
           <DOUT_S_ROOT>$HOME/a/CESM_DATA/$CASE/outputdata</DOUT_S_ROOT>
           <DOUT_L_MSROOT>$HOME/a/CESM_DATA/MSROOT</DOUT_L_MSROOT>
           <CCSM_BASELINE>$HOME/a/CESM_DATA/BASELINE</CCSM_BASELINE>
           <CCSM_CPRNC>$HOME/CESM/cesm1_2_1/tools/cprnc/cprnc</CCSM_CPRNC>
           <BATCHQUERY>squeue</BATCHQUERY>
           <BATCHSUBMIT>sbatch</BATCHSUBMIT>
           <SUPPORTED_BY>(Your NetID here)</SUPPORTED_BY>
           <GMAKE_J>1</GMAKE_J>
           <MAX_TASKS_PER_NODE>48</MAX_TASKS_PER_NODE>
           <PES_PER_NODE>48</PES_PER_NODE>
   </machine>

In place of each $HOME variable, put your home directory. Any path with $CASE in it will
automatically create the desired directories for each case when called. You may need to
create the Baseline and long term archiving (DOUT_L_MSROOT) directories yourself
though.

Machine compilers
=================
Try ``cp /data/keeling/a/mailes2/CESM/cesm1_2_1/scripts/ccsm_utils/Machines/config_compilers.xml .``

Make sure the keeling entry (below the intel entry) looks like below.

.. code-block:: xml

   <compiler MACH="keeling" COMPILER="intel">
    <NETCDF_PATH> /sw/netcdf4-4.7.4-intel-15.0.3</NETCDF_PATH>
    <ADD_SLIBS>$(shell /sw/netcdf4-4.7.4-intel-15.0.3/bin/nc-config --flibs)</ADD_SLIBS>
   </compiler>

Environment machine specific
=============================

Since we didn't change env_mach_specific for the individual case, we won't change it here.

Copy the userdefined file:

``cp env_mach_specific.userdefined env_mach_specific.keeling``

Make batch
============
Try ``cp /data/keeling/a/mailes2/CESM/cesm1_2_1/scripts/ccsm_utils/Machines/mkbatch.keeling .``

Check the time limit is set to one day:

``set tlimit = "1-00:00:00"``

Under the first USERDEFINED section, it should look like below.

.. code-block:: xml

   #SBATCH --job-name=${jobname}
   #SBATCH --partition=sesempi
   #SBATCH --nodes=${nodes}
   #SBATCH --ntasks=${ntasks}
   #SBATCH --time=${tlimit}
   #SBATCH --mem-per-cpu=5g
   #SBATCH --constraint=j48
   #       --mail-type=BEGIN
   #SBATCH --mail-type=FAIL
   #SBATCH --mail-type=END
   #SBATCH --mail-user=(your email)
   #

Change ``--mail-user`` to your own email.

The according PBS lines should look like the following:

.. code-block:: xml

   ##PBS -N ${jobname}
   ##PBS -q batch
   ##PBS -l nodes=${nodes}:ppn=${taskpernode}
   ##PBS -l walltime=${tlimit}

And the BSUB lines:

.. code-block:: xml

   ##BSUB -l nodes=${nodes}:ppn=${taskpernode}:walltime=${tlimit}
   ##BSUB -q batch
   ...
   ###BSUB -W ${tlimit}

Under the second USERDEFINED section, the MPI exec and run lines should look like this:

.. code-block:: xml

   #mpiexec -n ${maxtasks} \$EXEROOT/cesm.exe >&! cesm.log.\$LID
   mpirun -np ${maxtasks} \$EXEROOT/cesm.exe >&! cesm.log.\$LID

Make sure env_mach_specific.keeling and mkbatch.keeling are executable!

Running a case
===============
You should now be able to run a case! Try the following:

.. code-block:: console

   ./create_newcase -case test1_keeling -res f45_g37 -compset X -mach keeling
   cd scripts/test1_keeling
   ./cesm_setup
   ./test1_keeling.build
   sbatch test1_keeling.run

If you run into any errors, try to make according changes in Macros and other editable
files, similar to the "Porting keeling" tutorial.
