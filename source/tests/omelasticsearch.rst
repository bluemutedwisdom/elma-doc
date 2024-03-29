=========================================
 rsyslog omelasticsearch bechmark report
=========================================

::

    /usr/share/elma-tests/bin/elma_elasticsearch_compression_rate.sh
    ===========================================================================================
    Scriptname : elma_elasticsearch_compression_rate.sh
    -------------------------------------------------------------------------------------------
    Description: rsyslogd performance and elasticsearch compression test
    Author     : Joerg Heinemann
    Website    : http://enterprise-log-management-appliance.org
    ===========================================================================================
    --- without compression
    ===========================================================================================
    --- 23:47:38.195 - init -------------------------------------------------------------------
    --- 23:47:38.228 - delete elasticsearch diag index ----------------------------------------
    {"ok":true,"acknowledged":true}
    --- 23:47:38.581 - start rsyslogd ---------------------------------------------------------
    # author: Joerg Heinemann
    # website: http://enterprise-log-management-appliance.org
    # license: GPL v3
    #
    # /opt/elma/config/rsyslog.d/elma_imptcp_elasticsearch-no-compression.conf
    #
    # simple async writing test to elasticsearch
    # Joerg Heinemann, 2012-12-31
    $IncludeConfig /opt/elma/config/rsyslog.d/diag-common.conf
    $MaxMessageSize 10k
    $ModLoad imptcp
    $MainMsgQueueTimeoutShutdown 10000
    $InputPTCPServerRun 13514
    # ######### Enable In-Memory queues ##########
    #
    # An in-memory queue is created for this action. If the remote host or
    # database is down, messages are spooled to memory and sent when it is
    # up again.
    #
    # The advantages are that it will be consistently good in terms of performance
    # and it will fail fast.
    #
    # The downside is that you can keep a limited number of messages in that queue,
    # less than you normally can with a disk-assisted queue.
    #
    # allocate memory dynamically for the queue. Better for handling spikes
    $MainMsgQueueType LinkedList
    # how many messages (messages, not bytes!) to hold in memory
    $MainMsgQueueSize 8000000
    # don't throttle receiving messages when the queue gets full
    $MainMsgQueueTimeoutEnqueue 0
    # ######### Event forwarding to ElasticSearch ##########
    #
    # Enable ElasticSearch output
    #
    $ModLoad omelasticsearch.so
    # Template for JSON output
    #
    #$template ls_json,"{\"@timestamp\":\"%timereported:1:19:date-rfc3339%.%timereported:1:3:date-subseconds%\",\"@source_host\":%source:::jsonf%\",\"@source\":\"syslog://%fromhost-ip:::json%\",\"@message\":\"%timereported% %app-name%:%msg:::json%\",\"@fields\":{%syslogfacility:facility%,%syslogfacility-text:facility_label%,%syslogseverity:severity%,%syslogseverity-text:severity_label%,%pri:priority%,%app-name:program%,%procid:processid%},\"@customer\":\"AIDB\"}"
    $template ls_json,"{%timestamp:::date-rfc3339,jsonf:@timestamp%,%source:::jsonf:@source_host%,\"@source\":\"syslog://%fromhost-ip:::json%\",\"@message\":\"%timestamp% %app-name%:%msg:::json%\",\"@fields\":{%syslogfacility:::jsonf:facility%,%syslogfacility-text:::jsonf:facility_label%,%syslogseverity:::jsonf:severity%,%syslogseverity-text:::jsonf:severity_label%,%pri:::jsonf:priority%,%app-name:::jsonf:program%,%procid:::jsonf:processid%},\"@customer\":\"AIDB\",\"@type\":\"events\"}"
    $template srchidx,"diag-no-compression"
    # now we put everything together
    # "template" is for storing the syslog fields we want
    #
    local4.* action(type="omelasticsearch"
                    template="ls_json"
                    searchIndex="srchidx"
                    dynSearchIndex="on"
                    asyncrepl="on"
                    searchType="events"
                    server="localhost"
                    serverport="9200"
    #                uid="elma"
    #                pwd="fjg892345sdfsdwf43d"
                    bulkmode="on"
                    queue.dequeuebatchsize="65536"
                    queue.type="linkedlist"
                    queue.filename="dbq"
                    queue.highwatermark="500000"
                    queue.lowwatermark="400000"
                    queue.discardmark="5000000"
                    queue.timeoutenqueue="0"
                    queue.maxdiskspace="5g"
                    queue.size="2000000"
                    queue.saveonshutdown="on"
                    action.resumeretrycount="-1")
    # There are some other nice things you can use:
    #
    #    searchIndex               - for letting rsyslog know where to get these names
    #    dynSearchIndex="on"       - for having variable index names
    #    searchType="mycustomtype" - to specify a different type than "events".
    #                                You can have dynSearchType="on" to have it variable, like you can with indices.
    #    serverport="9200"         - the default setting, but you can specify a different port
    #    asyncrepl="on"            - to enable asyncronous replication.
    #                                That is, Elasticsearch gives an answer imediately
    #                                after inserting to the main shard(s).
    #                                It doesn't wait for replicas to be updated as well, which is the default setting
    #    timeout="1m"              - how long to wait for a reply from Elasticsearch.
    #                                More info here, near the end:
    #                                http://www.elasticsearch.org/guide/reference/api/index_.html
    # /opt/elma/config/rsyslog.d/diag-common.conf
    #
    # This is a config include file. It sets up rsyslog so that the
    # diag system can successfully be used. Also, it generates a file
    # "rsyslogd.started" after rsyslogd is initialized. This config file
    # should be included in all tests that intend to use common code for
    # controlling the daemon.
    # NOTE: we assume that rsyslogd's current working directory is
    # ./tests (or the distcheck equivalent), in particlular that this
    # config file resides in the testsuites subdirectory.
    # rgerhards, 2009-05-27
    $ModLoad imdiag
    $IMDiagServerRun 13500
    $template startupfile,"rsyslogd.started" # trick to use relative path names!
    :syslogtag, contains, "rsyslogd"  ?startupfile
    # I have disabled the directive below, so that we see errors in testcase
    # creation. I am not sure why it was present in the first place, so for
    # now I just leave it commented out -- rgerhards, 2011-03-30
    #$ErrorMessagesToStderr off
    --- 23:47:38.710 - wait for rsyslogd startup ----------------------------------------------
    rsyslogd: error starting up disk queue, using pure in-memory mode [try http://www.rsyslog.com/e/2036 ]
    rsyslogd started with pid  15881
    --- 23:47:38.832 - run tcpflood -----------------------------------------------------------
    00005 open connections
    starting run 1
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.116
    sleeping 1 seconds before next run
    starting run 2
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.126
    sleeping 1 seconds before next run
    starting run 3
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.114
    sleeping 1 seconds before next run
    starting run 4
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.124
    sleeping 1 seconds before next run
    starting run 5
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.148
    sleeping 1 seconds before next run
    starting run 6
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.127
    sleeping 1 seconds before next run
    starting run 7
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.109
    sleeping 1 seconds before next run
    starting run 8
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.119
    sleeping 1 seconds before next run
    starting run 9
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.077
    sleeping 1 seconds before next run
    starting run 10
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.110
    Runs:     10
    Runtime:
      total:  1.170
      avg:    0.117
      min:    0.077
      max:    0.148
    All times are wallclock time.
    00005 close connections
    End of tcpflood Run
    Raw message lenght (Byte):                      230
    Messages sent during one tcpflood test:         10000
    Number of tcpflood tests:                       10
    Seconds to sleep between tcpflood runs:         1
    Concurrent tcpflood connections:                5
    tcpflood transport protocol:                    tcp
    tcpflood rsyslog target port:                   13514
    tcpflood rsyslog target address:                127.0.0.1
    Total messages:                                 100000
    Total tcpflood runtime (milli seconds):         10182
    Loging rate (MPS):                              9821
    --- 23:47:49.027 - count elasticsearch indexed messages -----------------------------------
    37253 messages indexed (1000 milliseconds - Abort in 120000 milliseconds)
    74143 messages indexed (2000 milliseconds - Abort in 180000 milliseconds)
    74143 messages indexed (3000 milliseconds - Abort in 180000 milliseconds)
    74143 messages indexed (4000 milliseconds - Abort in 179000 milliseconds)
    74143 messages indexed (5000 milliseconds - Abort in 178000 milliseconds)
    74143 messages indexed (6000 milliseconds - Abort in 177000 milliseconds)
    100000 messages indexed (7000 milliseconds - Abort in 176000 milliseconds)
    --- 23:47:56.280 - show elasticsearch index statistics ------------------------------------
    {
      "ok" : true,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "failed" : 0
      },
      "_all" : {
        "primaries" : {
          "docs" : {
            "count" : 100000,
            "deleted" : 0
          },
          "store" : {
            "size" : "77.2mb",
            "size_in_bytes" : 81009144,
            "throttle_time" : "0s",
            "throttle_time_in_millis" : 0
          },
          "indexing" : {
            "index_total" : 100000,
            "index_time" : "12.1s",
            "index_time_in_millis" : 12113,
            "index_current" : 0,
            "delete_total" : 0,
            "delete_time" : "0s",
            "delete_time_in_millis" : 0,
            "delete_current" : 0
          },
          "get" : {
            "total" : 0,
            "time" : "0s",
            "time_in_millis" : 0,
            "exists_total" : 0,
            "exists_time" : "0s",
            "exists_time_in_millis" : 0,
            "missing_total" : 0,
            "missing_time" : "0s",
            "missing_time_in_millis" : 0,
            "current" : 0
          },
          "search" : {
            "query_total" : 0,
            "query_time" : "0s",
            "query_time_in_millis" : 0,
            "query_current" : 0,
            "fetch_total" : 0,
            "fetch_time" : "0s",
            "fetch_time_in_millis" : 0,
            "fetch_current" : 0
          }
        },
        "total" : {
          "docs" : {
            "count" : 100000,
            "deleted" : 0
          },
          "store" : {
            "size" : "77.2mb",
            "size_in_bytes" : 81009144,
            "throttle_time" : "0s",
            "throttle_time_in_millis" : 0
          },
          "indexing" : {
            "index_total" : 100000,
            "index_time" : "12.1s",
            "index_time_in_millis" : 12113,
            "index_current" : 0,
            "delete_total" : 0,
            "delete_time" : "0s",
            "delete_time_in_millis" : 0,
            "delete_current" : 0
          },
          "get" : {
            "total" : 0,
            "time" : "0s",
            "time_in_millis" : 0,
            "exists_total" : 0,
            "exists_time" : "0s",
            "exists_time_in_millis" : 0,
            "missing_total" : 0,
            "missing_time" : "0s",
            "missing_time_in_millis" : 0,
            "current" : 0
          },
          "search" : {
            "query_total" : 0,
            "query_time" : "0s",
            "query_time_in_millis" : 0,
            "query_current" : 0,
            "fetch_total" : 0,
            "fetch_time" : "0s",
            "fetch_time_in_millis" : 0,
            "fetch_current" : 0
          }
        },
        "indices" : {
          "diag-no-compression" : {
            "primaries" : {
              "docs" : {
                "count" : 100000,
                "deleted" : 0
              },
              "store" : {
                "size" : "77.2mb",
                "size_in_bytes" : 81009144,
                "throttle_time" : "0s",
                "throttle_time_in_millis" : 0
              },
              "indexing" : {
                "index_total" : 100000,
                "index_time" : "12.1s",
                "index_time_in_millis" : 12113,
                "index_current" : 0,
                "delete_total" : 0,
                "delete_time" : "0s",
                "delete_time_in_millis" : 0,
                "delete_current" : 0
              },
              "get" : {
                "total" : 0,
                "time" : "0s",
                "time_in_millis" : 0,
                "exists_total" : 0,
                "exists_time" : "0s",
                "exists_time_in_millis" : 0,
                "missing_total" : 0,
                "missing_time" : "0s",
                "missing_time_in_millis" : 0,
                "current" : 0
              },
              "search" : {
                "query_total" : 0,
                "query_time" : "0s",
                "query_time_in_millis" : 0,
                "query_current" : 0,
                "fetch_total" : 0,
                "fetch_time" : "0s",
                "fetch_time_in_millis" : 0,
                "fetch_current" : 0
              }
            },
            "total" : {
              "docs" : {
                "count" : 100000,
                "deleted" : 0
              },
              "store" : {
                "size" : "77.2mb",
                "size_in_bytes" : 81009144,
                "throttle_time" : "0s",
                "throttle_time_in_millis" : 0
              },
              "indexing" : {
                "index_total" : 100000,
                "index_time" : "12.1s",
                "index_time_in_millis" : 12113,
                "index_current" : 0,
                "delete_total" : 0,
                "delete_time" : "0s",
                "delete_time_in_millis" : 0,
                "delete_current" : 0
              },
              "get" : {
                "total" : 0,
                "time" : "0s",
                "time_in_millis" : 0,
                "exists_total" : 0,
                "exists_time" : "0s",
                "exists_time_in_millis" : 0,
                "missing_total" : 0,
                "missing_time" : "0s",
                "missing_time_in_millis" : 0,
                "current" : 0
              },
              "search" : {
                "query_total" : 0,
                "query_time" : "0s",
                "query_time_in_millis" : 0,
                "query_current" : 0,
                "fetch_total" : 0,
                "fetch_time" : "0s",
                "fetch_time_in_millis" : 0,
                "fetch_current" : 0
              }
            }
          }
        }
      }
    }
    --- 23:47:56.314 - show elasticsearch index status ----------------------------------------
    {
      "ok" : true,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "failed" : 0
      },
      "indices" : {
        "diag-no-compression" : {
          "index" : {
            "primary_size" : "77.2mb",
            "primary_size_in_bytes" : 81009144,
            "size" : "77.2mb",
            "size_in_bytes" : 81009144
          },
          "translog" : {
            "operations" : 0
          },
          "docs" : {
            "num_docs" : 100000,
            "max_doc" : 100000,
            "deleted_docs" : 0
          },
          "merges" : {
            "current" : 0,
            "current_docs" : 0,
            "current_size" : "0b",
            "current_size_in_bytes" : 0,
            "total" : 0,
            "total_time" : "0s",
            "total_time_in_millis" : 0,
            "total_docs" : 0,
            "total_size" : "0b",
            "total_size_in_bytes" : 0
          },
          "refresh" : {
            "total" : 1,
            "total_time" : "0s",
            "total_time_in_millis" : 0
          },
          "flush" : {
            "total" : 3,
            "total_time" : "1.8s",
            "total_time_in_millis" : 1879
          },
          "shards" : {
            "0" : [ {
              "routing" : {
                "state" : "STARTED",
                "primary" : true,
                "node" : "pqp3jV5fT7Gc2tULGxLoYg",
                "relocating_node" : null,
                "shard" : 0,
                "index" : "diag-no-compression"
              },
              "state" : "STARTED",
              "index" : {
                "size" : "77.2mb",
                "size_in_bytes" : 81009144
              },
              "translog" : {
                "id" : 1363906059093,
                "operations" : 0
              },
              "docs" : {
                "num_docs" : 100000,
                "max_doc" : 100000,
                "deleted_docs" : 0
              },
              "merges" : {
                "current" : 0,
                "current_docs" : 0,
                "current_size" : "0b",
                "current_size_in_bytes" : 0,
                "total" : 0,
                "total_time" : "0s",
                "total_time_in_millis" : 0,
                "total_docs" : 0,
                "total_size" : "0b",
                "total_size_in_bytes" : 0
              },
              "refresh" : {
                "total" : 1,
                "total_time" : "0s",
                "total_time_in_millis" : 0
              },
              "flush" : {
                "total" : 3,
                "total_time" : "1.8s",
                "total_time_in_millis" : 1879
              }
            } ]
          }
        }
      }
    }
    --- 23:47:56.335 - show elasticsearch index settings --------------------------------------
    {
      "diag-no-compression" : {
        "settings" : {
          "index.query.default_field" : "@message",
          "index.refresh_interval" : "30s",
          "index.number_of_replicas" : "0",
          "index.number_of_shards" : "1",
          "index.store.compress.stored" : "false",
          "index.cache.field.type" : "soft",
          "index.version.created" : "200599"
        }
      }
    }
    --- 23:47:56.422 - show elasticsearch index mappings --------------------------------------
    {
      "diag-no-compression" : {
        "events" : {
          "properties" : {
            "@customer" : {
              "type" : "string",
              "index" : "not_analyzed",
              "omit_norms" : true,
              "index_options" : "docs"
            },
            "@fields" : {
              "dynamic" : "true",
              "properties" : {
                "facility" : {
                  "type" : "integer"
                },
                "facility_label" : {
                  "type" : "string",
                  "index" : "not_analyzed",
                  "omit_norms" : true,
                  "index_options" : "docs"
                },
                "priority" : {
                  "type" : "integer"
                },
                "processid" : {
                  "type" : "string",
                  "index" : "not_analyzed",
                  "omit_norms" : true,
                  "index_options" : "docs"
                },
                "program" : {
                  "type" : "string",
                  "index" : "not_analyzed",
                  "omit_norms" : true,
                  "index_options" : "docs"
                },
                "severity" : {
                  "type" : "integer"
                },
                "severity_label" : {
                  "type" : "string",
                  "index" : "not_analyzed",
                  "omit_norms" : true,
                  "index_options" : "docs"
                }
              }
            },
            "@message" : {
              "type" : "string"
            },
            "@source" : {
              "type" : "string",
              "index" : "not_analyzed",
              "omit_norms" : true,
              "index_options" : "docs"
            },
            "@source_host" : {
              "type" : "string",
              "index" : "not_analyzed",
              "omit_norms" : true,
              "index_options" : "docs"
            },
            "@timestamp" : {
              "type" : "date",
              "format" : "dateOptionalTime"
            },
            "@type" : {
              "type" : "string",
              "index" : "not_analyzed",
              "omit_norms" : true,
              "index_options" : "docs"
            }
          }
        }
      }
    }
    --- 23:47:56.451 - shutdown rsyslogd when main queue is empty -----------------------------
    --- 23:47:56.474 - wait for main message queue to be empty --------------------------------
    imdiag[13500]: mainqueue empty
    --- 23:47:57.243 - wait for rsyslogd shutdown ---------------------------------------------
    --- 23:47:57.362 - exit -------------------------------------------------------------------
    ===========================================================================================
    PASS:  (without compression)
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
    Total tcpflood runtime (milli seconds):                 10182
    Loging rate (MPS):                                      9821
    Transmission speed (MBit/s):
    Compressed data size (MByte):
    Compressed data indexing runtime (milli seconds):
    Compression method:                                     index.store.compress.stored = false
    Compression ratio (%):
    Decompression runtime (milli seconds):
    Uncompressed data size (MByte):                         77.25
    Uncompressed data indexing runtime (milli seconds):     12113
    ===========================================================================================
    --- with compression
    ===========================================================================================
    --- 23:47:57.397 - init -------------------------------------------------------------------
    --- 23:47:57.439 - delete elasticsearch diag index ----------------------------------------
    {"ok":true,"acknowledged":true}
    --- 23:47:57.486 - start rsyslogd ---------------------------------------------------------
    # author: Joerg Heinemann
    # website: http://enterprise-log-management-appliance.org
    # license: GPL v3
    #
    # /opt/elma/config/rsyslog.d/elma_imptcp_elasticsearch.conf
    #
    # simple async writing test to elasticsearch
    # Joerg Heinemann, 2012-12-31
    $IncludeConfig /opt/elma/config/rsyslog.d/diag-common.conf
    $MaxMessageSize 10k
    $ModLoad imptcp
    $MainMsgQueueTimeoutShutdown 10000
    $InputPTCPServerRun 13514
    # ######### Enable In-Memory queues ##########
    #
    # An in-memory queue is created for this action. If the remote host or
    # database is down, messages are spooled to memory and sent when it is
    # up again.
    #
    # The advantages are that it will be consistently good in terms of performance
    # and it will fail fast.
    #
    # The downside is that you can keep a limited number of messages in that queue,
    # less than you normally can with a disk-assisted queue.
    #
    # allocate memory dynamically for the queue. Better for handling spikes
    $MainMsgQueueType LinkedList
    # how many messages (messages, not bytes!) to hold in memory
    $MainMsgQueueSize 2000000
    # don't throttle receiving messages when the queue gets full
    $MainMsgQueueTimeoutEnqueue 0
    # ######### Event forwarding to ElasticSearch ##########
    #
    # Enable ElasticSearch output
    #
    $ModLoad omelasticsearch.so
    # Template for JSON output
    #
    #$template ls_json,"{\"@timestamp\":\"%timereported:1:19:date-rfc3339%.%timereported:1:3:date-subseconds%\",\"@source_host\":%source:::jsonf%\",\"@source\":\"syslog://%fromhost-ip:::json%\",\"@message\":\"%timereported% %app-name%:%msg:::json%\",\"@fields\":{%syslogfacility:facility%,%syslogfacility-text:facility_label%,%syslogseverity:severity%,%syslogseverity-text:severity_label%,%pri:priority%,%app-name:program%,%procid:processid%},\"@customer\":\"AIDB\"}"
    $template ls_json,"{%timestamp:::date-rfc3339,jsonf:@timestamp%,%source:::jsonf:@source_host%,\"@source\":\"syslog://%fromhost-ip:::json%\",\"@message\":\"%timestamp% %app-name%:%msg:::json%\",\"@fields\":{%syslogfacility:::jsonf:facility%,%syslogfacility-text:::jsonf:facility_label%,%syslogseverity:::jsonf:severity%,%syslogseverity-text:::jsonf:severity_label%,%pri:::jsonf:priority%,%app-name:::jsonf:program%,%procid:::jsonf:processid%},\"@customer\":\"AIDB\",\"@type\":\"events\"}"
    $template srchidx,"diag"
    # now we put everything together
    # "template" is for storing the syslog fields we want
    #
    local4.* action(type="omelasticsearch"
                    template="ls_json"
                    searchIndex="srchidx"
                    dynSearchIndex="on"
                    asyncrepl="on"
                    searchType="events"
                    server="localhost"
                    serverport="9200"
    #                uid="elma"
    #                pwd="fjg892345sdfsdwf43d"
                    bulkmode="on"
                    queue.dequeuebatchsize="65536"
                    queue.type="linkedlist"
                    queue.filename="dbq"
                    queue.highwatermark="500000"
                    queue.lowwatermark="400000"
                    queue.discardmark="2000000"
                    queue.timeoutenqueue="0"
                    queue.maxdiskspace="5g"
                    queue.size="2000000"
                    queue.saveonshutdown="on"
                    action.resumeretrycount="-1")
    # There are some other nice things you can use:
    #
    #    searchIndex               - for letting rsyslog know where to get these names
    #    dynSearchIndex="on"       - for having variable index names
    #    searchType="mycustomtype" - to specify a different type than "events".
    #                                You can have dynSearchType="on" to have it variable, like you can with indices.
    #    serverport="9200"         - the default setting, but you can specify a different port
    #    asyncrepl="on"            - to enable asyncronous replication.
    #                                That is, Elasticsearch gives an answer imediately
    #                                after inserting to the main shard(s).
    #                                It doesn't wait for replicas to be updated as well, which is the default setting
    #    timeout="1m"              - how long to wait for a reply from Elasticsearch.
    #                                More info here, near the end:
    #                                http://www.elasticsearch.org/guide/reference/api/index_.html
    # /opt/elma/config/rsyslog.d/diag-common.conf
    #
    # This is a config include file. It sets up rsyslog so that the
    # diag system can successfully be used. Also, it generates a file
    # "rsyslogd.started" after rsyslogd is initialized. This config file
    # should be included in all tests that intend to use common code for
    # controlling the daemon.
    # NOTE: we assume that rsyslogd's current working directory is
    # ./tests (or the distcheck equivalent), in particlular that this
    # config file resides in the testsuites subdirectory.
    # rgerhards, 2009-05-27
    $ModLoad imdiag
    $IMDiagServerRun 13500
    $template startupfile,"rsyslogd.started" # trick to use relative path names!
    :syslogtag, contains, "rsyslogd"  ?startupfile
    # I have disabled the directive below, so that we see errors in testcase
    # creation. I am not sure why it was present in the first place, so for
    # now I just leave it commented out -- rgerhards, 2011-03-30
    #$ErrorMessagesToStderr off
    --- 23:47:57.608 - wait for rsyslogd startup ----------------------------------------------
    rsyslogd: error starting up disk queue, using pure in-memory mode [try http://www.rsyslog.com/e/2036 ]
    rsyslogd started with pid  16053
    --- 23:47:57.727 - run tcpflood -----------------------------------------------------------
    00005 open connections
    starting run 1
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.186
    sleeping 1 seconds before next run
    starting run 2
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.088
    sleeping 1 seconds before next run
    starting run 3
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.162
    sleeping 1 seconds before next run
    starting run 4
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.084
    sleeping 1 seconds before next run
    starting run 5
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.131
    sleeping 1 seconds before next run
    starting run 6
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.079
    sleeping 1 seconds before next run
    starting run 7
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.093
    sleeping 1 seconds before next run
    starting run 8
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.123
    sleeping 1 seconds before next run
    starting run 9
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.130
    sleeping 1 seconds before next run
    starting run 10
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.101
    Runs:     10
    Runtime:
      total:  1.177
      avg:    0.117
      min:    0.079
      max:    0.186
    All times are wallclock time.
    00005 close connections
    End of tcpflood Run
    Raw message lenght (Byte):                      230
    Messages sent during one tcpflood test:         10000
    Number of tcpflood tests:                       10
    Seconds to sleep between tcpflood runs:         1
    Concurrent tcpflood connections:                5
    tcpflood transport protocol:                    tcp
    tcpflood rsyslog target port:                   13514
    tcpflood rsyslog target address:                127.0.0.1
    Total messages:                                 100000
    Total tcpflood runtime (milli seconds):         10190
    Loging rate (MPS):                              9813
    --- 23:48:07.929 - count elasticsearch indexed messages -----------------------------------
    40177 messages indexed (8000 milliseconds - Abort in 120000 milliseconds)
    81311 messages indexed (9000 milliseconds - Abort in 180000 milliseconds)
    81311 messages indexed (10000 milliseconds - Abort in 180000 milliseconds)
    81311 messages indexed (11000 milliseconds - Abort in 179000 milliseconds)
    81311 messages indexed (12000 milliseconds - Abort in 178000 milliseconds)
    81311 messages indexed (13000 milliseconds - Abort in 177000 milliseconds)
    100000 messages indexed (14000 milliseconds - Abort in 176000 milliseconds)
    --- 23:48:15.065 - show elasticsearch index statistics ------------------------------------
    {
      "ok" : true,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "failed" : 0
      },
      "_all" : {
        "primaries" : {
          "docs" : {
            "count" : 100000,
            "deleted" : 0
          },
          "store" : {
            "size" : "23.4mb",
            "size_in_bytes" : 24585756,
            "throttle_time" : "0s",
            "throttle_time_in_millis" : 0
          },
          "indexing" : {
            "index_total" : 100000,
            "index_time" : "11.3s",
            "index_time_in_millis" : 11353,
            "index_current" : 0,
            "delete_total" : 0,
            "delete_time" : "0s",
            "delete_time_in_millis" : 0,
            "delete_current" : 0
          },
          "get" : {
            "total" : 0,
            "time" : "0s",
            "time_in_millis" : 0,
            "exists_total" : 0,
            "exists_time" : "0s",
            "exists_time_in_millis" : 0,
            "missing_total" : 0,
            "missing_time" : "0s",
            "missing_time_in_millis" : 0,
            "current" : 0
          },
          "search" : {
            "query_total" : 0,
            "query_time" : "0s",
            "query_time_in_millis" : 0,
            "query_current" : 0,
            "fetch_total" : 0,
            "fetch_time" : "0s",
            "fetch_time_in_millis" : 0,
            "fetch_current" : 0
          }
        },
        "total" : {
          "docs" : {
            "count" : 100000,
            "deleted" : 0
          },
          "store" : {
            "size" : "23.4mb",
            "size_in_bytes" : 24585756,
            "throttle_time" : "0s",
            "throttle_time_in_millis" : 0
          },
          "indexing" : {
            "index_total" : 100000,
            "index_time" : "11.3s",
            "index_time_in_millis" : 11353,
            "index_current" : 0,
            "delete_total" : 0,
            "delete_time" : "0s",
            "delete_time_in_millis" : 0,
            "delete_current" : 0
          },
          "get" : {
            "total" : 0,
            "time" : "0s",
            "time_in_millis" : 0,
            "exists_total" : 0,
            "exists_time" : "0s",
            "exists_time_in_millis" : 0,
            "missing_total" : 0,
            "missing_time" : "0s",
            "missing_time_in_millis" : 0,
            "current" : 0
          },
          "search" : {
            "query_total" : 0,
            "query_time" : "0s",
            "query_time_in_millis" : 0,
            "query_current" : 0,
            "fetch_total" : 0,
            "fetch_time" : "0s",
            "fetch_time_in_millis" : 0,
            "fetch_current" : 0
          }
        },
        "indices" : {
          "diag" : {
            "primaries" : {
              "docs" : {
                "count" : 100000,
                "deleted" : 0
              },
              "store" : {
                "size" : "23.4mb",
                "size_in_bytes" : 24585756,
                "throttle_time" : "0s",
                "throttle_time_in_millis" : 0
              },
              "indexing" : {
                "index_total" : 100000,
                "index_time" : "11.3s",
                "index_time_in_millis" : 11353,
                "index_current" : 0,
                "delete_total" : 0,
                "delete_time" : "0s",
                "delete_time_in_millis" : 0,
                "delete_current" : 0
              },
              "get" : {
                "total" : 0,
                "time" : "0s",
                "time_in_millis" : 0,
                "exists_total" : 0,
                "exists_time" : "0s",
                "exists_time_in_millis" : 0,
                "missing_total" : 0,
                "missing_time" : "0s",
                "missing_time_in_millis" : 0,
                "current" : 0
              },
              "search" : {
                "query_total" : 0,
                "query_time" : "0s",
                "query_time_in_millis" : 0,
                "query_current" : 0,
                "fetch_total" : 0,
                "fetch_time" : "0s",
                "fetch_time_in_millis" : 0,
                "fetch_current" : 0
              }
            },
            "total" : {
              "docs" : {
                "count" : 100000,
                "deleted" : 0
              },
              "store" : {
                "size" : "23.4mb",
                "size_in_bytes" : 24585756,
                "throttle_time" : "0s",
                "throttle_time_in_millis" : 0
              },
              "indexing" : {
                "index_total" : 100000,
                "index_time" : "11.3s",
                "index_time_in_millis" : 11353,
                "index_current" : 0,
                "delete_total" : 0,
                "delete_time" : "0s",
                "delete_time_in_millis" : 0,
                "delete_current" : 0
              },
              "get" : {
                "total" : 0,
                "time" : "0s",
                "time_in_millis" : 0,
                "exists_total" : 0,
                "exists_time" : "0s",
                "exists_time_in_millis" : 0,
                "missing_total" : 0,
                "missing_time" : "0s",
                "missing_time_in_millis" : 0,
                "current" : 0
              },
              "search" : {
                "query_total" : 0,
                "query_time" : "0s",
                "query_time_in_millis" : 0,
                "query_current" : 0,
                "fetch_total" : 0,
                "fetch_time" : "0s",
                "fetch_time_in_millis" : 0,
                "fetch_current" : 0
              }
            }
          }
        }
      }
    }
    --- 23:48:15.104 - show elasticsearch index status ----------------------------------------
    {
      "ok" : true,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "failed" : 0
      },
      "indices" : {
        "diag" : {
          "index" : {
            "primary_size" : "23.4mb",
            "primary_size_in_bytes" : 24585756,
            "size" : "23.4mb",
            "size_in_bytes" : 24585756
          },
          "translog" : {
            "operations" : 0
          },
          "docs" : {
            "num_docs" : 100000,
            "max_doc" : 100000,
            "deleted_docs" : 0
          },
          "merges" : {
            "current" : 0,
            "current_docs" : 0,
            "current_size" : "0b",
            "current_size_in_bytes" : 0,
            "total" : 0,
            "total_time" : "0s",
            "total_time_in_millis" : 0,
            "total_docs" : 0,
            "total_size" : "0b",
            "total_size_in_bytes" : 0
          },
          "refresh" : {
            "total" : 1,
            "total_time" : "0s",
            "total_time_in_millis" : 0
          },
          "flush" : {
            "total" : 3,
            "total_time" : "1.6s",
            "total_time_in_millis" : 1656
          },
          "shards" : {
            "0" : [ {
              "routing" : {
                "state" : "STARTED",
                "primary" : true,
                "node" : "pqp3jV5fT7Gc2tULGxLoYg",
                "relocating_node" : null,
                "shard" : 0,
                "index" : "diag"
              },
              "state" : "STARTED",
              "index" : {
                "size" : "23.4mb",
                "size_in_bytes" : 24585756
              },
              "translog" : {
                "id" : 1363906077910,
                "operations" : 0
              },
              "docs" : {
                "num_docs" : 100000,
                "max_doc" : 100000,
                "deleted_docs" : 0
              },
              "merges" : {
                "current" : 0,
                "current_docs" : 0,
                "current_size" : "0b",
                "current_size_in_bytes" : 0,
                "total" : 0,
                "total_time" : "0s",
                "total_time_in_millis" : 0,
                "total_docs" : 0,
                "total_size" : "0b",
                "total_size_in_bytes" : 0
              },
              "refresh" : {
                "total" : 1,
                "total_time" : "0s",
                "total_time_in_millis" : 0
              },
              "flush" : {
                "total" : 3,
                "total_time" : "1.6s",
                "total_time_in_millis" : 1656
              }
            } ]
          }
        }
      }
    }
    --- 23:48:15.126 - show elasticsearch index settings --------------------------------------
    {
      "diag" : {
        "settings" : {
          "index.query.default_field" : "@message",
          "index.refresh_interval" : "30s",
          "index.number_of_replicas" : "0",
          "index.number_of_shards" : "1",
          "index.store.compress.stored" : "true",
          "index.cache.field.type" : "soft",
          "index.version.created" : "200599"
        }
      }
    }
    --- 23:48:15.217 - show elasticsearch index mappings --------------------------------------
    {
      "diag" : {
        "events" : {
          "properties" : {
            "@customer" : {
              "type" : "string",
              "index" : "not_analyzed",
              "omit_norms" : true,
              "index_options" : "docs"
            },
            "@fields" : {
              "dynamic" : "true",
              "properties" : {
                "facility" : {
                  "type" : "integer"
                },
                "facility_label" : {
                  "type" : "string",
                  "index" : "not_analyzed",
                  "omit_norms" : true,
                  "index_options" : "docs"
                },
                "priority" : {
                  "type" : "integer"
                },
                "processid" : {
                  "type" : "string",
                  "index" : "not_analyzed",
                  "omit_norms" : true,
                  "index_options" : "docs"
                },
                "program" : {
                  "type" : "string",
                  "index" : "not_analyzed",
                  "omit_norms" : true,
                  "index_options" : "docs"
                },
                "severity" : {
                  "type" : "integer"
                },
                "severity_label" : {
                  "type" : "string",
                  "index" : "not_analyzed",
                  "omit_norms" : true,
                  "index_options" : "docs"
                }
              }
            },
            "@message" : {
              "type" : "string"
            },
            "@source" : {
              "type" : "string",
              "index" : "not_analyzed",
              "omit_norms" : true,
              "index_options" : "docs"
            },
            "@source_host" : {
              "type" : "string",
              "index" : "not_analyzed",
              "omit_norms" : true,
              "index_options" : "docs"
            },
            "@timestamp" : {
              "type" : "date",
              "format" : "dateOptionalTime"
            },
            "@type" : {
              "type" : "string",
              "index" : "not_analyzed",
              "omit_norms" : true,
              "index_options" : "docs"
            }
          }
        }
      }
    }
    --- 23:48:15.240 - shutdown rsyslogd when main queue is empty -----------------------------
    --- 23:48:15.254 - wait for main message queue to be empty --------------------------------
    imdiag[13500]: mainqueue empty
    --- 23:48:16.044 - wait for rsyslogd shutdown ---------------------------------------------
    --- 23:48:16.059 - exit -------------------------------------------------------------------
    ===========================================================================================
    PASS:   (with compression)
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
