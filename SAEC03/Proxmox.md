Valentin Tugot
# Installation GOAD sous proxmox

Après l'installation le schéma du réseau est le suivant:

![schema](img/schema.png)

## 1. Installation de pfSense

L'utilisation de pfSense permet d'avoir un pare-feu virtuel qui nous permet de gérer la connexion entre notre proxmox et l'environnement GOAD tout en ayant de la sécurité et différents services utiles tel que le DHCP ou le DNS.<br>
Le pare-feu pfSense permettra également d'accéder au machine de GOAD avec un accès OpenVPN.
<br>

Dans un premier temps il faut configurer 3 bridges sur le proxmox pour réaliser les différents réseau (WAN, LAN, VLAN10):

![bridge](img/1.png)
<br>

Une fois les bridges crée, il faut créer la VM avec pfSense.<br>
J'ai récupéré l'iso pfsense sur le site officiel, j'ai importé l'iso sur proxmox puis j'ai crée la VM.<br>
Afin d'accéder au pare-feu, on se connecte en SSH sur le proxmox en faisant un forward de l'IP du pfSense sur un port de ma machine locale avec la commande suivante:
```bash
ssh -L 8082:192.168.1.2:80 root@10.202.100.200
```

### Configuration des interfaces

Une fois la VM installé j'ai configuré les interfaces du pare-feu:
<br>
J'ai assigné chaque interface du pare-feu à un bridge du proxmox crée précedemment

![assignement](img/2.png)


Pour les interfaces, l'adressage est le suivant:
- WAN: 10.0.0.2/30
- LAN: 192.168.1.2/24
- VLAN10: 192.168.10.1/24

### Configuration du filtrage

Pour les règles de filtrage, on applique par défaut une règle qui bloque tout les paquets et on autorise seulement le trafic utile au fonctionnement de notre installation. <br>

__Interface WAN:__

![wan-fw](img/wan-fw.png)

Pour l'interface WAN on ajoute 3 règles:
- L'accès au VPN sur le port 2137
- L'accès au l'interface pfSense en SSH
- Et en HTTP
<br>

__Interface LAN:__

![lan-fw](img/lan-f<.png)

Pour l'interface LAN on configure 1 règles (la 1ère règles et configuré automatiquement par pfSense). La règle à ajouter permet aux machines sur le réseau LAN d'accèder à Internet et à tout les autres services.
<br>

__Interface VLAN10:__

![vlan-fw](img/vlan-fw.png)

Sur l'interface du VLAN les 3 règles ajoutés permettent:
- D'autoriser le trafic entrant sur le VPN
- D'autoriser les machines sur le VLAN d'accéder au DNS (UDP/53)
- D'autoriser le trafic des machines du VLAN10 vers les machines du réseau LAN et du VLAN10 entre elles.
<br>

Afin que les machines puissent d'accéder à internet il faut aussi configurer certaines règles sur le proxmox directement via SSH avec les commandes suivantes:

```bash
# Activer le routage
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
# Autorise l'ICMP
iptables -t nat -A PREROUTING -i vmbr0 -p icmp -j ACCEPT
# Autorise le SSH
iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 22 -j ACCEPT
# Autorise l'accès au Proxmox sur le WEB
iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 8006 -j ACCEPT
# Redirection de tout le trafic sur le pfsense
iptables -t nat -A PREROUTING -i vmbr0 -j DNAT --to 10.0.0.2
# Source NAT sur l'IP de la salle
iptables -t nat -A POSTROUTING -o vmbr0 -j SNAT -s 10.0.0.0/30 --to-source 10.202.100.200
```

### Configuration du VLAN10

On configure dans un premier temps l'interface sur le bon bridge et on donne le Tag du VLAN:

![vlan-int](img/vlan10.png)

Ensuite, on configure l'IP pour ce VLAN:

![vlan-ip](img/img/vlan10-ip.png)
<br>

Enfin, on configure un server DHCP pour le VLAN afin d'adresser les machines Windows durant leurs installations.

![vlan-dhcp](img/img/vlan10-dhcp.png)
