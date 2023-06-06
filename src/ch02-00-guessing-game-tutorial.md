# Programmare un Guessing Game

Addentriamoci nella programmazione in Rust lavorando insieme ad un progetto più pratico! Questo capitolo ti introduce a diversi concetti comuni di Rust mostrandoti come usarli in un programma reale. Imparerai come usare `let`, `match`, metodi, funzioni associate, crate esterni, ed altro ancora! Nei capitoli successivi, esploreremo questi concetti con maggior dettaglio. In questo capitolo, farai solo pratica dei fondamenti.

Implementeremo un classico problema per programmatori principianti: un guessing game. Ecco come funzionerà: il programma genererà un intero casuale tra 1 e 100. Poi chiederà all'utente di indovinare. Dopo che l'utente avrà inserito la sua risposta, il programma gli indicherà se la sua risposta è troppo bassa o troppo alta. Se la risposta è corretta, il gioco stamperà un messaggio di congratulazioni ed uscirà.

## Impostare un Nuovo Progetto

Per impostare un nuovo progetto, vai alla cartella *projects* che hai creato nel Capitolo 1 e crea un nuovo progetto usando Cargo, così:

```console
$ cargo new guessing_game
$ cd guessing_game
```

Il primo comando, `cargo new`, prende il nome del progetto (`guessing_game`)
come primo argomento. Il secondo comando va alla cartella del nuovo progetto.

Guarda il file *Cargo.toml* appena generato:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial
rm -rf no-listing-01-cargo-new
cargo new no-listing-01-cargo-new --name guessing_game
cd no-listing-01-cargo-new
cargo run > output.txt 2>&1
cd ../../..
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/Cargo.toml}}
```

Come hai visto nel primo capitolo, `cargo new` genera un programma “Hello, world!” per te. Controlla il file *src/main.rs*:

<span class="filename">Nome File: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/src/main.rs}}
```

Ora compila questo programma “Hello, world!” ed eseguilo allo stesso tempo usando il comando `cargo run`:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/output.txt}}
```

Il comando `run` diventa utile quando hai bisogno di iterare rapidamente durante un progetto, come faremo con questo gioco, testando rapidamente ogni iterazione prima di andare avanti alla prossima.

Riapri il file *src/main.rs*. Scriverai tutto il codice in questo file.

## Elaborare una Risposta

La prima parte del guessing game chiederà all'utente un input, processerà
quest'input, e controllerà che l'input sia nella forma che si attende. Per iniziare, permetteremo al giocatore di inserire una risposta. Inserici il codice presente in Listing 2-1 in *src/main.rs*.

<span class="filename">Nome File: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:all}}
```

<span class="caption">Listing 2-1: Codice che prende la risposta dall'utente e la stampa</span>

Questo codice contiene molte informazioni, perciò analizziamolo riga per riga. Per ottenere un input dall'utente e poi stampare il risultato come output, dobbiamo importare la libreria di input/output `io` nello scope. La libreria `io` viene dalla libreria standard, conosciuta come `std`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:io}}
```

Di default, Rust ha un set di elementi definiti nella standard library che vengono portati nello scope di ogni programma. Questo set è chiamato il *prelude*, e puoi vedere tutto ciò che contiene [nella documentazione della standard library (inglese)][prelude].

Se un tipo che vuoi usare non è presente nel prelude, dovrai portare quel tipo in scope esplicitandolo con uno  `use` statement. Usare la libreria `std::io` ti fornisce un buon numero di feature utili, che includono l'abilità di accettare input dall'utente.

Come hai visto nel Capitolo 1, la funzione `main` è l'entry point all'interno del programma:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:main}}
```

La sintassi `fn` dichiara una nuova funzione; le parentesi tonde vuote, `()`, indicano che non sono presenti particolari parametri; e la parentesi graffa, `{`, indica l'inizio del body della funzione.

Come avrai imparato nel Capitolo 1, `println!` è una macro che stampa una stringa a schermo:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print}}
```

Questo codice sta stampando una richiesta che dice qual è il gioco e chiedendo un input all'utente.

### Memorizzare valori con le variabili

Successivamente, creeremo una *variabile* per memorizzare l'input dell'utente, così:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:string}}
```

Ora il programma si sta facendo interessante! Stanno succedendo molte cose in questa piccola riga. Usiamo lo statement `let` per creare la variabile. Ecco un altro esempio:

```rust,ignore
let apples = 5;
```

Questa riga crea una nuova variabile chiamata `apples` e le dà il valore 5. In
Rust, le variabili sono rese immutabili di default, ciò significa che una volta che diamo un valore ad una variabile, quel valore non cambierà. Discuteremo questo concetto più in dettaglio nella sezione [“Variabili e Mutabilità”][variables-and-mutability]<!-- ignore -->
del Capitolo 3. Per rendere mutabile una variabile, aggiungiamo `mut` prima del nome della variabile:

```rust,ignore
let apples = 5; // immutable
let mut bananas = 5; // mutable
```

> Nota: La sintassi `//` indica l'inizio di un commento che continua fino alla fine della riga. Rust ignora tutto ciò che è presente nei commenti. Discuteremo dei commenti più in dettaglio nel [Capitolo 3][comments]<!-- ignore -->.

Tornando al nostro programma, adesso sai che `let mut guess` introdurrà una variabile mutabile chiamata `guess`. Il segno uguale (`=`) dice a Rust che vogliamo assegnare qualcosa alla variabile. A destra del segno uguale c'è il valore che sarà assegnato a  `guess` , che è il risultato della chiamata di `String::new`, una funzione che ritorna una nuova istanza di una `String`.
[`String`][string]<!-- ignore --> è un tipo fornito dalla standard
library, ed è un pezzettino di testo estendibile, codificato in UTF-8.

La sintassi `::` nella riga `::new` indica che `new` è una funzione associata al tipo `String`. Una *funzione associata* è una funzione che viene implementata per uno specifico tipo, in questo caso `String`. Questa funzione `new` crea una nuova stringa vuota. Troverai una funzione `new` in molti tipi perché è un nome comune per una funzione che crea un nuovo valore di qualche sorta.

Nella sua interezza, la riga `let mut guess = String::new();` ha creato una variabile mutabile a cui è assegnata attualmente una nuova istanza vuota di una `String`. Whew!

### Prendere Input dall'Utente

Ricorda che abbiamo incluso le funzionalità di input/output functionality dalla standard
library con `use std::io;` nella prima riga del programma. Ora chiameremo la funzione `stdin` dal modulo `io` , che ci permetterà di gestire gli input dell'utente:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:read}}
```

Se non avessimo importato la libreria `io` con `use std::io;` all'inizio del programma, potremmo continuare ad utilizzare la funzione scrivendo la sua chiamata come `std::io::stdin`. La funzione `stdin` ritorna un'istanza di [`std::io::Stdin`][iostdin]<!-- ignore -->, che è un tipo che rappresenta un handle per il tuo terminale.

Successivamente, la riga `.read_line(&mut guess)` chiama il metodo [`read_line`][read_line]<!--
ignore --> dell'handle dello standard input per prendere input dall'utente.
Passeremo anche `&mut guess` come argomento a `read_line` per indicarle in quale stringa memorizzare l'input. Il lavoro di `read_line` è quello di prendere qualunque cosa l'utente scriva nello standard input e aggiungerlo in coda ad una stringa
(senza sovrascriverne il contenuto), è ragione per cui passiamo quella stringa come argomento. L'argomento stringa necessita di essere mutabile, in modo tale che il metodo possa cambiarne il contenuto.

L' `&` indica che questo argomento è una *reference*, che ti dà un modo per dare accesso ad un pezzo di dato a più parti di codice senza doverlo ricopiare in memoria più volte. Le reference sono una feature complessa, ed uno dei maggiori vantaggi di Rust è la facilità e sicurezza che dà nell'utilizzo delle reference. Non hai bisogno di conoscere molti di questi dettagli per finire questo programma. Per ora, tutto ciò che devi sapere è che, come le variabili, le reference sono rese immutabili di default. Quindi, dovrai scrivere `&mut guess` piuttosto che `&guess` per renderla mutabile. (il Capitolo 4 spiegherà le reference in maniera più completa.)

<!-- Old heading. Do not remove or links may break. -->

<a id="handling-potential-failure-with-the-result-type"></a>

### Gestire Potenziali Falle con `Result`

Continuiamo a lavorare su questa riga di codice. Ora parleremo di una terza riga di testo, ma nota che continua a far parte di una singola riga logica di codice. La prossima parte è questo metodo:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:expect}}
```

Avremmo potuto scrivere questo codice in questo modo:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

Tuttavia, una riga così lunga è difficile da leggere, quindi è sempre meglio dividerla in più pezzi. È spesso saggio andare a capo ed altri spazi per aiutare a spezzettare lunghe righe quando chiami un metodo con la sintassi `.method_name()` . Adesso discutiamo insieme cosa fa questa riga.

Come menzionato precedentemente, `read_line` mette qualunque cosa l'utente inserisca, nella stringa che le viene passata, ma ritorna anche un valore `Result` . [`Result`][result]<!--
ignore --> è un'[*enumeration (enumerazione in inglese)*][enums]<!-- ignore -->, spesso chiamato *enum*,
ovvero un tipo che può trovarsi in uno o più stati. Chiamiamo ogni possibile stato una *variant* (variante in inglese).

[Il Capitolo 6][enums]<!-- ignore --> coprirà gli enum in maniera più dettagliata. Lo scopo di questi tipi `Result` è quello di codificare le informazioni di error-handling.

Le varianti di `Result` sono `Ok` ed `Err`. La variante `Ok` indica che l'operazione è avvenuta con successo, ed all'interno di `Ok` il valore è stato generato correttamente.
La variante `Err` indica il fallimento dell'operazione, ed `Err` contiene informazioni
su come o perché l'operazione sia fallita.

I valori del tipo `Result` , come i valori di qualunque tipo, hanno metodi definiti su di loro. Un istanza di `Result` ha un [metodo `expect`][expect]<!-- ignore -->
che puoi richiamare. Se quest'istanza di `Result` è un valore `Err` , `expect`
manderà in crash il programma, e mostrerà un messaggio con il messaggio che hai passato come argomento ad `expect`. Se il metodo `read_line` ritorna un `Err`, sarà probabile che il risultato di un errore arrivi dal sistema operativo sottostante.
Se l'istanza di `Result` è un valore `Ok` , `expect` prenderà il valore return che `Ok` contiene e lo ritorna a te, in modo tale che tu possa utilizzarlo. In questo caso, quel valore è un numero di byte nell'input dell'utente.

Se non chiami `expect`, il programma compilerà, ma ti darà un warning:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-02-without-expect/output.txt}}
```

Rust ti avverte del mancato utilizzo del valore `Result` ritornato da `read_line`,
indicando che il programma non ha gestito un possibile errore.

Il modo giusto per evitare il warning è scrivere il codice di error-handling,
ma nel nostro caso, vogliamo che il programma crashi qualora sopraggiunga un promlema, quindi possiamo usare `expect`. Imparerai di più su come gestire gli errori nel [Capitolo 9][recover]<!-- ignore -->.

### Stampare Valori con `println!` ed i Placeholder

Oltre alla parentesi graffa di chiusura, c'è solo un'altra riga di cui parlare nel codice che abbiamo scritto fino ad ora:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print_guess}}
```

Questa riga stampa la stringa che ora contiene l'input dell'utente. La `{}` coppia di parentesi graffe sono un placeholder (segnaposto): pensa a `{}` come ad una piccola chela di granchio che mantiene il valore al suo posto. Quando stampiamo il valore di una variabile, il nome della stessa va nelle parentesi graffe. Quando stampi il risultato di un'espressione, metti delle parentesi graffe vuote nella stringa formattata, e poi inserisci la lista di espressioni da stampare separate l'una dall'altra da una virgola nello stesso ordine nel quale hai posizionato nella stringa ogni coppia di parentesi graffe vuote placeholder. Stampare una variabile ed il risultato di un'espressione in una sola chiamata di `println!` sarà simile al seguente esempio:

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
```

Quello che verrà stampato da questo codice è  `x = 5 and y + 2 = 12`.

### Testare la Prima Parte

Testiamo la prima parte del nostro guessing game. Eseguilo usando `cargo run`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-01/
cargo clean
cargo run
input 6 -->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 6.44s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

A questo punto, la prima parte del gioco è completa: prendiamo l'input da tastiera e lo stampiamo.

## Generating a Secret Number

Next, we need to generate a secret number that the user will try to guess. The
secret number should be different every time so the game is fun to play more
than once. We’ll use a random number between 1 and 100 so the game isn’t too
difficult. Rust doesn’t yet include random number functionality in its standard
library. However, the Rust team does provide a [`rand` crate][randcrate] with
said functionality.

### Using a Crate to Get More Functionality

Remember that a crate is a collection of Rust source code files. The project
we’ve been building is a *binary crate*, which is an executable. The `rand`
crate is a *library crate*, which contains code that is intended to be used in
other programs and can’t be executed on its own.

Cargo’s coordination of external crates is where Cargo really shines. Before we
can write code that uses `rand`, we need to modify the *Cargo.toml* file to
include the `rand` crate as a dependency. Open that file now and add the
following line to the bottom, beneath the `[dependencies]` section header that
Cargo created for you. Be sure to specify `rand` exactly as we have here, with
this version number, or the code examples in this tutorial may not work:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:8:}}
```

In the *Cargo.toml* file, everything that follows a header is part of that
section that continues until another section starts. In `[dependencies]` you
tell Cargo which external crates your project depends on and which versions of
those crates you require. In this case, we specify the `rand` crate with the
semantic version specifier `0.8.5`. Cargo understands [Semantic
Versioning][semver]<!-- ignore --> (sometimes called *SemVer*), which is a
standard for writing version numbers. The specifier `0.8.5` is actually
shorthand for `^0.8.5`, which means any version that is at least 0.8.5 but
below 0.9.0.

Cargo considers these versions to have public APIs compatible with version
0.8.5, and this specification ensures you’ll get the latest patch release that
will still compile with the code in this chapter. Any version 0.9.0 or greater
is not guaranteed to have the same API as what the following examples use.

Now, without changing any of the code, let’s build the project, as shown in
Listing 2-2.

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
rm Cargo.lock
cargo clean
cargo build -->

```console
$ cargo build
    Updating crates.io index
  Downloaded rand v0.8.5
  Downloaded libc v0.2.127
  Downloaded getrandom v0.2.7
  Downloaded cfg-if v1.0.0
  Downloaded ppv-lite86 v0.2.16
  Downloaded rand_chacha v0.3.1
  Downloaded rand_core v0.6.3
   Compiling libc v0.2.127
   Compiling getrandom v0.2.7
   Compiling cfg-if v1.0.0
   Compiling ppv-lite86 v0.2.16
   Compiling rand_core v0.6.3
   Compiling rand_chacha v0.3.1
   Compiling rand v0.8.5
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
```

<span class="caption">Listing 2-2: The output from running `cargo build` after
adding the rand crate as a dependency</span>

You may see different version numbers (but they will all be compatible with the
code, thanks to SemVer!) and different lines (depending on the operating
system), and the lines may be in a different order.

When we include an external dependency, Cargo fetches the latest versions of
everything that dependency needs from the *registry*, which is a copy of data
from [Crates.io][cratesio]. Crates.io is where people in the Rust ecosystem
post their open source Rust projects for others to use.

After updating the registry, Cargo checks the `[dependencies]` section and
downloads any crates listed that aren’t already downloaded. In this case,
although we only listed `rand` as a dependency, Cargo also grabbed other crates
that `rand` depends on to work. After downloading the crates, Rust compiles
them and then compiles the project with the dependencies available.

If you immediately run `cargo build` again without making any changes, you
won’t get any output aside from the `Finished` line. Cargo knows it has already
downloaded and compiled the dependencies, and you haven’t changed anything
about them in your *Cargo.toml* file. Cargo also knows that you haven’t changed
anything about your code, so it doesn’t recompile that either. With nothing to
do, it simply exits.

If you open the *src/main.rs* file, make a trivial change, and then save it and
build again, you’ll only see two lines of output:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
touch src/main.rs
cargo build -->

```console
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
```

These lines show that Cargo only updates the build with your tiny change to the
*src/main.rs* file. Your dependencies haven’t changed, so Cargo knows it can
reuse what it has already downloaded and compiled for those.

#### Ensuring Reproducible Builds with the *Cargo.lock* File

Cargo has a mechanism that ensures you can rebuild the same artifact every time
you or anyone else builds your code: Cargo will use only the versions of the
dependencies you specified until you indicate otherwise. For example, say that
next week version 0.8.6 of the `rand` crate comes out, and that version
contains an important bug fix, but it also contains a regression that will
break your code. To handle this, Rust creates the *Cargo.lock* file the first
time you run `cargo build`, so we now have this in the *guessing_game*
directory.

When you build a project for the first time, Cargo figures out all the versions
of the dependencies that fit the criteria and then writes them to the
*Cargo.lock* file. When you build your project in the future, Cargo will see
that the *Cargo.lock* file exists and will use the versions specified there
rather than doing all the work of figuring out versions again. This lets you
have a reproducible build automatically. In other words, your project will
remain at 0.8.5 until you explicitly upgrade, thanks to the *Cargo.lock* file.
Because the *Cargo.lock* file is important for reproducible builds, it’s often
checked into source control with the rest of the code in your project.

#### Updating a Crate to Get a New Version

When you *do* want to update a crate, Cargo provides the command `update`,
which will ignore the *Cargo.lock* file and figure out all the latest versions
that fit your specifications in *Cargo.toml*. Cargo will then write those
versions to the *Cargo.lock* file. Otherwise, by default, Cargo will only look
for versions greater than 0.8.5 and less than 0.9.0. If the `rand` crate has
released the two new versions 0.8.6 and 0.9.0, you would see the following if
you ran `cargo update`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
cargo update
assuming there is a new 0.8.x version of rand; otherwise use another update
as a guide to creating the hypothetical output shown here -->

```console
$ cargo update
    Updating crates.io index
    Updating rand v0.8.5 -> v0.8.6
```

Cargo ignores the 0.9.0 release. At this point, you would also notice a change
in your *Cargo.lock* file noting that the version of the `rand` crate you are
now using is 0.8.6. To use `rand` version 0.9.0 or any version in the 0.9.*x*
series, you’d have to update the *Cargo.toml* file to look like this instead:

```toml
[dependencies]
rand = "0.9.0"
```

The next time you run `cargo build`, Cargo will update the registry of crates
available and reevaluate your `rand` requirements according to the new version
you have specified.

There’s a lot more to say about [Cargo][doccargo]<!-- ignore --> and [its
ecosystem][doccratesio]<!-- ignore -->, which we’ll discuss in Chapter 14, but
for now, that’s all you need to know. Cargo makes it very easy to reuse
libraries, so Rustaceans are able to write smaller projects that are assembled
from a number of packages.

### Generating a Random Number

Let’s start using `rand` to generate a number to guess. The next step is to
update *src/main.rs*, as shown in Listing 2-3.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:all}}
```

<span class="caption">Listing 2-3: Adding code to generate a random
number</span>

First we add the line `use rand::Rng;`. The `Rng` trait defines methods that
random number generators implement, and this trait must be in scope for us to
use those methods. Chapter 10 will cover traits in detail.

Next, we’re adding two lines in the middle. In the first line, we call the
`rand::thread_rng` function that gives us the particular random number
generator we’re going to use: one that is local to the current thread of
execution and is seeded by the operating system. Then we call the `gen_range`
method on the random number generator. This method is defined by the `Rng`
trait that we brought into scope with the `use rand::Rng;` statement. The
`gen_range` method takes a range expression as an argument and generates a
random number in the range. The kind of range expression we’re using here takes
the form `start..=end` and is inclusive on the lower and upper bounds, so we
need to specify `1..=100` to request a number between 1 and 100.

> Note: You won’t just know which traits to use and which methods and functions
> to call from a crate, so each crate has documentation with instructions for
> using it. Another neat feature of Cargo is that running the `cargo doc
> --open` command will build documentation provided by all your dependencies
> locally and open it in your browser. If you’re interested in other
> functionality in the `rand` crate, for example, run `cargo doc --open` and
> click `rand` in the sidebar on the left.

The second new line prints the secret number. This is useful while we’re
developing the program to be able to test it, but we’ll delete it from the
final version. It’s not much of a game if the program prints the answer as soon
as it starts!

Try running the program a few times:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-03/
cargo run
4
cargo run
5
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4

$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

You should get different random numbers, and they should all be numbers between
1 and 100. Great job!

## Comparing the Guess to the Secret Number

Now that we have user input and a random number, we can compare them. That step
is shown in Listing 2-4. Note that this code won’t compile just yet, as we will
explain.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-04/src/main.rs:here}}
```

<span class="caption">Listing 2-4: Handling the possible return values of
comparing two numbers</span>

First we add another `use` statement, bringing a type called
`std::cmp::Ordering` into scope from the standard library. The `Ordering` type
is another enum and has the variants `Less`, `Greater`, and `Equal`. These are
the three outcomes that are possible when you compare two values.

Then we add five new lines at the bottom that use the `Ordering` type. The
`cmp` method compares two values and can be called on anything that can be
compared. It takes a reference to whatever you want to compare with: here it’s
comparing `guess` to `secret_number`. Then it returns a variant of the
`Ordering` enum we brought into scope with the `use` statement. We use a
[`match`][match]<!-- ignore --> expression to decide what to do next based on
which variant of `Ordering` was returned from the call to `cmp` with the values
in `guess` and `secret_number`.

A `match` expression is made up of *arms*. An arm consists of a *pattern* to
match against, and the code that should be run if the value given to `match`
fits that arm’s pattern. Rust takes the value given to `match` and looks
through each arm’s pattern in turn. Patterns and the `match` construct are
powerful Rust features: they let you express a variety of situations your code
might encounter and they make sure you handle them all. These features will be
covered in detail in Chapter 6 and Chapter 18, respectively.

Let’s walk through an example with the `match` expression we use here. Say that
the user has guessed 50 and the randomly generated secret number this time is
38.

When the code compares 50 to 38, the `cmp` method will return
`Ordering::Greater` because 50 is greater than 38. The `match` expression gets
the `Ordering::Greater` value and starts checking each arm’s pattern. It looks
at the first arm’s pattern, `Ordering::Less`, and sees that the value
`Ordering::Greater` does not match `Ordering::Less`, so it ignores the code in
that arm and moves to the next arm. The next arm’s pattern is
`Ordering::Greater`, which *does* match `Ordering::Greater`! The associated
code in that arm will execute and print `Too big!` to the screen. The `match`
expression ends after the first successful match, so it won’t look at the last
arm in this scenario.

However, the code in Listing 2-4 won’t compile yet. Let’s try it:

<!--
The error numbers in this output should be that of the code **WITHOUT** the
anchor or snip comments
-->

```console
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-04/output.txt}}
```

The core of the error states that there are *mismatched types*. Rust has a
strong, static type system. However, it also has type inference. When we wrote
`let mut guess = String::new()`, Rust was able to infer that `guess` should be
a `String` and didn’t make us write the type. The `secret_number`, on the other
hand, is a number type. A few of Rust’s number types can have a value between 1
and 100: `i32`, a 32-bit number; `u32`, an unsigned 32-bit number; `i64`, a
64-bit number; as well as others. Unless otherwise specified, Rust defaults to
an `i32`, which is the type of `secret_number` unless you add type information
elsewhere that would cause Rust to infer a different numerical type. The reason
for the error is that Rust cannot compare a string and a number type.

Ultimately, we want to convert the `String` the program reads as input into a
real number type so we can compare it numerically to the secret number. We do
so by adding this line to the `main` function body:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/src/main.rs:here}}
```

The line is:

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

We create a variable named `guess`. But wait, doesn’t the program already have
a variable named `guess`? It does, but helpfully Rust allows us to shadow the
previous value of `guess` with a new one. *Shadowing* lets us reuse the `guess`
variable name rather than forcing us to create two unique variables, such as
`guess_str` and `guess`, for example. We’ll cover this in more detail in
[Chapter 3][shadowing]<!-- ignore -->, but for now, know that this feature is
often used when you want to convert a value from one type to another type.

We bind this new variable to the expression `guess.trim().parse()`. The `guess`
in the expression refers to the original `guess` variable that contained the
input as a string. The `trim` method on a `String` instance will eliminate any
whitespace at the beginning and end, which we must do to be able to compare the
string to the `u32`, which can only contain numerical data. The user must press
<span class="keystroke">enter</span> to satisfy `read_line` and input their
guess, which adds a newline character to the string. For example, if the user
types <span class="keystroke">5</span> and presses <span
class="keystroke">enter</span>, `guess` looks like this: `5\n`. The `\n`
represents “newline.” (On Windows, pressing <span
class="keystroke">enter</span> results in a carriage return and a newline,
`\r\n`.) The `trim` method eliminates `\n` or `\r\n`, resulting in just `5`.

The [`parse` method on strings][parse]<!-- ignore --> converts a string to
another type. Here, we use it to convert from a string to a number. We need to
tell Rust the exact number type we want by using `let guess: u32`. The colon
(`:`) after `guess` tells Rust we’ll annotate the variable’s type. Rust has a
few built-in number types; the `u32` seen here is an unsigned, 32-bit integer.
It’s a good default choice for a small positive number. You’ll learn about
other number types in [Chapter 3][integers]<!-- ignore -->.

Additionally, the `u32` annotation in this example program and the comparison
with `secret_number` means Rust will infer that `secret_number` should be a
`u32` as well. So now the comparison will be between two values of the same
type!

The `parse` method will only work on characters that can logically be converted
into numbers and so can easily cause errors. If, for example, the string
contained `A👍%`, there would be no way to convert that to a number. Because it
might fail, the `parse` method returns a `Result` type, much as the `read_line`
method does (discussed earlier in [“Handling Potential Failure with
`Result`”](#handling-potential-failure-with-result)<!-- ignore-->). We’ll treat
this `Result` the same way by using the `expect` method again. If `parse`
returns an `Err` `Result` variant because it couldn’t create a number from the
string, the `expect` call will crash the game and print the message we give it.
If `parse` can successfully convert the string to a number, it will return the
`Ok` variant of `Result`, and `expect` will return the number that we want from
the `Ok` value.

Let’s run the program now:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/
cargo run
  76
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

Nice! Even though spaces were added before the guess, the program still figured
out that the user guessed 76. Run the program a few times to verify the
different behavior with different kinds of input: guess the number correctly,
guess a number that is too high, and guess a number that is too low.

We have most of the game working now, but the user can make only one guess.
Let’s change that by adding a loop!

## Allowing Multiple Guesses with Looping

The `loop` keyword creates an infinite loop. We’ll add a loop to give users
more chances at guessing the number:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-04-looping/src/main.rs:here}}
```

As you can see, we’ve moved everything from the guess input prompt onward into
a loop. Be sure to indent the lines inside the loop another four spaces each
and run the program again. The program will now ask for another guess forever,
which actually introduces a new problem. It doesn’t seem like the user can quit!

The user could always interrupt the program by using the keyboard shortcut
<span class="keystroke">ctrl-c</span>. But there’s another way to escape this
insatiable monster, as mentioned in the `parse` discussion in [“Comparing the
Guess to the Secret Number”](#comparing-the-guess-to-the-secret-number)<!--
ignore -->: if the user enters a non-number answer, the program will crash. We
can take advantage of that to allow the user to quit, as shown here:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-04-looping/
cargo run
(too small guess)
(too big guess)
(correct guess)
quit
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/main.rs:28:47
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Typing `quit` will quit the game, but as you’ll notice, so will entering any
other non-number input. This is suboptimal, to say the least; we want the game
to also stop when the correct number is guessed.

### Quitting After a Correct Guess

Let’s program the game to quit when the user wins by adding a `break` statement:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-05-quitting/src/main.rs:here}}
```

Adding the `break` line after `You win!` makes the program exit the loop when
the user guesses the secret number correctly. Exiting the loop also means
exiting the program, because the loop is the last part of `main`.

### Handling Invalid Input

To further refine the game’s behavior, rather than crashing the program when
the user inputs a non-number, let’s make the game ignore a non-number so the
user can continue guessing. We can do that by altering the line where `guess`
is converted from a `String` to a `u32`, as shown in Listing 2-5.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-05/src/main.rs:here}}
```

<span class="caption">Listing 2-5: Ignoring a non-number guess and asking for
another guess instead of crashing the program</span>

We switch from an `expect` call to a `match` expression to move from crashing
on an error to handling the error. Remember that `parse` returns a `Result`
type and `Result` is an enum that has the variants `Ok` and `Err`. We’re using
a `match` expression here, as we did with the `Ordering` result of the `cmp`
method.

If `parse` is able to successfully turn the string into a number, it will
return an `Ok` value that contains the resultant number. That `Ok` value will
match the first arm’s pattern, and the `match` expression will just return the
`num` value that `parse` produced and put inside the `Ok` value. That number
will end up right where we want it in the new `guess` variable we’re creating.

If `parse` is *not* able to turn the string into a number, it will return an
`Err` value that contains more information about the error. The `Err` value
does not match the `Ok(num)` pattern in the first `match` arm, but it does
match the `Err(_)` pattern in the second arm. The underscore, `_`, is a
catchall value; in this example, we’re saying we want to match all `Err`
values, no matter what information they have inside them. So the program will
execute the second arm’s code, `continue`, which tells the program to go to the
next iteration of the `loop` and ask for another guess. So, effectively, the
program ignores all errors that `parse` might encounter!

Now everything in the program should work as expected. Let’s try it:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-05/
cargo run
(too small guess)
(too big guess)
foo
(correct guess)
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 4.45s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

Awesome! With one tiny final tweak, we will finish the guessing game. Recall
that the program is still printing the secret number. That worked well for
testing, but it ruins the game. Let’s delete the `println!` that outputs the
secret number. Listing 2-6 shows the final code.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-06/src/main.rs}}
```

<span class="caption">Listing 2-6: Complete guessing game code</span>

At this point, you’ve successfully built the guessing game. Congratulations!

## Summary

This project was a hands-on way to introduce you to many new Rust concepts:
`let`, `match`, functions, the use of external crates, and more. In the next
few chapters, you’ll learn about these concepts in more detail. Chapter 3
covers concepts that most programming languages have, such as variables, data
types, and functions, and shows how to use them in Rust. Chapter 4 explores
ownership, a feature that makes Rust different from other languages. Chapter 5
discusses structs and method syntax, and Chapter 6 explains how enums work.

[prelude]: ../std/prelude/index.html
[variables-and-mutability]: ch03-01-variables-and-mutability.html#variables-and-mutability
[comments]: ch03-04-comments.html
[string]: ../std/string/struct.String.html
[iostdin]: ../std/io/struct.Stdin.html
[read_line]: ../std/io/struct.Stdin.html#method.read_line
[result]: ../std/result/enum.Result.html
[enums]: ch06-00-enums.html
[expect]: ../std/result/enum.Result.html#method.expect
[recover]: ch09-02-recoverable-errors-with-result.html
[randcrate]: https://crates.io/crates/rand
[semver]: http://semver.org
[cratesio]: https://crates.io/
[doccargo]: http://doc.crates.io
[doccratesio]: http://doc.crates.io/crates-io.html
[match]: ch06-02-match.html
[shadowing]: ch03-01-variables-and-mutability.html#shadowing
[parse]: ../std/primitive.str.html#method.parse
[integers]: ch03-02-data-types.html#integer-types
