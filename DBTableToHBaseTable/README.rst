==============================================================
Batch Database Table To CDAP HBase Table Adapter Configuration
==============================================================

The ETL Batch Template can be used to create an Adapter that reads from a Batch Source and
persists it to a Sink. In this example, we will read an entire DB table in batch and use a
TableSink to write the database table's rows to HBase.

The config.json contains a sample Adapter configuration that you can use to accomplish the
above task. Our sample Adapter uses these components:

- ETLBatch Application Template, since we want to perform ETL in batch
- Database source, to read data from the Database table 
- Table sink, to write the rows from the Database table to an HBase table
- A jar file containing the JDBC driver for your database. Along with this, you also need 
  a JSON file that describes the JDBC driver as an external plugin. This file should have
  the same name as the jar file (with only the extension changed to '.json'). See
  ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json`` as examples.

You can create and start the Adapter by using the CDAP CLI (or you can use the UI for a
more visual approach).

Note: You need to fill in the following configurations in a file such as config.json
before creating the Adapter.

Configurations for the Database Table Source
--------------------------------------------

#. ``driverClass``: This is the JDBC driver class used to connect to your database.

#. ``connectionString``: This is the JDBC connection string that includes the database name.

#. ``tableName``: This is the table from which you wish to import.

#. ``user``: This is the username used to connect to the specified database. It is 
   required for databases that need authentication, optional for those that do not.

#. ``password``: This is the password used to connect to the specified database. If the 
   database requires authentication, both username and password must be provided.

#. ``importQuery``: This is the SELECT query used to import data from the specified table. 
   You can specify an arbitrary number of columns to import, or import all columns using
   \*. You can also specify a number of WHERE clauses or ORDER BY clauses. However, LIMIT
   and OFFSET clauses should not be used in this query.

#. ``countQuery``: This is the SELECT query used to get the count of records to import 
   from the specified table. Examples: SELECT COUNT(*) from <my_table> where <my_column>
   1, SELECT COUNT(my_column) from my_table). NOTE: Please include the same WHERE clauses
   in this query as the ones used in the import query to reflect an accurate number of
   records to import.

#. ``jdbcPluginName``: The name of the external JDBC plugin. This is the value of the 
   ``name`` field in the external plugin's JSON configuration file. Defaults to 'jdbc'.
   Please find examples of external plugins in the files
   ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json``. Also refer to the
   CDAP documentation on external plugins for more details.

#. ``jdbcPluginType``: The name of the external JDBC plugin. This is the value of the
   ``type`` field in the external plugin's JSON configuration file. Defaults to 'jdbc'.
   Please find examples of external plugins in the files
   ``mysql-connector-java-5.1.35.json`` and ``postgresql-9.4.json``. Also refer to the
   CDAP documentation on external plugins for more details.

Configurations for the CDAP HBase Table Sink
--------------------------------------------

#. ``name``: This is the name of the HBase table to use as a sink.

#. ``schema``: This is the JSON representation of the HBase Table's schema.

#. ``schema.row.field``: This is the field in the ``StructuredRecord`` input to this Sink
   that will be used as the ``rowKey`` in the HBase table.

Creating an adapter using CDAP CLI
----------------------------------

::

  cdap> create adapter dbIngest DBTableToHBaseTable/config.json
  Successfully created adapter 'dbIngest'

  cdap> start adapter dbIngest
  Successfully started adapter 'dbIngest'

To verify that the data has been written to the HBase Table execute the following CDAP CLI
command::

  cdap> execute 'select * from dataset_hbase_postgres_table'

You have now successfully created an Adapter that reads from a Database Table and writes
to a CDAP HBase Table.

To stop and delete the Adapter execute the following commands using the CDAP CLI::

  cdap> stop adapter dbIngest
  Successfully stopped adapter 'dbIngest'

  cdap> delete adapter dbIngest
  Successfully deleted adapter 'dbIngest'


Share and Discuss!
==================

Have a question? Discuss at the `CDAP User Mailing List
<https://groups.google.com/forum/#!forum/cdap-user>`__.

License
=======

Copyright © 2015 Cask Data, Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
