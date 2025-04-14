## TP2 – Exercices MongoDB

### Exercice 1 – Logements et Voyageurs

#### Question 1 – Insérer un document logement

```json
{
  "code": "pi",
  "nom": "U Pinzutu",
  "capacité": 10,
  "type": "Gîte",
  "lieu": "Corse",
  "activités": [
    {
      "codeActivité": "Voile",
      "description": "Pratique du dériveur et du catamaran"
    },
    {
      "codeActivité": "Plongée",
      "description": "Baptêmes et préparation des brevets"
    }
  ]
}
```

---
#### Question 2 – Insérer un document voyageur

```json
{
  "idVoyageur": 10,
  "nom": "Fogg",
  "prénom": "Phileas",
  "ville": "Ajaccio",
  "région": "Corse",
  "séjours": [
    {
      "idSéjour": 1,
      "codeLogement": "pi",
      "début": 20,
      "fin": 20
    },
    {
      "idSéjour": 6,
      "codeLogement": "pi",
      "début": 10,
      "fin": 12
    }
  ]
}
```

---
#### Question 3 – Schéma JSON de validation

```json
{
  "bsonType": "object",
  "required": ["code", "nom", "capacité", "type", "lieu"],
  "properties": {
    "code": {
      "bsonType": "string"
    },
    "nom": {
      "bsonType": "string"
    },
    "capacité": {
      "bsonType": "int"
    },
    "type": {
      "bsonType": "string"
    },
    "lieu": {
      "bsonType": "string"
    },
    "activités": {
      "bsonType": "array",
      "items": {
        "bsonType": "object",
        "required": ["codeActivité", "description"],
        "properties": {
          "codeActivité": {
            "bsonType": "string"
          },
          "description": {
            "bsonType": "string"
          }
        }
      }
    }
  }
}
```

---
#### Question 4 – Créer la collection `logements` avec validation JSON Schema

```js
use tp_voyageurs;

db.createCollection("logements", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["code", "nom", "capacité", "type", "lieu"],
      properties: {
        code: { bsonType: "string" },
        nom: { bsonType: "string" },
        capacité: { bsonType: "int" },
        type: { bsonType: "string" },
        lieu: { bsonType: "string" },
        activités: {
          bsonType: "array",
          items: {
            bsonType: "object",
            required: ["codeActivité", "description"],
            properties: {
              codeActivité: { bsonType: "string" },
              description: { bsonType: "string" }
            }
          }
        }
      }
    }
  }
});
```

---
### Exercice 2 – Étudiants et UEs

#### Question 1 

![[Pasted image 20250408102215.png]]
#### Question 2 – Structure imbriquée

```json
{
  "id": "ue:11",
  "titre": "Java",
  "étudiants": [
    {
      "id": 978,
      "nom": "Jean Dujardin",
      "note": 12
    }
  ]
}
```

```json
{
  "id": "ue:27",
  "titre": "Bases de données",
  "étudiants": [
    {
      "id": 978,
      "nom": "Jean Dujardin",
      "note": 17
    },
    {
      "id": 476,
      "nom": "Vanessa Paradis",
      "note": 10
    }
  ]
}
```
