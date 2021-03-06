---
title: 'PCA-basierte Anomalieerkennung: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul für die PCA-basierte Anomalieerkennung zur Erstellung eines Anomalieerkennungsmodells basierend auf der Hauptkomponentenanalyse (PCA) verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 0672b9769feae65c73a6f752a268968a7bad9e4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79504078"
---
# <a name="pca-based-anomaly-detection"></a>PCA-basierte Anomalieerkennung

In diesem Artikel wird beschrieben, wie das Modul für die **PCA-basierte Anomalieerkennung** in Azure Machine Learning-Designer (Vorschau) verwendet wird, um ein Anomalieerkennungsmodell auf der Basis der Hauptkomponentenanalyse (Principal Component Analysis, PCA) zu erstellen.

Dieses Modul hilft Ihnen bei der Erstellung eines Modells in Szenarien, in denen es einfach ist, Trainingsdaten aus einer Klasse abzurufen, z. B. gültige Transaktionen, aber schwierig ist, ausreichende Beispiele für die angestrebten Anomalien zu erhalten. 

So haben Sie z. B. zur Erkennung von betrügerischen Transaktionen sehr oft nicht genügend Beispiele für einen Betrug, um sie zu trainieren, aber Sie verfügen über viele Beispiele für gute Transaktionen. Das Modul für die **PCA-basierte Anomalieerkennung** löst das Problem, indem es verfügbare Merkmale analysiert, um zu bestimmen, was eine „normale“ Klasse ausmacht, und Abstandsmetriken anwendet, um Fälle zu identifizieren, die Anomalien darstellen. So können Sie ein Modell mit vorhandenen unausgeglichenen Daten trainieren.

## <a name="more-about-principal-component-analysis"></a>Weitere Informationen zur Hauptkomponentenanalyse

Die *Hauptkomponentenanalyse*, die häufig mit PCA (Principal Component Analysis) abgekürzt wird, ist eine bewährte Technik beim maschinellen Lernen. PCA wird häufig bei der explorativen Datenanalyse verwendet, da diese Technik die innere Struktur der Daten aufzeigt und die Varianz in den Daten erklärt.

Die PCA analysiert Daten, die mehrere Variablen enthalten. Sie sucht nach Korrelationen zwischen den Variablen und bestimmt die Kombination von Werten, die die Unterschiede in den Ergebnissen am besten erfasst. Diese kombinierten Merkmalswerte werden verwendet, um einen kompakteren Merkmalsraum zu schaffen, der als *Hauptkomponenten* bezeichnet wird.

Bei der Erkennung von Anomalien wird jede neue Eingabe analysiert, und der Algorithmus zur Anomalieerkennung berechnet seine Projektion auf die Eigenvektoren zusammen mit einem normalisierten Rekonstruktionsfehler. Dieser normalisierte Fehler wird als Anomalieergebnis verwendet. Je höher der Fehler, desto anomaler ist die Instanz.

Weitere Informationen zur Funktionsweise von PCA sowie zur Implementierung für die Anomalieerkennung finden Sie diesen Dokumenten:

- [A randomized algorithm for principal component analysis (Randomisierter Algorithmus für die Hauptkomponentenanalyse)](https://arxiv.org/abs/0809.2274). Rokhlin, Szlan und Tygert

- [Finding Structure with Randomness: Probabilistic Algorithms for Constructing Approximate Matrix Decompositions (Suchen nach Strukturen mit Zufälligkeit: Wahrscheinlichkeitsalgorithmen für das Konstruieren angenäherter Matrixzerlegungen)](http://users.cms.caltech.edu/~jtropp/papers/HMT11-Finding-Structure-SIREV.pdf) (PDF-Download). Halko, Martinsson und Tropp

## <a name="how-to-configure-pca-anomaly-detection"></a>Konfigurieren der PCA-Anomalieerkennung

1. Fügen Sie das Modul für die **PCA-basierte Anomalieerkennung** zu Ihrer Pipeline im Designer hinzu. Sie finden dieses Modul in der Kategorie **Anomalieerkennung**.

2. Klicken Sie im rechten Panel des Moduls für die **PCA-basierte Anomalieerkennung** auf die Option **Trainingsmodus**. Geben Sie an, ob Sie das Modell mit einem bestimmten Parametersatz trainieren oder einen Parametersweep verwenden möchten, um die besten Parameter zu finden.

    - **Single Parameter** (Einzelner Parameter):  Wählen Sie diese Option, wenn Sie wissen, wie Sie das Modell konfigurieren möchten, und geben Sie einen bestimmten Satz von Werten als Argumente an.

3. **Number of components to use in PCA (Anzahl der Komponenten zur Verwendung in der PCA)** : Geben Sie die Anzahl der Ausgabemerkmale oder Komponenten an, die Sie ausgeben möchten.

    Die Entscheidung, wie viele Komponenten einzubeziehen sind, ist ein wichtiger Teil des Experimententwurfs mittels PCA. Als allgemeine Richtlinie gilt, dass Sie nicht die gleiche Anzahl von PCA-Komponenten einbeziehen sollten, wie es Variablen gibt. Stattdessen sollten Sie mit einer kleineren Anzahl von Komponenten beginnen und diese erhöhen, bis einige Kriterien erfüllt sind.

    Die besten Ergebnisse werden erzielt, wenn die Anzahl der Ausgabekomponenten **weniger** als die Anzahl der im Dataset verfügbaren Merkmalsspalten beträgt.

4. Geben Sie den Betrag der Überquotierung an, die während des PCA-Trainings in Zufallsreihenfolge durchgeführt werden soll. Bei Problemen mit der Erkennung von Anomalien erschweren unausgeglichene Daten die Anwendung von standardmäßigen PCA-Techniken. Durch die Angabe eines gewissen Betrags an Überquotierung können Sie die Anzahl der Zielinstanzen erhöhen.

    Wenn Sie 1 angeben, wird keine Überquotierung durchgeführt. Wenn Sie einen höheren Wert als 1 angeben, werden zusätzliche Beispiele generiert, die beim Training des Modells verwendet werden.

    Es gibt zwei Optionen, je nachdem, ob Sie einen Parametersweep verwenden oder nicht:

    - **Oversampling parameter for randomized PCA (Parameter für Überquotierung für PCA in Zufallsreihenfolge)** : Geben Sie eine einzelne ganze Zahl ein, die das Verhältnis der Überquotierung der Minderheitsklasse zur normalen Klasse darstellt. (Verfügbar bei Verwendung der Trainingsmethode **Single Parameter (Einzelner Parameter)** ).

    > [!NOTE]
    > Sie können das überquotierte Dataset nicht anzeigen. Weitere Informationen zur Verwendung der Überquotierung bei PCA finden Sie unter [Technische Hinweise](#technical-notes).

5. **Enable input feature mean normalization (Aktivieren der mittleren Normalisierung für Eingabemerkmale)** :   Wählen Sie diese Option aus, um alle Eingabemerkmale auf einen Mittelwert von Null zu normalisieren. Eine Normalisierung oder Skalierung auf Null wird im Allgemeinen für die PCA empfohlen, da das Ziel der PCA darin besteht, die Varianz zwischen den Variablen zu maximieren.

     Diese Option ist standardmäßig aktiviert. Deaktivieren Sie diese Option, wenn die Werte bereits mit einer anderen Methode oder Skalierung normalisiert wurden.

6. Stellen Sie eine Verbindung eines markierten Trainingsdatasets mit einem der Trainingsmodule her:

    - Wenn Sie die Option **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, müssen Sie das Modul [Train Anomaly Detection Model](train-anomaly-detection-model.md) (Modell für die Anomalieerkennung trainieren) verwenden.

7. Übermitteln Sie die Pipeline.

## <a name="results"></a>Ergebnisse

Wenn das Training abgeschlossen ist, können Sie das trainierte Modell entweder speichern oder es mit dem Modul [Score Model](score-model.md) (Modell bewerten) verbinden, um die Anomalieergebnisse vorherzusagen.

Die Auswertung der Ergebnisse eines Modells zur Erkennung von Anomalien erfordert einige zusätzliche Schritte:

1. Stellen Sie sicher, dass eine Ergebnisspalte in beiden Datasets verfügbar ist.

    Wenn Sie versuchen, ein Anomalieerkennungsmodell auszuwerten und den Fehler „Es ist keine Ergebnisspalte in einem bewerteten Dataset für einen Vergleich verfügbar“ erhalten, bedeutet dies, dass Sie ein typisches Auswertungsdataset verwenden, das eine Beschriftungsspalte, aber keine Wahrscheinlichkeitsergebnisse enthält. Sie müssen ein Dataset auswählen, das mit der Schemaausgabe für Anomalieerkennungsmodelle übereinstimmt, die eine Spalte **Scored Labels** (Bewertete Bezeichnungen) und **Scored Probabilities** (Bewertete Wahrscheinlichkeiten) enthält.

2. Stellen Sie sicher, dass die Bezeichnungsspalten markiert sind.

    Manchmal werden die mit der Bezeichnungsspalte verbundenen Metadaten im Pipelinegraph entfernt. Wenn Sie in diesem Fall das Modul [Evaluate Model](evaluate-model.md) (Modell auswerten) verwenden, um die Ergebnisse zweier Anomalienerkennungsmodelle zu vergleichen, erhalten Sie möglicherweise den Fehler „Es gibt keine Bezeichnungsspalte im bewerteten Dataset“ oder „Es ist keine Bezeichnungsspalte im bewerteten Dataset für einen Vergleich verfügbar“.

    Sie können diesen Fehler vermeiden, indem Sie das Modul [Edit Metadata](edit-metadata.md) (Metadaten bearbeiten) vor dem Modul [Evaluate Model](evaluate-model.md) (Modell auswerten) hinzufügen. Verwenden Sie den Spaltenselektor, um die Klassenspalte auszuwählen, und wählen Sie in der Dropdownliste **Felder** die Option **Bezeichnung** aus.

3. Verwenden Sie [Python-Skript ausführen](execute-python-script.md), um die Spaltenkategorien als 1 (positiv, normal) und 0 (negativ, anormal) zu kennzeichnen.

    ````
    label_column_name = 'XXX'
    anomaly_label_category = YY
    dataframe1[label_column_name] = dataframe1[label_column_name].apply(lambda x: 0 if x == anomaly_label_category else 1)
    ````

    
## <a name="technical-notes"></a>Technische Hinweise

Dieser Algorithmus verwendet PCA zur Annäherung an den Teilbereich, der die normale Klasse enthält. Der Teilbereich wird von Eigenvektoren umfasst, die mit den oberen Eigenwerten der Datenkovarianzmatrix verknüpft sind. Für jede neue Eingabe berechnet die Anomalieerkennung zuerst die Projektion auf die Eigenvektoren und dann den normalisierten Rekonstruktionsfehler. Dieser Fehler ist das Anomalieergebnis. Je höher der Fehler, desto anomaler ist die Instanz. Details zur Berechnung des normalen Bereichs finden Sie in Wikipedia: [Hauptkomponentenanalyse (Principal Component Analysis)](https://wikipedia.org/wiki/Principal_component_analysis) 


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 

Eine Liste mit Fehlern, die bei den Designer-Modulen auftreten können, finden Sie unter [Ausnahmen und Fehlercodes für den Designer (Vorschau)](designer-error-codes.md).