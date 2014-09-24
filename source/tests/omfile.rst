================================
 rsyslog omfile bechmark report
================================

::

    /usr/share/elma-tests/bin/elma_imptcp_gzipwr.sh
    ===========================================================================================
    Scriptname : elma_imptcp_gzipwr.sh
    -------------------------------------------------------------------------------------------
    Description: rsyslogd performance test with gzip output compression
    Author     : Joerg Heinemann
    Website    : http://enterprise-log-management-appliance.org
    ===========================================================================================
    --- 23:34:54.580 - init -------------------------------------------------------------------
    --- 23:34:54.647 - start rsyslogd ---------------------------------------------------------
    # author: Joerg Heinemann
    # website: http://code.google.com/p/enterprise-log-management-appliance/
    # license: GPL v3
    #
    # /opt/elma/config/rsyslog.d/elma_imptcp_gzipwr.conf
    #
    # simple async writing test
    # rgerhards, 2010-03-09
    $IncludeConfig /opt/elma/config/rsyslog.d/diag-common.conf
    $MaxMessageSize 10k
    $ModLoad imptcp
    $MainMsgQueueTimeoutShutdown 10000
    $InputPTCPServerRun 13514
    $template outfmt,"%msg:F,58:2%,%msg:F,58:3%,%msg:F,58:4%\n"
    $template dynfile,"rsyslog.out.log" # trick to use relative path names!
    $OMFileFlushOnTXEnd off
    $OMFileZipLevel 9
    $OMFileIOBufferSize 256k
    local4.* ?dynfile;outfmt
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
    --- 23:34:54.754 - wait for rsyslogd startup ----------------------------------------------
    rsyslogd started with pid  15678
    --- 23:34:54.977 - run tcpflood -----------------------------------------------------------
    00005 open connections
    starting run 1
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.076
    sleeping 1 seconds before next run
    starting run 2
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.123
    sleeping 1 seconds before next run
    starting run 3
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.063
    sleeping 1 seconds before next run
    starting run 4
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.116
    sleeping 1 seconds before next run
    starting run 5
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.097
    sleeping 1 seconds before next run
    starting run 6
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.168
    sleeping 1 seconds before next run
    starting run 7
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.030
    sleeping 1 seconds before next run
    starting run 8
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.079
    sleeping 1 seconds before next run
    starting run 9
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.093
    sleeping 1 seconds before next run
    starting run 10
    Sending 10000 messages.
    00010000 messages sent
    runtime: 0.061
    Runs:     10
    Runtime:
      total:  0.906
      avg:    0.090
      min:    0.030
      max:    0.168
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
    Total tcpflood runtime (milli seconds):         10141
    Loging rate (MPS):                              9860
    --- 23:35:05.131 - time for the rsyslogd tcp receiver to settle ---------------------------
    --- 23:35:07.156 - shutdown rsyslogd when main queue is empty -----------------------------
    --- 23:35:07.177 - wait for main message queue to be empty --------------------------------
    imdiag[13500]: mainqueue empty
    --- 23:35:07.955 - wait for rsyslogd shutdown ---------------------------------------------
    --- 23:35:08.069 - sequence check gzip to see if everything was properly received ---------
    -rw-r--r-- 1 root root 313669 Mar 21 23:35 rsyslog.out.log
    -rw-r--r-- 1 root root 24400000 Mar 21 23:35 work
    00000000,230,XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    chkseq: start 0, end 99999
    --- 23:35:08.907 - exit -------------------------------------------------------------------
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
