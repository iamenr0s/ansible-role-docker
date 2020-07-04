[![Build Status](https://travis-ci.com/enr0s/ansible-role-docker.svg?branch=master)](https://travis-ci.com/enr0s/ansible-role-docker)
![.github/workflows/molecule.yml](https://github.com/enr0s/ansible-role-docker/workflows/.github/workflows/molecule.yml/badge.svg)
[![quality](https://img.shields.io/ansible/quality/49594)](https://galaxy.ansible.com/enr0s/ansible-role-docker)
![LICENSE](https://img.shields.io/github/license/enr0s/ansible-role-docker)

Role Name
=========

Install docker on your Raspberry (64 bit architecture).

Role Variables
--------------

run_not_in_container - the variable is used to skip some tasks during molecule test. For example, the /etc/hosts file is crucial for Docker's linking system and it should only be manipulated manually at the image level, rather than the container level.

[https://docs.docker.com/network/links/#updating-the-etchosts-file]



Dependencies
------------

```
ansible-galaxy install enr0s.ansible_role_bootstrap
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
  ---
  - hosts: all
    roles:
      - {role: ansible-role-bootstra, run_not_in_container: True }
```

License
-------

Apache-2.0


Author Information
------------------

[https://blog.enros.me]
