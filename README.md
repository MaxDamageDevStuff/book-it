# The Rust Programming Language

![Build Status](https://github.com/rust-lang/book/workflows/CI/badge.svg)

This repo contains the source of the italian version of "The Rust Programming Language" book. If you wish the repo of the original version of it (English), please check this [link][ original ]:

[ original ]: https://github.com/rust-lang/book

Questo repository contiene i sorgenti del libro "The Rust Programming Language" tradotto in italiano. La traduzione è ancora estremamente incompleta e poco rifinita, per tanto se ne sconsiglia l'utilizzo per un serio apprendimento. 

Molti link presenti in questo README rimandano direttamente alle versioni originali (in inglese) degli stessi. 

Ogni contribuzione è gradita : )

[Questo libro è disponibile anche in forma dead-tree (stampata) su No Starch Press (solo in inglese)][nostarch].

[nostarch]: https://nostarch.com/rust

Puoi leggere gratuitamente il libro online. Ti preghiamo di usare il libro  dell'ultima versione [stable], [beta], o [nightly] di Rust rilasciata. Tieni presente che i problemi
presenti in queste versioni versioni potrebbero essere già stati corretti in questo repository, visto che sono versioni aggiornate con una frequenza minore.

[stable]: https://doc.rust-lang.org/stable/book/
[beta]: https://doc.rust-lang.org/beta/book/
[nightly]: https://doc.rust-lang.org/nightly/book/

Visita [releases] per scaricare solo il codice di tutti gli esempi che appaiono nel libro.

[releases]: https://github.com/rust-lang/book/releases

## Requisiti

Buildare il libro richiede [mdBook], preferibilmente usando la stessa versione utilizzata in [questo file][rust-mdbook] di rust-lang/rust . Per ottenerlo usa il seguente comando:

[mdBook]: https://github.com/rust-lang-nursery/mdBook
[rust-mdbook]: https://github.com/rust-lang/rust/blob/master/src/tools/rustbook/Cargo.toml

```bash
$ cargo install mdbook --version <version_num>
```

## Building

per buildare il libro, digita:

```bash
$ mdbook build
```

L'output sarà nella sottocartella `book` . Per dargli un'occhiata, aprilo nel tuo web browser.

_Firefox:_

```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_

```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

Per eseguire i test:

```bash
$ mdbook test
```

## Contributing

We'd love your help! Please see [CONTRIBUTING.md][contrib] to learn about the
kinds of contributions we're looking for.

[contrib]: https://github.com/rust-lang/book/blob/main/CONTRIBUTING.md

Because the book is [printed](https://nostarch.com/rust), and because we want
to keep the online version of the book close to the print version when
possible, it may take longer than you're used to for us to address your issue
or pull request.

So far, we've been doing a larger revision to coincide with [Rust
Editions](https://doc.rust-lang.org/edition-guide/). Between those larger
revisions, we will only be correcting errors. If your issue or pull request
isn't strictly fixing an error, it might sit until the next time that we're
working on a large revision: expect on the order of months or years. Thank you
for your patience!

### Translations

We'd love help translating the book! See the [Translations] label to join in
efforts that are currently in progress. Open a new issue to start working on
a new language! We're waiting on [mdbook support] for multiple languages
before we merge any in, but feel free to start!

[Translations]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations
[mdbook support]: https://github.com/rust-lang-nursery/mdBook/issues/5

## Spellchecking

To scan source files for spelling errors, you can use the `spellcheck.sh`
script available in the `ci` directory. It needs a dictionary of valid words,
which is provided in `ci/dictionary.txt`. If the script produces a false
positive (say, you used word `BTreeMap` which the script considers invalid),
you need to add this word to `ci/dictionary.txt` (keep the sorted order for
consistency).
