#processus #cpu #processeur

## Listez les processus en cours sur le système

Package **`procps`**, fournit beaucoup de commandes pour auditer l'activité du système :
- **`ps`**
- **`kill`**
- **`top`**
- etc

Liste de tout les processus:
```bash
ps -ef
```
Filtre pour l'utilisateur seb:
```bash
ps -edf | grep seb
```
Les 5 processus qui consomme le plus de mémoire:
```bash
ps -edf --sort=+pmem | tail -5
```
Les 5 processus qui consomme le plus de temps cpu:
```bash
ps -edf --sort=+pcpu | tail -5
```

## Affichez la hiérarchie des processus

>[!Info]
>Lorsque la machine démarre, Linux lance un premier processus qui aura un ID=1 (**`init`**  ou **`systemd`** en fonction du type et de l'âge de la distribution). Tous les autres processus seront des fils et petits-fils de ce processus.

- Arborescence des processus = quel processus est le père de quel autre processus.

Afficher sous forme d'arbre:
```bash
ps -edf --forest
```
Afficher sous forme d'arbre résumé:
```bash
pstree
```
- vient du package `psmisc`.

## Gestionnaires de tâches sous Linux en mode terminal

- Commande **`top`** = équivalent du gestionnaire des tâches de windows.

Afficher gestionnaire des tâches:
```bash
top
```
- touche `h` pour afficher l'aide
- `f` pour voir et sélectionner ce qui s'affiche

`top` améliorer:
```bash
htop
```

## Modifiez la priorité d’un processus

Le champ **`NI`**(pour **NICE**) correspond à l'affichage de la valeur numérique du facteur de priorité et sous Linux :
- L'intervalle numérique de priorité va de **-20 à 20,**
- Plus la valeur de la priorité est élevée, moins le processus est prioritaire. Un moyen mnémonique très simple de se souvenir de ce fait est de traduire NICE par gentil en français, et de considérer que **plus un processus est gentil, plus il laisse les autres occuper du temps CPU avant lui,**
- Un processus plus prioritaire occupera plus de temps CPU et de ressources du système qu'un processus moins prioritaire.

Sans indication contraire, les processus se lancent naturellement avec une priorité intermédiaire valant 0.

Tous les utilisateurs peuvent modifier uniquement à la baisse (c'est-à-dire en augmentant la valeur) la priorité de leur processus. Seul **`root`** peut modifier à la hausse la priorité d'un processus (c'est-à-dire en diminuant la valeur).

Dans la plupart des cas, on aura besoin de modifier la priorité d'un processus qui est déjà lancé :
1. soit pour l'augmenter afin de terminer ses tâches plus rapidement,
2. soit pour la diminuer car il bloque l'exécution normale de ses petits copains sur le système.

Augmenter la priorité du processus 588 (PID) (il sera donc moins prioritaire):
```bash
renice +10 588
```
Augmenter la priorité du processus via la commande `top`:
```bash
top
```
- Touche R
- Rentrer PID
- Rentrer la nouvelle valeur de priorité

## Déclenchez manuellement la terminaison d’un processus

>[!Info]
>il est possible d'envoyer une information à un processus en cours d'exécution. Cette information est nommée un **signal**.

- Le noyau Linux peut envoyer des signaux à ces processus
- Un processus peut envoyer un signal à un autre processus
- Et un processus peut même s’envoyer à lui-même un signal

Sous Linux, il existe 30 signaux principaux normalisés, numérotés de 1 à 30. Ainsi lorsqu'un processus reçoit un signal, en fonction de sa nature, il peut l’"**écouter**" et appliquer son message ou bien **l’ignorer**.

Il faut bien comprendre que l'interprétation d'un signal est de la responsabilité du processus qui le reçoit. Ce dernier peut tout à fait être codé en ignorant un ou plusieurs signaux (sauf 1 signal particulier), mais c'est très rarement le cas.

3 signaux permettant d'interrompre le fonctionnement d'un processus :
1. SIGINT (signal numéro 2)
2. SIGTERM (signal numéro 15)
3. SIGKILL (signal numéro 9)

#### SIGINT et SIGTERM
Ces deux signaux indiquent au processus sa terminaison de façon propre. Autrement dit : il y a ici une volonté à ce que ce soit le processus lui-même qui se termine, en libérant correctement toutes les ressources qu'il occupe à cet instant.
SIGINT correspond généralement à l'interruption clavier **CTRL+c**.

>[!Info]
>Ces deux signaux peuvent être ignorés par le processus cible (notamment les processus parent de tous les autres, **`init`** ou **`systemd`** ou encore tous les processus gérés directement par le noyau Linux).

Tuer via SIGINT sans CTRL+C:
```bash
kill -2 PID  # -2 pour SIGINT
```

#### SIGKILL

C’est le signal destructeur par définition. Souvent, il est envoyé lorsque les deux précédents n'ont pas fonctionné. Le processus ciblé est terminé brutalement :
- pas forcément aussi proprement qu'avec SIGINT ou SIGTERM,
- et les ressources associées ne sont pas libérées proprement.

>[!Info]
>Les processus ne peuvent pas ignorer ce signal. Tout simplement parce qu'il ne leur est pas adressé directement. SIGKILL est envoyé au processus principal de Linux (**`init`** ou **`systemd`**) qui se charge, lui, de tuer le processus cible.

Tuer le processus père (force la fin du processus fils):
```bash
kill -9 PID  # -2 pour SIGKILL
```
