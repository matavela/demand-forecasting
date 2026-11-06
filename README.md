# House Demand Forecasting

**Data-Driven Modeling – Gruppenprojekt zur Analyse und Prognose des Immobilienmarktes in King County, Washington.**

Dieses Projekt untersucht zeitliche Muster und Zusammenhänge zwischen Immobilienangebot, Vermarktungsdauer, Hypothekenzinsen und Baugenehmigungen. Auf dieser Grundlage werden monatliche **Pending Listings** – Immobilienangebote mit vereinbartem Kauf – für einen Horizont von **einem bis zwölf Monaten** prognostiziert.

Das Repository dokumentiert den gesamten Analyseprozess: von der ersten Datenprüfung über explorative Analysen und Datenaufbereitung bis zu Modellvergleich, Bewertung und Fehleranalyse.

## Projektziel

Wir untersuchen, wie gut sich Pending Listings anhand historischer Immobilien- und Wirtschaftsindikatoren vorhersagen lassen. Dabei stehen drei Fragen im Mittelpunkt:

- Welche saisonalen Muster und zeitlichen Zusammenhänge zeigen die Daten?
- Verbessern statistische Modelle und Machine-Learning-Verfahren die Prognosen gegenüber einfachen Referenzmodellen?
- Liefern Baugenehmigungen zusätzliche Informationen für die Vorhersage?

Die Vermarktungsdauer (**Days on Market, DOM**) wird ebenfalls analysiert und als Zielgröße in der Datenaufbereitung berücksichtigt. Die abschließende Modellierung in den Notebooks `05.1` bis `05.3` konzentriert sich auf **Pending Listings**. Diese dienen als Indikator der Marktaktivität; sie messen nicht die gesamte Immobiliennachfrage.

## Datenbasis

Die Quelldateien befinden sich im Ordner [Datensätze](Datensätze/). Die Quellenangaben in der folgenden Tabelle entsprechen der Konfiguration der Analyse-Notebooks.

| Datei | Inhalt | Frequenz | Raumbezug | Quelle laut Notebook |
| --- | --- | --- | --- | --- |
| `MEDDAYONMAR53033.csv` | Mediane Vermarktungsdauer in Tagen | Monatlich | King County | Realtor.com / FRED |
| `PENLISCOU53033.csv` | Pending Listings | Monatlich | King County | Realtor.com / FRED |
| `ACTLISCOU53033.csv` | Aktive Immobilienangebote | Monatlich | King County | Realtor.com / FRED |
| `MORTGAGE30US.csv` | Hypothekenzins für 30-jährige Finanzierung | Wöchentlich | USA | Freddie Mac / FRED |
| `Building_Permits_Seattle_2016_2026.csv` | Genehmigte Wohneinheiten, insgesamt und nach Gebäudekategorie | Monatlich | Seattle–Tacoma–Bellevue | U.S. Census Bureau / Building Permits Survey |

Zusätzlich enthält der Datenordner `kc_house_data.csv`. Diese Datei wird von den sieben hier beschriebenen Analyse- und Modellierungsnotebooks nicht eingelesen.

Für die gemeinsame Monatsbasis verwendet die Datenaufbereitung den Zeitraum **September 2017 bis August 2026**. Wöchentliche Hypothekenzinsen werden zu Monatsmittelwerten aggregiert. Unterschiedliche räumliche Bezüge der Datenquellen sind bei der Interpretation zu berücksichtigen.

## Notebooks und Ausführungsreihenfolge

Die Notebooks liegen unter [Notebook](Notebook/). Sie sollten in der folgenden Reihenfolge und jeweils vollständig von oben nach unten ausgeführt werden.

| Schritt | Notebook | Aufgabe |
| --- | --- | --- |
| 1 | [01_IDA.ipynb](Notebook/01_IDA.ipynb) | Quelldaten, Datentypen, Zeiträume, Duplikate, Fehlwerte und Plausibilität prüfen |
| 2 | [02_EDA.ipynb](Notebook/02_EDA.ipynb) | Rohdaten explorativ untersuchen: Verteilungen, Saisonmuster, zeitversetzte Zusammenhänge und Robustheit |
| 3 | [03_Data_Preparation_A.ipynb](Notebook/03_Data_Preparation_A.ipynb) | Monatsbasis, Lag-Features, Kalendermerkmale, Zielwerte und zeitliche Trainingsfenster erstellen und exportieren |
| 4 | [04_EDA_Aufbereitete_Daten.ipynb](Notebook/04_EDA_Aufbereitete_Daten.ipynb) | Aufbereitete Trainingsdaten, Merkmalsverteilungen und Beziehungen zu zukünftigen Zielwerten untersuchen |
| 5 | [05.1_Training_und_Vorhersagen.ipynb](Notebook/05.1_Training_und_Vorhersagen.ipynb) | 24 Modellkandidaten trainieren und Vorhersagen für die definierten Prognosestarts speichern |
| 6 | [05.2_Modellvergleich_und_Bewertung.ipynb](Notebook/05.2_Modellvergleich_und_Bewertung.ipynb) | Modelle vergleichen, Kandidaten auswählen sowie Stabilität und Zusatznutzen der Baugenehmigungen bewerten |
| 7 | [05.3_Bekannter_Test_und_Fehleranalyse.ipynb](Notebook/05.3_Bekannter_Test_und_Fehleranalyse.ipynb) | Bekannten Testzeitraum auswerten, monatliche Fehler analysieren und Ergebnisse zusammenführen |

Die Modellierungsnotebooks bauen über gespeicherte Dateien aufeinander auf. Insbesondere erzeugt **05.2** die Datei `evaluation_summary.csv`, die **05.3** zur vollständigen Zusammenfassung benötigt.

## Lokale Einrichtung

### 1. Repository herunterladen

```bash
git clone https://github.com/matavela/demand-forecasting.git
cd demand-forecasting
```

Alternativ kann das Repository über GitHub als ZIP heruntergeladen und entpackt werden.

### 2. Python-Umgebung vorbereiten

Eine virtuelle Umgebung trennt die Projektbibliotheken von anderen Python-Projekten:

```bash
python -m venv .venv
```

Aktivierung unter Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

Aktivierung unter macOS oder Linux:

```bash
source .venv/bin/activate
```

Die direkt verwendeten Bibliotheken und JupyterLab installieren:

```bash
python -m pip install numpy pandas matplotlib scikit-learn statsmodels jupyterlab
```

Im Repository ist derzeit keine Datei mit festgeschriebenen Paketversionen enthalten. Das Training speichert die verwendeten Versionen von NumPy, pandas, scikit-learn und statsmodels in `run_protocol.json`.

### 3. Pfade an den eigenen Rechner anpassen

**Vor dem ersten Durchlauf müssen die Pfaddefinitionen in den Notebooks angepasst werden.** Der aktuelle Code enthält persönliche Windows-Pfade und verwendet teilweise Ordnernamen, die von der GitHub-Struktur abweichen.

| Einstellung | Anpassung für dieses Repository |
| --- | --- |
| `project_dir` bzw. `PROJECT_DIR` | Absoluter Pfad zum lokal heruntergeladenen Ordner `demand-forecasting` |
| Rohdaten: `data_dir` bzw. `RAW_DIR` | Projektverzeichnis / `Datensätze`, statt des im Code verwendeten Ordners `daten` |
| Ausgabe der Datenaufbereitung: `OUTPUT_DIR` | Einheitlich Projektverzeichnis / `ergebnisse` / `data_preparation` |
| Eingabe in Notebook 04: `data_dir` | Auf denselben Ordner `ergebnisse/data_preparation` ändern; aktuell steht dort `data_preparation_A` |
| Modellierung: `prepared_data_dir` | Bei Bedarf explizit auf `ergebnisse/data_preparation` setzen |
| Modellierung: `results_dir` | In allen drei Modellierungsnotebooks dasselbe Verzeichnis `ergebnisse/modellierung` verwenden |

Beispiel für eine explizite Konfiguration; den Projektpfad durch den eigenen lokalen Pfad ersetzen:

```python
from pathlib import Path

project_dir = Path("C:/Projekte/demand-forecasting")
data_dir = project_dir / "Datensätze"
results_dir = project_dir / "ergebnisse" / "modellierung"
```

In Notebook 03 heißen die Variablen `PROJECT_DIR`, `RAW_DIR` und `OUTPUT_DIR`. Dort wird `RAW_DIR` in Abschnitt 2 erneut gesetzt; auch diese Zuweisung muss angepasst werden. Die automatische Pfaderkennung in Notebook 03 und 04 prüft auf einen Ordner namens `notebooks`, während der Repository-Ordner `Notebook` heißt. Ein explizit gesetztes Projektverzeichnis vermeidet diese Abweichung.

### 4. Notebooks starten und ausführen

```bash
jupyter lab
```

Die Notebooks in der oben angegebenen Reihenfolge öffnen und jeweils alle Zellen ausführen. In VS Code ist alternativ die Ausführung mit Python- und Jupyter-Unterstützung möglich; dabei die eingerichtete Python-Umgebung als Kernel auswählen.

## Datenaufbereitung und zeitliche Trennung

Die Datenaufbereitung prüft die Quellen auf fehlende Werte, doppelte Zeitpunkte und unplausible Angaben. Das auffällige Anfangssegment der Pending-Reihe wird in der IDA gesondert untersucht; die gemeinsame Modellierungsbasis beginnt im September 2017.

Die Prognosemerkmale umfassen verzögerte Beobachtungen von DOM, Pending Listings, aktiven Angeboten, Hypothekenzinsen und Baugenehmigungen. Sinus und Kosinus bilden den Kalendermonat des zukünftigen Zielwertes ab.

Trainings- und Bewertungsdaten werden zeitlich getrennt. Für die Zielwerte gilt eine **angenommene Veröffentlichungsverzögerung von zwei Monaten**. Ein Zielwert darf erst dann in das Training eingehen, wenn er unter dieser Annahme am Prognosestart verfügbar wäre. Die Ridge-Modelle standardisieren ihre Lag-Features jeweils anhand des aktuellen Trainingsfensters.

Die Rohdaten-EDA betrachtet September 2017 bis August 2025. Die EDA der aufbereiteten Daten verwendet dagegen den Trainingsstand Juli 2023. Diese Analysefenster sind von den späteren Bewertungsfenstern der Modellierung zu unterscheiden.

## Modelle

Notebook 05.1 vergleicht insgesamt **24 Kandidaten**:

| Modellfamilie | Varianten | Anzahl |
| --- | --- | ---: |
| Referenzmodelle | Naive, Seasonal Naive, Seasonal Mean 3Y | 3 |
| Exponentielle Glättung (ETS) | Ohne Trend, mit additivem Trend, mit gedämpftem Trend; jeweils additive Saisonalität | 3 |
| SARIMA | `(1,0,0) × (1,1,0,12)` | 1 |
| Ridge-Regression | Drei Merkmalsgruppen mit jeweils fünf Alpha-Werten: 0,1; 1; 10; 100; 1000 | 15 |
| Gradient Boosting | Merkmalsgruppen Basis und Permits | 2 |

Die Merkmalsgruppen sind:

- **Minimal:** Pending-Lags und Kalendermerkmale.
- **Basis:** alle vorbereiteten Merkmale außer dem Baugenehmigungs-Lag.
- **Permits:** alle vorbereiteten Merkmale einschließlich Baugenehmigungen.

Ridge und Gradient Boosting werden getrennt für jeden Prognosehorizont trainiert. Die Referenz- und Zeitreihenmodelle verwenden die zum Prognosestart verfügbare Pending-Historie.

## Bewertung

| Prüfbereich | Prognosestarts | Fälle je Modell | Zweck |
| --- | --- | ---: | --- |
| Jährliche Validierung | August 2021, 2022, 2023 und 2024 | 48 | Modellauswahl anhand des kleinsten MAE |
| Monatliche Sensitivitätsprüfung | Januar 2023 bis August 2024 | 240 | Ergänzende Prüfung der Stabilität |
| Bekannter Test | August 2025, Zielmonate September 2025 bis August 2026 | 12 | Abschließende Fehleranalyse des bereits bekannten Zeitraums |

Die vorbereiteten Validierungsfenster aus Notebook 03 sind nicht mit allen späteren Prognosestarts identisch. Für die Modellbewertung sind die in Notebook 05.1 definierten und in `run_protocol.json` gespeicherten Starts maßgeblich.

Verwendete Fehlermaße:

| Kennzahl | Bedeutung |
| --- | --- |
| **MAE** | Mittlere absolute Abweichung in Pending Listings; Auswahlkriterium der jährlichen Validierung |
| **MASE** | Absoluter Fehler relativ zur saisonalen Vergleichsskala aus der jeweiligen Trainingshistorie |
| **WAPE** | Summe der absoluten Fehler geteilt durch die Summe der tatsächlichen Werte, in Prozent |
| **Bias** | Mittlerer Fehler: positive Werte bedeuten Überschätzung, negative Werte Unterschätzung |

Als Projektkriterien gelten **WAPE ≤ 12 %** als Ziel und **WAPE ≤ 15 %** als maximale Toleranz. Diese Grenzen beziehen sich auf aggregierte Fehler und garantieren keine maximale Abweichung für einen einzelnen Monat.

### Ergebnisse des gespeicherten Notebook-Laufs

Die im Repository gespeicherten Ausgaben weisen **GB Basis** als ausgewählten Kandidaten aus. Im bekannten Test liegt dessen MAE etwa **8,0 % unter Seasonal Naive**. Unter den dort gezeigten Varianten erreicht **Seasonal Mean 3Y** den kleinsten MAE; die zuvor getroffene Modellauswahl bleibt dennoch unverändert.

Das explorative Fehlerband deckt beim ausgewählten Kandidaten **9 von 12 Monaten (75 %)** ab. Diese Angaben stammen aus den gespeicherten Notebook-Ausgaben und wurden für dieses README nicht neu berechnet. Die vollständigen Kennzahlen werden bei der Ausführung als CSV exportiert.

## Erzeugte Ergebnisse

Die Ausgabedateien werden lokal beim Ausführen der Notebooks erstellt. Die folgende Übersicht verwendet den oben beschriebenen einheitlichen Pfad für die Datenaufbereitung.

| Ordner | Wichtige Ausgaben |
| --- | --- |
| `ergebnisse/data_preparation/` | `monthly_base.csv`, zwölf Dateien `supervised_h01_unscaled.csv` bis `supervised_h12_unscaled.csv`, EDA-Trainingsdaten, Skalierungsparameter, Qualitätsbericht und `preparation_protocol.json` |
| `ergebnisse/eda/` | Grafiken und Tabellen der explorativen Analysen |
| `ergebnisse/modellierung/` | Prognosen, Modellvergleich, Fehlermaße und Protokolle |
| `ergebnisse/modellierung/grafiken/` | Diagramme zu Horizontfehlern, jährlichen Fehlern, WAPE, bekanntem Test und Fehlerband |

Die wichtigsten Dateien der Modellierung sind:

| Datei | Erzeugt durch | Inhalt |
| --- | --- | --- |
| `all_predictions.csv` | 05.1 | Einzelprognosen, tatsächliche Werte und Fehler |
| `run_protocol.json`, `fit_log.json` | 05.1 | Einstellungen, Paketversionen und Trainingsprotokolle |
| `comparison.csv` | 05.2 | Vergleich ausgewählter Modellvarianten |
| `selection.json` | 05.2 | Ausgewählter Modellkandidat |
| `evaluation_summary.csv` | 05.2 | Jährliche Validierung und monatliche Sensitivitätsprüfung |
| `permits_matched_comparison.csv` | 05.2 | Zusatznutzen der Baugenehmigungen auf identischen Prognosefällen |
| `known_test_summary.csv` | 05.3 | Modellkennzahlen für den bekannten Test |
| `known_test_monthly_errors.csv` | 05.3 | Monatliche Fehler des ausgewählten Kandidaten |
| `exploratory_intervals.csv`, `interval_summary.csv` | 05.3 | Explorative Fehlerbänder und beobachtete Abdeckung |
| `final_evaluation_summary.csv` | 05.3 | Gemeinsame Übersicht der drei Prüfbereiche |

## Häufige Probleme

| Problem | Vorgehen |
| --- | --- |
| Eine Rohdatendatei wird nicht gefunden | `data_dir` bzw. `RAW_DIR` prüfen und auf den lokalen Ordner `Datensätze` setzen |
| Notebook 04 findet die vorbereiteten Daten nicht | Notebook 03 vollständig ausführen und die Abweichung `data_preparation_A` korrigieren |
| Notebook 05.1 findet keine oder mehrere vorbereitete Datenordner | `prepared_data_dir` explizit auf den gewünschten Ordner mit allen 13 Eingabedateien setzen |
| `evaluation_summary.csv` fehlt in Notebook 05.3 | Notebook **05.2** einschließlich des letzten Exportblocks ausführen und denselben `results_dir` verwenden |
| Ein importiertes Paket fehlt | Paket in der Python-Umgebung des tatsächlich ausgewählten Notebook-Kernels installieren |

## Grenzen der Untersuchung

- Der Testzeitraum September 2025 bis August 2026 ist bereits bekannt. Seine Auswertung ist **keine unabhängige Bestätigung** der Prognoseleistung.
- Die Veröffentlichungslags sind Arbeitsannahmen. Sie ersetzen keinen Nachweis historischer Datenverfügbarkeit oder die Berücksichtigung späterer Datenrevisionen.
- Zusammenhänge aus der EDA belegen keine kausalen Wirkungen.
- Bei den jährlichen August-Starts sind Prognosehorizont und Zielmonat miteinander verknüpft. Monatliche Prognosefenster überlappen teilweise.
- Das Fehlerband basiert auf dem 80-%-Quantil historischer absoluter Validierungsfehler. Es garantiert keine 80-%-Abdeckung und ist nicht mit der WAPE-Toleranz gleichzusetzen.
- Die Datenquellen beziehen sich auf unterschiedliche geografische Ebenen. Die Ergebnisse sind nicht ohne weitere Prüfung auf andere Immobilienmärkte übertragbar.

## Projektkontext

Dieses Repository entstand als Gruppenprojekt im Bereich **Data-Driven Modeling**. Es dient der nachvollziehbaren Dokumentation der Datenanalyse und des Modellvergleichs. Die Nutzungsbedingungen der ursprünglichen Datenanbieter sind bei einer Weiterverwendung der Daten zu beachten.
