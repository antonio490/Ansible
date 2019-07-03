
# ANSIBLE

Ours hosts file contains just one target device named  raspberry1:


    raspberry1 ansible_host=192.168.1.57 ansible_connection=ssh ansible_port=22 ansible_user=pi ansible_ssh_pass=******


### INSTALLATION

We must install ansible on the controller machine:

<code>
    $ sudo apt-get install ansible   

    $ sudo apt-get install sshpass
</code>


First run a simple ping to the target machine:

<code>
    $ ansible raspberry1 -m ping -i hosts
</code>

