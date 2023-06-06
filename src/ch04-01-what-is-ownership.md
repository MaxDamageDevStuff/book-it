## Cos'è l'Ownership?

L'*Ownership* è un insieme di regole che governa come un programma Rust gestisce la memoria.
Tutti i programmi devono gestire l'utilizzo della memoria del computer mentre vengono eseguiti. Alcuni linguaggi hanno un garbage collector che controlla regolarmente se c'è memoria non più utilizzata mentre il programma è in esecuzione; In altri linguaggi invece, il programmatore deve allocare e liberare la memoria in modo esplicito. Rust usa un terzo approccio: la memoria viene gestita tramite un sistema di ownership con uno specifico set di regole che il compilatore controlla. Se una qualunque di queste regole viene violata, il programma non verrà compilato. Nessuna delle features di ownership rallenteranno il programma mentre è in esecuzione.

Dato che l'ownership è un concetto nuovo per molti programmatori, questa richiede un po' di tempo per farci la mano. La buona notizia è che più fai esperienza con Rust e con le regole del sistema di ownership, più troverai naturale sviluppare codice che sia sicuro ed efficiente. Teini duro!

Una volta capita l'ownership, avrai dei solidi fondamenti per capire le features che rendono Rust unico. In questo capitolo, imparerai come funziona l'ownership lavorando su alcuni esempi che si concentrano su una struttura dati molto comune: le stringhe.

> ### La Pila e la Coda
> 
> Molti linguaggi di programmazione non ti richiedono di pensare alla pila (stack) e alla coda (heap) molto spesso. Ma in linguaggi rivolti alla programmazione di sistemi come Rust, sapere se un valore si trova nelloa coda o nella pila influisce su come il linguaggio si comporta e perché devi prendere certe decisioni. Parti dell'ownership saranno descritte in relazione alla pila e alla coda in questo capitolo, quindi eccoti una breve spiegazione per prepararti.
> 
> Sia pila che coda sono parti della memoria disponibili per essere utilizzate a runtime dal tuo codice, ma sono strutturate in modi differenti. La pila memorizza valori nell'ordine in cui li riceve, e li rimuove nell'ordine opposto. Ci si riferisce a questo concetto con *last in, first out* (l'ultimo ad entrare è il primo ad uscire). Pensa ad una pila di piatti: quando aggiungi dei piatti, li metti in cima alla pila, e quando hai bisogno di un piatto, ne prendi uno dalla cima. Aggiungere o rimuovere piatti dal centro o dal basso non funzionerebbe così bene! Aggiungere dati alla pila si dice *pushing*, e rimuovere dati dalla pila si dice *popping*. Tutti i dati memorizzati nella pila devono avere una dimensione fissa e conosciuta. Dati con dimensione sconosciuta durante la compilazione o con dimensione variabile devono essere invece memorizzati nella coda.
> 
> La coda è meno ordinata: quando inserisci dati nella coda, richiedi una certa quantità di spazio. L'allocatore di memoria trova uno spazio vuoto nella coda che sia grande abbastanza, lo contrassegna come in uso, e ritorna un *puntatore*, ovvero l'indirizzo della locazione di memoria. Questo processo è definito come *allocazione nella coda* ed alcune volte viene abbreviato semplicemente con *allocare* (pushare valori nella pila non viene considerato come allocazione). Dato che un puntatore alla coda è di una dimensione fissa e conosciuta, puoi memorizzare il puntatore nella coda, ma quando vorrai accedere ai dati reali, dovrai seguire il puntatore. Pensa di essere seduto ad un ristorante. Quando entri, riferisci il numero di persone presenti nel gruppo, ed il ristoratore trova un tavolo libero che può ospitare tutti e ti ci porta. Se qualcuno nel gruppo arriva tardi, può chiedere dove sei seduto per raggiungerti.
> 
> Pushare sulla pila è più veloce di allocare nella coda perché l'allocatore non dovrà mai cercare un posto dove memorizzare i nuovi dati; la locazione è sempre in cima alla pila. Al contrario, allocare spazio nella coda richiede più lavoro perché l'allocatore deve prima trovare uno spazio abbastanza grande per mantenere in memoria il dato e poi deve effettuare il bookkeeping per preparare la prossima allocazione.
> 
> Accedere ai dati dalla coda è più lento che accedere ai dati nella pila perché devi seguire un puntatore per arrivarci. Inoltre i processori sono più veloci se fanno meno salti in memoria. Continuando l'analogia, considera cameriere al ristorante che prende gli ordinu da molti tavoli. Il metodo più efficiente di prendere gli ordini è quello di prendere tutti gli ordini ad un tavolo prima di andare a quello successivo. Prendere un ordine dal tavolo A, poi prendere un ordine dal tavolo B, poi un altro dal tavolo A , ed un altro ancora dal tavolo B sarebbe un processo molto più lento. Per lo stesso motivo, un processore può fare il suo lavoro in maniera migliore se lavora con dati che sono vicino ad altri dati (come lo sono nella pila) piuttosto che con quelli che sono più lontani (come potrebbe accadere nella coda).
> 
> Quando il tuo codice richiama una funzione, i valori passati alla funzione (inclusi i potenziali puntatori a dati nella coda) e le variabili locali della funzione vengono "pushati" nella pila. Quando la funzione termina il suo lavoro, questi valori vengono "poppati" via dalla pila.
> 
> Mantener traccia di quali parte del codice usano quali dati nella coda,
> minimizza il numero di dati duplicati nella stessa, e ripulir via dati dalla coda dati non più utilizzati dalla pila in modo tale da non rimanere senza più spazio nella stessa sono tutti problemi affrontati dall'ownership. Una volta che avrai compreso l'ownership, non avrai bisogno di pensare alla pila ed alla coda molto spesso, ma sapere che lo scopo principale dell'ownership è la gestione dei dati della pila può aiutare a capire perché funziona in questo modo.

### Regole di Ownership

Prima di tutto, diamo un'occhiata alle regole di ownership. Tieni a mente queste regole mentre esploriamo gli esempi che le illustrano:

* Ogni valore in Rust ha un *owner*.
* Può esserci un solo owner alla volta.
* Quando l'owner esce dallo scope, il valore verrà eliminato.

### Scope delle Variabili

Ora che abbiamo appreso la sintassi di base di Rust, non includeremo `fn main() {`
negli esempi, quindi se stai scrivendo gli esempi insieme a noi, assicurati di mettere questi esempi nella funzione `main` manualmente. Questo è stato fatto per rendere i nostri esempi un po' più concisi, permettendoci di concentrarci sui reali dettagli degli stessi piuttosto che sul codice boilerplate.

Come primo esempio di ownership, daremo uno sguardo allo *scope* di alcune variabili. Uno scope è la porzione di programma nella quale un "oggetto" è valido. Guarda la seguente variabile:

```rust
let s = "hello";
```

La variabile `s` si riferisce ad una string literal, dove il valore della stringa è codificata all'interno del testo del nostro programma. La variabile è valida dal momento nel quale viene dichiarata fino alla fine dello *scope* attuale. Listing 4-1 mostra un programma con commenti che annotano dove la variabile `s` sarà valida.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-01/src/main.rs:here}}
```

<span class="caption">Listing 4-1: Una variabile e lo scope in cui è valida</span>

In altre parole, ci sono due punti nel tempo degni di nota qui:

* Quando `s` entra *in* scope, è valida.
* resta valida finché non diventa *out of* scope.

A questo punto, la relazione tra scope e la validità delle variabili è simile a quella di altri linguaggi di programmazione. Ora costruiremo su questo concetto introducendo il tipo `String`.

### Il Tipo `String`

Per illustrare le regole dell'ownership, abbiamo bisogno di un tipo di dato che sia più complesso di quelli che abbiamo descritto nella sezione [“Tipi Dati”][data-types]<!-- ignore --> del Capitolo 3. I tipi descritti in precedenza hanno una dimensione definita, possono essere memorizzati nella pila ed essere "poppati fuori" dalla pila quando il loro scope finisce, e possono essere copiati in maniera veloce e banale per creare un istanza nuova ed indipendente se un'altra parte di codice necessita di utilizzare lo stesso valore in uno scope diverso. Ma vogliamo osservare i dati che sono memorizzati nella coda ed esplorare come Rust sa quando ripulire quei dati, ed il tipo `String` è un ottimo esempio.

Ci concentreremo sugli aspetti delle `String` legate all'ownership. Questi aspetti si applicano anche ad altri tipi di dati complessi, indipendentemente dal fatto che siano forniti dalla standard library o che siano creati da te. Discuteremo le `String` con maggior dettaglio nel [Capitolo 8][ch8]<!-- ignore -->.

Abbiamo già visto delle string literals, dove un valore stringa è codificato nel nostro programma. Le string literals sono pratiche, ma non sono adatte a tutte le situazioni nelle quali vogliamo usare un testo. Una delle motivazioni è che sono immutabili. Un altro è che non sempre possiamo conoscere i valori stringa di cui necessitiamo nel momento in cui scriviamo il nostro codice: per esempio, cosa dovremmo fare se volessimo prendere una stringa in input dall'utente e memorizzarla? Per queste situazioni, Rust ha un secondo tipo stringa, `String`. Questo tipo gestisce dati allocati nella coda che in quanto tale è capace di memorizzare un quantitativo di testo sconosciuto durante la compilazione. Puoi creare una `String` da una string
literal usando la funzione `from` , così:

```rust
let s = String::from("hello");
```

L'operatore doppi due punti `::` ci permette di richiamare la funzione `from`
dal namespace del tipo `String` piuttosto che usare qualche nome astruso come `string_from`. Discuteremo questa sintassi più nel dettaglio nella sezione [“Sintassi dei Metodi”][method-syntax]<!-- ignore --> del Capitolo 5, e quando parleremo del namespacing con i moduli in [“Paths for Referring to an Item in the
Module Tree”][paths-module-tree]<!-- ignore --> in Chapter 7.

Questo tipo di stringa *può* essere modificata:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```

Quindi, qual è la differenza? Perché `String` può essere mutata ma le literals
non possono? La differenza sta in come questi due tipi gestiscono la memoria.

### Memoria ed Allocazione

Nel caso di una string literal, ne conosciamo il contenuto durante la compilazione, quindi il testo è codificato direttamente nel file eseguibile finale. Questo è il motivo per cui le string literals sono veloci ed efficienti. Ma queste proprietà derivano unicamente dall'immutabilita della string
literal. Sfortunatamente, non possiamo inserire un blob di memoria nei binari per ogni pezzo di testo la cui dimensione è sconosciuta durante la compilazione e la cui dimensione potrebbe cambiare durante l'esecuzione del programma.

Con il tipo `String` , per poter supportare un testo modificabile e con dimensione variabile, abbiamo bisogno di allocare un quantitativo di memoria nella coda, sconosciuto durante la compilazione, per mantenerne in memoria i contenuti. Questo significa che:

* La memoria deve essere richiesta all'allocatore di memoria a runtime.
* Abbiamo bisogno di un modo per restituire questa memoria all'allocatore quando abbiamo finito con la nostra `String`.

La prima parte la facciamo noi: quando chiamiamo `String::from`, la sua implementazione richiede la memoria di cui necessita. Questo è abbastanza universale per i linguaggi di programmazione.

Tuttavia, la seconda parte è differente. Nei linguaggi con un *garbage collector
(GC)*, il GC tiene traccia della memoria e la ripulisce nel momento in cui non è più utilizzata, e non siamo obbligati a pensarci. Nella maggior parte dei linguaggi senza GC,
è nostra responsabilità identificare quando la memoria non è più utilizzata e chiamare del codice per liberarla esplicitamente, allo stesso modo di come abbiamo fatto con la richiesta. Fare questa cosa correttamente è storicamente un problema di programmazione parecchio ostico. Se ce ne dimentichiamo, sprecheremo memoria. se lo facciamo troppo presto, avremo delle variabili non valide. Anche farlo due volte risulterà in un bug. Dobbiamo associare un singolo `allocate` con un singolo `free`.

Rust applica un diverso approccio: la memoria viene restituita automaticamente quando la variabile che la "possiede" va out of scope. Questa è una versione del nostro esempio scope da Listing 4-1 che usa una `String` invece di una string literal:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```

C'è un punto naturale nel quale possiamo restituire all'allocatore la memoria di cui necessita `String`: quando `s` va out of scope. Quando una variabile va out of scope, Rust chiama una funzione speciale per noi. Questa funzione si chiama
[`drop`][drop]<!-- ignore -->, ed è dove l'autore di `String` può mettere il codice di restituzione della memoria. Rust chiama `drop` automaticamente alla parentesi graffa chiusa.

> Nota: In C++, questo pattern di deallocazione delle risorse alla fine del ciclo di vita di un "oggetto" è a volte chiamato *Resource Acquisition Is Initialization (RAII)*.
> La funzione `drop` in Rust ti sarà familiare se hai usato pattern RAII.

Questo pattern ha un profondo impatto nel modo in cui il codice Rust viene scritto. Potrebbe sembrare semplice ora, ma il comportamento del codice può essere inaspettato in situazioni più complicate nelle quali vogliamo che più variabili usino i dati che abbiamo allocato nella coda. Ora esploriamo alcune di queste situazioni.

<!-- Old heading. Do not remove or links may break. -->

<a id="ways-variables-and-data-interact-move"></a>

#### Variabili e Dati che Interagiscono con Move

Più variabili possono interagire con gli stessi dati in maniere differenti in Rust.
Diamo uno sguardo ad un esempio usando un intero in Listing 4-2.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

<span class="caption">Listing 4-2: Assegnazione del valore intero della variabile `x`
ad `y`</span>

Possiamo supporre cosa sta succedendo: “associa il valore `5` ad `x`; e poi fa una copia del valore in `x` e lo associa ad `y`.” Adesso abbiamo due variabili, `x`
ed `y`, ed entrambe sono uguali a `5`. Questo è effettivamente cosa sta accadendo, perché gli interi sono valori semplici con una dimensione conosciuta e fissa, e questi due valori `5` vengono "pushati" nella pila.

Adesso diamo un'occhiata alla versione `String` :

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```

Questo somiglia molto all'esempio precedente, quindi potremmo assumere che funzionerà nello stesso modo: ovvero, la seconda riga dovrebbe fare una copia del valore in `s1` e lo associa ad `s2`. Ma non è propriamente ciò che succede.

Osserva la Figure 4-1 per capire ciò che sta succedendo alla `String` sotto al cofano. Una `String` è composta da tre parti, partendo da destra: un puntatore alla memoria che contiene il contenuto della stringa, la lunghezza, e la capacità.
Questo gruppo di dati è contenuto nella pila. A destra c'è la memoria della coda che contiene il contenuto.

<img alt="Due tabelle: la prima tabella contiene la rappresentazione di s1 nella pila, consiste nella sua lunghezza (5), capacità (5), ed un puntatore al primo valore nella seconda tabella. La seconda tabella contiene la rappresentazione del dato stringa nella coda, byte per byte." src="img/trpl04-01.svg" class="center"
style="width: 50%;" />

<span class="caption">Figure 4-1: Rappresentazione nella memoria di una `String`
contenente il valore `"hello"` associato ad `s1`</span>

La lunghezza è quanta memoria, in byte, il contenuto della `String` è attualmente in uso. La capacità è il quantitativo totale di memoria, in byte, che la
`String` ha ricevuto dall'allocatore. La differenza tra lunghezza e
capacità è importante, ma non in questo contesto, quindi per ora, va bene ignorare la capacità.

Quando assegnamo `s1` ad `s2`, i dati della `String` vengono copiati, ciò significa che copiamo il puntatore, la lunghezza, e la capacità che si trovano nella pila. Non copiamo i dati nella coda che vengono puntati dal puntatore. In altre parole, la rappresentazione dei dati in memoria somiglia alla Figure 4-2.

<img alt="Tre tabelle: le tabelle s1 ed s2 rappresentano quelle stringhe sulla pila, rispettivamente, ed entrambe puntano agli stessi dati stringa nella coda."
src="img/trpl04-02.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-2: Rappresentazione nella memoria della variabile  `s2` che ha una copia di puntatore, lunghezza, e capacità di `s1`</span>

La rappresentazione in memoria non è simile alla Figure 4-3, che è come la memoria sarebbe organizzata se Rust avesse invece copiato anche i dati della coda. Se Rust avesse fatto questo, l'operazione `s2 = s1` potrebbe essere veramente dispendiosa in termini di performance di runtime se i dati nella coda fossero di grandi dimensioni.

<img alt="Quattro tabelle: due tabelle rappresentano i dati della pila per s1 ed s2, ed ognuno che punta alla sua copia personale dei dati stringa nella coda."
src="img/trpl04-03.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-3: Un'altra possibilità di cosa `s2 = s1` sarebbe potuto essere se Rust avesse copiato anche i dati nella coda</span>

Prima abbiamo detto che quando una variabile va out of scope, Rust chiama automaticamente la funzione `drop` e ripulisce la memoria della coda per quella variabile. Ma la Figure 4-2 mostra entrambi i puntatori puntare alla stessa locazione. Questo è un problema: quand `s2` ed `s1` vanno out of scope, proveranno entrambe a liberare la stessa memoria. Quest'operazione è nota come *double free* error ed è uno dei bug dell'integrità della memoria che abbiamo menzionato precedentemente. Liberare la memoria due volte può portare a corruzione della memoria, che può portare a potenziali vulnerabilità di sicurezza.

Per assicurare l'integrità della memoria, dopo la riga `let s2 = s1;`, Rust considerà `s1` come non più valido. Ragion per cui, Rust non deve liberare nulla quando `s1` va
out of scope. Osserva cosa succede quando provi ad usare `s1` dopo che `s2` è stata creata; non funzionerà:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/src/main.rs:here}}
```

Avrai un errore come questo perché Rust ti impedisce di usare reference invalidate:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/output.txt}}
```

Se hai sentito i termini *shallow copy* e *deep copy* mentre lavoravi con altri linguaggi, il concetto di copiare puntatore, lunghezza e capacità senza copiare i dati probabilmente ti sembrerà come se stessi facendo una shallow copy. Ma per via del fatto che Rust invalida anche la prima variabile, invece di essere una shallow copy, questa operazione è nota come un *move*. In quest'esempio, potremmo dire che `s1`
è stata *spostata* in `s2`. Quindi, ciò che realmente succede viene mostrato in Figure 4-4.

<img alt="Tre tabelle: le tabelle s1 ed s2 rappresentano quelle stringhe nella pila, rispettivamente, ed entrambe puntano agli stessi dati stringa nella coda.
La tabella s1 è ingrigita perché s1 non è più valida; solo s2 può essere usata per accedere ai dati nella coda." src="img/trpl04-04.svg" class="center" style="width:
50%;" />

<span class="caption">Figure 4-4: Rappresentazione in memoria dopo che `s1` è stata invalidata</span>

Questo risolve il nostro problema! Con solo `s2` valida, quando quest'ultima va out of scope sarà l'unica a liberare la memoria, ed avremo fatto.

In aggiunta a ciò, c'è una scelta progettuale dovuta a ciò: Rust non creerà mai una copia “deep” dei tuoi dati. Ragion per cui, qualunque copia *automatica* può essere considerata come non dispendiosa in termini di performance di runtime.

<!-- Old heading. Do not remove or links may break. -->

<a id="ways-variables-and-data-interact-clone"></a>

#### Interazioni di Variabili e Dati con Clone

Se *vogliamo* fare una deep copy dei dati della coda di `String`, non solo i dati nella pila, possiamo usare un metodo comune chiamato `clone`. Discuteremo la sintassi dei metodi nel Capitolo 5, ma visto che i metodi sono una feature comune a molti linguaggi di programmazione, è probabile che tu li abbia visti in precedenza.

Ecco un esempio del metodo `clone` in azione:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-05-clone/src/main.rs:here}}
```

Questo funziona perfettamente e produce esplicitamente il comportamento mostrato nella Figure 4-3, dove i dati della coda *vengono* copiati.

Quando vedi una chiamata di `clone`, sai che del codice arbitrario verrà eseguito e che quel codice sarà dispendioso in termini di performance. È un indicatore visivo che qualcosa di diverso sta accadendo.

#### Dati Esclusivi della Pila: Copia

C'è un'altra particolarità di cui non abbiamo ancora parlato. Questo codice usa gli interi —una parte di esso è stata mostrata nel Listing 4-2—funziona ed è valido:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-06-copy/src/main.rs:here}}
```

Ma questo codice sembra contraddire ciò che abbiamo appena imparato: Non c'è stato bisogno di chiamare `clone`, ma `x` continua ad essere valido e non è stato spostato in `y`.

La ragione sta nel fatto che tipi come gli interi che hanno una dimensione nota durante la compilazione sono memorizzati interamente nella pila, quindi le copie dei valori reali sono molto veloci da fare. Ciò significa che non c'è ragione di rendere non più valida ad `x` dopo aver creato la variabile `y`. In altre parole, qui non c'è differenza tra copia deep e shallow, quindi chiamare `clone` non farebbe nulla di diverso dalla solita copia shallow, quindi possiamo evitare di usarla.

Rust ha una notazione speciale chiamata trait (tratto) `Copy` che possiamo mettere sui tipi che sono memorizzati nella pila, come lo sono gli interi (parleremo meglio dei trait nel [Capitolo 10][traits]<!-- ignore -->). Se un tipo implementa il trait `Copy`, le variabili che lo usano non faranno un move, ma verranno semplicemente copiate,
rendendole ancora valide dopo l'assegnazione ad un'altra variabile.

Rust non ci permette di annotare un tipo con `Copy` se il tipo, od una qualunque delle sue parti, ha implementato il trait `Drop`. Se il tipo necessita che avvenga qualcosa di speciale quando il valore va out of scope ed aggiungiamo l'annotazione `Copy` a quel tipo, avremo un errore durante la compilazione. Per apprendere come aggiungere l'annotazione `Copy` al tuo tipo per implementarne il tratto, dai un'occhiata a [“Trait Derivabili”][derivable-traits]<!-- ignore --> nell'appendice C.

Quindi, che tipi implementano il trait `Copy` ? Puoi controllare la documentazione per ogni tipo per essere sicuro, ma come regola generale, ogni gruppo di valori scalari semplici può implementare `Copy`, e nulla che richieda l'allocazione o qualche forma di risorsa può implementare `Copy`. Qui ci sono alcuni dei tipi che implementano `Copy`:

* Tutti i tipi interi, come ad esempio `u32`.
* Il tipo boleano, `bool`, con valori `true` e `false`.
* tutti i tipi in virgola mobile, come ad esempio `f64`.
* Il tipo carattere, `char`.
* Le tuple, se contengono solamente tipi che implementano `Copy`. Per esempio,
  `(i32, i32)` implementa `Copy`, ma `(i32, String)` non lo fa.

### Ownership e Funzioni

Il meccanismo di passaggio del valore ad una funzione è simile a quello di quando si assegna un valore ad una variabile. Passare una variabile ad una funzione farà il move o la copia, nello stesso modo in cui lo farebbe nell'assegnazione. Il Listing 4-3 ha un esempio con alcune annotazioni che mostra dove le variabili entrano ed escono dallo scope.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```

<span class="caption">Listing 4-3: Funzioni con ownership e scope annotato</span>

Se proviamo ad usare `s` dopo aver chiamato `takes_ownership`, Rust lancerà un errore di compilazione. Questi controlli statici ci proteggono dagli errori. Proviamo ad aggiungere del codice al `main` che usa `s` ed `x` per vedere dove puoi usarle e dove le regole di ownership ti impediscono di farlo.

### Ritornare Valori e Scope

Ritornare un valore può anche trasferirne l'ownership. Il Listing 4-4 mostra un esempio di una funzione che ritorna alcuni valori, con annotazioni simili a quelle del Listing
4-3.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```

<span class="caption">Listing 4-4: Trasferire l'ownership di un valore ritornato</span>

L'ownership di una variabile segue lo stesso pattern tutte le volte: assegnare un valore ad un'altra variabile ne fa il move. Quando una variabile che include dati nella coda va out of scope, il valore sarà ripulito da `drop` a meno ché l'ownership
dei dati non sia stata spostata ad un'altra variabile.

Nonostante funzioni, prendere l'ownership e poi ritornarla con ogni funzione è abbastanza tedioso. Cosa dovremmo fare se volessimo fare in modo che una funzione possa usare un valore ma non ne prenda l'ownership? È abbastanza fastidioso che qualunque cosa passiamo deve essere ridata indietro se vogliamo usarla di nuovo, insieme ad ogni dato risultante dal body della funzione che potremmo voler ritornare.

Rust ci lascia ritornare diversi valori usando una tupla, come mostrato nel Listing 4-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-05/src/main.rs}}
```

<span class="caption">Listing 4-5: Ritornare l'ownership dei parametri</span>

Ma ci sono troppe formalità e molto lavoro per un concept che dovrebbe essere comune. Fortunatamente per noi, Rust ha una feature per usare un valore senza trasferirne l'ownership, chiamata *reference*.

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
