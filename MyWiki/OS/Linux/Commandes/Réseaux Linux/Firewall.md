
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
