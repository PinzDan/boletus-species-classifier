[🇬🇧 English](README.md) | [🇮🇹 Italiano](README.it.md)

# BoletusSpieces — What Type of Boletus Is It? 🍄

Un progetto sperimentale di **classificazione delle immagini** realizzato con Python, fast.ai e PyTorch.

L'obiettivo è esplorare come il transfer learning possa essere utilizzato per distinguere specie di funghi visivamente simili, usando fotografie raccolte dal web.

Questo progetto è stato ispirato dalla prima lezione di [Practical Deep Learning for Coders](https://course.fast.ai/Lessons/lesson1.html).

> **Avvertenza:** questo è un esperimento didattico, non uno strumento affidabile per l'identificazione dei funghi. Le previsioni non devono mai essere utilizzate per stabilire se un fungo sia sicuro da mangiare.

## 1. Panoramica del progetto

Il progetto segue un flusso di lavoro completo di machine learning:

1. Raccogliere fotografie di funghi tramite ricerche automatizzate sul web.
2. Organizzare le immagini in cartelle specifiche per ciascuna specie.
3. Verificare e preparare il dataset.
4. Addestrare un classificatore di immagini utilizzando una rete neurale preaddestrata.
5. Valutare le prestazioni del modello.
6. Effettuare previsioni su immagini mai viste prima.

### Specie supportate

| Specie                | Etichetta della classe |
| --------------------- | ---------------------- |
| *Boletus aereus*      | `Boletus_aereus`       |
| *Boletus edulis*      | `Boletus_edulis`       |
| *Boletus pinophilus*  | `Boletus_pinophilus`   |
| *Boletus reticulatus* | `Boletus_reticulatus`  |

Attualmente il classificatore è limitato a queste quattro specie.

## 2. Tecnologie utilizzate

Il progetto utilizza:

- **Python 3.12** — Linguaggio di programmazione principale
- **fast.ai** — Framework di deep learning
- **PyTorch** — Libreria alla base della rete neurale
- **DDGS** — Ricerca automatizzata delle immagini
- **fastcore** — Utilità per la manipolazione dei dati
- **Matplotlib** — Visualizzazione dei risultati dell'addestramento
- **Jupyter Notebook** — Sperimentazione e documentazione

## 3. Raccolta del dataset

Le immagini vengono raccolte automaticamente utilizzando DDGS.

Per ogni specie di fungo, il programma genera una query di ricerca e recupera gli URL delle immagini. Le fotografie vengono scaricate in cartelle separate usando la funzione `download_images()` di fast.ai.

Le immagini vengono quindi ridimensionate e controllate per individuare eventuali file danneggiati.

**Limiti del dataset:** le ricerche sul web non garantiscono una corretta identificazione delle specie. Etichette errate, fotografie duplicate e qualità non uniforme delle immagini possono influire sulle prestazioni del modello.

Il dataset scaricato non è incluso in questo repository. I diritti d'autore e le licenze delle immagini rimangono di proprietà dei rispettivi titolari.

## 4. Preparazione dei dati

Il dataset viene preparato utilizzando l'API `DataBlock` di fast.ai.

Le etichette vengono assegnate alle immagini in base alla cartella che le contiene.

I dati vengono suddivisi casualmente in:

- **80% training set**
- **20% validation set**

Viene utilizzato un seed casuale fisso (`seed=42`) per rendere riproducibile la suddivisione, purché il dataset rimanga invariato.

Le immagini vengono ridimensionate a 192 × 192 pixel utilizzando il padding per conservarne le proporzioni.

## 5. Addestramento del modello

Il classificatore utilizza **ResNet18**, una rete neurale convoluzionale preaddestrata su un ampio dataset di immagini.

Invece di addestrare la rete da zero, il progetto utilizza il transfer learning per adattare le caratteristiche visive già apprese al problema della classificazione dei funghi.

Il modello viene addestrato tramite il metodo `fine_tune()` di fast.ai, con cinque epoche di fine-tuning.

### Configurazione dell'addestramento

| Parametro               | Valore            |
| ----------------------- | ----------------- |
| Architettura            | ResNet18          |
| Approccio               | Transfer learning |
| Epoche di fine-tuning   | 5                 |
| Risoluzione immagini    | 192 × 192         |
| Dimensione del batch    | 16                |
| Suddivisione validation | 20%               |
| Metrica di valutazione  | Error rate        |

## 6. Risultati iniziali

L'esperimento iniziale ha prodotto i seguenti risultati sul validation set:

| Metrica                 | Risultato  |
| ----------------------- | ---------- |
| Immagini di validazione | 29         |
| Previsioni corrette     | 13         |
| Accuracy                | **44,83%** |
| Error rate              | **55,17%** |
| Validation loss minima  | **1,3796** |

### Osservazioni sull'addestramento

La training loss è diminuita in modo costante, indicando che il modello stava apprendendo schemi dalle immagini di addestramento.

Tuttavia, la validation loss ha smesso di migliorare durante le prime epoche di fine-tuning.

Questo suggerisce un possibile overfitting, anche se sono necessarie ulteriori analisi, dato che il dataset e il validation set sono relativamente piccoli.

### Matrice di confusione

La matrice di confusione fornisce informazioni aggiuntive sulle specie che il modello fatica a distinguere.

Un risultato particolarmente significativo riguarda *Boletus edulis*: nessuna delle sue sei immagini di validazione è stata classificata correttamente.

Questi risultati evidenziano l'importanza di migliorare la qualità del dataset prima di sperimentare architetture più complesse.

## 7. Inferenza del modello

Dopo l'addestramento, il classificatore può effettuare previsioni su una nuova fotografia utilizzando il metodo `learn.predict()` di fast.ai.

La previsione include:

- La specie di fungo prevista.
- L'indice numerico della classe.
- I punteggi di probabilità associati a ciascuna classe.

Questi punteggi non garantiscono che la specie prevista sia corretta.

## 8. Limiti attuali

Questo esperimento presenta diversi limiti importanti:

- Il dataset è stato raccolto tramite ricerche generiche di immagini sul web.
- Le etichette delle specie non sono state verificate sistematicamente.
- Il validation set è piccolo.
- Fotografie duplicate o quasi identiche possono influire sulla valutazione.
- Il modello non è stato valutato su un test set separato e accuratamente selezionato.
- Specie di funghi sconosciute al modello potrebbero essere classificate erroneamente come una delle quattro specie supportate.

## 9. Miglioramenti futuri

I possibili sviluppi includono:

- Ampliare il dataset con fotografie di specie identificate in modo affidabile.
- Rimuovere i duplicati e correggere le etichette.
- Migliorare la suddivisione tra training set e validation set.
- Applicare tecniche di data augmentation.
- Confrontare diverse architetture di reti neurali.
- Misurare precision e recall per le singole specie.
- Esportare il modello addestrato.
- Sviluppare un'interfaccia dimostrativa.

## 10. Conclusioni

Questo progetto rappresenta il mio primo esperimento indipendente di classificazione delle immagini utilizzando fast.ai.

Sebbene i risultati iniziali mostrino che il classificatore necessita di miglioramenti significativi, il progetto mi ha permesso di acquisire esperienza pratica nella preparazione dei dataset, nel transfer learning, nella valutazione dei modelli e nell'analisi degli errori.

L'obiettivo principale non è ottenere immediatamente previsioni perfette, ma comprendere come la qualità dei dati, le scelte di addestramento e i metodi di valutazione influenzino le prestazioni di un modello di deep learning.

---

**Stato del progetto:** `v0.1 — Prototipo sperimentale`
