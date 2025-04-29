**DSU - Disjoint Set Union**, anche detta Union-Find:
- è una struttura dati per gestire insiemi disgiunti
- utilizzato per operazioni di unione e ricerca efficienti su insiemi disgiunti
- la gestione efficiente di insiemi disgiunti è utile in contesti come l'evoluzione di un grafo nel tempo attraverso l'aggiunta di archi
- ha tre operazioni fondamentali:
	- `Crea(S)`: restituisce una struttura dati Union-Find sull'insieme $S$ di elementi dove ciascun elemento andrà in un insieme separato
	- ` Find(x,C)`: restituisce il nome dell'insieme della struttura dati $C$ cui appartiene l'elemento $x$
	- `Union(A,B,C)`: modifica la struttura dati $C$ fondendo le componenti $A$ e $B$ e restituisce il nome della nuova componente
<br>
- con l'operazione `find(u)` possiamo determinare a quale componente appartiene un nodo e quindi se due nodi $u$ e $v$ appartengono alla stessa componente controllando se `find(u)` == `find(v)`
- quando viene aggiunto un arco $(u,v)$ ad un grafo, se non si trovano nella stessa componente connessa quindi se $A = find(u) \ne B = find(v)$ allora possiamo unire le due componenti con `Union(A,B)` ottenendo così un nuovo grafo


> [!info]+ cosa si intende per nome dell'insieme
> - c'è ampia flessibilità nella scelta, ma quello che conta è che `find(u)` = `find(v)` se e solo se sono nello stesso insieme
> - nelle implementazioni che seguono si sceglie come nome dell'insieme un particolare elemento dell'insieme stesso
> - come ad esempio il nome dell'elemento massimo contenuto nell'insieme

### Implementazione
il modo più semplice di implementare questa struttura dati per $n$ elementi è di mantenere il vettore $C$ delle componenti:
- ogni elemento è in un insieme distinto vale a dire $C[i] = i$
- quando la componente $i$ viene fusa con la componente $j$:
	- se $i>j$ allora tutte le occorrenze di $j$ nel vettore $C$ verranno sostituite da $i$
	- se $i<j$ allora succede il contrario

_codice_:
```python
def Crea(G):
	C = [i for i in range(len(G))]
	return C
 
def Find(u, C):
	return C[u]
 
def Union(a, b, C):
	if a > b:
		for i in range(len(C)):
			if C[i] == b:
				C[i] = a
	else:
		for i in range(len(C)):
			if C[i] == a:
				C[i] = b
```
**costo**:
- `Crea()` -> $\theta(n)$
- `Find()` -> $\theta(1)$
- `Union()` -> $\theta (n)$

> bilanciamento union costi e implementazione 2:

con qualche modifica implementativa possiamo **bilanciare** i costi:
- aumentiamo leggermente il costo di `Find`
- rendiamo `Union` più efficiente

_idea_: usare il vettore dei padri:
- `Find`: quando voglio sapere in che componente si trova un nodo devo semplicemente risalire alla sua radice
	- costo: $O(n)$
- `Union`: quando fondo due componenti una diventa figlia dell'altra
	- costo: $O(1)$

esempio:
![[dir/dir algoritmi/asset/file 35.png]]
il nuovo _codice_ sarà:
```python
def Crea(G):
	C = [i for i in range(len(G))]
	return C
 
def Find(u, C):
	while u != C[u]:
		u = C[u]
	return u
 
def Union(a, b, C):
	if a > b:
		C[b] = a
	else:
		C[a] = b
```

**costo**:
- `Crea()` -> $\theta(n)$
- `Find()` -> $O(n$)
- `Union()` -> $\theta (1)$

> bilanciamento find e implementazione 3

non vogliamo che il `Find` abbia un costo elevato quindi: è importante che i cammini per raggiungere le radici del vettore dei padri non diventino troppo lunghi:
- per evitare questo è preferibile mantenere gli alberi _bilanciati_

_idea_:
 - quando eseguo la `Union` per fondere componenti scelgo sempre come nuova radice la componente che contiene il maggior numero di elementi
- così da non aumentare la lunghezza del cammino per almeno la metà dei nodi presenti nelle due componenti coinvolte nella fusione (in pratica quelle dell'albero con il maggior numero di elementi)

in questo modo garantiamo la seguente proprietà:
> se un insieme ha altezza $h$ allora l'insieme contiene almeno $2^h$ elementi

da questa proprietà deduciamo che l'altezza delle componenti non potrà mai superare $\log_{2}n$ (in caso contrario avrei nella componente più di $n$ nodi, assurdo)

- devo far si che ai nodi radice sia associato anche il numero di elementi che la componente contiene
- ogni elemento è caratterizzato da una coppia $(x,\ numero)$ dove:
	- $x$ è il nome dell'elemento
	- $numero$ è il numero di nodi nell'albero radicato in $x$

_codice_:
```python
def Crea(G):
	C = [(i, 1) for i in range(len(G))]
	return C
 
def Find(u, C):
	while u != C[u]:
		u = C[u]
	return u
 
def Union(a, b, C):
	tota, totb = C[a][1], C[b][1]
	if tota >= totb:
		C[a] = (a, tota + totb)
		C[b] = (a, totb)
	else:
		C[a] = (b, tota)
		C[b] = (b, tota + totb)
```

**costo**:
- `Crea()` -> $\theta(n)$
- `Find()` -> $O(\log n)$
- `Union()` -> $O(1)$

---
prova della proprietà precedentemente descritta
proprietà:
- se una componente ha altezza $h$ allora la componente contiene almeno $2^h$ nodi
prova:
- assumiamo che per assurdo durante una delle fusioni si sia formata una nuova componente di altezza $h$ che non rispetta le proprietà
- consideriamo la prima volta che ciò accade
- e siano $ca$ e $cb$ le componenti che si fondono

- possono accadere due cose:
	- $ca$ e $cb$ erano componenti con la stessa altezza:
		allora entrambe avevano altezza $h-1$ ed ognuna aveva almeno $2^{h-1}$ elementi per ipotesi induttiva, quindi il numero totale di elementi della nuova componente è $2^{h-1}+2^{h-1} = 2^h$, contraddizione quindi la proprietà è verificata
	- $ca$ e $cb$ avevano altezze diverse:
		supponiamo che $ca$ abbia altezza $h$ e $cb < h$
		l'altezza dopo la fusione è quella della componente di altezza maggiore che doveva essere già di altezza $h$, in questo caso $ca$, e conteneva da sola già $2^h$ elementi per ipotesi induttiva, quindi la nuova componente avrà almeno $2^h$nodi, contraddizione anche in questo caso

![[Pasted-image-20250325192830.png]]