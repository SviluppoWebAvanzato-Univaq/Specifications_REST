# Specifica API "Catalogo"

## Formato catalogo

- Insieme di *sezioni*,  
- Ogni **sezione** è definita da codice univoco, nome, descrizione *(opzionale)*, lista di *prodotti*.
- Ogni **prodotto** è definito da codice univoco, nome, prezzo *(numero con due decimali)*, flag di disponibilità, descrizione breve, e una serie opzionale di *dettagli*.
- I **dettagli di un prodotto** possono includere: descrizione lunga, immagine *(binario)*, lista ingredienti *(stringhe)*, lista allergeni *(stringhe)*, valori nutrizionali *(kcal, grassi, carboidrati, proteine)*, modalità di conservazione, modalità di preparazione, origine *(es. Italia)*, tag *(lista di stringhe come "bio", "vegano", ...)*.


## Operazioni

1. Lettura lista sezioni ("radice" catalogo).
2. Lettura dettagli di una sezione.
3. Aggiornamento descrizione di una sezione.
4. Lettura prodotti di una sezione.
5. Lettura dei soli prodotti di una sezione attualmente disponibili.
6. Calcolo numero prodotti globale e per sezione.
7. Lettura dettagli di un prodotto.
8. Inserimento di un nuovo prodotto.
9. Modifica (anche parziale) di un prodotto.
10. Eliminazione di un prodotto.
11. Inserimento (o spostamento) di un prodotto in una sezione.
12. Ricerca di prodotti per codice e/o nome e/o tag e/o sezione e/o fascia di prezzo e/o disponibilità.
13. Scaricamento immagine (binaria) di un prodotto.
14. Inserimento/Aggiornamento immagine (binaria) di un prodotto.
