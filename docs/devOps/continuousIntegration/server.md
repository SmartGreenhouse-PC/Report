---
title: Server
parent: Continuous integration
grand_parent: Pratiche devOps
has_children: false
nav_order: 2
---

# Server

Il *workflow* realizzato per il Server è composto da tre *job* principali: `build`, `release` e `success`.

Il *job* di `build` è responsabile di compilare e testare il codice su tre sistemi operativi diversi: *Ubuntu*, *macOS* e *Windows*. Durante questa fase viene configurato il database MongoDB utilizzando l'*action* [ankane/setup-mongodb@v1](https://github.com/ankane/setup-mongodb) e caricato il database iniziale per poter effettuare i test. Il codice viene compilato e testato con Gradle e, se tutti i test hanno esito positivo, la fase di `build` viene considerata completata con successo. 

Il *job* di `release` è responsabile della distribuzione del codice compilato e testato. Durante questa fase, il codice viene scaricato, viene configurato Node.js e viene eseguito un comando di rilascio utilizzando la libreria `semantic-release`, che verrà spiegato più in dettaglio nel capitolo successivo sul Version control.

Il *job* `success` viene eseguita solo se le fasi di build e release hanno avuto esito positivo. Durante questa fase, viene eseguito un passaggio che verifica che non vi siano stati errori durante le fasi precedenti. Se tutti i passaggi hanno esito positivo, il workflow viene considerato completato con successo.

<div align="center">
<img src="img/pipeline_server.png", alt="pipeline server", id="fig1">
 <p align="center">[Fig 1] Pipeline implementata per il Server</p>
</div>