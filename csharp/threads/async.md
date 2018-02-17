# Threading e programmazione asincrona

## Introduzione
È possibile evitare colli di bottiglia nelle prestazioni e migliorare la risposta della UI oppure ottimizzando il lavoro generale di calcolo dell'applicazione utilizzando la programmazione asincrona eseguendo delle operazioni in cosiddetti thread paralleli.  

Tuttavia, le tecniche tradizionali per la creazione di applicazioni asincrone possono essere complesse, pertanto può essere difficile scrivere, fare il debug e gestire queste applicazioni.

Già nelle prime versioni di .NET e Visual Studio 2005 esistevano modelli di thread asincroni, appoggiandosi molto sulla tecnica di C.

L'approccio non era molti triviale e cambiare codice sincrono in asincrono era un lavoro oneroso. Da partire di Visual Studio 2012 con il relativo .NET Framework 4.5 è stato introdotto un approccio semplificato. Il compilatore esegue il lavoro complesso che lo sviluppatore era abituato a fare, e l'applicazione mantiene una struttura logica simile al codice sincrono. Di conseguenza, si ottengono tutti i vantaggi della programmazione asincrona con uno sforzo minimo.

In questa soluzione mostro i diversi modelli, sintassi e possibili falle da tenere conto.

Nella soluzione si trovano gli esempi dei tre pattern che esistono e diversi esempi approfonditi con il pattern del giorno d'oggi (TAP):

Il design dei tre pattern:
- PatternAPM, seguendo il modello sviluppato nelle origini.
- PatternEAP, seguendo il modello tradizionale.
- PatternTAP seguendo il modello nuovo.

Esempi approfonditi del pattern TAP (Async e Await)
- BasicExample, Esempio che visualizza i therad.
- WPFAppExample, Esempio con WPF Gui.

## L'evoluzione del multithreading nei anni.
Programmare codice asincrono era già possibile neri primi anno dell' .NET Framework usando un approccio molto simile al linguaggio "C".
Nella prima revisione del Framework è stato introdotto il concetto di eventi e delegati sfruttando in tale maniera la possibilità di eseguire del codice parallelo.
Nella terza revisione, a partire del .NET 4.5 seguendo il trend della semplificazione (*rivedere il testo su video) è stato elaborato un concetto con l'obiettivo di trasformare del codice sincrono in asincrono ottenendolo facilmente raggiungibile implementando una tecnica chiamandosi Task paralleli.

## Tutorials in questa soluzione
Nei successivi esempi sono mostrati i tre modelli.

### Il Pattern APM in .NET 1
Asynchronus Programming Model:

- Il Pattern APM

  Utilizzato nel .Net Framework 1.0. Ancora poco utilizzato però utile a conoscerlo nel caso devi aggiornare del codice creato inizialmente con il framework in questione.

### Il Pattern EAP in .NET 2
Event-based Asynchronous Pattern:

- Pattern EAP

   Sicuramente ancora molto utilizzato e lo trovi per la maggior parte nei progetti creato con framework 2 fino al 4. Programmare con event and delegates è un uso comune di programmare ancora al giorno d'oggi.

### Il Pattern TAP in .NET 4.5

Task-based Asynchronous Pattern:

- Il Pattern TAP

  Il pattern che è stato introdotto nel .net Framework 4.5 ed sicuramente il futuro della programmazione asincrona, è molto facile da implementare visto che non si deve fare grosse modifiche ad un codice sincrono esistente per farlo diventare asincrono.

## Programmare elegantemente con Async e Await

Ma come il titolo del tutorial dice, noi andremo ad approfondire il modello TAP con Async e Await.
Con Async e Await si possono programmare in modo molto elegantemente operazioni di calcolo ma purtroppo è anche molto facile a perdere di vista le risorse nei vari Threads.
Un piccolo filmato su [video2brain](https://www.video2brain.com/de/tutorial/elegant-mit-async-und-await-codieren) spiega bene la casistica.

Nella soluzione trovi e seguenti esempi che spiegherò nelle pagine successive:
- BasicExample
- WpfAppExample
	
## Fonti
https://msdn.microsoft.com/it-it/library/hh191443%28v=vs.120%29.aspx?f=255&MSPPError=-2147217396
https://www.video2brain.com/de/tutorial/elegant-mit-async-und-await-codieren
https://www.video2brain.com/de/tutorial/asynchrone-programmierung-in-net
https://docs.microsoft.com/it-it/windows/uwp/threading-async/
https://docs.microsoft.com/it-it/dotnet/csharp/async
http://www.hinzberg.net/csharp/csharp/csharp/threadevents.html
Libri C#