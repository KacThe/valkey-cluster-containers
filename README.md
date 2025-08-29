# Valkey Cluster Ansible Playbook

> **Warning: Running the playbook restarts Valkey clusters and causes loss of all data!**

---

## Quick Start

### Run the playbook on test environment
ansible-playbook -i hosts/test/inventory playbook.yml

### Create clusters on test environment
ansible-playbook -i hosts/test/inventory playbook.yml --diff -e create_clusters=true

### Run the playbook on production environment
ansible-playbook -i hosts/prod/inventory playbook.yml

### Create clusters on production environment
ansible-playbook -i hosts/prod/inventory playbook.yml --diff -e create_clusters=true

---

## Checking Clusters Example

You can check cluster status with Valkey CLI:
valkey-cli -h 192.168.0.1 -p 6390 -a pass cluster info

---

## How It Works

- Running the **test** playbook provisions 3 clusters, each with 3 nodes, all on a single virtual machine.
- Running the **production** playbook provisions 3 clusters distributed across 3 virtual machines, each cluster having 6 nodes (2 nodes per VM).
- The `prod_create_clusters.yml` task creates clusters with the flag `--cluster-replicas 1`, which means each cluster has 3 master nodes and 3 slave nodes for high availability and fault tolerance.
- The test environment creates 3 master nodes per cluster, running faster but without replication.

---

## Prerequisites

- `valkey-tools` must be installed on target machines:
sudo apt install valkey-tools

- Setup SSH key exchange with target machines for passwordless authentication to enable automatic execution.
- Alternatively, when running playbook with password prompts use flags:
ansible-playbook -i hosts/test/inventory playbook.yml --diff -e create_clusters=true -kK
  - Ensure the user has `sudo` access **without a password prompt** on the target machines.

---

## Managing Secrets

- Store the cluster password securely in `group_vars/all/secret.yml`.
- It's **strongly recommended** to encrypt this file using Ansible Vault:
ansible-vault encrypt group_vars/all/secret.yml
- Ansible will automatically decrypt the file during playbook execution and supply the password to Valkey clusters.

---

## Notes

- Always backup cluster data if needed before running the playbook to avoid data loss.
- Adjust inventories (`hosts/test/inventory`, `hosts/prod/inventory`) to match your environment.
- For questions or issues, please create an issue or contact the repository maintainer.

---

Thank you for using this playbook to automate and manage your Valkey clusters efficiently!
