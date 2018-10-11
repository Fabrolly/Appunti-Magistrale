# Modelli della Concorrenza

6 CFU, modulo che fa parte di Modelli e computazione

Bernardiello

## Introduzione al corso

**Argomenti**
* Correttezza dei programmi sequenzali
* Modelli di distemi concorrenti: CSS e bisimulazione
* Modelli di sistemi concorrenti: reti di Petri
* Logiche temporali e model-checking


Esame: prova scritta (esercizi su tutti gli argomenti) e colloquio

Voto finale: media dei due moduli


## Sitema concorrente

* Competizione per l0accesso a risorse condivise
* Cooperazione per un fine comune
* Coordinamento di attività diverse
* Sincronia/asincronia

Molti progetti concorrenti, soprattutto inizialmente, hanno avuto esiti disastrosi: anche sistemi della NASA, apparechi medici ecc.

* Linguaggi
* Modelli (progettare un sistema ancora prima di definirne i dettagli)
* Logica, model checking

## Esempio
```
int  f(int n, const int v[]){
    int x = v[0],
    int h=1
    while (h<n)
    {
        if (x<v[h])
        x=v[h]
        h=h+1
    }
    return x
}
```

La qualità di questo codice è bassa:
* Mancano i commenti
* Le variabili non  hanno nomi significativi

A occhio, si capisce che questa funzione trova il **valore massimo** in un array. 

Si può **dimostrare** che questa funzione è corretto?

### Formalizziamo la situazione ("contratto")

**postcondizione**:

* $\forall i \in [0...n-1] v[i] <= X \land
\exists i \in [0..n-1] V[i]=x$

Ho bisogna anche di una **precondizione**:

* $n>0 \land \forall i \in [0..n-1] v[i]\in Z$

Ok, ma come la dimostro?

**Al passo $n$** posso dire che 

* $\forall i \in [0...h-1] v[i] <= X \land
\exists i \in [0..h-1] V[i]=x$

Ovvero uguale alla postcondizione prima ma ho $h$ al posto di $n$. Questa si chiama **invariante** per questo ciclo: ovvero una formula che se è vera all'inizio di una iterazione è vera anche alla fine. Potrebbe NON essere vera durante l'iterazione ma lo è all'inizio e alla fine dell'iterazione.

Bene, come arrivo alla postcondizione iniziale? **Per induzione**, ma devo trovare il caso base.

(Caso base e passo da capire, comunque verrà approfondita nel corso anche dopo, qui non h ocapito)

## Altra notazione
Proposizione logica, formata da due proposizioni e un programma.

* $\{p\} C \{q\}$

**Si legge:** 
"*Se si esegue il programma **C** a partire da uno **stato** in cui è vera la precondizione **p** ALLORA alla **fine** dell'esecuzione sarà **VERA** la postcozione **q***"
(importante saper leggere questa notazione nel modo corretto!)

Ma cos'è uno **stato?**

## Nozione di **stato** della memoria e **stato** del programma

V= insieme delle variabili che compaiono nel programma (solo valori interi per ora)
### Stato della memoria

Funzione dall'insieme delle **variabili** V all'insieme degli **Interi**.

E' una specie di fotografia ad un certo istante che ci dice il contenuto delle variabili. Istanti in cui il valore della memoria ha uno stato **definito**, non si valutano stati in cui la memoria ha stati indefiniti.

Quindi ora la notazione precedente la si può interpretare correttamente nel suo vero significato.

### Stato del programma
E' dato dallo stato della memoria + il valore del program counter

*ESEMPIO 1*

**Stato 1**
* x=-1, y=3

**Istruzione**
```
x=2*y
```

**Stato 2**
* x=6, y=3

I due stati collegati da una freccia avente come descrizione l'istruzione stessa ci porta da avere un automa.

*ESEMPIO 2*

**Stato 1**
* x=-5, y=1

**Istruzione**
```
if x>0 
  y=2*x 
```

**Stato 2**
* x=-5, y=0

I due stati collegati da una freccia avente come descrizione l'istruzione stessa ci porta da avere un automa.

Sostanzilamente un istruzione, un gruppo odi istruzioni o un programma intero è un qualcosa che mi fa cambiare stato. Posso costruire automi infiniti su un programma.

**Un programma è un trasformatore di stato.**


## Torniamo alla dimostrazione di correttezza di un programma

Abbiamo visto la notazione

 $\{p\} C \{q\}$

 ### Come scriviamo le pre e post condizioni?

 Useremo il **liguaggio proposizionale**. Vogliamo esprimere proprietà dello stato della memoria.

Che cosa ci serve:
 * V **->** Insieme delle variabili, useremo i simboli delle stesse
 * Z **->** valori potenziali delle variabili, numeri
 * = \* < > <= => - + sqrt **->** operatori aritemtici 
 * not, and, or, implica, coimplica **->** operatori logici

Usiamo i quantificatori per esprimere formule e non come predicati logici.

Useremo anche questa notazione ogni tanto per indicare "la formula F è vera nello stato di memoria $\sigma$":
* $\sigma  \models$ f


### Come scriviamo i comandi (notazione, linguaggio)
Questi sono gli unici comandi che useremo, con la segeuente sintassi

```
C::= x:=E                           #Comando di assegnamento. E=espressione
C::= x:=E | C; C                    #Comando composto. C=comando. Non serve il ; alla fine, solo dopo il primo comando

C::= if B then C else C endif       #Condizione di scelta booleana. Il ramo else ci deve sempre essere.

C::= skip                           #istruzione vuota che non cambia lo stato, posso usarlo come ramo dell'else se necessario

C::= while B do C endwhile          #Comando di iterazione

```
### Come scriviamo le condizioni (notazione, linguaggio)

Le condizioni le espirmiamio con questa sintassi
```
B::= true         #true
B::= false        #false
B::= !B           #not
B::= (B & B)      #and
B::= (B | B)      #or
B::= E < E        #minore
B::= E > E        #maggiore
B::= E == E       #uguale
```

Bene ora abbiamo tutte gli ingredienti per poter fare **le nostre belle dimostrazioni** di correttezza.

## Logica di Hoare

La nostra notazione di partenza viene detta anche **tripla**:
 $\{p\} C \{q\}$


*Questa tripla è vera se eseguento in comando p a partire da uno stato della memoria in cui è vera la precondizone p raggiungo uno stato finale della memoria in cui è vera la postcondizione.*

Ovvero la tripla si può leggere: se eseguo p in uno stato della memoria in cui è vera p allora raggiungerò uno stato in cui è vera p.

Le formule della logica di hore sono quindi tutte **proposizioni condizionali** ovvero composte da un antecedente e da un conseguente.

*(questa cosa è tipica domanda da orali)*

### Correttezza della tripla
In questo tipo di logica distinguo due tipi di correttezza:
* 1) Correttezza parziale, do per scontato che il programma termini, ovvero arrivo in uno stato finale q 
* 2) Correttezza totale, è vera se ogni volta che eseguo un comando p a partire da uno stato della memoria in cui è vera la precondizone p raggiungo uno stato finale della memoria in cui è vera la postcondizione.

Quando la tripla è vera indico così:

* $\models (p)$  $\{p\} C \{q\}$   #(p) parziale
* $\models (t)$  $\{p\} C \{q\}$   #(t) totale

### Dimostrabilità della tripla

Indico che una tripla è dimostrabile

$\vdash$  $\{p\} C \{q\}$


La dimostrabilità della tripla **implica** la correttezza della stessa: (quello che dimostro è anche vero, non dimostro se è falso)

$\vdash$ J  **=>**  $\models$ J 

*La dimostrabilità implica la correttezza.*


## Regola di derivazone
```

        T1, T2, ..., Tn    #premesse (triple)
        _______________
                T          #conclusione
```
------
**Regola di derivazione**

Suppongo che il mio comando sia
```
SKIP
```
Quindi per questa istruzine qualunque sia la precondizione **p** questo comando sarà vero. Non ho premsse

**Dimostrazione:**

$\{p\} SKIP \{q\}$ $\forall$ p

---------
**Regola derviazione per implicazione**


Suppongo ora di aver già dimostrato questa tripla

$\{p\} C \{q\}$

**dimostrazione**

```
    p1->p {p}C{q}
    _____________________
        {p1}C{q}
```
-----
**Regola di derivazione della sequenza**

```
     {p}C1{q} {q}C2{r}
    _____________________
        C1,C2
```
Combinando le due premesse ottengo:
```
     {p}C1{q} {q}C2{r}
    _____________________
        {p} C1,C2 {r}
```
-----
**Regola di dervivazione per l'assegnamento**

Posso ragionare direttamente sulla post condizione e istruzione in se. Non ho premesse quindi
```
     x=E
     x=2*y+1 {x>=0}

    {2*y+1>=0} x=2*y+1 {x>=0}
```

In forma generale:

```
     {p}C1{q} {q}C2{r}
    _____________________
        {q[E/x]} x=E {q}
```

*Non avendo premesse questa è sostanzilamente la dimostrazione di partenza di un programma.*


