## Hello, Cargo!

Cargo è il sistema di build e gestore di pacchetti di Rust. Molti Rustaceans usano questo strumento per gestire i loro progetti Rust perché Cargo gestisce molti compiti per te, come il building del codice, il download delle librerie da cui dipende il tuo codice, ed il building di suddette librerie. (Chiamiamo le librerie di cui il tuo codice necessita *dipendenze*.)

I programmi Rust più semplici, come quello che abbiamo scritto fin qui, non ha alcuna dipendenza. se avessimo buildato il progetto “Hello, world!” con Cargo, userebbe solo la parte di Cargo che si occupa del building del codice. Man mano che scrivi programmi Rust più complessi, aggiungerai dipendenze, e se inizi un progetto usando Cargo, aggiungere dipendenze sarà molto più facile.

Poiché la maggior parte dei progetti Rust usa Cargo, il resto di questo libro assume che anche tu stia usando Cargo. Cargo sarà stato già installato con Rust se hai usato l'installer ufficiale di cui abbiamo parlato nella sezione
[“Installazione”][installation]<!-- ignore -->. Se hai installato Rust in altri modi, controlla se Cargo è stato installato inserendo il seguente comando nel terminale:

```console
$ cargo --version
```

Se vedi il numero della versione, ce l'hai! Se vedi un errore, come `command
not found`, controlla la documentazione per il tuo metodo di installazione per installare per capire come installare Cargo separatamente.

### Creare un Progetto con Cargo

Creiamo un nuovo progetto usando Cargo e vediamo le differenze dal nostro progetto originale "Hello, world!”. Torna alla tua cartella *projects* (o dovunque tu abbia deciso di memorizzare il tuo codice). Quindi, su qualsiasi sistema operativo,0
esegui i seguenti comandi:

```console
$ cargo new hello_cargo
$ cd hello_cargo
```

Il primo comando crea un nuovo progetto con la sua relativa cartella e la chiama *hello_cargo*.
Abbiamo chiamato il nostro progetto *hello_cargo*, e Cargo ha creato i suoi file in una cartella con lo stesso nome.

Vai nella cartella *hello_cargo* ed elencane i file. Vedrai che Cargo
ha generato due file ed una cartella per noi: un file *Cargo.toml* ed una cartella
*src* contenente un file *main.rs*.

Inoltre ha inizializzato un repository Git con il suo relativo file *.gitignore*.
I file Git non saranno generati se esegui `cargo new` in un repository Git già inizializzato; puoi sovrascrivere questo comportamento usando `cargo new --vcs=git`.

> Nota: Git è un version control system comune. Puoi modificare `cargo new` per utilizzare un diverso version control system o per non utilizzarne nessuno usa il flag `--vcs` . Esegui `cargo new --help` per vedere le opzioni disponibili.

Apri *Cargo.toml* in un editor di testo a tua scelta. Dovrebbe somigliare al codice in Listing 1-2.

<span class="filename">Filename: Cargo.toml</span>

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

<span class="caption">Listing 1-2: Contenuto di *Cargo.toml* generato da`cargo
new`</span>

Questo file è nel formato [*TOML*][toml]<!-- ignore --> (*Tom’s Obvious, Minimal
Language*), che è il formato di configurazione di Cargo.

La prima riga, `[package]`, è una sezione di heading che indica che le istruzioni successive configurano un pacchetto. Man mano che aggiungiamo informazioni a questo file, aggiungeremo altre sezioni.

Le tre righe successive impostano le informazioni di configurazione di cui Cargo necessita per la compilazione del tuo programma: il nome, la versione, e l'edizione di Rust da usare. Parleremo delle `edition` (edizioni) chiave in [Appendice E][appendix-e]<!-- ignore -->.

L'ultima riga, `[dependencies]`, è l'inizio di una sezione nella quale elencare qualunque dipendenza del tuo progetto. In Rust, ci si riferisce ai pacchetti di codice con il termine
*crates*. Non avremo bisogno di altri crates in questo progetto, ma ne avremo bisogno nel primo progetto nel Capitolo 2 , quindi discuteremo della sezione dependncies lì.

Adesso apri *src/main.rs* e guarda:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

Cargo ha generato per te un programma “Hello, world!” come quello che abbiamo scritto in Listing 1-1! Fin qui, le differenze tra il nostro progetto ed il progetto generato da Cargo stanno nel fatto che Cargo ha messo il codice nella cartella *src* e che abbiamo un file di configurazione *Cargo.toml* nella cartella principale.

Cargo si aspetta che i tuoi file sorgenti si trovino nella cartella *src*. La cartella principale del progetto è solo per i file README, le informazioni sulla licenza,
i file di configurazione, e qualsiasi altra cosa che non riguardi il tuo codice. Usare Cargo
ti aiuta ad organizzare i tuoi progetti. C'è un posto per qualunque cosa, ed ogni cosa è al suo posto.

Se hai iniziato un progetto che non usa Cargo, come abbiamo fatto con il nostro progetto “Hello,
world!” , puoi convertirlo in un progetto che usa Cargo. Sposta tutto il codice del progetto nella cartella *src* e crea un file *Cargo.toml* appropriato.

### Buildare ed Eseguire un Progetto Cargo

Adesso vediamo cosa cambia quando buildiamo ed eseguiamo il programma “Hello, world!” con Cargo! Dalla tua cartella *hello_cargo*, builda il tuo progetto inserendo il seguente comando:

```console
$ cargo build
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 2.85 secs
```

Questo comando crea un file eseguibile in *target/debug/hello_cargo* (o
*target\debug\hello_cargo.exe* su Windows) piuttosto che nella cartella corrente. Poiché la build di default è una build di debug. Cargo mette il file binario in una cartella chiamata *debug*. Puoi lanciare l'eseguibile con questo comando:

```console
$ ./target/debug/hello_cargo # or .\target\debug\hello_cargo.exe on Windows
Hello, world!
```

Se tutto è andato secondo i piani, dovrebbe essere stampata la scritta `Hello, world!` nel terminale. Eseguire `cargo build` per la prima volta implica anche che Cargo crei un nuovo file nella cartella principale: *Cargo.lock*. Questo file tiene traccia delle esatte versioni delle dipendenze nel tuo progetto. Questo progetto non ha dipendenze, quindi il file è un po' vuoto. non dovrai mai cambiare questo file manualmente; Cargo gestisce per te i suoi contenuti.

Abbiamo appena buildato un progetto con `cargo build` e lo abbiamo eseguito con
`./target/debug/hello_cargo`, ma possiamo anche usare `cargo run` per compilare il codice e poi eseguirne l'eseguibile risultante tutto in un solo comando:

```console
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Usare `cargo run` è più comodo rispetto a dover ricordare di eseguire `cargo
build` e poi usare l'intero percorso del file binario, quindi la maggior parte degli sviluppatori usa `cargo run`.

Nota che questa volta non abbiamo visto alcun output che indicasse che Cargo stesse compilando `hello_cargo`. Cargo è riuscito a capire che i file non sono cambiati, quindi non ha rebuildato nulla ma ha solo eseguito il binario. Se avessi modificato il tuo codice sorgente, Cargo avrebbe rebuildato il progetto prima di eseguirlo, ed avresti visto questo output:

```console
$ cargo run
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.33 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Cargo fornisce anche un comando chiamato `cargo check`. Questo comando controlla velocemente il tuo codice per assicurarsi che compili, ma non produce alcun eseguibile:

```console
$ cargo check
   Checking hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.32 secs
```

Perché dovresti non volere un eseguibile? Spesso, `cargo check` è molto più veloce di
`cargo build` perché salta il passaggio di produzione di un eseguibile. Se controlli continuamente il tuo lavoro mentre scrivi codice, usare `cargo check` velocizzerà il processo di lasciarti sapere se il tuo progetto continua a compilare! Pertanto, molti Rustaceans eseguono periodicamente `cargo check` durante la scrittura del loro programma per esser sicuri che compili. Poi eseguono `cargo build` quando sono pronti ad utilizzare l'eseguibile del programma.

Ricapitoliamo cos'abbiamo imparato su Cargo finora:

* Possiamo creare un nuovo progetto usando `cargo new`.
* Possiamo buildare un progetto usando `cargo build`.
* Possiamo buildare ed eseguire in un unico passaggio usando `cargo run`.
* Possiamo buildare un progetto senza produrre alcun eseguibile per controllare se siano presenti errori usando `cargo check`.
* Invece di salvare il risultato della build nella stessa cartella come nel nostro vecchio codice, Cargo lo memorizza nella cartella *target/debug*.

Usare Cargo porta il vantaggio aggiuntivo di avere sempre gli stessi comandi senza doverci preoccupare del sistema operativo su cui stiamo lavorando. Quindi, a questo punto, non daremo più istruzioni precise per Linux e macOS o Windows.

### Buildare una versione Release

Quando il tuo progetto è finalmente pronto per essere rilasciato, puoi usare `cargo build --release` per compilarlo con delle ottimizzazioni. questo comando creerà un eseguibile in *target/release* invece che in *target/debug*. Le ottimizzazioni fanno in modo che il tuo codice Rust venga eseguito in maniera più veloce, ma quando vengono abilitate queste aumentano il tempo necessario per la compilazione del tuo programma. Questa è la motivazione dietro la scelta di due profili: uno per lo sviluppo, pensato per quando vuoi rebuildare spesso e velocemente, ed un altro per la build del programma finale che darai agli utenti che non lo rebuilderanno ripetutamente e che girerà alla massima velocità possibile. Se fai un benchmark dei tempi di esecuzione del tuo codice, assicurati di eseguire `cargo build --release` e testa l'eseguibile in *target/release*.

### Cargo come Convenzione

Per progetti semplici, Cargo non offre molti vantaggi rispetto all'utilizzo di
`rustc`, ma dimostrerà il suo valore quando i tuoi programmi diverranno più complessi.
Quando i programmi cresceranno in dimensioni, richiedendo l'utilizzo di molteplici file o quando si avrà bisogno di aggiungere qualche dipendenza, sarà molto più facile lasciar coordinare la build a Cargo.

Anche se il progetto `hello_cargo` resta molto semplice, adesso usa molta della strumentazione che userai in situazioni reali nel resto della tua carriera da programmatore Rust. Infatti, per lavorare su qualsiasi progetto esistente, puoi usare i seguenti comandi per fare il check out del codice usando Git, andare nella cartella del progetto, e fare una build:

```console
$ git clone example.org/someproject
$ cd someproject
$ cargo build
```

Per maggiori informazioni su Cargo, dai un'occhiata alla [sua documentazione][cargo].

## Ricapitolando

Sei già partito alla grande nella tua avventura con Rust! In questo capitolo,
hai imparato come:

* Installare l'ultima versione stabile di Rust usando `rustup`
* Aggiornare Rust ad una versione più recente
* Aprire la documentazione installata in locale
* Scrivere ed eseguire un programma “Hello, world!” usando direttamente `rustc`
* Creare ed eseguire un nuovo progetto usando le convenzioni di Cargo

Questo è un ottimo momento per produrre un programma più sostanzioso per abituarsi a leggere e scrivere codice Rust. Quindi, nel Capitolo 2, creeremo un guessing game.
Se invece preferisci iniziare imparando come funzionano alcuni concetti comuni della programmazionie, vai al Capitolo 3 e poi torna al Capitolo 2.

[installation]: ch01-01-installation.html#installation
[toml]: https://toml.io
[appendix-e]: appendix-05-editions.html
[cargo]: https://doc.rust-lang.org/cargo/
