===================================
 Performance and compression tests
===================================

Feeding generic log events with a constant logging rate to local or
remote rsyslog server to determine the **maximum receiving log rate**
(MPS - Messages Per Second) considering:

-  no `rsyslog on-disk
   queueing <http://www.rsyslog.com/doc/rsyslog_high_database_rate.html>`__
   (Indication that import and indexing into ElasticSearch is fast
   enough)
-  `prevention of message
   loss <http://www.rsyslog.com/doc/rsyslog_high_database_rate.html>`__
-  **real time compression** of stored events
-  **ElasticSearch** near **real time inserting and indexing** (indexing
   delay < 30s)


Generation of continious tcp logging
====================================

+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| **Server**              | `rsyslog <http://www.rsyslog.com>`__                                          | 7.2.5 (v7-stable)   | localhost   |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| **Benchmark tool**      | `tcpflood <http://www.rsyslog.com/how-to-use-tcpflood/>`__                    | 7.2.5               | localhost   |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| Protocol                | TCP                                                                           |                     | tcp/13514   |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| Compression             | `zlib-compression <http://www.rsyslog.com/doc/rsyslog_conf_actions.html>`__   |                     | -           |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| Encryption              | `TLS (SSL) <http://www.rsyslog.com/doc/rsyslog_secure_tls.html>`__            |                     | -           |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| Message lenght (Byte)   |                                                                               |                     | 244         |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| Logging rate (MPS)      |                                                                               |                     | 10.000      |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| Runs                    |                                                                               |                     | 10          |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+
| Total messages          |                                                                               |                     | 100.000     |
+-------------------------+-------------------------------------------------------------------------------+---------------------+-------------+

rsyslog output modules
======================

rsyslog `output
modules <http://www.rsyslog.com/doc/rsyslog_conf_modules.html/rsyslog_conf_modules.html>`__
process messages. With them, message formats can be transformed and
messages be transmitted to various different targets.

omfile - File Output Module
---------------------------

::

    /usr/share/elma-tests/bin/elma_imptcp_gzipwr.sh
    ===========================================================================================
    Scriptname : elma_imptcp_gzipwr.sh
    -------------------------------------------------------------------------------------------
    Description: rsyslogd performance test with gzip output compression
    Author     : Joerg Heinemann
    Website    : http://enterprise-log-management-appliance.org
    ===========================================================================================
    PASS:
    -------------------------------------------------------------------------------------------
    Raw message lenght (Byte):                              230
    Total message lenght (Byte):                            244
    Messages sent during one tcpflood test:                 10000
    Number of tcpflood tests:                               10
    Seconds to sleep between tcpflood runs:                 1
    Concurrent tcpflood connections:                        5
    tcpflood transport protocol:                            tcp
    tcpflood rsyslog target port:                           13514
    tcpflood rsyslog target address:                        127.0.0.1
    Total messages:                                         100000
    Total tcpflood runtime (milli seconds):                 10141
    Loging rate (MPS):                                      9860
    Transmission speed (MBit/s):                            18.35
    Compressed data size (MByte):                           .29
    Compressed data indexing runtime (milli seconds):
    Compression method:                                     $OMFileZipLevel 9
    Compression ratio (%):                                  98.71
    Decompression runtime (milli seconds):                  454
    Uncompressed data size (MByte):                         23.26
    Uncompressed data indexing runtime (milli seconds):
    ===========================================================================================

.. toctree::
   :maxdepth: 4

   omfile

omelasticsearch - ElasticSearch Output Module
---------------------------------------------

::

    /usr/share/elma-tests/bin/elma_elasticsearch_compression_rate.sh
    ===========================================================================================
    Scriptname : elma_elasticsearch_compression_rate.sh
    -------------------------------------------------------------------------------------------
    Description: rsyslogd performance and elasticsearch compression test
    Author     : Joerg Heinemann
    Website    : http://enterprise-log-management-appliance.org
    ===========================================================================================
    PASS:
    -------------------------------------------------------------------------------------------
    Raw message lenght (Byte):                              230
    Total message lenght (Byte):
    Messages sent during one tcpflood test:                 10000
    Number of tcpflood tests:                               10
    Seconds to sleep between tcpflood runs:                 1
    Concurrent tcpflood connections:                        5
    tcpflood transport protocol:                            tcp
    tcpflood rsyslog target port:                           13514
    tcpflood rsyslog target address:                        127.0.0.1
    Total messages:                                         100000
    Total tcpflood runtime (milli seconds):                 10190
    Loging rate (MPS):                                      9813
    Transmission speed (MBit/s):
    Compressed data size (MByte):                           23.44
    Compressed data indexing runtime (milli seconds):       11353
    Compression method:                                     index.store.compress.stored = true
    Compression ratio (%):                                  69.65
    Decompression runtime (milli seconds):
    Uncompressed data size (MByte):                         77.25
    Uncompressed data indexing runtime (milli seconds):     12113
    ===========================================================================================

.. toctree::
   :maxdepth: 4

   omelasticsearch

JSON format:

::

    {
        _index: rsyslog-2012-03-01
        _type: events
        _id: Oq9TRThNRVyyEYWRtA3geA
        _version: 1
        _score: 1
        _source: {
            @timestamp: 2012-03-01T01:00:00+01:00
            @source_host: 172.20.245.8
            @source: syslog://127.0.0.1
            @message: Mar 1 01:00:00 tag: msgnum:00000001:200:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
            @fields: {
                facility: 20
                facility_label: local4
                severity: 7
                severity_label: debug
                priority: 167
                program: tag
                processid: -
            }
            @customer: AIDB
            @type: events
        }
    }

Comparison
==========

+-----------------------+----------------+----------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+----------------------------+-------------------------+
| **Benchmark**         | **Messages**   | **Message lenght**   | **Data size**   | **Compression method**                                                                                                | **Compressed data size**   | **Compression ratio**   |
|                       |                | (Byte)               | (MByte)         |                                                                                                                       | (MByte)                    | (%)                     |
+-----------------------+----------------+----------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+----------------------------+-------------------------+
| **omfile**            | 100.000        | 244                  | 23.26           | `rsyslog gzip compression <http://www.rsyslog.com/doc/omfile.html>`__ - ZipLevel 9                                    | 0.29                       | 98                      |
+-----------------------+----------------+----------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+----------------------------+-------------------------+
| **omelasticsearch**   | 100.000        | 244                  | 77.25           | -`Des.index.store.compress.stored <http://www.elasticsearch.org/guide/reference/index-modules/store.html>`__\ =true   | 23.44                      | 69                      |
+-----------------------+----------------+----------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------+----------------------------+-------------------------+

::

    Compression ratio = 100 * ( Data size - Compressed data size ) / Data size

Results of `comparable compression
tests <https://gist.github.com/2970009>`__.

Log space sizing
================

In real life ElasticSearch compression rates of 55% and flat file
compression rates of over 90% are realistic.

Assuming an average message lenght of 244 Byte per message and 10.000
events per second the following projection can give you an idea of
difficulties for suitable filesystem sizing:

+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
|                            | **Benchmark**     | **LVM Volume**           | **second**   | **minute**   | **hour**    | **day**     | **week**   | **month**   | **year**      |
+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Messages**               |                   |                          | 10 k         | 600 k        | 36 M        | 864 M       | 6.048 M    | 181.440 M   | 2.177.280 M   |
+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Compressed data size**   | omfile            | /var/log/syslog          | 0.03 MB      | 1.74 MB      | 104.40 MB   | 2.45 GB     | 17.13 GB   | 68.51 GB    | 822.15 GB     |
+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Compressed data size**   | omelasticsearch   | /var/lib/elasticsearch   | 3.50 MB      | 210.00 MB    | 12.30 GB    | 295.31 GB   | 2.02 TB    | 8.07 TB     | 96.90 TB      |
+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+

Testbed
=======

Hardware for this testbed:

-  Low performance desktop PC based on a ASUS mainboard with a dual core
   AMD Athlon(tm) II X2 240e Processor
-  8 GB RAM
-  3 older slow IDE hard disk drives, configured in a RAID-5 fashion
   using the linux (x86\_64) built-in softRAID features
