# README

This repository is a documentation of sorts about my infrastructure and the way it took me to get to some point.

My old infrastructure uses a Hetzner server. There I use Proxmox to separate my services. Most of those were setup by hand and also a long time ago.

## Services

- [x] DNS - required
- [x] Nextcloud - required
- [ ] Mailserver - required
- [ ] Wireguard Overlay Mesh Network - required
- [ ] Jitsi - optional
- [ ] Matrix - optional

## Secrets

I will use Ansible-Vault files for any secrets and store them in an private repo. If this would be a more enterprise setup I would use something like Hashicorp Vault.

## Backups

In my old setup I use a Hetzner storage box to store my backups. I use BorgBackup to create a Backup of the configuration and the data. I will not have to make a backup of the configuation because of the use of Ansible. At least that is my goal.

I need to figure out a storage provider or some way of storing the backup data in my own way.

My current idea is to use the Wireguard VPN to store the data on a NAS in my local network.

## Documentation

### Matrix

For Matrix I will use the ansible playbook from this Github repo https://github.com/spantaleev/matrix-docker-ansible-deploy. I will not build my own playbook.
