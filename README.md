# Deadline- & Aufgabenmeister

Ein KI-basierter Agent, der akademische Termine und Fristen aus unstrukturierten Dokumenten (Syllabi, Modulhandbücher, PDFs) automatisch extrahiert und in digitale Kalender-Apps synchronisiert. Realisiert als **N8N-Workflow** mit GPT-4o als Reasoning-Kern und Google Calendar als Zielintegration. Das Projekt folgt dem Design Science Research (DSR)-Ansatz und wurde entwickelt, um die kognitive Last der Studienorganisation signifikant zu reduzieren.

| Datei | Inhalt |
|---|---|
| [`report.md`](./report.md) | Vollständiger DSR-Abschlussbericht |
| [`n8n-workflow.json`](./n8n-workflow.json) | Importierbarer N8N-Workflow (Form Trigger → GPT-4o → Google Calendar / Telegram) |

## Schnellstart

1. N8N-Instanz starten (Self-hosted oder Cloud)
2. `n8n-workflow.json` über **Import → From File** laden
3. Credentials einrichten: OpenAI API, Google Calendar OAuth2, Telegram Bot
4. Variable `TELEGRAM_CHAT_ID` in den N8N-Einstellungen setzen
5. Workflow aktivieren und Formular-URL aufrufen