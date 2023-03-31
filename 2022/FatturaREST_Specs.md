# Specifica API "Fatture"

## Formato fattura

- data,
- numero,
- dati intestatario *(ragione sociale, indirizzo, partita IVA)*,
- lista elementi *(codice, descrizione, prezzo unitario, quantità, unità di misura, aliquota IVA, prezzo totale)*, 
- totali *(IVA, IVA esclusa, IVA inclusa)*

## Operazioni

1.  estrazione lista fatture
2.  estrazione fatture relative a una certa partita IVA
3.  estrazione fattura specifica
4.  inserimento di una fattura
5.  modifica di una fattura
6.  cancellazione (annullamento) di una fattura
7.  estrazione fatture di un anno specifico
8.  estrazione partita IVA dell'intestatario di una certa fattura
9.  lettura numero fatture in archivio 
10. lettura numero fatture emesse per una certa partita IVA
11. estrazione elementi venduti in una certa fattura
12. estrazione dettaglio riga fattura
13. download PDF (binario) fattura
14. inserimento o aggiornamento del PDF (binario) di una certa fattura
15. estrazione delle fatture in cui viene venduto un certo prodotto