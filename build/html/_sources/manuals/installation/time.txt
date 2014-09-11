============================================
 Hardware (CMOS/BIOS) Clock and System Time
============================================

Requirements and recommendations
================================

rsyslog and Kibana **time stamped indices** use your selected time range
to create a list of indices **that match a specified timestamp
pattern**. Please also note that **rsyslog indices should rollover at
midnight UTC**.

Therefore it is strongly recommend that the `Hardware (CMOS/BIOS) clock,
system and local time is set to
UTC/GMT <https://en.opensuse.org/SDB:Configuring_the_clock>`__ before
first boot!

Coordinated Universal Time
==========================

UTC (Coordinated Universal Time) is the international time standard. It
is the current term for what was commonly referred to as Greenwich
Meridian Time (GMT). Zero hours UTC is midnight in Greenwich, England,
which lies on the zero longitudinal meridian. Coordinated Universal Time
is based on a 24 hour clock; therefore, afternoon hours such as 4 pm UTC
are expressed as 16:00 UTC.

System time
===========

It is strongly recommend that the **system clock (as well as the
CMOS/BIOS clock) is set to UTC/GMT**.

System time and date:

::

    timedatectl status

This way Linux can keep the user space clock on the correct time when
the change for Daylight Saving Time occurs. The safest way is to **set
the CMOS clock to UTC before beginning the installation process**.

-  `Configure hardware and system clock <https://en.opensuse.org/SDB:Configuring_the_clock>`__
-  `Clock and time adjusting <http://susefaq.sourceforge.net/howto/time.html>`__

Reading the clock
=================

Reading the clock:

::

    hwclock --show ; date ; date -u

The commands above show, in succession:

-  **CMOS time** (should be **UTC**)
-  **Local system time** (should be **UTC**)
-  **UTC system time**

Is your system time synchronized and up to date?
================================================

One of the quickest commands to verify that ntpd is still up and running
as desired is:

::

    ntpq -p

To compare time offsets between local system time and time taken from
your ntp server:

::

    ntpdate -d server

To synchronize your system time manually:

::

    systemctl stop ntp.service
    ntpdate server
    systemctl start ntp.service
