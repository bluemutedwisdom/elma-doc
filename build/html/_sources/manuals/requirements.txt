==============
 Requirements
==============

Basic Knowledge
===============

ELMA appliance comes out of the box without any Linux graphical user
interface. Therefore it is useful to have some basic knowledge of
working with a Linux shell because some operation and administration
tasks only can be done with shell commands.

The default shell in openSUSE is Bash (GNU Bourne-Again Shell).

For more detailed installation instructions and deployment strategies,
see the `openSUSE documentation <http://doc.opensuse.org/>`__.

System and Hardware Requirements
================================

-  Pentium III 500 MHz or higher processor (Pentium 4 2.4 GHz or higher
   or any AMD64 or Intel EM64T processor recommended)
-  4096 MB physical RAM (8 GB recommended)
-  A minimum of 1 GB Elasticsearch heap space (50% of available system
   RAM for busy systems recommended)
-  It is strongly recommend that the `Hardware (CMOS/BIOS) clock, system
   and local time is set to UTC/GMT <installation/time.html>`__!
-  40 GB available disk space for the base system with following volume
   mount paths:

   ::

     / (8192 MB)
     /srv/ (4096 MB)
     /var/lib/elasticsearch (16384 MB)
     /var/log/ (8192 MB)
     /var/spool/ (4096 MB)

-  Separate or extended LVM Volumes with a sizing depending on your
   specific requirements:

   ::

     /var/lib/elasticsearch (ElasticSearch default working space)
     /var/log/syslog (rsyslog flat file archive)

A busy system will blow up ElasticSearch or rsyslog working space with
data relatively quickly. Assuming an average message lenght of 244 Byte
per message and 10.000 events per second the following projection can
give you an `idea of difficulties for suitable filesystem
sizing <../tests>`__:

+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
|                            | **Benchmark**     | **LVM Volume**           | **second**   | **minute**   | **hour**    | **day**     | **week**   | **month**   | **year**      |
+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Messages**               |                   |                          | 10 k         | 600 k        | 36 M        | 864 M       | 6.048 M    | 181.440 M   | 2.177.280 M   |
+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Compressed data size**   | omfile            | /var/log/syslog          | 0.03 MB      | 1.74 MB      | 104.40 MB   | 2.45 GB     | 17.13 GB   | 68.51 GB    | 822.15 GB     |
+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+
| **Compressed data size**   | omelasticsearch   | /var/lib/elasticsearch   | 3.50 MB      | 210.00 MB    | 12.30 GB    | 295.31 GB   | 2.02 TB    | 8.07 TB     | 96.90 TB      |
+----------------------------+-------------------+--------------------------+--------------+--------------+-------------+-------------+------------+-------------+---------------+

**It is strongly recommended to use seperate hard disks** both for your
data and archive file systems using integrated Logical Volume Manager
(LVM) inside ELMA CLI. Then ELMA appliance is completely independant
from your application data and can easily upgraded.

To clone ELMA appliance and make necessary adaptations inside SUSEstudio
is also a suitable method.

Partition Setup
---------------

The ELMA integrated `Logical Volume Manager
(LVM) <https://activedoc.opensuse.org/book/opensuse-reference/chapter-3-advanced-disk-setup>`__
setup provides in most cases a reasonable partition scheme that can be
accepted without change.

**Logical Volume Manager (LVM) applies to the Disk image, VMware, OVF
and Preload ISO formats only!**

To make changes in the proposal (like changing the file system type or
encrypt partitions), select Edit Partition Setup and adjust the
settings. See `using the YaST
Partitioner <https://activedoc.opensuse.org/book/opensuse-reference/chapter-3-advanced-disk-setup#fig.yast2.i_y2_disk_part>`__
for further instructions.

File System Sizing
------------------

::

    df -h

    Filesystem                         Size  Used Avail Use% Mounted on
    devtmpfs                           2.0G   40K  2.0G   1% /dev
    tmpfs                              2.0G     0  2.0G   0% /dev/shm
    tmpfs                              2.0G  6.6M  2.0G   1% /run
    /dev/mapper/systemVG-LVRoot        7.5G  2.0G  5.2G  28% /
    tmpfs                              2.0G     0  2.0G   0% /sys/fs/cgroup
    tmpfs                              2.0G  6.6M  2.0G   1% /var/run
    tmpfs                              2.0G  6.6M  2.0G   1% /var/lock
    /dev/vda1                          199M   53M  136M  28% /boot
    /dev/mapper/systemVG-LVvar_log_    7.8G  159M  7.2G   3% /var/log
    /dev/mapper/systemVG-LVvar_spool_  3.9G  137M  3.6G   4% /var/spool
    /dev/mapper/systemVG-LVopt_        3.9G  219M  3.5G   6% /opt
    /dev/mapper/dataVG-LVdata           63G  266M   60G   1% /var/lib/elasticsearch
    /dev/mapper/syslogVG-LVsyslog       63G  201M   60G   1% /var/log/syslog
