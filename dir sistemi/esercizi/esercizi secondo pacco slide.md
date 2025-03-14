> [!WARNING] possibile domanda d'esame
> con l'aiuto del mount cercare di capire le varie opzioni di mounting dei vari file system

- creare l'intero albero di directory: `home/utente1/dir1/dir3/dir/7` :
	- `mkdir dir1`
	- `mkdir dir1/dir3`
	- `mkdir dir1/dir3/dir7`
- creare un file vuoto in `dir7`:
	- `cd dir1/dir3/dir7`
	- `touch file`
- aprirlo ed editarlo con gedit
	- `gedit file`
	- se volessimo editarlo con nano: `nano file`
- verificare che `utente3` non sia presente in `/etc/passwd`,
	- crearlo
	- controllare che sia presente
	- verificare che non sia nel gruppo sudo
	- aggiungerlo al gruppo sudo
	- controllare che ora sia presente
- creare un file
	- modificarlo
	- leggerlo
	- verificare che i valori per atime ed mtime cambiano
- capire perché il comando `adduser` va usato solo da un utente amministratore, mentre il comando `groups` no

