
# ANSIBLE

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

    # Simple ansible playbook.yml
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

