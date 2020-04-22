NX is a free, secure, compressed protocol for remote X Window System connections for Windows, Linux, Mac OSX, and Solaris. We currently support connecting to the Hoffman2 cluster via the NoMachine client as well as the X2Go client.

NoMachine client
----------------

Download NoMachine Client for your operating system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A free NoMachine client is available from NoMachine for Windows, Mac OSX, and most Linux distributions.

* Download NoMachine from the `website <https://www.nomachine.com/download>`_. 
* Use the download link for your operating system (Windows, Mac OSX, or Linux Distributions).

Install
^^^^^^^

* Once downloaded, double click on the downloaded package file.
* Click the ``Run/Install`` button.
* When the NoMachine Client Setup Wizard starts, accept all the setup defaults.

Configure
^^^^^^^^^

* Open the NoMachine client and continue to the connection page.
* Click on the ``New`` link to create a connection to the Hoffman2 Cluster.
* Specify the protocol as ``SSH`` and press ``Continue``.
* Provide the host name (*nx.hoffman2.idre.ucla.edu*).
* Leave the port field set to 22 and press the ``Continue`` button.
* Select ``Use the system login`` and press the ``Continue`` button.
* Select ``Password`` and press the ``Continue`` button.
* Select ``Don’t use a proxy`` and press the ``Continue`` button.
* Name your connection and press the ``Done`` button.

Please notice that if you are planning to suspend and reconnect to an NX session, you will need to repeat the configuration steps described above for the hosts: ::

 nx1.hoffman2.idre.ucla.edu
 nx2.hoffman2.idre.ucla.edu

Run
^^^

* Open the NoMachine client and continue to the connection page.
* Click on the saved connection that you previously configured.
* Enter your username and password and your NX session with a virtual desktop on the cluster will start.

How to get a terminal window
""""""""""""""""""""""""""""
After you have logged into the Hoffman2 remote desktop, click on the ``Applications`` menu. Then select ``System Tools`` and ``Terminal``.

.. note::  To pin the terminal to the panel, right click on the ``Terminal`` menu item and select ``Add this launcher to panel``.

How to terminate your NX session
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

From the gnome desktop, select: ::

System -> Log Out login_id … 

A small window will open. Click the ``Log Out`` button.

How to suspend an NX session
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Make note of the actual NX node where your session is running (typically NX1 or NX2) to allow for future reconnections. To terminate the NX client on your computer, close the application or close the virtual desktop window.

How to reconnect to a suspended NX session
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you are planning to reconnect to a session, make note of the actual NX node where your session is running (typically NX1 or NX2). Make sure to configure your NX client for the NX node where your session is running. You will need to create a configuration using either nx1.hoffman2.idre.ucla.edu or nx2.hoffman2.idre.ucla.edu. See the Configuration section for more detailed instructions.

.. note::  Reconnecting to a suspended NX session is not always possible. Lengthy computations should be run as batch jobs. We recommend terminating NX sessions (logout) instead of suspending them.

On fast networks, a preferred alternative to NX is to run an X server on your platform (e.g., X11 packages for linux platforms, XQuartz for mac OSX, XMing and Xming fonts for Windows), and connect to the cluster via a terminal (or via PuTTY on Windows platforms) by setting up X11 forwarding.

X2Go client
-----------

Install X2Go client
^^^^^^^^^^^^^^^^^^^
Follow the installation guide at `X2Go site <https://wiki.x2go.org/doku.php/doc:installation:x2goclient>`_.
Download the X2Go client, select the appropriate client for your operating system (Windows, Mac OS, Linux) and the correct version onto your computer.

Double click on the downloaded package, accept the licensing terms, and follow the instruction on the screen. 

.. note::  MacOS users will need to install the XQuartz dependency prior to installing X2Go.  It may be obtained on the `XQuartz website <https://www.xquartz.org/>`_. You may also encounter a security warning that "x2goclient.app" is from an unidentified developer.  Simply right click on the app again and select Open.

Configure the X2Go client to connect to Hoffman2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Launch the application by double clicking on the X2Go Client desktop icon, if one was created, or by searching for the application in your App folder and double clicking on the application file. If prompted by the Windows Firewall, select ``Allow access`` so that ``sshd.exe`` and other X2Go related programs will be able to run.

Create a new session
^^^^^^^^^^^^^^^^^^^^

From the ``Session`` tab in the X2Go client select ``New session``. You can name your session with any name (e.g., x2go@hoffman2). Leave the ``Path`` field unchanged and in the ``Host`` field enter: ::

 x2go.hoffman2.idre.ucla.edu

Leave the other fields unchanged and choose the ``Session type`` to be either the default ``KDE`` or ``GNOME``. Save the new session by pressing the ``OK`` button.

You can also create sessions for the specific nodes ``x2go1`` and ``x2go2`` if you will need to reconnect to a suspended session. However, we strongly suggest you start new sessions by using the session associated with the ``x2go.hoffman2.idre.ucla.edu`` address. Sessions on the ``x2go1`` and ``x2go2`` nodes can be created by using in the “Host” field respectively: ::

 x2go1.hoffman2.idre.ucla.edu
 x2go2.hoffman2.idre.ucla.edu

Name the sessions accordingly (for example: *x2go1@hoffman2*, *x2go2@hoffman2*).

Start the X2Go client
^^^^^^^^^^^^^^^^^^^^^

To connect to the Hoffman2 cluster open the X2Go client, double click on an already created session, and enter your Hoffman2 username and password. The first time you connect, you will be prompted to trust the host key. Do so by pressing the ``Yes`` button. On Windows, a few more firewalls will pop up and you will have to allow access to the X2Go Client and associated programs.

You should now be connected to a remote desktop on the Hoffman2 cluster.



