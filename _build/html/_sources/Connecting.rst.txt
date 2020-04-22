.. _Connecting:

########################
   Connecting/Logging in
########################


.. toctree::
   :maxdepth: 2

.. _Connecting-via-a-terminal:

Connecting via a terminal
=========================

In order to be able to connect to the Hoffman2 Cluster and open
graphical user interfaces such as Maya you need to have an SSH-client
and an X Window System installed on your computer. Step by step
instructions on how to acquire and use these tools are given below for
Linux, Mac and Windows operating systems.


All the connections to the cluster are based on the SSH secure
protocol that requires authentication. Depending on the
operating system that your computer is running, you have several
choices for which command line application to use when connecting with
the cluster.


.. _Connecting-from-Linux:

Connectiong from Linux
----------------------

An X Window System server is generally available and running with the
default installation (or can be readily installed via the OS package
manager). If you are connecting from a Linux box, you can use the
standard SSH-client generally installed with the OS and available via
any terminal application. Open a terminal window and type the
following: ::

 ssh -X login_id@hoffman2.idre.ucla.edu

where ``login_id`` is replaced by your cluster user name.

.. _Connecting-from-MacOS:

Connecting from a MacOS
-----------------------

On Mac OS X, the X windows system is called XQuartz. Mac OS X 10.5
10.6 and 10.7 installed it by default, but as of 10.8 Apple has
dropped dedicated support and directs users to the open source XQuartz
page. You can install XQuartz from the OS distribution media or
download it from `XQuartz page <https://www.xquartz.org/>`_.

MacOS, the operating system running on a Mac computer, comes equipped
with a fully functional `Termainal application
<https://support.apple.com/guide/terminal/welcome/mac>`_ and an
SSH-client. The Terminal application can be located using the
`Spotlight Search
<https://support.apple.com/guide/mac-help/spotlight-mchlp1008/mac>`_
or searching the Applications folder in `Finder
<https://support.apple.com/guide/mac-help/finder-mchlp2605/mac>`_.

.. note:: To enable indirect GLX and to allow remote visualization on the Hoffman2 Cluster, open the MacOS Termianl application and issue at its command prompt: ::

 defaults write org.macosforge.xquartz.X11 enable_iglx -bool true

.. Warning:: You will need to reboot your machine before being able to open GUI applications on Hoffman2.

.. note:: You will need to perform the operation above only once


To connect to the Hoffman2 Cluster, open the MacOS Terminal
application and issue at its command prompt: ::

 ssh -Y login_id@hoffman2.idre.ucla.edu

where ``login_id`` is replaced by your cluster user name. 

.. _Connecting-from-Windows:

Connecting from Windows
-----------------------


To connect to the Hoffman2 Cluster from your Windows computer Install **one** of any of the following free ssh client programs. 

.. Note: :: The following list, which is not exhaustive and you may opt for a different solution, is ordered from our most to least recommended applications.

.. Note:: Only one of the following options is needed to connect to the cluster via SSH.

* `MobaXterm <http://mobaxterm.mobatek.net/>`_ a free X server for Windows with SSH terminal, SFTP and more. 
* `XMing <http://sourceforge.net/projects/xming/>`_ and `Xming fonts <http://sourceforge.net/projects/xming/files/Xming-fonts/>`_ provides a X Window System Server for Microsoft to be used in conjunction with `GitBash <https://gitforwindows.org/>`_ an emulation shell for Windows.
* `Cygwin <http://www.cygwin.com/>`_ a free Linux-like environment for Windows. To add Cygwin/X server, select the ``xinit`` package from the X11 category. 
* `Xshell <https://www.netsarang.com/en/xshell/>`_ a commercial option.


Instructions are given for the first two of the options described above.

.. _Instructions-for-MobaXterm:

Instructions for MobaXterm
^^^^^^^^^^^^^^^^^^^^^^^^^^

Download and install the `MobaXterm Home Edition <https://mobaxterm.mobatek.net/download-home-edition.html>`_, either the Portable or Installer edition will work. To log into the Hoffman2 Cluster open MobaXterm and click on the Session button (the Sessions button is circled in green in :ref:`MobaXterm-w-green-sessions`). 


.. _MobaXterm-w-green-sessions:
.. figure:: _static/moba.png
  :width: 400
  :alt: Image showing the MobaXterm application open with the Sessions button in the upper left corner circled in green 

  MobaXterm application with the Sessions button circled in green.

A new window will pop-up to allow you to choose a type of session. Choose SSH by clicking on the SSH button (upper left corner) and fill the ``Remote host`` field with the Hoffman2 Cluster address, that is: ::

 hoffman2.idre.ucla.edu

save the session by pressing the OK button.

.. _MobaXterm-w-SSH:
.. figure:: _static/moba-ssh.png
  :width: 400
  :alt: Image showing the MobaXterm application open with the SSH Sessions in which the Remote host field has been filled with the Hoffman2 Cluster address (hoffman2.idre.ucla.edu). The OK button in the center bottom is circled in green 

  MobaXterm application with the SSH session set for Hoffman2.

You can also create a new session type which you will use to transfer
files back and from the cluster to your local machine, by selecting
the Sessions button again and choosing SFTP as the session type and fill the ``Remote host`` field with the Hoffman2 Cluster data transfer node address, that is: ::

 dtn1.hoffman2.idre.ucla.edu

save the session by pressing the OK button.

To access the cluster double click on the save SSH session.


.. _Instructions-for-XMing-and-GitBash:

Instructions for XMing and GitBash
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Download and install `XMing <http://sourceforge.net/projects/xming/>`_ and `Xming fonts <http://sourceforge.net/projects/xming/files/Xming-fonts/>`_ following the installer instructions. Download and install `GitBash <https://gitforwindows.org/>`_ following the installer instructions. 

Start Xming from the programs, open GibBAsh from the Start Menu, at its command promopt issue: ::

 export DISPLAY-localhost:0

Connect to the Hoffman2 Cluster by typing: ::

 ssh -XY login_id@hoffman2.idre.ucla.edu

where ``login_id`` is replaced by your cluster user name.


.. _Opening-GUI-applications:


.. _Connecting-via-NX-clients:

Connecting via NX clients
=========================

.. include:: NX-clients.rst






