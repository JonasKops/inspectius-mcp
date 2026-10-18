# Werkzeuge des Inspectius-Konnektors

Stand: 18.10.2026 · 23 Werkzeuge · Server `https://mcp.inspectius.de/mcp`.
Die verbindlichen Beschreibungen liefert der Server selbst (`tools/list`). Welche Themen in welchem Bundesland vorhanden sind, steht in der [Datenabdeckung](https://inspectius-re.de/datenabdeckung#ki-assistent).

| Werkzeug | Titel | Kurzbeschreibung |
|---|---|---|
| `flurstueck_an_adresse` | Flurstück an einer Adresse | Gemarkung, Flur, Flurstücksnummer, Fläche laut Kataster; in Bayern nur als Karte, ohne Nummer und Fläche |
| `bodenrichtwert_an_adresse` | Bodenrichtwert an einer Adresse | Wert der Zone in €/m²; Stichtag und Zonenmerkmale, soweit der Landesdienst sie angibt; nicht in jedem Land |
| `immobilienrichtwert_an_adresse` | Immobilienrichtwert | in den Ländern, deren Lizenz die Weitergabe erlaubt |
| `bebauungsplan_an_adresse` | Bebauungsplan an einer Adresse | Plannummer, Planart, Rechtsstand, Datum, Link zum Plandokument; Berlin zusätzlich Baunutzungsplan 1960 |
| `bebauungsplan_mit_festsetzung` | Plan mit Festsetzung | Planausschnitt mit Etikett (GRZ, Reserve, Geschosse …), wo vorhanden. Rechnerische Orientierung — maßgeblich sind der amtliche Bebauungsplan und die Bauaufsicht. |
| `bebauungsplan_ausschnitt` | Bebauungsplan-Ausschnitt am Grundstück | Planzeichnung am Grundstück als Bild, mit Zoom |
| `bebauungsplan_textfestsetzungen` | Textliche Festsetzungen | Textteil des Plans abschnittsweise |
| `bebaubarkeit_an_adresse` | Bebaubarkeit (rechnerisch) | GRZ laut Plan, Bestand, Reserve, § 19 Abs. 4 BauNVO; ohne Plan § 34/§ 35 BauGB mit Indiz. Rechnerische Orientierung — maßgeblich sind der amtliche Bebauungsplan und die Bauaufsicht. |
| `gebaeude_auf_flurstueck` | Gebäude auf dem Flurstück | Gebäude und Grundflächenzahl im Bestand |
| `grundstueck_zuschnitt` | Grundstückszuschnitt | Breite, Tiefe, Form, Straßenfront, Eckgrundstück |
| `luftbild_an_adresse` | Luftbild an einer Adresse | Luftbild des Landes |
| `grundstuecksblatt_an_adresse` | Grundstücksblatt als Bild | Luftbild mit Flurstücksgrenze und Kennzahlen |
| `risiken_an_adresse` | Standortrisiken | Hochwasser, Überschwemmungsgebiet, Wasserschutz, Erdbeben, Bergbau, Bodenbewegung, Radon, Grundwasser, Naturschutz, Denkmal, Pegel, Altlasten — je Thema mit Status |
| `umfeld_an_adresse` | Umfeld einer Adresse | u. a. Zensus, Breitband, Mobilfunk, Solar, Wärmeplanung |
| `laerm_an_adresse` | Umgebungslärm | Straße, Schiene, Flug — je nach Kartierung des Landes |
| `mietniveau_an_adresse` | Mietniveau | Zensus 2022 |
| `wertvorschau_an_adresse` | Wertindikation | auf Basis der ImmoWertV-Regeln, mit Rechenschritten. Indikation — kein Gutachten nach § 194 BauGB. |
| `verlauf_an_adresse` | Verlauf an einer Adresse | gespeicherte Messungen je Thema mit Änderungen |
| `standorte_vergleichen` | Standorte vergleichen | 2–5 Adressen nebeneinander |
| `expose_pruefen` | Inserat prüfen | Kennzahlen eines öffentlichen Inserats gegen die Daten an der Adresse |
| `ort_merken` | Ort merken | legt eine Adresse in „Meine Orte“ des Kontos ab |
| `meine_orte` | Meine Orte | gemerkte Orte des Kontos |
| `ort_entfernen` | Ort entfernen | entfernt einen gemerkten Ort |

**Statuswerte bei Risiken und Umfeld:** betroffen · in der Umgebung · geschützt · nicht betroffen (im abgefragten Dienst) · Angabe · nicht angebunden · Dienst nicht erreichbar (keine Entwarnung).
