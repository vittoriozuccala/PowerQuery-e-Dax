# Calcolo Tassi Finanziari

## Calcolo TAN
// Calcolo del Tan Assofin comprensivo della assicurazione all'interno degli interessi
CalcoloTanAssofin = LAMBDA(NumeroRate,ImportoRata,InteressiAssofin,
  RATE(NumeroRate, ImportoRata, -(NumeroRate*ImportoRata - InteressiAssofin)) * 12
);
  
// Calcolo del Tan NON Assofin e quindi che genera gli interessi per Vivibanca
CalcoloTanNoAssofin = LAMBDA(NumeroRate,ImportoRata,InteressiAssofin,CostoVita,CostoImpiego,
  RATE(NumeroRate,ImportoRata,-(NumeroRate*ImportoRata-InteressiAssofin+CostoVita+CostoImpiego))*12
);



## Calcolo TAEG
// Calcolo del TAEG:
CalcoloTAEG = LAMBDA(NumeroRate, ImportoRata, NettoCliente,
  EFFECT(RATE(NumeroRate, ImportoRata, -NettoCliente) * 12, 12)
);


## Calcolo IRR
// IRR EFFETTIVO Gestionale:
CalcoloIRR_OCS =LAMBDA(NumeroRate, ImportoRata, FinanziatoConAss,
  EFFECT(RATE(NumeroRate, ImportoRata, -FinanziatoConAss) * 12, 12)
);

// IRR EFFETTIVO aziendale:
CalcoloIRR_VVB =LAMBDA(NumeroRate,ImportoRata, InteressiAssofin, Istruttoria, CostoImpiego, CostoVita, [Over], [RappelPerc], [ContributiPerc], [Extra], [RetrocessioneIstruttoria],
  EFFECT(
    RATE(
        NumeroRate,
        ImportoRata,
        -NumeroRate * ImportoRata +  
        InteressiAssofin + 
        Istruttoria - 
        CostoImpiego - CostoVita - 
        Over - 
        RappelPerc * NumeroRate * ImportoRata - ContributiPerc * NumeroRate * ImportoRata -
        Extra - 
        RetrocessioneIstruttoria
    ) * 12,
    12
)
);