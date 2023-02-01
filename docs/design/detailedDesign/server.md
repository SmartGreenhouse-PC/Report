---
title: Sistema di backend
parent: Design dettagliato
has_children: false
nav_order: 2
---
# Sistema di backend

Una volta individuati i bounded context presenti all'interno del sistema di backend, rappresentato da greenhouse core, è necessario individuare una strategia di integrazione tale per cui i bouded context al suo interno siano il più autonomi possibili; solo in questo modo si possono evitare ad esempio dei rallentamenti o indisponibilità del sistema. A seguito dell'analisi dei requisiti e considerando l'autonomia dei bounded context, si è quindi scelto di realizzare un'architettura a micro-servizi, il quale porta con se diversi vantaggi, l'isolamento dagli errori a singoli componenti del sistema, una maggiore scalabilità, semplicità nel deployment etc.

<div align="center">
<img src="img/exagonal_architecture.png" alt="Architettura esagonale">
<p align="center">[Fig 1] Architettura esagonale</p>
</div>



Come detto precedentemente, nella sezione relativa al Design architetturale, sono stati individuati otto micro-servizi: GreenhouseCommunication, Greenhouse, Operation, Brighness, Humidity, SoilMoisture e Temperature

Ogni micor-servizio, al fine sempre di ricercare la massima autonomia e isolamento della logica di dominio, come suggerito dalle linee guida del DDD, è stato realizzato tramite un'architettura **esagonale**, anche chiamata **port and adapters** (figura 1); il che significa che la logica del dominio che questi servizi possono presentare viene isolata dal resto e resa indipendente dalle diverse tecnologie e interfacce che vengono utilizzate. Tale indipendenza è ottenuta grazie anche al **principio di inversione delle dipendenze** applicato al suo interno, secondo cui un livello può dipendere solamente dai livelli sottostanti, non da quelli al di fuori di lui; di conseguenza, la logica di dominio è il livello più interno della nostra architettura e non deve dipendere da nessuno, il livello applicativo è l'unico che può dipendere da esso, mentre il livello infrastrutturale può dipendere sia dal livello applicativo che da quello del dominio. 

I diversi micro-servizi realizzati, come si può vedere nella figura 2, prevedono tutti caratteristiche simili, ossia:

- espongono una ``API``, che indica le operazioni che possono essere richieste al servizio. Le API sono poi implementate dalla classe ``Model``;
- presentano uno o più ``Adapters`` incaricati di gestire e filtrare la comunicazione con l'esterno a seconda del protocollo di comunicazione utilizzato. Nel nostro caso sono stati impiegati MQTT e HTTP
- racchiudono i diversi ``adapters`` e ``Model`` in una classe ``Service``, il cui compito è installare i diversi adapters e rappresentare il servizio che verrà istanziato.

<div align="center">
<img src="img/classi_service_generale.png" alt="classi service generale">
<p align="center">[Fig 2] Diagramma delle classi: struttura generale di un micro-servizio</p>
</div>



Da quanto detto prima, quindi, possiamo intuire che il micro-servizio potrà svolgere funzionalità sia di **Server**, al fine di poter ricevere ed accogliere le richieste provenienti dai diversi micro-servizi in esecuzione, che di **Client** per poter a sua volta inviare richieste ai micro-servizi attivi, con il quale comunica, in modo da riuscire a portare a termine le sue attività. In particolare la componente Server sarà gestita dalla classe ``Adapter``, mentre la componente Client sarà gestita dalla classe ``Model``.

I micro-servizi: ``Greenhouse``, ``Operation``,``Brightness``, ``Temperature``, ``SoilMoisture`` e ``Humidity``, per riuscire a soddisfare le richieste provenienti dagli altri micro-servizi, interagiscono con un apposito database. Per questo motivo, per questi servizi, oltre alle componenti viste in precedenza, sono state progettate anche le classi che si occupano della persistenza dei dati e delle interazioni con il database.

Per capire meglio la progettazione che è stata fatta possiamo guardare la figura 3, la quale mostra un esempio di come è stata modellata l'interazione con il database per i servizi che si occupano di monitorare un parametro specifico della pianta.

Come si può vedere, per rendere indipendente il ``Model`` rispetto al livello di persistenza dei dati, viene utilizzato un componente ``PlantValueController``, che svolge la funzione di intermediario fra il ``PlantValueDatabase`` e il ``Model``. Grazie a questa struttura è possibile aggiungere un livello di astrazione, infatti se in futuro si decidesse di cambiare il modello della persistenza dei dati, o di avvalersi di altre tecnologie, queste modifiche non andrebbero ad intaccare il ``Model``, garantendo così il suo funzionamento anche in caso di modifiche future al livello dei dati. 

``PlantValue``, in questo caso, rappresenta una delle entità del dominio, che viene utilizzata dal livello della persistenza dei dati per poter fornire i dati nel formato corretto a chi lo richiede. 

In conclusione, possiamo dire che questa struttura, ci consente di tenere separati fra loro i diversi livelli dell'architettura esagonale e di mantenere applicato il principio di inversione delle dipendenze.

<div align="center">
<img src="img/classi_esempio_persistenza.png" alt="classi service DB">
 <p align="center">[Fig 3] Diagramma delle classi: esempio progettazione entità e interazioni con il DB</p>
</div>

## Interazione tra i diversi micro-servizi
