## Installation de Samba :

## Configuration de la machine :

### Configuration du nom de la machine :

nano /etc/hostname :

remplacer la ligne par : servAD.rt.iut

  

nano /etc/hosts :

avoir la ligne suivante :

192.168.10.1 servAD.rt.iut

  

### Configuration de l'adresse ip :

nano /etc/network/interfaces :

auto eth1

iface eth1 inet dhcp

  

auto eth0

iface eth0 inet static

address 192.168.100.1/24

Puis reboot

  

## Installation et configuration de kerberos :

### Installation des outils pour kerberos et kerberos :

apt-get install dnsutils

apt-get install attr

apt-get install krb5-user libpam-krb5

  

### Entrer dans l’ordre :

RT.IUT (maj !)

servAD.rt.iut

servAD.rt.iut

  

### Configuration de Kerberos :

  

nano /etc/krb5.conf :

[libdefaults]

default_realm = RT.IUT

dns_lookup_realm = false #Ligne à ajouter

dns_lookup_kdc = true #Ligne à ajouter

[realms]

RT.IUT = {

kdc = servAD.rt.iut

admin_server = servAD.rt.iut

}

[domain_realm]

.rt.iut = RT.IUT #Ligne à ajouter

rt.iut = RT.IUT #Ligne à ajouter

  
  

### Installer les outils nécessaire au DC :

apt-get install winbind

apt-get install samba ldap-utils

apt-get install msktutil

  

Tapez : 

samba-tool domain provision et comme réponse : 

  

RT.IUT, RT, dc, SAMBA_INTERNAL,<ip_forwarder>, pass Rt85000lry!

  

Tapez ensuite : 

#cp /var/lib/samba/private/krb5.conf /etc/

samba-tool domain passwordsettings set --complexity=off

samba-tool domain passwordsettings set --min-pwd-length=5

samba-tool user add rt (avec rtlry en mdp)

samba-tool user add root (avec rtlry en mdp)

samba-tool user add administrateur (avec Rtlry! en mdp)

samba-tool group addmembers "Domain Admins" administrateur

  

On redémarre ensuite les services : 

  

systemctl unmask samba-ad-dc

systemctl enable samba-ad-dc

systemctl disable samba winbind nmbd smbd

systemctl mask samba winbind nmbd smbd

reboot

# Configuration en DC et DNS avec SAMBA

## Configuration de Samba :

  

### Effacer le fichier smb.conf : 

rm -f /etc/samba/smb.conf

  

### Passage de Samba en DC :

samba-tool domain provision

Information à entrer après la commande : RT.IUT, RT, dc, SAMBA_INTERNAL, <ip_forwarder>, pass Rt85000lry!

cp /var/lib/samba/private/krb5.conf /etc/

samba-tool domain passwordsettings set --complexity=off

samba-tool domain passwordsettings set --min-pwd-length=5

samba-tool user add rt  Mot de passe : rtlry 

samba-tool user add root Mot de passe : rtlry

samba-tool user add administrateur  Mot de passe : Rtlry!

samba-tool group addmembers "Domain Admins" administrateur

  

systemctl unmask samba-ad-dc

systemctl enable samba-ad-dc

systemctl disable samba winbind nmbd smbd

systemctl mask samba winbind nmbd smbd

  

Puis reboot

  

### Vérifier la résolution DNS : 

dig @localhost servAD.rt.iut

  

### Configurer la résolution du serveur :

nano /etc/resolv.conf

domain rt.iut

search rt.iut

nameserver 127.0.0.1

  

### tester avec nslookup :

nslookup servAD

nslookup servrtrezo.rtprive.rt

  

### Vérifier l'accès à l'AD  :

kinit administrateur@RT.IUT

net ads info

net ads user

  

### Vérifier winbind : 

wbinfo -u

  

### Test Lanman et LM/mschap :

wbinfo -a rt (erreur possible avec mschap !)

  

### Test Ntlmv2 et LM2/mschap Modifier :

nano /etc/samba/smb.conf

Dans [global] : ntlm auth = yes 

/etc/init.d/samba-ad-dc restart

wbinfo -a rt

wbinfo -a rt --ntlmv2

  

### Test Kerberos : 

wbinfo -K rt

  

### Vérifier Ticket Kerberos : 

kinit administrator Mot de passe : Rt85000lry!

  

### Lister les tickets Kerberos : 

klist

  

### Qu'est que krbtgt ?

KRBTGT est également le nom du principal de sécurité utilisé par le KDC pour un domaine Windows Server. Il est créé automatiquement lorsqu'un nouveau domaine est créé

  

### Supprimer les tickets Kerberos : 

kdestroy

  

### Enregistrer un ticket :

samba-tool domain exportkeytab rt.keytab --principal=rt

  

### Lire un fichier keytab : 

klist -k rt.keytab -e

  

### S'authentifier avec ce ticket : 

kinit -t rt.keytab -k rt

  

### Lire l'annuaire ldap : sur servAD :

ldapsearch -x -D "cn=administrateur,cn=Users,dc=rt,dc=iut" -W -h servAD.rt.iut -b "CN=Users,dc=rt,dc=iut"

  

### vous obtenez une erreur à cause car l'échange est non chiffré :

nano /etc/ldap/ldap.conf ajouter TLS_REQCERT never

  

### pdbedit permet également d’interroger la base : 

pdbedit -L -v

# Routeur NAT (PAT)

## Configurer le routage sur le serveur samba

### activer le routage :

nano /etc/sysctl.conf Ajouter : net.ipv4.ip_forward=1

sysctl -p

Configurer le PAT

nano /etc/network/interfaces

post-up iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE

  
  

# Serveur DHCP et DNSMASQ

Dnsmasq intègre les services DHCP et DNS simplifié pour un réseau simple

## Installer et configurer le DHCP et le DNS

  

### Installer le paquet dnsmasq : 

apt update ; apt install dnsmasq

  

### Toujours sauvegarder le fichier de configuration avant modification :

cp /etc/dnsmasq.conf /etc/dnsmasq.conf.save

  

### Modifier la configuration :

nano /etc/dnsmasq.conf

interface=eth0

port=0 #desactive DNS

dhcp-option=option:dns-server,192.168.10.1

dhcp-option=option:domain-name,rt.iut

#server=192.168.10.1

dhcp-authoritative

dhcp-option=option:router,192.168.10.1

dhcp-range=192.168.10.10,192.168.10.50,12h

  

### Mettre les IP/ noms des machines dans /etc/hosts :

192.168.10.1 dns.rt.iut dhcp.rt.iut www.rt.iut

192.168.10.11 poste1.rt.iut

  

### Redémarrer le service DNSMASQ : 

/etc/init.d/dnsmasq restart