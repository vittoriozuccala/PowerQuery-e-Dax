# fxNumbersToDate
## Descrizione
Per creare una data esiste la funzione `#date(yy,mm,aa)`
Questa funzione, tuttavia, aggiunge la possibilità nel caso in cui alcuni dei campi in input siano degli zeri oppure un testo o ancora dei *null*,  inserire una data di default 01/01/1900.
La funzione, inoltre, controlla che i giorni siano da 1 a 31 ed i mesi da 1 a 12.
La funzionalità è particolarmente interessante nel momento in cui le date arrivano da AS400.
Questa piattaforma restituisce spesso date divise in campi differenti.

## Input
- ggVal = un numero che indica il giorno
- mmVal = un numero che indica il mese
- aaVal = un numero che indica l'anno
- errore_o_ data = opzionale. "errore" o "data". Ciò che restituisce in caso di errore

## Esempio
Supponiamo una tabella di ingresso:
```sql
= #table(
    {"Anno","Mese","Giorno"},
    {
        {2025,3,6},
        {null, 3,3},
        {"testo",10,2},
        {2025,14,2},
        {2025,4,5},
        {0,3,1},
        {2025,0,4},
        {2025,4,0},
        {0,0,0}
    }
)
```

Applicando la funzione `=fxNumbersToDate([Giorno],[Mese],[Anno], "errore")` si ottiene

| Anno	    | Mese	    | Giorno    | Data       |
| --------- | --------- | --------- | ---------- |
| 2025	    | 3	        | 6	        | 06/03/2025 | 
| null	    | 3	        | 3	        | errore     |
| testo	    | 10	    | 2	        | errore     | 
| 2025	    | 14	    | 2	        | errore     |
| 2025	    | 4	        | 5	        | 05/04/2025 |
| 0	        | 3	        | 1	        | errore     |
| 2025	    | 0	        | 4	        | errore     |
| 2025	    | 4	        | 0	        | errore     |
| 0	        | 0	        | 0	        | errore     |


## Funzione

```sql
(ggVal as any, mmVal as any, aaVal as any, optional errore_o_data as nullable text ) =>

// Permette di convertire tre numeri interi (anno, mese, giorno) in una unica data.
// Nel caso in cui i numeri non siano valorizzati, viene convertita in 1 gennaio 1900
// INPUT:
//	- aaVal: anno in formato numerico
//	- mmVal: mese in formato numerico
//	- ggVal: giorno in formato numerico
//  - errore_o_data: opzionale. "errore" o "data". Ciò che restituisce in caso di errore
// OUTPUT: una data ggVal/mmVal/aaVal

let
    Parametro = if 
                errore_o_data = "data" or errore_o_data is null
                then Date.FromText("1/1/1900") 
                else "errore",
    DateOut = if 
                ggVal is number and ggVal >=1 and ggVal <=31 and 
                mmVal is number and mmVal >=1 and mmVal<=12 and
                aaVal is number and aaVal >=1900
            then
                Date.FromText( Text.Combine({Number.ToText(ggVal), Number.ToText(mmVal), Number.ToText(aaVal)}, "/") ) 
            else 
                Parametro

in
    DateOut
```