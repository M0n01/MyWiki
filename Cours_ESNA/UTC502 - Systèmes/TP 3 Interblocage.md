>Gaël le Maréchal

Dans ce programme, les processus A et B utilisent des verrous pour protéger l'accès aux ressources partagées. Le processus A acquiert le verrou1, puis essaie d'acquérir le verrou2, mais celui-ci est déjà verrouillé par le processus B, qui lui-même essaie d'acquérir le verrou1. Les deux processus se bloquent mutuellement en attendant l'accès aux verrous nécessaires, créant ainsi un interblocage.

Exemple de programme :
```bash
from multiprocessing import Process, Lock  
import time  
  
def processA(verrou1, verrou2):  
    verrou1.acquire()  
    print("Process A : verrou1 acquis")  
    time.sleep(1)  
    print("Process A : en attente du verrou2")  
    verrou2.acquire()  
    print("Process A : verrou2 acquis")  
    verrou2.release()  
    print("Process A : verrou2 relâché")  
    verrou1.release()  
    print("Process A : verrou1 relâché")  
  
def processB(verrou1, verrou2):  
    verrou2.acquire()  
    print("Process B : verrou2 acquis")  
    time.sleep(1)  
    print("Process B : en attente du verrou1")  
    verrou1.acquire()  
    print("Process B : verrou1 acquis")  
    verrou1.release()  
    print("Process B : verrou1 relâché")  
    verrou2.release()  
    print("Process B : verrou2 relâché")  
  
if __name__ == '__main__':  
    verrou1 = Lock()  
    verrou2 = Lock()  
    processA = Process(target=processA, args=(verrou1, verrou2))  
    processB = Process(target=processB, args=(verrou1, verrou2))  
    processA.start()  
    processB.start()  
    processA.join()  
    processB.join()
```

1.       Expliquez pourquoi il y a interblocage.

Il y a interblocage car aucun des deux processus ne peut progresser car chacun détient le verrou que l'autre processus attend.


2.       Essayez de trouver une solution toute simple sans modifiez le code des processus pour éviter cet interblocage.

En inversant l'ordre des verrou. Le processusA va utiliser le verrou1 et le processusB également, le processusB va donc attendre. Ainsi le processusA s’exécutera en entier puis le processusB s’exécutera en entier. 
```python
processA = Process(target=processA, args=(verrou1, verrou2))
processB = Process(target=processB, args=(verrou2, verrou1))
```

Voici le résultat :
![[Pasted image 20231221091317.png]]

