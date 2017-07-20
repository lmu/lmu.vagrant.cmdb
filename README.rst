===================
lmu.vagrant.cmdb
===================

Vagrant and Ansible based test setup for a CMDB Instance setup as used at LMU Dez. VI.

Requirements
============

* git
* Vagrant (tested on Vagrant 1.9.x, https://www.vagrantup.com/)
* Ansible (2.3+, https://www.ansible.com/)

Your implicit requirements for this setup is:

* for Vagrant / used Vagrant base box:

  * A 64 bit Processor
  * A vPro enabled System / Processor (Intel I5/I7) so that you could use 64 bit guests
  * Virtualbox (https://www.virtualbox.org/)

* for Ansible:

  * An Unix complied Environment (Linux, Mac OS, *BSD)
  * Python2

Dependencies
============

This Git repository uses submodules for including ansible rolls and playbooks used at the LMU Dez. VI.
Please refer to the lmu.ansible.playbooks repository at https://github.com/lmu/lmu.ansible.playbooks for them.

Setup
=====

Please clone this git Repository with all submodules (lmu.ansible.playbooks).
go into the directory and start Vagrant

.. code:: bash

    git clone --recursive https://github.com/lmu/lmu.vagrant.cmdb.git
    cd lmu.vagrant.redmine
    git clone https://github.com/lmu/lmu.ansible.playbooks / ln -s <path>/lmu.ansible.playbooks
    vagrant up

If you update or change the cmdb playbook rerun the vagrant provision with:

.. code:: bash

    vagrant provision


Development
===========

If you want to contribute to this vagrant environment please contact me at Alexander.Loechel@lmu.de and tell me your github account, so that I could add you to the allowed contributers list.
