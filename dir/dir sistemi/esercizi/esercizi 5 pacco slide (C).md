#### Esercizio 1
- compiliamo ed eseguiamo `hello_world`
- precompilazione e compilazione di `hello_world_1.c` e `hello_world_2.c`
- linking `hello_world_1.c` e `hello_world_2.c`
- includere `hello_world_1.c` e `hello_world_2.c`
	- si può ancora fare una compilazione separata?
	- come si compilano per ottenere l'eseguibile
- fare l'inclusione inversa prima `hello_world_2.c` e poi `hello_world_1.c`

>scompattare file: `tar -xzf filename.tgz`
 
#### Esercizio 2
- Spiegare l'output del seguente codice:
```C
#include <stdio.h>
int main()
{
printf("aaaaa\nbbbbb\f\rccccc\r\fddddd\reeeee\ffffff\n");
}
```


#### Esercizio 3
- guardare il codice in `integerSize.c` e in `realSize.c`
- compilarlo ed eseguirlo
- usando il man cercare di capire perché serve includere le librerie:
	- `stdlin`, `#include <stdlib.h>`
	- `limits`, `#include <limits.h>`
	- `float`, `#include <float.h>`
- riscrivere i 2 programmi usando `sizeof`
- 
>la dimensione di un tipo di dato può essere ottenuta usando la funzione 
> - `sizeof (datatype)`


###### Esercizio 4
4.1
- perché quando viene dichiarata una variabile ma non viene inizializzata assume un valore indeterminato?
4.2
- di che tipo devono essere le seguenti variabili:
	- ` distanza = velocita * tempo:
	- `dailyRate = 65.3:`
	- `hours = 8;`
	- `hourlyRate = pay/hours`
- se `hours = 0` che succede?
4.3
- scrivete un programma c in cui si mostrano le varie opzioni di formattazione  usando tutti i campi dei placeholder ossia `[parameter] [flags] [width] [.precision] [length]` per:
	- reali
	- interi
	- char


#### Esercizio 5
- che valore assumerà `printCount`:
```C 
int num1 = 55;
int num2 = 30;
int sum = num1 + num2;
int printCount;
printCount = printf("%d + %d = %d\n", num1, num2, sum);
```
la funzione restituisce come risultato il numero di caratteri stampati

#### Esercizio 6
- che valore assumerà `dataCount`:
```C
int dataCount;
dataCount = scanf("%d %d", &height, &weight);
```
la funzione restituisce come risultato il numero di valori di input letti

#### Esercizio 7
- scrivere un programma che consente di leggere diversi tipi di dati in input da tastiera
- provate anche a legger più dati con la stessa `scanf`
- cosa accade se si inserisce un dato di input sbagliato ovvero un formato diverso da quello atteso? (char al posto di intero)
```C
int main() {
	// declare variables
	int x;
	int y;
	int sum;

	//read the values for x and y from standard input
	printf("enter value for x: ");
	scanf("%d", &x);
	printf("enter value for y: ");
	scanf("%d", &y);

	sum = x + y;

	//print
	printf("x = %d\n", x);
	printf("y = %d\n", y);
	printf("x + y = %d\n", sum);
	
	printf("%d + %d = %d\n", x, y, sum);
	printf("%d - %d = %d\n", x, y, x - y);
	printf("%d * %d = %d\n", x, y, x * y);

	return 0
}
```

#### Esercizio 8
- scaricare `divione.c`
- verificare in che ordine vengono fatte le divisioni in questa espressione
	- `11 / 2 / 2.0 / 2`


#### Esercizio 9
- implementare il seguente codice usando un while loop 
```C
do{
printf("Enter a positive weight: ");
scannf("%d", &weight);
} while (weight <= 0);
```

#### Esercizio 10
- scrivere un programma che riceva come input un valore n che ne indica il numero di dati che debbono essere letti da stdin
- per ogni dato letto da stdin, verificare e imporre che sia maggiore di 0 e minore di 10
- dopo aver letto gli n dati in input ne calcola e stampa il valore della somma e il valore medio
- farlo con:
	- while
	- while + do while
	- for + while/do-while

#### Esercizio 11
- scrivere un programma che, date due stringhe $s1$ e $s2$ con `strlen(s1)>5` e `strlen(s2)>5`, copi i primi 5 caratteri di $s2$ in $s1$. Usare la funzione `strncpy`
- scrivere un programma che date 2 stringhe $s1$ e $s2$ concateni i primi n caratteri di s1 ad s2. Usare la funzione `strncat`
- scrivere un programma che date 2 stringhe $s1$ e $s2$ determini quella di dimensione minore, denotata come `nMin`, e confronti i primi nMin caratteri di $s1$ e $s2$

#### Esercizio 12
- progettare ed implementare una funzione che abbia lo stesso comportamento di `char *strncpy(char *dest, const char *src, size_t n);`
- progettare ed implementare una funzione che abbia lo stesso comportamento di `char *strncat(char *dest, const char *src, size_t n);`

#### Esercizio 13
- scrivere un programma che, facendo uso della funzione `getchar` legga dallo `stdin` una stringa di lunghezza arbitraria (ovvero lunghezza non nota a priori)(file stringInput.c)