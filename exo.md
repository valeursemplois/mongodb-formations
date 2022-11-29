# TP DEFINITION DE VALIDATION A LA CREATION DE LA COLLECTION

## 1- Création de base de données

````
  use school
````

## 2 - Définition de la validation et intialisation des données
crée un fichier js et y stocker à l'intérieur le code ci-dessous
````
var validation = {
    "nom":{
      "bsonType":"string",
      "description":"nom"
    },
    "prenom":{
      "bsonType":"string",
      "description":"prenom"
    },
    "adresse.codePostal":{
      "bsonType":"int",
      "description":"code postal"
    },
    "adresse.ville":{
      "bsonType":"int",
      "description":"code postal"
    },
    "adresse.pays":{
      "bsonType":"int",
      "description":"code postal"
    },
     "age":{
      "bsonType":"int",
      "minimum": 15,
      "maximum": 27,
      "description":"code postal"
    },
    "status":{
      "enum":["Inscris","NonInscris" ],
      "description":"Inscription"
    }
  }

etudiants =[
  {
    "nom":"Yves",
    "prenom":"Julien",
    "adresse":{
      "codePostal": 59000
      "ville": "Lille",
      "pays":"France"
    },
    "age":NumberInt(18),
    "status":"Inscris"
  },
  {
    "nom":"Jules",
    "prenom":"Verdes",
    "adresse":{
      "codePostal": 59200
      "ville": "Tourcoing",
      "pays":"France"
    },
    "age":NumberInt(23),
    "status":"Inscris"
  }
]
`````
### 3 chargement du fichier js
````
  tapez la commande suivante pour charger le fichier précédement crée dans le shell
  load("cheminfichier.js");
````
### 4 Execution de la validation lors de la création de la collection

````
 db.createCollection("etudiants", { validator: { $jsonSchema: { "bsonType": "object", "required": ["nom", "prenom", "status", "age"], "properties": validation } } })
````

### 4 Création des données

````
db.etudiants.insertMany(etudiants);
````

### 5 Création d'un document qui ne respecte pas la validation
`````
db.etudiants.insertOne({"nom":"dummyData","prenom":"DummyData",age:12,"status":"Inscris"})

``````
L'enregistrement echouera car l'utilisateur n'a pas l'age requis

### 6 validation d'un champ tableau

Rajouter le code ci-dessous à notre objet validation du précédent fichier js
`````
  "notes":{
    "bsonType":"array",
    "minItems": 1,
    "maxItems": 3,
    "items":{
      "required":["matiere","note"],
      "bsonType":"object",
      "properties":{
        "matiere":{
          "bsonType":"string",
          "description":"nom de la matiere"
        },
        "note":{
          "bsonType":"int",
          "minimum":0,
          "maximum":20,
          "description": "note de l'etudiant" 
        }
      }
    }
  }

`````
Recharger le fichier et modifier la validation à l'aide des commandes ci-dessous
`````
load("cheminfichier.js"); // chargement du fichier
// Modification de la validation

db.runCommand({ "collMod": "etudiants", "validator": { $jsonSchema: { "bsonType": "object", "required": ["nom","prenom","age","status","notes"], "properties": validation } } });

``````