
# ANSIBLE

Ansible control machine can only be Linux and not Windows
 - Windows machines can be targets of Ansible and thus be part of the automation (Ansble connects using winrm)

 

Ours hosts file contains just one target device named  raspberry1:

    # hosts file
    raspberry1 ansible_host=192.168.1.57 ansible_connection=ssh ansible_port=22 ansible_user=pi ansible_ssh_pass=******


### INSTALLATION

We must install ansible on the controller machine:

    $ sudo apt-get install ansible   
    
    $ sudo apt-get install sshpass


First run a simple ping to the target machine:

    $ ansible raspberry1 -m ping -i hosts


### ANSIBLE PLAYBOOKS

With ansible playbooks we can configure a set of instructions that we want our remnote devices to automatically execute.

- Execute commands
- Run a script
- Install a package
- Shutdown / Restart

    ### Simple ansible playbook.yml
    -
        name: Play 1
        hosts: localhost
        tasks:
            - name: Execute command's date
              command: date
            
            - name: Execute script on server
              script: test_script.sh
            
            - name: Install httpd service
              yum:
                name: httpd
                state: present


How to run a playbook:


- ansible

    $ ansible <hosts> -a <command>

    $ ansible all -a "/sbin/reboot"

    $ ansible <hosts> -m <module>

    $ ansible target1 -m ping

- ansible-playbook

    $ ansible-playbook <playbook file name>

    $ ansible-playbook playbook-webserver.yaml


### Ansible Modules

- System
    - User
    - Group
    - Hostname
    - Iptables
    - Make
    - Mount
    - Systemd

- Commands
    - Command
    - Expect
    - Raw
    - Script
    - Shell

- Files
    - Acl
    - Archive
    - Copy
    - File
    - Find
    - Replace
    - Stat
    - Template

- Database
    - Mongodb
    - MySQL
    - PostgreSQL

- Cloud
    - Amazon
    - Atomic
    - Azure
    - Docker
    - VMWare
    - RackSpace
    - Google

- Windows
    - Win_copy
    - Win_command
    - Win_file
    - Win_msi
    - Win_path
    - Win_ping

    ### Ansible module example
    -
        name: Setup web server on all nodes
        hosts: web_nodes
        tasks:
            -
            name: Update entry into /etc/resolv.conf
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'

            -
            name: Create a new user
            user:
                name: web_user
                uid: 1040
                group: developers

            -
            name: Execute a script
            script: /tmp/install_script.sh

            -
            name: Start httpd service
            service:
                name: httpd
                state: present


### Loops

    -
        name: Install required packages
        hosts: localhost
        vars:
            packages:
            - httpd
            - binutils
            - glibc
            - ksh
            - libaio
            - libXext
            - gcc
            - make
            - sysstat
            - unixODBC
            - mongodb
            - nodejs
            - grunt

        tasks:
            -
            yum: name="{{ item }}" 
                state=present
            
            with_items: '{{ packages }}'
     
