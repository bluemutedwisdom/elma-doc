elma-doc (master)
===============================

The documentation for the ELMA project is a work in progress.
We are currently migrating over to a new document generation framework.

Enterprise Log Management Appliance (ELMA)
------------------------------------------

ELMA is a logging and syslog framework complemented by a range of free and open-source tools that can help you aggregate and deliver metrics, analytics and vital performance data.

Ship logs from any source, get the right timestamp, parse and normalize them into structured JSON format, index them.
All your logs and other event data from all over your infrastructure in a central place – Search, graph and analyse them.

##Support

- Website: http://enterprise-log-management-appliance.org
- Support: http://code.google.com/p/enterprise-log-management-appliance/issues/list
- Discussion group: http://groups.google.com/group/enterprise-log-management-appliance

##Development

- Appliance Build Service: https://susestudio.com/a/TOYySW/enterprise-log-management-appliance--4
- Distribution Build Service: https://build.opensuse.org/project/show/security:logging:elma
- ReST Documentation Repository: https://github.com/enterprise-log-management-appliance/elma-doc

##Copyrights

ELMA - <a href="https://build.opensuse.org/package/view_file/security:logging:elma/elma/elma.doc.COPYING">Copyright © 2012-2014 by Joerg Heinemann</a>"Copyright © 2012-2014 by Joerg Heinemann":https://build.opensuse.org/package/view_file/security:logging:elma/elma/elma.doc.COPYING.
Licensed under "GNU GPL v3 or higher":http://www.gnu.org/licenses/gpl.html.

##Licenses

All of the "source code to this project":https://build.opensuse.org/project/show/security:logging:elma is available under licenses which are both "free":http://www.gnu.org/philosophy/free-sw.html and "open source":http://www.opensource.org/docs/definition.php.
Most of it is available under "GNU GPL":http://www.gnu.org/licenses/gpl.html and "Apache License, Version 2.0":http://www.apache.org/licenses/LICENSE-2.0.

Sphinx documentation framework
------------------------------

If you are new to rst and Sphinx, visit the Sphinx doc to get started:
http://sphinx-doc.org/contents.html

- Generation of ELMA HTML Documentation assumes the installation of all ELMA devel related packages:
```
zypper -qn --no-gpg-checks --gpg-auto-import-keys install elma-devel
```
- Creation of a new ELMA documentation package:
```
/usr/share/elma-devel/bin/elma-build.sh --elma-doc
```
