---
title: Client Desktop
parent: Continuous integration
grand_parent: Pratiche devOps
has_children: false
nav_order: 3
---

# Client Desktop

Il *workflow* realizzato per il Client Desktop risulta molto simile a quella del Server, l'unica cosa che cambia è che non ha bisogno di impostare il database MongoDB.

Anche in questo caso il *workflow* è eseguito ogni volta che viene effettuato una *push*.

Il workflow è diviso in tre *jobs* principali: `build`, `release` e `success`. Il *job* `build` consiste nel compilare il codice ed eseguire i test sui tre sistemi operativi: *Linux*, *macOS* e *Windows*.

Successivamente c'è la fase di `release` per effettuare il rilascio del codice in automatico.

Infine, la fase `success` verifica se ci sono stati errori durante l'esecuzione dei *jobs* precedenti.


<div align="center">
<img src="img/pipeline_desktop.png", alt="pipeline desktop", id="fig1">
 <p align="center">[Fig 1] Pipeline implementata per il Client Desktop</p>
</div>