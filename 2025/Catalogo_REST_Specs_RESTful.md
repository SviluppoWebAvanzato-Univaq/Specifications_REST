# Specifica API "Catalogo" rielaborata

## Tipi

Tutte le API accettano/restituiscono JSON se non altrimenti specificato.

### Struttura oggetto JSON catalogo

```json
{
  "nome": "Catalogo Prodotti Alimentari",
  "sezioni": [
    {
      "codice": "frutta",
      "nome": "Frutta",
      "descrizione": "Frutta fresca di stagione",
      "prodotti": [
        {
          "codice": "P-0012",
          "nome": "Mela Golden",
          "prezzo": 1.49,
          "descrizioneBreve": "Mela dolce e croccante",
          "dettagli": {
            "descrizioneLunga": "La Mela Golden è coltivata in Trentino e raccolta a mano.",
            "immagine": "iVBORw0KGgoAAAANSUhEUgAAAAUA...",
            "ingredienti": [
              "Mela"
            ],
            "valoriNutrizionali": {
              "kcal": 52,
              "grassi": 0.2,
              "carboidrati": 14,
              "proteine": 0.3
            },
            "origine": "Italia",
            "etichette": [
              "bio",
              "km0"
            ]
          }
        }
      ]
    },
    {
      "codice": "XYZ789",
      "nome": "Bevande",
      "prodotti": [
        {
          "codice": "P-0100",
          "nome": "Succo d'Arancia",
          "prezzo": 2.99,
          "descrizioneBreve": "100% arancia spremuta",
          "dettagli": {
            "ingredienti": [
              "Succo d'arancia",
              "Acidificante"
            ],
            "origine": "Spagna",
            "modalitaConservazione": "Conservare in luogo fresco e asciutto",
            "modalitaPreparazione": "Pronta al consumo"
          }
        },
        {
          "codice": "P-0200",
          "nome": "Chinotto",
          "prezzo": 3.99,
          "disponibile": false,
          "descrizioneBreve": "Una bevanda antica",
          "dettagli": {
            "ingredienti": [
              "un",
              "sacco",
              "di",
              "roba"
            ]
          }
        }
      ]
    }
  ]
}
```

## **URL di base del servizio**

`http://swa.net/catalog/rest` (di seguito indicata con **[BASE]**)


## Operazioni


### 1. Lettura lista sezioni ("radice" catalogo)

#### Richiesta
`GET [BASE]/sections`

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 OK
```json
[
  {
    "codice": "frutta",
    "nome": "Frutta",
    "url": "[BASE]/sections/frutta"
  }
]
```


### 2. Lettura dettagli di una sezione

#### Richiesta
`GET [BASE]/sections/{code}`

##### parametri
- `code`: codice di una sezione

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 OK
Un oggetto sezione contenente tutti i dettagli della sezione esclusi i prodotti (oppure anche alcune informazioni base dei suoi prodotti,
a seconda delle esigenze).


### 3. Aggiornamento descrizione di una sezione

#### Richiesta
`PUT [BASE]/sections/{code}/description`

##### parametri
- `code`: codice di una sezione

##### payload
Stringa contenente la nuova descrizione

##### headers
- Content-Type: application/json

#### Risposta

##### codice: 204 NO CONTENT


### 4. Lettura prodotti di una sezione

#### Richiesta
`GET [BASE]/sections/{code}/products`

##### parametri
- `code`: codice di una sezione

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 OK
```json
[
 {
  "codice": "P-0200",
  "nome": "Chinotto",
  "prezzo": 3.99,
  "disponibile": false,
  "descrizioneBreve": "Una bevanda antica",
  "url": "[BASE]/products/P-0200"
 },
 ...
]
```
                
### 5. Lettura dei soli prodotti di una sezione attualmente disponibili

#### Richiesta
- `GET [BASE]/sections/{code}/products/available`
- `GET [BASE]/sections/{code}/products?available=true`

##### parametri
- `code`: codice di una sezione

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 OK
Come operazione 4.


### 6. Calcolo numero prodotti globale e per sezione

#### Richiesta
`GET [BASE]/products/count` (globale)
`GET [BASE]/sections/{code}/products/count` (per sezione)

##### parametri
- `code`: codice di una sezione

##### headers
- Accept: application/json (o text/plain)

#### Risposta

##### codice: 200 OK
Un numero.


### 7. Lettura dettagli di un prodotto

#### Richiesta
`GET [BASE]/products/{code}`

##### parametri
- `code`: codice di un prodotto

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 OK
Un oggetto prodotto completo.


### 8. Inserimento di un nuovo prodotto.

#### Richiesta
`POST [BASE]/products`

##### payload
Un oggetto prodotto completo.

##### headers
- Content-Type: application/json

#### Risposta

##### codice: 201 CREATED

###### headers
- Location: `[BASE]/products/{code}`


### 9. Modifica (anche parziale) di un prodotto

#### Richiesta
`PATCH [BASE]/products/{code}`

##### parametri
- `code`: codice di un prodotto

##### payload
Un oggetto prodotto, anche parziale.

##### headers
- Content-Type: application/json

#### Risposta

##### codice: 204 NO CONTENT


### 10. Eliminazione di un prodotto

#### Richiesta
`DELETE [BASE]/products/{code}`

##### parametri
- `code`: codice di un prodotto

#### Risposta

##### codice: 204 NO CONTENT


### 11. Inserimento (o spostamento) di un prodotto in una sezione.

#### Richiesta 1
`PUT [BASE]/products/{code}/section`

##### parametri
- `code`: codice di un prodotto

##### payload
Il codice di una sezione

##### headers
- Content-Type: application/json o text/plain

#### Risposta 1

##### codice: 204 NO CONTENT

#### Richiesta 2
`POST [BASE]/section/{code}/products`

##### parametri
- `code`: codice di una sezione

##### payload
Il codice di un prodotto

##### headers
- Content-Type: application/json o text/plain

#### Risposta 2

##### codice: 201 CREATED

###### headers
- Location: `[BASE]/products/{code}`  
  *Non necessario*


### 12. Ricerca di prodotti per codice e/o nome e/o tag e/o sezione e/o fascia di prezzo e/o disponibilità

Chiaramente può "contenere" l'operazione 5, che a questo punto potrebbe non essere realizzata.

#### Richiesta
`GET [BASE]/products?code=P&name=X&tag=A,B&available=true&section=S&minPrice=U&maxPrice=V&page=N`

##### parametri
- `code`: codice di un prodotto
- `name`: nome o parte di nome
- `tag`: lista di tag separata da virgole
- `available`: false=No, true=Sì
- `section`: codice di una sezione
- `minPrice`: prezzo minimo, intero
- `maxPrice`: prezzo massimo, intero
- `page`: numero di pagina di risultati, intero

*Tutti i parametri sono opzionali*

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 OK
Come operazione 4.


### 13. Scaricamento immagine (binaria) di un prodotto

#### Richiesta
`GET [BASE]/products/{code}/image`

##### parametri
- `code`: codice di un prodotto

##### headers
- Accept: application/octet-stream

#### Risposta

##### codice: 200 OK
Un binario.


### 14. Inserimento/Aggiornamento immagine (binaria) di un prodotto

#### Richiesta
`PUT [BASE]/products/{code}/image`

##### headers
- Content-Type: application/octet-stream

##### payload
Un binario.

#### Risposta

##### codice: 204 (NO CONTENT)