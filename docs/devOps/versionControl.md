---
title: Version control
parent: Pratiche devOps
has_children: false
nav_order: 3
---
# Version control

Per software versioning si intende il rpocesso si assegnare un identificatore univoco a uno stato del software. L'identificativo che viene associato è di norma una sequanza alfanumerica di caratteri separati da punti, _slashes_ o _dashes_.

Nel nostro caso per poter distinguere le diverse versioni del software, si è deciso di seguire le linee guida proposte dl [Semantic Versioning](https://semver.org/). La versione del software, di conseguenza, è rappresentata da tre numeri separati da un punto: MAJOR.MINOR.PATCH.

L'incremento di uno di questi tre numeri viene eseguito attraverso queste regole:

- **MAJOR**, quando viengono effettuate delle modifiche incompatibili con le diverse API prodotte;
- **MINOR**, quando vengono aggiunte delle funzionalità che sono retrocompatibili con versioni precedenti del sistema;
- **PATCH**, quando vengono aggiornati dei bug mantenendo una retrocompatibilità.

Il team, ha inoltre deciso, di promuovere il processo di semantic versioning in modo automatico, sfruttando per questo le action di GitHub. In particolare, i quattro diversi progetti principali che sono stati realzzati: `ArduinoSensor`, `Server`, `ClientDesktop` e `ClientMobile` presentano tutti un branch principale `master` e uno di sviluppo `develop`, nel rispetto dell'ottica git flow, ed è stato possibile impostare le GitHub actions in modo tale che ogni qual volta venisse effettuata l'operazione di `push` sul branch `master`, una nuova versione del software vevenisse rilasciata. La determinazione della versione corretta da associare allo stato del software viene fatta sula base dei commit salvati, questi infatti, sono stati scritti seguendo l'approcio [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/), un commit viene pertanto scritto seguendo quil seguente formato:

```bash
<type>[optional scope]: <description>
```

I tipi di commit che sono stati adottati, nel nostro caso, sono i seguenti: 

- **build**, per cambiamenti che riguardano il build system, oppure
dipendenze esterne;
- **chore**, in caso di cambiamenti che non riguardano il codice di produzione;
- **ci**, per cambiamenti che riguardano la continuous integration;
- **docs**, per cambiamenti che riguardano solo la documentazione;
- **feat**, in caso di aggiunta di una nuova feature;
- **fix**, quando un bug viene corretto;
- **perf**, in caso di cambiamenti del codice effettuati per migliorare le performance;
- **refactor**, in caso di cambiamenti del codice che non riguardano né la correzzione di un bug né l'aggiunta di una nuova feature (e.g. spostare un metodo da una classe ad un’altra);
- **style** per cambiamenti che non impattano sul senso del codice (e.g. rimozione di spazi bianchi, formatting, ecc…);
- **test** per l'aggiunta di nuovi test o correzione di test già esistenti.

Di conseguenza, associando ai commit i tipi precedenti e impostando opportunamente le GitHub actions, è stato possibile far si che il sistema fosse in grado di riconoscere automaticamente quale versione associare al software, sulla base delle operazioni effettuate e documentate.