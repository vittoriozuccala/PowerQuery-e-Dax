# fxCleaningTextFields


## Descrizione
Questa funzione riceve una tabella ed una lista di campi testuali che si vogliono pulire.
Successivamente ripulisce i campi dagli spazi multipli siano essi interni o alle estremità ed elimina i caratteri non leggibili.
Posso inserire due ulteriori parametri opzionali per fare una ricerca di caratteri da sostituire con altri.


## Input
- PrecedentTable: una tabella dalla quale prelevare i campi testuali
- ListOfFields: una lista di campi testuali da sistemare
- CharactersToBeChanged: una lista opzionale con tutti i caratteri da sostituire
- CharactersToChange: una lista opzionale che deve avere la stessa lunghezza della precedente con i caratteri con i quali sostituire. Se non specificata, verrà assunto che si vuole sostituire ogni carattere della CharactersToBeChanged in ""

## Output
Una tabella con lo stesso numero di campi iniziale ma con i campi di tipo testo depurati

## Esempio

A fronte della seguente tabella in input:
| Data	    | Indirizzo	                    | Città	            | Importo |
| --------- | ----------------------------- | ----------------- | ------- |
| 3/3/2025	| via     Orietta  # Berti,65#	| Genova	        | 1245,01 |
| 4/4/2025	|     Piazza -- Calamaro,22   --|	Savona	        | 3456,22 |
| 6/6/2025	|   C.so    Umberto, #  2	 	|                   | 657     |
| 5/4/2025	| via O. R. 5	                | Collegno      	| 4555,23 |
| 3/5/2025	| no address	                | Villanova d'Asti 	| 2134,33 |

Applicando la funzione `= fxCleaningTextFieldsCopia(Source,{"Indirizzo","Città"},{"#","--"},{"","-"})` si ottiene:

| Data	    | Indirizzo	                    | Città	            | Importo |
| --------- | ----------------------------- | ----------------- | ------- |
| 3/3/2025	| via Orietta  Berti,65	        | Genova	        | 1245,01 |
| 4/4/2025	| Piazza - Calamaro,22 -	    | Savona	        | 3456,22 |
| 6/6/2025	| C.so Umberto,  2		        |                   | 657     |
| 5/4/2025	| via O. R. 5	                | Collegno	        |4555,23  |
| 3/5/2025	| no address	                | Villanova d'Asti  | 2134,33 |


## Funzione

```sql
/* 
Questa funzione riceve una tabella ed una lista di campi testuali che si vogliono pulire.
Successivamente ripulisce i campi dagli spazi multipli siano essi interni o alle estremità ed elimina i caratteri non leggibili.
Posso inserire due ulteriori parametri opzionali per fare una ricerca di caratteri da sostituire con altri.

INPUT:
- PrecedentTable: una tabella dalla quale prelevare i campi testuali
- ListOfFields: una lista di campi testuali da sistemare
- CharactersToBeChanged: una lista opzionale con tutti i caratteri da sostituire
- CharactersToChange: una lista opzionale che deve avere la stessa lunghezza della precedente con i caratteri con i quali sostituire. Se non specificata, verrà assunto che si vuole sostituire ogni carattere della CharactersToBeChanged in ""

OUTPUT:
Una tabella con lo stesso numero di campi iniziale ma con i campi di tipo testo depurati
*/

(PrecedentTable as table, ListOfFields as list, optional CharactersToBeChanged as list, optional CharactersToChange as list) =>
let
    Source = PrecedentTable,
    
    // Creo una lista nella quale inserisco {"NomeDelCampo", FunzioneDaApplicare}
    CleanTheSpaces = List.Combine(
                { List.Transform(
                        ListOfFields,
                        each { 
                            _,
                            each let   
                                //Split the text at each space character
                                SplitText = Text.Split(_," "),
                                //Remove the blank items from the list
                                ListNonBlankValues = List.Select(SplitText,each _<> ""),
                                //Join the list with a space character between each item
                                TextJoinList = Text.Combine(ListNonBlankValues," "),
                                CleanTheText = Text.Clean(Text.Trim(TextJoinList))
                            in CleanTheText,
                            type text
                        }
                        )
                    }
                ),
    
    /* Applico ad ogni elemento la lista di funzioni precedentemente create */
    RemoveTheSpaces = Table.TransformColumns(Source,CleanTheSpaces),

    /* Se CharactersToChange non è nulla, modifico la tabella sostituendo i caratteri non graditi o con una stringa vuota,
    oppure se è valorizzata anche CharactersToChange li sostituisce con i caratteri presenti in quest'ultima */
    RemoveCharacters = if 
                        CharactersToBeChanged = null 
                        then RemoveTheSpaces
                        else if CharactersToChange = null 
                            then List.Accumulate(
                                CharactersToBeChanged,
                                RemoveTheSpaces,
                                (tableOut, listElement) => Table.ReplaceValue(tableOut,listElement,"",Replacer.ReplaceText,ListOfFields)
                            )
                            else List.Accumulate(
                                CharactersToBeChanged,
                                [
                                    tabellaINPUT = RemoveTheSpaces,
                                    placeHolder = 0
                                ],
                                (recordOut, listElement) => [
                                    tabellaINPUT = Table.ReplaceValue(recordOut[tabellaINPUT],listElement,CharactersToChange{recordOut[placeHolder]},Replacer.ReplaceText,ListOfFields),
                                    placeHolder = recordOut[placeHolder]+1
                                ]
                            )[tabellaINPUT]
  
in
    RemoveCharacters
```