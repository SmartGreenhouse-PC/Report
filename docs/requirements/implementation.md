---
title: Requisiti implementativi
parent: Requisiti
has_children: false
nav_order: 5
---

# Requisiti implementativi

I requisiti di implementazione vincolano l’intera fase di realizzazione del sistema, ad esempio richiedendo l’uso di uno specifico linguaggio di programmazione e/o di uno specifico tool software.

Di seguito vengono riportati i requisiti, che sono stati individuati relativamente all’implementazione del sistema che si intende realizzare:

1. le componenti Hardware del sistema saranno programmate con il linguaggio _C++_;
2.  le applicazioni Desktop e Mobile e la componente di gestione della logica saranno sviluppate in _Java 11_;
3.  il testing del sistema, per le componenti programmate in linguaggio _Java_, sarà effettuato utilizzando _Unit_;
4.  per la persistenza dei dati verrà utilizzato un database di tipo non-relazionale;
5.  l’applicazione non deve mai interrompersi qualora si verifichi un errore ma deve, invece, mostrare un messaggio di errore all’utente