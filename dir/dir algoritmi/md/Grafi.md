*%% lezione 28/02/2025 - %%*

Definizione di grafo: $G(V,E)$
$|V|=n, \ V$ è l'insieme dei nodi (finito)
$|E|= m, \ E \subseteq V \times V$ è un insieme di archi che possono essere orientati (grafo diretto) o meno (grafo non diretto) dove: arco = coppie di nodi
- $0 \le m \le n(n-1) = O(n^2)$ se il grafo è diretto
- $0 \le m \le \frac{n(n-1)}{2} = O(n^2)$ se il grafo è non diretto[^1]
 
 ![[dir/dir algoritmi/asset/file 1.png|dir algoritmi/asset/file 1.png]]

Esistono diverse caratterizzazioni di grafi: 
- grafi sparsi: se $m = O(n)$, quindi "ha pochi archi"
- grafi densi: se $m = \Omega(n^2)$
	- grafi completi: hanno tutti gli archi possibili, m = $\theta(n^2)$
	- torneo: se in un grafo diretto vi è una esattamente un arco per ogni coppia di nodi
- un grafo non deve essere necessariamente denso o sparso, può $m =\theta(n\log n)$


[^1]: non vengono contate le coppie uguali in quanto gli archi sono bidirezionali, ma asintoticamente non cambia poiché $\frac{1}{2}$ è una costante

## Albero
Un albero è un grafo connesso, senza cicli, e sparso in quanto ha esattamente $n-1$ archi [^2]
![[dir/dir algoritmi/asset/file 2.png|dir algoritmi/asset/file 2.png]]

[^2]: non tutti i grafi sono alberi (vale il contrario)

![[dir/dir algoritmi/asset/file 3.png|300]]grafo sconnesso con ciclo

Per albero radicato si intende un albero cui abbiamo definito una radice, definire una radice porta a una gerarchia e quindi in base al nodo cui radichiamo avremo un diverso albero, potremmo intenderlo come un grafo diretto

|        | albero non radicato                | albero radicato                                                      |
| ------ | ---------------------------------- | -------------------------------------------------------------------- |
| foglie | nodo con un solo arco              | nodo senza figli                                                     |
| grado  | numero di archi incidenti sul nodo | si distingue in entrate/uscente (archi che arrivano/che partono)[^3] |


[^3]: $\text{entrante: deg}^-(v) \ / \ \text{uscente: deg}^+(v)$

si dimostra facilmente per induzione sul numero n di nodi che un albero ha sempre $m = n-1$ archi, sfruttando la proprietà degli alberi di avere foglie (nodi di grado 1), dato che non sono grafi ciclici

## Grafo planare
I grafi planari sono quei grafi che posso disegnare sul piano senza che gli archi si intersechino e sono un esempio di grafo sparso
![[dir/dir algoritmi/asset/file 4.png|dir algoritmi/asset/file 4.png]]
gli alberi sono un sottoinsieme di grafi planari
tutti i grafi di 4 nodi sono planari

#### Teorema di Eulero
Un grafo planare di $n > 2$ nodi ha al più $3n-6$ archi
![[dir/dir algoritmi/asset/file 5.png|dir algoritmi/asset/file 5.png]]
Osservando la tabella deduciamo quindi che da $n=5$ in poi esistono di certo grafi non planari, essenzialmente la tabella mi dice che se ad esempio ho 5 nodi e disegno 10 archi è impossibile avere un grafo planare ma se ne disegno 9 ho la possibilità di fare un grafo planare.

### Rappresentazione di grafi tramite matrici binarie
Dato un grafo $M[i][j] = 1$ se è solo se c'è un arco diretto da $i$ a $j$
$n^2$ è la dimensione di matrice 
$$
\begin{array}{|c|c|c|c|c|c|}
\hline 0 & 0 & 1 & 0 & 0 & 1 \\
\hline 0 & 0 & 0 & 0 & 0 & 1  \\
\hline 1 & 0 & 0 & 0 & 1 & 1  \\
\hline 0 & 0 & 0 & 0 & 1 & 0   \\
\hline 0 & 0 & 1 & 1 & 0 & 1 \\
\hline 1 & 1 & 1 & 0 & 1 & 0 \\
\hline
\end{array}
$$

in un albero diretto sulla $j$ ci stanno gli archi entranti e sulla $i$ gli archi uscenti
### Rappresentazione di grafi tramite liste di adiacenza
Si utilizza una lista di liste, che ha tanti elementi quanti nodi del grafo $G$, dove $G[x]$ è una lista contenente i nodi raggiunti da archi che partono da $x$, *(nodi adiacenti al nodo $x$)*
![[dir/dir algoritmi/asset/file 7.png|dir algoritmi/asset/file 7.png]] rappresentazione più usata

Vantaggi:
- risparmio di spazio nel caso di grafi sparsi
- vedere se due archi sono connessi può costare anche $O(n)$
[[Definizioni#Chi preferire?|quale preferire?]]
- col vettore dei padri posso ricavarmi la lista di adiacenza

#### Pozzi
[[Pozzi]]

#### Visite nei grafi
Per scoprire le proprietà di un grafo devo visitarlo: 
- [[Visite nei grafi]]
	- [[Visite nei grafi#DFS Depth-first search|DFS Depth-first search]]
	- [[Visite nei grafi#BFS Breadth-First Search|BFS Breadth-First Search]]
- durante una visita DFS non vengono attraversati tutti gli archi ma solo un sottoinsieme e poiché non ci sono cicli l'algoritmo crea un albero detto _albero DFS_
![[dir/dir algoritmi/asset/file.png|dir algoritmi/asset/file.png]]
a sx un grafo $G$, a dx gli alberi DFS che si ottengono a partire dai nodi $9,4,3$ 
(supponendo che le liste di adiacenza siano ordinate crescentemente)

- un albero DFS può essere memorizzato tramite il _vettore dei padri_
<br>
- durante una visita BFS vengono creati gli alberi BFS:
  ![[dir/dir algoritmi/asset/file 27.png]]a sx un grafo $G$, a dx gli alberi BFS che si ottengono a partire dai nodi $0,5,2$ 

#### Il vettore dei padri
Il vettore dei padri $P$ di un albero DFS di un grafo di $n$ nodi ha $n$ componenti:
- se $i$ è nodo dell'albero allora $P[i]$ contiene il padre del nodo $i$
- se $i$ non è nodo (non è stato esplorato) allora $P[i]$ contiene il valore $-1$

il valore $-1$ viene dato anche per convenzione negli algoritmi quando non è ancora stato esplorato
inoltre se $P[i]=i$ allora $i$ è la radice dell'albero

prendendo l'albero DFS fatto a partire dal nodo $9$ abbiamo che il vettore dei padri è[^4]:
$$
\begin{array}{|c|c|}
\hline index & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9\\
\hline vettore \ vuoto &-1 & -1 & -1 & -1 & -1 & -1 & -1 & -1 & -1 & -1 \\
\hline vettore \ dei padri &9 & 0 & 9 & 2 & 7 & -1 & -1 & 2 & 9 & 9 \\
\hline
\end{array}
$$
[^4]: il padre di $0$ è $9$

col vettore dei padri non ho nessuna difficoltà dalla foglia a salire fino alla radice

- se il grafo non è diretto: ci saranno lo stesso numero di nodi, non importa la radice
- mentre se è diretto: potrà variare in quanto potrebbero esserci dei pozzi ad esempio
- sta di fatto che in entrambi in base alla radice che scegliamo avremmo un forma diversa

codice $O(n+m)$:
```python 
def Padri(u,G):
	#genera il vettore dei padri P radicato nel nodo u
	def DFSr(x,G,P):
		for y in G[x]: #per ogni nodo y adiacente a x (liste di adiacenza G[x]) 
			if P[y] == -1: #-1 = non ancora visitato
				P[y] = x #assegna x come padre di y
				DFSr(y,G,P)

	n = len(G)
	P = [-1] * n #inizializza il vettore dei padri O(n)
	P[u] = u
	DFSr(u,G,P) #O(n+m)
	return P
```
il vettore dei padri rimane lo stesso salvo modifiche del tipo aggiunta/rimozione archi/nodi
##### Cammino 
Il vettore dei padri radicato in $x$ permette di verificare se un nodo $y$ è raggiungibile ma anche di ricostruire il percorso da $x$ a $y$:
- se $P[y]\ne-1$ allora $y$ è raggiungibile
- partendo da $y$ risaliamo l'albero fino ad $x$
- ci salviamo il percorso e invertendolo abbiamo ottenuto il **cammino** da $x$ a $y$ 

codice $O(n)$ (se si dispone del vettore dei padri):
```python
	Def CamminoR(u.P):
		if P[u] == -1 return []
		if P[u] == u return [u]
		return CamminoR(P[u],P) +u 
```

iterative code[^5] 

ove esistessero più cammini che vanno dal nodo $x$ al nodo $y$ la funzione $Cammino$ non garantisce di restituire il cammino minimo, (quello che attraversa il minor numero di archi)

[^5]: ```python 
	def Cammino(u,P):
		#restituisce cammino dal nodo radice dell'albero P al nodo u
		if P[u]==1 return []
		path = []
		while P[u] != u: #O(n), could be log ma dobbiamo considere il caso chain
			path.append(u)
			u = P[u]
		path.append(u)
		path.reverse() #O(n) (magari è il primo figlio)
		return path
	```


#### Colorazione di Grafi
[[Colorazione di Grafi]]

#### Grafi trasposti
Dato un grafo diretto $G$ il grafo trasposto di $G$, $G^T$ ha gli stessi nodi di $G$ ma archi con direzione opposta
![[dir/dir algoritmi/asset/file 14.png|dir algoritmi/asset/file 14.png]]

- i nodi che in $G$ portano a $u$ sono i nodi che in $G^T$ sono raggiungibili a partire da $u$
- codice che traspone il grafo in $O(n+m)$:
```python 
def Trasposto(G):
	GT = [[] for _ in G]
	for i in range(len(G)):
		for v in G[i]:
			GT[v].append(i)
	return GT
```


 Un grafo diretto cattura relazioni di propedeuticità un arco da $a$ a $b$ indica che $a$ è propedeutico a $b$, se riesco a ordinare i nodi del grafo in modo che gli archi vadano tutti ida sinistra verso destra potrò rispettare le propedeuticità
>questo ordinamento è detto [[Ordinamento topologico|topologico]]

un grafo diretto può avere da $0$ a $n!$ ordinamenti topologici [^6]

[^6]: un algoritmo esaustivo che genera i differenti ordinamenti e controlla il vincolo sugli archi ha complessità $\Omega(n!)$ ed è quindi improponibile

Un grafo G si dice parzialmente orientato se contiene sia archi orientati che archi non orientati
![[dir/dir algoritmi/asset/file 18.png|dir algoritmi/asset/file 18.png]]

#### Cicli
Dato un grafo $G$ diretto o non diretto ed un suo nodo $u$ vogliamo sapere se da $u$ è possibile raggiungere un ciclo in $G$
![[dir/dir algoritmi/asset/file 19.png|dir algoritmi/asset/file 19.png]]
partendo da questo grafo e dal nodo $1$ è possibile raggiungere il ciclo $2,4,6$ un idea per questo problema potrebbe essere: fare una ricerca e se si visita un nodo già visitato allora siamo in un ciclo[^7], ma non funziona nel caso avessimo a che fare con grafi non diretti in quanto due nodi se sono collegati appariranno ciascuno nella lista di adiacenza dell'altro e questo viene interpretato come un ciclo di lunghezza $2$ dall'algoritmo andando a restituire True

[^5]: ```python 
def ciclo(u, G):
	visitati = [0] * len(G)
	return DFSr(u, G, visitati)
	#
	def DFSr(u, G, visitati):
		#restituisce True se nella visita da u si incontra un nodo già visitato
		visitati[u] = 1
		for v in G[u]:
			if visitati[v] == 1:
				return True
			if DFSr(v, G, visitati):
					return True
		return False
	```

Per risolvere questo problema devo distinguere per ciascun nodo $y$ il nodo $x$ che mi ha portato a visitarlo:
```python 
def ciclo(u, G):
	visitati = [0] * len(G)
	return DFSr(u, u, G, visitati)
 
def DFSr(u, padre, G, visitati):
	visitati[u] = 1
	for v in G[u]:
		if visitati[v] == 1:
			if v != padre:
				return True
		else:
			if DFSr(v, u, G, visitati):
				return True
	return False
```

 complessità $O(n)$:
- se il grafo non contiene cicli allora ha al più $n−1$ archi e quindi $O(n+m)=O(n)$
- se invece contiene cicli ne scopriamo uno dopo aver considerato al più $n$ archi e l’algoritmo termina

In realtà l'algoritmo originariamente pensato potrebbe sbagliare anche con alcuni grafi diretti come ad esempio:  ![[dir/dir algoritmi/asset/file 20.png|dir algoritmi/asset/file 20.png]]

Durante la visita DFS posso incontrare nodi già visitati in tre modi diversi:
- **archi in avanti** ovvero le frecce che da un antenato puntano ad un discendente
- **archi all’indietro** ovvero le frecce che da un discendente vanno ad un antenato
- **archi di attraversamento** ovvero quelle frecce che ci portano da un sottoalbero ad un altro

![[dir/dir algoritmi/asset/file 21.png|300]] in questo albero DFS abbiamo diversi nodi già visitati:
- 3 → 5 questo è un arco in avanti dato che 3 è antenato di 5
- 6 → 1 arco indietro perché 1 e antenato di 6
- 2 → 3 è di attraversamento dato che ci porta in un altro sottoalbero (o comunque non è ne avanti ne indietro)

Soltanto la presenza di archi all’indietro indica la presenza di un ciclo. Dobbiamo quindi distinguere la scoperta di nodi già visitati grazie ad un arco all’indietro rispetto agli altri

Soltanto nel caso di archi all’indietro andiamo a visitare un nodo già visitato che non ha ancora finito la sua ricorsione, ad esempio nel grafo sopra abbiamo 6 che ci porta ad 1, un nodo già visitato e 1 non ha ancora finito la sua ricorsione

Progettiamo un algoritmo per il vettore V dei visitati, un nodo vale:

- 0 se il nodo non è stato ancora visitato
- 1 se il nodo è stato visitato ma la ricorsione su quel nodo non è ancora finita
- 2 se il nodo è stato visitato e ha terminato la sua ricorsione

In questo modo se troviamo un arco diretto verso un nodo che ha valore 1 abbiamo trovato un ciclo, codice con:
```python 
def DFSr(u, G, visitati):
	#restituisce True se la DFS da u trova un arco all'indientro e quindi un ciclo
	visitati[u] = 1
	for v in G[u]:
		if visitati[v] == 1:
			return True #nodo già esplorato, caso arco all'indietro
		if visitati[v] == 0: #non ancora visitato
			if DFSr(v, G, visitati):
				return True
	visitati[u] = 2 #nodo completamente esplorato
	return False
 
def cicloD(u, G):
	visitati = [0] * len(G)
	return DFSr(u, G, visitati)
```

complessità $O(n+m)$ in quanto devo visitarlo tutto non importa da che punto parto, stessa cosa vale nel caso di grafi diretti[^7]

[^7]: ```python 
def cicloD(G):
	visitati = [0] * len(G)
	for u in range(len(G)):
		if visitati[u] == 0: #viene avviata la DFS solo dai nodi non ancora visitati
			if DFSr(u, G, visitati):
				return True
	return False
	```

### Ponti
Un arco la cui eliminazione disconnette il grafo è detto **ponte**, essi rappresentano criticità nel grafo ed è utile identificarli
![[dir/dir algoritmi/asset/file 22.png|dir algoritmi/asset/file 22.png]]

- un grafo può non avere neanche un ponte, caso di grafi ciclici
- oppure ogni suo arco potrebbe essere un ponte, caso degli alberi

Per trovare i ponti potremmo:
- fare una ricerca esaustiva: controllare se ogni arco è ponte o meno
	- eliminiamo un arco $(a,b)$ fare una DFS e controllare se $b$ è raggiungibile da $a$, complessità $O(m+n)*m = O(m^2) = O(n^4)$ nel caso di un grafo denso
- usare una DFS opportunamente modificata per risolvere il problema in $O(m)$
	- nota: i ponti vanno ricercati unicamente tra gli archi dell’albero DFS 
	- ![[dir/dir algoritmi/asset/file 23.png|400]]
	- nota: se un arco non è un ponte, significa che esiste almeno un altro percorso alternativo
	- proprietà: se prendiamo $(u,v)$ un arco dell’albero DFS con $u$ padre e $v$ figlio, l’arco è un ponte se e solo se non ci sono altri archi tra i nodi del sottoalbero radicato in $v$ e il nodo $u$o antenati di $u$ 
	- ![[dir/dir algoritmi/asset/file 24.png|200]] cancellare l’arco $u−v$ in questo grafo non lo disconnette, abbiamo l’arco $x−y$ che ci “protegge”, dove $x$ è discendente di $v$ e $x$ è antenato di $u$, l'arco $u$ non ha informazioni tali da poter capire se l'arco $(u,v)$ è un ponte, ma lo scoprirà una volta terminata la visita di $v$
	- algoritmo:[^8]
		- Nodo $v$: esplora il proprio sottoalbero e restituisce al padre $u$ il valore $b$, cioè il livello minimo raggiungibile da $v$ e i suoi discendenti utilizzando anche eventuali archi all’indietro
		- Nodo $u$: confronta $b$ con la propria altezza, se b è maggiore dell’altezza di $u$, l’arco $(u, v)$ è l’unico collegamento, dunque è un ponte; altrimenti, c’è un percorso alternativo che collega il sottoalbero di $v$ a $u$ o ad un suo antenato, per cui l’arco non è un ponte
	- ricordiamo di non dover considerare l’arco stesso per il calcolo dell’altezza minima da restituire
	- implementazione in $O(n+m)$ [^9]
![[dir/dir algoritmi/asset/file 26.png|dir algoritmi/asset/file 26.png]]

[^8]: algoritmo dettagliato:
	- per ogni arco padre - figlio $(u,v)$ presente nell’albero DFS il nodo $u$ è in grado di scoprire se l’arco $(u,v)$ è ponte o meno usando questa strategia:
		- ogni nodo $v$
			- calcola la sua altezza nell’albero
			- calcola e restituisce al padre l’altezza minima che si può raggiungere con archi che partono dai nodi del suo sottoalbero diversi dall’arco $(u,v)$
		- Il nodo $u$ ricevuta l’informazione dal figlio $v$ confronta la sua altezza con quella ricevuta del figlio, affinché l’arco sia ponte, l’altezza di $u$ deve essere minore di quella restituita dal figlio

[^9]: ```python 
	def trova_ponti(G):
	    """ 
	    trova tutti i ponti in un grafo connesso non orientato G.
	    G è rappresentato come una lista di adiacenza.
	    restituisce una lista di coppie (u, v) che sono ponti.
	    """
	    def dfs(x, padre, altezza, ponti):
	        #assegna l'altezza al nodo corrente
	        if padre == -1:
	            altezza[x]=0
	        else:
	            altezza[x] = altezza[padre] + 1
	        min_raggiungibile = altezza[x] #minima altezza raggiungibile dal sottoalbero di x
	        for y in G[x]:
	            if altezza[y] == -1: #il nodo y non è stato ancora visitato
	                b = dfs(y, x, altezza, ponti)
	                if b > altezza[x]: #l'altezza di x è minore di quella ritornata da y
	                    ponti.append((x, y)) #(x,y) è un ponte
	                min_raggiungibile = min(min_raggiungibile, b)
	            elif y != padre: #y già visitato e (x,y) è un arco all'indietro
	                min_raggiungibile = min(min_raggiungibile, altezza[y])
	        return min_raggiungibile
	    altezza = [-1] * len(G) #array per memorizzare l'altezza dei nodi nella DFS
	    ponti = []
	    dfs(0, -1, altezza, ponti)
	    return ponti
	```

- un punto di articolazione è un vertice la cui rimozione è in grado di sconnettere il grafo
- un grafo cactus è un grafo connesso non orientato in cui ogni arco appartiene al massimo ad un ciclo
	- in un grafo cactus due cicli distinti possono avere al massimo un vertice in comune, senza condividerne archi 
	- il grafo a sinistra non è cactus in quanto l'arco $(1,2)$ appartiene a due cicli![[dir/dir algoritmi/asset/file 26.png]]

####  Tecnica greedy e grafi pesati
La tecnica greedy è un paradigma algoritmico:
- le decisioni prese sono irrevocabili, _no backtracking_ non si torna indietro
- le decisioni prese sono  basate sul una scelta "locale" si fa la scelta migliore in quel momento
- solitamente si ottiene una soluzione subottimale 
[[Grafi Pesati]]