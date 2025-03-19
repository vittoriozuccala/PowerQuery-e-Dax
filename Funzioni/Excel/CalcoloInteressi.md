# Calcolo Interessi

// Calcolo Interessi con TAN Assofin comprensivi di Assicurazione
CalcoloInteressiAssofin =LAMBDA(NumeroRate,Montante,TAN,
  (PMT(TAN / 12,NumeroRate, - Montante,0) * NumeroRate - Montante) / 
  (PMT(TAN / 12,NumeroRate, - Montante,0) * NumeroRate) * Montante
);

// Calcolo Interessi Banca ovvero il guadagno vero della Banca
CalcoloInteressiBanca =LAMBDA(NumeroRate,Montante,TanBase,
  (PMT(TanBase / 12,NumeroRate, - Montante,0) * NumeroRate - Montante) / 
  (PMT(TanBase / 12,NumeroRate, - Montante,0) * NumeroRate) * Montante
);