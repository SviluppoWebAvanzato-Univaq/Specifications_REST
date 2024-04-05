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

`GET [BASE]/events?from=2024-02-01T00:00Z&to=2024-03-01T00:00Z&cat=work,personal`

#### parametri di query opzionali

- from={...}&to={...} per la paginazione
- view={...} per selezionare i dettagli (vista) da restituire in output tra un set predefinito
- fields=f1,f2 per definire in dettaglio i campi da restituire per ciascun elemento... da usare solo nel caso in cui si debba essere veramente generici

#### output

 (200):

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

`GET [BASE]/events/current?cat=work,personal`

#### parametri di query opzionali

vedi `GET /events`

#### output

(200): vedi `GET /events`

--------------------------------------------------------------    

### 3. estrazione evento specifico


`GET [BASE]/events/{uid}`

#### output

(200): `{evento}`

--------------------------------------------------------------  

### 4. inserimento di un evento

`POST [BASE]/events`

#### input

`{evento}`

(per ottimizzare, potremmo/dovremmo omettere
il campo attachment dalla struttura restituita)

#### output

(201 - CREATED): URL di accesso all'evento (`[BASE]/events/{uid}`)

--------------------------------------------------------------  

### 5. modifica di un evento

Consideriamo la modifica parziale, quindi se disponibile preferiamo PATCH a PUT.

`PATCH [BASE]/events/{uid}`

#### input

`{evento}`

#### output

(204 - NO CONTENT)

--------------------------------------------------------------    

### 6. cancellazione (annullamento) di un evento

`DELETE [BASE]/events/{uid}`

#### output

(204 - NO CONTENT)   

--------------------------------------------------------------    

### 7. lettura numero eventi in un particolare periodo opzionalmente filtrati per categorie

`GET [BASE]/events/count?from=2024-02-01T00:00Z&to=2024-03-01T00:00Z&cat=work,personal`

#### output

(200): `conteggio`

--------------------------------------------------------------    

### 8. estrazione lista partecipanti a un evento

`GET [BASE]/events/{uid}/participants`


#### output

(200): (soluzione base)

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

`GET [BASE]/events/{uid}/attachment`

#### output

(200): `{binario}` (*Content-Type: application/octet-stream*)

--------------------------------------------------------------    

### 10. inserimento o aggiornamento dell'attachment (binario) di una certo evento

In questo caso l'evento esiste già, e il suo binario è magari semplicemente inizializzato
con un default in fase di inserimento dell'evento stesso. Quindi inserire o modificare
tale binario significa semplicemente *sovrascrivere* l'attributo corrispondente della evento.

`PUT [BASE]/events/{uid}/attachment`

#### input

`{binario}` (*Content-Type: application/octet-stream*)

#### output

(204 - NO CONTENT)

--------------------------------------------------------------    

### 11. aggiunta di un partecipante a un evento

`POST [BASE]/events/{uid}/participants`

#### input

`{partecipante}`

#### output

(201 - CREATED): URL di accesso al lista dei partecipanti (`[BASE]/events/{uid}/participants`), 
visto che non possiamo puntare a uno specifico (non è prevista le GET sul partecipante)

--------------------------------------------------------------    

### 12. rimozione di un partecipante da un evento

Come identificativo di un partecipante possiamo usare l'indirizzo email.

`DELETE [BASE]/events/{uid}/participants/{email}`

#### output

(204 - NO CONTENT)   

--------------------------------------------------------------    

### 13. aggiornamento della regola di ricorrenza di un evento

`PUT [BASE]/events/{uid}/recurrence`

#### input

`{ricorrenza}`

#### output

(204 - NO CONTENT)