---
title: Sistema di backend
parent: Design dettagliato
has_children: false
nav_order: 2
---
# Sistema di backend

Una volta individuati i bounded context presenti all'interno del sistema di backend, rappresentato da greenhouse core, è necessario individuare una strategia di integrazione tale per cui i bouded context al suo interno siano il più autonomi possibili; solo in questo modo si possono evitare ad esempio dei rallentamenti o indisponibilità del sistema. A seguito dell'analisi dei requisiti e considerando l'autonomia dei bounded context, si è quindi scelto di realizzare un'architettura a micro-servizi, il quale porta con se diversi vantaggi, l'isolamento dagli errori a singoli componenti del sistema, una maggiore scalabilità, semplicità nel deployment etc.

<div align="center">
<img src="img/exagonal_architecture.png" alt="Architettura esagonale" id="fig1">
<p align="center">[Fig 1] Architettura esagonale</p>
</div>



Come detto precedentemente, nella sezione relativa al Design architetturale, sono stati individuati otto micro-servizi: GreenhouseCommunication, Greenhouse, Operation, Brighness, Humidity, SoilMoisture e Temperature

Ogni micor-servizio, al fine sempre di ricercare la massima autonomia e isolamento della logica di dominio, come suggerito dalle linee guida del DDD, è stato realizzato tramite un'architettura **esagonale**, anche chiamata **port and adapters** (<a href="#fig1">figura 1</a>); il che significa che la logica del dominio che questi servizi possono presentare viene isolata dal resto e resa indipendente dalle diverse tecnologie e interfacce che vengono utilizzate. Tale indipendenza è ottenuta grazie anche al **principio di inversione delle dipendenze** applicato al suo interno, secondo cui un livello può dipendere solamente dai livelli sottostanti, non da quelli al di fuori di lui; di conseguenza, la logica di dominio è il livello più interno della nostra architettura e non deve dipendere da nessuno, il livello applicativo è l'unico che può dipendere da esso, mentre il livello infrastrutturale può dipendere sia dal livello applicativo che da quello del dominio. 

I diversi micro-servizi realizzati, come si può vedere nella <a href="#fig2">figura 2</a>, prevedono tutti caratteristiche simili, ossia:

- espongono una ``API``, che indica le operazioni che possono essere richieste al servizio. Le API sono poi implementate dalla classe ``Model``;
- presentano uno o più ``Adapters`` incaricati di gestire e filtrare la comunicazione con l'esterno a seconda del protocollo di comunicazione utilizzato. Nel nostro caso sono stati impiegati MQTT e HTTP
- racchiudono i diversi ``adapters`` e ``Model`` in una classe ``Service``, il cui compito è installare i diversi adapters e rappresentare il servizio che verrà istanziato.

<div align="center">
<img src="img/classi_service_generale.png" alt="classi service generale" id="fig2">
<p align="center">[Fig 2] Diagramma delle classi: struttura generale di un micro-servizio</p>
</div>



Da quanto detto prima, quindi, possiamo intuire che il micro-servizio potrà svolgere funzionalità sia di **Server**, al fine di poter ricevere ed accogliere le richieste provenienti dai diversi micro-servizi in esecuzione, che di **Client** per poter a sua volta inviare richieste ai micro-servizi attivi, con il quale comunica, in modo da riuscire a portare a termine le sue attività. In particolare la componente Server sarà gestita dalla classe ``Adapter``, mentre la componente Client sarà gestita dalla classe ``Model``.

I micro-servizi: ``Greenhouse``, ``Operation``,``Brightness``, ``Temperature``, ``SoilMoisture`` e ``Humidity``, per riuscire a soddisfare le richieste provenienti dagli altri micro-servizi, interagiscono con un apposito database. Per questo motivo, per questi servizi, oltre alle componenti viste in precedenza, sono state progettate anche le classi che si occupano della persistenza dei dati e delle interazioni con il database.

Per capire meglio la progettazione che è stata fatta possiamo guardare la <a href="#fig3">figura 3</a>, la quale mostra un esempio di come è stata modellata l'interazione con il database per i servizi che si occupano di monitorare un parametro specifico della pianta.

Come si può vedere, per rendere indipendente il ``Model`` rispetto al livello di persistenza dei dati, viene utilizzato un componente ``PlantValueController``, che svolge la funzione di intermediario fra il ``PlantValueDatabase`` e il ``Model``. Grazie a questa struttura è possibile aggiungere un livello di astrazione, infatti se in futuro si decidesse di cambiare il modello della persistenza dei dati, o di avvalersi di altre tecnologie, queste modifiche non andrebbero ad intaccare il ``Model``, garantendo così il suo funzionamento anche in caso di modifiche future al livello dei dati. 

``PlantValue``, in questo caso, rappresenta una delle entità del dominio, che viene utilizzata dal livello della persistenza dei dati per poter fornire i dati nel formato corretto a chi lo richiede. 

In conclusione, possiamo dire che questa struttura, ci consente di tenere separati fra loro i diversi livelli dell'architettura esagonale e di mantenere applicato il principio di inversione delle dipendenze.

<div align="center">
<img src="img/classi_esempio_persistenza.png" alt="classi service DB" id="fig3">
 <p align="center">[Fig 3] Diagramma delle classi: esempio progettazione entità e interazioni con il DB</p>
</div>

## Interazione tra i diversi micro-servizi
I diversi micro-servizi per poter svolgere le loro funzioni hanno la necessità di comunicare e interagire tra loro, per capire quali sono le dinamiche del sistema, almeno ad alto livello, possiamo analizzare le figure <a href="#fig4">4</a> e <a href="#fig5">5</a>. Più in dettaglio la figura 4 mostra come avvengono le comunicazioni all'interno del bounded context **Gestione Serra**. Tale bounded context prevede infatti la presenza di cinque micro-servizi: Brightness, Humidity, SoilMoisture, Temperature e Greenhouse i quali comunicano tra loro per mezzo delle **API** messe a disposizione da ciascuno. Nello specifico la comunicazione, come si può vedere in figura, avviene in modo unidirezionale a partire dal servizio ``Greenhouse``, in quanto è quest'ultimo che ha il compito di ricevere i dati rilevati all'interno della serra e delegare poi al rispettivo servizio il compito di storicizzarli.

<div align="center">
<img src="img/gestione_serra.png" alt="Gestione serra interazioni" id="fig4">
 <p align="center">[Fig 4] Interazione dei micro-servizi all'interno di Gestione serra</p>
</div>

Nella <a href="#fig5">figura 5</a> è invece possibile osservare come avvengono le interazioni tra i microservizi che compongono i bounded context presenti nel sub-domain **Greenhouse core**. È da notare che in questo caso sono stati omessi i micro-servizi presenti all'interno del bounded context Gestione serra, al fine di rendere più chiara la rappresentazione.

<div align="center">
<img src="img/interazioni_microservizi.png" alt="interazioni microservizi", id="fig5">
 <p align="center">[Fig 5] Interazione dei micro-servizi presenti nel sub domain Greenhouse core</p>
</div>

L'interazione in questo caso coinvolgere due fonti distinte: il client, desktop o mobile, oppure il sistema di automazione. Nel primo caso le interazioni che possono avvenire da e verso il client, passano tutte per il servizio ``ClientCommunication``. Nel caso delle richieste effettuate dal client verso il sistema di backend, il servizio può comunicare con uno dei micro-servizi presenti all'interno di **Gestione serra**, qualora fosse interessato a reperire le informazioni relative alla serra o ad uno o più dei parametri rilevati, oppure con il servizio ``Operation`` presente all'interno del bounded context **Operation**, qualora fosse interessato a reperire o effettuare un'operazione sulla serra. Per quanto riguarda invece le comunicazioni verso il client, queste possono solo partire dal servizio ``Greenhouse``, le quali passando, come detto, per ``ClientCommunication`` arrivano al client.

Nel caso, invece, in cui le interazioni coinvolgano il sistema di automazione, tutte le comunicazioni sono mediate dal servizio ``GreenhouseCommunication`` presente all'interno del bounded context **Gestione comunicazione serra**. Tale servizio può comunicare solo con il servizio ``Greenhouse`` al fine di informarlo dei nuovi rilevamenti, mentre può ricevere delle richieste solo dal servizio ``Operation`` il quale lo informa di eseguire una determinata operazione sulla serra, indipendentemente dalla modaltà di gestione adottata.

Il servizio ``Operation``, come si può notare in figura, può quindi ricevere le richieste sia da ``ClientCommunication``, il quale, come detto, può richiedere le informazioni relative alle operazioni oppure richiederne l'esecuzione di una specifica quando la modalità di gestione è manuale, oppure da ``Greenhouse`` qualora la modalità di gestione fosse automatica e risulta necessario effettuare un'operazione correttiva.