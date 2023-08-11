Les formats de données sont simplement un moyen de stocker et d'échanger des données dans un format structuré. **HTML** (Hypertext Markup Language) par exemple.

Les formats de données ont des règles et une structure similaires à celles que nous avons avec la programmation et les langages écrits. Chaque format de données aura des caractéristiques spécifiques :

- Syntaxe, qui inclut les types de parenthèses utilisés, tels que [ ], ( ), { }, l'utilisation d'espaces blancs ou l'indentation, les guillemets, les virgules, etc.
- Comment les objets sont représentés, tels que les caractères, les chaînes, les listes et les tableaux.
- Comment les paires clé / valeur sont représentées. La clé se trouve généralement sur le côté gauche et identifie ou décrit les données. La valeur à droite correspond aux données elles-mêmes et peut être un caractère, une chaîne, un nombre, une liste ou un autre type de données.

Formats de données courants utilisés dans de nombreuses applications, notamment l'automatisation du réseau et la programmabilité :

**Format JSON**
Notation d'objet JavaScript
```json
{
	"message": "success",
	"timestamp": 1560789260,
	"iss_position": {
		"latitude": "25.9990",
		"longitude": "-132.6992"
	}
}
```

**Format YAML**
YAML n'est pas un langage de balisage = Ain’t Markup Language
```yaml
message: success
timestamp: 1560789260
iss_position:
    latitude: '25.9990'
    longitude: '-132.6992'
```

**Format XML**
Langage de balisage extensible = Extensible Markup Language
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<root>
  <message>success</message>
  <timestamp>1560789260</timestamp>
  <iss_position>
    <latitude>25.9990</latitude>
    <longitude>-132.6992</longitude>
  </iss_position>
</root>
```


## JSON

JSON est un format de données lisible par l'humain utilisé par les applications pour stocker, transférer et lire des données. JSON est un format très populaire utilisé par les services Web et les API pour fournir des données publiques. Ceci est dû au fait qu'il est facile à analyser et peut être utilisé avec la plupart des langages de programmation modernes, y compris Python.

La sortie suivante montre un exemple de sortie partielle d'IOS d'une commande de **show interface GigabitEthernet0/0/0** sur un routeur.

**Sortie du routeur IOS**

```
GigabitEthernet0/0/0 is up, line protocol is up (connected)
  Description: Wide Area Network
  Internet address is 172.16.0.2/24
```

Ces mêmes informations peuvent être représentées au format JSON. Notez que chaque objet (chaque paire clé / valeur) est une donnée différente sur l'interface, y compris son nom, une description et si l'interface est activée.

**Sortie JSON**

```json
{
    "ietf-interfaces:interface": {
         "name": "GigabitEthernet0/0/0",
         "description": "Wide Area Network",
         "enabled": true,
         "ietf-ip:ipv4": {
             "address": [
                 {
                     "ip": "172.16.0.2",
                     "netmask": "255.255.255.0"
                 }
             ]
          }
    }
}
```

#### Règles de syntaxe

Ce sont quelques-unes des caractéristiques de JSON:

- Il utilise une structure hiérarchique et contient des valeurs imbriquées.
- Il utilise des accolades { } pour contenir des objets et des crochets \ [ ] contiennent des tableaux.
- Ses données sont écrites sous forme de paires clé / valeur.

Dans JSON, les données appelées objet sont une ou plusieurs paires clé / valeur entre accolades { }. La syntaxe d'un objet JSON comprend:

- Les clés doivent être des chaînes entre guillemets " ".
- Les valeurs doivent être un type de données JSON valide (chaîne, nombre, tableau, booléen, nul ou autre objet).
- Les clés et les valeurs sont séparées par deux points.
- Plusieurs paires clé / valeur dans un objet sont séparées par des virgules.
- L'espace blanc n'est pas significatif.

Parfois, une clé peut contenir plusieurs valeurs. C'est ce qu'on appelle un tableau. Un tableau en JSON est une liste ordonnée de valeurs. Les caractéristiques des tableaux dans JSON incluent:

- La clé suivie de deux-points et d'une liste de valeurs entre crochets \ [].
- Un tableau est une liste ordonnée de valeurs.
- Le tableau peut contenir plusieurs types de valeurs, y compris une chaîne, un nombre, un booléen, un objet ou un autre tableau à l'intérieur du tableau.
- Chaque valeur du tableau est séparée par une virgule.

## YAML

Certaines des caractéristiques de YAML comprennent:

- C'est comme JSON et est considéré comme un surensemble de JSON.
- Il a un format minimaliste qui facilite la lecture et l'écriture.
- Il utilise l'indentation pour définir sa structure, sans utiliser de crochets ou de virgules.

## XML

Certaines des caractéristiques de XML incluent:

- C'est comme HTML, qui est le langage de balisage normalisé pour la création de pages Web et d'applications Web.
- Il est auto-descriptif. Il enferme les données dans un ensemble de balises: **<tag>data</tag>**
- Contrairement à HTML, XML n'utilise ni balises ni structure de document prédéfinies.

Les objets XML sont une ou plusieurs paires clé / valeur, avec la balise de début utilisée comme nom de la clé: **<key>value</key>**
