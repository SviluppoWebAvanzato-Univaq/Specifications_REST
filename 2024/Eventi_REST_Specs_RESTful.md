# Specifica API "Eventi" rielaborata

## Tipi

Tutte le API accettano/restituiscono JSON se non altrimenti specificato

### Struttura oggetto JSON evento

```json
{
  "uid": "IDabc",
  "summary": "Event IDabc",
  "start": "2024-04-10T13:48:48+02:00",
  "end": "2024-04-10T13:48:48.295207+02:00",
  "categories": ["a","b"],
  "attachment": "Y2lhbyBhIHR1dHRp", 
  "participants": [
    {
      "name": "Pinco Pallino #0",
      "email": "pinco.pallino0@univaq.it"
    },
    {
      "name": "Pinco Pallino #1",
      "email": "pinco.pallino1@univaq.it"
    }
  ],
  "recurrence": {
    "interval": 2,
    "until": "2024-06-10T13:48:00+02:00",
    "frequency": "WEEKLY"
  }
}
```

## URL di base del servizio

`http://swa.net/events/rest` (di seguito indicata con [BASE])

## Operazioni

### 1. estrazione lista eventi filtrati per periodo e opzionalmente per categorie

#### Richiesta

`GET [BASE]/events?from=2024-02-01T00:00Z&to=2024-03-01T00:00Z&cat=work,personal`

##### parametri di query

- from={...}&to={...} per i limiti del periodo con cui filtrare
- cat={...} per le categorie opzionali con cui filtrare

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 (OK)

##### headers
- Content-Type: application/json

##### payload

  (soluzione base)

```json
  [
    "[BASE]/events/X",
    "[BASE]/events/Y",
    "[BASE]/events/Z"
  ]
```

  (soluzione mirata - utile per le funzionalità del lato client)

```json
  [
	{ 
		"uid" : "IDgScqc2lrkr", 
		"summary" : "Event IDgScqc2lrkr", 
		"end" : "2024-03-18T18:08:40.7339204+01:00", 
		"start" : "2024-03-18T17:08:40.7339204+01:00",
		"url" : "http://localhost:8080/EventsREST/rest/events/IDgScqc2lrkr"
	}
    ....    
  ]
```

  (soluzione variabile - usando parametri di query come view o fields)

--------------------------------------------------------------  

### 2. estrazione lista eventi correnti opzionalmente filtrati per categorie

#### Richiesta

`GET [BASE]/events/current?cat=work,personal`

##### parametri di query opzionali

vedi `GET /events`

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 (OK)

##### headers
- Content-Type: application/json

##### payload

vedi `GET /events`

--------------------------------------------------------------    

### 3. estrazione evento specifico

#### Richiesta

`GET [BASE]/events/{uid}`

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 (OK)

##### headers
- Content-Type: application/json

##### payload

Un oggetto evento completo

--------------------------------------------------------------  

### 4. inserimento di un evento

#### Richiesta

`POST [BASE]/events`

##### headers
- Content-Type: application/json

##### payload

Un oggetto evento completo. Per ottimizzare, potremmo/dovremmo omettere il campo attachment dalla struttura.

#### Risposta

##### codice: 201 (CREATED)

##### headers
- Location: URL di accesso all'evento (`[BASE]/events/{uid}`)


--------------------------------------------------------------  

### 5. modifica di un evento

Consideriamo la modifica parziale, quindi se disponibile preferiamo PATCH a PUT.

#### Richiesta

`PATCH [BASE]/events/{uid}`

##### headers
- Content-Type: application/json

##### payload

Un oggetto evento, anche parziale.

#### Risposta

##### codice: 204 (NO CONTENT)

--------------------------------------------------------------    

### 6. cancellazione (annullamento) di un evento

#### Richiesta

`DELETE [BASE]/events/{uid}`

#### Risposta

##### codice: 204 (NO CONTENT)

--------------------------------------------------------------    

### 7. lettura numero eventi in un particolare periodo opzionalmente filtrati per categorie

#### Richiesta

`GET [BASE]/events/count?from=2024-02-01T00:00Z&to=2024-03-01T00:00Z&cat=work,personal`

##### parametri di query

- from={...}&to={...} per i limiti del periodo con cui filtrare
- cat={...} per le categorie opzionali con cui filtrare

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 (OK)

##### headers
- Content-Type: application/json

##### payload

Un numero.

--------------------------------------------------------------    

### 8. estrazione lista partecipanti a un evento

#### Richiesta

`GET [BASE]/events/{uid}/participants`

##### headers
- Accept: application/json

#### Risposta

##### codice: 200 (OK)

##### headers
- Content-Type: application/json

##### payload

(soluzione base)

```json
    [ 
		{ 
			"name" : "Pinco Pallino #0", 
			"email" : "pinco.pallino0@univaq.it" 
		}, 
		{ 
			"name" : "Pinco Pallino #1", 
			"email" : "pinco.pallino1@univaq.it" 
		} 
	]
```

(anche qui possiamo prevedere viste specifiche sui dati, 
come nel caso di `GET /events`)

--------------------------------------------------------------    

### 9. scaricamento attachment (binario) di un certo evento

#### Richiesta

`GET [BASE]/events/{uid}/attachment`

##### headers
- Accept: application/octet-stream

#### Risposta

##### codice: 200 (OK)

##### headers
- Content-Type: application/octet-stream

##### payload

Un binario

--------------------------------------------------------------    

### 10. inserimento o aggiornamento dell'attachment (binario) di una certo evento

In questo caso l'evento esiste già, e il suo binario è magari semplicemente inizializzato
con un default in fase di inserimento dell'evento stesso. Quindi inserire o modificare
tale binario significa semplicemente *sovrascrivere* l'attributo corrispondente della evento.

#### Richiesta

`PUT [BASE]/events/{uid}/attachment`

##### headers
- Content-Type: application/octet-stream

##### payload

Un binario

#### Risposta

##### codice: 204 (NO CONTENT)

--------------------------------------------------------------    

### 11. aggiunta di un partecipante a un evento

#### Richiesta

`POST [BASE]/events/{uid}/participants`

##### headers
- Content-Type: application/json

##### payload

Un sotto-oggetto participant, ad esempio
```json
{
 "name": "Pinco Pallino #0",
 "email": "pinco.pallino0@univaq.it"
}
```

#### Risposta

##### codice: 201 (CREATED)

##### headers
- Location: URL di accesso al lista dei partecipanti (`[BASE]/events/{uid}/participants`), 

...visto che non possiamo puntare a uno specifico (non è prevista le GET sul partecipante)

--------------------------------------------------------------    

### 12. rimozione di un partecipante da un evento

Come identificativo di un partecipante possiamo usare l'indirizzo email.

#### Richiesta

`DELETE [BASE]/events/{uid}/participants/{email}`

#### Risposta

##### codice: 204 (NO CONTENT)

--------------------------------------------------------------    

### 13. aggiornamento della regola di ricorrenza di un evento

#### Richiesta

`PUT [BASE]/events/{uid}/recurrence`

##### headers
- Content-Type: application/json

##### payload

Un sotto-oggetto *recurrence*, ad esempio
```json
{
 "interval": 2,
 "until": "2024-06-10T13:48:00+02:00",
 "frequency": "WEEKLY"
}
```

#### Risposta

##### codice: 204 (NO CONTENT)
