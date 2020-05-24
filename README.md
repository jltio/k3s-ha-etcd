# Cluster k3s HA

## Description du projet

Ce projet est un POC. Le code et l'implémentation sont améliorables.

En se basant sur les projets Github et documentations suivants:

* [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
* [K3S Ansible](https://github.com/rancher/k3s-ansible)
* [Exemple Haproxy](https://gitlab.com/xavki/presentations-kubernetes/-/tree/master/37-kubspray-haproxy)

L'objectif est de créer un cluster haute disponibilité avec la version Kubernetes k3s fourni par Rancher.

Dans ce code, 6 serveurs sont utilisés.

* 1 CPU par serveur
* 1G de RAM par serveur
* CentOS 7

Les 3 premiers serveurs seront utilisés pour

* La base de données etcd 3.x en cluster et haute disponibilité + connexions client/serveur/peer en TLS
* Le Controle plane Kubernetes k3s se connectant au cluster etcd. Ces serveurs master k3s ne seront pas vu dans la liste des nodes.

Les 3 derniers serveurs seront utilisés pour

* Node k3s
* Avec Docker

Au final, le cluster sera disponible uniquement avec

* coredns
* traefik ingress controler

Dans k3s, la gestion des certificats TLS se gère automatiquement.

## Lancement des Playbooks Ansible

Ce projet fonctionne sur Ansible version 2.9

Avant de lancer, se connecter sur les 6 serveurs via SSH avec une clé SSH ne demandant pas de mot de passe (sans passphrase ou ssh-agent)

Fonctionne uniquement sur CentOS 7

```bash
ansible-playbook 00-prereq.yml
```

Puis reboot des serveurs

```bash
ansible-playbook 01-etcd.yml
```

```bash
ansible-playbook 02-k3s-masters.yml
```

```bash
ansible-playbook 03-haproxy.yml
```

```bash
ansible-playbook 04-k3s-nodes.yml
```

```bash
ansible-playbook 99-delete-data.yml
```
