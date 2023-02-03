
## PHP conf

```bash
apt-get update

apt-get install apache2

apt-get install php
```

Créer un phpinfo.php
```bash
touch phpinfo.php
nano phpinfo.php
	<?php
	phpinfo() ;
	?>
```

Executer
```bash
phph phpinfo.php
```


Ajouter un script PHP  
```bash
cd /var/www/html
touch index.php
nano index.php
	<?php
	$droits=$_GET['droits'];
	if ($droits=="admin") { echo "PANNEL ADMIN"; }
	else { echo "PANNEL USER"; }
	?>
	<form action="index.php" method="GET">
	<input type="hidden" name="droits" value="user">
	<input type="submit" value="Envoyer">
	</form>
```

Tester sur navigateur machine hote
```
http://192.168.X.X/index.php?droits=user
```
puis changer le "user" par "admin"

## Nikto

Installer nikto via github
```bash
export http_proxy=http://192.168.2.7:3128
export https_proxy=http://192.168.2.7:3128
git clone https://github.com/sullo/nikto.git
```

Tester
```bash
cd nikto/program
./nikto.pl -h 127.0.0.1 -C all      // pour relever les failles
```


## Script Python (rootme)

Challenges → App – Script → Python – input() 
Webssh

```python
#!/usr/bin/python3
import sys
def youLose():
        print ("Try again ;-)")
        sys.exit(1)
try:
        p = input("Please enter password : ")
except:
        youLose()
with open(".passwd") as f:
        passwd = f.readline().strip()
        try:
                if (p==passwd):
                        print ("Nice! You can validate with this password>
                else:
                        youLose()
        except:
                print("haha")
```
"try" et "except" permettent de gérer les erreurs. Si on entre un mauvais type de variable pour "passwd" le programme ne va pas exécuter le "else" mais générer une erreur. Donc si il y a une erreur on passe au "except".