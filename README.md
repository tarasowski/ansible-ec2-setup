To connect from a controller host (where Ansible is installed) to another target host (EC2 instance), you can follow these general steps:

1. **Install Ansible on the Controller Host:**
   Make sure Ansible is installed on your controller host. You can install it using a package manager like `apt`, `yum`, or `dnf` depending on your operating system.

   Example for Ubuntu:
   ```bash
   sudo apt update
   sudo apt install ansible
   ```

   Example for CentOS/RHEL:
   ```bash
   sudo yum install epel-release
   sudo yum install ansible
   ```

2. **Configure SSH Access:**
   Ensure that you have SSH access to the target host from your controller host. This involves having the SSH private key on the controller host and the corresponding public key added to the `~/.ssh/authorized_keys` file on the target host.

   Example:
   ```bash
   ssh-keygen -t rsa -b 2048
   ssh-copy-id user@your_target_host_ip
   ```
```bash
# create a .pub key from a .pem (private key)
ssh-keygen -y -f /path/to/your-key.pem > /path/to/your-key.pub
ssh-copy-id user@your_taret_host_ip
``` 
   
   Enrico:
   ```bash
   ssh key erzeugen:
   ssh-keygen -t rsa -b 4096 -C "Ihre-E-Mail@example.com"
   
   Pub key kopieren:
   ssh-copy-id -i /home/rico003/.ssh/vps_ec.pub root@173.111.44.567

   wenn fehler auftaucht:
   ssh-keygen -f "/home/rico003/.ssh/known_hosts" -R "173.249.46.248"
   ```

4. **Inventory File:**
   Create an inventory file that lists the target hosts you want to manage. This file typically named `inventory` can look like this:

   ```ini
   [target_hosts]
   target_host_ip ansible_ssh_user=user ansible_ssh_private_key_file=/path/to/private/key.pem
   ```

   Replace `target_host_ip` with the actual IP address of your target host, `user` with the SSH user, and `/path/to/private/key.pem` with the path to your private key.

5. **Test Connection:**
   Use the `ansible` command to test the connection to the target host:

   ```bash
   ansible -i inventory target_hosts -m ping
   ```

   If everything is set up correctly, you should see a successful ping response.

6. **Create and Run Ansible Playbooks:**
   Write Ansible playbooks to define the tasks you want to perform on the target hosts. Playbooks are written in YAML and define the desired state of the system.

   For example, create a playbook file (`example.yml`):

   ```yaml
   ---
   - name: Example Playbook
     hosts: target_hosts
     tasks:
       - name: Ensure Nginx is installed
         package:
           name: nginx
           state: present
       - name: Ensure Nginx is running
         service:
           name: nginx
           state: started
   ```

   Run the playbook with:

   ```bash
   ansible-playbook -i inventory example.yml
   ```

These are the basic steps to connect from a controller host to another EC2 instance using Ansible. Adjust the details based on your specific setup and requirements.
