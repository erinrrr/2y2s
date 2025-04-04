#### Classi e Oggetti
Nella fase di analisi ci si concentra sulle classi in quanto gli oggetti verranno creati modificati e distrutti durante l'esecuzione del programma.
Un oggetto in UML:
- ha vita propria
- è un'istanza di una classe (classe più specifica)
- è identificato univocamente mediante l'identificatore di oggetto
- viene indicato così: ![[dir/dir basi/asset/file 1.png|300]]
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
 ![[dir/dir basi/asset/file 2.png|dir basi/asset/file 2.png]]
- il livello superiore è detto delle classi, intensionale ed è quello più astratto
- mentre quello inferiore è il possibile livello degli oggetti, estensionale
<br>
- un oggetto è identificato da un identificatore univoco
- un diagramma delle classi in generale permette la coesistenza di oggetti identici
- il simbolo `<< >>` indica uno stereotipo

### Link e Associazioni
- un'associazione modella la _possibilità_ che oggetti di due o più oggetti abbiano dei legami (inizia per minuscola e segue lo snake_case)
- le istanze delle associazioni si chiamano link
	- quindi se $A$ è un’associazione tra le classi $C_{1}$ e $C_{2}$, una istanza di $A$ è un link tra due oggetti, uno di $C_{1}$ e l’altro di $C_{2}$
	- ![[dir/dir basi/asset/file 3.png|dir basi/asset/file 3.png]]
	- i link non hanno degli identificatori espliciti quindi vengono identificati implicitamente dalla coppia di oggetti, non si possono quindi avere due link uguali

_esempio_:
abbiamo un applicazione che permette ai clienti di prenotare hotel via web
![[dir/dir basi/asset/file 4.png|dir basi/asset/file 4.png]]
così facendo Alice non potrebbe prenotare una seconda volta presso h1 perché avremmo due link uguali e quindi indistinguibili, per risolvere creiamo una nuova classe prenotazione in modo da dare alla prenotazione "vita propria"
![[dir/dir basi/asset/file 5.png|dir basi/asset/file 5.png]]

- Tra le diverse classi possono essere definite più associazioni che modellano legami di natura diversa:![[dir/dir basi/asset/file 6.png|dir basi/asset/file 6.png]]


#### Vincoli di molteplicità
UML permette di definire vincoli di integrità in un diagramma delle classi ossia ulteriori restrizioni sui livelli estensionali ammessi

I vincoli di molteplicità:![[dir/dir basi/asset/file 7.png|dir basi/asset/file 7.png]]
In questo modo stiamo dicendo che:
- "un qualsiasi oggetto della classe Impiegato può essere associato soltanto ad un oggetto di Città" (non si può nascere in due città)
<br>
- `1..1` - "ogni istanza di impiegato deve essere coinvolta in un numero di link dell’associazioni nascita che va da 1 a 1", equivalente a "un’istanza di Impiegato può essere associata ad un’istanza di città solo una volta tramite l’associazione nascita"
- `0..*` - "ogni istanza di Città deve essere coinvolta in un numero di link dell’associazione nascita che da 0 all’infinito", equivalente a "un’istanza di città può essere associata ad un qualunque numero di istanze di Impiegato tramite nascita"

###### Associazioni che insistono più volte sulla stessa classe
Supponiamo di voler modellare i sovrani di un regno, di ogni sovrano ci interessa:
- nome
- periodo in cui ha regnato
- predecessore
![[dir/dir basi/asset/file 8.png]]
sappiamo che un link sono coppie di oggetti Sovrano quindi
- siamo obbligati a dare i _nomi di ruolo_ per etichettare la coppia affinché non ci sia ambiguità, la coppia ($s_{1},s_{2}$) diventa (predecessore: $s_{1}$, successore: $s_{2}$)
- "ogni oggetto Sovrano partecipa alla relazione successione con al massimo un predecessore e al massimo un successore" (con cardinalità che va da $0$ a $1$)


#### Associazione con attributi
Vogliamo progettare un sistema per gestire gli esiti dei test superati dagli studenti di un corso, che è diviso in moduli e uno studente può superare ogni modulo al più una volta; il voto non è né una proprietà di Studente né di Modulo, è una _proprietà dell'associazione_
- possiamo aggiungere degli attributi all'associazione tra Studente e Modulo ![[dir/dir basi/asset/file 9.png|dir basi/asset/file 9.png]]
- in questo modo lo stesso studente può partecipare più volte alla relazione `test_superato` (per diversi esami) e possiamo assegnare ad ogni link un voto ed una data diversi
<br>
- aggiungere attributi alla relazione non ci permette comunque di avere più link fra gli stessi oggetti, (es: stesso studente partecipare a 2 link con stesso modulo e attributi diversi nel link)
- in altre parole: stessa coppia di oggetti può formare al più un link dell'associazione

Un’associazione di questo tipo è anche chiamata **association class** dato che a sua volta può essere collegata con altre associazioni, e come prima non ci permette comunque di avere due o più link uguali anche se con attributi o relazioni associate diversi
- esempio:![[dir/dir basi/asset/file 10.png|dir basi/asset/file 10.png]]con questa struttura, non sarebbe ammesso avere due record distinti dello stesso `studente` che ha superato lo stesso `modulo` con `voti`, `date` e `docenti` diversi


#### Tipi di dato concettuali
Durante l'analisi non vogliamo effettuare scelte tecnologiche ma dobbiamo definire il tipo di ogni attributo, di classe e di associazione:
- usiamo dei _tipi concettuali_ che siano realizzabili con qualsiasi tecnologia
- _tipi base_, Stringa, Intero, Reale, Booleano, Data, Ora$\dots$
- _tipi specializzati_: con un range di valori ammessi, `Intero > 0`, `18..30`$\dots$
- _tipi enumerativi_: che definiscono esplicitamente e completamente l'insieme di valori possibili per l'attributo, `{maschio, femmina}`, sono etichette non stringhe
- _nuovi tipi_ di dato: `Tipo Genere = {maschio, femmina}` (dominio enumerativo)
- _tipi record_: tipi di dato composti da più campi, `Indirizzo = (via:Stringa, civico:Intero>0, cap:Intero>0)` (potremmo trovare dei composti anche al loro interno): ![[dir/dir basi/asset/file 11.png|300]]

- **vincoli di molteplicità degli attributi**: anche gli attributi di classe e associazione possono avere vincoli di molteplicità, di default sono `1..1`
	- ad esempio uno studente può avere più email:![[dir/dir basi/asset/file 12.png|300]]


#### Vincoli di identificazione di classe
Il **vincolo di identificazione di classe** impone che non possono coesistere oggetti di una classe che abbiano stesso valore su determinati attributi scelti, o sono legati tramite link agli stessi oggetti di altre classi
- ogni attributo può partecipare a più gruppi identificativi
- un vincolo di identificazione di classe può coinvolgere solo attributi e/o ruoli della classe a molteplicità `1..1`

ove non volessimo avere due o più istanze di `Persona` che hanno simultaneamente uguali il campo `cf` e neanche gli attributi `nome`,`cognome`,`nascita` (considerato come gruppo, quindi contemporaneamente)![[dir/dir basi/asset/file 14.png|300]]

- un vincolo di identificazione di classe può coinvolgere anche i _ruoli_ della classe:![[dir/dir basi/asset/file 15.png|400]]in questo caso possono esistere due o più studenti con la stessa matricola ma non all'interno della stessa università

#### Relazione is-a tra classi
Molte volte è necessario rappresentare il fatto che due classi abbiano una relazione di _sottoinsieme_ e possiamo farlo con il concetto di **relazione is-a** tra classi:
- "è anche un", ogni istanza della classe Studente (sotto-classe) è anche un istanza della classe Persona
- non vale il viceversa, non tutte le istanze di Persona devono per forza essere anche istanze di Studente
![[dir/dir basi/asset/file 17.png|350]]
In presenza di relazioni is-a vige il meccanismo dell'**ereditarietà**:
- avremo che ogni Studente ha anche i campi per gli attributi `nome`,`genere` oltre che a `matricola`
- possiamo avere anche la relazione is-a a più livelli, concetto di transitività ![[dir/dir basi/asset/file 18.png|450]]
tutti gli StudentiStraniero sono <em>anche</em> Studenti e quindi sono <em>anche</em> Persone, quindi di ogni StudenteStraniero stiamo rappresentando: `nome`, `genere`, `matricola`stiamo anche rappresentando le relazione delle varie sottoclassi, ad ogni Persona è associata una `città di nascita`, quindi anche Studente e StudenteStraniero parteciperanno ad un link `nascita`, poi abbiamo Studente che può partecipare a `tutor stud`, e ove vi partecipasse, stessa cosa farà StudenteStraniero ma non Persona

#### Generalizzazioni e Classi più specifiche di un oggetto
- le **classi più specifiche** sono le classi di cui l'oggetto è istanza che sono sono a loro volta super classi di altre classi dell'oggetto
- un oggetto può essere istanza di più classi
- una classe non può essere sottoclasse di più classi
- in questo caso l'oggetto `anna` è istanza di `Persona`, `Studente` e `Lavoratore`, l'insieme delle classi più specifiche sono `Studente`, `Lavoratore` in quanto non sono superclassi di altre di cui `anna` è istanza![[dir/dir basi/asset/file 19.png|450]]
- il costrutto della **generalizzazione** è un costrutto più complesso della relazione is-a
- permette di definire che le istanze di una classe possono essere istanze di più classi figlie secondo uno stesso criterio concettuale ![[dir/dir basi/asset/file 20.png|400]] - otteniamo:
	- `Studente` is-a `Persona`
	- `Lavoratore` is-a `Persona`
	- il concetto dell'`occupazione` è quello secondo cui una `Persona` è uno `Studente` e/o un `Lavoratore`
	- quindi un oggetto in questo modo potrebbe essere sia `Studente` che `Lavoratore`
- la stessa classe può essere superclassi di generalizzazioni distinte, quindi più di un criterio per ogni classe![[dir/dir basi/asset/file 21.png|400]]
	- secondo il criterio del genere le `Persone` possono essere `Uomo` e/o `Donna`
	- secondo il criterio dell’occupazione(indipendente) le `Persone` possono essere `Studente` e/o `Lavoratore`
	- dunque un'istanza di `Persona` può essere:
		- sia `Uomo` che `Studente`
		- sia `Uomo` che `Donna`
		- né `Uomo` né `Donna`

**Vincoli sulle generalizzazioni**:
- _disjoint_: l'insieme delle istanze è disgiunto, quindi un oggetto può essere istanza soltanto di una classe fra quelle della generalizzazione
	- resta il fatto che possa essere nessuna delle due
- *complete*: l'intersezione delle istanze è uguale all'insieme totale delle istanze, un oggetto sarò istanza di almeno una classe fra quelle della generalizzazione
	- non limitiamo il caso in cui sia istanza di più classi
- unendo i vincoli avremmo _disjoint + complete_ così da risolvere i casi ambigui
	- in questo caso ogni oggetto sarà istanza di esattamente una sola delle sottoclassi della generalizzazione 
![[dir/dir basi/asset/file 22.png|400]]
generalmente: ![[dir/dir basi/asset/file 23.png|dir basi/asset/file 23.png]]
<br>
- una classe può essere sottoclasse di più classi, anche se generalmente questo concetto non è rappresentabile a livello pratico ![[dir/dir basi/asset/file 24.png|400]]

#### Operazioni di classe
Come detto all'inizio, un oggetto ha:
- proprietà statiche: i valori cambiano a seguito di espliciti modifica dei dati da parte degli utenti nel sistema
- _proprietà dinamiche_ (operazioni): i valori vengono calcolati ogni volta che servono, a partire dai valori di altre proprietà
<br>
- in generale una operazione della classe C indica che su ogni oggetto della classe C si può eseguire un calcolo per:
	- calcolare un valore da altri dati
	- effettuare cambiamenti di stato dell’oggetto, dei link in cui è coinvolto e/o degli oggetti a lui collegati
<br>
- sintassi generale: `nome_operazione(argomenti) : tipo_ritorno`
	- `argomenti` è una lista di elementi di forma `nome_argomento : tipo_argomento`
	- `tipo_ritorno` è il tipo del valore restituito dall'operazione
- i tipi di ritorno e degli argomenti possono essere tipi concettuali o anche classi del diagramma
- un’operazione di classe può essere invocata solo su un oggetto di classe, in questo modo: `oggetto.operazione(argomenti)` 
<br>
- un operazione può accedere a:
	- attributi della classe cui appartiene
	- parametri passati all'operazione
	- oggetti della classe stessa, compresi i loro attributi e altre operazioni
	- oggetti di altre classi, se esiste un'associazione o una dipendenza, permettendo l'accesso alle loro operazioni o attributi
	- le associazioni della classe, navigando i link con altre classi e accedendo agli oggetti collegati.
<br>
- un'operazione di classe supporta i vincoli di molteplicità sia in input che in output
<br>
- il concetto di ereditarietà si applica anche alle operazioni di classe:
	- dato che StudenteStraniero essendo sottoclasse di Studente eredita anche le operazioni quindi possiamo chiamare `ss.media_fino_a(2/6/2023)` che restituisce `28.3` ![[dir/dir basi/asset/file 26.png|350]]
- il diagramma delle classi non definisce cosa calcolano le operazioni e nemmeno il come modificano i dati, ad ogni diagramma andrà affiancato un documento che specifica per queste informazioni.

#### Specializzazioni
In una generalizzazione una classe può avere proprietà aggiuntive rispetto alla superclasse ma può anche **specializzare** le proprietà ereditate dalla superclasse, restringendone il tipo e quindi il dominio (non potrà mai estenderlo)

###### Specializzazione di attributo:
- lo schema a sinistra è ammesso perché l’articolo nuovo riduce il dominio di `anni_garanzia` richiedendo una garanzia di almeno 2 anni, la classe a destra non è ammessa perché estende il dominio da `Interi` a `Reali`![[file 27.png]]
###### Specializzazione di associazioni:
- un’associazione con degli attributi abbiamo detto che si chiama *association class* ed essendo quindi una classe può anche essere radice di relazioni is-a e generalizzazioni
- È importante che l’associazione `vend_nuovo` sia compatibile con l’associazione `venditore`, quindi che metta in relazione oggetti che hanno lo stesso tipo degli oggetti messi in relazione dalla relazione padre (`venditore`) ![[file 28.png]]
- in questo caso è ok perché `ArticoloNuovo` è un `ArticoloInVendita` e un `VenditoreProfessionale` è anche un `Utente`
- un `ArticoloNuovo` può essere legato a un `VenditoreProfessionale` e può partecipare a più link `venditore` con `Utente`
	- link venditore può diventare un link vend_nuovo quindi restringendo il dominio
	- preso $a_{1},a_{2}$ articoli nuovo e $v_{1},v_{2}$ venditori professionali:
		- data la coppia (i link sono coppie): $(a_{1},v_{1})$ vend_nuovo, non può esistere la coppia $(a_{2},v_{1})$ con vend_nuovo però può esistere $(a_{1},v_{2})$ con venditore
		- $(a_{1},v_{1})$ con vend_nuovo e $(a_{1},v_{1})$ con venditore non possono esistere poiché un link vend_nuovo è un link venditore (subset)
<br>
- in questo caso il sistema deve rappresentare impiegati e il dipartimento a cui afferisce ogni impiegato con data di inizio del lavoro e il direttore del dipartimento (che è un impiegato)
- i direttori devono afferire al dipartimento che dirigono![[file 29.png|400]]
###### Condizioni di Correttezza
Le specializzazioni di associazioni hanno bisogno di particolare attenzione per quanto riguarda i vincoli di molteplicità

- `ArticoloNuovo` può partecipare a infiniti link di `vend_nuovo`, ma questo va in contraddizione con l’associazione venditore che limita i link di `ArticoloInVendita` ad 1
- link `vend_nuovo` è un link `venditore` a tutti gli effetti, possiamo solo restringere il vincolo
![[file 30.png]]

- in questo caso siamo obbligati ad avere due link di `vend_nuovo` ma l’associazione `venditore` ci limita ad 1 e quindi vanno in contraddizione
![[file 31.png]]

###### Specializzazione di operazioni di classe
Anche le operazioni possono essere oggetto di specializzazioni in sottoclassi
- uno schema per degli articoli in vendita avrà un’operazione per calcolare il prezzo in base ad alcuni parametri
- la sottoclasse `ArticoloInOfferta` eredita la stessa operazione ma la modifica ad esempio applicandoci uno sconto ![[file 32.png|400]]
- le operazioni devono essere _compatibili_:
	- stesso numero e tipo di argomenti
	- il tipo di ritorno dell’operazione nella sottoclasse è dello stesso tipo o sotto-tipo di quello dell’operazione nella superclasse
<br>
- per le operazioni abbiamo il documento di specifica delle operazioni, in questo caso: ![[file 33.png]]