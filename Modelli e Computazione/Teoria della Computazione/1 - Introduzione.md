# Teoria della computazione

## Cose importanti

compitino prima di natale, sia teoria che pratica (sopartutto pratica credo di aver cpaito)

## Teoria della computazione

* Capire cosa si riesce a computare in modo automatico con una macchina di calcolo.
* Quali verità logiche è possibile dimostrare

Turing e la macchina astratta per dimostrare le verità logiche dimostrabili.

## Complessità computazionale

**obiettivo principale** classificare i problemi in base alla difficoltà di soluzione mediante macchine di calcolo

La difficolta è basata sull'uso di risorse di tempo/spazio, basato sulla macchina di Touring.

### Esempi di problemi NP difficili

Vedremo la definizione vera di NP difficile dopo.
* Cavalieri tavola rotonda, non puoi mettere vicini i cavalieri che si odiano (a coppie)
* Mappa stati uniti da colorare con 4 colori diversi mai confinanti. Possiamo usarne meno? Quanto è il minimo?
  
* P -> Eddicently Computable Solutions
* NP-> Eddicently Verifiable Solution (non è la classe più difficile in assoluto)

**esempio di problema P**: cammino minimo mappe

* NPC -> i problemi NP-completi sono i più difficili problemi nella classe NP ("problemi non deterministici in tempo polinomiale") nel senso che, se si trovasse un algoritmo in grado di risolvere "velocemente" (nel senso di utilizzare tempo polinomiale) un qualsiasi problema NP-completo, allora si potrebbe usarlo per risolvere "velocemente" ogni problema in NP.

Verificare è facile, risolvere no.

Il problema delle classi P e NP si risolve quindi nella seguente domanda:

### P è uguale a NP?
 Supponiamo di voler calcolare tutti i divisori (divisione intera, ovvero con resto pari a zero) di un numero n. Il problema, quindi, è trovare tutti i numeri x tali che x è un divisore di n.

È abbastanza facile verificare che un certo numero x0 è divisore di n; è sufficiente svolgere l'operazione di divisione e controllare il resto: se è pari a zero, il numero è un divisore, altrimenti non lo è. Il numero di passaggi richiesti per eseguire l'operazione di divisione è tanto maggiore quanto maggiore è il numero n, tuttavia essa risulta sempre abbastanza veloce perché il tempo da essa richiesto sia considerato accettabile.

Al contrario, potrebbe non essere altrettanto facile determinare l'insieme di tutti i divisori. Infatti, quasi tutti i metodi[1] finora ideati nel corso dei secoli richiedono un tempo che aumenta rapidamente al crescere del valore di n, troppo perché esso sia considerato accettabile.

Un ruolo importante in questa discussione è giocato dall'insieme dei problemi NP-completi, che possono essere intuitivamente descritti come quei problemi che sicuramente non apparterrebbero a P se P fosse diverso da NP. Al contrario, dimostrando anche per un solo problema NP-completo la sua appartenenza a P, si otterrebbe una dimostrazione che P e NP sono equivalenti. In un certo senso, i problemi NP-completi sono quelli che meno probabilmente appartengono anche a P.

