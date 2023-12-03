#fichiers 
## cat
`cat` est une commande historique qui permet de prendre un ou plusieurs flux de données en paramètre d'entrée, et de les afficher à l'écran sur le terminal.

>[!Info]
>"**cat**" provient de l'abréviation de "**catenate**" en anglais, qui signifie "concaténer". Il est effectivement possible de concaténer plusieurs flux de données passés en paramètre d'entrée avec cette commande.

Exemple:
```bash
cat /etc/os-release /etc/passwd
```

Voir les numéro de ligne
```bash
cat -n /chemin_fichier
```

## less
La commande `less` permet également d'afficher un fichier mais de manière paginé. 

Exemple:
```bash
less /etc/pam.d/login
```
- "barre espace"/"b" pour afficher la page suivante/précédente
- "-N" pour afficher les lignes
- "G" pour aller à la fin
- "g" pour aller au début
- "10g" pour se déplacer à la ligne 10
- "10" pour avancer de 10 lignes
- "/" pour rechercher des mots
	- "n"/"N" pour aller aux occurrences suivantes/précédentes

> Le fichier /etc/pam.d/login est le fichier de configuration du processus d'authentification.

`less` a été codée après `more` par Mark Nudelman qui souhaitait apporter plus de fonctionnalités.. 

>[!Citation]
>“Less is more, but more more than more is, so more is less less, so use more less if you want less more.” - [**Slackware Linux Essentials**](https://www.slackbook.org/html/file-commands-pagers.html)

## Head
Pour voir les premières lignes (10 par défaut).
```bash
head /etc/passwd
```

## Tail
Pour voir les dernières lignes (10 par défaut).
```bash
tail /etc/passwd
```

## Sort
Pour trier (alphabétique, numérique..). Par défaut alphabétique
```bash
cat /etc/passwd | sort
```

## grep
Permet de chercher un pattern dans un fichier.
Chercher "debian" dans le fichier os-release:
```bash
grep debian /etc/os-release
grep -n debian /etc/os-release # pareil mais avec les lignes
grep -i debian /etc/os-release # pas sensible à la casse. Peut trouver "debian" et "DebiAn"
grep -o debian /etc/os-release # affiche que le patterne rechercher
grep -r debian /etc/* # récursif donc tout les fichier dans /etc/
grep -nior debian /etc/* # tout
grep -v "nologin\|false" /etc/passwd # exclu les lignes contenant "nologin" ou "false"
```

## sed
Permet de proposer des transformations des flux. 
Remplacer un patterne par un autre:
```bash
sed 's/Debian/Ubuntu/' /etc/os-release # remplace "Ubuntu" par "Debian" dans le fichier os-release. Ne modifie pas le fichier.
sed 's/[D|d]ebian/Ubuntu/' # pareil mais pour "debian" et "Debian"
sed -i 's/Debian/Ubuntu/' /etc/os-release # pour appliquer la modification. Modifie le fichier.
```

## Awk
Affiche que le premier et le dernier élément de chaque ligne:
```bash
cat /etc/passwd | awk '{print $1, $NF}'
```

## Cut
Permet de spécifier un "délimiteur" sur lequel on peut jouer pour avoir un output différent.
Ici, on défini le ":" comme un délimiteur. Puis on affiche que la première partie de chaque ligne:
```bash
head /etc/passwd | cut -d":" -f1
```

## Tr
Change ":" en " " dans /etc/passwd
```bash
cat /etc/passwd | tr ":" " "
```

## Column
Pour mettre bien en colonne (tableau)
```bash
cat /etc/passwd | column -t
```

## Wc
Affiche le nombre de lignes.
```bash
cat /etc/passwd | wc -l
```


## RegEx (Regular expressions)

### Grouping Operators

|     | **Operators** | **Description**                                                                                                                                                             |
| --- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | `(a)`         | Used to group parts of a regex. Within the brackets, you can define further patterns which should be processed together.                             |
| 2   | `[a-z]`       | Used to define character classes. Inside the brackets, you can specify a list of characters to search for.                                          |
| 3   | `{1,10}`      | Used to define quantifiers. Inside the brackets, you can specify a number or a range that indicates how often a previous pattern should be repeated. |
| 4   | `|`          | Also called the OR operator and shows results when one of the two expressions matches                                                                                       |
| 5   | `.*`          | Also called the AND operator and displayed results only if both expressions match                                                                                           |

Cela évite notamment d'utiliser plusieurs grep à la suite.

Chercher "my" ou "false". Le -E pour appliquer le regex étendu.
```bash
grep -E "(my|false)" /etc/passwd
```
Chercher "my" et "false".
```bash
grep -E "(my.*false)" /etc/passwd
```

