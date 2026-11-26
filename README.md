# Labo 0 — Déploiement d'un Serveur Linux Rocky 9.8 sous KVM

## Objectif
Déployer un serveur Linux Rocky Linux 9.8 Minimal en machine virtuelle KVM, le configurer, créer des utilisateurs et valider l'accès distant en SSH depuis Kali Linux.

## Environnement
- Hôte : Kali Linux (operator00)
- Hyperviseur : KVM / libvirt / virt-manager
- VM : rocky-srv
- OS : Rocky Linux 9.8 (Blue Onyx) Minimal
- Réseau : NAT libvirt - 192.168.122.0/24
- IP du serveur : 192.168.122.17/24

## Caractéristiques de la VM
- 2 vCPU
- 2 Go RAM
- 20 Go disque
- Interface : enp1s0
- Virtualisation : kvm

## Étapes réalisées

### 1. Création de la VM
cmd:
virt-install --name rocky-srv --memory 2048 --vcpus 2 \
--disk size=20 --os-variant rocky9 --network default \
--graphics spice --location /home/operator00/Telechargements/Rocky-9.8-x86_64-minimal.iso \
--extra-args "console=ttyS0"


### 2. Installation
Installation en mode Minimal via Anaconda, choix du disque 20 Go, création du mot de passe root, reboot.

### 3. Configuration post-install
hostnamectl set-hostname rocky-srv
systemctl enable --now sshd
firewall-cmd --add-service=ssh --permanent
firewall-cmd --reload

### 4. Vérification
hostnamectl
ip a
cat /etc/os-release

Résultat :
- Static hostname: rocky-srv
- OS: Rocky Linux 9.8 (Blue Onyx)
- IP: 192.168.122.17/24 sur enp1s0

### 5. Création des utilisateurs
useradd -m natasha
useradd -m saara
echo "redhat" | passwd --stdin natasha
echo "redhat" | passwd --stdin saara
id natasha
id saara

Résultat :
- uid=1000(natasha) gid=1000(natasha)
- uid=1001(saara) gid=1001(saara)

### 6. Test SSH depuis Kali
ssh natasha@192.168.122.17
whoami
hostname
pwd

Résultat : connexion réussie -> [natasha@rocky-srv ~]$, dossier /home/natasha

## Preuves / Captures
- 01-commande-virt-install.png : Création de la VM
- 02-menu-boot-rocky.png : Menu de boot Rocky
- 03-installation-progress.png : Installation en cours
- 04-installation-complete.png : Installation terminée
- 05-first-login.png : Premier login
- 06-verification-systeme.png : hostnamectl + ip a + os-release
- 07-create-2-users.png : Création natasha et saara
- 08-test-ssh-depuis-kali.png : Connexion SSH réussie depuis Kali

## Compétences validées
- Virtualisation KVM / libvirt
- Installation Rocky Linux 9
- Administration : hostnamectl, useradd, firewalld
- Réseau : IP, NAT, SSH
- Documentation technique par captures

## Auteur
Labo réalisé par operator00 - Novembre 2026

