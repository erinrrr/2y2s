*%% lezione 27/02/2025 %%*
## Algoritmo efficiente/inefficiente/ottimo
- un algoritmo si dice efficiente se è di ordine **polinomiale**, nella dimensione n dell'input:
	- $O(n^c)$ per una qualche costante $c$
- inefficiente se la sua complessità è di ordine **super-polinomiale**:
	- *esponenziale*: funzione di ordine $\theta(c^n)=2^{\theta(n)}$
	- *super-esponenziale*: funzione che cresce più velocemente di un esponenziale 
	esempi: $2^{\theta(n^2)}\ \text{oppure}\ 2^{\theta(n\log n)},$ (credo che sia circa $2^{\Omega(n)}$, ma senza l'uguale)
	- *sub-esponenziale*: funzione che cresce più lentamente di un esponenziale $2^{o(n)}$
	esempi: $2^{\theta(\log^2 n)}=n^{\theta(\log n)},\ 2^{\theta(n)}, \ 2^{\theta(n^c)}\ c<1$
- ottimo se la complessità asintotica coincide con il limite inferiore del problema che risolve
	- In altre parole, se un algoritmo ha una complessità $O(g(n))$ (limitazione superiore) e il problema ha un limite inferiore $\Omega(f(n))$, allora l'algoritmo è detto **ottimo** se $f(n) = g(n)$. Ciò implica che l'algoritmo ha la miglior complessità possibile in termini di ordine di grandezza, poiché nessun altro algoritmo può risolvere il problema in modo asintoticamente più efficiente
	- complessità dell'algoritmo $\ne$ complessità del problema

#### Modi per calcolare le limitazioni inferiori 
- Dimensione dei dati:
	se un problema ha in input $n$ dati e richiede di esaminarli tutti, allora la limitazione inferiore della complessità è $\Omega(n)$
	(ricerca del max in una lista, basta scorrerla una volta), però non funziona con l'ordinamento in quanto non basta solo scorrere i dati ma vanno anche confrontati tra di loro
- Eventi contabili:
	se un problema richiede che un certo evento sia ripetuto almeno m volte abbiamo una limitazione inferiore pari a $\Omega(m)$
	(sorting basati su confronto, $n$ elem, numeri di confronti minimi da fare = $n\log n$ $\Rightarrow \Omega(n\log n)$)
	(se aggiungiamo dei vincoli il lower bound varia, magari usando algoritmi che non si basano sul confronto)[^1]
#### Problema intrattabile[^2]
- un problema si dice intrattabile se non può avere algoritmi efficienti
(esempio: stampare tutte le permutazioni di n elementi, lower bound: $2^{\Omega(n\log n)}$)

[^1]: Lower bound più forte prodotto di due matrici quadrate $n \times n$ è $\Omega(n^2)$

[^2]: Tesi di Church-Turing estesi: i modelli di calcolo realistici sono tra loro polinomialemente correlati. Il concetto di trattabilità è indipendente dalla macchina

-----
