# ansible-story

Ansible playbook for deploying and managing a **Story L1 mainnet node**
(Story consensus + Story Geth) on a dedicated server.

---

## Requirements

- Ansible 2.14+
- SSH access to the server
- Ubuntu / Debian-based Linux
- Root access for initial server preparation

---

## Edit `inventory.yml`

Edit `inventory.yml` and specify:
- Server IP address
- SSH user
- System user for running Story services

Example:

```yaml
dedicated:
  hosts:
    hetzner_story_ansible_S-1:
      ansible_host: 77.42.95.114
      ansible_user: root
      system_users:
        - state: present
          name: story_main

# MAINNET
story_mainnets:
  hosts:
    story_main:
      ansible_host: 77.42.95.114
      ansible_user: story_main
      type: relayer_mainnet
      prepare: true
```

---

## Deploy node

### Prepare server (run as root)

Creates system users, installs dependencies, configures firewall and system settings.

```bash
ansible-playbook main.yml -t prepare -e "target=hetzner_story_ansible_S-1"
```

### Install Story node (run as story user)

Installs Story, Story Geth, Cosmovisor, initializes configs and systemd services.

```bash
ansible-playbook main.yml -t install -e "target=story_main"
```

---

## Start services and read logs

### Story Geth
```bash
systemctl start story-geth_mainnet
journalctl -f -u story-geth_mainnet
```

### Story consensus node
```bash
systemctl start story_mainnet
journalctl -f -u story_mainnet
```

---

## Additional Ansible run options

Run individual steps if needed:

### Sync system users
```bash
ansible-playbook main.yml -t sync_users -e "target=hetzner_story_ansible_S-1"
```

### Reconfigure Story node
```bash
ansible-playbook main.yml -t configure -e "target=story_main"
```

### Prepare Start SYSTEMD for all Story services
```bash
ansible-playbook main.yml -t start -e "target=story_main"
```

---

## Troubleshooting

Check service status:
```bash
systemctl status story_mainnet
systemctl status story-geth_mainnet
```

View recent logs:
```bash
journalctl -u story_mainnet -n 100 --no-pager
journalctl -u story-geth_mainnet -n 100 --no-pager
```
