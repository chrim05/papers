*This paper is licensed by the license file in the following path: `/LICENSE`*

------

Durante lo sviluppo di Software, prima o poi, incontriamo la necessità di ottimizzare il nostro codice con il fine di velocizzarne il tempo di esecuzione e diminuirne il consumo di memoria.

In questo documento si parla solamente della prima: diminuire il tempo di esecuzione.

Ci sono 2 classi di ottimizzazioni che possiamo applicare per raggiungere il nostro obiettivo:
* Micro Optimizations
* Macro Optimizations

La classificazione in una delle due categorie non è fatta sulla base dell'impatto della suddetta Ottimizzazione, ma sulla base della sua **Forma**.

L'Architettura di un programma consiste nella massima astrazione, che descrive i macro Step necessari per processare l'Input e trasformarlo nell'Output atteso. Quindi **Architettura** = Catena di Steps, dove **Step** = Singolo Componente/Funzione

Sulla base di questo, possiamo definire le Macro Optimizations come una classe di semplificazioni dell'Architettura, e le Micro Optimizations come una classe di dettagli d'implementazione di questi Steps.

Alcuni esempi di Micro Optimizations:
* Ridurre il numero di `malloc`
* Scegliere se usare stringhe null-terminated + `strlen` o stringhe length-specified
* Usare un approccio Data Oriented (DOD)
* Quasi tutti i tipi di ottimizzazioni performate da un compilatore (Call Inlining, Loop Unrolling, Constant Folding, ...)

Alcuni esempi di Macro Optimizations:
* Eliminazione degli step ridondanti
* Rimozione delle Indirezioni: le Indirezioni nell'architettura di un programma portano a diversi problemi, tra cui:
    * Impossibilità di far applicare al compilatore delle Micro Optimizations (ad esempio: Inheritance)
    * Necessità di dati intermedi che contengano gli stati necessari per "coordinare" i lati dell'Indirezione, ad esempio:
        * Memorizzare messaggi di Logging in un Buffer temporaneo, da cui un Loop attingerà per stampare i suddetti messaggi (il Buffer è il dato intermedio, i componenti che scrivono nel Buffer sono il lato A e il Loop che legge dal Buffer è il lato B)
        * Memorizzare i Token in una Lista durante la fase di Lexing in un compilatore (la Lista è il dato intermedio, il Lexer è il lato A e il Parser è il lato B)

----

Le Macro, ovviamente, influiscono sulla Big O Notation di un programma, le Micro no. Tuttavia sono entrambe importantissime, proprio perchè la classificazione non è fatta sulla base dell'impatto sulla velocità di esecuzione, ma sulla sua forma.

----

Quale delle 2 comporta problemi se fatta prematuramente?

E' un discorso piuttosto complesso ma possiamo semplificarlo in questo modo:
Le Macro girano attorno alla modifica dell'architettura del programma, è quindi probabile che il programma debba essere interamente ristrutturato e la maggior parte dei componenti, di conseguenza, riscritti. Le Micro, invece, girano attorno alla modifica di dettagli implementativi di suddetti Step, che possono senza ombra di dubbio aggiunge Attrito durante la manutenzione del Software (ad esempio: DOD).

Personalmente trovo, perciò, poco utile applicare entrambe prematuramente: le Micro sono assolutamente inutili senza le Macro (sarebbe come comparare un `O(n) + Micro` con un `O(log n) + No Micro`, ovviamente vince il secondo), ma le Macro non sono molto adatte a Software troppo giovani (soggetti a continui radicali cambiamenti).

Inoltre è importante considerare che per poter applicare Macro Optimizations in modo efficace, lo sviluppatore ha assolutamente bisogno di conoscere molto bene la struttura di quel genere di programmi.

----

Quali delle 2 convengono di più in generale?

Non è difficile immaginare, spesso, l'impatto e la complessità di un'ottimizzazione, di qualunque sia la classe di quest'ultima.
Si potrebbe pensare che le Macro aggiungano complessità, ma è quasi sempre il contrario, siccome queste semplificano l'Architettura del programma, che inoltre comporta spesso un'impatto significativo sulla velocità di esecuzione.

Al contrario, le Micro spesso aggiungono parecchia complessità e non è assolutamente detto che l'impatto sia sufficiente per giustificare la complessità aggiunta, quindi è possibile che non valga la pena implementarle.