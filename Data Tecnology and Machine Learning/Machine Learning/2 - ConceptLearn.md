## Concept Learn

Concept Learning = derivare una funzione a valore booleana a partire dagli esempi e dall'input e output 

*esempio* ho un universo di individui, devo trovare gli Elefanti. Do esempi positivi e negativi, o solo una delle due categorie, il risultato è vero o falso.

## Esmepio

Voglio un sistema che mi dica se posso o non posso fare sport in base alle informazioni meteo.

* Anzitutto devo scegliere delle **caratterisitche** (meteo, vento, umidità, temperatura)
* Rappresento poi le **ipotesi** con un insieme di vincoli sugli attributi
 * attributo singolo meteo=sole
 * valore "don't care" meteo=?
 * valore nullo permesso meteo=0

Sky   |  Temp | Humid  | Wind | Water  |Forecast

Sunny   | ?     |    ?    |  Strong  |    ?    |     Same

**dati:**
* Una istanza x
* una funzione target booleana
* un ipotesi H, insieme di vincoli sugli attributi
* un insime di traning D con esempi positivi e negativi <x1, c(x1)>..<xn,c(xn)>

**ottengo:**
un ipotesi h$\in$H tale che $\forall$xh$\in$D (h(x) = c(x))


**Quante istanze, concetti e ipotesi ho, in questo caso?**
* Sky: 				Sunny, Cloudy, Rainy
* AirTemp: 				Warm, Cold
* Humidity: 				Normal, High
* Wind: 				Strong, Weak
* Water: 				Warm, Cold
* Forecast: 				Same, Change
* Distinct instances: 			3\*2\*2\*2\*2\*2 = 96
* Distinct concepts: 			2^96
* Syntactically distinct hypotheses: 	5\*4\*4\*4\*4\*4 = 5120 (come sopra ma devo sommare il valore don't care e il valore "non ammesso")
* Semantically distinct hypotheses: 	1+4\*3\*3\*3\*3\*3 = 973 (il caso di errore lo devo togliere da tutti, lo valuto solo una volta +1)

**Considero due ipotesi**
* h1 = <Sunny, ?, ?, Strong, ?, ?> 
* h2 = <Sunny, ?, ?, ?, ?, ?>

h2 è più larga di h1 e darà molte più istanze positive poichè so solo che c'è il sole, mentre nell'altra so che c'è il vento forte (ergo non posso fare sport con la vela per esempio)

Quindi Hj è più generale o uguale a Hk, se e solo se                        
$\forall x \in X : \left[ \left( h _ { k } ( x ) = 1 \right) \rightarrow \left( h _ { j } ( x ) = 1 \right) \right]$




## Algoritmo Find-S 
Parto dal più specifico e rilasso finche non va bene

(DA SISTEMARE)

 Initialize h to the most specific hypothesis in H

 For each positive training instance x

For each attribute constraint ai in h

    If the constraint ai in h is satisfied by x

        then do nothing

        else replace ai in h by the next more general constraint that is satisfied by x

  Output hypothesis h

## Proprietà algoritmo Find-S

* Hypothesis space described by conjunctions of attributes
* Find-S will output the most specific hypothesis within H that is consistent with the positive training examples
* The output hypothesis will also be consistent with the negative examples, provided the target concept is contained in H


