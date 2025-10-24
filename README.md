# Projekt Setup und Infrastruktur

![CI Pipeline](https://github.com/MouadBourbian/Modul-324/workflows/CI%20Pipeline/badge.svg)

## Projekt: Ticketsystem

Wir entwickeln ein **Ticketsystem** basierend auf **Spring Boot** als Backend und **MongoDB** als Datenbank.
Das Projekt wird als Web-API umgesetzt, jede User Story beschreibt einen eigenen Endpoint.  
Ein Frontend wird nicht implementiert.

---

## Ordnerstruktur

- [Code](./Code/) – Quellcode des Projekts
- [Dokumentation](./Dokumentation/) – Markdown-Dokumente zur Projekt- und Theorie-Dokumentation
- [Theorie](./Theorie/) – Theorieblöcke und Lerninhalte
- [Zeitlogging](./Zeitlogging/) – Aufzeichnung der täglichen Arbeitszeit

---

## Projekt/Technologie Entscheidung

- **Programmiersprache:** Java
- **Framework:** Spring Boot (REST API, Security, Tests)
- **Datenbank:** MongoDB (Cloud: Atlas / lokal: Docker-Image)
- **Tests:** JUnit & Spring Test

---

## Wahl Infrastruktur

**Git Repository**

- GitHub oder GitLab (inkl. Schreibrechte für Lehrperson)

**CI/CD Prozesse**

- GitHub Actions (Build, Tests, Deployment)
- ✅ **Linting**: ESLint für Code-Qualität und Stil
- ✅ **Testing**: Jest mit MongoDB Service Container
- 📝 **Deployment**: Geplant für zukünftige Implementierung
- 📖 Setup-Anleitung: Siehe [SETUP_INSTRUCTIONS.md](./SETUP_INSTRUCTIONS.md)

**Kanban Board**

- GitHub Projects
- Phasen: _To Do_, _In Progress_, _Review_, _Done_

**Zeit loggen**

- Pro Modultag wird die Zeit erfasst, pro Task aufgeschlüsselt.
- Siehe [Zeitlogging](./Zeitlogging/).

**Lernjournal**

- Mouad: [Mouad Bourbian / Modul 324 – GitLab](https://gitlab.com/Mouad.Bourbian/modul-324-mouad-bourbian)

**Dokumentation**

- Siehe [Dokumentation](./Dokumentation/)

**Theorie**

- Siehe [Theorie](./Theorie/)

**Aufgaben**

- Siehe [Aufgaben](./Aufgaben/)
