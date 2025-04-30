consideriamo il problema della **selezione di attività** per presentare gli algoritmi greedy:
- abbiamo una lista di $n$ attività da eseguire:
- ogni attività è caratterizzata da una coppia: tempo di inizio e tempo di fine
- due attività sono dette _compatibili_ se non si sovrappongono
- vogliamo trovare un sotto insieme di attività compatibili di massima cardinalità


![[file 39.png]]

 capiamo facilmente che l'insieme da selezionare è $\{b,e,h\}$, in quanto non c'è alcun insieme di 4 attività compatibili, e nessun altro insieme di 3 attività compatibili
 
volendo utilizzare il paradigma greedy:
- dovremmo trovare una regola semplice da calcolare
- regola che ci permetta di effettuare ogni volta la scelta giusta

per questo problema abbiamo diverse potenziali regole di scelta:
- prendere l'attività compatibile che inizia prima
- l'attività compatibile che dura meno
- l'attività che ha meno conflitti con le rimanenti

> la regola giusta è: prendere l'attività compatibile che finisce prima

_dimostrazione_:
 - supponiamo per assurdo che la soluzione greedy $SOL$ trovata non sia ottima
 - le soluzioni ottime differiscono dunque da $SOL$
 - nel caso ci fossero più di una soluzione ottima, sia $SOL^*$ quella che differisce nel minor numero di attività da $SOL$
 - dimostreremo ora che esiste un'altra soluzione ottima $SOL'$ che differisce ancor meno da $SOL$ il che è assurdo
<br>
- siano $A_{1},A_{2},\dots$ le attività nell'rodine in cui sono state selezionate dal greedy
- sia $A_{i}$ la prima attività scelta dal greedy e non dall'ultimo
- nella soluzione ottimale ci sarà un'altra attività $A'$ che va in conflitto con $A_{i}$ (altrimenti $SOL^*$ non sarebbe ottima)
- a questo punto si può sostituire in $SOL^*$ l'attività $A'$ con l'attività $A_{i}$ senza creare conflitti (perché seguendo i criteri di scelta del greedy $A_{i}$ termina prima di $A'$)
- si ottiene quindi una soluzione ottima $SOL'$: le cui attività sono tutte compatibili e la cardinalità di $SOL'$ è uguale a quella di $SOL^*$
- ma $SOL'$ differisce da $SOL$ di un'attività in meno rispetto a $SOL^*$

![[file 40.png]]


#### Implementazione
idee:
- cercare nella lista delle attività quella che finisce prima, scorrendo la lista costa $O(n)$ ad ogni iterazione del while, conviene fare un pre-processing ordinando le attività per tempo di fine crescente, $\theta(n\log n)$ per il sort e $\theta(1)$ per l'estrazione dell'attività
- per eseguire efficientemente la condizione di conflitto conviene mantenere una variabile con il tempo di fine dell'ultima attività presente nella soluzione

```python
def selezione_a(lista):
	lista.sort(key = lambda x: x[1])
	libero = 0
	sol = []
	for inizio, fine in lista:
		if libero < inizio:
			sol.append((inizio, fine))
			libero = fine
	return sol
```

**costo**:
- sorting -> $\theta(n\log n)$
- il for viene eseguito $n$ volte e ogni iterazione costa $O(1)$ -> $O(n)$
- costo totale -> $\theta(n\log n)$

### assegnazione attività

- abbiamo una lista di attività, caratterizzate da tempo di inizio e di fine
- le attività vanno tutte eseguite
- vogliamo assegnarle al minor numero di aule 
- in una stessa aula non possono eseguirsi più attività in parallelo

![[file 41.png]]

un possibile algoritmo greedy potrebbe essere:
- occupiamo aule finché ci sono attività da assegnare
- ad ogni aula, una volta inaugurata assegnare il maggior numero di attività non ancora assegnate che è in grado di contenere
questa soluzione non è corretta

_soluzione_:
- si inizializza la soluzione con un aula senza attività
- finché ci sono attività da assegnare,  estraiamo quella che inizia prima
	- se nella soluzione c'è un'aula cui è possibile eseguirla, la assegniamo a quell'aula
	- altrimenti nella soluzione inseriamo una nuova aula e gli assegniamo l'attività

_correttezza_:
- sia $k$ il numero di aule utilizzate della soluzione
- ci sono nella lista $k$ attività incompatibili a coppie
	- questo implica che sono necessarie $k$ aule
- sia $(a,b)$ l'attività che ha portato all0introduzione della $k-esima$ aula
- in quel momento nelle altre $k-1$ aule era impossibile eseguire l'attività $(a,b)$
- ma per il criterio di scelta greedy posso dire che nell'istante di tempo $a$ erano tutte occupate (iniziavano prima del tempo $a$ e non erano ancora finite)
- quindi nell'istante di tempo $a$ a due a due tutte queste $k$ attività sono incompatibili

![[file 42.png]]

**idea + efficienza**:
- per individuare efficientemente l'attività che inizia prima anche qui si effettua il pre-processing in cui si ordinano le attività per tempo di inizio
- per individuare se una delle aule in soluzione è in grado di eseguire un certo lavoro $(a,b)$ uso un $heap$ minimo in cui metto le coppie $(libera,i)$ dove:
	- $libera$ indica il tempo in cui si libera l'aula $i$
	- in modo da sapere quale aula si libera prima verificando $H[0][0]$ in $O(1)$
- se l'aula che si libera prima può eseguire l'attività allora quando gli viene assegnata la nuova attività devo aggiornare il valore $libera$ della coppia che la rappresenta nell'heap
- in caso contrario ne creiamo una nuova e inseriamo nell'heap la coppia che rappresenta questa nuova aula
	- ricordandoci che inserimenti e cancellazioni in un heap costano $O(\log n)$

```python
def assegnazioneAule(lista):
	from heapq import heappop, heappush
	Sol = [[]]
	H = [(0,0)]
	lista.sort()
	for inizio, fine in lista:
		libera, aula = H[0]
		if libera < inizio:
			Sol[aula].append((inizio, fine))
			heappop(H)
			heappush(H, (fine, aula))
		else:
			Sol.append([(inizio, fine)])
			heappush(H, (fine, len(Sol)-1))
	return Sol
```

**costo**:
- sorting -> $\theta(n\log n)$
- il for viene eseguito $n$ volte
	- al caso pessimo può essere eseguita un'estrazione dall'heap seguita da un inserimento, entrambe costano $O(\log n)$ il for costa-> $O(n\log n)$
- costo totale -> $\theta(n\log n)$

[[esercizi algo#Esercizio slide 13 algoritmi greedy]]
