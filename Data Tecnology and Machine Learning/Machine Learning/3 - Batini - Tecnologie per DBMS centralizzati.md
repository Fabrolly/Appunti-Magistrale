# Tecnologie per DBMS centralizzati

DBMS->CPU->STORAGE

## Indipendenza logica e fisica

**Schema fisico**: sono rappresentati gli aspetti relativi all'effettiva memorizzazione dei dati sulla memoria secondaria. I programmi NON vedono lo schema fisico e non ci interagiscono. Consente l’utilizzo di basi di dati su piattaforme diverse, o la distribuzione di una base di dati su più macchine.

**Schema logico**: l'SQL Data Description Language. Quello che usano i programmi per vedere i dati.

Viene quindi garantita l'**indipendenza fisica**, ovvero se viene modificato qualcosa a livello fisico, i programmi **non decono essere modificati**. L'unico aspetto da modificare è il mapping tra lo schema logico e quello fisico.

DB->Schema Fisico->Schema Logico->Schemi esterni

**schemi esterni**: descrive una porzione della base di dati attraverso il modello logico,riflettendo il punto di vista di un utente. Generalmente è realizzato per mezzo di viste,relazioni derivate da quelle che costituiscono lo schema logico.

Questo consiste una **indipendenza logica** ovvero se cambia qualche aspetto dello schema logico non devo modificare i programmi poichè se non cambia lo schema esterno non è necessario. Dovrò solo cambiare il mapping tra schema esterno e schema logico.

Quindi indipendenza sia fisica che logica.


Viene chiamata **architettura ANSI SPARK** 

## Architettura funzionale
Argomento di tutta la lezione di oggi.

*Facciamo un sempio:*

Voglio fare una transazione, in questo caso un bonifico.

Cliente->Conto Corrente->Operazioni di CC

**Transazione**: Bonifico, -100 euro da CC A, +100 euro CC B

Scriviamo la transazione in linguaggio programmaione

**Transazione**:
```
begin bonifico
leggi saldo(A)

se saldo(A)>100 euri
    Sottrai 100euri da CC A
    Aggiungi 100 euri a CC B
    Committ
Altrimenti
    abort
end transaction #terminazione del programma
```

----

**Commit**: esprime un impegno a concludere l'esecuzione del programma. Ovvero devo trasferire da memoria principale a memoria secondaria i dati. E' un "impegno" poichè devo ancora salvarli sulla memoria secondaria, finche faccio sul programma NON sto agendo sul DB (memoria secondaria) ma sulla memoria primaria.

**abort**: E' diverso da commit, serve a dire che la transazione deve essere interrotta. Ci sono due tipi di abort:
* **Abort da applicazione** (quello dell'esempio, inserito dal programmatore volutamente)
* **Abort da sistema operativo** (se si verificano certe condizioni, il prg deve essere interrotto. Tipo una divisione per 0, deve essere impedita.) Deve anche essere ripristinata la memoria **secondaria** allo stato **antecedente alla transazione**.


## Proprietà ACID del DBMS

**Atomicità**: Ogni transazioe deve essere eseguita **o tutta o per niente**, **NON** può essere eseguita solo in parte. Con una committ TUTTI i dati devono essere devono essere passati alla memoria secondaria. L'atomicità deve essere garantita. Sempre. Un guasto o un errore prima del commit causa un UNDO (disfare) delle operazioni gia’
eseguite. Un guasto o un errore dopo il commit puo’ richiedere il REDO (rieseguire) delle operazioni eseguite, se il loro effetto sul DB non e’ garantito a causa del guasto


**Isolamento**: La proprieta’ di isolamento di una transazione garantisce che essa venga eseguita come se non ci fosse concorrenza, ovvero come se fosse unica.

**Consistenza**: Al termine dell'esecuzione di una transazione, i vincoli di integrità debbono essere soddisfatti "Durante" l'esecuzione ci possono essere violazioni, ma se restano alla fine allora il sistema deve eseguire un rollback come per il caso
dell’atomicita’.

**Durabilità**: La conclusione positiva di una transazione corrisponde ad un impegno (in inglese commit) a mantenere traccia del
risultato in modo definitivo, anche in presenza di guasti.
Come vedremo, questo ha delle implicazioni sul sistema di
gestione dell’ affidabilita’ (recovery) del DBMS


* Gestore delle transazioni ->Consistenza
* Festore della concorrenza -> Isolamento
* Gestore della affidabilità -> Atomicita’ e Durability