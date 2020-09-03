 apache_failover
=================

A simple Ansible role create web service Failover. Install and configuring the Apache, keepalived and lsyncd on RHEL/CentOS, Debian 10, Ubuntu 20.04

- Install the necessary packages;
- Configure apache, vhost and ssl
- Configure keepalived
- Configure lsyncd


Requirements
------------

- The SELinux and firewall settings are not considered to be a concern of this role.

- A user with a key to the standby server with write permission to /var/www/html is required. In my example I'm using root with public key for the standby server. if you use another user for this activity to make changes to the lsyncd.conf.j2 file, in the following line below.

```
rsync={rsh ="/usr/bin/ssh -l root -i /root/.ssh/id_rsa",}

```

Attention the communication between the master and the backup through a public key must exist before running the playbook for the correct operation of lsyncd

Role Variables
--------------

Necessary changes to defaults / main.yml

| Variable                                     | Default                       | Comments                                                                                |
| :---                                         | :---                          | :---                                                                                    |
| `apache_failover_docroot`                    | '/var/www/html'               | Path to the document root (directory containing html files)                             |
| `apache_failover_vip_ip`                     |                               | Inform ip address that will be used by the keepalived VIP
|                                              |                               |
| `apache_failover_domain`                     |                               | Inform your domain
|                                              |                               |

The IP that is used in the variable apache_failover_vip_ip must be the same configured in the DNS server for the domain informed in the variable apache_failover_domain.


Dependencies
------------

No dependencies.

Installing Certificates
-----------------------

It is necessary to exchange the certificates below for your certificates

```
├── files
│   ├── almircandido.com_ca
│   ├── almircandido.com.crt
│   └── almircandido.com.key

```

Attention
==========

The inventory and playbook should be created like the example below

Playbook
---------

```
---
- hosts: master,backup
  become: yes
  roles:
    - /path/acandid.apache_failover
...
```

inventory
---------

```
[master]
192.168.0.13

[standby]
192.168.0.15

```

## Contributing

Issues, feature requests, ideas are appreciated and can be posted in the Issues section.


Author Information
------------------
LinkedIn: https://br.linkedin.com/in/almircandido
