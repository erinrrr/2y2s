- **kernel**:
	- componente del sistema operativo
	- gestisce le risorse disponibile 
	- offre l'accesso e l'utilizzo delle risorse da parte dei processi
- le risorse principali gestite sono:
	- CPU
	- RAM
	- I/O
- system call
	- limitato numero di punti di accesso al kernel (in base all'os e alla versione del sistema)
	- permettono ad un processo di accedere ai servizi offerti dal kernel
	- quindi ad un programmatore di creare programmi che interagiscano con il kernel

![[dir/dir sistemi/asset/file 28.png|400]]

## System call
- funzioni che permettono ai programmi di interagire con il sistema operativo
- ogni sistema operativo ha un meccanismo diverso per implementare e gestire le system call
- in generale tutte le system call corrispondono esattamente a una funzione in C con lo stesso nome
