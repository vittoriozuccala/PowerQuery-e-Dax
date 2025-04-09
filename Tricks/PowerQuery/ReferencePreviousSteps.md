# Reference Previous Steps

Questo trick è stato recuperato nella pagina di [the biccountant](https://www.thebiccountant.com/2023/02/07/reference-intermediate-step-from-a-different-query-in-power-query/)

E' possibile, in una query, riferirsi ad un passaggio qualsiasi di un'altra query utilizzando i metadati.
Questo permette di non riferirsi obbligatoriamente all'ultimo passaggio ma anche ad uno intermedio.

## Query Started
Iniziamo con la query Started dove definiamo la seguente tabella:

| Name	  | Age | Wage |
| ------- | --- | ---- |
| Betty	  | 3,3	| 120  |
| Carl	  | 4,5	| 140  |
| Anthony |	2,5	| 107  |

Su questa tabella effettuo diversi passaggi ma sull'ultimo inserisco la keyword meta per riferirmi a passaggi precedenti.
La keyword meta può essere solo nell'ultimo passaggio

```sql
let
    Source = #table(
          {"Name", "Age", "Wage"}, 
          {
           {"Betty", 3.3, 120}, 
           {"Carl", 4.5,140},
           {"Anthony", 2.5, 107}
         }),
    MultipyAgex10 = Table.TransformColumns(Source, {{"Age", each _ * 10, type number}}),
    MultipyWagex1000 = Table.TransformColumns(MultipyAgex10, {{"Wage", each _ * 1000, type number}}),
    RemovedColumns = Table.RemoveColumns(
                        MultipyWagex1000,
                        {"Age", "Wage"}
                        ) meta [ReferenceAge = MultipyAgex10, ReferenceWage = MultipyWagex1000]
in
    RemovedColumns
```



## Query ExternalReference
A questo punto, nella seconda query, posso riferirmi direttamente alla Started oppure, come vediamo di seguito, ad un passaggio intermedio in questo modo

```sql
let
    Source = Value.Metadata(Started)[ReferenceAge]
in
    Source
```