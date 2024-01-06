> Gaël le Maréchal
### 1. Exclusion Mutuelle

Ce TP vous permet de comprendre les concepts de base de l'exclusion mutuelle entre processus, y compris la création de processus, l'utilisation de variables partagées et la synchronisation des processus à l'aide de verrous. Il vous permet également de comprendre comment gérer les conflits d'accès simultané à des variables partagées.

L'exclusion mutuelle entre processus peut se faire en utilisant la bibliothèque multiprocessing  de Python et plus précisément son mécanisme de verrous:

1. Importez la bibliothèque multiprocessing
```python
import multiprocessing
```

2. Créez deux processus, appelés processusA et processusB, en utilisant la classe multiprocessing.Process
```python
processusA = multiprocessing.Process(target=incrementer)
processusB = multiprocessing.Process(target=incrementer)
```

3. Créez une variable partagée, appelée compteur, en utilisant la classe multiprocessing.Value
```python
compteur = multiprocessing.Value('i', 0)
```

4. Le processusA incrémente la variable compteur de 1 dans une boucle de 100 itérations
5. Le processusB incrémente la variable compteur de 1 dans une boucle de 100 itérations
6. Utilisez la classe multiprocessing.Lock pour verrouiller la variable compteur lorsque les processus y accèdent pour éviter les conflits d'accès simultané.
```python
def incrementer():
    for i in range(100):
        with multiprocessing.Lock():
            compteur.value += 1
```

7. Lancez les processus processusA et processusB en utilisant les méthodes start()
```python
processusA.start()
processusB.start()
```

8. Utilisez la méthode join() pour attendre la fin des processus
```python
processusA.join()
processusB.join()
```

9. Affichez la valeur finale de compteur pour vérifier que la somme des incrémentations des deux processus est égale à 200
```python
print("Valeur finale du compteur :", compteur.value)
```

Voici le code complet :
```python
import multiprocessing

def incrementer():
    for i in range(100):
        with multiprocessing.Lock():
            compteur.value += 1

compteur = multiprocessing.Value('i', 0)

processusA = multiprocessing.Process(target=incrementer)
processusB = multiprocessing.Process(target=incrementer)

processusA.start()
processusB.start()

processusA.join()
processusB.join()

print("Valeur finale du compteur :", compteur.value)
```

### 2. Producteur Consommateur

Dans ce TP vous allez :  

- Implémentez un système de producteur-consommateur en utilisant des threads en Python
- Utilisez des sémaphores pour synchroniser l'accès à une zone de mémoire partagée qui contiendra les données produites
- Trouver une solution sans sémaphores ?

Rappel : Les 2 processus coopèrent en partageant un même tampon. Le producteur produit des objets qu’il dépose dans le tampon, le consommateur les retire du tampon pour les consommer.

Il y a plusieurs problématiques à gérer :

- Le producteur et le consommateur ne peuvent pas accéder au tampon en même temps
- Le producteur veut déposer un objet alors que le tampon est plein.
- Le consommateur veut retirer un objet du tampon alors qu’il est vide

La solution classique est d’introduire un sémaphore pour cela :  

1. Créez une liste partagée appelée "buffer" qui stockera les données produites
```python
buffer = []
```

2. Créez un semaphore pour protéger l'accès au buffer
```python
semaphore = threading.Semaphore(1)
```

3. Implémentez une fonction appelée "producteur" qui produit des données aléatoires toutes les secondes et les ajoute au buffer. Utilisez le semaphore pour protéger l'accès au buffer lorsque vous y ajoutez des données.
```python
def producteur(): # Remplissage de données aléatoire
    while True:
        time.sleep(1)
        semaphore.acquire() # verrou pour protéger l'accès
        buffer.append(random.randint(1, 2)) # ajoute une donnée
        print(f"Produit : {donnee}, Buffer : {buffer}")
        semaphore.release() # libère le verrou
        if len(buffer) >= 5: # Buffer plein si plus de 5 données
            print("BUFFER PLEIN")
            break
```

4. Implémentez une fonction appelée "consommateur" qui consomme les données en tête du buffer toutes les deux secondes. Utilisez le semaphore pour protéger l'accès au buffer lorsque vous consommez des données.
```python
def consommateur(): # Consommation toute les 2 secondes
    while True:
        time.sleep(2)
        semaphore.acquire() # verrou pour protéger l'accès
        if buffer: #retire 1er donnée de "buffer" si il contient des données
            donnee_consomme = buffer.pop(0) 
            print(f"Consommé : {donnee_consomme}, Buffer : {buffer}")
        semaphore.release() # libère le verrou 
# Si le buffer est vide, on termine le consommateur 
		if not buffer: 
			print("BUFFER VIDE")
			break
```

5. Créez un thread pour lancer la fonction "producteur" et un autre thread pour lancer la fonction "consommateur"
```python
thread_producteur = threading.Thread(target=producteur)
thread_consommateur = threading.Thread(target=consommateur)
```

6. Utilisez les méthodes join() pour attendre la fin de l'exécution des threads.
```python
thread_producteur.join()
thread_consommateur.join()
```

7. Une autre solution pour ne pas utiliser de sémaphore serait d'utiliser une variable d’état précisant si les données sont disponibles ou non. Pourquoi cela fonctionne moins bien ?

Il pourrait y avoir des cas où un thread pense que des données sont disponibles, mais lorsqu'il tente de les consommer, elles ont déjà été consommées par un autre thread. Cela peut conduire à un blocage potentiel.

8. Ajoutez un verrou d’accès au buffer et arrêtez le producteur quand le buffer est plein pour que le programme puisse se terminer.
```python
semaphore.acquire() # verrou pour protéger l'accès
semaphore.release() # libère le verrou
```
J'aurais également pu utiliser **`threading.Lock()`**.

```python
import threading
import time
import random

# Liste partagée "buffer" pour stocker les données produites
buffer = []

# Sémaphore pour protéger l'accès au buffer en même temps de producteur et consommateur
semaphore = threading.Semaphore(1)

def producteur(): # Remplissage de données aléatoire
    while True:
        time.sleep(1)
        semaphore.acquire() # verrou pour protéger l'accès
        buffer.append(random.randint(1, 2)) # ajoute une donnée aléatoire
        print(f"Produit : {donnee}, Buffer : {buffer}")
        semaphore.release() # libère le verrou
        if len(buffer) >= 5: # Buffer plein si plus de 5 données
            print("BUFFER PLEIN")
            break
            
def consommateur(): # Consommation toute les 2 secondes
    while True:
        time.sleep(2)
        semaphore.acquire() # verrou pour protéger l'accès
        if buffer: #retire 1er donnée de "buffer" si il contient des données
            donnee_consomme = buffer.pop(0) 
            print(f"Consommé : {donnee_consomme}, Buffer : {buffer}")
        semaphore.release() # libère le verrou  
		if not buffer: # Si le buffer est vide, on termine le consommateur
			print("BUFFER VIDE")
			break

# Création des threads
thread_producteur = threading.Thread(target=producteur)
thread_consommateur = threading.Thread(target=consommateur)

# Démarrage des threads
thread_producteur.start()
thread_consommateur.start()

# Attente de la fin des threads
thread_producteur.join()
thread_consommateur.join()
```