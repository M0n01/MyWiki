## Installation de Samba 

### Configuration de la machine 

#### Configuration du nom de la machine :

```bash
nano /etc/hostname
```
remplacer la ligne par : servAD.rt.iut

```bash
nano /etc/hosts
```
avoir la ligne suivante : 192.168.10.1 servAD.rt.iut

#### Configuration de l'adresse ip :

```bash
nano /etc/network/interfaces

auto eth1   // coté NAT
iface eth1 inet dhcp

auto eth0   // coté LAN interne
iface eth0 inet static
address 192.168.10.1/24
```
Puis reboot

### Installation et configuration de kerberos :

#### Installation des outils pour kerberos et kerberos :

```bash
apt-get install dnsutils
apt-get install attr
apt-get install krb5-user libpam-krb5
```

#### Entrer dans l’ordre :

RT.IUT (en maj)

servAD.rt.iut

servAD.rt.iut

#### Configuration de Kerberos :

nano /etc/krb5.conf :
```bash
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
```

#### Installer les outils nécessaire au DC :

```bash
apt-get install winbind
apt-get install samba ldap-utils
apt-get install msktutil
```
Winbind intègre NTLM

Taper : 
```bash
samba-tool domain provision 
```

et comme réponse : 

RT.IUT, RT, dc, SAMBA_INTERNAL,<ip_forwarder>, pass Rt85000lry!

Si erreur, renommer smb.conf:
```bash
mv /etc/samba/smb.conf /etc/samba/smb_old.conf
```
ou le supprimer
```bash
rm -f /etc/samba/smb.conf
```

Taper ensuite : 
```bash
cp /var/lib/samba/private/krb5.conf /etc/

samba-tool domain passwordsettings set --complexity=off
samba-tool domain passwordsettings set --min-pwd-length=5
samba-tool user add rt (avec rtlry en mdp)
samba-tool user add root (avec rtlry en mdp)
samba-tool user add administrateur (avec Rtlry! en mdp)
samba-tool group addmembers "Domain Admins" administrateur
```

On redémarre ensuite les services : 
```bash
systemctl unmask samba-ad-dc
systemctl enable samba-ad-dc
systemctl disable samba winbind nmbd smbd
systemctl mask samba winbind nmbd smbd
```

Et on reboot
```bash
reboot
```

Vérifier la résolution DNS
```bash
dig @localhost servAD.rt.iut
```

Configurer la résolution du serveur
```bash
nano /etc/resolv.conf
domain rt.iut
search rt.iut
nameserver 127.0.0.1
```

Tester avec nslookup
```bash
nslookup servAD
nslookup servrtrezo.rtprive.rt
```

Vérifier l'accès à l'AD  :
```bash
kinit administrateur@RT.IUT
net ads info
net ads user
```

Vérifier winbind : 
```bash
wbinfo -u
```

Test Lanman et LM/mschap :
```bash
wbinfo -a rt (erreur possible avec mschap !)
```

Test Ntlmv2 et LM2/mschap Modifier :
```bash
nano /etc/samba/smb.conf

Dans [global] : ntlm auth = yes 

/etc/init.d/samba-ad-dc restart

wbinfo -a rt

wbinfo -a rt --ntlmv2
```

Test Kerberos : 
```bash
wbinfo -K rt
```

Vérifier Ticket Kerberos : 
```bash
kinit administrator
```
Mot de passe : Rt85000lry!

Lister les tickets Kerberos : 
```bash
klist
```

Qu'est que krbtgt ?

"krbtgt" est un compte utilisateur spécial utilisé par Microsoft Active Directory pour représenter la clé de chiffrement principale utilisée pour chiffrer les tickets Kerberos. Le nom d'utilisateur "krbtgt" signifie "Kerberos Ticket Granting Ticket", qui est le service de sécurité central qui délivre des tickets Kerberos pour les utilisateurs authentifiés.

Le compte krbtgt est créé automatiquement lorsqu'un domaine Active Directory est créé, et il est utilisé pour générer la clé de chiffrement principale qui est partagée entre tous les contrôleurs de domaine dans le domaine. Cette clé est utilisée pour chiffrer les tickets Kerberos qui sont émis par le contrôleur de domaine pour les utilisateurs qui tentent d'accéder aux ressources du domaine.


Supprimer les tickets Kerberos : 
```bash
kdestroy
```

Enregistrer un ticket :
```bash
samba-tool domain exportkeytab rt.keytab --principal=rt
```

Lire un fichier keytab : 
```bash
klist -k rt.keytab -e
```

S'authentifier avec ce ticket : 
```bash
kinit -t rt.keytab -k rt
```

Activer l'échange chiffré :
```bash
nano /etc/ldap/ldap.conf
TLS_REQCERT never #Ligne à ajouter
```

Lire l'annuaire ldap : sur servAD :
```bash
ldapsearch -x -D "cn=administrateur,cn=Users,dc=rt,dc=iut" -W -h servAD.rt.iut -b "CN=Users,dc=rt,dc=iut"

(sur la même ligne)
```  

pdbedit permet également d’interroger la base : 
```bash
pdbedit -L -v
```


## Configurer le routage sur le serveur samba

activer le routage :
```bash
nano /etc/sysctl.conf 
Ajouter : net.ipv4.ip_forward=1
```
puis
```bash
sysctl -p
```

Configurer le PAT
```bash
nano /etc/network/interfaces

post-up iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE
```


## Installer et configurer le DHCP et le DNS

Dnsmasq intègre les services DHCP et DNS simplifié pour un réseau simple

Installer le paquet dnsmasq : 
```bash
apt update
apt install dnsmasq
```

Toujours sauvegarder le fichier de configuration avant modification :
```bash
cp /etc/dnsmasq.conf /etc/dnsmasq.conf.save
```

Modifier la configuration :
```bash
nano /etc/dnsmasq.conf

interface=eth0
port=0 #desactive DNS
dhcp-option=option:dns-server,192.168.10.1
dhcp-option=option:domain-name,rt.iut
#server=192.168.10.1
dhcp-authoritative
dhcp-option=option:router,192.168.10.1
dhcp-range=192.168.10.10,192.168.10.50,12h
```

Mettre les IP/ noms des machines dans /etc/hosts :
```bash
192.168.10.1 dns.rt.iut dhcp.rt.iut www.rt.iut
192.168.10.11 poste1.rt.iut
```

Redémarrer le service DNSMASQ : 
```bash
/etc/init.d/dnsmasq restart
```

# VM Client Linux et NTLM / VM Radius

Configuration du client linux :
```bash
nano /etc/network/interfaces

auto eth0
iface eth0 inet static
address 192.168.10.100/24

/etc/resolv.conf     // refaire après reboot
search rt.iut 
nameserver 192.168.10.1 

/etc/hostname 
radius.rt.iut 

/etc/hosts : 
127.0.0.1 localhost 
192.168.10.100 radius.rt.iut 
```

Installer les utilitaires :
```bash
apt install ntpdate
apt install winbind 
apt install cifs-utils 
apt install ldap-utils
```

Synchro l’heure :
```bash
ntpdate servAD.rt.iut     // ne marche pas mais pg
```


Intégrer le client dans le domaine :

nano /etc/samba/smb.conf
[global]
   workgroup = RT #Ligne a remplacer
   realm = RT.IUT
   security = ads
   kerberos method = secrets and keytab
   winbind use default domain = yes

Puis reboot

reboot

Adhérer le client au domaine :

net ads join -U administrateur

Test de connexion avec l’AD ( LDAP ) :

nano /etc/ldap/ldap.conf 
TLS_REQCERT never #Ligne à ajouter

ldapsearch -x -ZZ -LLL -D "cn=administrateur,cn=Users,dc=rt,dc=iut" -W -h servAD.rt.iut -b "CN=Users,dc=rt,dc=iut"

Vérifier avec Winbind :

service winbind restart
wbinfo -u
wbinfo -g
wbinfo -a administrateur
wbinfo -a rt --ntlmv2 		#test authentification LM (--lanman)/NTLM(--ntlmv2)
wbinfo -K rt 			#échec du test authentification kerberos !!!

vérifier une authentification avec ntlm_auth :

 ntlm_auth --request-nt-key –domain=RT.IUT --username=rt –password=rtlry

-   Linux et KERBEROS Installation des utilitaires :

apt install krb5-user && apt install libpam-krb5

l’installation de kerberos doit produire cette configuration : /etc/krb5.conf :

mv /etc/krb5.conf /etc/krb5.conf.bak
nano /etc/krb5.conf
(Ligne à ajouter)
[libdefaults]
	default_realm = RT.IUT
	dns_lookup_realm = false
	dns_lookup_kdc = true
[realms]
	RT.IUT = {
    	kdc = servAD.rt.iut
    	default_domain = rt.iut
	}
[domain_realm]
	.rt.iut = RT.IUT
          	rt.iut = RT.IUT

Kerberos : récupérer un ticket de session administrateur :

kinit administrateur@RT.IUT
klist -e
kdestroy

Vous êtes donc en mesure d’utiliser les différents services disponibles dans le domaine

wbinfo -u 	( si erreur redémarrer service winbind )
wbinfo -K rt 	test authentification kerberos

# [](https://github.com/WolfAnto/TP_SAMBA4_RADIUS/blob/main/README.md#vm-serveur-radius)VM Serveur Radius

(Refaire les étapes précédente ou sinon reprendre le Client Linux pour le transformer en serveur Radius)

-   Configuration Serveur Radius Installation de freeradius :

apt-get install freeradius

Configuration de freeradius :

nano /etc/freeradius/3.0/clients.conf
client localhost {
ipaddr = 127.0.0.1 ou ip de VM3
secret = testing123
}

Configuration des users :

nano /etc/freeradius/3.0/users

A la fin du fichier config ajouter :
nom-p Cleartext-Password := "azerty"

Redémarrer le service :

systemctl restart freeradius.service

Vérification :

radtest nom-p azerty localhost 0 testing123
Resultat : Received Access-Accept...

Vérification des différents mode :

radtest -t pap nom-p azerty localhost 0 testing123
Resultat : Received Access-Accept…

radtest -t chap nom-p azerty localhost 0 testing123
Resultat : Received Access-Accept…

radtest -t mschap nom-p azerty localhost 0 testing123
Resultat : Received Access-Accept…

Décommenter la ligne # unix :

nano /etc/freeradius/3.0/sites-enabled/default
authorize {
unix      
}

redémarrer le service freeradius :

systemctl restart freeradius.service

retester (remarque : vous pouvez à la fois utiliser les comptes locaux et unix) :

radtest rt rtlry localhost 0 testing123

Se mettre en DHCP pour recevoir IP+DNS de la part de l’AD :

nano /etc/network/interfaces
auto lo
iface lo inet loopback

auto eth0
#iface eth0 inet static
#address 192.168.10.100/24   // enlève l’ancienne conf statique
#gateway 192.168.10.1
iface eth0 inet dhcp

Puis reboot

reboot

Test de création de hash LM et NTLM :

smbencrypt motdepassecompliqué

Installer winbind :

apt-get install winbind

nano /etc/samba/smb.conf
security = ADS
realm = rt.iut
workgroup = RT

Rejoindre le domaine :

net join ads -U administrateur

service winbind restart

nano /etc/freeradius/3.0/users
Ajouter :  DEFAULT Auth-Type := ntlm_auth

Ajoutez ces lignes dans la partie Authenticate :

nano /etc/freeradius/3.0/sites-enabled/default 
nano /etc/freeradius/3.0/sites-enabled/inner-tunnel

Auth-Type ntlm_auth {
          ntlm_auth
}

nano /etc/freeradius/3.0/mods-available/ntlm_auth

exec ntlm_auth {
    	wait = yes
    	program = "/usr/bin/ntlm_auth --request-nt-key --domain=RT --username=%{mschap:User-Name} --password=%{User-Password}"
}

Arrêter le service et démarrer freeradius en mode debug :

service freeradius stop
freeradius -X

Dans une autre console en localhost :

radtest administrateur Rtlry! localhost 0 testing123

Mschap avec ntlm_auth :

nano /etc/freeradius/3.0/users
DEFAULT Auth-Type:= mschap

nano /etc/freeradius/3.0/mods-available/mschap
En dessous de la ligne 82 ajouter :

ntlm_auth = "/usr/bin/ntlm_auth --request-nt-key --username=%{mschap:User-Name:-None} --domain=%{%{mschap:NT-Domain}:-RT} --challenge=%{mschap:Challenge:-00} --nt-response=%{mschap:NT-Response:-00}"

winbind_username = "%{mschap:User-Name}"
winbind_domain = "RT"

Pour que freeradius puisse appeler winbindd :

usermod -a -G winbindd_priv freerad
chown root:winbindd_priv /var/lib/samba/winbindd_privileged/

stop freeradius efficacement :

pkill freeradius

démarrer freeradius en mode debug :

freeradius -X

dans une autre console :

radtest -t mschap rt rtlry localhost 0 testing123