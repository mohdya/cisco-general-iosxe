Ansible Role : Cisco IOS-XE device general config (hardening)
=========

Sample for Cisco IOS-XE devices general configuration.
This sample gives a brief idea on ansible playbook, jinja2 templates, etc for general mostly used Cisco IOS-XE configuration.

Requirements
------------

Basic Ansible install should be good enough to use it.


Role Variables
--------------

Variables used in the playbook are listed below.
Some of the hardening did not need varaiables and hence coded directly in the Jinja2 Template. So please make sure to check it out.

Specifying the list of NTP and SNMP Servers as below.

      ntp_servers_list:
        - 23.23.23.23
        - 2.2.2.3
      snmp_servers_list:
        - 23.44.44.44
        - 34.33.22.33

Specifying the SNMP Community, Domain name and Exec Timeout values.
      snmp_community: cisco
      domain_name: virl.info
      exec_timeout_min: 0

Dependencies
------------

None.

Example Playbook
----------------

- hosts: all
  gather_facts: true
  connection: local
  become: yes
  become_method: enable
  roles:
    - role: cisco-general
      ntp_servers_list:
        - 23.23.23.23
        - 2.2.2.3
      snmp_servers_list:
        - 23.44.44.44
        - 34.33.22.33
      snmp_community: cisco
      domain_name: virl.info
      exec_timeout_min: 0

Inside the Role:

   - name: Check what will be pushed.
     ios_config: 
       diff_against: intended
       intended_config: general.j2

   - name: General config template push
     ios_config: 
       backup: yes
       src: general.j2

   - name: Set Banner Login
     ios_banner:
       banner: login
       text: |
         This is the login banner for a confidential device.
         Please disconnect if you are not authorised.
       state: present


   - name: Set Banner Motd
     ios_banner:
       banner: motd
       text: |
         This is the motd banner for a confidential device.
         Please disconnect if you are not authorised.
       state: present

TODO:
Adding other general configurations.

License
-------
BSD

Author Information
------------------

This role was created by Yaseen M. 
Version 6 - Uploaded in August 2019.
