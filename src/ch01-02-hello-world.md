## Hello, World!

Ora che Rust è installato, è ora di scrivere il tuo primo programma in Rust.
è tradizione che quando si impara un nuovo linguaggio, si scriva un piccolo programma che stampi la frase `Hello, world!` a schermo, ed è quello che faremo!

> Nota: Questo libro assume che tu abbia un minimo di familiarità con la linea di comando. Rust non richiede nulla di specifico per quanto riguarda l'editing, il tooling o dove il codice viene memorizzato, quindi se preferisci usare uno strumento di sviluppo integrato (IDE) invece della linea di comando, sentiti libero di usare il tuo IDE preferito. Molti IDE adesso hanno diversi gradi di supporto a Rust; controlla la documentazione del tuo IDE di riferimento per i dettagli. Il team di Rust
> si è concentrato nel dare grande supporto agli IDE via `rust-analyzer`. Vedi
> [Appendice D][devtools]<!-- ignore --> Per maggiori dettagli.

### Crea una cartella del Progetto

Iniziamo creando una cartella dove memorizzare il tuo codice Rust.  Non importa dove salvi il tuo codice, ma per quest'esercizio e per i progetti di questo libro, ti consigliamo di creare una cartella *projects* nella tua cartella home e conservare lì i tuoi progetti.

Apri un terminale ed inserisci i seguenti comandi per creare la cartella *projects* e la cartella per il progetto “Hello, world!” nella cartella *projects*.

Su Linux, macOS, e in PowerShell su Windows, inserisci:

```console
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

Per Windows CMD, inserisci:

```cmd
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

### Scrivere ed eseguire un programma in Rust

Dopodiché, crea un nuovo file sorgente e chiamalo *main.rs*. I file Rust terminano sempre con l'estensione *.rs*. Se stai usando più di una parola nel nome del tuo file, è convenzione utilizzare un underscore per separarle. Per esempio, si usa
*hello_world.rs* piuttosto che *helloworld.rs*.

ora apri il file *main.rs* che hai appena creato ed inserisci il codice in Listing 1-1.

<span class="filename">Nome del file: main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

<span class="caption">Listing 1-1: Un programma che stampa `Hello, world!`</span>

Salva il file e torna alla finestra del tuo terminale nella cartella 
*~/projects/hello_world*. Su Linux o macOS, inserisci il seguente comando per compilare ed eseguire il file:

```console
$ rustc main.rs
$ ./main
Hello, world!
```

Su Windows, inserisci il comando `.\main.exe` invece di `./main`:

```powershell
> rustc main.rs
> .\main.exe
Hello, world!
```

Indipendentemente dal tuo sistema operativo, dovrebbe essere stampata nel terminale la stringa `Hello, world!`. se non vedi quest'output, fai riferimento alla parte della sezione Installazione
[“Troubleshooting”][troubleshooting]<!-- ignore --> per ricevere aiuto.

Se `Hello, world!` viene stampato, congratulazioni! Hai ufficialmente scritto un programma in Rust. Ciò ti rende un programmatore Rust, benvenuto/a!

### Struttura di un Programma Rust

Rivediamo questo programma “Hello, world!” nel dettaglio. Qui c'è il primo pezzo del puzzle:

```rust
fn main() {

}
```

Queste righe definiscono una funzione denominata `main`. La funzione `main` è speciale: è sempre la prima porzione di codice che viene eseguita in qualunque programma in Rust. Qui, la prima riga dichiara una funzione chiamata `main` che non ha parametri e che non ritorna nulla. se ci fossero stati parametri, sarebbero stati inseriti nelle parentesi `()`.

Il corpo della funzione è contenuto in `{}`. Rust richiede che le parentesi graffe includano tutto il corpo della funzione. È stilisticamente apprezzabile che l'apertura delle parentesi graffe venga piazzata sulla stessa riga della dichiarazione della funzione, aggiungendo uno spazio nel mezzo.

> Nota: Se vuoi rimanere aderente allo stile standard nei tuoi progetti Rust, puoi usare un tool di formattazione automatica chiamato  `rustfmt` per formattare il tuo codice in un particolare stile (più informazioni su `rustfmt` si trovano in
> [Appendice D][devtools]<!-- ignore -->). Il team di Rust ha incluso questo strumento nella distribuzione standard di Rust, come fatto con `rustc` , quindi dovrebbe essere già installato sul tuo computer!

Il corpo della funzione `main` contiene il seguente codice:

```rust
    println!("Hello, world!");
```

Questa riga fa tutto ciò che deve essere fatto in questo programma: stampa testo a schermo. Ci sono quattro dettagli importanti da notare qui.

Primo, lo stile di Rust prevede l'indentazione fatta tramite quattro spazi, e non tramite un tab.

Secondo, `println!` richiama una macro di Rust. Se avesse invece richiamato una funzione, avremmo inserito `println` (senza il `!`). Discuteremo le macro più in dettaglio nel Capitolo 19. Per ora, devi solo sapere che usare `!`
significa che stai richiamando una macro invece che una normale funzione, e che le macro non seguono sempre le stesse regole di una funzione.

Terzo, vedrai la stringa `"Hello, world!"`. Qui stiamo passando la stringa come argomento a `println!`, e la stringa è stampata a schermo.

Quarto, finiamo la riga con un punto e virgola (`;`), che indica che quest'espressione è finita e che la prossima è pronta ad iniziare. Gran parte delle righe in codice Rust
finiscono con un punto e virgola.

### Compilare ed Eseguire Sono Passaggi Separati

Hai appena eseguito un programma appena creato, dunque esaminiamo ogni passaggio di questo processo.

Prima dell'esecuzione di un programma Rust, devi compilarlo usando il compilatore Rust inserendo il comando `rustc` e passandogli il nome del file sorgente, così:

```console
$ rustc main.rs
```

Se hai un background da programmatore C o C++ , avrai notato che è simile a `gcc`
o `clang`. Dopo la corretta compilazione, Rust darà in output un eseguibile binario.

Su Linux, macOS, e PowerShell su Windows, puoi vedere l'eseguibile inserendo il comando `ls` nella tua shell:

```console
$ ls
main  main.rs
```

Su Linux e macOS, vedrai due file. Con PowerShell su Windows, vedrai gli stessi tre file che vedresti usando CMD. Con CMD su Windows, dovrai inserire:

```cmd
> dir /B %= the /B option says to only show the file names =%
main.exe
main.pdb
main.rs
```

Questo mostra il file sorgente con l'estensione  *.rs* , il file eseguibile
(*main.exe* su Windows, *main* su tutte le altre piattaforme), e, quando si usa Windows, un file contenente le informazioni di debug con estensione *.pdb*.
Da qui, esegui il file *main* o *main.exe*, così:

```console
$ ./main # or .\main.exe on Windows
```

Se il tuo *main.rs* è il tuo programma “Hello, world!” , questa riga stampa `Hello,
world!` sul tuo terminale.

Se hai più familiarità con un linguaggio dinamico, come Ruby, Python, o
JavaScript, potresti non essere abituato a compilare ed eseguire il programma come passaggi separati. Rust è un linguaggio *ahead-of-time compiled*, ciò significa che puoi compilare il programma e dare l'eseguibile a qualcuno, che potrà eseguirlo senza neanche avere Rust installato. Se dai a qualcuno il tuo file *.rb*, *.py*, or
*.js*, dovranno avere un'implementazione Ruby, Python, o JavaScript installata (rispettivamente). Ma in questi linguaggi, avrai bisogno di un solo comando per compilare ed eseguire il tuo programma. È tutta questione di compromessi nella progettazione del linguaggio.

È perfettamente normale compilare con `rustc` per programmi semplici, ma man mano che il tuo progetto diventa più grande, vorrai gestire tutte le opzioni e rendere facile la condivisione del tuo codice. Prossimamente to introdurremo a Cargo, che ti aiuta a sviluppare dei programmi Rust in situazioni più realistiche.

[troubleshooting]: ch01-01-installation.html#troubleshooting
[devtools]: appendix-04-useful-development-tools.md
