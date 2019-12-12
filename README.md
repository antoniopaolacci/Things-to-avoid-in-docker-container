# Things-to-avoid-in-docker-container
Things to avoid in a docker containers

Alla fine, bene o male tutti, ci siamo arresi ai containers perché risolvono molti problemi ed hanno molti **vantaggi**:

1. **First**: Containers sono immutabili. Il sistema operativo, le versioni di librerie, configurazioni, cartelle e le applicazioni sono tutte *wrapped* all'interno del container.

2. **Second**: Containers sono leggeri. Invece di centinaia o migliaia di MB, il container alloca solo la memoria per il processo principale.
3. **Third**: Containers sono veloci. E' possibile avviare un container con la stessa velocità tipica dello start di un tipico processo linux.

Tuttavia, molti utenti trattano i containers come normali virtual machine e dimenticano che i contenitori hanno una caratteristica importante: i contenitori *sono usa e getta*. **Containers are ephemeral**.

Questa caratteristica costringe l'utente a cambiare la modalità di come dovrebbero gestire i contenitori. Vediamo cosa *non* dovremmo fare per continuare ad sfruttare i vantaggi dei containers:

1. Non memorizzare dati nei containers. Un container potrà essere stoppato, distrutto, rimpiazzato. Un applicazione versione 1.0 che esegue in un container potrà essere rimpiazzata con la versione 1.1 senza nessun impatto o perdita di dati. Per questa ragione, se hai bisogno di memorizzare dati, fallo in un volume. In tal caso, è necessario prestare attenzione anche se due contenitori scrivono dati sullo stesso volume in quanto potrebbero causare danni. Assicurarsi che le applicazioni siano progettate per scrivere correttamente su un archivio dati condiviso.

2. *Don’t ship your application in two pieces.* Molte persone considerano i containers come virtual machine, molti di loro pensano di porte that effettuare il deploy della loro applicazione in esistenti running containers. Questo potrebbe essere accettato durante la fase di sviluppo dove si ha bisogno di effettuare il deploy e il debug continuamente; ma per una CD (continuously delivery) pipeline verso la produzione, la tua applicazione dovrebbe far parte dell'immagine. Ricorda: *Containers sono immutabili.*

3. Non creare immagini grandi. – Una grande immagina sarà più difficile da distribuire. Assicurati di averne solo i file e le librarie necessarie per eseguire la tua applicazione. Non installare *packages* non necessari *updates* (yum update) che effettuerebbero il downloads di molti files.

4. Non utilizzare un'immagine in un singolo layer. Per utilizzare in modo efficace il file system a livelli, crea sempre il livello di base per il sistema operativo, un altro livello per la definizione del nome utente, un altro livello per il *runtime*, un altro livello per la configurazione e finalmente un altro livello per la tua applicazione. Sarà più facile ricreare, gestire e distribuire la tua immagine.

5. *Don’t create images from running containers.* In altri termini, non utilizzare “docker commit” per creare un'immagine. Questo metodo non è riproducibile, utilizzare sempre *Dockerfile* e sarà possibile tracciare tutte le modifiche salvando il *Dockerfile* in un repository per il versionamento del codice sorgente (git).

6. Don’t use only the **latest** tag, *latest* tag è come il tag *SNAPSHOT* per Maven. L'uso di *tags* è incoraggiato vista la natura del *layer* filesytem del containers. L'obbiettivo è non avere sorprese quando effettueremo il *build* della nostra immagine alcuni mesi dopo e  scoprire che la tua applicazione non potrà essere eseguita perché un livello padre (*FROM* nel Dockerfile) è stato rimpiazzato da una nuova versione che non è *backward compatible* con la precedente. Il tag *latest* dovrebbe essere evitato quando si effettua il deploy di containers in ambiente di produzione perché non potremmo tracciare quale versione dell'immagine stà eseguendo.

7. **Don’t run more than one process in a single container** – Containers sono perfetti per eseguire un singolo processo (http server, application server, database), ma se si ha più di un singolo processo, potresti avere problemi di gestione, per recuperare log e per  l'aggiornamento di uno solo dei processi che stanno eseguendo nello stesso container.

8. **Don’t store credentials in the image.** Invece utilizza variabili d'ambiente, recuperare le informazioni come username e password dall'esterno del container.

9. **Don’t run processes as a root user.** Di default i containers docker eseguono come *root*. L'immagine deve utilizzare l'istruzione *USER* per specificare un utente non root per eseguire i containers.

10. **Non affidarsi ad IP addresses** – Ogni container ha il suo indirizzo IP interno e puo' cambiare se si effettua uno start & stop del container. Se la tua applicazione o servizio ha bisogno di comunicare con un altro container, utilizza  variabili d'ambiente per passare la corretta coppia hostname & port da un container ad un altro.

