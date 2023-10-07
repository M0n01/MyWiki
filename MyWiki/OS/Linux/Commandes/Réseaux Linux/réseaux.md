
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

echo -e "GET / HTTP/1.1\r\nHost: 127.0.0.1\r\nConnection: close\r\n\r\n" | nc 127.0.0.1 80  // requete HTTP localhost

curl "http://127.0.0.1/%2F139ed38c%2034ce3cdc%2F3ae18ce9%20" // encoder /139ed38c 34ce3cdc/3ae18ce9
```

**Configuration réseau**
```bash
/etc/network/interfaces

auto eth0
iface eth0 inet static
address 192.168.25.10/24

auto eth1
iface eth1 inet dhcp
```

**Activer routage**
```bash
/etc/sysctl.conf

net.ipv4.ip_forward=1
```

Activer PAT
```bash
nano /etc/network/interfaces

post-up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

Rédémarrer service réseau
```bash
service networking restart
```