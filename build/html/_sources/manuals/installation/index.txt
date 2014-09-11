====================
 Installation Guide
====================

.. toctree::
   :maxdepth: 4

   time
   firstboot

All openSUSE and ELMA related RPM packages are provided by the `openSUSE
Build Service <https://build.opensuse.org/>`__ a public instance of the
Open Build Service (OBS) used for development of the openSUSE and ELMA
distribution:

-  `ELMA main
   repository <https://build.opensuse.org/project/show/security:logging:elma>`__
-  `ELMA devel
   repository <https://build.opensuse.org/project/show/security:logging:elma:devel>`__
-  `Current releases <../../changelog.html>`__

This HOWTO explains the initial steps of creating a basic setup of ELMA
and openSUSE distribution.

It's really simple:

-  Download `ELMA
   appliance <http://susestudio.com/a/TOYySW/enterprise-log-management-appliance--4>`__
   (based on openSUSE Studio 13.1 64-bit x86 and `openSuse ELMA
   RPMs <https://build.opensuse.org/project/show/security:logging:elma>`__)

If you are importing into VMware ESX, choose the OVF appliance download.
Logical Volume Manager (LVM) applies to the `Disk image, VMware, OVF and
Preload ISO
formats <http://susestudio.com/help/create/appliance-formats.html>`__
only!

-  You need have at least `36 GB hard drive space <../requirements.html>`__
   and further `LVM partitions for ElasticSearch database and/or rsyslog
   flat file archiv <../requirements.html>`__.
-  Read ELMA `release notes, technical
   details <http://susestudio.com/a/TOYySW/enterprise-log-management-appliance--4>`__
   and `system requirements <../requirements.html>`__
-  Read more informations about `SUSE Studio Onsite
   services <https://www.suse.com/documentation/suse_studio/pdfdoc/suse_studio_user_guide/suse_studio_user_guide.pdf>`__
-  For first boot and login into the appliance following beneath details

ELMA initial installation
=========================

Before first boot it is strongly recommend that the `Hardware
(CMOS/BIOS) clock is set to UTC/GMT <time.html>`__!

-  `Clock and time adjusting <time.html>`__
-  On the `first boot <firstboot.html>`__, you will be asked to
   configure several items:

   ::

     Hard Drive Preparation (Preload ISO image only) 
     Language
     Keyboard Layout
     Hostname and Domain Name
     Network Configuration
     Clock and Time Zone
     Set the ROOT password

-  Login into the appliance (Use the static IP or FQDN you have configured during first boot)

   ::

     ssh 192.168.178.25

   ::

     user: root
     password: (use the password you entered during the wizard)

-  Separate or extended `LVM Volumes <../requirements.html>`__
   with a sizing depending on your specific requirements

   ::

     /var/lib/elasticsearch (ElasticSearch default working space)
     /var/log/syslog (rsyslog flat file archive)

-  Run the reconfigure script for initial configuration

   ::

     /usr/share/elma/bin/elma-reconfigure.sh --all

-  Login into web gui (Use the static IP or FQDN you have configured during first boot)

   ::

     https://192.168.178.25

   ::

     user: admin
     password: biglog

-  Immediately change the admin user password
-  Check or modify ELMA and openSUSE configuration

   ::

     Network settings
     DNS settings
     NTP settings
     Proxy settings
     Apache authentication settings
     ElasticSearch heap space settings
     SuSEfirewall settings

-  Test internet connectivity

Reset ELMA configuration parameter to factory defaults
======================================================

-  Run the reset script

   ::

     /usr/share/elma/bin/elma-reconfigure.sh --reset

Next installation steps
=======================

-  Next steps for installation are the same as for `patches, updates and
   upgrades <../update.html>`__
-  Afterward follow ELMA `configuration <../configuration>`__ guide
-  Read ELMA `user <../user.html>`__, `operation <../operation.html>`__ and
   `administration <../administration.html>`__ guide
