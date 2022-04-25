
# Домашнее задание к занятию "5.2. Применение принципов IaaC в работе с виртуальными машинами"



## Задача 1

- Опишите своими словами основные преимущества применения на практике IaaC паттернов.

Iaac позволяет автоматизировать процесс развертывание рабочих сред и, соответственно ускорить процесс вывода и масштабируемости  инфраструктуры для разработки и тестирования. Свойство идемпотентности, при этом, позволяет многократно повторять процесс развертывания и масштабирования имея заранее подготовленную конфигурацию инфраструктуры без наличия несоответствий в настройках отдельных сегментов инфраструктуры (своего рода стандартизация ячеек инфраструктуры).  
Принципы IaC позволяют не фокусироваться на рутине, а заниматься более важными задачами. Автоматизация инфраструктуры позволяет эффективнее использовать существующие ресурсы. Также автоматизация позволяет минимизировать риск возникновения человеческой ошибки  

- Какой из принципов IaaC является основополагающим?  

CI\CD – continuous intregration\continuous delivery непрерывная интеграция и непрерывная доставка, позволяющие в автоматическом режиме делать частые слияния веток разработки в одну общую ветку и таким образом делать выкаты ПО или IaaC кода часто, что позволит обнаруживать неполадки на ранних этапах и ускорять выкатку разработанного кода.  


## Задача 2

- Чем Ansible выгодно отличается от других систем управление конфигурациями?

Ansible позволяет подключаться к уже существующей структуре при помощи ssh, без необходимости установки доп. ПО. Еще одной отличительной особенностью является наличие множества разработанных модулей в открытом доступе.  

- Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?

Метод Push мне кажется предпочтительным, что подкрепляется популярностью ansible, который как раз является системой с push, т.к удобнее управлять конфигурацией всей инфраструктуры (или ее части) из одного места, что, так же, создает единую точку отказа.  

## Задача 3

Установить на личный компьютер:

- VirtualBox
- Vagrant
- Ansible

*Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.*  

```bash
root@valyan-pc:~# vagrant --version  
Vagrant 2.2.6  
root@valyan-pc:~# vboxmanage --version  
6.1.32_Ubuntur149290  
root@valyan-pc:~# ansible --version  
ansible 2.9.6  
  config file = /etc/ansible/ansible.cfg  
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']  
  ansible python module location = /usr/lib/python3/dist-packages/ansible  
  executable location = /usr/bin/ansible  
  python version = 3.8.10 (default, Mar 15 2022, 12:22:08) [GCC 9.4.0]  
root@valyan-pc:~# ^C  
```

## Задача 4 (*)

Воспроизвести практическую часть лекции самостоятельно.

- Создать виртуальную машину.
- Зайти внутрь ВМ, убедиться, что Docker установлен с помощью команды  
```bash
root@valyan-pc:~/vagrant# vagrant up
Bringing machine 'server1.netology' up with 'virtualbox' provider...
==> server1.netology: Clearing any previously set network interfaces...
==> server1.netology: Preparing network interfaces based on configuration...
    server1.netology: Adapter 1: nat
    server1.netology: Adapter 2: bridged
==> server1.netology: Forwarding ports...
    server1.netology: 22 (guest) => 2222 (host) (adapter 1)
==> server1.netology: Running 'pre-boot' VM customizations...
==> server1.netology: Booting VM...
==> server1.netology: Waiting for machine to boot. This may take a few minutes...
    server1.netology: SSH address: 127.0.0.1:2222
    server1.netology: SSH username: vagrant
    server1.netology: SSH auth method: private key
    server1.netology: 
    server1.netology: Vagrant insecure key detected. Vagrant will automatically replace
    server1.netology: this with a newly generated keypair for better security.
    server1.netology: 
    server1.netology: Inserting generated public key within guest...
    server1.netology: Removing insecure key from the guest if it's present...
    server1.netology: Key inserted! Disconnecting and reconnecting using new SSH key...
==> server1.netology: Machine booted and ready!
==> server1.netology: Checking for guest additions in VM...
==> server1.netology: Setting hostname...
==> server1.netology: Configuring and enabling network interfaces...
==> server1.netology: Mounting shared folders...
    server1.netology: /vagrant => /root/vagrant
```

```bash
root@server1:~# cat /etc/*release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.3 LTS"
NAME="Ubuntu"
VERSION="20.04.3 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.3 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
root@server1:~# ip a | grep inet | grep 192
    inet 192.168.0.29/24 brd 192.168.0.255 scope global dynamic eth1
root@server1:~# hostname -f
server1.netology
root@server1:~# free
              total        used        free      shared  buff/cache   available
Mem:        1004800      123576      328168         956      553056      724652
Swap:       2009084           0     2009084
root@server1:~# 

agrant@server1:~$ exit
logout
Connection to 127.0.0.1 closed.
root@valyan-pc:~/vagrant# vagrant halt
==> server1.netology: Attempting graceful shutdown of VM...
root@valyan-pc:~/vagrant# vagrant status
Current machine states:

server1.netology          poweroff (virtualbox)

The VM is powered off. To restart the VM, simply run `vagrant up`
root@valyan-pc:~/vagrant# vagrant destroy
    server1.netology: Are you sure you want to destroy the 'server1.netology' VM? [y/N] y
==> server1.netology: Destroying VM and associated drives...
root@valyan-pc:~/vagrant# vagrant status
Current machine states:

server1.netology          not created (virtualbox)

The environment has not yet been created. Run `vagrant up` to
create the environment. If a machine is not created, only the
default provider will be shown. So if a provider is not listed,
then the machine is not created for that environment.
root@valyan-pc:~/vagrant# 
```

```bash
root@valyan-pc:~/vagrant# cat Vagrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :
ISO = "~/vagrant/ubuntu-20.04"
NET = "192.168.10."
DOMAIN = ".netology"
HOST_PREFIX = "server"

INVENTORY_PATH = "../.ansible/inventory"



servers = [
 {
 :hostname => HOST_PREFIX + "1" + DOMAIN,
 :ip => NET + "11",
 :ssh_host => "20011",
 :ssh_vm => "22",
 :ram => 1024,
 :core => 1
 }
] 
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.


    config.vm.synced_folder ".", "/vagrant", disabled: false
    servers.each do |machine|
      config.vm.define machine[:hostname] do |node|
        node.vm.box = ISO
        node.vm.hostname = machine[:hostname]
        node.vm.provider "virtualbox" do |vb|
          vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
          vb.customize ["modifyvm", :id, "--cpus", machine[:core]]
          vb.name = machine[:hostname]
	node.vm.provision "ansible" do |setup|
 	  setup.inventory_path = INVENTORY_PATH
 	  setup.playbook = "../.ansible/playbook.yml"
 	  setup.become = true
 	  setup.extra_vars = { ansible_user: 'vagrant' }
	end
        end
      end
    end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  #  config.vm.network "private_network"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end



root@valyan-pc:~/.ansible# ls
ansible.cfg  hosts.ini  inventory  playbook.yml  tmp
root@valyan-pc:~/.ansible# cat ansible.cfg
[defaults]
inventory=./inventory
deprecation_warnings=False
command_warning=False
ansible_port=22
interpreter_python=/usr/bin/python3
root@valyan-pc:~/.ansible# cat inventory
[nodes:children]
manager

[manager]
server1.netology ansible_host=127.0.0.1 ansible_port=2222 ansible_user vagrant
root@valyan-pc:~/.ansible# cat hosts.ini
127.0.0.1

root@valyan-pc:~/.ansible# cat playbook.yml
---

- hosts: nodes
  become: yes
  become_user: root
  remote_user: vagrant

  tasks:
- name: Create directory for ssh-keys
  file: state-directory mode=0700 dest=/root/.ssh/

- name: Adding rsa-key in /root./ssh/authorized_keys
  copy: src=~/.ssh/id_rsa.pub dest=/root/.ssh/authorized_keys owner=root mode=0600
  ignore_errors: yes

- name: Checking DNS
  command: host -t A google.com

- name: Installing tools
  apt: >
   package={{ item }}
   state=present
   update_cache=yes
  with_items:
  - git
  - curl
		
- name: Installing_docker
  shell: curl -fsSL get.docker.com -o get-docker.sh && chmod +x get-docker.sh && ./get-docker.sh

- name: Add the current user to docker group
  user: name=vagrant append=yes groups=docker


```

```bash
oot@valyan-pc:~/vagrant# vagrant up
Bringing machine 'server1.netology' up with 'virtualbox' provider...
==> server1.netology: Importing base box '~/vagrant/ubuntu-20.04'...
==> server1.netology: Matching MAC address for NAT networking...
==> server1.netology: Setting the name of the VM: server1.netology
==> server1.netology: Clearing any previously set network interfaces...
==> server1.netology: Preparing network interfaces based on configuration...
    server1.netology: Adapter 1: nat
    server1.netology: Adapter 2: bridged
==> server1.netology: Forwarding ports...
    server1.netology: 22 (guest) => 2222 (host) (adapter 1)
==> server1.netology: Running 'pre-boot' VM customizations...
==> server1.netology: Booting VM...
==> server1.netology: Waiting for machine to boot. This may take a few minutes...
    server1.netology: SSH address: 127.0.0.1:2222
    server1.netology: SSH username: vagrant
    server1.netology: SSH auth method: private key
    server1.netology: 
    server1.netology: Vagrant insecure key detected. Vagrant will automatically replace
    server1.netology: this with a newly generated keypair for better security.
    server1.netology: 
    server1.netology: Inserting generated public key within guest...
    server1.netology: Removing insecure key from the guest if it's present...
    server1.netology: Key inserted! Disconnecting and reconnecting using new SSH key...
==> server1.netology: Machine booted and ready!
==> server1.netology: Checking for guest additions in VM...
==> server1.netology: Setting hostname...
==> server1.netology: Configuring and enabling network interfaces...
==> server1.netology: Mounting shared folders...
    server1.netology: /vagrant => /root/vagrant
==> server1.netology: Running provisioner: ansible...
Vagrant has automatically selected the compatibility mode '2.0'
according to the Ansible version installed (2.9.6).

Alternatively, the compatibility mode can be specified in your Vagrantfile:
https://www.vagrantup.com/docs/provisioning/ansible_common.html#compatibility_mode

    server1.netology: Running ansible-playbook...
[WARNING]:  * Failed to parse /root/.ansible/inventory with yaml plugin: Syntax
Error while loading YAML.   did not find expected <document start>  The error
appears to be in '/root/.ansible/inventory': line 2, column 1, but may be
elsewhere in the file depending on the exact syntax problem.  The offending
line appears to be:  [nodes:children] manager ^ here
[WARNING]:  * Failed to parse /root/.ansible/inventory with ini plugin:
/root/.ansible/inventory:5: Expected key=value host variable assignment, got:
ansible_user
[WARNING]: Unable to parse /root/.ansible/inventory as an inventory source
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that
the implicit localhost does not match 'all'
[WARNING]: Could not match supplied host pattern, ignoring: server1.netology
ERROR! Syntax Error while loading YAML.
  found a tab character that violate indentation

The error appears to be in '/root/.ansible/playbook.yml': line 27, column 1, but may
be elsewhere in the file depending on the exact syntax problem.

The offending line appears to be:

  - curl

^ here
There appears to be a tab character at the start of the line.

YAML does not use tabs for formatting. Tabs should be replaced with spaces.

For example:
    - name: update tooling
      vars:
        version: 1.2.3
#    ^--- there is a tab there.

Should be written as:
    - name: update tooling
      vars:
        version: 1.2.3
# ^--- all spaces here.
Ansible failed to complete successfully. Any error output should be
visible above. Please fix these errors and try again.
```

```
docker ps
```