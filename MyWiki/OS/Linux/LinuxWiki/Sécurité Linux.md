
### Bases

Il existe différentes applications et services tels que Snort, chkrootkit, rkhunter, Lynis et d'autres qui peuvent contribuer à la sécurité de Linux. De plus, certains paramètres de sécurité doivent être définis, tels que :

- Supprimer ou désactiver tous les services et logiciels inutiles
- Suppression de tous les services qui reposent sur des mécanismes d'authentification non chiffrés
- Assurez-vous que NTP est activé et que Syslog est en cours d'exécution
- Assurez-vous que chaque utilisateur dispose de son propre compte
- Appliquer l'utilisation de mots de passe forts
- Configurer le vieillissement des mots de passe et restreindre l'utilisation des mots de passe précédents
- Verrouillage des comptes d'utilisateurs après des échecs de connexion
- Désactivez tous les binaires SUID/SGID indésirables


**NAC** = Network Access Control. Cela inclut : 
### Discretionary access control (DAC) voir [[Droits#DAC = Discretionary Access Control|Droits]]

Les utilisateurs et les groupes qui possèdent une ressource spécifique peuvent décider qui a accès à leurs ressources et quelles actions ils sont autorisés à effectuer. Ces autorisations peuvent être définies pour la lecture, l'écriture, l'exécution ou la suppression de la ressource.

### Mandatory access control (MAC)

MAC est utilisé dans une infrastructure qui offre un contrôle plus précis sur l'accès aux ressources que les systèmes DAC. Ces systèmes définissent des règles qui déterminent l'accès aux ressources en fonction du niveau de sécurité de la ressource et du niveau de sécurité de l'utilisateur ou du processus demandant l'accès.

Chaque ressource se voit attribuer une étiquette de sécurité qui identifie son niveau de sécurité, et chaque utilisateur ou processus se voit attribuer une habilitation de sécurité qui identifie son niveau de sécurité. L'accès à une ressource n'est accordé que si le niveau de sécurité de l'utilisateur ou du processus est égal ou supérieur au niveau de sécurité de la ressource.

### Role-based access control (RBAC)

RBAC attribue des autorisations aux utilisateurs en fonction de leurs rôles au sein d'une organisation.

RBAC simplifie la gestion des autorisations d'accès, réduit le risque d'erreurs et garantit que les utilisateurs peuvent accéder uniquement aux ressources nécessaires pour exécuter leurs fonctions professionnelles.

Par rapport aux systèmes de contrôle d'accès discrétionnaire (DAC), RBAC offre une approche plus flexible et évolutive de la gestion de l'accès aux ressources. Dans un système RBAC, chaque utilisateur se voit attribuer un ou plusieurs rôles, et chaque rôle se voit attribuer un ensemble d'autorisations. L'accès aux ressources est accordé en fonction du rôle attribué à l'utilisateur plutôt que de son identité ou de sa propriété de la ressource.

Les systèmes RBAC sont généralement utilisés dans des environnements comportant de nombreux utilisateurs.


## Hardening 

#### SELinux

C'est un MAC. Il est conçu pour fournir un contrôle d’accès précis aux ressources système et aux applications. SELinux fonctionne en appliquant une politique qui définit les contrôles d'accès pour chaque processus et fichier du système. Il offre un niveau de sécurité plus élevé en limitant les dommages que peut causer un processus compromis.

|     |                                                                                                              |
| --- | ------------------------------------------------------------------------------------------------------------ |
| 1.  | Install SELinux on your VM.                                                                                  |
| 2.  | Configure SELinux to prevent a user from accessing a specific file.                                          |
| 3.  | Configure SELinux to allow a single user to access a specific network service but deny access to all others. |
| 4.  | Configure SELinux to deny access to a specific user or group for a specific network service.                 |

#### AppArmor

C'est est un MAC. AppArmor est implémenté en tant que module de sécurité Linux (LSM) et utilise des profils d'application pour définir les ressources auxquelles une application peut accéder. AppArmor est généralement plus facile à utiliser et à configurer que SELinux, mais peut ne pas fournir le même niveau de contrôle précis.

|     |                                                                                                               |
| --- | ------------------------------------------------------------------------------------------------------------- |
| 5.  | Configure AppArmor to prevent a user from accessing a specific file.                                          |
| 6.  | Configure AppArmor to allow a single user to access a specific network service but deny access to all others. |
| 7.  | Configure AppArmor to deny access to a specific user or group for a specific network service.                 |

#### TCP Wrappers

Les wrappers TCP sont un mécanisme de contrôle d'accès au réseau basé sur l'hôte qui peut être utilisé pour restreindre l'accès aux services réseau en fonction de l'adresse IP du système client. Il fonctionne en interceptant les requêtes réseau entrantes et en comparant l'adresse IP du système client aux règles de contrôle d'accès. Ceux-ci sont utiles pour limiter l’accès aux services réseau à partir de systèmes non autorisés.

|     |                                                                                                    |
| --- | -------------------------------------------------------------------------------------------------- |
| 8.  | Configure TCP wrappers to allow access to a specific network service from a specific IP address.   |
| 9.  | Configure TCP wrappers to deny access to a specific network service from a specific IP address.    |
| 10. | Configure TCP wrappers to allow access to a specific network service from a range of IP addresses. |

Les wrappers TCP utilisent les fichiers de configuration suivants :

    /etc/hosts.allow

    /etc/hosts.deny

En bref, le fichier `/etc/hosts.allow` spécifie quels services et hôtes sont autorisés à accéder au système, tandis que le fichier `/etc/hosts.deny` spécifie quels services et hôtes ne sont pas autorisés à accéder.
##### Exemples

/etc/hosts.allow
```bash
# Allow access to SSH from the local network
sshd : 10.129.14.0/24

# Allow access to FTP from a specific host
ftpd : 10.129.14.10

# Allow access to Telnet from any host in the inlanefreight.local domain
telnetd : .inlanefreight.local
```

/etc/hosts.deny
```bash
# Deny access to all services from any host in the inlanefreight.com domain
ALL : .inlanefreight.com

# Deny access to SSH from a specific host
sshd : 10.129.22.22

# Deny access to FTP from hosts with IP addresses in the range of 10.129.22.0 to 10.129.22.255
ftpd : 10.129.22.0/24
```
