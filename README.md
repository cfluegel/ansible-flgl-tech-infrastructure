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
- [ ] Dockerize the redirect and websites

## Secrets

I will use Ansible-Vault files for any secrets and store them in an private repo. If this would be a more enterprise setup I would use something like Hashicorp Vault. I wont provide any further documentation just yet.

Secrets are still held in a separate repository. The repository contains yml files based on the FQDN of an particular host. See the inventory file for reference. The files are also encrypted with ansible-vault, besides being stored in a private repo.

## Backups

In my old setup I use a Hetzner storage box to store my backups. I use BorgBackup to create a Backup of the configuration and the data. I will not have to make a backup of the configuation because of the use of Ansible. At least that is my goal.

~~I need to figure out a storage provider or some way of storing the backup data in my own way.~~ Even though Hetzner had some [trouble](https://www.bleepingcomputer.com/news/security/hetzner-lost-customer-data-and-gave-20-as-compensation/) with their storage boxes, I still give it a try. I dont rely on their snapshot storage, but I rather keep extra backups around and I will look into other solutions as well.

~~My current idea is to use the Wireguard VPN to store the data on a NAS in my local network. This would also provide me with the option to use additional storage locations in the future.~~ This idea is still in my mind but postponed due to the higher energy prices. I dont have a working solar setup to reduce the costs but that is something I will have to setup first

## Documentation

### Docker and why I switched to it...
I am in the process of converting my Nextcloud and Webserver playbooks to docker based deployments. Why? Yeah, that is indeed a good question. The apache+suexec+php deployment works okay and it still allows me to provide access to other persons, but I feel that the container based deployment might be easier in the future. I am still new to "how to deploy and work with container setups" which is why this is also a way for me to learn more about the topic.

I decided against roles for now. The ```deploy-docker-base.yml``` is the starting point. All other ``deploy-docker-*.yml``` require the steps from that playbook. Keep that in mind. I will convert the different ```deploy-docker-*.yml```files into roles but it is easier to have deployable playbooks without the extra layer of complexity right now. Thanks to Christian from [goNeuland](https://goneuland.de/) for creating such excellent instructions about docker and containerized deployments. Go check his website out and maybe donate some money.

My soon to be filled [blog](https://blog.flgl.tech) is setup as a static website docker container. I utilize Github Workflow to autogenerate a new docker image on every new git push to the gituhb repository. When used in conjunction with Watchtower, which checks and recreate a docker deployment, my site will be updated in regular intervals. Although I am still debating if a simple cronjob might not be better suited...

### Security

The SSHd PasswordAuthentication is set to no on each host. This is done with the deploy-base-configuration.yml and ensures a base security. Some of my SSH Keys are deployed during this step as well.

The docker host is monitored by local [Crowdsec](https://www.crowdsec.net/) instance and secured with the firewall bouncer. Additionally, the traefik proxy uses the same list to stop unwanted access on the http/https route. I am sure the firewall bouncer, which I installed later, would be enough but I am still in the process of understanding Crowdsec :) I might add crowdsec to each and every host and let them talk to eachother.

### Matrix

For Matrix I will use the ansible playbook from this Github repo https://github.com/spantaleev/matrix-docker-ansible-deploy. I will not build my own playbook, but I have to find a way to integrate it into my 

### Mailcow - Mailserver

The Mailcow installation was done [manually](https://mailcow.github.io/mailcow-dockerized-docs/i_u_m/i_u_m_install/) because I had to change my email server on short notice. I was not ready to use my playbook. The Mailcow Ansible Role looks promising but I did not want to risk any problems during the migration.

Backup is also configued manually. The backup is done by the mailcow backup program. I keep 30 days worth of backup files on a shared storage drive.

## TODO

- ~~Think about how I can configure the ipv6 address to the main network interface of an host. This configuration depends on the provider I use.~~
  I might have found a way for my current provider, which should work for others as well but I dont know for sure because I have all eggs in one basket right now.
- ~~Perform a namend-checkzone after the template process to make sure that the zonefile can be loaded~~
- ~~Make FastCGI PHP work. Right now it does not work as I do not have a need for it. The groundwork is done though.~~
- Test Mailcow deployment on another host to make sure my thoughts are correct.