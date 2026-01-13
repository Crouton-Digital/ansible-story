# ansible-story

```bash
ansible-playbook main.yml -t "prepare" -e "target=hetzner_story_ansible_S-1"
ansible-playbook main.yml -t "sync_users" -e "target=hetzner_story_ansible_S-1"
ansible-playbook main.yml -t "install" -e "target=story_main"
ansible-playbook main.yml -t "configure" -e "target=story_main"
ansible-playbook main.yml -t "start" -e "target=story_main"
```


systemctl start story_mainnet
journalctl -f -u story_mainnet

systemctl start story-geth_mainnet
journalctl -f -u story-geth_mainnet