# Specifica API "Eventi"

## Formato evento

- identificativo unico,
- data/ora inizio e fine *(data e ora, formato ISO)*,
- descrizione,
- luogo,
- lista categorie *(stringhe/tag)*,
- lista partecipanti *(nome, email)*,
- regola di ricorrenza *(numero ricorrenze, data ultima di ricorrenza, intervallo di ricorrenza, frequenza)*,
- allegato *(binario generico)*

## Operazioni

1.  estrazione lista eventi filtrati per periodo e opzionalmente per categorie
2.  estrazione lista eventi correnti opzionalmente filtrati per categorie
3.  estrazione evento specifico
4.  inserimento di un nuovo evento
5.  modifica di un evento
6.  cancellazione (annullamento) di un evento
7.  lettura numero eventi in un particolare periodo opzionalmente filtrati per categorie
8. estrazione lista partecipanti a un evento
9. scaricamento attachment (binario) di un certo evento
10. inserimento o aggiornamento dell'attachment (binario) di una certo evento
11. aggiunta di un partecipante a un evento
12. rimozione di un partecipante da un evento
13. aggiornamento della regola di ricorrenza di un evento
