# Inspectius MCP-Konnektor

**Daten zu Flurstück und Lage für KI-Assistenten — überwiegend aus Daten amtlicher Stellen, für Adressen in Deutschland, wo die Länder sie bereitstellen, mit Quellenangabe.**

Der Inspectius-Konnektor ist ein [MCP-Server](https://modelcontextprotocol.io) (Model Context Protocol). Über ihn fragt ein KI-Assistent wie Claude zu einer Adresse Flurstück, Bodenrichtwert, Bebauungsplan, Gebäude, Standortrisiken und eine Wertindikation ab — überwiegend mit Daten amtlicher Stellen, jeweils mit Quelle und Lizenzvermerk.

| | |
|---|---|
| **Server** | `https://mcp.inspectius.de/mcp` (Streamable HTTP) |
| **Anmeldung** | OAuth 2.1 mit einem freigeschalteten Inspectius-Konto |
| **Werkzeuge** | 23 — Liste in [`docs/werkzeuge.md`](docs/werkzeuge.md) |
| **Claude-Verzeichnis** | als Community-Konnektor gelistet: <https://claude.ai/directory/inspectius> |
| **Dokumentation** | <https://inspectius-re.de/konnektor> |
| **Datenschutz** | <https://inspectius-re.de/rechtliches/datenschutz#konnektor> |
| **Kontakt** | jonaskops@inspectius.de |

> Dieses Repository enthält Dokumentation und Beispiele. Der Quellcode des Servers ist nicht öffentlich.

## Einrichten

**Claude (Web und Desktop):** Einstellungen → Konnektoren → im Verzeichnis nach „Inspectius“ suchen und verbinden — oder als eigenen Konnektor mit der Adresse `https://mcp.inspectius.de/mcp` anlegen. Danach mit dem Inspectius-Konto anmelden.

**Claude Code:**

```bash
claude mcp add --transport http inspectius https://mcp.inspectius.de/mcp
```

Dann in Claude Code `/mcp` aufrufen, „inspectius“ wählen und im Browser anmelden.

**Andere MCP-Clients:** Jeder Client, der entfernte MCP-Server über Streamable HTTP mit OAuth unterstützt, kann sich mit derselben Adresse verbinden.

## Was der Konnektor liefert

- **Flurstück** am Adresspunkt: Gemarkung, Flur, Nummer, Fläche laut Kataster (in Bayern nur als Karte, ohne Nummer und Fläche)
- **Bodenrichtwert** der Zone — Stichtag und Zonenmerkmale, soweit der Landesdienst sie angibt; nicht in jedem Land (wo kein Wert vorliegt, nennt die Antwort die zuständige Stelle)
- **Bebauungsplan**: Plannummer, Rechtsstand, Datum, Link zum Plandokument der Kommune
- **Gebäude** auf dem Flurstück und Grundflächenzahl im Bestand
- **Luftbild** und **Grundstücksblatt** als Bild (Luftbild mit Flurstücksgrenze und Kennzahlen)
- **Standortrisiken** (Hochwasser, Überschwemmungsgebiet, Wasserschutz, Erdbeben, Bergbau, Radon, Grundwasser, Denkmal, Altlasten …), **Umfeld** und **Umgebungslärm**
- **Bebaubarkeit** (rechnerisch): GRZ laut Plan, Bestand, Reserve. Rechnerische Orientierung — maßgeblich sind der amtliche Bebauungsplan und die Bauaufsicht.
- **Wertindikation** auf Basis der ImmoWertV-Regeln, mit Rechenweg. Indikation — kein Gutachten nach § 194 BauGB.
- **Inserat prüfen** per Link, **Meine Orte**, Zuschnitt, Immobilienrichtwert, Mietniveau, Verlauf, Standortvergleich

Welche Themen je Bundesland vorhanden sind: <https://inspectius-re.de/datenabdeckung#ki-assistent>

## Beispiele (echte Antworten vom 18.10.2026)

Alle Beispiele an öffentlichen Gebäuden, gekürzt. Ungekürzte Antworten in [`beispiele/`](beispiele/). Stand der Antworten: 18.10.2026 — Werte ändern sich, maßgeblich ist die aktuelle Abfrage.

**„Welches Flurstück und welcher Bodenrichtwert gilt am Stadthaus Deutz in Köln?“** → `flurstueck_an_adresse`, `bodenrichtwert_an_adresse`

```json
{ "gemarkung": "Deutz", "flur": "33", "flurstueck": "748", "flaeche_m2": 24205,
  "quelle": "ALKIS vereinfacht NRW", "vermerk": "© Geobasis NRW", "lizenz": "dl-de/zero-2-0" }
{ "wert_eur_je_m2": 1030, "stichtag": "2026-01-01", "nutzungsart": "Kerngebiet (MK)",
  "gutachterausschuss": "Der Gutachterausschuss für Grundstückswerte in der Stadt Köln",
  "quelle": "Bodenrichtwerte Geobasis NRW, dl-de/zero-2-0" }
```

**„Welcher Bebauungsplan gilt an der Hochschule Ruhr West in Mülheim?“** → `bebauungsplan_an_adresse`

```json
{ "plannummer": "M_9", "titel": "Hochschule Ruhr West / Duisburger Straße - M 9",
  "planart": "Qualifizierter BPlan (§ 30 Abs. 1 BauGB)", "gueltig_seit": "2012-03-15",
  "textteil_url": "https://geodaten.muelheim-ruhr.de/files/bplantxt/m09.pdf" }
```

**„Welche Risiken gibt es am Hamburger Rathaus?“** → `risiken_an_adresse`

```json
{ "uebersicht": "1× an der Adresse betroffen · 5× nicht betroffen · 4× Angabe · 1× in der Umgebung · 1× nicht angebunden",
  "themen": [ { "thema": "hochwasser", "status": "betroffen",
                "angabe": "Risikogebiet Tideelbe mit Neuwerk (Risiko Hoch) …" } ] }
```

## Grundsätze

- **Quelle in den Antworten.** Die Antworten nennen Quelle und Lizenz des jeweiligen Dienstes — teils auch nicht amtlicher Quellen wie OpenStreetMap.
- **Keine Schätzung statt Lücke.** Liefert ein Land keinen Wert, sagt die Antwort das ausdrücklich.
- **Ausfall ist keine Entwarnung.** Ist ein Landesdienst nicht erreichbar, meldet der Konnektor „Dienst nicht erreichbar“.
- **Nur lesen** — einzige Ausnahme ist die Liste „Meine Orte“ des angemeldeten Kontos. Objektakten gibt der Konnektor nicht frei.
- **Orientierung, kein Auszug.** Maßgeblich sind die amtlichen Auszüge. Die Wertindikation ist kein Verfahren nach § 6 ImmoWertV. Indikation — kein Gutachten nach § 194 BauGB.

## Zugang

Der Konnektor ist in den Inspectius-Paketen für Unternehmen, Kommunen und Zertifizierte Makler enthalten und wird für das jeweilige Konto freigeschaltet. Zugang anfragen: <https://inspectius-re.de/kontakt?paket=Konnektor>

---

## English summary

The Inspectius connector is a remote MCP server (`https://mcp.inspectius.de/mcp`, Streamable HTTP, OAuth) that lets AI assistants such as Claude look up German property data for addresses in Germany, where the state data services provide them — parcel (cadastre), standard land value (Bodenrichtwert), zoning plan (Bebauungsplan), buildings, site risks, and a value indication based on ImmoWertV rules — mostly from public authorities' data services, with source and licence. It is listed as a community connector in the Claude directory. Access requires an Inspectius account enabled for the connector. Documentation (German): <https://inspectius-re.de/konnektor>
Indication — not a valuation report under § 194 BauGB. Orientation only — the official zoning plan and the building authority are authoritative.

---

Claude ist eine Marke von Anthropic. Inspectius ist kein Partner von Anthropic. Der Konnektor wird von Inspectius betrieben, nicht von Anthropic.
Inspectius, Inh. Jörg Jonas-Kops · Inheidener Str. 71 · 60385 Frankfurt am Main · <https://inspectius-re.de>
