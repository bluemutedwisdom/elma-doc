======================
 Administration Guide
======================

Basic Knowledge
===============

ELMA appliance comes out of the box without any Linux graphical user
interface. Therefore it is useful to have some basic knowledge of
working with a Linux shell because some operation and administration
tasks only can be done with shell commands.

Supported web browsers
======================

ELMA user and admin interfaces should work in all modern web browsers as
long as they support SVG:

-  Tested with **Firefox > 19.x**.
-  **IE8 & IE9 don't work correctly!**

ELMA admin interfaces
=====================

Depending on integration into your corporate infrastructure you have to
login with single sign on admin user credentials provided by your
operation or administration team.

Point your browser at your local webserver, the one with ELMA already
installed.

::

    https://syslog.elma.org/elasticsearch.html

-  `ElasticSearch Head <../_images/elasticsearch-head.png>`__ - Browsing
   and interacting with an ElasticSearch clusters
-  `ElasticSearch Paramedic <../_images/elasticsearch-paramedic.png>`__ -
   Monitors and inspect ElasticSearch clusters
-  `ElasticSearch BigDesk <../_images/elasticsearch-bigdesk.png>`__ - Live
   charts and statistics for ElasticSearch clusters
-  ElasticSearch Kibana - User frontend that allows search, graph,
   analyze and otherwise make sense of a mountain of logs

ElasticSearch Head
------------------

::

    https://syslog.elma.org:9443/_plugin/head/

By default Head will immediately attempt to connect to a cluster node at
`http://syslog.elma.org:9443/ <http://syslog.elma.org:9443/>`__. Enter a
different node address in the connect box and click 'Connect' if
required.

Head has a four major operations:

-  A cluster overview, which shows the topology of your cluster and allows
   you to perform index and node level operations
-  A couple of search interfaces that allow you to query the cluster a
   retrieve results in raw json or tabular format
-  Several quick access tabs that show the status of the cluster
-  An input section that allows arbitrary call to the RESTful API to be
   made. This interface includes several options that can be combined to
   produce interesting results:

  -  Select request method (get, put, post, delete), json query data, node, and path
  -  JSON validator
  -  Ability to repeat requests on a timer
  -  Ability to transform the result using javascript expressions
  -  Ability to collect results over time (using timer), or compare results
  -  Ability to chart the transformed results in a simple bar graph (including time series)

ATTENTION: **You can destroy and corrupt data** from this interface if
you are not careful.

ElasticSearch Paramedic
-----------------------

::

    https://syslog.elma.org:9443/_plugin/paramedic/

Paramedic displays real-time statistics and information about your
ElasticSearch nodes and indices, as well as shard allocation within the
cluster.

Basic information about your cluster:

-  cluster name, health
-  number of nodes and shards
-  etc.

Stats displays key metrics from the Nodes Stats API, updated every
second.

Nodes displays the most important information about the cluster nodes
(used disk space and memory, number of nodes, machine load and
ElasticSearch CPU consumption, etc.).

Indices displays basic information about the indices:

-  number of primary shards
-  number of replicas
-  basic index statistics, using the Cluster State, Indices Status and
   Indices Stats APIs

Primary shards are displayed in blue, allocated replicas in green,
unassigned replicas in yellow, and unassigned (missing) primary shards
in red.

To display shard allocation across the nodes, use the “Show Details”
button.

All information is updated periodically, which allows you to see node
and index statistics, shard initialization or relocation, etc. in real
time.

ElasticSearch BigDesk
---------------------

::

    https://syslog.elma.org:9443/_plugin/bigdesk/

With BigDesk you can connect to a particular ElasticSearch host:

-  You should see cluster name and list of its nodes
-  You can switch between nodes in the cluster
-  New nodes are added and old nodes are removed automatically on the
   fly
-  You can change the Bigdesk refresh interval and amount of data that
   is displayed by charts

ElasticSearch Kibana
--------------------

::

    https://syslog.elma.org

Kibana is an open source (Apache Licensed), browser based analytics and
search dashboard for ElasticSearch.

Nagois
------

::

    https://syslog.elma.org/nagios/

WebYast
-------

::

    https://syslog.elma.org:xxxx

ELMA user management
====================

-  ElasticSearch basic authentication - `Jetty
   plugin <https://github.com/sonian/elasticsearch-jetty#readme>`__
-  Add Jetty User or change Jetty Group
-  The passwords should be obfuscated or MD5 hashed using `Jetty
   Password
   Utility <http://wiki.eclipse.org/Jetty/Howto/Secure_Passwords>`__
-  ElasticSearch Jetty Passwd User Interface
-  ElasticSearch Jetty service UID and password

ELMA group and profile management
=================================

.. |image0| image:: elasticsearch-bigdesk.png
.. |image1| image:: elasticsearch-head.png
.. |image2| image:: elasticsearch-paramedic.png




