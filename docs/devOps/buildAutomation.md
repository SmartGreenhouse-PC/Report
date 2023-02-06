---
title: Build automation
parent: Pratiche devOps
has_children: false
nav_order: 1
---
# Build automation

Una volta definiti i bounded context e l'architettura del progetto, si è proseguito andando a definirene la struttura e l'organizzazione. È stata predisposta un'organizzazione di base su GitHub ed è stata chiamata **SmartGreenhouse-22-23**. All'interno di questa sono stati predisposti quattro repository principali:
- **ArduinoSensor**, che si occupa del sotto-progetto relativo all'implementazione del sotto-dominio sistema di automazione;
- **Server**, che si occupa della parte backend del sistema, rappresentando quindi il sotto-dominio greenhouse core;
- **ClientDesktop**, che si occupa di implementare il client desktop, facente parte del sotto-dominio client;
- **ClientMobile**, che si occupa di implementare il client mobile, facente parte anch'esso del sotto-dominio client.

Tutti i repository, ad eccezione di **ArduinoSensor**, sono stati strutturati utilizzando il build tool **Gradle**, il quale è stato pensato per funzionare principalmente su linguaggi _Java like_.

Grazie a Gradle è stato possibile gestire in modo semplice ed efficace la _build automation_ e quindi le dipendenze ed i _plugin_, di ogni progetto, ed eventualmente sotto-progetto, anche grazie all'utilizzo del file di configurazione ``libs.versions.toml``.


## Struttura multi-progetto

Come detto nella sezione relativa all'design, si è pensato di realizzare il sistema di backend adottando un'architettura a micro-servizi. Per riuscire a realizzare questo tipo di architettura si è scelto di adottare la struttura multi-progetto messa a disposizione da gradle, che fosse il più possibile aderente all’analisi DDD effettuata. Pertanto all'interno del _root project_, e repository **Server**, sono stati definiti i seguenti sotto-progetti:

- **brightnessService**, il quale rappresenta il micro-servizio relativo alla storicizzazione e il reperimento dei dati del sensore di luminosità; 
- **clientCommunicationGateway**, il cui compito è quello di rappresentare il micro-servizio incaricato di gestire la comunicazione con i client;
- **common**, rappresenta una modulo che racchiude gli elementi comuni relativi ai parametri di una pianta;
- **greenhouseCommunicationGateway**, il cui compito è quello di rappresentare il micro-servizio incaricato di gestire la comunicazione con il sistema di automazione;
- **greenhouseService**, il quale rappresenta il micro-servizio incaricato della storicizzazione e il reperimento delle informazioni relative alla serra, tra cui la pianta coltivata al suo interno, i range ottimali per i suoi parametri vitali e la modalità attuale di gestione: manuale o automatica. Questo servizio si occupa, inoltre, di analizzare i diversi dati rilevati dai sensori, in modo da individuare eventuali situazioni di allarme e in tal caso stabilire l'operazione correttiva da intraprendere; 
- **humidityService**, il quale rappresenta il micro-servizio relativo alla storicizzazione e il reperimento dei dati del sensore dell'umidità ambientale; 
- **operationService**, il quale rappresenta il micro-servizio relativo all'esecuzione, storicizzazione e il reperimento dei dati riguardanti le operazioni eseguite all'interno della serra; 
- **soilMoistureService**, il quale rappresenta il micro-servizio relativo alla storicizzazione e il reperimento dei dati del sensore dell'umidità del suolo; 
- **temperatureService**, il quale rappresenta il micro-servizio relativo alla storicizzazione e il reperimento dei dati del sensore della temperatura ambientale; 

## Task personalizzati
Per ogni progetto e sottoprogetto sono stati creati dei task personalizzati, inseriti all’interno dei relativi file ``build.gradle.kts``. Per eseguire i task basterà eseguire il seguente comando ``./gradlew nomeTask``, sostituendo **nomeTask** con il nome del task presente nel progetto.

In particolare per ogni progetto sono stati creati:

- un task ``test`` che a partire dalla root del progetto consentirà di lanciare i test di ogni sottoprogetto. Inoltre, aggiungendo al comando il parametro ``--parallel`` tutti i task di test dei sottoprogetti verranno lanciati in parallelo, permettendo di effettuare un test dell’intero progetto molto più velocemente.

- un task ``shadowJar``, il quale permette di generare il jar per il progetto ed ogni sotto-progetto.

- un task ``Jacoco``, il quale permette di generare il report della coverage per il sottoprogetto e al fine di aggregare i diversi report, il task `JacocoAggregatedReport`.
