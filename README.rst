==============
MonetDB docker
==============
Docker container for MonetDB_.

.. _MonetDB: https://www.monetdb.org/

-----
Usage
-----
The simplest way to start a MonetDB container is to use the images
published on DockerHub_::

  docker run -P -it monetdb/monetdb:latest

.. _DockerHub: https://hub.docker.com/repository/docker/monetdb/monetdb/tags

This command will start a container from the latest tagged image on
docker hub and it will map a random port on localhost to the port
50000 where the MonetDB daemon is listening for connections (the
effect of the ``-P`` flag). Alternatively you can specify the port on
the local host using the flag ``-p``.

The docker image creates the database farm on the directory
``/var/monetdb5/dbfarm/`` in the container the first time it
starts. You can mount a local directory on that path in the container
in order to have access to the data from the local host.

The image accepts a number of parameters in the form of environment
variables (i.e. passed using the ``-e`` docker flag) that will
configure the behavior of the container.

MDB_DAEMON_PASS
   This is the passphrase used to contact the MonetDB daemon remotely,
   i.e. from outside the container. For example::

    monetdb -h localhost -P <passphrase> create SF-1

   The default value is ``monetdb`` but we *strongly* advise you to set a
   different value explicitly.

MDB_LOGFILE
   The file where the daemon should write the log messages. By default
   it’s the file ``merovingian.log`` relative to the database farm
   directory in the container.

MDB_SNAPSHOT_DIR
   This is the directory in the container where database snapshots
   will be written. If no value is given the daemon will not produce
   any snapshots. You can mount a local directory in order to have
   access to the snapshots from the local host.

MDB_SNAPSHOT_COMPRESSION
   This specifies the compression algorithm to be used for snapshot
   files. Default value is ``.tar.lz4`` and the other possible values are
   ``.tar``, ``.tar.gz``, ``.tar.xz`` and ``.tar.bz2``.

MDB_CREATED_DBS
   This specifies what databases to build the first time the container
   comes up. You can specify multiple databases by separating their
   names with a single comma character (no spaces between them)::
     [...] -e MDB_CREATED_DBS=db1,db2,db3 [...]

MDB_DB_ADMIN_PASS
   The password to use for the ``monetdb`` (admin) user in the created
   databases. By default it is ``monetdb`` but we *strongly* advise
   you to set a different password. Please note that all the databases
   get the same admin password.

Access
------
Once the image is running you can get a shell in it::

  docker exec -it <image_name> bash

---------------------------
Building the image manually
---------------------------

Clone this git repository_ and run::

  docker build . -t <local tag>

Then you can use the image you just built as described in section
`Usage`_ above.

.. _repository: https://github.com/MonetDBSolutions/monetdb-docker
