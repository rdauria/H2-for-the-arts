########
Renderin
########

Computational resources on the Hoffman2 cluster are managed by a scheduler. Any CPU or memory intensive computing 
task should be performed within either a scheduled interactive session or a batch. This section addresses how to
request an interactive session and how to submit a non interactive job for batch execution. 

Upon connecting to the cluster you are on a login node (unless you are connecting to the cluster via `Jupyter Noteboks or Jupyter Lab <../Connecting/index.html#connecting-via-jupyter-nb-lab>`_). Login nodes are meant as a gateway to the computational resources on the cluster and should be used only for light editing, interactive session requests, or batch job submission. (For more information, see `Role of the login nodes <../../Policies/Role-of-the-login-nodes.html>`_).

Getting an interactive-session
==============================

Basic usage
-----------
To request an interactive session, open your terminal/GitBash window and connect to the Cluster (see: `Connecting/Logging in <../Connecting/index.html>`_). You can request an interactive session on one or more compute nodes. 

To get an interactive session with a runtime longer than the default 2 hours, you will need to specify a value for the 
scheduler complex ``h_rt``. To request more memory than the default value of 1GB, you should specify a value for the
scheduler complex ``h_data``. 

For example, to request an interactive session with a runtime of 3 hours and a total of 4GB of mememory, issue at the Hoffman2 command prompt: ::

 qrsh -l h_rt=3:00:00,h_data=4G

If you are planning to run an application that will use more than one CPU core, you should request cores using the 
``-pe <parallel environment> <n>`` directive (where ``<parallel environment>`` is the name of the parallel environment and ``<n>``
is the number of cores that you are plannig to use). 

If your application can run multiple threads, or, more generally, uses multiple cores in a shared memory paradigm, you will 
need to reqeust the number of cores you are planning to use with the *-pe shared <n>* directive. 

For example, to request 4 CPU core, a runtime of 8 hours, and 2GB of memory per core, issue: ::

 qrsh -l h_rt=8:00:00,h_data=2G -pe shared 4

.. note:: Notice that ``h_data`` is memory per core. Since you are requesting 2GB of memory per core and 4 cores, your total memory request is: 2GB x 4 cores = 8GB. 

If your job/program can run across multiple nodes (e.g. MPI), use: :: 

 qrsh -l h_rt=8:00:00,h_data=2G -pe dc\*  4

A word about ``h_data``:

When invoking an interactive session with ``qrsh``, the proper memory size needs to be specified via ``h_data``. If you are unsure of what amount of memory is appropriate for your interactive session, you could add ``-l exclusive`` to your ``qrsh`` command. In that case, the ``h_data`` value is used by the scheduler to select a compute node having a total amount of memory equal or greater than what specified with ``h_data``. In this case, the memory limit for the job is the compute node’s physical memory size. 

For example, the command: ::

 qrsh -l h_rt=8:00:00,h_data=32G,exclusive

will start an interactive session on a compute node equipped with at least 32G of physical memory. 

.. note:: You can only request as much memory as is available on nodes on the cluster. Interactive session requested via ``qrsh`` without specifying an ``h_data`` value are automatically assigned an ``h_data=1G``, which may or may not be too small for your application.

Resource limitation
-------------------

You can specify up to 24 hours in your qrsh request using: ::

 qrsh -l h_rt=24:00:00
 
Hoffman2 cluster’s compute nodes have different memory sizes. When you request more then one core (using: ``-pe shared <n>``), the total memory requested on the node will be the product of the number of cores time the memory per core (``h_data``). In general, the larger the total memory requested, the longer the wait. Please refer to the output of the command: ::
 
 qhost 

to see what total memory is available on the various nodes on the cluster, keeping in mind that not all hosts may be accessible to you. 

When you request multiple cores, or a large amount of total memory, you may or may not get the interactive session immediately, depending on how busy the cluster is and the permission level of your account. To see to which class of nodes (memory, number of cores, etc.) you have access to, you can enter the following at the Hoffman2 command prompt: ::
 
 mygroups 

What is a node?
--------------

A node is a physical server comprised of mutliples cores. Hoffman2 has a number of nodes available to the entire 
UCLA community. Additionally, research groups can purchase dedicated compute nodes. Group owned nodes, allow users 
a runtime up to fourteen days, and jobs from the group are garanteed to start within twenty-four hours from submission 
(and most typically less). For more information on running jobs on owned please see:  
`Job scheduling policy <../../Policies/Job-scheduling.html#queues-with-time-limit-of-14-days-highp>`_. 
If your group is interested in purchasing nodes please visit: `Purchasing nodes <../Purchasing-nodes.html>`_


Running on your group’s nodes
-----------------------------
 
**The following section does not apply to you if your research group has not purchased Hoffman2 compute nodes**

To run on your group nodes, add the ``-l highp`` switch to your ``qrsh`` command. For example, to request an 
interacrive session with a duration of two days (48 hours), 4GB of memory (and one core), issue the command::

 qrsh -l highp,h_rt=48:00:00,h_data=4G

You could also request multiple cores using the ``-pe dc\* <n>`` or ``-pe shared <n>`` as described above. When 
combining with ``-l highp``, the amount of cores or memory size need to be compatible with your group compute nodes. 
Contact user support if you are not sure.

Although you are allowed to specify ``h_rt`` as high as 336 hours (14 days) for an ``qrsh`` session, it is not recommended. For example, if the network connection is interrupted (e.g. your laptop or desktop computer goes into sleep mode), the ``qrsh`` session may be lost, possibly terminating all running programs within it.

qrsh examples
-------------
.. Note:: The parameters associated with the ``-l directive`` are separated by commas without any white space in between.

Request a single processor for 2 hours.
The default memory size depends on the queue in which your session starts. For campus users, the default memory size is 1GB. Use the ``-l directive`` with the ``h_data`` parameter to request more memory. 

To request a single processor for 24 hours from the interactive queues, issue the command: ::

 qrsh -l h_rt=24:00:00,h_data=1G

To request 8 processors for 4 hours (total 8*1G=8GB memory) on a single node from the interactive queues, issue the command: ::

 qrsh -l h_data=1G,h_rt=4:00:00,h_data=1G -pe shared 8

To request 4 processors for 3 hours (total 4*1G=4GB memory) on a single node, issue the command: ::

 qrsh -l h_data=1G,h_rt=3:00:00,h_data=1G -pe shared 4

To request 12 processors, 1GB of memory per processor, for 2 hours, issue the command: ::

 qrsh -l h_data=1G,h_rt=2:00:00 -pe dc\* 12

.. note:: The 12 CPUs are distributed across multiple compute nodes. The backslash ``\`` in ``dc\*`` is significant when you issue this command in an interactive unix shell.

qrsh startup time
-----------------

A ``qrsh`` session is scheduled along with all other jobs managed by the scheduler software. The shorter time (the ``-l h_rt`` option), and the fewer number of processors (the ``-pe`` option), the better chance you have of getting a session. Request just what you need for the best use of computing resources. Be considerate to other users by exiting your ``qrsh`` session when you are done to release the computing resources to other users.

Interpreting error messages
---------------------------
Occasionally, you may encounter one of the following messages:

``error: no suitable queues``

or,

``qrsh: No match.``

 

If you see the no suitable queues message and you are requesting the interactive queues, be sure you have not requested more than 24 hours. This message may mean there is something incompatible with the various parameters you have specified and your ``qrsh`` session can never start. For example, if you have requested ``-l h_rt=25:00:00`` but your ``userid`` is not authorized to run sessions or jobs for more than 24 hours.

If your session could not be scheduled, first try your ``qrsh`` command again in case it was a momentary problem with the scheduler.

If your session still cannot be scheduled, try lowering either the value of ``h_rt``,  the number of processors requested, or both, if possible.

Running MPI with qrsh
---------------------

The following instructions are specific to IntelMPI library. They may not apply to other MPI implementations (see: How to run MPI for more information).

There are 2 main steps to run an MPI program in a ``qrsh`` session. You need to do step #1 only once per qrsh session. You can repeatedly execute step #2 within the same ``qrsh`` session. The executable MPI program is named ``foo`` in the following example.

**Set up the environment**
 

In the ``qrsh`` session at the shell prompt, enter one of the following commands: 

If you are in ``bash`` shell: ::

 source /u/local/bin/set_qrsh_env.sh 
 
If you are in ``csh/tcsh`` shell: ::

 source /u/local/bin/set_qrsh_env.csh

**Launch your MPI program**

Assume your MPI program is named ``foo`` and is located in the current directory. Run the program using all allocated processors with the command: ::

 mpiexec.hydra -n $NSLOTS ./foo

You could replace ``$NSLOTS`` with an integer, which is less than the number of processors you requested on your ``qrsh`` command. For example: ::

 mpiexec.hydra -n 4 ./foo

The command to see ``mpiexec`` options is: ::

 mpiexec.hydra -help

You do not have to create a hostfile and pass it to ``mpiexec.hydra`` with its ``-machinefile`` or ``-hostfile`` option because ``mpiexec.hydra`` automatically retrieves that information from ``UGE``.

Additional tools
-----------------
Additional scripts are available that may help you run other parallel distributed memory software. You can enter these commands at the compute node’s shell prompt. ::

 get_pe_hostfile

Returns the contents of the ``UGE pe_hostfile`` file for the current ``qrsh`` session. If you have used the ``-pe`` directive to request multiple processors on multiple nodes, you will probably need to tell your program the names of those nodes and how many processors have been allocated on each node. This information is unique to your current ``qrsh`` session.

To create an MPI-style hostfile named ``hfile`` in the current directory: ::

 get_pe_hostfile | awk '{print $1" slots="$2}' > hfile

The ``UGE pe_hostfile`` is located: ::

 $SGE_ROOT/$SGE_CELL/spool/node/active_jobs/sge_jobid.1/pe_hostfile

or, ::

 $SGE_ROOT/$SGE_CELL/spool/node/active_jobs/sge_jobid.sge_taskid/pe_hostfile

where ``node`` and ``sge_jobid`` are the ``hostname`` and ``UGE $JOB_ID``, respectively, of the current ``qrsh`` session.

``sge_taskid`` is the task number of an array job ``$SGE_TASK_ID``.

To return the value of ``UGE JOB_ID`` for the current ``qrsh`` session, issue the command: ::

 get_sge_jobid

To return the contents of the ``UGE`` environment file for the current ``qrsh`` session, issue: :: 

 get_sge_env

which is used by the ``set_qrsh_env`` scripts.

``UGE``-specific environment variables are defined in the file: ::

 $SGE_ROOT/$SGE_CELL/spool/node/active_jobs/sge_jobid.1/environment

or, ::

 $SGE_ROOT/$SGE_CELL/spool/node/active_jobs/sge_jobid.sge_taskid/environment
 
where ``node`` and ``sge_jobid`` are the ``hostname`` and ``UGE $JOB_ID``, respectively, of the current ``qrsh`` session. ``sge_taskid`` is the task number of a array job ``$SGE_TASK_ID``.

Submitting batch jobs
=====================


Submit batch jobs from the cluster nodes
----------------------------------------

In order to run a job under the Univa Grid Engine, you need to supply ``UGE`` directives and their arguements to the job scheduler. The easiest way to do this is to create a ``UGE`` command file that consists of a set of ``UGE`` directives along with the commands required to execute the actual job. The command file for submitting a job can either be built using queue scripts provided by IDRE, or by building an ``UGE`` command file yourself.

Queue scripts
-------------
Each IDRE-provided queue script is named for a type of job or application. The queue script builds a UGE command file for that particular type of job or application. A queue script can be run either as a single command to which you provide appropriate options, or as an interactive application, which presents you with a menu of choices and prompts you for the values of options.

For example, if you simply enter a queue script command such as: ::

 job.q

without any command-line arguments, the queue script will enter its interactive mode and present you with a menu of tasks you can perform. One of these tasks is to build the command file, another is to submit a command file that has already been built, and another is to show the status of jobs you have already submitted. See ``queue scripts`` for details, or select ``Info`` from any queue script menu, or enter ``man queue`` at a shell prompt.

You can also enter ``myjobs`` at the shell prompt to show the status of jobs you have submitted and which have not already completed. You can also enter ``groupjobs`` at the shell prompt to show the status of pending jobs everyone in your group has submitted. Enter ``groupjobs -help`` for options.

IDRE-provided queue scripts can be used to run the following types of jobs:

* Serial Jobs
* Serial Array Jobs
* Multi-threaded Jobs
* MPI Parallel Jobs
* Application Jobs

Serial jobs
-----------
A serial job runs on a single thread on a single node. It does not take advantage of multi-processor nodes or the multiple compute nodes available with a cluster.

To build or submit an ``UGE`` command file for a serial job, you can either enter: ::

 job.q [queue-script-options]

or, you can provide the name of your executable on the command line: ::

 job.q [queue-script-options] name_of_executable [executable-arguments]

When you enter ``job.q`` without the name of your executable, it will interactively ask you to enter any needed memory, wall-clock time limit, and other options, and ask you if you want to submit the job. You can quit out of the queue script menu and edit the ``UGE`` command file, which the script built, if you want to change or add other Univa Grid Engine options before you submit your job.

If you did not submit the command file at the end of the menu dialog and decided to edit the file before submitting it, you can submit your command file using either a queue script ``Submit`` menu item, or the ``qsub`` command: ::

 qsub executable.cmd

When you enter ``job.q`` with the name of your executable, it will by default build the command file using defaults for any queue script options that you did not specify, submit it to the job scheduler, and delete the command file that it built.

Serial array jobs
-----------------
Array jobs are serial jobs or multi-threaded jobs that use the same executable but different input variables or input files, as in parametric studies. Users typically run thousands of jobs with one submission.

The ``UGE`` command file for a serial array job will, at the minimum, contain the ``UGE`` keyword statement for a lower index value and an upper index value. By default, the index interval is one. ``UGE`` keeps track of the jobs using the environment variable ``SGE_TASK_ID``, which varies from the lower index value to the upper index value for each job. Your program can use ``SGE_TASK_ID`` to select the input files to read or the options to be used for that particular run.

If your program is multi-threaded, you must edit the ``UGE`` command file built by the ``jobarray.q`` script and add an ``UGE`` keyword statement that specifies the shared parallel environment and the number of processors your job requires. In most cases you should request no more than 8 processors because the maximum number of processors on most nodes is 8. See the *For a multi-threaded OpenMP job* section for more infromation.

To build or submit an ``UGE`` command file for a serial array job, enter: ::

 jobarray.q

For details, see the section *Running an Array of Jobs Using UGE*.

Multi-threaded jobs
-------------------
Multi-threaded jobs are jobs which will run on more than one thread on the same node. Programs using the OpenMP-based threaded library are a typical example of those that can take advantage of multi-core nodes.

If you know your program is multi-threaded, you need to request that ``UGE`` allocate multiple processors. Otherwise your job will contend for resources with other jobs that are running on the same node, and all jobs on that node may be adversely affected. The queue script will prompt you to enter the number of tasks for your job. The queue script default is 4 tasks. You should request at least as many tasks as your program has threads, but usually no more than 8 tasks because the maximum number of processors on most nodes is 8. See the `Scalability Benchmark <../../Using-H2/Computing/Computing.html#how-to-specify-gpu-types>`_ section for information on how to determine the optimal number of tasks.

To build or submit an ``UGE`` command file for a multi-threaded job, enter: ::

 openmp.q

For details, see OpenMP programs and Multi threaded programs.

MPI parallel jobs
-----------------

MPI parallel jobs are those executable programs that are linked with one of the message passing libraries like OpenMPI. These applications explictly send messages from one node to another using either a Gigabit Ethernet (GE) interface or Infiniband (IB) interface. IDRE recommends that everyone use the Infiniband interface because latency for message passing is short with the IB interface compared to the GE interface.

When MPI jobs are submitted to the cluster, one needs to tell the UGE scheduler how many processors are needed to run the jobs. The queue script will prompt you to enter the number of tasks for your job. The queue script default for generic jobs is 4 parallel tasks. Please see the `Scalability Benchmark <../../Using-H2/Computing/Computing.html#how-to-specify-gpu-types>`_ below for information on how to determine the optimal number of tasks.

To build or submit an ``UGE`` command file for a parallel job, enter: ::

 mpi.q

For details, see the *How to Run MPI* section.

Application jobs
----------------

An application job is one which runs software provided by a commercial vendor or is open source. It is usually installed in system directories (e.g., MATLAB).

To build or submit an ``UGE`` command file for an application job, enter: ::

 application.q

where application is replaced with the name of the application. For example, use ``matlab.q`` to run MATLAB batch jobs. For details, see `Software <../../Using-H2/Software/Software.html>`_ and its subsequent links for each package or program on to how to run them.

Batch job output files
-----------------------

When a job has completed, ``UGE`` messages will be available in the ``stdout`` and ``stderr`` files that were were defined in your ``UGE`` command file with the ``-o`` and ``-e`` or ``-j`` keywords. Program output will be available in any files that your program has written.

If your ``UGE`` command file was built using a queue script, ``stdout`` and ``stderr`` from ``UGE`` will be found in one of: ::

 jobname.joblog
 jobname.joblog.$JOB_ID
 jobname.joblog.$JOB_ID.$SGE_TASK_ID (for array jobs)

Output from your program will be found in one of: ::

 jobname.out
 jobname.out.$JOB_ID
 jobname.output.$JOB_ID
 jobname.output.$JOB_ID.$SGE_TASK_ID (for array jobs)

Build a UGE command file for your job and use UGE commands directly
-------------------------------------------------------------------

This section describes building an ``UGE`` command file yourself, instead of letting a queue script build it for you. Or you may modify an ``UGE`` command file that a queue script has built, according to the information presented here.

The ``UGE`` keyword statements in a command file are called active comments because they begin with ``#$`` and comments in a script file normally begin with ``#``.

Any ``qsub`` command line option can be used in the command file as an active comment. The ``qsub`` command line options are listed on the submit man page

Each ``UGE`` keyword statement begins with ``#$`` followed by the ``UGE`` keyword and its value, if any. For example: ::

 #$ -cwd
 #$ -o jobname.joblog
 #$ -j y

where ``jobname`` is the name of your job. Here the first ``UGE`` statement ``#$ -cwd`` specifies that the current working directory is to be used for the job. The second ``UGE`` statement ``#$ -o jobname.joblog`` names the output file in which the ``UGE`` command file will write its standard out messages. The third ``#$ -j y`` specifies that any messages that ``UGE`` may write to standard error are to be merged with those it writes to standard out.

After you have created the ``UGE`` command file, issue the appropriate ``UGE`` commands from a login node to submit and monitor the job. See Commonly-Used UGE Commands

Using job arrays
----------------

For job arrays you need to use an ``UGE`` keyword statement of the form: ::

 #$ -t lower-upper:interval

Please see Running an Array of Jobs Using UGE for more information.

For a parallel MPI job
-----------------------
For a parallel MPI job you need to have a line that specifies a parallel environment: ::

 #$ -pe dc* number_of_slots_requested

The maximum ``number_of_slots_requested`` value that you should use depends on your account’s access level.

For a multi-threaded/OpenMP job
--------------------------------

For a multi-threaded OpenMP job you need to request that all processors be on the same node by using the shared parallel environment. ::

 #$ -pe shared number_of_slots_requested

where the maximum ``number_of_slots_requested`` no larger than the number of CPU/cores of a compute node.

Parallel environments (PE)
--------------------------

+---------------------------------+-------------------------------------------------------------------+
| **For Threaded Programs (e.g. OpenMP)**                                                             |
+---------------------------------+-------------------------------------------------------------------+
| PE                              | Description                                                       |
+---------------------------------+-------------------------------------------------------------------+
| ``shared p``                    | ``p`` processors on a single node                                 |
+---------------------------------+-------------------------------------------------------------------+
| **For MPI Programs**                                                                                |
+---------------------------------+-------------------------------------------------------------------+
| PE                              | Description                                                       |
+---------------------------------+-------------------------------------------------------------------+
|``dc* p``                        | ``p`` processors across multiple nodes. The ``*`` is significant. |
|                                 | significant. There is no space between ``dc`` and ``*``. There is |
|                                 | space between ``*`` and the value of ``p``.                       |
+---------------------------------+-------------------------------------------------------------------+
| **Requesting whole nodes**                                                                          |
+---------------------------------+-------------------------------------------------------------------+
| PE                              | Description                                                       |
+---------------------------------+-------------------------------------------------------------------+
| ``node* n``                     | ``n`` nodes (normally used with ``-l exclusive``)                 |
+---------------------------------+-------------------------------------------------------------------+


How to reserve an entire node
-----------------------------

**One or more whole nodes for parallel jobs**

To get one or more entire nodes for parallel jobs, use ``-pe node* n -l exclusive`` in your ``qsub`` or ``qrsh`` command or add them to your ``UGE`` command file, where ``n`` is the number of nodes you are requesting.

Example of requesting 2 whole nodes with ``qsub``: ::

 qsub -pe node 2 -l exclusive [other options]

Example of requesting 3 whole nodes in a ``UGE`` command file: ::

 #$ -l exclusive
 #$ -pe node 3 

**Submit a batch job from the UCLA Grid Portal**

.. warning:: UCLA Grid Portal will be taken down in near future.

The UCLA Grid Portal provides a web portal interface to the Hoffman2 Cluster. Every Hoffman2 Cluster user can access the UCLA Grid Portal. To submit a batch job from the UCLA Grid Portal, click the ``Job Services`` tab then click one of the following: ``Generic Jobs``, ``Applications``, or ``Multi-Jobs``.

*Generic Jobs*: Use this page to submit a job that runs a program or script that either you or a colleague have written and is usually installed in your home directory. In the fill-in form provided, supply the name of the executable, any job parameters, time request, number of processors.

*Applications*: Use this page to submit a commonly used application. Normally, you are required to know less about an application than a generic job, as the UCLA Grid Portal keeps track of the location of the executable and other information about the application. You normally must prepare an input file that the application will read or run. Some applications can present forms to you on the UCLA Grid Portal that you can fill in to create the input file if you are not familiar with application requirements.

*Multi-Jobs*: Use this page to submit multiple jobs that run a program or script that either you or a colleague have written. For details, see Running an Array of Jobs Using UGE.
After you submit a job, click ``Job Status`` where you can monitor its progress, and view and download its output after your job completes.

GPU-access
===========

How to access GPU nodes
-----------------------

In order to use a node that has a GPU, you need to request it from the job scheduler. Nodes may have two GPUs (Tesla T10) or three GPUs (Tesla M2070 nodes). To begin an interactive session, at the shell prompt, enter: ::

 qrsh -l gpu
 
On Hoffman2 there are currently four publicly available GPU nodes cuda capability of 6.1 (all other publicly available nodes have cuda capability less than 3), each of this node is equipped with one P4 card. To request one of these nodes, please use: ::

 qrsh -l gpu,P4

The above ``qrsh`` command will reserve an entire gpu node with its 2 or 3 gpu processors. An interactive session made with the above ``qrsh ``command will expire in 2 hours.

To specify a different time limit for your session, use the ``h_rt`` or time parameter. Example for requesting 9 hours: ::
 
 qrsh -l gpu,h_rt=9:00:00

To reserve two gpu-nodes use: ::
 
 qrsh -l gpu -pe dc_gpu 2

To see which node(s) were reserved, at a gpu-node shell prompt enter: ::
 
 get_pe_hostfile

To see if the gpu nodes are up and/or in use, at any shell prompt enter: ::
 
 qhost_gpu_nodes

To see the specifics for a particular gpu node, at a g-node shell prompt enter: ::

 gpu-device-query.sh

To get a quick session for compiling or testing your code: ::

 qrsh -l i,gpu

How to specify GPU types
------------------------
There are multiple GPU types available in the cluster. Each type of GPU has a different compute capability, memory size and clock speed, among other things. If your GPU program requires a specific GPU type to run, you need to specify it explicitly. Without specifying GPU type allows UGE to arbitrarily pick any available GPU for your job. You may need to compile your code on the machine that has the required type of GPU. Currently, the following GPU types are available:


+-------------+-------------------+-----------------+--------------------+-------------+
|GPU type     |Compute Capability | Number of cores | Global Memory Size | UGE option  |
+=============+===================+=================+====================+=============+
|Tesla V100   | 7.0               | 5120            | 32 GB              | -l gpu,V100 |
+-------------+-------------------+-----------------+--------------------+-------------+
|Tesla P4     | 6.1               | 2560            | 8 GB               | -l gpu,P4   |
+-------------+-------------------+-----------------+--------------------+-------------+
|Tesla T10    | 1.3               | 240             | 4.3 GB             | -l gpu,T10  |
+-------------+-------------------+-----------------+--------------------+-------------+
|Tesla M2070  | 2.0               | 448             | 5.6 GB             | -l gpu,fermi|
+-------------+-------------------+-----------------+--------------------+-------------+
|Tesla M2090  | 2.0               | 512             | 6 GB               | -l gpu,fermi|
+-------------+-------------------+-----------------+--------------------+-------------+


References:

http://developer.nvidia.com/cuda-gpus
http://www.nvidia.com/object/preconfigured-clusters.html
The ``UGE`` options in the table above can be combined with other ``UGE`` options, for example: ::

 qrsh -l gpu,fermi,h_rt=3:00:00

[1] If you specify ``-l fermi`` the job will go to either M2070 or M2090 GPU nodes. If you specify ``-l M2070`` the job will only go to M2070 and will not go to M2090 even when the later is available. If you specify ``-l M2090`` the job will only go to M2090 and will not go to M2070 even when the later is available. This implies potentially longer wait time.

For most users, we recommend using ``-l fermi`` instead of ``-l M2070`` or ``-l M2090`` unless you specifically want to use either one of them (e.g., benchmarking the differences between M2070 and M2090)

CUDA
----
CUDA is installed in ``/u/local/cuda/`` on the Hoffman2 Cluster. There are several versions available. To see which versions of cuda are available please issue: ::

 module av cuda

to load version 10.), use: ::

 module load cuda/10.0

.. Note:: You will be able to load a cuda module only when on a GPU node (either in an interactive session, requested with: ``qrsh -l gpu``, or within batch jobs in which you have requested one or more GPU nodes with: ``-l gpu``).

Already compiled samples of NVIDIA GPU Computing Software Development Kit are available, for example for cuda 10.0, in: ::

 /u/local/cuda/10.0/NVIDIA_CUDA-10.0_Samples/bin/x86_64/linux/releasemodule load cuda/10.0

To install CUDA in your home directory, please see the instructions in the ``/u/local/cuda/README_ATS`` file. To install the NVIDIA GPU Computing Software Development Kit in your home directory, please see the instructions in the



