Keeling file systems
====================

Home directory
--------------

When you log in to Keeling, you begin in your home directory.
Your personal space on this file system is relatively small with a current quota
of 100 GB. By keeping the per-user quota low, this file system is able to be
backed up nightly to ensure its safety in the case of disk failure.

You may check your file system usage against your quota with the command:

.. code-block:: console

    quota -s

The recommendation is that you use your home directory for small and very critical
files such as:

Source code (models and analysis)
Model inputs and scripts

Group file systems
------------------

Most Keeling users have access to storage purchased by research groups or projects.
These file systems are available on Keeling through directories:

.. code-block:: console

    /data/<group PI>/filesystem

These file systems should also be available as symbolic links in your `$HOME` directory.

To get a listing of file systems available to you and their current amount of disk space available:

.. code-block:: console

    df /data/<group PI>/*

Note that these file systems are shared resource by members of your group and that usage should b
carefully monitored. Often multiple people may be competing for disk space with either file
transfers/downloads or running models that are writing data to disk, all of which will
fail when disk space is exhausted (by yourself or someone else).
