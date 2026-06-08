# Abschlussbericht: KI-Agent „Deadline- & Aufgabenmeister"

---

## 01 Einleitung: Problem, Motivation und Forschungsziel

### Problem

Studierende verwalten pro Semester durchschnittlich sechs verschiedene Module. Die relevanten Termine – Abgaben, Prüfungen und Präsentationen – liegen dabei meist nur in unstrukturierter Form vor: eingebettet in PDF-Dokumente, Modulhandbücher oder semesterbegleitende Lehrveranstaltungsunterlagen.

### Motivation

Die manuelle Übertragung dieser Informationen in Kalendersysteme ist zeitintensiv und fehleranfällig. Ein einziges administratives Versäumnis – ein übersehener Abgabetermin oder eine falsch notierte Prüfungszeit – kann kritische Auswirkungen auf den Studienerfolg haben. Angesichts der wachsenden Informationsmenge im Studienalltag besteht ein konkreter Bedarf an einer intelligenten Automatisierungslösung.

### Forschungsziel

Ziel dieses Projekts ist die Entwicklung eines KI-basierten Agenten als Artefakt, der Informationen aus unstrukturierten Dokumenten autonom extrahiert, logisch aufbereitet und in digitale Planungstools integriert. Der Agent soll die kognitive Last der Studienorganisation messbar reduzieren und als Brücke zwischen akademischem Informationsträger und persönlichem Zeitmanagement-System fungieren.

---

## 02 Wissensbasis: Stand der Forschung und Forschungslücke

### Bestehende Artefakte

Etablierte Kalender-Software (z. B. Google Calendar, Outlook) und Task-Manager agieren als **passive Speicher**: Sie setzen voraus, dass Daten entweder bereits strukturiert vorliegen oder manuell eingegeben werden. Bestehende Import-Funktionen (z. B. via `.ics`-Dateien) erfordern ebenfalls vorstrukturierte Eingaben und lösen das Ausgangsproblem nicht.

### Forschungslücke

Es mangelt an einer automatisierten Brücke zwischen dem akademischen Informationsträger (Syllabus, Modulhandbuch) und dem persönlichen Zeitmanagement-Tool. Bestehende Automatisierungsansätze scheitern regelmäßig an der Interpretation vager Zeitangaben in natürlicher Sprache – etwa „letzte Juni-Woche", „vor der Klausurphase" oder „zwei Wochen nach Semesterbeginn". Diese semantische Lücke ist der zentrale Anknüpfungspunkt des vorliegenden Projekts.

---

## 03 Methodik: DSR-Prozess und Projektaufbau

Das Projekt folgt dem **Design Science Research (DSR)**-Ansatz, der auf die systematische Entwicklung und Evaluation von IT-Artefakten zur Lösung praxisrelevanter Probleme ausgerichtet ist.

Als technische Umsetzungsplattform wird **N8N** eingesetzt – eine Open-Source-Low-Code-Automatisierungsplattform, die visuelle Workflow-Orchestrierung mit vollständiger programmatischer Erweiterbarkeit verbindet. Diese Wahl ermöglicht eine schnelle Prototypenentwicklung bei gleichzeitiger Anpassungsfähigkeit der Geschäftslogik.

Der Prozess verläuft iterativ in drei Hauptphasen:

1. **Anforderungsanalyse**: Erhebung und Formalisierung von Extraktionsregeln auf Basis realer Lehrveranstaltungsdokumente aus verschiedenen Lehrstühlen. Die Regeln werden direkt als N8N-Workflow-Logik modelliert.
2. **Prototypenentwicklung**: Aufbau des N8N-Workflows mit den Kernknoten Form Trigger, Extract From File, OpenAI Chat, Code-Node und Google Calendar. Der Workflow ist als importierbare JSON-Datei (`n8n-workflow.json`) im Repository verfügbar.
3. **Nutzertestphase**: Validierung der Ergebnisse durch empirische Tests mit Studierenden im realen Semesterbetrieb, mit anschließender iterativer Verbesserung des Artefakts.

Die Wahl des DSR-Rahmens ist begründet durch den praxisorientierten Charakter der Problemstellung und die Notwendigkeit, das Artefakt sowohl technisch als auch nutzerzentriert zu evaluieren.

---

## 04 Artefaktbeschreibung: Struktur und Begründung

### Artefakttyp

Das entwickelte Artefakt ist ein **Software-Artefakt** in Form eines KI-Prozess-Agenten, realisiert als N8N-Workflow (`n8n-workflow.json`).

### Struktur

Der N8N-Workflow besteht aus sieben Knoten, die drei funktionale Schichten abbilden:

**Schicht 1 – Parsing (Texterkennung)**

| N8N-Knoten | Funktion |
|---|---|
| `Form Trigger` | Nimmt das Dokument (PDF/DOCX/TXT) und den Semesterbeginn als Formular-Upload entgegen |
| `Extract From File` | Extrahiert den Rohtext aus der hochgeladenen Datei |

**Schicht 2 – Reasoning (LLM)**

| N8N-Knoten | Funktion |
|---|---|
| `OpenAI` (GPT-4o) | Erhält den Rohtext und den Semesterbeginn, identifiziert alle Termine und gibt ein strukturiertes JSON-Array zurück (Felder: `titel`, `datum`, `typ`, `modul`, `mehrdeutig`, `hinweis`) |
| `Code` | Parst die LLM-Antwort, bereinigt eventuelle Markdown-Wrapper und teilt das Array in einzelne N8N-Items auf |

**Schicht 3 – Integration (API & Human-in-the-Loop)**

| N8N-Knoten | Funktion |
|---|---|
| `IF` | Prüft das Feld `mehrdeutig`: eindeutige Termine gehen in den Kalender-Pfad, unklare in den Rückfrage-Pfad |
| `Google Calendar` | Erstellt einen ganztägigen Kalender-Eintrag mit typ-basierter Farbcodierung (Prüfung = Rot, Abgabe = Orange, Präsentation = Grün) |
| `Telegram` | Sendet bei mehrdeutigen Terminen eine formatierte Rückfrage-Nachricht an den Studierenden (Human-in-the-Loop) |

### Begründung

Die Integration von GPT-4o als zentrales Reasoning-Modul ist notwendig, um Kontextinformationen in natürlicher Sprache zu verstehen und in präzise, maschinenlesbare Datenpunkte zu übersetzen. Regelbasierte Systeme allein sind nicht in der Lage, die semantische Vielfalt akademischer Formulierungen zuverlässig zu verarbeiten. N8N ermöglicht dabei die nahtlose Orchestrierung aller drei Schichten ohne proprietäre Infrastruktur und ist vollständig self-hostbar.

---

## 05 Demonstration und Evaluation: Anwendungsfall und Erkenntnisse

### Anwendungsfall

Als Demonstration dient ein Testlauf mit Dokumenten aus **sechs unterschiedlichen Lehrstühlen**. Die Dokumente variieren in Format, Sprache und Formulierungsstil, um die Robustheit des Agenten unter realen Bedingungen zu prüfen.

### Evaluation

Die Evaluation erfolgt auf zwei Ebenen:

1. **Technischer Check**: Vollständigkeitsprüfung der extrahierten Fristen im Vergleich zum Originaldokument (Precision/Recall). Jeder im Dokument enthaltene Termin wird manuell gelistet und dem Agenten-Output gegenübergestellt.

2. **Nutzerzentrierte Evaluation**: Einbeziehung von Studierenden, die den Agenten im realen Semesterbetrieb testen. Erfasst werden Usability (nach gängigen Heuristiken) und wahrgenommener Nutzwert (Zeitersparnis, Fehlerreduktion, Akzeptanz).

---

## 06 Diskussion: Beitrag und Limitationen

### Wissenschaftlicher Beitrag

Das Projekt liefert empirische Erkenntnisse über die Effizienzsteigerung im akademischen Selbstmanagement durch autonome Prozessagenten. Es demonstriert, wie ein LLM-basiertes Reasoning-Modul die Lücke zwischen unstrukturierter Informationsquelle und strukturiertem Planungswerkzeug schließen kann, und schafft damit eine übertragbare Vorlage für ähnliche Anwendungsfälle in der Bildungsdomäne.

### Limitationen

Die Genauigkeit des Agenten ist direkt abhängig von der **Qualität des Ausgangsdokuments**. Schlecht formatierte, gescannte oder mehrdeutige Dokumente erhöhen die Fehlerquote. Bei unlösbaren Unklarheiten – etwa wenn ein Datum aus dem Dokumentkontext nicht eindeutig ableitbar ist – benötigt das System eine **Fallback-Logik (Human-in-the-Loop)**, um fehlerhafte Kalendereinträge zu vermeiden und das Vertrauen der Nutzenden in das System zu erhalten.

---

## 07 Fazit: Erkenntnisse und nächste Schritte

### Zentrale Erkenntnis

KI-Agenten können die kognitive Last der Studienorganisation signifikant senken. Entscheidend ist dabei, dass das System nicht vollständig autonom operiert, sondern eine klare **Schnittstelle für menschliche Rückfragen** vorhält. Nur durch diese Kombination aus Automatisierung und gezieltem Human-in-the-Loop lässt sich sowohl Effizienz als auch Verlässlichkeit sicherstellen.

### Nächste Schritte

Als unmittelbare Weiterentwicklung ist die **Ausweitung des Agenten auf direkte Interaktion über Messenger-Dienste** (z. B. WhatsApp, Telegram, Microsoft Teams) geplant. Durch den Einsatz eines Konversationsinterfaces können Nutzende unklare Termine direkt im Dialog korrigieren, ohne eine separate Anwendung öffnen zu müssen. Dieser Ansatz senkt die Interaktionshürde weiter und ermöglicht eine natürlichere Integration des Agenten in den studentischen Alltag.
