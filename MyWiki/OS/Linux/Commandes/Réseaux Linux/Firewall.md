#firewall #iptables 

Règles
```bash
iptables -A INPUT -i eth0 -j ACCEPT
iptables -A OUTPUT -i eth0 -j ACCEPT

iptables -A INTPUT -p icmp -j ACCEPT
iptables -A OUTPUT -p icmp -j ACCEPT

iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP

iptables -t filter -A INPUT -p icmp -J ACCEPT
iptables -t filter -A INPUT -i lo -J ACCEPT

#Accépter en entrée que ce que je demande (config poste client)
iptables -t filter -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

#autorisé en sortis les packets HTTP / HTTPS / DNS / PROXY / NTP / IMAP / SMTP ... à sortir su un port (config poste client)
iptables -t filter -A OUTPUT -p tcp --dport 80,443,53,3128 -j ACCEPT

#autorisé en entrée les packets HTTP
iptables -t filter -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -t filter -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

#toutes la commutation
-A FORWARD -t tcp --dport 80 -j ACCEPT
-i eth0 -j ACCEPT
-A FORWARD --state ESTABLISHED
```

## Iptables 

Il existe également d'autres solutions comme **nftables**, **ufw** et **firewalld**. 

- **Nftables** fournit une syntaxe plus moderne et des performances améliorées par rapport à iptables. Cependant, la syntaxe des règles **nftables** n'est pas compatible avec iptables, la migration vers **nftables** nécessite donc quelques efforts. 
- **UFW** (Uncomplicated Firewall) fournit une interface simple et conviviale pour configurer les règles de pare-feu. **UFW** est construit sur le framework iptables comme nftables et offre un moyen plus simple de gérer les règles de pare-feu. 
- Enfin, **FirewallD** fournit une solution de pare-feu dynamique et flexible qui peut être utilisée pour gérer des configurations de pare-feu complexes. Il prend en charge un riche ensemble de règles pour filtrer le trafic réseau et peut être utilisé pour créer des zones et des services de pare-feu personnalisés. Il se compose de plusieurs composants qui fonctionnent ensemble pour fournir une solution de pare-feu flexible et puissante. 

Les principaux composants d'**iptables** sont :

|   |   |
|---|---|
|`Tables`|Tables are used to organize and categorize firewall rules.|
|`Chains`|Chains are used to group a set of firewall rules applied to a specific type of network traffic.|
|`Rules`|Rules define the criteria for filtering network traffic and the actions to take for packets that match the criteria.|
|`Matches`|Matches are used to match specific criteria for filtering network traffic, such as source or destination IP addresses, ports, protocols, and more.|
|`Targets`|Targets specify the action for packets that match a specific rule. For example, targets can be used to accept, drop, or reject packets or modify the packets in another way.|

### Tables

Les capacités d'iptables sont utilisées pour catégoriser et organiser les règles de pare-feu en fonction du type de trafic qu'elles sont conçues pour gérer. Ces tableaux sont utilisés pour organiser et catégoriser les règles de pare-feu. Chaque table est chargée d'effectuer un ensemble spécifique de tâches.

|   |   |   |
|---|---|---|
|`filter`|Used to filter network traffic based on IP addresses, ports, and protocols.|INPUT, OUTPUT, FORWARD|
|`nat`|Used to modify the source or destination IP addresses of network packets.|PREROUTING, POSTROUTING|
|`mangle`|Used to modify the header fields of network packets.|PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING|

En plus des tables intégrées, iptables fournit une quatrième table appelée table brute, qui est utilisée pour configurer des options spéciales de traitement des paquets. La table brute contient deux chaînes intégrées : PREROUTING et OUTPUT.

### Chains

Dans iptables, les chaînes organisent des règles qui définissent la manière dont le trafic réseau doit être filtré ou modifié. Il existe deux types de chaînes dans iptables :
- Built-in chains
- User-defined chains

Les **User-defined chains** peuvent simplifier la gestion des règles en regroupant les règles de pare-feu en fonction de critères spécifiques, tels que l'adresse IP source, le port de destination ou le protocole. Ils peuvent être ajoutés à l’un des trois tableaux principaux. Par exemple, si une organisation dispose de plusieurs serveurs Web qui nécessitent tous des règles de pare-feu similaires, les règles de chaque serveur peuvent être regroupées dans une chaîne définie par l'utilisateur.

Les **Built-in chains** sont prédéfinies et créées automatiquement lors de la création d'une table. Chaque table possède un ensemble différent de chaînes intégrées. 

La table de `filter` comporte trois chaînes intégrées. Ces chaînes sont utilisées pour filtrer le trafic réseau entrant et sortant, ainsi que le trafic transmis entre différentes interfaces réseau : **INPUT, OUTPUT, FORWARD**.

La table `nat` possède deux chaînes intégrées. La chaîne **PREROUTING** est utilisée pour modifier l'adresse IP de destination des paquets entrants avant que la table de routage ne les traite. La chaîne **POSTROUTING** est utilisée pour modifier l'adresse IP source des paquets sortants après que la table de routage les a traités :

La table `mangle` comporte cinq chaînes intégrées. **PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING**.

Ces chaînes permettent de modifier les champs d'en-tête des paquets entrants et sortants et des paquets en cours de traitement par les chaînes correspondantes.


### Rules and Targets

Les règles Iptables sont utilisées pour définir les critères de filtrage du trafic réseau et les actions à entreprendre pour les paquets qui correspondent aux critères. Les règles sont ajoutées aux chaînes à l'aide de l'option -A suivie du nom de la chaîne, et elles peuvent être modifiées ou supprimées à l'aide de diverses autres options.

|   |   |
|---|---|
|`ACCEPT`|Allows the packet to pass through the firewall and continue to its destination|
|`DROP`|Drops the packet, effectively blocking it from passing through the firewall|
|`REJECT`|Drops the packet and sends an error message back to the source address, notifying them that the packet was blocked|
|`LOG`|Logs the packet information to the system log|
|`SNAT`|Modifies the source IP address of the packet, typically used for Network Address Translation (NAT) to translate private IP addresses to public IP addresses|
|`DNAT`|Modifies the destination IP address of the packet, typically used for NAT to forward traffic from one IP address to another|
|`MASQUERADE`|Similar to SNAT but used when the source IP address is not fixed, such as in a dynamic IP address scenario|
|`REDIRECT`|Redirects packets to another port or IP address|
|`MARK`|Adds or modifies the Netfilter mark value of the packet, which can be used for advanced routing or other purposes|

### Matches

Les correspondances sont utilisées pour spécifier les critères qui déterminent si une règle de pare-feu doit être appliquée à un paquet ou une connexion particulière. Les correspondances sont utilisées pour faire correspondre des caractéristiques spécifiques du trafic réseau, telles que l'adresse IP source ou de destination, le protocole, le numéro de port, etc.

|   |   |
|---|---|
|`-p` or `--protocol`|Specifies the protocol to match (e.g. tcp, udp, icmp)|
|`--dport`|Specifies the destination port to match|
|`--sport`|Specifies the source port to match|
|`-s` or `--source`|Specifies the source IP address to match|
|`-d` or `--destination`|Specifies the destination IP address to match|
|`-m state`|Matches the state of a connection (e.g. NEW, ESTABLISHED, RELATED)|
|`-m multiport`|Matches multiple ports or port ranges|
|`-m tcp`|Matches TCP packets and includes additional TCP-specific options|
|`-m udp`|Matches UDP packets and includes additional UDP-specific options|
|`-m string`|Matches packets that contain a specific string|
|`-m limit`|Matches packets at a specified rate limit|
|`-m conntrack`|Matches packets based on their connection tracking information|
|`-m mark`|Matches packets based on their Netfilter mark value|
|`-m mac`|Matches packets based on their MAC address|
|`-m iprange`|Matches packets based on a range of IP addresses|










































