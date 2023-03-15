---
title: Strumenti utilizzati
parent: Processo di sviluppo
has_children: false
nav_order: 1
---

## Strumenti utilizzati

Per la realizzazione del progetto sono stati utilizzati differenti _tool_ a supporto del processo di sviluppo. Tali strumenti, hanno come obiettivo quello di agevolare gli sviluppatori durante tutta la realizzazione del progetto, cercando di automatizzarne i diversi aspetti.

### Notion
[Notion](https://www.notion.so/) è lo strumento che è stato utilizzato per gestire e organizzare i lavori necessari per la realizzazione del progetto, è un tool multi piattaforma (web, Mac, Windows, iOS, Android) che permette di organizzare attività, prendere note, creare calendari, eventi, _wiki_, _CRM_ e tanto altro in un unica area di lavoro, in maniera modulare e collaborativa.

Ci ha consentito di organizzare le attività per i diversi sprint e di raccogliere in un unico punto tutti i documenti ed elementi utili, necessari per il progetto.

<div align="center">
<img src="img/notion_icon.png" width="100" height="100" alt="icona notion" id="fig1">
 <p align="center">[Fig 1] Notion</p>
</div>

### Egon

[Egon.io](https://egon.io/) è un _lightweight tool_ di supporto al Domain storytelling che dà la possibilità di rappresentare attraverso un linguaggio pittografico le storie raccontate dagli esperti del dominio, durante il processo di knowledge crunching.

<div align="center">
<img src="img/egon_icon.png" width="100" height="100" alt="icona egon" id="fig2">
 <p align="center">[Fig 2] Egon.io</p>
</div>

### Arduino Cloud
[Arduino Cloud](https://cloud.arduino.cc/) è una piattaforma online pensata per rendere semplice la creazione, lo sviluppo e il moitoraggio di progetti IoT. È una piattaforma che permette a chiunque di creare progetti IoT, con un'interfaccia _user friendly_ e una soluzione _all-in-one_ per: la configurazione, la scrittura di codice, il caricamento e la visualizzazione dei dati.

<div align="center">
<img src="img/arduinocloud_icon.png" alt="icona egon" id="fig2">
 <p align="center">[Fig 3] Arduino Cloud</p>
</div>



### GitHub 

[GitHub](https://github.com/) è un servizio di _hosting_ per lo sviluppo del software e gestione del _version-control_ basato su Git. Fornisce: il _distributed version control_ di Git più l'_access control_, il tracciamento dei bug, le richieste di funzionalità software, la gestione delle attività, l'integrazione continua e _wiki_ per ogni progetto.

Per il nostro progetto abbiamo utilizzato: **GitHub** come servizio di hosting per il codice sorgente, le **GitHub actions** per promuovere il processo di _continuous integration_ e le **GitHub pages** per la documentazione e spiegazione del sistema realizzato.

<div align="center">
<img src="img/github_icon.png" width="200" alt="icona github" id="fig3">
 <p align="center">[Fig 3] GitHub</p>
</div>

### Gradle

[Gradle](https://gradle.org/) è un _build automation tool open-source_, JVM-based, che consente di automatizzare la costruzione dei progetti Java; esso rende semplice anche l’impostazione di diverse librerie, che possono essere utilizzate all’interno del progetto, senza la necessità di includere i `.jar`, ma specificando direttamente delle dipendenze da un _repository_ remoto. Gradle consente anche la gestione di un progetto costituito da diversi moduli e delle diverse dipendenze che vi possono essere fra questi.

<div align="center">
<img src="img/gradle_icon.png" width="200" alt="icona gradle" id="fig4">
 <p align="center">[Fig 4] Gradle</p>
</div>

### UnitTest

Per il progetto si è deciso di utilizzare il framework [JUnit 5](https://junit.org/junit5/) per la realizzazione di test automatizzati sul codice Java.

JUnit è un _framework open-source_ che può essere utilizzato per la realizzazione di test per il linguaggio Java, fornisce asserzioni per testare i risultati attesi, annotazioni per identificare i metodi e _test runner_ grafici e testuali; inoltre, mette a disposizione una _suite_ per organizzare e permette la condivisone dei risultati. 

<div align="center">
<img src="img/junit_icon.png" width="200" alt="icona junit" id="fig5">
 <p align="center">[Fig 5] JUnit 5</p>
</div>

### MongoDB

[MongoDB](https://www.mongodb.com/) è un database non relazionale _open source_, in grado di elaborare dati strutturati, semi-strutturati e non strutturati. Si tratta di un database orientato ai documenti che sfrutta un linguaggio di query non strutturato.

MongoDB, quindi, si allontana dalla struttura tradizionale basata su tabelle dei database relazionali in favore di documenti in stile JSON, rendendo l'integrazione di alcuni tipi di dati più facile e veloce.

<div align="center">
<img src="img/mongo_icon.png" width="200" alt="icona gmongodb" id="fig6">
 <p align="center">[Fig 6] MongoDB</p>
</div>

### Vertx 

[Vertx](https://vertx.io/) è un _framework_, per la realizzazione di applicazioni reattive in Java. Offre un modello di programmazione _event driven_ e tutte le sue API sono progettate per lavorare principalmente in modo asincrono.

<div align="center">
<img src="img/vertx_icon.png" width="200" alt="icona vertx" id="fig7">
 <p align="center">[Fig 7] Vertx</p>
</div>

### Docker
[Docker](https://www.docker.com/) è una piattaforma _open-source_ per lo sviluppo, il rilascio e l’esecuzione di applicazioni. Esso consente di separare le applicazioni dall'infrastruttura dell’host, in modo da poter fornire il software rapidamente.

Docker consente di racchiudere un’applicazione in un ambiente isolato chiamato container. È possibile eseguire più container isolati all’interno di uno stesso host. I Container sono _lightweight_ e pensati per fornire solo il necessario ad eseguire l’applicazione che essi contengono.

<div align="center">
<img src="img/docker_icon.png" width="200" alt="icona docker" id="fig8">
<p align="center">[Fig 8] Docker</p>
</div>

### Android Espresso
[Android Espresso](https://developer.android.com/training/testing/espresso) è un _framework_ di test automatizzato per le app Android sviluppate da Google. Ha l'obiettivo di rendere più facile e efficiente la scrittura di unit test e integrazione per le app Android. Questo _framework_, permette agli sviluppatori di scrivere codice di test, che interagiscano con l'applicazione allo stesso modo in cui lo farebbe un utente reale. Espresso è anche noto per essere veloce nell'esecuzione dei test, poiché utilizza direttamente l'emulatore o il dispositivo Android.

<div align="center">
<img src="img/espresso.png" width="150" alt="icona android espresso" id="fig9">
 <p align="center">[Fig 9] Android Espresso</p>
</div>