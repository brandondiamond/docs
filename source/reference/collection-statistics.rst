===============================
Collection Statistics Reference
===============================

.. default-domain:: mongodb
.. highlight:: javascript

Synopsis
--------

To fetch collection statistics, call the :func:`stats()` method on
a collection object in the :program:`mongo` shell:

.. code-block:: javascript

   db.collection.stats()

You may also use the literal command format:

.. code-block:: javascript

   db.runCommand( { collStats: "collection" } )

Replace "``collection``" in both examples with the name of the
collection you want statistics for. By default, the return values will
appear in terms of bytes. You can, however, enter a scaling
factor. For example, you can convert the return values to kilobytes
like so:

.. code-block:: javascript

   db.collection.stats(1024)

Or:

.. code-block:: javascript

   db.runCommand( { collStats: "collection", scale: 1024 } )

.. note::

   The scale factor rounds values to whole numbers. This can
   produce unpredictable and unexpected results in some situations.

.. seealso:: The documentation of the ":dbcommand:`collStats`" command
   and the ":func:`stats()`," method in the :program:`mongo` shell.

Fields
------

.. stats:: ns

   The namepsace of the current collection, which follows the format
   "``[database].[collection]``".

.. stats:: count

   The number of objects or documents in this collection.

.. stats:: size

   The size of the collection. The "``scale``" factor affects this
   value.

.. stats:: avgObjSize

   The average size of an object in the collection. The "``scale``"
   factor affects this value.

.. stats:: storageSize

   The total amount of storage size. This is equal to the total number
   of extents allocated by this collection. The "``scale``" factor affects this
   value.

.. stats:: numExtents

   The total number of contiguously allocated data file regions.

.. stats:: nindexes

   The number of indexes on the collection. On standard, non-capped
   collections, there is always at least one index on the primary key
   (i.e. :term:`_id`).

.. stats:: lastExtentSize

   The size of the last extent allocated. The "``scale``" factor
   affects this value.

.. stats:: paddingFactor

   The amount of space added to the end of each document at insert
   time. The document padding provides a small amount of extra space
   on disk to allow a document to grow slightly without needing to
   move the document. :program:`mongod` automatically calculates this
   padding factor

.. stats:: flags

   Indicates the number of flags on the current collection. At the
   preset, the only flag notes the existence of an :term:`index` on
   the :term:`_id` field.

.. stats:: totalIndexSize

   The total size of all indexes. The "``scale``" factor affects this
   value.

.. stats:: indexSizes

   This field specifies the key and size of every existing index on
   the collection. The "``scale``" factor affects this value.
