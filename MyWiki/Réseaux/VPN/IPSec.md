
IPSec est un standard IETF (RFC 2401-2412) qui définit comment un VPN peut être sécurisé sur des réseaux IP. IPSec protège et authentifie les paquets IP entre la source et la destination. **IPSec peut protéger le trafic de la couche 4 à la couche 7.**

*En utilisant la structure IPSec, IPSec fournit ces fonctions de sécurité essentielles:*

- **Confidentialité** 
>IPSec utilise des algorithmes de chiffrement pour empêcher les cybercriminels de découvrir le contenu des paquets.

- **Intégrité** 
>IPSec utilise des algorithmes de hachage pour garantir que les paquets n'ont pas été modifiés entre la source et la destination.

- **Authentification d'origine** 
>IPSec utilise le protocole IKE (Internet Key Exchange) pour authentifier la source et la destination. Les méthodes d'authentification, y compris l'utilisation des clés pré-partagées (mots de passe), de certificats numériques ou de certificats RSA.

- **Diffie-Hellman** 
>L'échange de clés sécurisées concerne généralement divers groupes de l'algorithme DH.

IPsec n'est lié à aucune règle spécifique concernant les communications sécurisées. Cette flexibilité du structure permet à IPSec d'intégrer facilement aux nouvelles technologies de sécurité sans mettre à jour les normes IPSec existantes. Les technologies actuellement disponibles sont alignées sur leur fonction de sécurité spécifique. Les emplacements ouverts dans la structure IPSec illustrés dans la figure peuvent être remplis avec n'importe quel choix disponibles pour cette fonction IPsec afin de créer une association de sécurité (SA) unique.

La figure indique les options de structure IPSec. Il y'a cinq fonctions IPSec, le protocole IPSec, la confidentialité, l'intégrité, l'authentification et Diffie-Hellman. Chaque fonction a des options qui sont disponible. Les options du protocole IPSec sont: AH, ESP ou ESP + AH. Les options de confidentialité sont: DES, 3DES, AES ou SEAL. Les options d'intégrité sont MD5 ou SHA. Les options d'authentification sont: PSK ou RSA. Les options de Diffie-Hellman sont DH1, DH2, DH5, etc.

![[IPSEC1.png]]

- **Protocole IPSec**
>Les choix pour le protocole IPSec incluent En-tête d'authentification (AH) ou Protocole de sécurité d'encapsulation (ESP). AH authentifie la couche 3 paquet. ESP chiffre le paquet de couche 3. **Remarque**: ESP + AH est rarement utilisé car cette combinaison n'a pas réussi à traverser un périphérique NAT.

- **Confidentialité**
>Le chiffrement garantit la confidentialité du paquet de couche 3. Les choix comprennent Data Encryption Standard (DES), Triple DES (3DES), Advanced Encryption Standard (AES), ou Software-Optimized Encryption Algorithm (SEAL). L'absence de cryptage est également une option.

- **Intégrité**
>Garantit que les données arrivent inchangées à la destination à l'aide d'un hachage algorithme, tel que message-digest 5 (MD5) ou Secure Hash Algorithm (SHA).

- **Authentification**
>IPSec utilise Internet Key Exchange (IKE) pour authentifier les utilisateurs et des appareils capables de communiquer indépendamment. IKE utilise plusieurs types d'authentification, y compris le nom d'utilisateur et le mot de passe, mot de passe à usage unique, biométrie, clés pré-partagées (PSK) et numérique certificats utilisant l'algorithme Rivest, Shamir et Adleman (RSA).

- **Diffie-Hellman**
>IPSec utilise l'algorithme DH pour fournir une méthode d'échange de clés publiques pour que deux homologues établissent une clé secrète partagée. Il y a plusieurs différents groupes au choix dont DH14, 15, 16 et DH 19, 20, 21 et 24. DH1, 2 et 5 ne sont plus recommandés.

La figure indique des exemples de SA pour deux implémentations différentes. Un SA est le bloc de construction de base d'IPSec. Lors l'établissement d'une liaison VPN, les homologues doivent partager la même SA pour négocier les paramètres d'échange de clés, établir une clé partagée, s'authentifier mutuellement et négocier les paramètres de chiffrement. Notez que l'exemple SA 1 n'utilise aucun chiffrement.

![[IPSECex.png]]


## Encapsulation

Le choix de l'encapsulation du protocole IPsec est le premier élément principale du structure. IPSec encapsule les paquets à l'aide de l'en-tête d'authentification (AH) ou du protocole de sécurité d'encapsulation (ESP).

Le choix de AH ou ESP détermine quels autres blocs de construction sont disponibles.

![[IPSec_ENC.png]]

- **AH** 
>Approprié que lorsque la confidentialité n'est pas requise ou autorisée. Il fournit l'authentification et l'intégrité des données, mais il n'assure pas la confidentialité des données (cryptage). Tout le texte est transporté non crypté.

- **ESP** 
>Fournit la confidentialité et l'authentification. Il assure la confidentialité en effectuant le cryptage sur le paquet IP. ESP fournit une authentification pour le paquet IP interne et l'en-tête ESP. L'authentification fournit l'authentification de l'origine des données et l'intégrité des données. Bien que le chiffrement et l'authentification soient facultatifs dans ESP, au moins l'un d'eux doit être sélectionné.






