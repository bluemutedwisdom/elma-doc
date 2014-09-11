========
 Nagios
========

Nagios plugins
==============

Once you get used to writing plug-ins in Nagios and the complexity of
the plug-ins you write grows, you may encounter this error, service
check timed out.

If some of your service checks have this problem, you can isolate the
problem in these 3 values:

1. how slow is the plugin

    This is the first thing you should do. Check if how much time does
    your plugin needs before it can finish checking and provide an exit
    status. Log-on to the server you’re monitoring and run the plugin
    locally. Use the time command to measure.

    $ time /usr/lib/nagios/plugins/check\_service

2. how short is NRPE's patience

    Once you have the value (in seconds) in step #1, check your NRPE
    configuration in that same server . The default location of NRPE’s
    configuration is /etc/nagios/nrpe.cfg

    Find this parameter, command\_timeout. The value of this parameter,
    in seconds, must be greater than the value that you’ve got in step
    #1.

    Once the parameters been set, restart the NRPE service (service
    nrpe restart).

3. how short is Nagios patience

    Nagios executes a command, check\_nrpe, to connect to a NRPE
    service. check\_nrpe has a timeout paramer, -t. This parameter must
    have a bigger value than the one you set in step #2.

    Log-on to your Nagios server and you can set this by opening the
    commands configuration file, /etc/nagios/objects/commands.cfg

    Find check\_nrpe, and edit its command\_line and set the -t
    parameter. For instance, if you want the timeout value to be 500
    seconds, it will look like this: command\_line $USER1$/check\_nrpe
    -H $HOSTADDRESS$ -c $ARG1$ -t 500

    Restart the Nagios service afterwards (service nagios restart).

In most cases these 3 steps should do :)
