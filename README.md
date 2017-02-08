freebsd-jailed-joomla
=========

This role provides a jailed Joomla server. The jail is set up using the 
latest version. 

Additionally an user is created who is allowed to access this site via SFTP. 

Furthermore Lets Encrypt is used to create and manage server certificates.

To see this role in action, have a look at [this project of mine](https://github.com/JoergFiedler/freebsd-ansible-demo).

Requirements
------------

This role is intent to be used with a fresh FreeBSD installation. There is a
Vagrant Box with providers for VirtualBox and EC2 you may use.

You will find a sample project which uses this role [here](https://github.com/JoergFiedler/freebsd-ansible-demo).

Role Variables
--------------

##### joomla_server_name
The server name for this Joomla installation. Use the domain name here, e.g.
`example.com`. Default: `{{ jail_name }}`.

##### joomla_server_aliases
The domain name aliases for this server, e.g. `www.example.com`. Default: `''`.

##### joomla_server_force_www
Create redirect rules to redirect all requests to `www` subdomain. You need to
specify `joomla_server_aliases`. Default: `false`.

##### joomla_server_force_https
Create redirect rules to redirect all requests to `https` scheme. Certificates
will be created using Lets Encrypt. Default: `false`.

##### joomla_download_url
The Download URL. Default: `'https://github.com/joomla/joomla-cms/releases/download/3.6.5/Joomla_3.6.5-Stable-Full_Package.zip'`.

##### joomla_db_name
The MariaDB database name. The database will be created if not exists.
Default: : Default: `'joomla_{{ wp_server_name_ }}'`.

##### joomla_db_user
The db user name for this WordPress installation. Default: `'wp_{{ joomla_server_name_ }}'`.

##### joomla_db_password
The db user's password. Default: `'joomla_{{ server_name_ }}'`.

##### joomla_db_priv
The privileges granted to the db user. Default: `'{{ joomla_db_name }}.*:All'`.

##### joomla_db_host
The MariaDB host ip. Default: `''`.

##### joomla_db_host_user
The administrative DB user used to create the user and the db. Default: `''`.

##### joomla_db_host_password
The password for the administrative DB user. Default: `''`.

##### joomla_sftp_uuid
The uid for the SFTP user to create. Default: `5000`.

##### joomla_sftp_port
The port the SFTP server should listen on. Default: `10200`.

##### joomla_sftp_user
The SFTP user name. Default: `'sftp_{{ joomla_server_name_ | truncate(5, True, "") }}'`.

##### joomla_sftp_authorized_keys
The authorized key file used for SFTP access to this WordPress installation. 
Default: `''`

Dependencies
------------

- [JoergFiedler.freebsd-jailed-nginx](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-nginx)

Example Playbook
----------------
    - { role: JoergFiedler.freebsd-jailed-joomla,
        tags: ['_joomla'],
        jail_name: 'joomla',
        joomla_server_name: 'example.com',
        joomla_server_aliases: 'www.example.com',
        joomla_server_force_www: true,
        joomla_server_force_https: true,
        joomla_db_host_user: 'root',
        joomla_db_host_password: 'password',
        joomla_db_password: 'password',
        joomla_sftp_port: 10101,
        joomla_sftp_authorized_keys: 'public.key.file',
        jail_net_ip: '10.1.0.207' }

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue
on GitHub. Thanks.
