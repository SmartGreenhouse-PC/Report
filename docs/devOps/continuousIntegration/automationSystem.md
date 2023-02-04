---
title: Sistema di automazione
parent: Continuous integration
grand_parent: Pratiche devOps
has_children: false
nav_order: 1
---

# Sistema di automazione

Il Sistema di automazione è composto da due sottoprogetti: uno sulla scheda di Arduino e uno sulla scheda NodeMCU con ESP. Per cui il *workflow* si occupa di compilare il progetto sia sulla scheda Arduino che sulla scheda NodeMCU. 

Il *workflow* è impostato per eseguirsi ogni volta che viene effettuato una *push* al repository e include tre *jobs* principali: `compile-sketch`, `find-tag` e `deploy`.

Il primo *job*, `compile-sketch` si occupa di compilare i due *sketch* `ArduinoProject` e `EspProject` nelle rispettive schede. Per fare questo, utilizza l'*action* [arduino/compile-sketches@v1](https://github.com/arduino/compile-sketches#readme) per la compilazione del progetto.

Il secondo *job*, `find-tag`, verrà eseguito solo se il primo *job* è stato eseguito con successo e se la modifica è stata effettuata sul branch *master*. Questo *job* si occupa di verificare se nel messaggio di commit è presente uno speciale segnalatore -TAG. Nel caso sia presente vuol dire che è stato specificato un tag manualmente, ciò può avvenire qualora si voglia far avanzare la versione *Major* o *Minor* avendo fatto cambiamenti significativi. Qualora non venisse specificato un tag, viene generato in automatico un nuovo tag incrementando l'ultimo numero (*bugfix*) del tag esistente.

Il terzo *job*, `deploy`, verrà attivato solamente se è stato generato correttamente il tag nel *job* precedente. Questo *job* crea una nuova release sul repository di GitHub e la pubblica utilizzando l'*action* [actions/create-release@v1](https://github.com/actions/create-release).

<div align="center">
<img src="img/pipeline_automation.png", alt="pipeline automation system", id="fig1">
 <p align="center">[Fig 1] Pipeline implementata per il sistema di automazione</p>
</div>