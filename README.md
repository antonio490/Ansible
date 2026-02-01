
# ANSIBLE

Ansible control machine can only be Linux and not Windows
 - Windows machines can be targets of Ansible and thus be part of the automation (Ansible connects using winrm)



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
     
## Roles

The easy way to create a role is by typing the command 

    $ ansible-galaxy init mysql-db 

After this we could see that it has been created a role with an structure with the next folders:

    - roles
        - mysql-db
            - defaults
            - handlers
            - meta
            - tasks
              - main.yml
            - tests
            - vars
  
  ## Asynchronous actions

  - Run a process and check on it later
  - Run multiple processes at once and check on them later
  - Run processes and forget about them.

    tasks:
        - command: /opt/monitor_web_app.py
          async: 360       # how long to run?
          poll: 60         # How frequently to check? default 10 seconds 
          register: webapp_result

        - name: Check status of tasks
          async_status: jid={{ webapp_result.ansible_job_id }}
          register: job_result
          until: job_result.finished
          retries. 30

## Vault

Encript our inventory.txt file so nobody can see ouu credentials:

    ansible-vault encrypt inventory.txt

    ansible-playbook playbook.yml -i inventory.txt -ask-vault-pass

    ansible-playbook playbook.yml -i inventory.txt -vault-password-file ~./vault_pass.txt

    ansible-vault view inventory.txt


## Ad-hocs examples

An Ansible ad hoc command uses the /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes. ad hoc commands are quick and easy, but they are not reusable. So why learn about ad hoc commands? ad hoc commands demonstrate the simplicity and power of Ansible.

### Files


    $ ansible localhost -m ansible.builtin.copy -a "src=/etc/hosts dest=/tmp/hosts"

    $ ansible localhost -m ansible.builtin.file -a "dest=/tmp/hosts state=absent"

    $ ansible localhost -m ansible.builtin.file -a "dest=/tmp/directorios mode=755 owner=antonio group=antonio state=directory"
    localhost | CHANGED => {
        "changed": true,
        "gid": 1000,
        "group": "antonio",
        "mode": "0755",
        "owner": "antonio",
        "path": "/tmp/directorios",
        "size": 4096,
        "state": "directory",
        "uid": 1000
    }


### Services

    $ ansible localhost -m ansible.builtin.service -a "name=ssh state=started"

    $ ansible localhost -m ansible.builtin.service -a "name=nginx state=restarted"

### Packages

    $ ansible localhost -m ansible.builtin.apt -a "name=acme state=latest"

    $ ansible localhost -m ansible.builtin.apt -a "name=acme state=absent"

    $ ansible localhost -m ansible.builtin.apt -a "name=acme state=present"
