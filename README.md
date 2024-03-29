# Ansible role [phpmyadmin](https://galaxy.ansible.com/ui/standalone/roles/buluma/phpmyadmin/documentation)

phpMyAdmin installation for Linux

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-phpmyadmin/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-phpmyadmin/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-phpmyadmin.svg)](https://github.com/buluma/ansible-role-phpmyadmin/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-phpmyadmin.svg)](https://github.com/buluma/ansible-role-phpmyadmin/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-phpmyadmin.svg)](https://github.com/buluma/ansible-role-phpmyadmin/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/phpmyadmin)](https://galaxy.ansible.com/ui/standalone/roles/buluma/phpmyadmin/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-phpmyadmin/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true

  vars:
    php_version: "7.3"
    phpmyadmin_enablerepo: "remi,remi-php73"
    phpmyadmin_mysql_user: sp3cial_us3r
    phpmyadmin_mysql_password: s3cure_password_h3r3

  pre_tasks:
    - name: Update apt cache.
      ansible.builtin.apt: update_cache=true cache_valid_time=600
      when: ansible_os_family == 'Debian'

  roles:
    # - role: geerlingguy.repo-remi  # TODO: Rebuild
    #   when: ansible_os_family == 'RedHat'
    # - role: geerlingguy.apache
    # - role: geerlingguy.mysql
    # - role: buluma.php_versions
    # - role: geerlingguy.php
    # - role: geerlingguy.php-mysql  # TODO: Rebuild
    - role: buluma.phpmyadmin

  post_tasks:
    - name: Ensure phpMyAdmin is running.
      ansible.builtin.uri:
        url: "http://127.0.0.1/phpmyadmin/"
        status_code: 200
      register: result
      until: result.status == 200
      retries: 60
      delay: 1
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-phpmyadmin/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: buluma.bootstrap
    - role: geerlingguy.apache
    - role: geerlingguy.mysql
    - role: geerlingguy.php
    - role: geerlingguy.php-mysql
    - role: geerlingguy.repo-remi
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-phpmyadmin/blob/master/defaults/main.yml):

```yaml
---
# Pass in a comma-separated list of repos to use (e.g. "remi,epel"). Used only
# for RHEL/CentOS.
phpmyadmin_enablerepo: epel

# Override if needed. This is set platform-specific in the vars dir if not set.
# phpmyadmin_config_file: /etc/phpmyadmin/config.inc.php
phpmyadmin_mysql_host: localhost
phpmyadmin_mysql_port: ""
phpmyadmin_mysql_socket: ""
phpmyadmin_mysql_connect_type: tcp
phpmyadmin_mysql_user: root
phpmyadmin_mysql_password: "{{ mysql_root_password }}"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-phpmyadmin/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[geerlingguy.repo-remi](https://galaxy.ansible.com/buluma/geerlingguy.repo-remi)|[![Ansible Molecule](https://github.com/buluma/geerlingguy.repo-remi/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/geerlingguy.repo-remi/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/geerlingguy.repo-remi.svg)](https://github.com/shadowwalker/geerlingguy.repo-remi)|
|[geerlingguy.apache](https://galaxy.ansible.com/buluma/geerlingguy.apache)|[![Ansible Molecule](https://github.com/buluma/geerlingguy.apache/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/geerlingguy.apache/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/geerlingguy.apache.svg)](https://github.com/shadowwalker/geerlingguy.apache)|
|[geerlingguy.mysql](https://galaxy.ansible.com/buluma/geerlingguy.mysql)|[![Ansible Molecule](https://github.com/buluma/geerlingguy.mysql/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/geerlingguy.mysql/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/geerlingguy.mysql.svg)](https://github.com/shadowwalker/geerlingguy.mysql)|
|[buluma.php_versions](https://galaxy.ansible.com/buluma/php_versions)|[![Ansible Molecule](https://github.com/buluma/ansible-role-php_versions/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-php_versions/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-php_versions.svg)](https://github.com/shadowwalker/ansible-role-php_versions)|
|[geerlingguy.php](https://galaxy.ansible.com/buluma/geerlingguy.php)|[![Ansible Molecule](https://github.com/buluma/geerlingguy.php/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/geerlingguy.php/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/geerlingguy.php.svg)](https://github.com/shadowwalker/geerlingguy.php)|
|[geerlingguy.php-mysql](https://galaxy.ansible.com/buluma/geerlingguy.php-mysql)|[![Ansible Molecule](https://github.com/buluma/geerlingguy.php-mysql/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/geerlingguy.php-mysql/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/geerlingguy.php-mysql.svg)](https://github.com/shadowwalker/geerlingguy.php-mysql)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-phpmyadmin/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|all|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|all|

The minimum version of Ansible required is 2.4, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-phpmyadmin/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-phpmyadmin/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-phpmyadmin/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)

