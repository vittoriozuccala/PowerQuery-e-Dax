
# fxCreateCalendar

## Descrizione
La funzione crea un calendario tra due date inserite in input.

## Input
- DateStart: la Data iniziale
- DateEnd: la Data finale
- State: è un parametro opzionale: una sigla composta da due lettere che indica lo stato es. IT, DE, UK

## Output
- Una tabella con l'elenco delle date sotto forma di calendario e relative informazioni tra le due date proposte in input

## Esempio

La funzione, se specificato un parametro quale la sigla dello stato, resituisce la tabella sottostanza con l'aggiunta dei giorni festivi.
Per fare questo necessita di una connessione web e sfrutta la seguente [API](https://date.nager.at/api/v3/publicholidays/2025/IT) inserendo tutti gli anni presenti nel range di date sotto espresse e la sigla dello stato interessata.

Dalla funzione inserire DateStart = 1/1/2025 e DateEnd = 8/1/2025
In output restituisce:

| Data | DateNumeric | Year | NumberDayOfYear | Month | NameOfMonth | StartOfMonth | EndOfMonth |NumberOfWeekInMonth | NumberOfWeekInYear	| NumberOfDayInWeek	| NameOfDay	| QuarterOfTheYear	| CompleteQuarterOfTheYear	| Day |
| ---------- | -------- | ---- | - | - | ------- | ---------- | ---------- | -| -- | - | -------- | - | ------ | - |
| 01/01/2025 | 20250101 | 2025 | 1 | 1 | gennaio | 01/01/2025 | 31/01/2025 | 1 | 1 | 2 | mercoledì | 1 | 2025Q1 | 1 | 
| 02/01/2025 | 20250102 | 2025 | 2 | 1 | gennaio | 01/01/2025 | 31/01/2025 | 1 | 1 | 3 | giovedì | 1 | 2025Q1 | 2 | 
| 03/01/2025 | 20250103 | 2025 | 3 | 1 | gennaio | 01/01/2025 | 31/01/2025 | 1 | 1 | 4 | venerdì | 1 | 2025Q1 | 3 | 
| 04/01/2025 | 20250104 | 2025 | 4 | 1 | gennaio | 01/01/2025 | 31/01/2025 | 1 | 1 | 5 | sabato | 1 | 2025Q1 | 4 | 
| 05/01/2025 | 20250105 | 2025 | 5 | 1 | gennaio | 01/01/2025 | 31/01/2025 | 1 | 1 | 6 | domenica | 1 | 2025Q1 | 5 |
| 06/01/2025 | 20250106 | 2025 | 6 | 1 | gennaio | 01/01/2025 | 31/01/2025 | 2 | 2 | 0 | lunedì | 1 | 2025Q1 | 6 | 
| 07/01/2025 | 20250107 | 2025 | 7 | 1 | gennaio | 01/01/2025 | 31/01/2025 | 2 | 2 | 1 | martedì | 1 | 2025Q1 | 7 | 
| 08/01/2025 | 20250108 | 2025 | 8 | 1 | gennaio | 01/01/2025 | 31/01/2025 | 2 | 2 | 2 | mercoledì | 1 | 2025Q1 | 8 | 


## Funzione

```sql
 (DateStart as text, DateEnd as text, optional State as text) =>
// Questa fx permette di creare un calendario completo.
// INPUT: 
//      - DataStart: è la data dalla quale si desidera far iniziare il calendario;
//      - DataEnd: è la data nella quale si vuole far terminare il calendario. 
//	    - State: è un parametro opzionale: è una sigla composta da due lettere che indica lo stato es. IT, DE, UK
// OUTPUT: un calendario completo
//
// NB: inserendo uno spazio nella DataEnd, il sistema prenderà la data odierna.
let
    Source = if DateEnd =" " then Table.FromColumns({{Number.From(Date.From( DateStart )) .. Number.From(Date.From( DateTime.LocalNow()))}},{"Data"}) else Table.FromColumns({{Number.From(Date.From( DateStart )) .. Number.From(Date.From( DateEnd))}},{"Data"}),
    ChangedType = Table.TransformColumnTypes(Source,{{"Data", type date}}),
    NumericDateAdded = Table.AddColumn(ChangedType, "DateNumeric", each Date.Year([Data])*10000+Date.Month([Data])*100+Date.Day([Data]),Int64.Type),
    
    // Year
    NumberOfYear = Table.AddColumn(NumericDateAdded, "Year", each Date.Year([Data]), Int64.Type),
    NumberDayOfYear = Table.AddColumn(NumberOfYear, "NumberDayOfYear", each Date.DayOfYear([Data]), Int64.Type),
    
    // Month
    MonthNumber = Table.AddColumn(NumberDayOfYear, "Month", each Date.Month([Data]), Int64.Type),
    NameOfMonth = Table.AddColumn(MonthNumber, "NameOfMonth", each Date.MonthName([Data]), type text),
    StartOfThisMonth = Table.AddColumn(NameOfMonth, "StartOfMonth", each Date.StartOfMonth([Data]), type date),
    EndOfThisMonth = Table.AddColumn(StartOfThisMonth, "EndOfMonth", each Date.EndOfMonth([Data]), type date),


    // Weeks
    NumberOfWeekInMonth = Table.AddColumn(EndOfThisMonth, "NumberOfWeekInMonth", each Date.WeekOfMonth([Data]), Int64.Type),
    NumberOfWeekInYear = Table.AddColumn(NumberOfWeekInMonth, "NumberOfWeekInYear", each Date.WeekOfYear([Data]), Int64.Type),
    NumberOfDayInWeek = Table.AddColumn(NumberOfWeekInYear, "NumberOfDayInWeek", each Date.DayOfWeek([Data]), Int64.Type),
    NameOfWeekDay = Table.AddColumn(NumberOfDayInWeek, "NameOfDay", each Date.DayOfWeekName([Data]), type text),

    // Quarter
    QuarterOfTheYear = Table.AddColumn(NameOfWeekDay, "QuarterOfTheYear", each Date.QuarterOfYear([Data]), Int64.Type),
    CompleteQuarterOfTheYear = Table.AddColumn(QuarterOfTheYear, "CompleteQuarterOfTheYear", each Text.From([Year])&"Q"&Text.From([QuarterOfTheYear]) , type text),
    
    // Day
    NumberOfDay = Table.AddColumn(CompleteQuarterOfTheYear, "Day", each Date.Day([Data]), Int64.Type),
    
    // Giorni Feriali
    NoWorkingDays= 
    if 
        State <> null 
    then
    let
        ListOfYears = List.Distinct(NumberOfDay[Year]), 
        ListTableNoWorkingDays = List.Accumulate(
            ListOfYears,
            {},
            (listOUT,listElement) => listOUT & Json.Document(Web.Contents("https://date.nager.at/api/v3/publicholidays/"& Text.From(listElement)&"/"&State))
        ),
        TableNoWorkingDays = Table.TransformColumnTypes( 
                    Table.RemoveColumns( 
                        Table.FromRecords( 
                            ListTableNoWorkingDays
                        ),
                        {"fixed", "global", "counties", "launchYear", "types"}
                    ),
                    {{"date", type date}, {"localName", type text}, {"name", type text}, {"countryCode", type text}}
                ),
        TableJoin = Table.Join(
            NumberOfDay,
            "Data",
            TableNoWorkingDays,
            "date",
            JoinKind.LeftOuter
        )
    in TableJoin
    else 
        NumberOfDay

    
in  NoWorkingDays
```