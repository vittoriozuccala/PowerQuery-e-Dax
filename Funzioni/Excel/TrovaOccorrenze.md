# TrovaOccorrenze

Questa funzione serve per filtrare una lista di valori.
All'interno della lista viene ricercata una singola parola o una frase.
La funzione restituisce un array con la lista dei valori trovati nella lista che corrispondono alla ricerca.
Se non ci sono corrispondenze, restituisce "Nessun Risultato"

```c
= TrovaOccorrenze = 
LAMBDA(
    Valore;
    Elenco;
    FILTRO(
        Elenco;
        VAL.NUMERO(
            RICERCA(Valore;Elenco)
        );
        "Non trovato"
     )
)
```