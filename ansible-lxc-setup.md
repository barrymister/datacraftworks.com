
# Ansible LXC Setup and Configuration

This guide provides detailed instructions to set up an **Ansible LXC container** on Proxmox, install Ansible, and configure it for managing your infrastructure.

---

## **Step 1: Create the Ansible LXC on Proxmox**

1. **Log into the Proxmox Node:**
   ```bash
   ssh root@<proxmox_ip>
   ```

2. **Create the LXC Container:**
   Use the Proxmox Web UI or CLI to create a new container with the following settings:
   - **Name:** `ansible`
   - **OS Template:** Ubuntu 20.04 or later.
   - **Disk Size:** At least 10GB.
   - **Memory:** 512MB (minimum).
   - **CPU:** 1 core.

3. **Verify the LXC is Running:**
   ```bash
   pct list
   ```

---

## **Step 2: Access and Update the LXC**

1. **Enter the LXC:**
   ```bash
   pct exec <lxc_id> -- /bin/bash
   ```

2. **Update the System:**
   ```bash
   apt update && apt upgrade -y
   ```

---

## **Step 3: Install Ansible**

1. **Install Required Dependencies:**
   ```bash
   apt install -y python3 python3-pip sshpass
   ```

2. **Install Ansible:**
   - Use the system package manager:
     ```bash
     apt install -y ansible
     ```
   - Or, for the latest version, use `pip`:
     ```bash
     pip3 install ansible
     ```

3. **Verify Ansible Installation:**
   ```bash
   ansible --version
   ```

---

## **Step 4: Configure Ansible Environment**

1. **Create the Inventory File:**
   ```bash
   mkdir -p /etc/ansible
   nano /etc/ansible/hosts
   ```
   Add your Proxmox host or other managed systems:
   ```ini
   [proxmox]
   <proxmox_ip> ansible_user=root ansible_ssh_private_key_file=/root/.ssh/id_rsa
   ```

2. **Test Ansible Connectivity:**
   ```bash
   ansible -i /etc/ansible/hosts all -m ping
   ```

---

## **Step 5: Add a Non-Root User for SSH**

1. **Create a New User:**
   ```bash
   adduser barry
   ```

2. **Grant `sudo` Privileges:**
   ```bash
   usermod -aG sudo barry
   ```

3. **Configure SSH Access for `barry`:**
   - Edit the SSH configuration file:
     ```bash
     nano /etc/ssh/sshd_config
     ```
     Ensure the following:
     ```plaintext
     PermitRootLogin prohibit-password
     PasswordAuthentication yes
     AllowUsers barry
     ```

   - Restart SSH:
     ```bash
     systemctl restart sshd
     ```

4. **Test SSH Access:**
   ```bash
   ssh barry@<lxc_ip>
   ```

---

## **Step 6: Optional - Set Up SSH Key Authentication**

1. **Generate an SSH Key Pair:**
   ```bash
   ssh-keygen -t rsa -b 4096
   ```

2. **Copy the Public Key to the LXC:**
   ```bash
   ssh-copy-id barry@<lxc_ip>
   ```

3. **Disable Password Authentication (Optional):**
   ```bash
   nano /etc/ssh/sshd_config
   ```
   Update:
   ```plaintext
   PasswordAuthentication no
   ```
   Restart SSH:
   ```bash
   systemctl restart sshd
   ```

---

## **Step 7: Test Ansible Playbook**

1. **Create a Test Playbook:**
   ```bash
   nano test_playbook.yml
   ```
   Example content:
   ```yaml
   - name: Test Ansible
     hosts: all
     tasks:
       - name: Ping the target
         ping:
   ```

2. **Run the Playbook:**
   ```bash
   ansible-playbook -i /etc/ansible/hosts test_playbook.yml
   ```

---

Your **Ansible LXC container** is now set up and ready to automate your environment.

