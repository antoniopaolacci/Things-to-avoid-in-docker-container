# Things-to-avoid-in-docker-container
Things to avoid in a docker containers

Alla fine, bene o male tutti, ci siamo arresi ai containers perché risolvono molti problemi ed hanno molti **vantaggi**:

1. **First**: Containers sono immutabili. Il sistema operativo, le versioni di librerie, configurazioni, cartelle e le applicazioni sono tutte *wrapped* all'interno del container.

2. **Second**: Containers sono leggeri. Invece di centinaia o migliaia di MB, il container alloca solo la memoria per il processo principale.
3. **Third**: Containers sono veloci. E' possibile avviare un container con la stessa velocità tipica dello start di un tipico processo linux.

Tuttavia, molti utenti trattano i containers come normali virtual machine e dimenticano che i contenitori hanno una caratteristica importante: i contenitori *sono usa e getta*. **Containers are ephemeral**.

Questa caratteristica costringe l'utente a cambiare la modalità di come dovrebbero gestire i contenitori. Vediamo cosa *non* dovremmo fare per continuare ad sfruttare i vantaggi dei containers:

