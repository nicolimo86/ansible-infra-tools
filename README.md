Ansible infra tools
=========

This role installs the tools that compose my development environment.
It installs the following tools:

- packer
- docker-dive
- ansible
- ansible-lint
- yamllint
- molecule
- test-infra
- task
- python3
- pip3
- virtualenv


Requirements
------------

ansible

Role Variables
--------------

The following variables are available:

    packer_version: 1.7.0
    taskfile_version: 3.2.2
    docker_dive_version: 0.9.2



Example Playbook
----------------

    ---
    - hosts: all
      become: true
      roles:
        - ansible-infra-tools


