=====================
 Configuration Files
=====================

This HOWTO explains the configuration files for ELMA and openSUSE
distribution.

Apache2
=======

+--------------------------------------------+------------+
| /opt/elma/config/apache2/conf.d/htpasswd   | :sup:`1`   |
+--------------------------------------------+------------+

+--------------------------------------------------------+------------+
| /opt/elma/config/apache2/vhosts.d/elma-ssl.conf        | :sup:`1`   |
+--------------------------------------------------------+------------+
| /opt/elma/config/apache2/vhosts.d/kibana-ssl.conf      | :sup:`1`   |
+--------------------------------------------------------+------------+
| /opt/elma/config/apache2/vhosts.d/redirect-http.conf   | :sup:`1`   |
+--------------------------------------------------------+------------+

:sup:`1`

ElasticSearch
=============

+-----------------------------------------------+----------------------------------------+------------+
| /opt/elasticsearch/config/elasticsearch.yml   | /etc/elasticsearch/elasticsearch.yml   | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elasticsearch/config/logging.yml         | /etc/elasticsearch/logging.yml         | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+

:sup:`1` ElasticSearch distribution config files - copied and modified
during /opt/elma/bin/install/elasticsearch-update.sh

systemd
-------

+--------------------------------------------------+-------------------------------------------------+------------+
| /opt/elma/config/sysconfig/elasticsearch         | /etc/sysconfig/elasticsearch                    | :sup:`1`   |
+--------------------------------------------------+-------------------------------------------------+------------+
| /opt/elma/config/systemd/elasticsearch.service   | /usr/lib/systemd/system/elasticsearch.service   | :sup:`1`   |
+--------------------------------------------------+-------------------------------------------------+------------+

:sup:`1` Copied and modified during
/opt/elma/bin/install/elasticsearch-update.sh

+--------------------------------------+-----------------------------+------------+
| openSUSE distribution config files   | /etc/security/limits.conf   | :sup:`1`   |
+--------------------------------------+-----------------------------+------------+
| openSUSE distribution config files   | /etc/permissions.local      | :sup:`1`   |
+--------------------------------------+-----------------------------+------------+
| openSUSE distribution config files   | /etc/init.d/boot.local      | :sup:`1`   |
+--------------------------------------+-----------------------------+------------+

:sup:`1` Modified during /opt/elma/bin/install/elasticsearch-update.sh

logrotate
---------

+----------------------------------------------+----------------------------------+------------+
| /opt/elma/config/logrotate.d/elasticsearch   | /etc/logrotate.d/elasticsearch   | :sup:`1`   |
+----------------------------------------------+----------------------------------+------------+

:sup:`1` Linked to logrotate configurations

jetty
-----

+----------------------------------------------------------------+----------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/jetty-adons-elasticsearch.yml   | /etc/elasticsearch/jetty-adons-elasticsearch.yml   | :sup:`1`   |
+----------------------------------------------------------------+----------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/jetty-local-server.xml          | /etc/elasticsearch/jetty-local-server.xml          | :sup:`1`   |
+----------------------------------------------------------------+----------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/jetty-no-http.xml               | /etc/elasticsearch/jetty-no-http.xml               | :sup:`1`   |
+----------------------------------------------------------------+----------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/keystore                        | /etc/elasticsearch/keystore                        | :sup:`1`   |
+----------------------------------------------------------------+----------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/realm.properties                | /etc/elasticsearch/realm.properties                | :sup:`1`   |
+----------------------------------------------------------------+----------------------------------------------------+------------+

:sup:`1` Copied during /opt/elma/bin/install/elasticsearch-update.sh

templates
---------

+---------------------------------------------------------------------------------+---------------------------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/templates/template\_diag-no-compression.json     | /etc/elasticsearch/templates/template\_diag-no-compression.json     | :sup:`1`   |
+---------------------------------------------------------------------------------+---------------------------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/templates/template\_diag.json                    | /etc/elasticsearch/templates/template\_diag.json                    | :sup:`1`   |
+---------------------------------------------------------------------------------+---------------------------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/templates/template\_kibana3-configuration.json   | /etc/elasticsearch/templates/template\_kibana3-configuration.json   | :sup:`1`   |
+---------------------------------------------------------------------------------+---------------------------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/templates/template\_logstash-daily.json          | /etc/elasticsearch/templates/template\_logstash-daily.json          | :sup:`1`   |
+---------------------------------------------------------------------------------+---------------------------------------------------------------------+------------+
| /opt/elma/config/elasticsearch/templates/template\_rsyslog-daily.json           | /etc/elasticsearch/templates/template\_rsyslog-daily.json           | :sup:`1`   |
+---------------------------------------------------------------------------------+---------------------------------------------------------------------+------------+

:sup:`1` Copied during /opt/elma/bin/install/elasticsearch-update.sh

ELMA
====

+-----------------------------------+-----------------------+---------------+
| /opt/elma/config/elma/elma.conf   | /etc/elma/elma.conf   | :sup:`1, 2`   |
+-----------------------------------+-----------------------+---------------+

| :sup:`1` Copied during /opt/elma/bin/install/elma-install.sh
| :sup:`2` Reconfigured during /opt/elma/bin/install/elma-update.sh

git
===

+-------------------------------+------------+
| /opt/elma/config/git/.netrc   | :sup:`1`   |
+-------------------------------+------------+

:sup:`1`

Kibana
======

+-------------------------------+-------------------------------+------------+
| /opt/kibana/KibanaConfig.rb   | /etc/kibana/KibanaConfig.rb   | :sup:`1`   |
+-------------------------------+-------------------------------+------------+

+------------+
| :sup:`1`   |
+------------+

+--------------------------------------------------+-------------------------------------------------+------------+
| /opt/elma/config/kibana-dashboard/default.elma   | /opt/elasticsearch-kibana3/dashboards/default   | :sup:`1`   |
+--------------------------------------------------+-------------------------------------------------+------------+

:sup:`1` Copied during /opt/elma/bin/install/kibana-update.sh

Logstash
========

+----------------------------------------------------------+----------------------------------------------+------------+
| /opt/elma/config/logstash/logstash-json-tcp-input.conf   | /etc/logstash/logstash-json-tcp-input.conf   | :sup:`1`   |
+----------------------------------------------------------+----------------------------------------------+------------+
| /opt/elma/config/logstash/logstash-json-udp-input.conf   | /etc/logstash/logstash-json-udp-input.conf   | :sup:`1`   |
+----------------------------------------------------------+----------------------------------------------+------------+

:sup:`1` Copied during /opt/elma/bin/install/logstash-update.sh

systemd
-------

+---------------------------------------------+--------------------------------------------+------------+
| /opt/elma/config/systemd/logstash.service   | /usr/lib/systemd/system/logstash.service   | :sup:`1`   |
+---------------------------------------------+--------------------------------------------+------------+

:sup:`1` Copied during /opt/elma/bin/install/logstash-update.sh

Nagios
======

+--------------------------------------+--------------------------+------------+
| openSUSE distribution config files   | /etc/nagios/nagios.cfg   | :sup:`1`   |
+--------------------------------------+--------------------------+------------+
| openSUSE distribution config files   | /etc/nagios/cgi.cfg      | :sup:`1`   |
+--------------------------------------+--------------------------+------------+

:sup:`1` Modified during /opt/elma/bin/install/nagios-update.sh

ELMA system monitoring
----------------------

+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/commands.cfg          | /etc/nagios/elma/commands.cfg          | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/contacts.cfg          | /etc/nagios/elma/contacts.cfg          | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/hostgroups.cfg        | /etc/nagios/elma/hostgroups.cfg        | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/hosts.cfg             | /etc/nagios/elma/hosts.cfg             | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/network.cfg.example   | /etc/nagios/elma/network.cfg.example   | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/network.cfg\_sample   | /etc/nagios/elma/network.cfg\_sample   | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/servicegroups.cfg     | /etc/nagios/elma/servicegroups.cfg     | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/services.cfg          | /etc/nagios/elma/services.cfg          | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/templates.cfg         | /etc/nagios/elma/templates.cfg         | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+
| /opt/elma/config/nagios/timeperiods.cfg       | /etc/nagios/elma/timeperiods.cfg       | :sup:`1`   |
+-----------------------------------------------+----------------------------------------+------------+

:sup:`1` Copied and modified during
/opt/elma/bin/install/nagios-update.sh

rsyslog
=======

+------------------------------------------------------------------------------------+------------+
| /opt/elma/config/rsyslog.conf                                                      | :sup:`1`   |
+------------------------------------------------------------------------------------+------------+
| /opt/elma/config/rsyslog.d/diag-common.conf                                        | :sup:`1`   |
+------------------------------------------------------------------------------------+------------+
| /opt/elma/config/rsyslog.d/elma\_diag\_elasticsearch.conf                          | :sup:`1`   |
+------------------------------------------------------------------------------------+------------+
| /opt/elma/config/rsyslog.d/elma\_diag\_imptcp\_elasticsearch-no-compression.conf   | :sup:`1`   |
+------------------------------------------------------------------------------------+------------+
| /opt/elma/config/rsyslog.d/elma\_diag\_imptcp\_elasticsearch.conf                  | :sup:`1`   |
+------------------------------------------------------------------------------------+------------+
| /opt/elma/config/rsyslog.d/elma\_diag\_imptcp\_gzipwr.conf                         | :sup:`1`   |
+------------------------------------------------------------------------------------+------------+
| /opt/elma/config/rsyslog.d/remote.conf                                             | :sup:`1`   |
+------------------------------------------------------------------------------------+------------+

:sup:`1`

SNMP trap receiver
==================

+----------------------------------------+------------+
| /opt/elma/config/snmp/snmptrapd.conf   | :sup:`1`   |
+----------------------------------------+------------+

:sup:`1`
