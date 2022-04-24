# README

This repository is a documentation of sorts about my infrastructure and the way it took me to get to some point.

My old infrastructure uses a Hetzner server. There I use Proxmox to separate my services. Most of those were setup by hand and also a long time ago.

## Services

- [x] DNS - required
- [x] Nextcloud - required
- [x] Mailserver - required (used Mailcow Solution)
- [x] Multi Tenant Webserver (partly done )
- [ ] Wireguard Overlay Mesh Network - required
- [ ] Jitsi - optional
- [x] Matrix - optional

## Secrets

I will use Ansible-Vault files for any secrets and store them in an private repo. If this would be a more enterprise setup I would use something like Hashicorp Vault. I wont provide any further documentation just yet.

## Backups

In my old setup I use a Hetzner storage box to store my backups. I use BorgBackup to create a Backup of the configuration and the data. I will not have to make a backup of the configuation because of the use of Ansible. At least that is my goal.

I need to figure out a storage provider or some way of storing the backup data in my own way.

My current idea is to use the Wireguard VPN to store the data on a NAS in my local network. This would also provide me with the option to use additional storage locations in the future.

## Documentation

### General thoughts

The SSHd PasswordAuthentication is set to no on each host. This is done with the deploy-base-configuration.yml and ensures a base security. Some of my SSH Keys are deployed during this step as well.

### Matrix

For Matrix I will use the ansible playbook from this Github repo https://github.com/spantaleev/matrix-docker-ansible-deploy. I will not build my own playbook.

### Mailcow - Mailserver

The Mailcow installation was done [manually](https://mailcow.github.io/mailcow-dockerized-docs/i_u_m/i_u_m_install/) because I had to change my email server on short notice. I was not ready to use my playbook. The Mailcow Ansible Role looks promising but I did not want to risk any problems during the migration.

## TODO

- Think about how I can configure the ipv6 address to the main network interface of an host. This configuration depends on the provider I use.
- ~~Perform a namend-checkzone after the template process to make sure that the zonefile can be loaded~~
- Make FastCGI PHP work. Right now it does not work as I do not have a need for it. The groundwork is done though.
- Test Mailcow deployment on another host to make sure my thoughts are correct.