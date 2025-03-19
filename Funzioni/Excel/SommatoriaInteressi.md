# SommatoriaInteressi

// Funzione LAMBDA ricorsiva che permette di calcolare gli interessi da una specifica rata fino alla fine del PAM
// Gli argomenti necessari sono:
// - interessiRataFinale: sono gli interessi calcolati all'ultima rata
// - RataPartenza: se in un finanziamento da 120 si parte dalla 30esima rata, il parametro va impostato a 30
// - TassoTan: deve essere espresso in percentuale
// - DurataTot: la durata della pratica: 60,72,84,120
// - Finanziato: in caso di CQS è il Finanziato Assofin

```
SommatoriaInteressi=LAMBDA(
  interessiRataFinale,RataPartenza,RataUltima,TassoTan,DurataTot,Finanziato,
  IF(
    RataPartenza=RataUltima,
    interessiRataFinale,
    SommatoriaInteressi(
      interessiRataFinale+IPMT(
                          TassoTan/12,
                          RataPartenza,
                          DurataTot,
                          -Finanziato),
      RataPartenza+1,RataUltima,TassoTan,DurataTot,Finanziato) ) );
```