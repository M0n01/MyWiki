
```bash
wget adress_web    // récupère index.html

iptables-save   // sauvegarder iptables et affiche le contenu du Firewall

nano /etc/iptables/rules.v4    // les règles du Firewall (iptables)

tracert adress_ip   // comme ping mais avec plus infos

nslookup www.site.com    // affiche le serveur DNS configuré par défaut (cette commande dispose de nombreuses options permettant de tester et de vérifier le processus DNS de manière approfondie)

lshw -C network   // info carte réseaux

ip -br ad    // afficher adresse des cartes réseaux

netstat -laputen    // liste toute les connexions active

ss -laputen     // nouveau netstat (plus info)

route -n      // affiche les ip et leur passerelles et autres info

netdiscover     // Découvrir les ip et MAC sur un réseau
```

**Configuration réseau**
/etc/network/interfaces
```
auto eth0
iface eth0 inet static
address 192.168.25.10/24

auto eth1
iface eth1 inet dhcp
```

**Activer routage**
/etc/sysctl.conf
```
net.ipv4.ip_forward=1
```