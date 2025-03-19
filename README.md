# PowerQuery-e-Dax
Funzioni, Challenges e Esercizi di PQ e PBI


- **Risorse DAX**
  - *Basi dati OData*
    - *Nothwind*: https://services.odata.org/northwind/northwind.svc/
    - *AdventureWorks*: https://services.odata.org/AdventureWorksV3/AdventureWorks.svc/
- **Risorse PowerQuery**
  - *API per giorni festivi*: [API](https://date.nager.at/api/v3/publicholidays/2025/IT) dove cambiare l'anno e lo stato alla fine
  - [Funzioni](./Funzioni/README.md)




# Libro PowerQuery Beyond the limits
# PowerQuery 

Questo repository contiene diversi esempi e spunti per l'ambiente *PowerQuery*

Inizio delle informazioni

Arrivato a pagina 110: "Example 3: Replacing Multiple Values by Using List.Accumulate"

## Chapter 01: Listes
### Estrarre da una lista di colonne solo le non date
Se ho una lista di colonne tipo:
- Product
- Color
- 31-Jan-22
- 28-Feb-22

```sql
= List.Select(
    Table.ColumnNames(ChangedType),
    each (try Date.From(_))[HasError]
)

-- Il contrario basta mettere un not
= List.Select(
    Table.ColumnNames(ChangedType),
    each not (try Date.From(_))[HasError]
)

-- Per vedere l'errore basta sostituire il Select con Transform
= List.Transform(
    Table.ColumnNames(ChangedType),
    each not (try Date.From(_))
)

```

## Chapter 02: Records
### Skippare una lista in base al tipo di dato
Per non avere un numero hard coded all'interno del codice, invece di usare
```sql
= List.Skip (Record.ToList (_), 2)
```

è possibile dirgli di skippare finchè non trovi un numero:

```sql
= List.Skip (Record.ToList (_), each not ( _ is number)
```

## Modi differenti per OR ed AND
C'è un modo diverso per fare questo OR:
```sql
if [Region] = "South" or [Region] = "West"
       then "South-West" else
           if [Region] = "North" or [Region] = "East"
       then "North-East" else "NA"
```

utilizzanto List.Contains:
```sql
if List.Contains({"South", "West"}, [Region])
          then "South-West" else
            if List.Contains({"North", "East"}, [Region])
          then "North-East" else "NA"
```

Per condizioni algebriche:

```sql
=if 
List.Contains(
    {[Sales] > 14500, [Profit] > 5000}, true
) then [Sales] * 0.1 else 0
```

Analogamente c'è un modo diverso per fare questo AND:
Condition:
- If the Date column value is between January and March and the Region Group column is South-West, return Seasonal.
- Otherwise, return Non Seasonal
La sintassi è la seguente
```sql
=List.ContainsAll (
    {A List to search in},
    {List of values to search for}
)
--  The following example returns true
-- because both of the values are found.
=List.ContainsAll (
    {"Boy", "Cat", "Horse"}, -- List of values to search in
    {"Cat", "Boy"} -- List of values to search for
)
```

utilizzanto List.ContainsAll:
```M
if
    List.ContainsAll(
        {"January","February","March","North-East"},
        {Date.MonthName([Date]), [Region Group]}
    )
then "Seasonal"
else "Non Seasonal"
``` 