=====================
 Configuration Guide
=====================

.. toctree::
   :maxdepth: 4

   configurationfiles

This HOWTO explains the configuration steps for ELMA and openSUSE
distribution.

ELMA
====

snmptrap
========

-  `http://mathias-kettner.de/checkmk\_mkeventd\_traps.html <http://mathias-kettner.de/checkmk_mkeventd_traps.html>`__

rsyslog
=======

-  `http://www.rsyslog.com/doc/omfwd.html <http://www.rsyslog.com/doc/omfwd.html>`__
-  `http://www.rsyslog.com/doc/relp.html <http://www.rsyslog.com/doc/relp.html>`__
-  `http://www.elasticsearch.org/tutorials/2012/05/19/elasticsearch-for-logging.html <http://www.elasticsearch.org/tutorials/2012/05/19/elasticsearch-for-logging.html>`__
-  `http://wiki.rsyslog.com/index.php/HOWTO:\_rsyslog\_%2B\_elasticsearch <http://wiki.rsyslog.com/index.php/HOWTO:_rsyslog_%2B_elasticsearch>`__
-  `http://untergeek.com/2012/10/11/using-rsyslog-to-send-pre-formatted-json-to-logstash/ <http://untergeek.com/2012/10/11/using-rsyslog-to-send-pre-formatted-json-to-logstash/>`__
-  `http://wiki.rsyslog.com/index.php/DailyLogRotation <http://wiki.rsyslog.com/index.php/DailyLogRotation>`__
-  `http://www.rsyslog.com/normalizer-first-steps-for-mmnormalize/ <http://www.rsyslog.com/normalizer-first-steps-for-mmnormalize/>`__
-  `http://www.gossamer-threads.com/lists/rsyslog/users/8050 <http://www.gossamer-threads.com/lists/rsyslog/users/8050>`__

-  `Available rsyslog
   properties <http://www.rsyslog.com/doc/property_replacer.html>`__
-  `Configuration of rsyslog omelasticsearch
   module <http://wiki.rsyslog.com/index.php/Queues_on_v6_with_omelasticsearch>`__
-  `Using ElasticSearch Bulk Mode with
   rsyslog <http://blog.gerhards.net/2012/06/using-elasticsearch-bulk-mode-with.html>`__
-  `Rsyslog queueing in
   general <http://www.rsyslog.com/doc/queues.html>`__
-  `rsyslog templates &
   json <http://blog.gerhards.net/2012/04/rsyslog-templates-json.html>`__
-  TCP event forwarding from rsyslog to ElasticSearch and logtash
-  `http://www.canonical.com/sites/default/files/active/Whitepaper-CentralisedLogging-v1.pdf <http://www.canonical.com/sites/default/files/active/Whitepaper-CentralisedLogging-v1.pdf>`__

logstash
========

-  `Getting started with
   logstash <http://www.logstash.net/docs/1.1.5/tutorials/getting-started-simple>`__
-  TCP event input form rsyslog in JSON format

ElasticSearch
=============

-  Security - `SSL
   encryption <http://pulkitsinghal.blogspot.de/2012/04/securing-elasticsearch-for-direct.html>`__
   on all backends and frontends

-  `ElasticSearch
   installation <http://www.elasticsearch.org/guide/reference/setup/installation.html>`__
-  `http://jprante.github.io/2012/11/28/Elasticsearch-Java-Virtual-Machine-settings-explained.html <http://jprante.github.io/2012/11/28/Elasticsearch-Java-Virtual-Machine-settings-explained.html>`__

   The ES\_HEAP\_SIZE environment variable allows to set the heap
   memory that will be allocated to elasticsearch java process. It will
   allocate the same value to both ES\_MIN\_MEM and ES\_MAX\_MEM
   values.

  ::

      grep "ES_HEAP_SIZE" /opt/elasticsearch/bin/service/elasticsearch.conf
      set.default.ES_HEAP_SIZE=2048
      wrapper.java.initmemory=%ES_HEAP_SIZE%
      wrapper.java.maxmemory=%ES_HEAP_SIZE%

  The most important setting for Java memory allocation is the -Xmx to
  control the maximum allowed memory for the process, and -Xms to
  control the minimum allocated memory for the process (in general,
  the more memory allocated to the process, the better).

  ::

      -Xss256k
      -Xms2048m
      -Xmx2048m

  ::

      ps -l --cols 20000 15456
      F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY        TIME CMD
      0 S  1000 15456 15454  1  80   0 - 877136 futex_ ?         0:35 /usr/lib64/jvm/java-1.7.0-openjdk-1.7.0/jre/bin/java -Delasticsearch-service -Des.path.home=/opt/elasticsearch -Xss256k -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+HeapDumpOnOutOfMemoryError -Djava.awt.headless=true -Des.max-open-files=true -Xms2048m -Xmx2048m -Djava.library.path=/opt/elasticsearch/bin/service/lib -classpath /opt/elasticsearch/bin/service/lib/wrapper.jar:/opt/elasticsearch/lib/elasticsearch-0.19.12.jar:/opt/elasticsearch/lib/elasticsearch-0.19.12.jar:/opt/elasticsearch/lib/jna-3.3.0.jar:/opt/elasticsearch/lib/log4j-1.2.17.jar:/opt/elasticsearch/lib/lucene-analyzers-3.6.1.jar:/opt/elasticsearch/lib/lucene-core-3.6.1.jar:/opt/elasticsearch/lib/lucene-highlighter-3.6.1.jar:/opt/elasticsearch/lib/lucene-memory-3.6.1.jar:/opt/elasticsearch/lib/lucene-queries-3.6.1.jar:/opt/elasticsearch/lib/snappy-java-1.0.4.1.jar:/opt/elasticsearch/lib/sigar/sigar-1.6.4.jar -Dwrapper.key=g-MRKbW_xtz1rqsz -Dwrapper.port=32000 -Dwrapper.jvm.port.min=31000 -Dwrapper.jvm.port.max=31999 -Dwrapper.disable_console_input=TRUE -Dwrapper.pid=15454 -Dwrapper.version=3.5.14 -Dwrapper.native_library=wrapper -Dwrapper.service=TRUE -Dwrapper.cpu.timeout=10 -Dwrapper.jvmid=1 org.tanukisoftware.wrapper.WrapperSimpleApp org.elasticsearch.bootstrap.ElasticSearchF

-  `Running ElasticSearch as a
   deamon <https://github.com/elasticsearch/elasticsearch-servicewrapper>`__
-  `Setting up
   tutorial <http://www.elasticsearch.org/tutorials/2010/07/01/setting-up-elasticsearch.html>`__
-  `Running ElasticSearch as a
   non-root-user <http://www.elasticsearch.org/tutorials/2011/02/22/running-elasticsearch-as-a-non-root-user.html>`__

  ::

      grep "elasticsearch" /etc/security/limits.conf

      elasticsearch soft nofile 64000
      elasticsearch hard nofile 64000
      elasticsearch soft memlock unlimited
      elasticsearch hard memlock unlimited

-  The `number of open files
   descriptors <http://www.elasticsearch.org/tutorials/2011/04/06/too-many-open-files.html>`__
   for the user elasticsearch has to be increased to 64k.

  ::

      STATUS | wrapper  | 2013/01/09 06:27:33 | --> Wrapper Started as Daemon
      STATUS | wrapper  | 2013/01/09 06:27:33 | Java Service Wrapper Community Edition 64-bit 3.5.14
      STATUS | wrapper  | 2013/01/09 06:27:33 |   Copyright (C) 1999-2011 Tanuki Software, Ltd. All Rights Reserved.
      STATUS | wrapper  | 2013/01/09 06:27:33 |     http://wrapper.tanukisoftware.com
      STATUS | wrapper  | 2013/01/09 06:27:33 | 
      STATUS | wrapper  | 2013/01/09 06:27:33 | Launching a JVM...
      INFO   | jvm 1    | 2013/01/09 06:27:34 | WrapperManager: Initializing...
      INFO   | jvm 1    | 2013/01/09 06:27:39 | [2013-01-09 06:27:39,994][INFO ][bootstrap                ] max_open_files [63977]

  ::

      http://www.elasticsearch.org/guide/reference/mapping/core-types.html

-  `ElasticSearch
   configuration <http://www.elasticsearch.org/guide/reference/setup/configuration.html>`__
-  `Config
   Mappings <http://www.elasticsearch.org/guide/reference/mapping/conf-mappings.html>`__
-  `Index
   Templates <http://www.elasticsearch.org/guide/reference/api/admin-indices-templates.html>`__
-  `Using Elasticsearch for
   logs <http://www.elasticsearch.org/tutorials/2012/05/19/elasticsearch-for-logging.html>`__
-  `Rotate and optimize old logs on time-based
   indices <http://www.elasticsearch.org/tutorials/2012/05/19/elasticsearch-for-logging.html>`__
-  JSON events generated by rsyslog
-  JSON events generated by logstash
-  `http://edgeofsanity.net/article/2012/12/26/elasticsearch-for-logging.html <http://edgeofsanity.net/article/2012/12/26/elasticsearch-for-logging.html>`__
-  `http://edgeofsanity.net/archives.html <http://edgeofsanity.net/archives.html>`__
-  `http://edgeofsanity.net/article/2012/06/17/central-logging-with-open-source-software.html <http://edgeofsanity.net/article/2012/06/17/central-logging-with-open-source-software.html>`__
-  `https://github.com/logstash/logstash/wiki/Elasticsearch-Storage-Optimization <https://github.com/logstash/logstash/wiki/Elasticsearch-Storage-Optimization>`__
-  `http://asquera.de/opensource/2012/11/25/elasticsearch-pre-flight-checklist/ <http://asquera.de/opensource/2012/11/25/elasticsearch-pre-flight-checklist/>`__
-  `http://cdn.oreillystatic.com/en/assets/1/event/80/Wrangling%20Logs%20with%20Logstash%20and%20ElasticSearch%20Presentation.pdf <http://cdn.oreillystatic.com/en/assets/1/event/80/Wrangling%20Logs%20with%20Logstash%20and%20ElasticSearch%20Presentation.pdf>`__

Apache2
=======

-  `http://wiki.apache.org/httpd/PasswordBasicAuth <http://wiki.apache.org/httpd/PasswordBasicAuth>`__

Create new htpasswd file: htpasswd2 -c /etc/apache2/conf.d/htpasswd
admin

Append new user into existing htpasswd file: htpasswd2
/etc/apache2/conf.d/htpasswd admin

Nagios
======

-  `http://lancet.mit.edu/mwall/projects/nagios/ <http://lancet.mit.edu/mwall/projects/nagios/>`__
-  `http://www.novell.com/coolsolutions/feature/19843.html <http://www.novell.com/coolsolutions/feature/19843.html>`__
-  `http://nagiosgraph.svn.sourceforge.net/viewvc/nagiosgraph/trunk/nagiosgraph/README.html <http://nagiosgraph.svn.sourceforge.net/viewvc/nagiosgraph/trunk/nagiosgraph/README.html>`__

Kibana
======

-  Configuration of Kibana web frontend
-  `http://three.kibana.org/about.html <http://three.kibana.org/about.html>`__
-  `http://lbroudoux.wordpress.com/2013/04/30/real-time-analytics-with-elasticsearch-and-kibana3/ <http://lbroudoux.wordpress.com/2013/04/30/real-time-analytics-with-elasticsearch-and-kibana3/>`__
