============================
 Frequently Asked Questions
============================

**Some things are simply often asked. Here we provide the answers!**

If you have questions that should be explained here, please let us know.

Issues, problems, enhancements and feature requests are managed with our
`project
workflow <http://code.google.com/p/enterprise-log-management-appliance/issues/list>`__.
For extended communication visit `ELMA discussion
group <https://groups.google.com/forum/?fromgroups#!forum/enterprise-log-management-appliance>`__.

For syslog specific questions please also have a look at
`rsyslog <http://www.rsyslog.com/faq/>`__ and
`syslog-ng <http://www.balabit.com/wiki/syslog-ng-faq-table-contents>`__
FAQs. Further informations and tips arround log management you can find
in `ElasticSearch
tutorials <http://www.elasticsearch.org/tutorials/>`__.


Which web browsers are supported?
=================================

ELMA user and admin interfaces should work in all modern web
browsers as long as they support SVG.

-  Tested with **Firefox > 19.x**.
-  Tested with Google Chrome > 26
-  **IE8 & IE9 don't work correctly!**

You may need to **clear your browsers cache after upgrading** to see
all of the new features.

If you are **running into ssl browser errors** you may need to
**delete your ELMA certificate inside your browsers server
certificate store**.

File System Sizing immediately after deployment
===============================================

A busy system will blow up `ElasticSearch or rsyslog working
space <../manuals/requirements.html>`__ with data relatively quickly.

It is strongly recommended to separate or to extend these LVM
Volumes with a sizing depending on your specific requirements using
integrated Logical Volume Manager (LVM) inside ELMA CLI.

A good procedure for File System Sizing immediately after deployment:

-  Stop services

  ::

    service rsyslog stop
    service elasticsearch stop

-  Rename ElasticSearch data directory from /var/lib/elasticsearch to /var/lib/elasticsearch-old

  ::

    mv /var/lib/elasticsearch /var/lib/elasticsearch-old

-  SUSE does not auto mount non-root LVM volume groups on boot, so be
   sure to run

  ::

    chkconfig boot.lvm on

-  Add another virtual disk, reasonably sized (I make it 100GB to
   start). Create a new volume group, and a new logical volume (using
   file system type EXT3 or better EXT4) and mount it at
   /var/lib/elasticsearch

-  Move the files from /var/lib/elasticsearch-old to the new
   /var/lib/elasticsearch

  ::

    cp -Rp /var/lib/elasticsearch-old/* /var/lib/elasticsearch

-  Set the owners and groupname back, just in case

  ::

    chown -R elasticsearch:root /var/lib/elasticsearch
    chgrp -R elasticsearch:root /var/lib/elasticsearch

-  Check path to ElasticSearch data directory

  ::

    grep "ES_DATA_DIR=" /etc/elma/elma.conf
    ES_DATA_DIR="/var/lib/elasticsearch"

-  System reboot

  ::

    reboot

Increase disk space using the YAST partitioner
==============================================

-  Using the VMware vSphere Client (or similar tool), increase the
   virtual disk to the size you want it to be.
-  Reboot ELMA to detect the new disk size
-  From the command prompt of ELMA, run "yast".
-  Select "System" --> "Partitioner"
-  Click "Yes" to continue after viewing the warning.
-  Verify that the "/dev/sda/" virtual disk is the new larger size.
-  Select "Hard Disks" --> "Add Partition" --> "Primary Partition" -->
   "Maximum Size"
-  Select "Do not format partition" --> File system ID = "0x8e Linux
   LVM"
-  Select "Volume Management" --> Resize "/dev/systemVG" --> Select the
   newly create partition and select "Add"
-  Resize each volume to your desired size. The volumes that will use
   the most storage are LVRoot (ElasticSearch database) and "LVvar\_log"
   (compressed syslog archive).

Final changes should look similar to this:

::

    Changes to partitioning:
     *  Create volume /dev/sda3 (786 GB)
     *  Extend volume group systemVG with /dev/sda4
     *  Extend logical volume /dev/systemVG/LVRoot to 759.80.79 GB
     *  Extend logical volume /dev/systemVG/LVopt_ to 10.00 GB
     *  Extend logical volume /dev/systemVG/LVvar_log_ to 29.98 GB
     *  Extend logical volume /dev/systemVG/LVvar_spool_ to 10.00 GB 

How do I limit how much storage space is used?
==============================================

ELMA search frontend get the logging events from ElasticSearch
database which stores the indexed data per default in
/var/lib/elasticsearch.

-  Watch how much your logs grow in a day. For example, if
   /var/lib/elasticsearch grows from 10 GB to 15 GB in a day, this is 5
   GB/day.

  ::

    df -h

    Filesystem                         Size  Used Avail Use% Mounted on
    devtmpfs                           1.9G   32K  1.9G   1% /dev
    tmpfs                              1.9G     0  1.9G   0% /dev/shm
    tmpfs                              1.9G  3.0M  1.9G   1% /run
    /dev/mapper/systemVG-LVRoot         60G   42G   17G  72% /
    tmpfs                              1.9G     0  1.9G   0% /sys/fs/cgroup
    tmpfs                              1.9G  3.0M  1.9G   1% /var/lock
    tmpfs                              1.9G  3.0M  1.9G   1% /var/run
    /dev/sda1                          183M   35M  139M  20% /boot
    /dev/mapper/systemVG-LVvar_log_     30G  3.2G   25G  12% /var/log
    /dev/mapper/systemVG-LVvar_spool_   29G   16G   13G  55% /var/spool

-  Divide your available storage by the daily usage to determine the
   number of days of logs you can keep. For example, if you have 100 GB
   of available storage and you are using 5 GB/day, you can keep 20 days
   of logs.

  ::

    100 / 5 = 20

-  Edit "/etc/elma/elma.conf"

  ::

    ######### Set the number of days ElasticSearch indexes should be hold #########
    ESARCHIVEPERIOD="20"

-  Reconfigure ELMA to use new setting:

  ::

    /usr/share/elma/bin/elma-reconfigure.sh --all

Apache failing to start after patching and upgrading
====================================================

-  For all **patching und upgrading** tasks the usage of ELMA **CLI is
   more secure** then the usage of ELMA webgui because of possible
   apache and php reloads during patches and upgrades of underlying
   operation system

  ::

    /usr/share/elma/bin/elma-reconfigure.sh --patch --update

Warning: Patch 'openSUSE-2014-493-1' is interactive, skipping
=============================================================

-  Interactive update from ELMA CLI

  ::

    zypper --no-gpg-checks --gpg-auto-import-keys patch

-  System reboot

  ::

    reboot

SearchPhaseExecutionException - Failed to execute phase query\_fetch
====================================================================

-  Filesystem is full (see above instructions for partitioning and log
   rotation)
-  rsyslogd is not running (see rsyslog configuration pages)
-  Issues in rsyslog configuration (see rsyslog configuration pages)

ElasticsearchException - java.lang.OutOfMemoryError: Java heap space
====================================================================

-  Extend Elasticsearch heap space (see Elasticsearch configuration
   pages)

Are older openSUSE 12.x appliances being supported in future?
=============================================================

There are to much changes from openSUSE 12.2 to 13.1 especially with
openSUSE startup scripts and logging features. **It's strongly
recommended to upgrade all of your ELMA appliances** (see below).

There will be **no further support for openSUSE 12.x** appliances.

Howto upgrade ELMA appliance to openSUSE 13.1?
==============================================

Upgrade howtos can be found on `openSUSE support
site <http://en.opensuse.org/SDB:System_upgrade>`__.

There will be **no further support for openSUSE 12.x** appliances.
Please upgrade to openSUSE 13.1!

How to forward syslog events from syslog-ng to ELMA rsyslogd?
=============================================================

-  Edit your syslogd.conf file, usually found in
   /opt/syslog-ng/etc/syslogd-ng.conf and make sure you have a source
   called s\_all:

  ::

    source s_all {
      internal();
      unix-stream("/dev/log");
      file("/proc/kmsg" program_override("kernel: "));
    };

-  Next configure the protocol and port, and put them in a destination
   entry, being sure to specify TCP. Replace the "192.168.0.1" with the
   address of your ELMA server.

  ::

    destination d_elma {
      tcp("192.168.0.1" port(514));
    };

-  Next tell syslog-ng to forward syslogs from "s\_all" source to the
   "d\_elma" destination. Note: This log statement must be before any
   "final" log statements, so make this the first log statement.

  ::

    log { 
      source(s_all); destination(d_elma); 
    };

-  Your configuration should now look as follows:

  ::

    source s_all {  
      internal();  
      unix-stream("/dev/log");  
      file("/proc/kmsg" program_override("kernel: "));  
    };
    destination d_elma {
      tcp("192.168.0.1" port(514));
    };  
    log {
      source(s_all); destination(d_elma); 
    };

-  Now, restart syslog-ng:

  ::

    /etc/init.d/syslog-ng restart

-  Test sending events to ELMA by using the command line tool logger:

  ::

    logger "my little pony"

-  Log into the ELMA search interface. Search for your events, starting
   with "my little pony".

How to forward syslog events from rsyslogd to ELMA rsyslogd?
============================================================

-  On sender site:

  ::

    # ######### Event forwarding to ELMA rsyslogd server ##########
    #
    # plain TCP event forwarding
    #
    *.* @@192.168.0.1:514

-  On receiver site: nothing to do

How to enable compressed event forwarding between rsyslogd servers?
===================================================================

-  On sender site:

  ::

    # ######### Compressed event forwarding to ELMA rsyslog server ##########
    #
    # plain TCP event forwarding with experimental framing
    # maximum zlib compression rate (z1 - z9)
    #
    *.* @@(o,z9)192.168.0.1:514

-  On receiver site: nothing to do

How to resolve original syslog sender IP to sender hostname?
============================================================

-  rsyslog @source\_host value is the original sender IP.
-  rsyslog @source value is the sender IP in front of your rsyslog
   server - could be the original sender IP or your syslog forwarding
   server.

By default rsyslogd makes reverse DNS requests to resolve the
hostname and stores it instead of sender IP. The is no further
customization on ELMA appliance necessary.

You have to **make sure that your ELMA box has right local DNS
server settings!**

You could test reverse DNS resolving using dig on ELMA linux shell:

::

    dig @localhost -x 204.152.184.167 +short

Wrong timpstamp in ELMA Search-Gui - Is your system time synchronized and up to date?
=====================================================================================

One of the quickest commands to verify that ntpd is still up and
running as desired is:

::

    ntpq -p

To compare time offsets between local system time and time taken
from your ntp server:

::

    ntpdate -d server

To synchronize your system time manually:

::

    systemctl stop ntp.service
    ntpdate server
    systemctl start ntp.service

No events in ELMA Search-Gui till UTC+XXX - Have you set system clock to UTC?
=============================================================================

Reading the clock:

::

    hwclock --show ; date ; date -u

The commands above show, in succession:

-  **CMOS time** (should be **UTC**)
-  **Local system time** (should be **UTC**)
-  **UTC system time**

rsyslog and Kibana time stamped indices use your selected time range
to create a list of indices that match a specified timestamp
pattern.

Please also note that **rsyslog indices should rollover at midnight
UTC**. Therefore it is strongly recommended to **set system clock to UTC**.

Coordinated Universal Time
==========================

UTC (Coordinated Universal Time) is the international time standard.
It is the current term for what was commonly referred to as
Greenwich Meridian Time (GMT). Zero hours UTC is midnight in
Greenwich, England, which lies on the zero longitudinal meridian.
Coordinated Universal Time is based on a 24 hour clock; therefore,
afternoon hours such as 4 pm UTC are expressed as 16:00 UTC.

System time
===========

It is strongly recommend that the **system clock (as well as the
CMOS/BIOS clock) is set to UTC/GMT**.

System time and date:

::

    timedatectl status

This way Linux can keep the user space clock on the correct time
when the change for Daylight Saving Time occurs. The safest way is
to **set the CMOS clock to UTC before beginning the installation
process**.

-  `Clock and time adjusting <../manuals/installation/time.html>`__

Rsyslog timestamp formatting - Timestamp parsing problems
=========================================================

Rsyslog timestamp formatting:

::

    date-rfc3339 2010-12-05T02:21:41.889482+01:00
    date-rfc3164 Dec 5 02:21:13
    date-pgsql 2010-12-05 02:27:34
    date-mysql 20101205022845

UnresolvedAddressException - Have you setup a static IP address?
================================================================

In some case if you are using DHCP (dynamic or static) there is no
/etc/host entry for your ELMA host. You have to insert your hostname
with correspondig IP address.

Better you change your network configuration to static for server
environment. **Make sure you have also correct DNS resolving
(especially reverse resolving)** for your appliance.
