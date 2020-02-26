PostgreSQL Master/Slave
=========

Preface
------------
Note: This is a fork off of bbaassssiiee.base_postgres role, from ansible galaxy
https://github.com/bbaassssiiee/base_postgres/
This role is used to ansibilize our SonarQube instance (sonarqube01.m.dfw.rtrdc.net/).
We have forked it off to be stored in-house for a couple reasons:
# There is a task that is commented out for some reason, `restart iptables`. This can cause the playbook to error out, since other tasks still invoke it.
# It has been archived and migrated to dockpack.base_postgres
# We also want to have more control over the configurations going forward
------------

PostgreSQL 9.6 on one or two RHEL/Centos boxes.

Requirements
------------
Internet. RedHat Linux 6 or 7, or Centos 6 or 7.

This role wast tested with molecule:


Role Variables
--------------
The first three vars you must set, the others are optional.

    base_postgres_mip  # This is the ip address of the primary/master database
    base_postgres_user # This is your user
    base_postgres_pass # This is your password

    base_postgres_net  # 192.168.20.0/24 This is the subnet granted access
	  base_postgres_role # With 2 hosts the one is master, the other slave
    base_postgres_sip  # The ip address of the slave when you use 2 databases


Dependencies
------------
Ansible Tower 2.4.5 is compatible with this.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

# Inventory

		[dataservers]
		data1 role=master
		data2 role=slave

# playbook for dbserver tier
  ---
	- name: 'dbservers.yml'
		hosts: dbservers
		become: yes
		gather_facts: True

		vars_files:
			- dbservers/secrets.yml

		pre_tasks:
			- include: dbservers/pre_tasks.yml

		roles:
			- bbaassssiiee.base_postgres_role
		- rsyslog

		tasks: []

		post_tasks:
			- include: dbservers/post_tasks.yml

License
-------

BSD, MIT

Author Information
------------------
http://twitter.com/bbaassssiiee
https://github.com/bbaassssiiee/base_postgres_role.git

