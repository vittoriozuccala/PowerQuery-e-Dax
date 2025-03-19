# ConfrontaDueTabelle
Questa funzione verifica le differenze tra due tabelle o tra due intervalli
Una volta trovate queste differenze, ne estrapola gli indirizzi e crea un collegamento ipertestuale
Cliccando sul collegamento, vengono evidenziate le celle differenti nella seconda tabella rispetto la prima.


```sql
=LAMBDA(
  tabella1;
  tabella2;
  COLLEG.IPERTESTUALE(
    "#"&TESTO.UNISCI(
      "; ";
      VERO;
      SE(
        tabella1<>tabella2;
        INDIRIZZO(RIF.RIGA(tabella2);RIF.COLONNA(tabella2));
        ""
      )
     )
   )
)
```