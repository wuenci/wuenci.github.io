Espressioni regolari (Regex)
============================

La sintassi
-------------
Un’espressione regolare `e un’espressione costruita secondo una sintassi predeﬁnita chiamato "pattern"  che permette di descrivere un insieme di stringhe.
Anche se il linguaggio delle espressioni regolari è indipendente dai linguaggi di programmazione (le regex sono uguali in Java,C#,C++ ecc.) non sempre il motore permette di valutare tutte le espressioni.

## La sintassi semplice
Un esempio in Javascript:  
`new RegExp("[abc]")`  
o semplicemente:  
`/[abc]/`

## La sintassi con modificatori
`new RegExp("[abc]", "i")`  
o semplicemente:  
`/\[abc]/i`

## Caratteri comuni
Note! Sono espressi solo i più comuni

| Pattern         |  Descrizione
|:---------------:|-----------------------------------------------------------------------------------
| /abc/           |  Cerca abc
| \[abc\]         |  Cerca a oppure b oppure c
| \[a-z\]         |   Cerca dà a fino z
| \[a-zA-Z\]      |  Cerca dà A fino a Z e dà a fino a z
| \[\^a-z\]       |  \^= negazione, Non cercare dà a fino a z, cioè trova risultato da A fino a Z
| \\w             |  Sta per Word e cerca qualsiasi parola Alfanumerica e underscore, è sinonimo di \[a-zA-Z\_0-9\]
| \\W             |  è la negazione di Word, tutto tranne Word
| \\s             |  Sta per spazi e tabulatori (whitespaces)
| \\S             |  negazione di \\s, nessun whitespace
| \\d             |  Cerca qualsiasi cifra, sinonimo di \[0-9\]
| \\D             |  Negazione di \\d. Cerca tutto quello che non è una cifra.
| \[a-zA-Z\_0-9\] |  Trova tutti maiuscoli, minuscoli, underscore e cifre
| .               |  Cerca tutto


## Caratteri speciali
Note! Non funziona in tutt i motori di espressioni regulari

| Pattern     | Descrizione
|:-----------:|-----------------------------------------------------------------------------------------------------------
| \\b         | Cerca un carattere vuoto all\'inizio oppure alla fine della riga
| \\B         | Cerca un carattere vuoto all\'interno della riga
| \\n         | Linebreak in formato Unix
| \\r         | Linebreak in formato Mac
| \\r\\n      | Linebreak in formato Windows
| \[\^\]      | Se è scritto all\'inizio è la negazione, altrimenti sta per l\'inizio della riga
| \[abc\]\^   | Sta per l\'inizio della riga
| \$          | Dipende dal contesto può essere come fine della riga oppure della stringa
| \[\^\]\$    | Questo vuol dire che la stringa deve avere minimo un carattere e alla fine non può essere una whitespace
| \\          | Mascherare o "escapare" un carattere chiave Esempio: `\.` Cerca solo il punto.


## Quantificatore
Note! Quantificare le volte che un carattere o cifra può presentarsi nella stringa.

| Pattern   | Descrizione
|:---------:|-------------------------------------------------------------
| ?         |  Ripetuto una o zero volte
| \*        |  Ripetuto zero o più volte
| \+        |  Ripetuto almeno una o più volte
| {2}       |  Ripetuto n volte (in questo esempio 2)
| {2,4}     |  Da n a m volte (In questo esempio minimo 2 massimo 4 volte)
| {5,\]     |  Ripetute almeno n volte fino a infinito
| \\{d3}    |  Tre posti decimali
| \\{d1,5}  |  Da uno a cinque posti decimali

## Alternative nella ricerca
Note! La pipe significa "oppure"

| Pattern | Descrizione
|:-------:|-------------
| &#124;  | Oppure (Or)

 ## Esempi di espressioni regolari
 Note! Alcuni esempi sono stati copiati da internet

 Veriﬁca nella riga la presenza di due caratteri qualsiasi seguiti dalla stringa ca:  
 `/..ca/`

 Validare un indirizzo E-Mail:  
`/[a-zA-Z_0-9?\.]{1,}@[a-zA-Z_0-9?\.]{1,}\.[a-zA-Z_0-9]{1,}/`

 Veriﬁca nella riga la presenza del formato temporale hh:mm:ss  
 `/\d\d:\d\d:\d\d/`

 Veriﬁca nella riga la presenza di una cifra esadecimale
 `/[0-9a-fA-F]/`

## Regex in CSharp
Un breve esempio come usare le espressioni regolari in C#
```
    static void Main(string[] args)
    {
        Regex regex = new Regex(@"abc", RegexOptions.IgnoreCase);

        Match match = regex.Match("abc");
        
        if(match.Success)
        {
            Console.WriteLine("Matching");
        }
    }
```

##Source:
Github: https://github.com/wuenci/RegExDemo
VSTS: https://nc-training.visualstudio.com/RegExTutorial