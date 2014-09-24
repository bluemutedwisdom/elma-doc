=====
 git
=====

This project's Git repository may be accessed using many different
client programs and plug-ins. See your `client's
documentation <http://git-scm.com/downloads>`__ for more information.

Commit Often, Perfect Later, Publish Once: `Git Best
Practices <http://sethrobertson.github.com/GitBestPractices/>`__

git repository
==============

::

    touch ~/.netrc
    vi ~/.netrc
    machine github.com login your-github-member-UID password your-github-member-UID-password

::

    Global setup:
    Set the right GoogleCode.com project member UID and email address:
    git config --global user.name "your-github-member-UID"
    git config --global user.email "your-email-address"
    chmod 600 ~/.gitconfig
    git config --list

::

    # ######### Init empty repository #########
    #
    # rm -R /opt/elma-0.0.35/.git 
    # cd /opt/elma-0.0.35; git init
    # cd /opt/elma-0.0.35; env GIT_SSL_NO_VERIFY=true git remote add origin https://github.com/enterprise-log-management-appliance/elma-doc.master/
    # cd /opt/elma-0.0.35; git add .
    # cd /opt/elma-0.0.35; git commit -m 'initial commit'
    # ######### Push new revision to remote repository #########
    #
    # cd /opt/elma-0.0.35; git add .
    # cd /opt/elma-0.0.35; git commit -m 'your revision comment'
    # cd /opt/elma-0.0.35; env GIT_SSL_NO_VERIFY=true git push origin master
    # ######### Pull new revision from remote repository #########
    #
    # cd /opt/elma-0.0.35; env GIT_SSL_NO_VERIFY=true git pull origin master
