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
