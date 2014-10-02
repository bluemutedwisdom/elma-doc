=================================
 Patch, Update and Upgrade Guide
=================================

All openSUSE and ELMA related RPM packages are provided by the `openSUSE
Build Service <https://build.opensuse.org/>`__ a public instance of the
Open Build Service (OBS) used for development of the openSUSE and ELMA
distribution:

-  `ELMA main
   repository <https://build.opensuse.org/project/show/security:logging:elma>`__
-  `ELMA devel
   repository <https://build.opensuse.org/project/show/security:logging:elma:devel>`__
-  `Current releases <../changelog.html>`__

This HOWTO explains the steps for patching, updating and upgrading ELMA
and openSUSE distribution.

It's really simple - just run:

::

    /usr/share/elma/bin/elma-reconfigure.sh --patch --update

Requirements and recommendations
================================

For all patch, update and upgrade procedures outbound internet access to
official SUSE repositories

-  download.opensuse.org

is mandatory. Regarding to security reasons the use of your corporate
internet proxy infrastructure is highly recommended.

In case of connectivity problems please check ELMA

-  Network settings
-  DNS settings
-  Proxy settings

For all **patching und upgrading** tasks the usage of ELMA **CLI is more
secure** then the usage of ELMA webgui because of possible apache and
php reloads during patches and upgrades of underlying operation system.

After every install, upgrade or update process please

-  Read carefully and follow all warnings and recommendations
-  Consider to reboot the system after major patches, updates or
   upgrades

Patches
=======

-  Run the patch script

   ::

     /usr/share/elma/bin/elma-reconfigure.sh --patch

-  Interactive patch from CLI (optional)

   ::

     zypper --no-gpg-checks --gpg-auto-import-keys patch

-  Check the differences between your and distribution config files

Updates
=======

-  Run the update script

   ::

     /usr/share/elma/bin/elma-reconfigure.sh --update

-  Interactive patch from CLI (optional)

   ::

     zypper --no-gpg-checks --gpg-auto-import-keys update

-  Check the differences between your and distribution config files

Upgrades
========

-  Upgrade howtos can be found on `openSUSE support
   site <http://en.opensuse.org/SDB:System_upgrade>`__.

Reconfigure ELMA
================

-  Reconfigure ELMA after every configuration change, patch, update or
   upgrade process

   ::

     /usr/share/elma/bin/elma-reconfigure.sh

Final Tests
===========

-  Test rsyslog and ElasticSearch installation

   ::

     cd /usr/share/elma-test/bin; /usr/share/elma-test/bin/elma_diag_all_after_update.sh

-  Check if all `performance and compression tests <../tests>`__ are
   successful
-  Test ElasticSearch admin frontend interfaces (Use the static IP or
   FQDN you have configured during first boot)

   ::

     https://192.168.178.25:9443/_plugin/head/
     https://192.168.178.25:9443/_plugin/paramedic/
     https://192.168.178.25:9443/_plugin/bigdesk/

-  Test ELMA user frontend interfaces (Use the static IP or FQDN you
   have configured during first boot)

   ::

     https://192.168.178.25

-  If you can see actual events in the web GUI everthing is working
   fine.

Reset ELMA configuration parameter to factory defaults
======================================================

-  Run the reset script

   ::

     /usr/share/elma/bin/elma-reconfigure.sh --reset
