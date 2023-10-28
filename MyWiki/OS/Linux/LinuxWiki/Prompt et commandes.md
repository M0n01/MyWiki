#path #shell #bash
La configuration du prompt est stockée dans un fichier et dans une variable gérée par le shell Bash. En l'occurrence, cette variable est nommée $PS1.

La variable $SHELL indique sur quel shell vous êtes connecté.
```bash
echo $SHELL
```
Pour connaître la version de bash
```bash
/bin/bash --version
```

Variable $PS1
```bash
echo $PS1
```

>[!Remarque]
>Un prompt est sous la forme user@hostname.
>Le $ à la fin du prompt = utilisateur standard
>Le # à la fin du prompt = root

Pour modifier la variable du prompt
```bash
export PS1="\u-\h:\$"  // par exemple
```
Ne fonctionne que pour la session courante. Le prompt redeviens comme avant à l'ouverture d'un nouveau bash.

Pour réellement modifier le prompt il faut modifier le fichier de configuration.
Soit en modifiant directement la variable $PS1 soit en faisant un export à la fin du fichier.

Il se trouve dans le répertoire de l'utilisateur */home/user/*. C'est le fichier *.bashrc*.

>[!Voir]
>Les syntaxes des commandes sont décrites dans un document de normalisation POSIX nommé [Utility Conventions](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html). Par exemple :
>- le nom de la commande doit comporter entre 2 et 9 caractères 
>- le nom de la commande doit comporter uniquement des caractères minuscules et des chiffres 
>- les options de la commande doivent être définies avec un seul caractère alphanumérique 
>- les options doivent être précédées du caractère **-** 
>- etc.
>
>**Ce document précise aussi le comportement général des commandes d’un shell**

Dans une commande il faut faire la différence entre une option et un argument.

Info sur une commande
```bash
type nom_commande
```

Info sur un fichier
```bash
file nom_fichier
```

Combiner les commandes (avec &&)
```bash
cd /dossier/ && ls   // exemple
```

Info sur utilisateur
```bash
id
```

2 types de commandes :

- Les **commandes internes** : Elles sont livrées avec l'interpréteur de commandes directement, comme une primitive du shell. 

- Les **commandes externes** : Elles ne sont pas fournies par le shell. Elles sont généralement présentes sur le système sous la forme d'un fichier compilé binaire ou d'un fichier disposant des droits d'exécution (comme un script Perl par exemple ou le fichier /bin/ls).

Comme le shell bash fait-il pour accéder aux commandes externes ?
Il va chercher la commande dans les répertoires qui sont référencer par la variable $PATH.
```bash
echo $PATH
```

Lorsque l'on créé une commande, on peut l'exécuter mais on doit spécifier son chemin.
```bash
./testcommand     // si je suis dans le même répertoire
```

Si je veux pouvoir exécuter cette commande comme les autres il faut que j'ajoute le chemin de celle-ci dans la variable $PATH du fichier *.bashrc*.

Faire un alias : éditer fichier *.bashrc*

Lister tous les alias
```bash
alias
```

