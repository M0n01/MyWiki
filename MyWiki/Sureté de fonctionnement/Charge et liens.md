# Bond-LVS-Fail_Over_Service-Web Sous VirtualBox
Agrégation de lien, avec répartition de charge du service Monkey Web

Prérequis :

- LB1 :
- 3 cartes réseau (Bridge, Bridge, Lan Segment)
- 3 Disques (20Go, 1Go, 1Go)

- LB2 :
- 3 cartes réseau (Bridge, Bridge, Lan Segment)
- 3 Disques (20Go, 1Go, 1Go)

- 1 machines DSL
- Carte en réseau interne (Lan segment, le même que LB1 et LB2)

# Schéma (IPs différente selon votre réseau)
![Untitled](https://user-images.githubusercontent.com/73076854/210636887-a64912a7-2a5e-4c95-8e87-edee4746c98a.png)


# TOUTES LES MANIPULATIONS SONT A FAIRE EN DOUBLE SUR LB1 & LB2

#### Agrégation de lien

- Conf réseau
	- clique droit décocher activer réseau
```
dhclient //obtenir ip
```

- Installer le paquet ifsenslave
```bash
apt-get install ifenslave
```
- Configuration du Bond0 (Adapter le nom de la carte réseau)
```bash
nano /etc/modprobe.d/alias-bond.conf
alias bond0 bonding
options bonding mode=1 primary=enp0s3 fail_over_mac=1
```
Configuration de la carte réseau bond0 (Adapter le nom des cartes réseau)
- Désactiver au préalable les autres cartes réseau
```bash
nano /etc/network/interfaces
auto bond0
iface bond0 inet dhcp
bond-mode 1
bond-slaves enp0s3 enp0s8
```
- Déchargement du module (Supprime l'ancien Bond, si il existe)
```bash
rmmod bonding
```
- Rédémarrage du service, puis rédemarrage machine
```bash
/etc/init.d/networking restart
reboot
```
- Vérification
```bash
cat /proc/net/bonding/bond0
ip -br ad
```
#### Mise en place FAIL OVER SERVICE
```bash
apt install keepalived
```
- modifier la configuration
```bash
nano /etc/keepalived/keepalived.conf

 vrrp_instance VI_1 {
 state MASTER
 interface bond0
 virtual_router_id 51
 priority 255
 advert_int 1
 authentication {
 auth_type PASS
 auth_pass 12345
 }
 virtual_ipaddress {
 192.168.2.100/21 dev bond0
'Cette IP doit avoir le meme format+CIDR que bond0'
 }
}

 vrrp_instance VI_2 {
 state MASTER
 interface enp0s9
 virtual_router_id 52
 priority 255
 advert_int 1
 authentication {
 auth_type PASS
 auth_pass 12345
 }
 virtual_ipaddress {
 192.168.56.100/24 dev enp0s9
'Cette IP doit avoir le meme format+CIDR que enp0s9'
 }
}
```
- Redémarrer le service keepalived
```bash
systemctl restart keepalived
systemctl enable keepalived
```
- Cloner la VM en LB2, modifier priority 254
- Ceci est la configuration pour LB2 :
```bash
nano /etc/keepalived/keepalived.conf

 vrrp_instance VI_1 {
 state MASTER
 interface bond0
 virtual_router_id 51
 priority 254
 advert_int 1
 authentication {
 auth_type PASS
 auth_pass 12345
 }
 virtual_ipaddress {
 192.168.2.100/24 dev bond0
 'Cette IP doit avoir le meme format+CIDR que bond0'
 }
}

 vrrp_instance VI_2 {
 state MASTER
 interface enp0s9
 virtual_router_id 52
 priority 254
 advert_int 1
 authentication {
 auth_type PASS
 auth_pass 12345
 }
 virtual_ipaddress {
 192.168.56.100/24 dev enp0s9
 'Cette IP doit avoir le meme format+CIDR que enp0s9'
 }
}
```
- Redémarrer le service keepalived
```bash
systemctl restart keepalived
systemctl enable keepalived
```
- Vérifier le basculement ( adresse ip de bond0 ) en cas de coupure de bond0 sur LB1, puis LB2
```bash
ip a show dev bond0
ip a show dev enp0s9
```
#### Répartition de charge LVS/Serveur Web
- Ajouter une troisieme carte réseau (en LAN Segment) (Adapter le nom de la carte réseau)
```bash
nano /etc/network/interfaces
auto enp0s9
iface enp0s9 inet static
address 192.168.56.1
netmask 255.255.255.0
```
- Sur la deuxieme machine
- Ajouter une troisieme carte réseau (en LAN Segment) (Adapter le nom de la carte réseau)
```bash
nano /etc/network/interfaces
auto enp0s9
iface enp0s9 inet static
address 192.168.56.2
netmask 255.255.255.0
```
- Rédémarrage du service
```bash
/etc/init.d/networking restart
```
- Définir les IPs sur les machine DSL

![image](https://user-images.githubusercontent.com/73076854/210634058-9aa6bf9e-1ebc-499a-bcc3-ee00ff2046ea.png)

- Lancer le service Monkey Web

![image](https://user-images.githubusercontent.com/73076854/208650237-a144f383-0dd0-43df-b6b5-a7b1e11f17b1.png)

- Installation des paquets
```bash
apt install iptables
apt install iptables-persistent
apt install ipvsadm
```
- Mettre en place le NAT sur les paquets sortants vers internet (Adapter selon votre carte réseau)
```bash
iptables -t nat -A POSTROUTING -o bond0 -j MASQUERADE
iptables-save > /etc/iptables/rules.v4   // pour sauvegarder
```
- Autorisation du port 80
```bash
iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT
iptables-save > /etc/iptables/rules.v4
```
- Activation du routage
```bash
nano /etc/sysctl.conf
net.ipv4.ip_forward=1
```
- Rédémarrer votre machine pour appliquer le routage
```bash
reboot
```
- Créer un service virtuel
```bash
'XX = Machine Hote IP Virtuel (keepAlived), YY = Machines DSL'
ipvsadm -A -t 192.168.7.120:80 -s rr
ipvsadm -a -t 192.168.7.120:80 -r 192.168.YY.YYY -m
ipvsadm -L
```
- Vérification
```bash
apt install curl
curl 192.168.XX.XXX
ou
http://192.168.XX.XXX
```
CTRL + F5 pour refresh sans cache

![image](https://user-images.githubusercontent.com/73076854/208654898-56884b7f-ccf9-4194-b065-5c00422ac0a4.png)

