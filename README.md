elma-doc (master)
===============================

Documentation for the elma project
----------------------------------

This is a work in progress. We are currently migrating over to a new document
generation framework.

## Learning the doc tools

If you are new to rst and Sphinx, visit the Sphinx doc to get started:
http://sphinx-doc.org/contents.html

## Instructions

These assume default installs of Python for Windows and Linux

### Generate HTML Documentation on Linux

1.  Download the pip installer from here: https://raw.github.com/pypa/pip/master/contrib/get-pip.py
2.  Run: python ./get-pip.py
3.  Run: pip install sphinx
4.  Checkout Branch in Repo –
  1.  Run: git clone https://github.com/rsyslog/rsyslog-doc.git
  2.  Run: cd elma-doc
  3.  Run: git checkout v5-stable
5.  Run: sphinx-build -b html source build
6.  open elma-doc/build/index.html in a browser

###Generate HTML Documentation on Windows

1.  Download the pip installer from here: https://raw.github.com/pypa/pip/master/contrib/get-pip.py
2.  Download and install Git for windows if you don’t already have Git:
  1.  https://code.google.com/p/msysgit/downloads/list?can=3&q=full+installer+official+git&colspec=Filename+Summary+Uploaded+ReleaseDate+Size+DownloadCount
  2.  Install Git for Windows.
3.  Run: c:\python27\python get-pip.py
4.  Run: c:\python27\scripts\pip install sphinx
5.  Checkout Branch in Repo –
  1.  Run: git clone https://github.com/rsyslog/rsyslog-doc.git
  2.  Run: cd rsyslog-doc
  3.  Run: git checkout v5-stable
6.  Run: c:\python27\scripts\sphinx-build -b html source build
7. open rsyslog-doc/build/index.html in a browser

Enterprise Log Management Appliance (ELMA)
------------------------------------------

ELMA is a logging and syslog framework complemented by a range of free and open-source tools that can help you aggregate and deliver metrics, analytics and vital performance data.

Ship logs from any source, get the right timestamp, parse and normalize them into structured JSON format, index them.
All your logs and other event data from all over your infrastructure in a central place – Search, graph and analyse them.

##Support:

- Website: http://enterprise-log-management-appliance.org
- Support: http://code.google.com/p/enterprise-log-management-appliance/issues/list
- Discussion group: http://groups.google.com/group/enterprise-log-management-appliance

##Development:

- Appliance Build Service: https://susestudio.com/a/TOYySW/enterprise-log-management-appliance--4
- Distribution Build Service: https://build.opensuse.org/project/show/security:logging:elma
- ReST Documentation Repository: https://github.com/enterprise-log-management-appliance/elma-doc

##Authors:

- Joerg Heinemann <heinemannj66@gmail.com>
