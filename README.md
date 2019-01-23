[![Build Status](https://travis-ci.org/JoergFiedler/freebsd-jailed-joomla.svg?branch=master)](https://travis-ci.org/JoergFiedler/freebsd-jailed-joomla)

freebsd-jailed-joomla
=========

This role provides a jailed Joomla server. Server can be managed via SFTP. 

Furthermore Lets Encrypt is used to create and manage server certificates.

Requirements
------------

This role is intent to be used with a fresh FreeBSD installation. There is a
Vagrant Box with providers for VirtualBox and EC2 you may use.

Role Variables
--------------
| Variable | Description | Default |
| :------- | :---------- | :-----: |
| joomla_db_host | IP address of the DB host. | `''` |
| joomla_db_host_password | Password of the user who is allowed to create tables and grant permissions. | 'passwd' |
| joomla_db_host_user | The user that is allowed to create tables and grant permissions. | `root` |
| joomla_db_name | The database name for the Joomla DB instance. | `joomla_{{ joomla_server_name_ }}` |
| joomla_db_password | The password for the Joomla DB instance. | `joomla_{{ joomla_server_name_ }}` |
| joomla_db_priv | Privileges given to the Joomla DB instance user. | `{{ joomla_db_name }}.*:All` |
| joomla_db_user | The Joomla DB instance user name. | `joomla_{{ joomla_server_name_ }}` |
| joomla_download_url | Joomla package URL. | `https://downloads.joomla.org/cms/joomla3/3-9-1/joomla_3-9-1-stable-full_package-tar-gz?format=gz` |
| joomla_server_aliases | Aliases (domain names) that the server should listen to. | `''` |
| joomla_server_force_www | Set to `yes` if the server should redirect to `www` subdomain. Add `www.{{ server_name }}` to aliases. | `no` |
| joomla_server_home | Server home directory. | `/srv/{{ joomla_server_name }}` |
| joomla_server_https_certbundle_file | CA certificate chain. | `localhost-certbundle.pem` |
| joomla_server_https_dhparam_file | DH param file. |  `localhost-dhparam.pem` |
| joomla_server_https_enabled | Enable HTTPS for this server. Automatic redirect of non-HTTPS request will happen. | `yes` |
| joomla_server_https_key_file | The server's private key. | `localhost-key.pem` |
| joomla_server_name | The server name (domain name). | `{{ jail_name }}` |
| joomla_nginx_pf_redirect | All http(s) traffic will be redirect from host to this jail. | `no` |
| joomla_server_syslogd_server | The syslogd server to use for request logging. | `localhost` |
| joomla_server_tarsnap_enabled | Backup the server's webroot using Tarsnap. Must be enabled on host level as well. | `no` |
| joomla_sftp_authorized_keys | File that should be used as `authorized_keys` file for SFTP. | `''` |
| joomla_sftp_port | Port to use for SFTP. | `10022` |
| joomla_sftp_uuid | UUID for the SFTP user. | `5000` |
| joomla_sftp_user | Name of the SFTP user. | `sftp_{{ joomla_server_name_  truncate(5, True, "", 0) }}` |

Dependencies
------------

- [JoergFiedler.freebsd-jailed-nginx](https://galaxy.ansible.com/JoergFiedler/freebsd-jailed-nginx)

Example Playbook
----------------

    - hosts: all
      become: true
    
      tasks:
        - include_role:
            name: 'JoergFiedler.freebsd-jailed-mariadb'
          vars:
            jail_freebsd_release: '11.2-RELEASE'
            jail_name: 'mariadb'
            jail_net_ip: '10.1.0.5'
        - include_role:
            name: 'JoergFiedler.freebsd-jailed-joomla'
          tags:
            - joomla
          vars:
            jail_freebsd_release: '11.2-RELEASE'
            jail_name: 'joomla'
            jail_net_ip: '10.1.0.10'
            joomla_db_host: '10.1.0.5'
            joomla_db_host_password: 'passwd'
            joomla_db_host_user: 'root'
            joomla_db_password: 'password'
            joomla_download_url: 'https://downloads.joomla.org/cms/joomla3/3-9-1/joomla_3-9-1-stable-full_package-tar-gz?format=gz'
            joomla_server_https_enabled: yes
            joomla_server_name: 'localhost'
            joomla_nginx_pf_redirect: yes
            joomla_sftp_authorized_keys: '~/.vagrant.d/insecure_private_key.pub'
            joomla_sftp_port: 10022
    
License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue
on GitHub. Thanks.
