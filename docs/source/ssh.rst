Access Keeling via ssh
======================

Keeling is the ATMS computing cluster that can be used for computing work.
When connecting off campus, the VPN must be used. Information on the CISCO
AnyConnect Virtual Private Network (VPN) is available
`here <https://techservices.illinois.edu/vpn-essentials/>`__.

The hostname for Keeling is:

.. code-block:: console

    keeling.earth.illinois.edu

Connecting via ssh on MacOS of Linux with terminal
--------------------------------------------------

Using the terminal, you can connect to Keeling by:

.. code-block:: console

    ssh -Y netID@keeling.earth.illinois.edu

Connecting via ssh on Windows
-----------------------------

PuTTY with X11 forwarding
^^^^^^^^^^^^^^^^^^^^^^^^^

#. Download Xming `here <https://sourceforge.net/projects/xming/>`__.

#. Install Xming after the download is complete.

#. Select install a PuTTy client or download Putty `here <https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html>`__.

#. Start XLaunch.

#. Select multiple windows option and continue to end

#. After Xlaunch is open, launch PuTTY.

#. In the hostname box enter ``netID@keeling.earth.illinois.edu``

#. On the left sidebar, click on ``SSH`` and then click on ``X11``

#. Click the checkbox ``Enable x11 forwarding``.

     .. hint::
       You should save the settings so you don't have always do these steps. This can be done by
       entering a name in the ``Saved Sessions`` box (such as keeling) and clicking ``Save``.
       In the future, you can just select the profile (double click or select and click Load).
#. Click ``Connect``

#. Enter your password to access Keeling.

Connecting via Visual Code Studio
---------------------------------

Visual Studio Code, also commonly referred to as VS Code, is a source-code editor
made by Microsoft and can be used on your local machine to connect to Keeling and
edit code.

#. Acquire the code from https://code.visualstudio.com/.

#. Install Visual Studio Code.

#. Open Visual Studio Code.

#. You will need to get the ssh extension Remote-SSH. This can be aquired by
   clicking the gear in the bottom left corner (Settings) and finding and installing
   Remote-SSH.

#. After installing the extension, in VS Code, select Remote-SSH: Connect to Host
   for the Command Palette (F1, ⇧⌘P). Enter the following::

     ssh <your netID>@keeling.earth.illinois.edu

#. Enter your password to connect to Keeling.

Graphical display
-----------------

In order to use a graphical display on Keeling, you must enable X Windows forwarding.
On MacOS, this requires XQuartz - an open-source project to develop X Window System.
Information regarding XQuartz can be found `here <https://www.xquartz.org/>`_.
On Windows, you must install an X11 forwarding software such as `Xming <https://sourceforge.net/projects/xming/>`__.

To enable X11 forwarding using Visual Studio Code:

#. Click Configure SSH Hosts

#. Under ``Host keeling.earth.illinois.edu``, add the following:

.. code-block:: console

 ForwardX11 yes
 ForwardX11Trusted yes

.. todo::
  With the addition to the Remote X11 (SSH) extension, does this work for Windows?
