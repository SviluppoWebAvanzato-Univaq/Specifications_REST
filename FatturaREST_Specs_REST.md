# Specifica API "Fatture" rielaborata

## Tipi

Tutte le API accettano/restituiscono JSON

### Struttura oggetto JSON fattura

```json
{
    "data":"2022-03-25",
    "numero":123,
    "intestatario": {
        "ragioneSociale":"ABC",
        "partitaIVA":123,
        "indirizzo":"ABC"
    },
    "elementi": [
        {
            "codice":"ABC",
            "descrizione":"ABC",
            "prezzoUnitario":1.23,
            "quantita":1.23,
            "unitaDiMisura":"ABC",
            "aliquotaIVA":22
            "prezzoTotale":1.23        
        }
    ],
    "totali": {
        "totaleIVAEsclusa":1.23,
        "totaleIVA":1.23,
        "totaleIVAInclusa":1.23
    }
}
```

Variante: visto che sappiamo di dover associare i dati della fattura anche al binario
della sua scansione, potremmo aggiungere alla struttura una voce del tipo
`"attachment": "[BASE]/fatture/{anno}/`{numero}`/attachment"`
in modo da non inserire il binario nella struttura ma fornire per completezza la URL per leggerla

## URL di base del servizio

 http://swa.net/fatturazione/rest (di seguito indicata con [BASE])

## Operazioni

### 1. estrazione lista fatture

`GET [BASE]/fatture `

#### parametri di query opzionali

- from={...}&to={...} per la paginazione
- view={...} per selezionare i dettagli (vista) da restituire in output tra un set predefinito
- fields=f1,f2 per definire in dettaglio i campi da restituire per ciascun elemento - da usare solo nel caso in cui si debba essere veramente generici

#### output

 (200):

  (soluzione base)

```json
  [
    "[BASE]/fatture/X",
    "[BASE]/fatture/Y",
    "[BASE]/fatture/Z"
  ]
```

  (soluzione mirata - utile per le funzionalità del lato client)

```json
  [
    {
        "data":"---",
        "numero":---,
        "totaleIVAInclusa":---
        "url": "[BASE]/fatture/X"
    }, 
    ....    
  ]
```

  (soluzione variabile - usando parametri di query come view o fields)

--------------------------------------------------------------  

### 2. estrazione fatture relative a una certa partita IVA

`GET [BASE]/fatture?pIVA={numero}`

#### parametri di query opzionali

vedi `GET /fatture`

#### output

(200): vedi `GET /fatture`

--------------------------------------------------------------    

### 3. estrazione fattura specifica

`GET [BASE]/fatture/{anno}/{numero}`

#### output

(200): `{fattura}`

--------------------------------------------------------------  

### 4. inserimento di una fattura

`POST [BASE]/fatture`

#### input:

{fattura}

#### output

(201 - CREATED): URL di accesso alla fattura (`[BASE]/fatture/{anno}/{numero}`)

--------------------------------------------------------------  

### 5. modifica di una fattura

`PUT [BASE]/fatture/{anno}/{numero}`

#### input:

{fattura}

#### output

(204 - NO CONTENT)

--------------------------------------------------------------    

### 6. cancellazione (annullamento) di una fattura

`DELETE [BASE]/fatture/{anno}/{numero}`

#### output

(204 - NO CONTENT)   

--------------------------------------------------------------    

### 7. estrazione fatture di un anno specifico

`GET [BASE]/fatture/{anno}` (oppure `GET [BASE]/fatture?anno={anno}`)

#### parametri di query opzionali

vedi `GET /fatture`

#### output

(200): vedi `GET /fatture`

--------------------------------------------------------------    

### 8. estrazione partita IVA dell'intestatario di una certa fattura

`GET [BASE]/fatture/{anno}/{numero}/pIVA`

#### output

(200): `{numero}`

--------------------------------------------------------------    

### 9. lettura numero fatture in archivio

`GET [BASE]/fatture/count`

#### output

(200): `{numero}`

--------------------------------------------------------------    

### 10. lettura numero fatture emesse per una certa partita IVA

`GET [BASE]/fatture/count?pIVA={numero}`

#### output

(200): `{numero}`

--------------------------------------------------------------    

### 11. estrazione elementi venduti in una certa fattura

`GET [BASE]/fatture/{anno}/{numero}/elementi`

#### parametri di query opzionali

vedi `GET /fatture`

#### output

(200): (soluzione base)

```json
    [
        {
            "codice":"...",
            "descrizione":"...",
            "prezzoUnitario":...,
            "quantita":...,
            "unitaDiMisura":"...",
            "aliquotaIVA":...
            "prezzoTotale":...            
        }
    ]
```

--------------------------------------------------------------    

### 12. estrazione dettaglio riga fattura

`GET [BASE]/fatture/{anno}/{numero}/elementi/{numero_riga}`

#### output

(200):   

```json
    {
        "codice":"...",
        "descrizione":"...",
        "prezzoUnitario":...,
        "quantita":...,
        "unitaDiMisura":"...",
        "aliquotaIVA":...
        "prezzoTotale":...            
    }
```

--------------------------------------------------------------    

### 13. download PDF (binario) di una certa fattura

`GET [BASE]/fatture/{anno}/{numero}/attachment`

#### output

(200): `{binario}`

--------------------------------------------------------------    

### 14. estrazione delle fatture in cui viene venduto un certo prodotto

`GET [BASE]/fatture?codiceProdotto={codice}`

  (oppure, in presenza di un'API più complessa relativa ai prodotti, si potrebbe implementare anche un endpoint del tipo

`GET [BASE]/prodotti/{codice}/fatture`

   eventualmente collegato allo stesso codice di quello principale, per avere API complete sia lato fatture che lato prodotti)

#### parametri di query opzionali

vedi `GET /fatture`

#### output

(200): vedi `GET /fatture`