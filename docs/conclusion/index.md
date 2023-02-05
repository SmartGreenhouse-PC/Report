---
title: Conclusioni
has_children: false
nav_order: 10
---

# Conclusioni

Si voleva realizzare un'applicazione che consentisse la gestione e il monitoraggio di una serra intelligente, al cui interno fosse coltivata una specifica tipologia di piantagione. Si è concordato con l'utente committente di realizzare due tipologie di applicazioni: un'applicazione Desktop per il monitoraggio della gestione della serra e la verifica dello stato di salute della pianta e un'Applicazione Mobile, che potesse essere utilizzata dall'operatore sul campo e che li consentisse di poter prendere il controllo manuale della gestione della serra in caso di necessità.

Per realizzare questo obiettivo, si è partiti con un'analisi più approfondita dei requisiti del sistema, estraendo la conoscienza del dominio e i termini dell’ubiquitous lenguage da addottare, grazie al processo di kwnoledge crunching; individuando i requisiti: di business, utente, funzionali, non funzionali e implementativi che il progetto richiedeva. Successivamente, sono stati individuati i sottodomini del sistema: sistema di automazione serra, greenhouse core e client e per ognuno di questi sono stati identificati più bounded context.

Dopodichè si è passati alla progettazione e implementazione delle diverse componenti, ed infine, giunti al termine del progetto, il gruppo si ritiene soddisfatto in quanto ha risposto in maniera completa ai requisiti prefissati.

L’adozione della metodologia di sviluppo SCRUM-inspired ha agevolato la realizzazione del progetto e l’adozione di tecnologie per la Continuous Integration ha permesso di verificare su diversi sistemi operativi, ogni qual volta venisse apportata una modifica al codice di produzione, che non fossero state introdotte regressioni nei test.

Riteniamo che la realizzazione di questo progetto abbia accresciuto le nostre competenze e la nostra professionalità in quanto:

- ci ha consentito per la prima volta di lavorare con la strategia Domain Driven Design;
- ci ha permesso di utilizzare e approfondire la strategia DevOps e alcuni strumenti messi a disposizione da GitHub;
- ci ha dato la possibilità di lavorare con un sistema distribuito e di poter utilizzare container Docker;
- necessitava di una programmazione complessa, ma guidata da un processo di sviluppo più sofisticato rispetto a quelli adottati nel percorso triennale;
- ha migliorato le capacità di collaborazione e di coordinazione all'interno del team.

**Sviluppi futuri**
Possibili funzionalità aggiuntive che possono essere realizzate in futuro, in aggiunta a quelle già presenti, grazie alla modularità del sistema, possono essere le seguenti:

- aggiunta di un sistema di notifica capace di informare l'operatore in caso di situazioni particolarmente critiche di allarme;
- introduzione di nuovi sensori ed attuatori capaci di migliorare ulteriormente la gestione della serra;
- inserimento e gestione di più serre, anche dislocate sul territorio;
- introduzione di meccanismi di visione artificiale per monitorare lo stato di salute della coltivazione;
- introduzione di algoritmi di machine learning capaci di anticipare le operazioni correttive da effettuare al fine di evitare situazioni di allarme.