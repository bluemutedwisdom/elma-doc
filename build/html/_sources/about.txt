=======
 About
=======

Introduction
============

ELMA is a **logging and syslog framework** complemented by a range of
free and open-source tools that can help you aggregate and deliver
metrics, analytics and vital performance data.

Log normalization and a `web-based query
interface <_images/kibana-search.png>`__ makes searching billions of log
events for arbitrary strings as easy as searching the web.

Provided as an `preinstalled openSUSE
appliance <http://susestudio.com/a/TOYySW/enterprise-log-management-appliance--2>`__
which can be `deployed and customized <manuals/installation>`__ in a few
minutes ELMA comes out of the box with all necessary services, `fine
customization and performance tuning <manuals/configuration>`__ and allows
easy `adminstration <manuals/administration.html>`__ and
`operations <manuals/operation.html>`__.

Issues, problems, enhancements and feature requests are managed with
`ELMA project
workflow <http://code.google.com/p/enterprise-log-management-appliance/issues/list>`__.
For extended communication please visit `ELMA discussion
group <https://groups.google.com/forum/?fromgroups#!forum/enterprise-log-management-appliance>`__.

At the moment we review and completly rewrite our wiki - **Every help is
welcome**.

Overview
========

Ingredients
-----------

-  `ELMA <http://susestudio.com/a/TOYySW/enterprise-log-management-appliance--2>`__
   - Enterprise Log Management Appliance
-  `rsyslog <http://www.rsyslog.com/>`__ - Enhanced syslogd for Linux
   and Unix
-  `logstash <http://www.elasticsearch.org/overview/logstash/>`__ -
   Managing events and logs
-  `Apache httpd <http://httpd.apache.org/>`__ - Support SSL, basic
   authentication and log all or some incoming requests in plain text or
   json formats
-  `ElasticSearch <http://www.elasticsearch.org/overview/elasticsearch/>`__
   - Distributed, RESTful and full-text indexing and search engine based
   on `Apache Lucene <https://lucene.apache.org>`__
-  `ElasticSearch
   Head <https://github.com/mobz/elasticsearch-head#readme>`__ -
   Browsing and interacting with ElasticSearch clusters
-  `ElasticSearch
   BigDesk <https://github.com/lukas-vlcek/bigdesk/#readme>`__ - Live
   charts and statistics for ElasticSearch clusters
-  `ElasticSearch
   Paramedic <https://github.com/karmi/elasticsearch-paramedic#readme>`__
   - Monitors and inspect ElasticSearch clusters
-  `Kibana <http://www.elasticsearch.org/overview/kibana/>`__ - User
   frontend that allows search, graph, analyze and otherwise make sense
   of a mountain of logs

Cookbook
--------

**Ship logs from any source**, get the right timestamp, parse and
normalize them into structured JSON format, index them:

|image0|

Menue
-----

**All your logs** and other event data from all over your infrastructure
**in a central place** - `Search, graph and
analyse <_images/kibana-search.png>`__ them.

|image1|

Details
=======

Syslog evolution
----------------

Rsyslog default flat file ASCII log format is not exactly the most
efficient way for searching large volumes of data. For longer historical
retention as you can see below - it's not so bad.

Logging into traditional relational databases is not scalable if your
amount of data gets higher then 1.000 - 2.000 events/second because of
missing full-text indexing and poor full-text search capabilities -
**Search of Death**.

Projects like `Sphinx <http://sphinxsearch.com/>`__ addresses this
issues but implementations are complicated. You have to adapt all SQL
based applications.

Logging is mostly done too unstructured. No consistent application log
schema or syntax, no consistent way of routing, filtering, indexing and
searching. **No real standard on how to format and structure data** on a
syslog message.

Log normalization with `rsyslog
mmnormalize <http://www.rsyslog.com/normalizer-first-steps-for-mmnormalize/>`__
helps to parse unstructured messages but further limitations are the
**static database models**.

It's **time for change**.

Next generation databases
-------------------------

`ElasticSearch <http://www.elasticsearch.org>`__ is a new data storage
technology for dealing with tons of data. It's also a search engine
built on top of `Apache's Lucene project <https://lucene.apache.org>`__.
It scales very well, both for distributed full-text indexing and
distributed searching.

It's search engine's data model is `JSON <http://json.org/>`__, which is
the de-facto standard for representing data these days. It's roots lie
with schema-free and `document-oriented
databases <http://en.wikipedia.org/wiki/Document-oriented_database>`__,
and as shown by the `NoSQL
movement <http://en.wikipedia.org/wiki/NoSQL>`__, this model proves very
effective for building applications.

Moreover, with JSON it is simple to provide `semi-structured
data <http://en.wikipedia.org/wiki/Semi-structured_model>`__ with
complex entities as well as being programming language neutral with
first-level parser support.

Common Event Expression - CEE enhanced syslog
---------------------------------------------

On `syslog community <https://fedorahosted.org/lumberjack/>`__ also
interesting stuff is happening and more precisely on the topic of syslog
format extension and structuring syslog data.

The main idea about `structuring syslog
messages <http://blog.gerhards.net/2012/02/announcing-project-lumberjack.html>`__
is to represent them using JSON along with the current free form strings
to prevent backward compatibility breakage. Therefore this format has to
be normalized and extended so that syslog softwares such as rsyslog and
syslog-ng can directly understand them. That's where `CEE enhanced
syslog <http://cee.mitre.org>`__ and
`Lumberjack <https://fedorahosted.org/lumberjack/>`__ kick in.

Our goal
--------

The first part should be the rulebase which holds format representations
of expected log messages. This permits very high performance
normalization of semantically equal messages from different devices (and
thus in different syntaxes).

Parse and normalize events into structured JSON format via rsyslog
mmnormalize and mmjsonparse.

Rsyslog new omelasticsearch module ships your logs nearly in real time
into a ElasticSearch cluster by using ElasticSearch bulk API for
indexing. With bulk mode, message processing is much faster, especially
if large loads are to be processed.

Kibana user frontend queries the REST interface of ElasticSearch. It
provides **enterprise wide**  full-text search and allows you to plot,
calculate and drill down into your rsyslog `timebased indexed logging
data <http://www.elasticsearch.org/tutorials/using-elasticsearch-for-logs/>`__.

|image2|

**Enjoy the performance**, it's very awesome - really.

Why not just archive everything in ElasticSearch?
-------------------------------------------------

That's an option, however rsyslog flat files are much easier to handle.
Compressed rsyslog flat files are 90% or less of the size of compressed
ElasticSearch documents.

Assuming an average message lenght of 244 Byte per message and 10.000
events per second the following projection can give you an idea of
difficulties for suitable filesystem sizing:

+----------------------------+-------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
|                            | **Benchmark**     | **second**   | **minute**   | **hour**    | **day**     | **week**   | **month**   | **year**      |
+----------------------------+-------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Messages**               |                   | 10 k         | 600 k        | 36 M        | 864 M       | 6.048 M    | 181.440 M   | 2.177.280 M   |
+----------------------------+-------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Compressed data size**   | omfile            | 0.03 MB      | 1.74 MB      | 104.40 MB   | 2.45 GB     | 17.13 GB   | 68.51 GB    | 822.15 GB     |
+----------------------------+-------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Compressed data size**   | omelasticsearch   | 3.50 MB      | 210.00 MB    | 12.30 GB    | 295.31 GB   | 2.02 TB    | 8.07 TB     | 96.90 TB      |
+----------------------------+-------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+

For further details have a look into ELMA `performance and compression
tests <tests>`__.

If you have a large volume of logs and you intend on keeping them around
for compliance reasons for a long period of time archiving in rsyslog
flat files allows longer historical retention.

Why ELMA?
---------

Provided as an `preinstalled openSUSE
appliance <http://susestudio.com/a/TOYySW/enterprise-log-management-appliance--2>`__
which can be `deployed and customized <manuals/installation>`__ in a few
minutes ELMA comes out of the box with all necessary services, `fine
customization and performance tuning <manuals/configuration>`__ and allows
easy `adminstration <manuals/administration.html>`__ and
`operations <manuals/operation.html>`__:

-  Parse events into structured JSON format
-  Log router - route between different input and output sources

-  Daily based indexes
-  Optimization of time-based indices
-  Logrotation (delete old indexes)

-  Basic HTTP authentication on all backends and frontends
-  Apache authentication, authorisation and accounting (AAA)
   functionality
-  LDAP and RADIUS integration
-  SSL Offloading
-  High availability
-  Loadbalancing
-  Single Logon

-  Admin interfaces
-  Design your own custom dashboards with configurable panels
-  System monitoring due to Nagios Core integration
-  Rsyslog and ElasticSearch and other serviceable probes

-  Performance - ext4 filesystem
-  Operation - bind, ntp, snmp and snmptrap integration

-  `performance and compression tests <tests>`__

-  Flexible integrations - `vmesx, xen, hardware <manuals/requirements.html>`__
-  Vertical and horizontal scalable

Look at ELMA `Loadbalancing and Redundancy Guide <manuals/loadbalancing.html>`__
- a good starting point for understanding the communication matrix
inside ELMA.

Howto integrate into your existing logging and monitoring infrasructure?
------------------------------------------------------------------------

Many organizations already have centralized syslog server and are
comfortable with collecting logs this way. There are many good reasons
to do this even if you want to use an infrastructure like ELMA provides.
Simply forward these syslog events to the central ELMA infrastructure to
combine the benefits of both sides without affecting legacy
applications.

**Further event forwarding to your favorite SIEM system make sense!**

.. |image0| image:: syslog-infrastructure.png
.. |image1| image:: kibana-chart.png
.. |image2| image:: kibana-search.png
