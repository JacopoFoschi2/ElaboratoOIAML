# Classificazione di tumori benigni e maligni

Il dataset preso in esame è il Breast Cancer Wisconsin Dataset, raggiungibile sia su Kaggle che su scikit-learn.

- [scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_breast_cancer.html)
- [Kaggle](https://www.kaggle.com/datasets/uciml/breast-cancer-wisconsin-data)

La versione utilizzata è quella di scikit-learn per comodità in quanto precaricata.

## Introduzione

In campo medico il machine learning sta venendo utilizzato in scala sempre maggiore. In questo caso specifico cerchiamo di utilizzare dei modelli che, basandosi sul dataset, riescano a **minimizzare il più possibile i falsi negativi** (cioè classificare come Benigno un tumore che è in realtà Maligno) - l'obiettivo primario è quindi massimizzare la Recall sulla classe maligna, poiché un falso negativo ha un impatto clinico molto più grave di un falso positivo.

## Obiettivi

1. Analizzare il dataset tramite EDA per individuare le feature più rilevanti nella distinzione tra tumori benigni e maligni.
2. Identificare e gestire correttamente i valori anomali (outlier), valutando il loro significato clinico prima di decidere se rimuoverli.
3. Ottimizzare gli iperparametri dei modelli tramite cross-validation e GridSearchCV, con l'obiettivo primario di massimizzare la Recall sulla classe Maligna, per minimizzare il rischio di falsi negativi.
4. Addestrare e confrontare più modelli di classificazione (Logistic Regression, SVM, Random Forest, MLP) su un problema di classificazione binaria.
5. Confrontare i modelli non solo in termini di performance, ma anche di interpretabilità, un aspetto estremamente rilevante in un contesto clinico.
6. Trarre conclusioni motivate sul modello più adatto al problema, giustificando la scelta con i risultati ottenuti.

## Metodologia

- **Analisi esplorativa (EDA)**: abbiamo analizzato distribuzioni, correlazioni tra feature e relazione con il target. Le feature legate a forma e concavità della massa (in particolare le variabili "worst") sono risultate quelle più discriminanti tra le due classi. È emersa, come auspicabile visto che sono feature intrinseche, una forte correlazione tra alcuni gruppi di feature (es. area, perimetro e raggio).
- **Outlier detection**: abbiamo identificato i valori anomali tramite il metodo IQR su tutte le feature. Anziché rimuoverli, abbiamo verificato che fossero concentrati soprattutto nella classe Maligna, coerentemente con l'interpretazione clinica (le forme più irregolari tendono ad appartenere a masse maligne). Per questo motivo abbiamo scelto di **non rimuovere gli outlier**, in quanto rappresentano informazione utile alla classificazione piuttosto che rumore e soprattutto in campo medico è sempre **fortemente sconsigliato** rimuovere outlier che possono invece essere utili per identificare casi estremi.
- **Modellazione**: i dati sono stati suddivisi in train/test (80/20) con split stratificato per mantenere le proporzioni tra le classi. Ogni modello è stato incapsulato in una Pipeline (scaling + classificatore) e valutato tramite cross-validation stratificata a 5 fold, ottimizzando gli iperparametri tramite GridSearchCV nei vari: SVM, Random Forest e MLP.

## Modelli utilizzati

- Logistic Regression
- SVM
- Random Forest
- MLP

## Risultati

Tutte le metriche sono calcolate sulla classe **Maligno**, poiché è la classe di interesse su cui vogliamo minimizzare i falsi negativi, e non sulla media tra le due classi.

| Modello | Precision (Maligno) | Recall (Maligno) | F1 (Maligno) |
|---|---|---|---|
| **Logistic Regression** | 0.98 | 0.98 | 0.98 |
| SVM | 0.93 | 0.98 | 0.95 |
| Random Forest | 0.91 | 0.95 | 0.93 |
| MLP | 0.93 | 0.95 | 0.94 |

Ordine dei modelli per performance sulla recall della classe maligna:

1. **Logistic Regression** e **SVM** - a pari merito (0.98)
2. **Random Forest** e **MLP** - a pari merito (0.95)

Logistic Regression resta comunque il modello migliore in assoluto, avendo anche la Precision e l'F1-Score più alti tra tutti, oltre alla Recall.

> [!NOTE]
> Durante lo svolgimento abbiamo anche provato a creare uno scorer personalizzato, che avevamo chiamato recall_maligno, da sostituire a recall_macro. Anche nei grid search, dove ci aspettavamo risultati differenti, è rimasto tutto invariato. DI conseguenza abbiamo ripristinato recall_macro.

---
**Autori:** Jacopo Foschi, Mosè Barbieri  
**Corso:** Laboratorio di Ottimizzazione, Intelligenza Artificiale e Machine Learning  
**Anno Accademico:** 2025/2026
