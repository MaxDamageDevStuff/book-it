## Installazione

Il primo passo è l'installazione di Rust. Scaricheremo Rust tramite  `rustup`, uno strumento a linea di comando per la gestione delle versioni di Rust e dei tool associati. Avrai bisogno di una connessione ad internet per il download.

> Nota: se preferisci non usare `rustup` per qualche motivo, ti preghiamo di visitare
> [Altri Metodi di Installazione di Rust][otherinstall] per opzioni alternative.

I passaggi successivi ti guideranno nell'installazione dell'ultima versione stabile del compilatore di Rust.
La garanzia di stabilità di Rust assicura che tutti gli esempi del libro che compilano, potranno continuare ad essere compilati con le versioni di Rust più aggiornate. L'output potrebbe differire leggermete tra le versioni perché Rust subisce miglioramenti ai messaggi di warning e di errore. In altre parole, qualunque versione stabile più aggiornata di Rust installerai usando questi passaggi dovrebbe funzionare come descritto nel libro.

> ### Notazione della Linea di Comando
> 
> In questo capitolo e per tutto il libro, mostreremo alcuni comandi da usare nel terminale. I comandi che dovresti inserire nel terminale cominceranno tutti con`$`. 
> Non dovrai inserire il carattere `$` ; è il prompt della linea di comando mostrato per indicare l' inizio di ogni comando. Le righe che non iniziano con  `$` tipicamente mostrano l'output del comando precedente. In aggiunta a ciò, gli esempi specifici di PowerShell useranno `>` piuttosto che `$`.

### Installare `rustup` su Linux o macOS

Se stai usando Linux o macOS, apri un terminale ed inserisci il seguente comando:

```console
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

Il comando scarica un script ed inizia l'installazione di `rustup`, che a sua volta installerà l'ultima versione stabile di Rust disponibile. Potrebbe esserti richiesto l'inserimento della password. Se l'installazione sarà avvenuta con successo, apparirà la seguente riga:

```text
Rust is installed now. Great!
```

Avrai bisogno anche di un *linker*, ovvero un programma che Rust usa per unire tutti gli output compilati in un singolo file. È probabile che tu ne abbia già uno. Se dovessi ricevere errori del linker, dovrai installare un compilatore C, Che tipicamente contiene un linker. Un compilatore C è utile anche perché alcuni pacchetti comuni di Rust dipendono su del codice C, per il quale avrai bisogno di un compilatore C.

Su macOS, puoi ottenere un compilatore C eseguendo:

```console
$ xcode-select --install
```

Gli utenti Linux generalmente dovrebbero installare GCC o Clang, come indicato dalla documentazione della loro distribuzione. Per esempio, se stai usando Ubuntu, puoi installare il pacchetto `build-essential` .

### Installare `rustup` su Windows

Su Windows, vai su [https://www.rust-lang.org/tools/install][install] e segui le istruzioni per installare Rust. Ad un certo punto nell'installazione, vedrai apparire un messaggio che spiega che avrai bisogno anche dei tool di build MSVC per
Visual Studio 2013 o più recente.

Per acquisire suddetti tool, dovrai installare [Visual Studio
2022][visualstudio]. Quando ti verrà richiesto quali workloads (carichi di lavoro) installare, dovrai selezionare:

* “Sviluppo Desktop C++”
* Windows 10 o 11 SDK
* Il componente English language pack, insieme a qualsiasi altro language pack di tua scelta

Il resto di questo libro usa comandi che funzionano sia in *cmd.exe* che in PowerShell.
Qualora dovessero esserci differenze specifiche differenze, spiegheremo quale usare.

### Troubleshooting

Per controllare se hai installato Rust correttamente, apri una shell ed inserisci questa riga:

```console
$ rustc --version
```

Dovresti vedere il numero di versione, il commit hash, e la data di commit dall'ultima versione stabile che è stata rilasciata, nel seguente formato:

```text
rustc x.y.z (abcabcabc yyyy-mm-dd)
```

Se vedi queste informazioni, hai installato Rust con successo! se non vedi quest'informazione, controlla che Rust sia nella tua variabile di sistema `%PATH%` come segue.

Nel CMD di Windows, usa:

```console
> echo %PATH%
```

In PowerShell, usa:

```powershell
> echo $env:Path
```

In Linux e macOS, usa:

```console
$ echo $PATH
```

Se è tutto corretto ma Rust continua a non funzionare, ci sono diversi posti dove puoi chiedere aiuto. Scopri come metterti in contatto con altri Rustaceans (uno sciocco nickname con il quale ci chiamiamo) sulla [pagina della community][community].

### Aggiornare e Disinstallare

Una volta che avrai installato Rust via `rustup`, aggiornandolo alla versione rilasciata più di recente è semplice. Dalla tua shell, esegui l'update script successivo:

```console
$ rustup update
```

Per disinstallare Rust e `rustup`, esegui il seguente uninstall script dalla tua
shell:

```console
$ rustup self uninstall
```

### Documentazione Locale

L'installazione di Rust include anche una copia locale della documentazione, in modo tale che tu possa leggerla offline. Esegui `rustup doc` per aprire la documentazione locale nel tuo browser.

Ogni volta che non dovessi essere sicuro di cosa un tipo o una funzione fornita dalla standard library faccia o di come si usa, usa la documentazione dell'application programming interface(API) per scoprirlo!

[otherinstall]: https://forge.rust-lang.org/infra/other-installation-methods.html
[install]: https://www.rust-lang.org/tools/install
[visualstudio]: https://visualstudio.microsoft.com/downloads/
[community]: https://www.rust-lang.org/community
