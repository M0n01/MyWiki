
Hash SHA de toto7
```bash
echo -n 'toto7' | sha256sum
```

Calcul, stockage et vérification du cheksum de "lefichier"
```bash
touch lefichier
nano lefichier  # modifier
sha256sum lefichier > lefichier.sha
sha256sum -c lefichier.sha
# Réussi
```

Re modifier "lefichier" et constater l'échec de la vérification
```bash
nano lefichier # modifier
sha256sum -c lefichier.sha
# Echec
```

Calculer le hash salé de "toto7" avec "7RBNnEK2" comme sel
```bash
mkpasswd -m md5 toto7 7RBNnEK2
```