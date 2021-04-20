My dev tools 
=========

This role installs the tools that compose my development environment, that is a vagrant box (ubuntu).
It installs the following tools:

- terraform
- aws-cli
- packer
- docker-dive
- ansible
- ansible-lint
- yamllint
- molecule
- test-infra
- task
- buildpack
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
    terraform_version: 0.14.9
    awscli_version: 2.0.30
    taskfile_version: 3.2.2
    docker_dive_version: 0.9.2



Example
----------------

./ansible/requirements.yml
    
    # install docker daemon
    - src: https://github.com/nickjj/ansible-docker.git
    # install dev tools
    - src: https://github.com/nicolimo86/ansible-infra-tools.git


./ansible/main.yml
    ---
    - hosts: all
      become: true
      roles:
        - ansible-infra-tools

./Vagrantfile

    Vagrant.configure("2") do |config|
      config.vm.define :vm1 do |vm1|
          vm1.vm.box = "ubuntu/bionic64"
          vm1.vm.provider "virtualbox" do |box|
              box.memory = 4086 
          end
          vm1.vm.provision :hosts, :sync_hosts => true
          vm1.vm.synced_folder "~/workspace", "/workspace"
          vm1.disksize.size = '15G'
      end
      config.vm.provision "ansible_local" do |ansible|
        ansible.verbose = "v"
        ansible.install_mode = "pip3"
        ansible.galaxy_role_file = "ansible/requirements.yml"
        ansible.galaxy_roles_path = "./ansible/roles"
        ansible.playbook = "ansible/playbook.yml"
      end
    end

Create and configure the VM:

    vagrant up

VSCode configuration
----------------

Install Remote Development plugin.
Launch VS Code Quick Open (Ctrl+P), paste the following command, and press enter.
    
    ext install ms-vscode-remote.vscode-remote-extensionpack

Add Vagrant host to the ssh config:
    
    vagrant ssh-config >> ~/.ssh/config

Connect to the Vagrant host from inside VSCode:
    
    F1 -> Remote-SSH: Connect to Host...
