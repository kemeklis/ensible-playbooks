# Ansible Project for System Setup and Configuration

This project contains Ansible playbooks and roles for configuring various setups on Linux (Manjaro) and macOS systems. The roles handle the installation of essential packages, environment setup, and application configuration.

## Project Structure

```plaintext
.
├── README.md                 # Project documentation
├── ansible.cfg               # Ansible configuration file
├── inventory.ini             # Inventory for target hosts
├── group_vars/               # Group-specific variables
│   ├── all.yml
│   ├── macos.yml
│   ├── manjaro.yml
│   └── vault.yml             # Encrypted secrets file
├── host_vars/                # Host-specific variables
│   ├── vyteniss-air.yml
│   ├── vyteniss-mbp.yml
│   ├── xvytis-lenovo_t430.yml
│   └── xvytis-macpro51.yml
├── playbooks/                # Main playbooks for different systems
│   ├── lenovo_t430_setup.yml
│   ├── macpro51_setup.yml
│   ├── manjaro_setup.yml
│   ├── mbp_setup.yml
│   └── site.yml              # Master playbook
└── roles/                    # Custom roles for configurations
    ├── common/ssh_key_upload
    ├── macos/
    ├── manjaro/
    └── ...other roles
```

---

## Initial Configuration on Target Machine

After a fresh OS install on a Linux machine, perform the following initial steps to prepare the machine for remote Ansible playbook execution:

1. **Install and Configure OpenSSH:**
   ```bash
   sudo pacman -S openssh  # For Manjaro Linux
   sudo systemctl start sshd
   sudo systemctl enable sshd
   ```

2. **Enable Sudo Access for the Ansible User:**
   Add your user (e.g., `xvytis`) to the sudoers file to grant sudo privileges:
   ```bash
   sudo usermod -aG wheel username
   ```
   Then configure the sudoers file by running:
   ```bash
   sudo visudo
   ```
   Ensure that the line `%wheel ALL=(ALL) ALL` is uncommented.

3. **SSH Key Setup:**
   Generate an SSH key pair (if not already generated):
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
   Copy the SSH public key to the target machine to allow Ansible access:
   ```bash
   ssh-copy-id username@target_machine_ip
   ```

4. **Install Python:**
   Ensure Python is installed as it’s required for Ansible.

---

## Running the Playbooks

After configuring the target machine, follow these steps to run the Ansible playbooks:

1. **Install Ansible on the Control Node:**
   ```bash
   sudo pacman -S ansible  # For Manjaro Linux
   ```

2. **Inventory Setup:**
   The inventory file (`inventory.ini`) should list all target machines. Here’s an example format:
   ```plaintext
   [all]
   xvytis-macpro51 ansible_host=192.168.1.100 ansible_user=xvytis
   ```

3. **Encrypt Sensitive Data:**
   Use Ansible Vault to store sensitive information such as passwords in `group_vars/vault.yml`.
   Encrypt the file:
   ```bash
   ansible-vault encrypt group_vars/vault.yml
   ```

4. **Edit Sensitive Data:**
   Use Ansible Vault to store sensitive information such as passwords in `group_vars/vault.yml`.
   Encrypt the file:
   ```bash
   ansible-vault edit group_vars/vault.yml
   ```

5. **Execute Playbooks:**
   Run the playbook for a specific machine:
   ```bash
   ansible-playbook -i inventory.ini playbooks/manjaro_setup.yml --ask-vault-pass -v
   ```

---

## Roles and Playbooks

Each role within `roles/` is responsible for specific tasks, such as setting up a development environment, configuring system preferences, or deploying applications like Obsidian or Docker-based applications.

---

## Edit the Vault File

Sensitive credentials are stored in `group_vars/vault.yml` and encrypted using Ansible Vault. To update the file:

1. **Edit the Vault File:**
   ```bash
   ansible-vault edit group_vars/vault.yml
   ```
---

## Notes

- Make sure to keep the inventory, playbooks, and roles updated as you add or modify configurations.
- Regularly backup and version-control the configurations and encrypted vaults.

For detailed configurations of each role, refer to the `tasks` and `defaults` directories within each role.
