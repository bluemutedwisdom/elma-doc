=============
 NoMachineNX
=============

Installation on openSUSE
========================

Installation of `NoMachine NX <http://www.nomachine.com>`__ 3.5 on
`openSUSE <http://www.opensuse.org>`__ 12.3.

-  Log in as superuser ("root")
-  `Download the
    RPMs <http://www.nomachine.com/download-package.php?Prod_Id=3772>`__
-  Change your working directory to the location where you saved the
    package and install it by running from a console:

    ::

      rpm -i nxclient-3.5.0-7.x86_64.rpm
      rpm -i nxnode-3.5.0-9.x86_64.rpm
      rpm -i nxserver-3.5.0-11.x86_64.rpm

-  Edit the SSH server configuration file:

   ::

     vi /etc/ssh/sshd_config

-  Look for the line that starts with AuthorizedKeysFile. Edit the line to be:

   ::

     AuthorizedKeysFile .ssh/authorized_keys2

   Note the "2" at the end.

-  Save it.
-  Find the ID of the /usr/sbin/sshd process.

   ::

     ps -ef | grep sshd

-  Send it a signal to re-read this config file:

   ::

     kill -HUP processID

-  Start the nxserver.

   ::

     /usr/NX/bin/nxserver --start

The NX service can be controlled by the command /usr/NX/bin/nxserver
--status\|--start\|--stop\|--restart. Additional commands are available
to configure the server. Try /usr/NX/bin/nxserver --help for more
information.
