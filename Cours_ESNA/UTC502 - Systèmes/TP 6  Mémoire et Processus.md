>Gaël le Maréchal

#### Instructions :

1. Tout d'abord, nous allons explorer comment partager de la mémoire entre les processus avec Python en utilisant le module mmap. Le code fourni dans la fonction sharing_with_mmap() créera une mémoire partagée de 100 octets et l'écrira dans un processus enfant. Dans le processus parent, nous attendrons 2 secondes et imprimerons la valeur de la mémoire partagée. Exécutez la fonction sharing_with_mmap() et observez la sortie.

>JumpToMemory.py
```python
import mmap  
import os  
import time  
import subprocess  
  
def mmap_io(filename):  
    with open(filename, mode="r", encoding="utf8") as file_obj:  
        with mmap.mmap(file_obj.fileno(), length=0, access=mmap.ACCESS_READ) as mmap_obj:  
            pycode = mmap_obj.read()  
            print(pycode)  
            result = subprocess.run(['python', '-c', pycode], stdout=subprocess.PIPE)  
            print(result.stdout.decode())  
  
def sharing_with_mmap():  
    BUF = mmap.mmap(-1,          # create anonymous memory in ram  
                    length=100,  # 100 bytes  
                    access=mmap.ACCESS_WRITE)  
  
    pid = os.fork()  
    if pid == 0:  
        # Child process  
        BUF[0:100] = b"a" * 100  
    else:  
        time.sleep(2)  
        print(BUF[0:100])  
   
sharing_with_mmap()
```
Après avoir appelé la fcontion sharing_with_map() on a ce résultat :
```python
b'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa'
```
On a donc 100 caractères 'a'.


2. Ensuite, nous allons explorer comment utiliser le module mmap pour exécuter du code Python stocké dans un fichier en mémoire partagée. La fonction mmap_io(filename) lit un fichier donné en utilisant mmap et exécute le code Python qu'il contient en utilisant la méthode subprocess.run(). Exécutez la fonction mmap_io() avec un fichier d'exemple "hello.py" contenant uniquement la commande _print("Hello, world!")_ et observez la sortie.

>JumpToMemory.py
```python
import mmap  
import os  
import time  
import subprocess  
  
def mmap_io(filename):  
    with open(filename, mode="r", encoding="utf8") as file_obj:  
        with mmap.mmap(file_obj.fileno(), length=0, access=mmap.ACCESS_READ) as mmap_obj:  
            pycode = mmap_obj.read()  
            print(pycode)  
            result = subprocess.run(['python', '-c', pycode], stdout=subprocess.PIPE)  
            print(result.stdout.decode())  
  
def sharing_with_mmap():  
    BUF = mmap.mmap(-1,          # create anonymous memory in ram  
                    length=100,  # 100 bytes  
                    access=mmap.ACCESS_WRITE)  
  
    pid = os.fork()  
    if pid == 0:  
        # Child process  
        BUF[0:100] = b"a" * 100  
    else:  
        time.sleep(2)  
        print(BUF[0:100])  
  
mmap_io('hello.py')  
```
>hello.py
```python
print('hello')
```
Voici le résultat 
```python
b"print('hello')"
hello
```


3. Maintenant, nous allons explorer comment jouer avec les droits en téléchargeant une charge depuis un serveur de commande et de contrôle (C&C) et en l'exécutant dans un processus. Tout d'abord, créez un fichier de script Python appelé "client.py". Ce fichier doit se connecter à un serveur de commande et de contrôle (C&C) et télécharger une charge à exécuter. Nous utiliserons la bibliothèque requests pour cela. Une fois que la charge est téléchargée, écrivez-la dans la mémoire partagée à l'aide de mmap. Enfin, utilisez subprocess.run() pour exécuter la charge dans un processus.

>client.py
```python
import subprocess  
import requests  
  
print("coucou")  
  
r = requests.get('https://pypi.org/project/requests/')  
print(r.status_code)  
print(r.headers['content-type'])
```
>JumpToMemory.py
```python
import mmap  
import os  
import time  
import subprocess  
  
def mmap_io(filename):  
    with open(filename, mode="r", encoding="utf8") as file_obj:  
        with mmap.mmap(file_obj.fileno(), length=0, access=mmap.ACCESS_READ) as mmap_obj:  uryi
            pycode = mmap_obj.read()  
            print(pycode)  
            result = subprocess.run(['python', '-c', pycode], stdout=subprocess.PIPE)  
            print(result.stdout.decode())  
  
def sharing_with_mmap():  
    BUF = mmap.mmap(-1,          # create anonymous memory in ram  
                    length=100,  # 100 bytes  
                    access=mmap.ACCESS_WRITE)  
  
    pid = os.fork()  
    if pid == 0:  
        # Child process  
        BUF[0:100] = b"a" * 100  
    else:  
        time.sleep(2)  
        print(BUF[0:100])  
  
mmap_io('client.py')  
```
Résultat
```
/home/user/cnam-utc502-os/venv/bin/python /home/user/cnam-utc502-os/content/labs/memory/jumpToMemory.py
b'\nimport subprocess\nimport requests\n\nprint("hello")\n\nr = requests.get(\'https://pypi.org/project/requests/\')\nprint(r.status_code)\nprint(r.headers[\'content-type\'])\n'

coucou
200
text/html; charset=UTF-8
```
Je récupère donc le message "coucou" ainsi que la requête Web obtenu via la bibliothèque `requests`.


4. Ensuite, créez un fichier de script Python appelé "server.py". Ce fichier doit écouter les connexions entrantes et renvoyer une charge à exécuter. Vous pouvez générer une charge avec un simple script Python qui envoie un message à la console ou vous pouvez utiliser un fichier binaire malveillant à des fins de test.  
> server.py
```python
from flask import Flask, request  
import subprocess  
import os  
  
app = Flask(__name__)  
  
@app.route('/execute', methods=['POST'])  
def execute_payload():  
    try:  
        # Recevoir le fichier depuis le client  
        uploaded_file = request.files['file']  
        if uploaded_file:  
            # Enregistrer le fichier sur le serveur  
            file_path = os.path.join(os.getcwd(), 'uploaded_file.py')  
            uploaded_file.save(file_path)  
  
            # Lire le contenu du fichier  
            with open(file_path, 'r') as file:  
                payload = file.read()  
  
            print(f"Fichier reçu du client :\n{payload}")  
  
            # Exécuter le contenu du fichier  
            result = subprocess.run(['python', '-c', payload], stdout=subprocess.PIPE, stderr=subprocess.PIPE)  
            output = result.stdout.decode('utf-8') if result.stdout else result.stderr.decode('utf-8')  
  
            return f"Résultat de l'exécution :\n{output}"  
        else:  
            return "Aucun fichier reçu.", 400  
    except Exception as e:  
        return f"Erreur lors de l'exécution du fichier : {str(e)}", 500  
  
if __name__ == '__main__':  
    app.run(host='127.0.0.1', port=12345)
```


5. Exécutez "server.py" dans un terminal et "client.py" dans un autre. Le client doit se connecter au serveur, télécharger la charge et l'exécuter dans un processus. Observez la sortie de "server.py" pour voir la connexion entrante et la charge renvoyée.

> client.py
```python
import requests  
  
  
def send_file():  
    url = 'http://127.0.0.1:12345/execute'  
  
    # Charger un fichier Python en tant que payload  
    with open('hello.py', 'rb') as file:  
        files = {'file': file}  
        response = requests.post(url, files=files)  
  
    print("Réponse du serveur:")  
    print(response.text)  
  
if __name__ == "__main__":  
    send_file()
```

```
/home/user/cnam-utc502-os/venv/bin/python /home/user/cnam-utc502-os/content/labs/memory/client.py
Réponse du serveur:
Résultat de l'exécution :
hello
```

```
/home/user/cnam-utc502-os/venv/bin/python /home/user/cnam-utc502-os/content/labs/memory/server.py
 * Serving Flask app 'server'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:12345
Press CTRL+C to quit
Fichier reçu du client :
print('hello')
127.0.0.1 - - [21/Dec/2023 13:02:26] "POST /execute HTTP/1.1" 200 -
```


6. Au fait, la page que nous avons créé pour charger notre payload n’a pas les droits d’exécution. Pourtant la payload arrive bien à s’exécuter. Comment est-ce possible ? Qu’est ce qui s’exécute réellement ?

Lorsque le client télécharge le fichier `hello.py` depuis le serveur et exécute son contenu à l'aide de la fonction `exec` dans le script client, le code Python est interprété et exécuté dans le contexte de l'interpréteur Python du client. Cela se fait indépendamment des permissions du fichier sur le système de fichiers du client.

7. Imaginez la même solution avec un payload binaire : comment l’éxécuter ?

Le client utilise un mécanisme pour exécuter le fichier binaire local via `subprocess` pour lancer un processus.

