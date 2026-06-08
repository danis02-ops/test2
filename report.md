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

Der Prozess verläuft iterativ in drei Hauptphasen:

1. **Anforderungsanalyse**: Erhebung und Formalisierung von Extraktionsregeln auf Basis realer Lehrveranstaltungsdokumente aus verschiedenen Lehrstühlen.
2. **Prototypenentwicklung**: Aufbau eines Low-Code-Prototypen, der die definierten Regeln implementiert und die Kernkomponenten des Agenten integriert.
3. **Nutzertestphase**: Validierung der Ergebnisse durch empirische Tests mit Studierenden im realen Semesterbetrieb, mit anschließender iterativer Verbesserung des Artefakts.

Die Wahl des DSR-Rahmens ist begründet durch den praxisorientierten Charakter der Problemstellung und die Notwendigkeit, das Artefakt sowohl technisch als auch nutzerzentriert zu evaluieren.

---

## 04 Artefaktbeschreibung: Struktur und Begründung

### Artefakttyp

Das entwickelte Artefakt ist ein **Software-Artefakt** in Form eines KI-Prozess-Agenten.

### Struktur

Der Agent besteht aus drei funktional getrennten Modulen:

1. **Parsing-Modul**: Übernimmt die Texterkennung und -vorverarbeitung. Es liest Eingabedokumente (PDF, DOCX, Plaintext) ein und bereitet den Rohtext für die weitere Verarbeitung auf.

2. **Reasoning-Modul (LLM)**: Der Kern des Agenten. Ein Large Language Model identifiziert Termine und Fristen im aufbereiteten Text, löst relative und vage Zeitangaben in konkrete Datumswerte auf und berechnet sinnvolle Vorlaufzeiten für Erinnerungen.

3. **API-Schnittstelle**: Synchronisiert die extrahierten, strukturierten Datenpunkte mit externen Kalender-Apps (z. B. Google Calendar API, CalDAV-kompatible Dienste).

### Begründung

Die Integration eines Large Language Model ist notwendig, um Kontextinformationen in natürlicher Sprache zu verstehen und in präzise, maschinenlesbare Datenpunkte zu übersetzen. Regelbasierte Systeme allein sind nicht in der Lage, die semantische Vielfalt akademischer Formulierungen zuverlässig zu verarbeiten.

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
