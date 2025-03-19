# Formato Numeri

In questa pagina si inseriscono alcuni formati dei numeri che potrebbero essere utili nelle attività ordinarie.

E' possibile trovare un riferimento esaustivo in questa pagina [https://exceljet.net/articles/custom-number-formats](https://exceljet.net/articles/custom-number-formats)


_(* #.##0_);_(* (#.##0);_(* "-"??_);_(@_)                                 => 4.000,00; (4.000,00); -
[Green]* #.##0_);[Red]* -#.##0;_(* "-"??_);_(@_)                          => verde 4.000.00; rosso -4.000,00; -
"POS" * #.##0_);"NEG" * -#.##0;_(* "-"??_);_(@_)                          => POS 4.000,00; NEG 4.000,00

//Numero con due cifre decimali e rosso quando negativo
// Poi salva il file come xltx in
// C:\Users\mdusr00052\Documents\Modelli di Office personalizzati 
// anche se il video di Wophkins dice qui:
// C:\Users\mdusr00052\AppData\Roaming\Microsoft\Excel\XLSTART
#.##0,##0;[Rosso](#.##0,##0);-