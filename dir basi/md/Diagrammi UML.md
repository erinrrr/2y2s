#### Classi e Oggetti
Nella fase di analisi ci si concentra sulle classi in quanto gli oggetti verranno creati modificati e distrutti durante l'esecuzione del programma.
Un oggetto in UML:
- ha vita propria
- è un'istanza di una classe (classe più specifica)
- è identificato univocamente mediante l'identificatore di oggetto
- viene indicato così: ![[dir basi/asset/file 1.png|300]]
	- `div_comm` è l'identificatore di oggetto
	- `Libro` è la classe più specifica di cui l'oggetto è istanza
	- si noti la <u>sottolineatura</u>, in quanto oggetto

Una classe invece:
- modella un insieme di oggetti omogenei (le istanze di una classe)
- descritta da:
	- un nome
	- un insieme di proprietà:
		- statiche: attributi
		- dinamiche: metodi
- ![[dir basi/asset/file 2.png]]
- il livello superiore è detto delle classi, intensionale ed è quello più astratto
- mentre quello inferiore è il possibile livello degli oggetti, estensionale

- un oggetto è identificato da un identificatore univoco
- un diagramma delle classi in generale permette la coesistenza di oggetti identici
- il simbolo `<< >>` indica uno stereotipo

#### Link e Associazioni
- un'associazione modella la _possibilità_ che oggetti di due o più oggetti abbiano dei legami (inizia per minuscola e segue lo snake_case)
- le istanze delle associazioni si chiamano link
	- quindi se $A$ è un’associazione tra le classi $C_{1}$ e $C_{2}$, una istanza di $A$ è un link tra due oggetti, uno di $C_{1}$ e l’altro di $C_{2}$
	- ![[dir basi/asset/file 3.png]]
	- i link non hanno degli identificatori espliciti quindi vengono identificati implicitamente dalla coppia di oggetti, non si possono quindi avere due link uguali

_esempio_:
abbiamo un applicazione che permette ai clienti di prenotare hotel via web
![[dir basi/asset/file 4.png]]
così facendo Alice non potrebbe prenotare una seconda volta presso h1 perché avremmo due link uguali e quindi indistinguibili, per risolvere creiamo una nuova classe prenotazione in modo da dare alla prenotazione "vita propria"
![[dir basi/asset/file 5.png]]

- Tra le diverse classi possono essere definite più associazioni che modellano legami di natura diversa:![[dir basi/asset/file 6.png]]


#### Vincoli di molteplicità
UML permette di definire vincoli di integrità in un diagramma delle classi ossia ulteriori restrizioni sui livelli estensionali ammessi

I vincoli di molteplicità:![[dir basi/asset/file 7.png]]
In questo modo stiamo dicendo che:
- "un qualsiasi oggetto della classe Impiegato può essere associato soltanto ad un oggetto di Città" (non si può nascere in due città)

- `1..1` - "ogni istanza di impiegato deve essere coinvolta in un numero di link dell’associazioni nascita che va da 1 a 1", equivalente a "un’istanza di Impiegato può essere associata ad un’istanza di città solo una volta tramite l’associazione nascita"
- `0..*` - "ogni istanza di Città deve essere coinvolta in un numero di link dell’associazione nascita che da 0 all’infinito", equivalente a "un’istanza di città può essere associata ad un qualunque numero di istanze di Impiegato tramite nascita"

###### Associazioni che insistono più volte sulla stessa classe
Supponiamo di voler modellare i sovrani di un regno, di ogni sovrano ci interessa:
- nome
- periodo in cui ha regnato
- predecessore
![[dir basi/asset/file 8.png]]
sappiamo che un link sono coppie di oggetti Sovrano quindi
- siamo obbligati a dare i _nomi di ruolo_ per etichettare la coppia affinché non ci sia ambiguità, la coppia ($s_{1},s_{2}$) diventa (predecessore: $s_{1}$, successore: $s_{2}$)
- "ogni oggetto Sovrano partecipa alla relazione successione con al massimo un predecessore e al massimo un successore" (con cardinalità che va da $0$ a $1$)


#### Associazione con attributi
Vogliamo progettare un sistema per gestire gli esiti dei test superati dagli studenti di un corso, che è diviso in moduli e uno studente può superare ogni modulo al più una volta; il voto non è né una proprietà di Studente né di Modulo, è una _proprietà dell'associazione_
- possiamo aggiungere degli attributi all'associazione tra Studente e Modulo ![[dir basi/asset/file 9.png]]
- in questo modo lo stesso studente può partecipare più volte alla relazione `test_superato` (per diversi esami) e possiamo assegnare ad ogni link un voto ed una data diversi

- aggiungere attributi alla relazione non ci permette comunque di avere più link fra gli stessi oggetti, (es: stesso studente partecipare a 2 link con stesso modulo e attributi diversi nel link)
- in altre parole: stessa coppia di oggetti può formare al più un link dell'associazione

Un’associazione di questo tipo è anche chiamata **association class** dato che a sua volta può essere collegata con altre associazioni, e come prima non ci permette comunque di avere due o più link uguali anche se con attributi o relazioni associate diversi
- esempio:![[dir basi/asset/file 10.png]]con questa struttura, non sarebbe ammesso avere due record distinti dello stesso `studente` che ha superato lo stesso `modulo` con `voti`, `date` e `docenti` diversi


###### Tipi di dato concettuali
Durante l'analisi non vogliamo effettuare scelte tecnologiche ma dobbiamo definire il tipo di ogni attributo, di classe e di associazione:
- usiamo dei _tipi concettuali_ che siano realizzabili con qualsiasi tecnologia
- _tipi base_, Stringa, Intero, Reale, Booleano, Data, Ora$\dots$
- _tipi specializzati_: con un range di valori ammessi, `Intero > 0`, `18..30`$\dots$
- _tipi enumerativi_: che definiscono esplicitamente e completamente l'insieme di valori possibili per l'attributo, `{maschio, femmina}`, sono etichette non stringhe
- _nuovi tipi_ di dato: `Tipo Genere = {maschio, femmina}` (dominio enumerativo)
- _tipi record_: tipi di dato composti da più campi, `Indirizzo = (via:Stringa, civico:Intero>0, cap:Intero>0)` (potremmo trovare dei composti anche al loro interno): ![[dir basi/asset/file 11.png|300]]

- **vincoli di molteplicità degli attributi**: anche gli attributi di classe e associazione possono avere vincoli di molteplicità, di default sono `1..1`
	- ad esempio uno studente può avere più email:![[dir basi/asset/file 12.png|300]]


#### Vincoli di identificazione di classe
Il **vincolo di identificazione di classe** impone che non possono coesistere oggetti di una classe che abbiano stesso valore su determinati attributi scelti, o sono legati tramite link agli stessi oggetti di altre classi
- ogni attributo può partecipare a più gruppi identificativi
- un vincolo di identificazione di classe può coinvolgere solo attributi e/o ruoli della classe a molteplicità `1..1`

ove non volessimo avere due o più istanze di `Persona` che hanno simultaneamente uguali il campo `cf` e neanche gli attributi `nome`,`cognome`,`nascita` (considerato come gruppo, quindi contemporaneamente)![[dir basi/asset/file 14.png|300]]

- un vincolo di identificazione di classe può coinvolgere anche i _ruoli_ della classe:![[dir basi/asset/file 15.png|400]]in questo caso possono esistere due o più studenti con la stessa matricola ma non all'interno della stessa università

#### Relazione is-a tra classi
Molte volte è necessario rappresentare il fatto che due classi abbiano una relazione di _sottoinsieme_ e possiamo farlo con il concetto di **relazione is-a** tra classi:
- "è anche un", ogni istanza della classe Studente (sotto-classe) è anche un istanza della classe Persona
- non vale il viceversa, non tutte le istanze di Persona devono per forza essere anche istanze di Studente
![[dir basi/asset/file 17.png|350]]
In presenza di relazioni is-a vige il meccanismo dell'**ereditarietà**:
- avremo che ogni Studente ha anche i campi per gli attributi `nome`,`genere` oltre che a `matricola`
- possiamo avere anche la relazione is-a a più livelli, concetto di transitività ![[dir basi/asset/file 18.png|450]]
tutti gli StudentiStraniero sono <em>anche</em> Studenti e quindi sono <em>anche</em> Persone, quindi di ogni StudenteStraniero stiamo rappresentando: `nome`, `genere`, `matricola`stiamo anche rappresentando le relazione delle varie sottoclassi, ad ogni Persona è associata una `città di nascita`, quindi anche Studente e StudenteStraniero parteciperanno ad un link `nascita`, poi abbiamo Studente che può partecipare a `tutor stud`, e ove vi partecipasse, stessa cosa farà StudenteStraniero ma non Persona

#### Generalizzazioni e Classi più specifiche di un oggetto