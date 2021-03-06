.. _mongoexport:

.. default-domain:: mongodb

.. binary:: mongoexport

=============================
:program:`mongoexport` Manual
=============================

Synopsis
--------

:program:`mongoexport` is a utility that produces a JSON or CSV export
of data stored in a MongoDB instance. See the
":doc:`/administration/import-export`" document for a more in depth
usage overview, and the ":doc:`mongoimport`" document for more
information regarding the :program:`mongoimport` utility, which
provides the inverse "importing" capability.

.. note::

   Do not use :program:`mongoimport` and :program:`mongoexport` for
   full-scale backups because they may not reliably capture data type
   information. Use :program:`mongodump` and :program:`mongorestore` as
   described in ":doc:`/administration/backups`" for this kind of
   functionality.

Options
-------

.. program:: mongoexport

.. option:: --help

   Returns a basic help and usage text.

.. option:: --verbose, -v

   Increases the amount of internal reporting returned on the command
   line. Increase the verbosity with the ``-v`` form by including
   the option multiple times, (e.g. ``-vvvvv``.)

.. option:: --version

   Returns the version of the :program:`mongoexport` utility.

.. option:: --host <hostname><:port>

   Specifies a resolvable hostname for the :program:`mongod` from which you
   want to export data. By default :program:`mongoexport` attempts to
   connect to a MongoDB process ruining on the localhost port number
   27017.

   Optionally, specify a port number to connect a MongboDB instance
   running on a port other than 27017.

   To connect to a replica set, use the ``--host`` argument with a
   setname, followed by a slash and a comma separated list of host and
   port names. The ``mongo`` utility will, given the seed of at least
   one connected set member, connect to primary node of that set. this
   option would resemble: ::

        --host repl0 mongo0.example.net,mongo0.example.net,27018,mongo1.example.net,mongo2.example.net

   You can always connect directly to a single MongoDB instance by
   specifying the host and port number directly.

TODO copypaste

.. option:: --port <port>

   Specifies the port number, if the MongoDB instance is not running on
   the standard port. (i.e. ``27017``) You may also specify a port
   number using the :option:`mongoexport --host` command.

.. option:: --ipv6

   Enables IPv6 support to allow :program:`mongoexport` to connect to
   the MongoDB instance using IPv6 connectivity. All MongoDB programs
   and processes, including :program:`mongoexport`, disable IPv6
   support by default.

TODO copypaste

.. option:: --username <username>, -u <username>

   Specifies a username to authenticate to the MongoDB instance, if your
   database requires authentication. Use in conjunction with the
   :option:`mongoexport --password` option to supply a password.

.. option:: --password <password>

   Specifies a password to authenticate to the MongoDB instance. Use
   in conjunction with the :option:`--username` option to supply a
   username.

.. option:: --dbpath <path>

   Specifies the directory of the MongoDB data files. If used, the
   ``--dbpath`` option enables :program:`mongoexport` to attach
   directly to local data files and insert the data without the
   :program:`mongod`. To run with ``--dbpath``, :program:`mongoexport`
   needs to lock access to the data directory: as a result, no
   :program:`mongod` can access the same path while the process runs.

.. option:: --directoryperdb

   Use the :option:`--directoryperdb` in conjunction with the
   corresponding option to :program:`mongod`, which allows
   :program:`mongoexport` to export data into MongoDB instances that
   have every database's files saved in discrete directories on the
   disk. This option is only relevant when specifying the
   :option:`--dbpath` option.

.. option:: --journal

   Allows :program:`mongoexport` operations to access the durability
   :term:`journal` to ensure that the export is in a
   consistent state. This option is only relevant when specifying the
   :option:`--dbpath` option.

.. option:: --db <db>, -d <db>

   Use the :option:`--db` option to specify a database for
   :program:`mongoexport` to export data from. If you do not specify a
   DB, :program:`mongoexport` will export all databases in this
   MongoDB instance. Use this option to create a copy of a smaller
   subset of your data.

.. option:: --collection <collection>, -c <collection>

   Use the :option:`--collection` option to specify a collection for
   :program:`mongoexport` to export. If you do not specify a
   "``<collection>``", all collections will exported.

.. option:: --fields <field1[,field2]>, -f <field1[,field2]>

   Specify a field or number fields to *include* in the export. All
   other fields will be *excluded* from the export. Comma separate a
   list of fields to limit the fields exported.

.. option:: --fieldFile <file>

   As an alternative to ":option:`--fields <mongoexport --fields>`"
   the :option:`--fieldFile` option allows you to specify a file
   (e.g. ``<file>```) to hold a list of field names to specify a list
   of fields to *include* in the export. All other fields will be
   *excluded* from the export. Place one field per line.

.. option:: --query <JSON>

   Provides a :term:`JSON document` as a query that optionally limits
   the documents returned in the export.

.. option:: --csv

   Changes the export format to a comma separated values (CSV)
   format. By default :program:`mongoexport` writes data using one
   :term:`JSON` document for every MongoDB document.

.. option:: --jsonArray

   Modifies the output of :program:`mongoexport` so that to write the
   entire contents of the export as a single :term:`JSON` array. By
   default :program:`mongoexport` writes data using one JSON document
   for every MongoDB document.

.. option:: --slaveOk, -k

   Allows :program:`mongoexport` to read data from secondary or slave
   nodes when using :program:`mongoexport` with a replica set. This
   option is only available if connected to a :program:`mongod` or
   :program:`mongos` and is not available when used with the
   ":option:`mongoexport --dbpath`" option.

   This is the default behavior.

.. option:: --out <file>, -o <file>

   Specify a file to write the export to. If you do not specify a file
   name, the :program:`mongoexport` writes data to standard output
   (e.g. ``stdout``).

Usage
-----

In the following example, :program:`mongoexport` exports the
collection "``contacts``" from the "``users``" database from the
:program:`mongod` instance running on the localhost port
number 27017. This command writes the export data in :term:`CSV`
format into a file located at "``/opt/backups/contacts.csv``".

.. code-block:: sh

   mongoexport --db users --collection contacts --csv --file /opt/backups/contacts.csv

The next example creates an export of the collection "``contacts``"
from the MongoDB instance running on the localhost port number ``27017``,
with journaling explicitly enabled. This writes the export to the
``contacts.json`` file in :term:`JSON` format.

.. code-block:: sh

   mongoexport --collection contacts --file contacts.json --journal

The following example exports the collection "``contacts``" from the
"``sales``" database located in the MongoDB data files located at
``/srv/mongodb/``. This operation writes the export to standard output
in :term:`JSON` format.

.. code-block:: sh

   mongoexport --db sales --collection contacts --dbpath /srv/mongodb/

.. warning::

   The above example will only succeed if there is no :program:`mongod`
   connected to the data files located in the ``/srv/mongodb/``
   directory.

The final example exports the collection "``contacts``" from the
database "``marketing``" . This data resides on the MongoDB instance
located on the host ``mongodb1.example.net``" running on port
``37017``", which requires the username "``user``" and the password
"``pass``".

.. code-block:: sh

   mongoexport --host mongodb1.example.net --port 37017 --username user --password pass --collection contacts --db marketing --file mdb1-examplenet.json
