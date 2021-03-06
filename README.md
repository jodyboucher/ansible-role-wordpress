# Ansible role: wordpress

An [Ansible](https://www.ansible.com/) role that installs and configures WordPress sites.

The role assumes any configured site will only provide access through the SSL port.

This role is designed for Ubuntu 16.04 Xenial.

## Requirements

This role requires `root` access so be sure to enable privilege escalation:

```yml
# privilege escalation of play
- hosts: wordpress-servers
  become: true
  roles:
    -role: wordpress

# escalation of single role only
- hosts: wordpress-servers
  roles:
    - role: wordpress
      become: true
```

## Role Variables

The available variables of this role are listed here along with default values:

```yml
# The name of the database root user or other user with
# privileges to create databases and user and assign priveleges.
wordpress_db_root_username: root
# The password for the database root user
wordpress_db_root_password: changeme


wordpress_sites:
example.com:
  title: "example.com site"
  admin_user: admin_user
  admin_password: adminuser
  admin_email: "admin@example.com"
  db:
    name: example_com
    host: localhost
    username: example_com
    password: example_com
    import_data: false
    import_data_file:
    table_prefix: "wp_"
  www_root: "/var/www/example_com"
  wordpress_root: "/var/www/example_com/wordpress"

# Install WordPress core
# core is installed into wordpress_data.[site].wordpress_root
wordpress_install_core: true

# The version of WordPress to installed
wordpress_version: 4.7.3

# The WordPress language to install
wordpress_language: en_US

# The user/group that runs nginx processes
wordpress_nginx_user: "www-data"
wordpress_nginx_group: "www-data"

# Various templates used to configure nginx
wordpress_nginx_common_template: "wordpress-common.conf.j2"
wordpress_nginx_common_restrictions_template: "wordpress-common-restrictions.conf.j2"
wordpress_nginx_site_ssl_template: "ssl-site.conf.j2"
wordpress_nginx_site_template: "available-site.conf.j2"
wordpress_nginx_site_default_template: "available-default.conf.j2"

# Base nginx configuration path
wordpress_nginx_config_path: "/etc/nginx"

# Path to nginx site confs
wordpress_nginx_config_sites_path: "{{ wordpress_nginx_config_path }}/sites-available"

# Path to nginx ssl confs
wordpress_nginx_config_ssl_path: "{{ wordpress_nginx_config_path }}/ssl"

# Path to nginx WordPress confs
wordpress_nginx_config_wordpress_path: "{{ wordpress_nginx_config_path }}/wordpress"

# Path to certificates used for SSL
wordpress_nginx_certificate_path: "/etc/letsencrypt/live"

# true to use self-signed certs, false to use production certs
wordpress_nginx_certificate_use_selfsigned: true

# The self-signed cert
wordpress_nginx_certificate_selfsigned: "{{ wordpress_nginx_config_path }}/snippets/snakeoil.conf"
```

## Dependencies

None.

## Example Playbook

```yml
---
- hosts: wordpress-servers
  become: true
  vars_files:
    - vars/main.yml
  roles:
    - { role: nginx }
```

Inside `vars/main.yml`:

```yml
---
wordpress_db_root_username: root
wordpress_db_root_password: somepassword

wordpress_sites:
  my-site.com:
    db:
      name: mysite_com
      host: localhost
      username: mysite_user
      password: mysite_password
      import_data: false
      import_data_file:
    www_root: /var/www/mysite_com
```

## Installation

On the command-line:

```bash
ansible-galaxy install git+https://github.com/jodyboucher/ansible-role-wordpress.git
```

or in a role file (requirements.yml):

```yml
- name: wordpress
  src: https://github.com/jodyboucher/ansible-role-wordpress
  version: master
```

## License

MIT

## Author Information

This role was created by [Jody Boucher](https://jodyboucher.com/).
