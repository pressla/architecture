# BIM AI System Anforderungsanalyse

Dieses Dokument bietet einen umfassenden Überblick über die BIM AI System-Anforderungen, einschließlich des Anforderungsflusses und der detaillierten Benutzerreise-Analyse.

## Anforderungsfluss

Das folgende Diagramm veranschaulicht den Kernprozessfluss des Systems:

```mermaid
flowchart TD
    %% Style-Definitionen
    classDef mandatory fill:#E6F3FF,stroke:#0066CC
    classDef optional fill:#F0F0F0,stroke:#808080

    %% Start-Knoten
    Start((Start))

    %% Benutzer subgraph
    subgraph Benutzer
        B1[Benutzerregistrierung]
    end

    %% Hausplan-Management subgraph
    subgraph Hausplan-Management
        H1[Hausplan Upload]
        H2[Automatische Analyse]
    end

    %% Planung subgraph
    subgraph Planung
        P1[Manuelle Konfiguration]
        P2[3D Visualisierung]
    end

    %% Angebot subgraph
    subgraph Angebot
        A1[Kostenkalkulation]
    end

    %% Integration & Speicherung subgraph
    subgraph Integration & Speicherung
        I1[Versionierung]
        I2[ERP/CAD Integration]
    end

    %% Kollaboration subgraph
    subgraph Kollaboration
        K1[Benachrichtigungen]
        K2[Team Kommunikation]
    end

    %% Abschluss subgraph
    subgraph Abschluss
        AB1[Angebotserstellung]
    end

    %% End-Knoten
    End((Ende))

    %% Flussverbindungen
    Start --> B1
    B1 --> H1
    H1 --> H2
    H2 --> P1 & P2
    P1 & P2 --> A1
    A1 --> I1 & I2
    I1 & I2 --> K1 & K2
    K1 & K2 --> AB1
    AB1 --> End

    %% Stile anwenden
    class B1,H1,H2,P1,P2,A1,I1,I2,K1,K2,AB1 mandatory
```

## Detaillierte Anforderungsmatrix

| Kategorie | Funktionale Schnittstellen | Frontend-Anforderungen | Nicht-funktionale Anforderungen |
|----------|---------------------|----------------------|---------------------------|
| Benutzerregistrierung & -verwaltung | - Rollen- und Berechtigungssystem (Pflicht)<br>- Benutzerprofilverwaltung (Pflicht) | - Registrierung/Login-Oberfläche<br>- Rollenbasierte Benutzeroberfläche | - Sicherheit: Verschlüsselung, Authentifizierung<br>- DSGVO-Konformität |
| Hausplan-Upload & Analyse | - PDF/CAD-Upload-Schnittstellen (Pflicht)<br>- Planzerlegungsalgorithmus (Pflicht) | - Upload-UI mit Fortschrittsanzeige<br>- Analyseergebnis-Visualisierung | - Optimierte Analysealgorithmen<br>- Zuverlässige Dateiverarbeitung |
| Manuelle Planung & Konfiguration | - Drag-and-Drop-Modulschnittstellen (Optional) | - Interaktive Planungsoberfläche<br>- Echtzeit-Visualisierung | - Intuitive Bedienbarkeit<br>- Skalierbarkeit für komplexe Pläne |
| 3D-Visualisierung & VR | - 3D-Rendering-Engine-APIs (Optional) | - 3D-Vorschauansicht<br>- Virtuelle Hausführungen | - Schnelle Ladezeiten<br>- Zuverlässige Datenverarbeitung |
| Kostenschätzung & Angebote | - Automatische Kostenberechnung (Pflicht) | - Angebotserstellung/Bearbeitung-UI<br>- PDF-Export | - Parallele Angebotsverarbeitung |
| Planspeicherung & -abruf | - Planversionierungs-Schnittstellen (Pflicht) | - Gespeicherte Pläne-Verwaltung UI | - Backup und Wiederherstellung |
| Kollaborationsfunktionen | - Chat/Kommentar-Schnittstellen (Optional) | - Echtzeit-Kommunikations-UI | - Minimale Ausfallzeiten |
| Benachrichtigungssystem | - E-Mail/Push-Benachrichtigungs-Schnittstellen (Optional) | - Benachrichtigungseinstellungen-UI | - Hohe Nachrichtenvolumen-Verarbeitung |
| Integrationen | - CAD-Software-Schnittstellen (Pflicht)<br>- ERP/Produktions-APIs (Pflicht) | - | - Multi-Format-Interoperabilität |
| Sicherheit | - SSL, Auth-Maßnahmen (Pflicht) | - | - DSGVO-Konformität |

## Main Customer Journey

### Persona: Sarah Thompson
- **Rolle**: Senior Bauarchitektin
- **Erfahrung**: 12 Jahre in der Architekturplanung
- **Ziele**: 
    + Effiziente Umwandlung traditioneller Hauspläne in modulare Baukonzepte
    + Zusammenarbeit mit Kunden und Bauteams
    + Sicherstellung von Designgenauigkeit und Compliance

### Detaillierte Schritte

1. **Initialer Zugang & Einrichtung**
    + Registrierung im BIM AI System
    + Einrichtung des Berufsprofils mit Referenzen
    + Erhalt rollenspezifischer Berechtigungen

2. **Projektinitiierung**
    + Upload bestehender CAD/PDF-Hauspläne
    + System analysiert und zerlegt Pläne in Standardmodule
    + Überprüfung der initialen automatischen Analyse

3. **Design-Optimierung**
    - Feinabstimmung der Modulanordnung mittels Drag-and-Drop
    + Anpassung der Spezifikationen basierend auf Kundenanforderungen
    + Nutzung der 3D-Visualisierung zur räumlichen Validierung

4. **Zusammenarbeit & Überprüfung**
    + Teilen virtueller Rundgänge mit Kunden
    + Empfang und Umsetzung von Feedback durch integrierte Kommunikationstools
    + Zusammenarbeit mit Ingenieurteam bei technischen Spezifikationen

5. **Dokumentation & Lieferung**
    + Generierung von Kostenschätzungen basierend auf ausgewählten Modulen
    + Erstellung detaillierter technischer Dokumentation
    + Export finaler Pläne und Spezifikationen

### Customer-journey-Flussdiagramm



```plantuml
@startuml journey
skinparam actorStyle awesome

actor "Bauarchitekt" as arch
participant "BIM AI System" as sys
database "Projektspeicher" as db

arch -> sys: Registrierung & Profilsetup
activate sys
sys -> db: Benutzerdaten speichern
deactivate sys

arch -> sys: Hauspläne hochladen
activate sys
sys -> sys: Pläne analysieren & zerlegen
sys -> db: Projektdaten speichern
sys --> arch: Analyseergebnisse anzeigen
deactivate sys

arch -> sys: Modulkonfiguration ändern
activate sys
sys -> sys: Echtzeit-3D-Rendering
sys --> arch: Visualisierung aktualisieren
deactivate sys

arch -> sys: Mit Stakeholdern teilen
activate sys
sys -> db: Teilbaren Link generieren
sys --> arch: Kollaborationstools
deactivate sys

arch -> sys: Dokumentation generieren
activate sys
sys -> sys: Kosten berechnen
sys -> db: Finales Design speichern
sys --> arch: Liefergegenstände exportieren
deactivate sys

@enduml
```
