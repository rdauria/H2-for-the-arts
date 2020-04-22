.. _Unix-command-line-101:

#####################
Unix command line 101
#####################

The Hoffman2 Cluster is a Linux-based computing platform. One of the
most powerful ways to interact with the Hoffman2 Cluster is via the
Unix-command line. You can access the Hoffman2 command line by logging
into the system either via a terminal application (see:
:ref:`Connecting-via-SSH`) or by opening a remote desktop on Hoffman2
(see: :ref:`Connecting-via-NX-clients`) and opening a terminal
window. Any of these methods will open a command-line interpreter or
unix shell (or most simply shell). This page presents a review of
common tasks you may need to execute or script in your shell.

.. _Navigation:

Navigation
----------

.. _Listing-files:

Listing files
^^^^^^^^^^^^^

To list what files are present in the current directory type at the Hoffman2 command prompt: ::

 ls


to list hidden files or folders: ::

 ls -a

to get extra information on the files or folders in the current working directory use: ::

 ls -la

``-l`` adds a long listing format with a total of nine fields, a typical line of output from ``ls -la`` could look, for example, as the following: ::

 -rwxr-xr--   1 joebruin gobruins 3974052 Apr 17 12:53 myexecutable

 
where: the first character of the first field indicates the file type
(``-``, for a regular file, ``d`` for a directory, ``l``, for a link,
etc.); the remiaining 9 characters of the first field indicate the
file permissions (for example: ``rwxr-xr--`` means that the file can
be read, written and executed by its owner, as denoted by the first
three fields ``rwx``; read and exectuted by the owner's group:
``r-x`` - if the path to the current directory is also accessible by
the group; and read by any other user on the system: ``r--`` - again
assuming that the path to the current directory is also accessible by
any user on the system)

.. Note:: By default on the Hoffman2 Cluster ``$HOME`` directories are accessible only by the owner and therefore no file or folder within them, despite their permissions, can be accessed by any other user.

the second field indicates the number of hard links to the file; the
third field indicates the file owner (for example ``joebruin``); the
fourth file indicates the file group (``gobruins`` in the example
above); the first field indicates the file size - in blocks; the
sixth, seventh and eight fields indicate the data and time of last
modification; and ninth and last field shows the file name
(``myexecutable`` in the example above).

to display the size of the files in units of bytes, MB or GB: ::

 ls -lah

for example: ::

 -rwxr-xr--   1 joebruin gobruins 3.8M Apr 17 12:53 myexecutable

to order the list of files by time (newest on top): ::

 ls -laht

to order the list of files by time oldest on top): ::

 ls -lahtr

.. _Files-and--directories-management:

Files and directories management
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Copying-moving-removing-files:

Copying, moving,removing files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To copy a file from a location to another: ::

 cp myfile.txt myfile1.txt

to move a file from a location to another: ::

 mv myfile.txt myfile1.txt 

to remove a file: ::

 rm myfile.txt

.. _Changing-directory:

Changing directory
""""""""""""""""""
Upon logging onto the Hoffman2 cluster you land in your ``$HOME``
directory which is a space with up to 20 GB of capacity fully
backed-up (see: :ref:`Your-home-directory`). To navigate to your
temporary storage directory (see: :ref:`File-system-for-temporary-use`) issue: ::

 cd $SCRATCH


to check in which directory you currently are issue: ::

 pwd

user ``joebruin`` would get: ::


 /u/scratch/j/joebruin

to move up two directories: ::

 cd ../..

to move back to the ```$HOME`` directory: ::

 cd ~

.. _Creating-removing-directories:

Creating/removing directories
"""""""""""""""""""""""""""""

To create the directory ``test`` in your ``$HOME`` directory issue: ::
 
 mkdir $HOME/test

to navigate to this newly created directory issue: ::

 cd $HOME/test

you check the location of this new directory, issue: ::

 pwd

user ``joebruin`` would get: ::


 /u/home/j/joebruin/test

To remove an empty directory, issue: ::

 rmdir test

to force removal of a file or a directory: ::

 rm -rf test

.. _Environmental-variables:

Environmental variables
-----------------------

On a unix-like system any string that is preceded by a ``$`` is a
variable, which means that the operating system will interpret the
string according to what it has been set to. Environmental variables
are a set of variables that influences the behavior of the
command-line or shell interpreter in use (typically `Bash
<https://www.gnu.org/software/bash/manual/html_node/What-is-Bash_003f.html>`_). Example
of environmental variables that are set for you are: ::

 $HOME
 $SCRATCH
 $PATH
 $LD_LIBRARY_PATH

to check the content of a variable, for example $HOME, issue: ::

 echo $HOME

the content of your ``$PATH`` and ``$LD_LIBRARAY_PATH`` influences the kind of executables and libraries that you can access at the command line (without specifying their full path).


.. _Working-with-files:

Working with files
------------------

.. _Displaying-files:

Displaying files
""""""""""""""""

Unix-like systems have more than one way to display the content of a
file, or selecgted snippet of it, on your unix shell. Among some of
the most usefule commands are: cut, less, more, head and tail. To see
how they work, issue at Hoffman2 command promt, for example::

 cut $HOME/.bashrc
 less $HOME/.bashrc
 more $HOME/.bashrc

to exit from displaying a file with ``more`` or ``less`` issue: ::

 q

To see just the first few lines of a file: ::

 head $HOME/.bashrc

or, for the first two lines: ::

 head -n 2 $HOME/.bashrc

to see the last few lines of a file: ::

 tail $HOME/.bashrc

or, for the last two lines: ::

 tail -n 2 $HOME/.bashrc

to look at the last few lines of a file that is being written on the fly:

 tail -f /u/local/licenses/LOGS/logit.matlab

use: 

 Control-C (to exit)

.. _Editing-files:

Editing files
"""""""""""""

Unix-like system have very powerful editors packed with shortcuts that
largely simplify the editing task. The learning curve on some of these
editors, such as ``vi``, can be pretty steep. However, there are
several simpler, if less powerful, alternatives. 

.. _Editors-that-do-not-require-X11-Forwarding:

Editors that do not require X11 Forwarding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here are some of the file editors available on the Hoffman2 Cluster
presented in order of the complexity of their use, from less to more complex: ::

 nano
 emacs
 vi

.. _Editors-that--require-X11-Forwarding:

Editors that require X11 Forwarding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here are some of the file editors available on the Hoffman2 Cluster
when X11 Forwarding is enabled (see: :ref:`Opening-GUI-applications`)
presented in order of the complexity of their use, from less to more
complex: ::

 nano
 gedit &
 emacs &
 vi
 gvim &

.. _Miscellaneus-commands:

Miscellaneous commands
----------------------

Most unix commands have manual pages that can be accessed via the ``man`` command, for example to learn more about the ``ls`` command, issue: ::

 man ls

or: ::

 man vi
