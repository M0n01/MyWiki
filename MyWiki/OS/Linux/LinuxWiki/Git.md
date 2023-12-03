#git #github #gitlab

**Git** : Gestionnaire de versions. Permet de conserver un historique des modifications et des versions des fichiers (dépôt local). 

**Dépôt local** : Entrepôt virtuel du projet. Permet d'enregistrer les versions du code et d'y accéder au besoin.
 
**Dépôt distant** : Permet de stocker les versions du code afin de garder un historique délocalisé. On peut avoir plusieurs dépôts distants avec des droits différents. Centralise de travail de chaque dev.

- *Dépôt publique* : Tout le monde peut collaborer au projet.
- *Dépôt privé* : Seul le propriétaire a accès au projet et peut décider des personnes qui pourront avoir accès au repo.

**Copie locale** : Clone du dépôt distant. Là où on fait les modifications du code.

## Plateformes

### GitHub

Service en ligne qui héberge les dépôts Git (dépôt distant). Outil de communication et de collaboration. C’est une interface web créée pour faciliter l’interaction avec Git. Considéré comme un véritable réseau social, et permet de contribuer à des projets open source.

### GitLab

Principale alternative à GitHub depuis le rachat de GitHub par Microsoft. 

### Bitbucket

Version de Atlassian.

## Configuration initiale

### Faire un dépôt local ou cloner un dépôt existant

1) Se mettre dans le dossier du projet
2) Set up user et email du compte git
```bash
git config --global user.name "nom"
git config --global user.email "email"
```
3) Initialiser le projet git
```bash
git init  // crée un dossier caché qui contient sa conf
```
4) Vérifier les paramètres
```bash
git config --list
```
5) Mettre en Stage
```bash
git add nom_fichier/dossier
git add . ## Si on veut ajouter tout le contenu
```
6) Commit
```bash
git commit -m "commentaire"
```

Voir l'état git
```bash
git status  
```

Lier le repos local au repos distant
```bash
git remote add origin https://github.com/../projet.git
```

Sélectionner la branche principale
```bash
git branch -M main
```

Push
```bash
git push -u origin main
```

## Mettre à jour

Indexer le fichier 
```bash
git add fichier
```

Créer une nouvelle version
```bash
git commit -m "commentaire"
```

Envoyer la nouvelle version sur le dépôt distant
```bash
git push origin main
```


![[git1.png]]

- **Le Working directory** : Cette zone correspond au dossier du projet l'ordinateur.
- **Le Stage ou index** : Intermédiaire entre le working directory et le repository. Elle représente tous les fichiers modifiés que vous souhaitez voir apparaître dans votre prochaine version de code.
- **Le Repository** : Lorsque l’on crée de nouvelles versions d’un projet, c’est dans cette zone qu’elles sont stockées.

Ces 3 zones sont donc présentes dans votre ordinateur, en local.

## Les branches

Une branche est une “copie” d’un projet sur laquelle on opère des modifications de code.
La branche principale (main ou master) portera l’intégralité des modifications effectuées. Le but n’est donc pas de réaliser les modifications directement sur cette branche, mais de les réaliser sur d’autres branches, et après divers tests, de les intégrer sur la branche principale.

Git va créer une branche virtuelle, mémoriser tous les changements, et seulement quand on le souhaite, les ajouter au projet principale après avoir vérifier qu'il n’y a pas de conflits avec d’autres fusions.

La liste des branches
```bash
git branch
```
L'étoile indique sur laquelle on se trouve

Créer une nouvelle branche
```bash
git branch nom_branche
```

Supprimer une branche
```bash
git branch -d nom_branch
git branch -d nom_branch ## supression branche + tous les fichiers et modifications qu'on aura pas commités sur cette branche 
```

Basculer sur une branche
```bash
git checkout nom_branche
```

On peut y créer des fichiers et les commit.

Push une branche
```bash
git push -u origin nom_branche
```

Fusionner une branche avec une autre :
- Se placer sur la branche de destination (ici main)
- Merge
```bash
git checkout main
git merge nom_branche
```

## Fetch

- **La commande git fetch** va récupérer toutes les données des commits effectués sur la branche courante qui n'existent pas encore dans votre version en local. Ces données seront stockées dans le répertoire de travail local mais ne seront pas fusionnées avec votre branche locale. Si vous souhaitez fusionner ces données pour que votre branche soit à jour, vous devez utiliser ensuite la commande git merge.

## Merge Commits

Au lieu de fusionner toute une branche avec une autre, on peut fusionner  1 ou plusieurs commit de cette branche.

1) Faire un git log pour récupérer le commit qui nous intéresse
```bash
git log
```
- On copie son hash

2) On bascule sur la branche qui va recevoir la fusion (ici main)
```bash
git checkout main
```

3) Appliquer 
```bash
git cherry-pick hash_du_commit
```
- Pour le faire avec plusieurs commit, il faut ajouter les hash à la suite

>[!Remarque]
>`git cherry-pick`  n'est pas une commande très appréciée dans la communauté des développeurs car elle duplique **des commits existants**. Il est plutôt conseillé de réaliser un merge.

## Pull

**git pull** est en fait la commande qui regroupe les commandes git fetch suivie de git merge. Cette commande télécharge les données des commits qui n'ont pas encore été récupérées dans votre branche locale puis fusionne ensuite ces données.

Cloner (copier) un dépôt distant en local
```bash
git clone url_du_repos
```

Dire au dépôt que l'on pointe vers un dépôt distant et créer un raccourci pour un dépôt avec un nom trop long
```bash
git remote add surnom url_du_repos
```

Mettre à jour le dépôt en local (ici la branche main)
```bash
git pull origin main
```

Dans un contexte pro la branche principale est souvent bloquée. On ne peut pas push sans que le code ne soit vérifié. On ne peut donc pas fusionner nos modifications nous même.

Une **pull request**, ou _demande de pull_, en français, est une fonctionnalité de GitHub qui permet de demander aux propriétaires d’un repository l’autorisation de fusionner nos changements sur la branche principale ou toute autre branche sur laquelle on souhaite apporter nos modifications.

Donc si on crée une pull request, on a au préalable :
1. Créé une nouvelle branche.
2. Envoyé votre code sur cette même branche.

Lorsque ces deux conditions sont remplies, un bandeau apparaît sur la page github pour nous suggérer de créer une pull request.

Une fois la Pull Request faite, on peut cliquer sur "Reviewers" pour demander à quelqu'un de relire notre code.

## Remise (mauvaise modif avant un commit)

Avant un commit donc après avoir fait un 
```bash
git add nom_fichier
```

On se rend compte qu'on est pas sur la bonne branche.
On peut donc créer une "remise". 
Une remise permet de mettre de côté des modifications dans le but de les réutiliser plus tard.

Faire une remise
```bash
git stash
```

Vérifier
```bash
git status
## on doit voir qu'il n'y a plus rien à commit
```

On crée et on va sur la branche sur laquelle on veut faire le commit
```bash
git branch branche1
git checkout branche1
```

Voir la liste des remises
```bash
git stash list
```

Appliquer la remise
```bash
git stash apply ## applique le dernier stash qui a été fait
## ou
git stash apply stash@{1} ## stipule le stash 1
```

## Mauvaise modif après un commit

Analyser les derniers commits
```bash
git log commit
```
On obtient notre id

Supprimer de la branche principale votre dernier commit.
```bash
git reset --hard HEAD^
```

Le HEAD^ indique que c'est bien le dernier commit que nous voulons supprimer. L’historique sera changé, les fichiers seront supprimés.

Créer et basculer sur une nouvelle branche
```bash
git branch brancheCommit
git checkout brancheCommit
```

Appliquer ce commit sur la nouvelle branche
```bash
git reset --hard identifiant ## ca83a6df par exemple (pas besoin de mettre tout l'identifiant)
```

## Modifier message commit

Après avoir fait mauvais message sur un commit.

Modifier le message d'un commit
```bash
git commit --amend -m "nouveau message"
```

## Fichier oublié après commit

Si on a oublié de mettre un fichier avant de commit.

Ajouter fichier à un commit
```bash
git add monFichierOublié
git commit --amend --no-edit ## ajoute les modif sans modifier les message du commit
```

## Push une erreur, comment revenir en arrière ?

Il est possible d'annuler son commit public avec la commande `git revert`. L'opération revert annule un commit en créant un nouveau commit. C'est une méthode sûre pour annuler des changements, car elle ne risque pas de réécrire l'historique du commit.

![[git_reset.png]]

Revert le dernier commit public (cela a créé un nouveau commit d'annulation)
```bash
git revert HEAD^
```
Une fois le commit "annulé", on peut enlever le fichier qu'on ne souhaitait pas commit et réaliser de nouveau le commit.

il vaut mieux utiliser  `git revert`  pour annuler des changements apportés à une branche publique, et  `git reset`  pour faire de même, mais sur une branche privée.

>[!Remarque]
`git revert`  sert à annuler des changements commités. Il va créer un nouveau commit.
`git reset`   permet d'annuler des changements non commités. Il va revenir à l'état précédent sans créer un nouveau commit.

![[revertVSreset.jpg]]

`git revert`  peut écraser les fichiers dans le répertoire de travail, il sera donc demandé de commiter les modifications ou de les remiser.

La commande    `git reset`  est un outil complexe et polyvalent pour **annuler les changements**. Elle peut être appelée de trois façons différentes, qui correspondent aux arguments de ligne de commande **--soft, --mixed et --hard**.

![[git_revert_HARD_etc.png]]

### Git reset HARD

**Vérifier 5 fois avant lancer et être sûr à 100 % !**

Revenir à n'importe quel commit mais en oubliant absolument tout ce qu'il s'est passé après
```
git reset notreCommitCible --hard
```

### Git reset MIXED

`git reset --mixed`  va permettre de revenir juste après le dernier commit ou le commit spécifié, sans supprimer les modifications en cours. Il permet aussi, dans le cas de fichiers indexés mais pas encore commités, de désindexer les fichiers.
```bash
git reset HEAD~
```

Si rien n'est spécifié après    `git reset`  , par défaut il exécutera un    `git reset --mixed HEAD~`  .

>[!Remarque]
>Le **HEAD** est un pointeur, une référence sur notrer position actuelle dans notrer répertoire de travail Git. Par défaut, HEAD pointe sur la branche courante, main/master, et peut être déplacé vers une autre branche ou un autre commit.

### Git reset SOFT

Cette commande permet de se placer sur un commit spécifique afin de voir le code à un instant donné, ou de créer une branche partant d'un ancien commit. Elle ne supprime aucun fichier, aucun commit, et ne crée pas de HEAD détaché.

## Gérer un conflit

- Exemple : Vous avez modifié du code pour afficher le message "Une super cagnotte !" alors qu'était déjà en place le message "Une cagnotte". Lorsque vous allez fusionner les deux branches, Git va voir que sur la même ligne on essaie de fusionner deux choses différentes. Git va donc afficher un conflit sur le fichier cagnotte.php et arrêtera le processus de fusion ou merge.

Réglez les conflits en comparant les deux lignes et en choisissant quelle modification vous voulez garder. 
Puis
```bash
git add cagnotte.php git commit
```
Git va détecter que vous avez résolu les conflits et va vous proposer un message de commit. Vous pouvez bien entendu le modifier.

## Log

- savoir qui a contribué à quoi 
- déterminer où des bugs ont été introduits 
- annuler les changements problématiques

Afficher les logs (les commits les plus récents en premier)
```bash
git log
```
Cette commande affiche chaque commit avec son **identifiant SHA**, l'auteur du commit, la date et le message du commit.

Afficher logs des commits ainsi que toutes les autres actions faite en local : modifications de messages, merges, resets, etc
```bash
git reflog
```

Pour revenir à une action donnée
```bash
git checkout huit_premiers_caractères_du_SHA  ## exemple : e789e7c
```

Voir les log sur un fichier particulier (qui a modifié quoi)
```bash
git blame nom_fichier
```
`git blame`  va afficher pour chaque ligne modifiée :
- son ID 
- l'auteur 
- l'horodatage 
- le numéro de la ligne 
- le contenu de la ligne

## SSH

créer une paire de clé
```bash
ssh-keygen -t rsa -b 4096 -C "adressemail@exemple.com"
```
- Copier la clé publique et la coller dans github


![[Résumé_git.png]]


