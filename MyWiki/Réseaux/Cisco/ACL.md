
Une liste de contrôle d'accès (ou ACL) est une série de commandes IOS qui déterminent si un routeur achemine ou abandonne les paquets en fonction des informations contenues dans l'en-tête de paquet. Par défaut, aucun ACL n'est configuré pour un routeur.

Les ACLs peuvent être configurées pour s'appliquer au trafic entrant et au trafic sortant.

>[!Remarque]
>Elles ne gèrent pas les paquets provenant du routeur lui-même.

Les routeurs Cisco prennent en charge deux types d'ACLs:

- ==**ACLs standard**== - les ACLs ne filtrent qu'au niveau de la couche 3 en utilisant uniquement l'adresse IPv4 source.
- ==**ACLs étendues**== - Les ACLs filtrent au niveau de la couche 3 en utilisant l'adresse IPv4 source et/ou destination. Ils peuvent également filtrer au niveau de la couche 4 en utilisant les ports TCP et UDP, ainsi que des informations facultatives sur le type de protocole pour un contrôle plus fin.

>Les ACL sont appliquées dans l’ordre par la machine, la première ACL (en haut) sera la première. Donc l’ordre est très important.

**ACL numérotées**

Les ACL numéro 1 à 99, ou 1300 à 1999 sont des ACL standard tandis que les ACL numéro 100 à 199, ou 2000 à 2699 sont des ACL étendues, comme indiqué dans la sortie.

```
R1(config)# access-list ?
  <1-99>       IP standard access list
  <100-199>    IP extended access list
  <1100-1199>  Extended 48-bit MAC address access list
  <1300-1999>  IP standard access list (expanded range)
  <200-299>    Protocol type-code access list
  <2000-2699>  IP extended access list (expanded range)
  <700-799>    48-bit MAC address access list
  rate-limit   Simple rate-limit specific access list
  template     Enable IP template acls
```

**ACL nommées**

Les ACL nommées sont la méthode préférée à utiliser lors de la configuration des ACL. Plus précisément, les listes ACL standard et étendues peuvent être nommées pour fournir des informations sur l'objet de la liste ACL. Par exemple, nommer un ACL FTP-FILTER étendu est beaucoup mieux que d'avoir une ACL numérotée 100.

La commande de configuration globale **ip** **access-list** est utilisée pour créer une liste ACL nommée, comme illustré dans l'exemple suivant.

```
R1(config)# ip access-list extended FTP-FILTER
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq ftp
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq ftp-data
```

### ACL standard

```
Router(config)# access-list 2 deny <ip de l’extérieur> <ip intérieur>

Router(config)# access-list 2 deny 192.168.10.1 0.0.0.0 // refuse l’ip 192.168.10.1 avec n’importe quel masque

Router(config)# access-list 2 permit 192.168.10.0 0.0.0.255 // autorise les ip du réseau 192.168.10.0 avec un masque de 255.255.255.0

Router(config)# access-list 2 deny 192.168.0.0 0.0.255.255 // refuse les ip du réseau 192.168.0.0 avec un masque de 255.255.0.0

Router(config)# access-list 2 permit 192.0.0.0 0.255.255.255 // autorise les ip du réseau 192.0.0.0 avec un masque de 255.255.255.0

Router(config)# no access-list 2 permit 192.0.0.0 0.255.255.255 // supprime l’acl

Router(config)# interface fastethernet0/0

Router(config-if)# ip access-group 2 in // applique les ACL sur les paquets entrant du port fastethernet0/0
```

>[!INFO]
>Les masques sont inversés dans les commandes.
>Access-list >= 100 pour les acl étendu.

### ACL étendu

```
Router(config)# access‐list 101 deny ip any host 192.168.34.2 // machine 192.168.34.2 inaccessible depuis l’extérieur.

Router(config)# interface fastethernet0/0

Router(config-if)# ip access-group 101 in

Router(config)# access-list 100 permit tcp 10.1.1.0 0.0.0.255 10.1.2.0 0.0.0.255 eq 21 // “eq” veut dire égal, et le numéro de port 21 utilisé par le serveur FTP. On autorise le réseau 10.1.1.0/24 à communiquer sur le port 21 (pour le FTP) avec le réseau 10.1.2.0/24.

Router(config)# access-list 100 permit tcp 10.1.1.0 0.0.0.255 10.1.2.0 0.0.0.255 neq 21 // “neq” veut dire non égal. Donc pareil mais on autorise le réseau 10.1.1.0/24 à communiquer sur tout sauf le port 21
```

Afficher les ACL
```
Router(config)# show access‐lists
```
Afficher les ACL avec leur interfaces
```
Router(config)# show running-config
```

#### Ports

```
Router(config)# access-list 102 permit tcp 192.168.1.5 any eq 80 // autorise le poste 192.168.1.5 à accéder au port 80 de l’extérieur 

Router(config)# access-list 102 permit tcp 192.168.1.5 any eq 21 // autorise le poste 192.168.1.5 à accéder au port 21 de l’extérieur 

Router(config)# access-list 102 deny tcp 192.168.1.5 any // refuse le poste 192.168.1.5 à l'accès à tous les ports. 

Router(config)# access-list 102 permit ip any any // accepte toutes les ip
```



## Bonnes pratiques

**Baser les ACLs sur les politiques de sécurité de l'organisation.**

>Vous serez ainsi certain d'implémenter les instructions relatives à la sécurité organisationnelle.

**Écrivez ce que vous voulez que l'ACL fasse.**

>Cela vous aidera à éviter de créer par inadvertance d'éventuels problèmes d'accès.

**Utilisez un éditeur de texte pour créer, modifier et enregistrer les listes de contrôle d'accès.**

>Vous pourrez ainsi créer une bibliothèque de listes de contrôle d'accès réutilisables.

**Documentez les ACL à l'aide de lacommande 'remarque'.**

>Cela vous aidera (et d'autres) à comprendre le but d'une ACL.

**Testez vos listes de contrôle d'accès sur un réseau de développement avant de les implémenter sur un réseau de production.**

>Vous éviterez ainsi de commettre des erreurs coûteuses.


### Placement

- ==Les ACLs étendues== doivent être placées le plus près possible de la source du trafic à filtrer. De cette manière, le trafic indésirable est refusé près du réseau source et ne traverse pas l'infrastructure de réseau.

- ==Les ACLs standard== doivent être placées le plus près possible de la destination. Si une liste de contrôle d'accès standard a été placée à la source du trafic, l'instruction « permit » ou « deny » est appliquée en fonction de l'adresse source, quelle que soit la destination du trafic.