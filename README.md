freebsd-jailed-joomla
=========

This role provides a jailed Joomla CMS server.

To see this role in action, have a look at [this project of mine](https://github.com/JoergFiedler/freebsd-ansible-demo).

Requirements
------------

This role is intent to be used with a fresh FreeBSD 10.2 installation. There is a Vagrant Box with providers for VirtualBox and EC2 you may use.

You will find a sample project which uses this role [here](https://github.com/JoergFiedler/freebsd-ansible-demo).

Role Variables
--------------

##### joomla_download_url
The Joomla Download URL. Default: `'https://github.com/joomla/joomla-cms/releases/download/3.4.8/Joomla_3.4.8-Stable-Full_Package.zip'`.

##### joomla_db_name
The MariaDB database name. The database will be created if not exists. Default: : Default: `'joomla_{{ server_name_ }}'`.

##### joomla_db_user
The db user name for Joomla CMS. Default: `'joomla_{{ server_name_ }}'`.

##### joomla_db_password
The Joomla CMS's db user password. Default: `'joomla_{{ server_name_ }}'`.

##### joomla_db_priv
The priviliges granted to the Joomla CMS's db user. Default: `'{{ joomla_db_name }}.*:All'`.

##### joomla_db_host
The MariaDB host ip. Default: `'10.1.0.4'`.

##### joomla_db_host_user
The user used to create the Joomla CMS database and user. Default: `'root'`.

##### joomla_db_host_password
The passwort for the administrative DB user. Default: `'passwd'`.

Dependencies
------------

- [JoergFiedler.freebsd-jailed-php-fpm](https://galaxy.ansible.com/detail#/role/7079)

Example Playbook
----------------

    - { role: JoergFiedler.freebsd-jailed-joomla,
        tags: ['joomla'],
        use_ssmtp: true,
        use_syslogd_server: true,
        jail_name: 'joomla',
        jail_net_ip: '10.1.0.6' }

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue on Github. Thanks.
