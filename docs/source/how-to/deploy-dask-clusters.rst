Deploy Dask clusters
====================

This page describes various ways to set up Dask on different hardware, either
locally on your own machine or on a distributed cluster.  If you are just
getting started, then this page is unnecessary.  Dask does not require any setup
if you only want to use it on a single computer.

You can continue reading or watch the screencast below:

.. raw:: html

   <iframe width="560"
           height="315"
           src="https://www.youtube.com/embed/TQM9zIBzNBo"
           style="margin: 0 auto 20px auto; display: block;"
           frameborder="0"
           allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
           allowfullscreen></iframe>

Dask has two families of task schedulers:

1.  **Single-machine scheduler**: This scheduler provides basic features on a
    local process or thread pool.  This scheduler was made first and is the
    default.  It is simple and cheap to use.  It can only be used on a single
    machine and does not scale.
2.  **Distributed scheduler**: This scheduler is more sophisticated. It offers
    more features, but also requires a bit more effort to set up.  It can
    run locally or distributed across a cluster.

|

.. figure:: ../images/dask-overview-distributed-callout.svg
   :alt: Dask is composed of three parts. "Collections" create "Task Graphs" which are then sent to the "Scheduler" for execution. There are two types of schedulers that are described in more detail below.
   
   High level collections are used to generate task graphs which can be executed on a single machine or a cluster. Using the Distributed scheduler enables creation of a Dask cluster for multi-machine computation.

|

If you import Dask, set up a computation, and call ``compute``, then you
will use the single-machine scheduler by default.  To use the ``dask.distributed``
scheduler you must set up a ``Client``

.. code-block:: python

   import dask.dataframe as dd
   df = dd.read_csv(...)
   df.x.sum().compute()  # This uses the single-machine scheduler by default

.. code-block:: python

   from dask.distributed import Client
   client = Client(...)  # Connect to distributed cluster and override default
   df.x.sum().compute()  # This now runs on the distributed system

Note that the newer ``dask.distributed`` scheduler is often preferable, even on
single workstations.  It contains many diagnostics and features not found in
the older single-machine scheduler.

There are also a number of different *cluster managers* available, so you can use
Dask distributed with a range of platforms. These *cluster managers* deploy a scheduler
and the necessary workers as determined by communicating with the *resource manager*.
`Dask Jobqueue <https://github.com/dask/dask-jobqueue>`_, for example, is a set of
*cluster managers* for HPC users and works with job queueing systems
(in this case, the *resource manager*) such as `PBS <https://en.wikipedia.org/wiki/Portable_Batch_System>`_,
`Slurm <https://en.wikipedia.org/wiki/Slurm_Workload_Manager>`_,
and `SGE <https://en.wikipedia.org/wiki/Oracle_Grid_Engine>`_.
Those workers are then allocated physical hardware resources.

.. figure:: ../images/dask-cluster-manager.svg
   :scale: 50%

   An overview of cluster management with Dask distributed.

To summarize, you can use the default, single-machine scheduler to use Dask
on your local machine. If you'd like use a cluster *or* simply take advantage
of the :doc:`extensive diagnostics <../diagnostics-distributed>`,
you can use Dask distributed. The following resources explain
in more detail how to set up Dask on a variety of local and distributed hardware:

- Single Machine:
    - :doc:`Default Scheduler <deploy-dask/single-machine>`: The no-setup default.
      Uses local threads or processes for larger-than-memory processing
    - :doc:`dask.distributed <deploy-dask/single-distributed>`: The sophistication of
      the newer system on a single machine.  This provides more advanced
      features while still requiring almost no setup.
- Distributed computing:
    - `Beginner's Guide to Configuring a Dask distributed Cluster <https://blog.dask.org/2020/07/30/beginners-config>`_
    - `Overview of cluster management options <https://blog.dask.org/2020/07/23/current-state-of-distributed-dask-clusters>`_
    - :doc:`Manual Setup <deploy-dask/cli>`: The command line interface to set up
      ``dask-scheduler`` and ``dask-worker`` processes.  Useful for IT or
      anyone building a deployment solution.
    - :doc:`SSH <deploy-dask/ssh>`: Use SSH to set up Dask across an un-managed
      cluster.
    - :doc:`High Performance Computers <deploy-dask/hpc>`: How to run Dask on
      traditional HPC environments using tools like MPI, or job schedulers like
      SLURM, SGE, TORQUE, LSF, and so on.
    - :doc:`Kubernetes <deploy-dask/kubernetes>`: Deploy Dask with the
      popular Kubernetes resource manager using either Helm or a native deployment.
    - `YARN / Hadoop <https://yarn.dask.org/en/latest/>`_: Deploy
      Dask on YARN clusters, such as are found in traditional Hadoop
      installations.
    - `Dask Gateway <https://gateway.dask.org/>`_ provides a secure,
      multi-tenant server for managing Dask clusters and allows users to launch
      and use Dask clusters in a shared cluster environment.
    - :doc:`Python API (advanced) <deploy-dask/python-advanced>`: Create
      ``Scheduler`` and ``Worker`` objects from Python as part of a distributed
      Tornado TCP application.  This page is useful for those building custom
      frameworks.
    - :doc:`Docker <deploy-dask/docker>` images are available and may be useful
      in some of the solutions above.
    - :doc:`Cloud <deploy-dask/cloud>` for current recommendations on how to
      deploy Dask and Jupyter on common cloud providers like Amazon, Google, or
      Microsoft Azure.
- Hosted / managed Dask clusters (listed in alphabetical order):
    - `Coiled <https://coiled.io/>`_ handles the creation and management of
      Dask clusters on cloud computing environments (AWS, Azure, and GCP).
    - `Saturn Cloud <https://saturncloud.io/>`_ lets users create
      Dask clusters in a hosted platform or within their own AWS accounts.
