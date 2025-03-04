*%% lezione 28/02/2025 %%*

Definizione di grafo: $G(V,E)$
$|V|=n, \ V$ è l'insieme dei nodi (finito)
$|E|= m, \ E \subseteq V \times V$ è un insieme di archi che possono essere orientati (grafo diretto) o meno (grafo non diretto) dove: arco = coppie  di nodi
- $0 \le m \le n(n-1) = O(n^2)$ se il grafo è diretto
- $0 \le m \le \frac{n(n-1)}{2} = O(n^2)$ se il grafo è non diretto[^1]
 
 ![[Pasted image 20250302213524.png]]

Esistono diverse caratterizzazioni di grafi: 
- grafi sparsi: se $m = O(n)$, quindi "ha pochi archi"
- grafi densi: se $m = \Omega(n^2)$
	- grafi completi: hanno tutti gli archi possibili, m = $\theta(n^2)$
	- torneo: se in un grafo diretto vi è una esattamente un arco per ogni coppia di nodi
- un grafo non deve essere necessariamente denso o sparso, può $m =\theta(n\log n)$


[^1]: non vengono contate le coppie uguali in quanto gli archi sono bidirezionali, ma asintoticamente non cambia poiché $\frac{1}{2}$ è una costante

## Albero
Un albero è un grafo connesso, senza cicli, e sparso in quanto ha esattamente $n-1$ archi [^2]
![[Pasted image 20250302220220.png]]

[^2]: non tutti i grafi sono alberi (vale il contrario)

![[Pasted image 20250302220511.png]] grafo sconnesso con ciclo

Per albero radicato si intende un albero cui abbiamo definito una radice, definire una radice porta a una gerarchia e quindi in base al nodo cui radichiamo avremo un diverso albero, potremmo intenderlo come un grafo diretto

|        | albero non radicato                | albero radicato                                                      |
| ------ | ---------------------------------- | -------------------------------------------------------------------- |
| foglie | nodo con un solo arco              | nodo senza figli                                                     |
| grado  | numero di archi incidenti sul nodo | si distingue in entrate/uscente (archi che arrivano/che partono)[^3] |


[^3]: $\text{entrante: deg}^-(v) \ / \ \text{uscente: deg}^+(v)$

si dimostra facilmente per induzione sul numero n di nodi che un albero ha sempre $m = n-1$ archi, sfruttando la proprietà degli alberi di avere foglie (nodi di grado 1), dato che non sono grafi ciclici

## Grafo planare
I grafi planari sono quei grafi che posso disegnare sul piano senza che gli archi si intersechino e sono un esempio di grafo sparso
![[Pasted image 20250302234216.png]]
gli alberi sono un sottoinsieme di grafi planari
tutti i grafi di 4 nodi sono planari

#### Teorema di Eulero
Un grafo planare di $n > 2$ nodi ha al più $3n-6$ archi
![[Pasted image 20250302234405.png]]
Osservando la tabella deduciamo quindi che da $n=5$ in poi esistono di certo grafi non planari, essenzialmente la tabella mi dice che se ad esempio ho 5 nodi e disegno 10 archi è impossibile avere un grafo planare ma se ne disegno 9 ho la possibilità di fare un grafo planare.

### Rappresentazione di grafi tramite matrici binarie
Dato un grafo $M[i][j] = 1$ se è solo se c'è un arco diretto da $i$ a $j$
$n^2$ è la dimensione di matrice
![[Pasted image 20250302234632.png]] 
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
![[Pasted image 20250302235308.png]] rappresentazione più usata
Vantaggi:
- risparmio di spazio nel caso di grafi sparsi
- vedere se due archi sono connessi può costare anche $O(n)$

### Pozzi
- In un grafo diretto un pozzo è un nodo senza archi uscenti, con un massimo di n pozzi
- mentre un pozzo universale è un pozzo verso cui tutti gli altri nodi hanno un arco, se c'è è unico
![[Pasted image 20250302235629.png]]$\Omega(n)$ per controllare se c'è un pozzo in quanto devo scorrere tutti i nodi (dimensione dei dati)

- per trovare un pozzo: 
	bisogna trovare (se c'è) la riga della matrice che contiene solo valori 0, quindi non ha archi uscenti
- per trovare il pozzo universale
	bisogna controllare che il nodo sia un pozzo,
	dobbiamo controllare se la colonna di quel nodo abbia tutti valori 1, (tranne su se stesso), ovvero che tutti puntino verso di lui
	 così: ![[Pasted image 20250304122438.png]]

a livello algoritmico per trovarlo potremmo usare un algoritmo che controlla l'intera matrice e quindi ci verrà a costare $O(n^2)$

Ma esiste un algoritmo che verifica se un grado diretto ha un pozzo universale in $\theta(n)$:
- prendiamo i nodi $i,j$ e controlliamo i loro valori:
$$
M[i][j]=\begin{cases}
1 \quad \text{i non é pozzo} \\
0 \quad \text{j non é pozzo universale}
\end{cases}
$$

se la cella vale $1$ allora $i$ ha un nodo verso $j$, avendo un arco uscente non può essere pozzo universale, se troviamo uno $0$ allora $j$ non è un pozzo universale dato che non tutti i nodi hanno un arco verso di lui, però può essere ancora un pozzo.

Utilizziamo un **approccio con due indici**: $i$ e $j$, inizializzati a $i = 0$, $j = 1$ (suppongo $i\ne j$)

| $i$ | $j$ | controllo $M[i][j]$ | Azione                                   |
| --- | --- | ------------------- | ---------------------------------------- |
| 0   | 1   | $M[0][1] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 0   | 2   | $M[0][2] = 1$       | $i$ non può essere pozzo → $i = 2$[^4]   |
| 2   | 3   | $M[2][3] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 2   | 4   | $M[2][4] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 2   | 5   | $M[2][5] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 2   | 6   | $M[2][6] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 2   | 7   | $M[2][7] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
```python
def pozzoU2(M):
    '''restituisce True se il grafo M ha pozzo universale, False Altrimenti'''
    L=[x for x in range(len(M))]
    while len(L)>1:
        a=L.pop()
        b=L.pop()
        if M[a][b]:
            L.append(b)
        else:
            L.append(a)
    x=L.pop()
    for j in range(len(M)):
        if M[x][j]: return False
    for i in range(len(M)):
        if i!=x and M[i][x]==0: return False
    return True
```

[^4]: (poiché $i=1$ è stato scartato prima)

