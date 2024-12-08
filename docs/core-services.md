# Architektur der Kerngeschäftsdienste

## Zusammenfassung
Die Architektur der Kerngeschäftsdienste definiert die grundlegenden Dienste, die die Hauptfunktionalitäten des Systems antreiben. Diese Architektur betont Modularität, Skalierbarkeit und eine klare Trennung der Zuständigkeiten zwischen verschiedenen Geschäftsbereichen.

## Architektonische Entscheidungen

```mermaid
graph TB
    %% Externe Systeme
    Sketchfab["Sketchfab<br/>3D Plattform"]
    style Sketchfab fill:#lightgray,stroke:#gray

    %% Kerndienste
    subgraph planService["Hausplan-Dienst"]
        planUpload["hochladen"]
        planAnalyze["analysieren"]
    end

    subgraph configService["Planungs- und Konfigurationsdienst"]
        configPort["konfig"]
        modulesPort["module"]
    end

    subgraph vizService["3D-Visualisierungsdienst"]
        renderPort["rendern"]
        vrPort["vr"]
    end

    subgraph costService["Kostenschätzungsdienst"]
        calcPort["berechnen"]
        offerPort["angebot"]
    end

    subgraph storageService["Planspeicherungsdienst"]
        storePort["speichern"]
        versionPort["version"]
    end

    %% Infrastrukturkomponenten
    planDB[(Plan DB)]
    versionDB[(Versions DB)]
    coreBroker{{"Kern-Nachrichtenbroker"}}

    %% Verbindungen
    Sketchfab --> vizService
    planService --> planDB
    storageService --> versionDB
    planService --> coreBroker
    costService --> coreBroker

    %% Dienst-Interaktionen
    planService -.-> configService
    configService -.-> vizService
    configService -.-> costService
    storageService -.-> planService
```

## Interne Dienstkomponenten

## Implementierungs- und Testerkenntnisse

### Überblick über die Kerndienste

1. **Hausplan-Dienst**
      - Planhochladen und -verarbeitung
      - Analyse und Validierung
      - Integration mit Konfigurationsdienst
      - Echtzeit-Updates über Nachrichtenbroker

2. **Planungs- und Konfigurationsdienst**
      - Modulverwaltung und -konfiguration
      - Durchsetzung von Planungsregeln
      - Konfigurationsvalidierung
      - Integration mit Visualisierungs- und Kostendiensten

3. **3D-Visualisierungsdienst**
      - Integration mit Sketchfab-Plattform
      - Echtzeit-3D-Modell-Rendering
      - VR/AR-Visualisierungsfähigkeiten
      - Leistungsoptimierung für webbasiertes 3D

4. **Kostenschätzungsdienst**
      - Echtzeitkostenberechnungen
      - Angebotserstellung
      - Preisaktualisierungen über Nachrichtenbroker
      - Konfigurationsbasierte Preisgestaltung

5. **Planspeicherungsdienst**
      - Versionskontrollverwaltung
      - Planspeicherung und -abruf
      - Verlaufsverfolgung
      - Backup und Wiederherstellung

### Infrastrukturkomponenten

1. **Datenbanken**
      - Plan-DB für primäre Speicherung
      - Versions-DB für Änderungsverfolgung
      - Optimiert für jeweilige Anwendungsfälle
      - Backup- und Replikationsstrategien

2. **Nachrichtenbroker**
      - Ereignisgesteuerte Architektur
      - Echtzeit-Updates
      - Dienst-Entkopplung
      - Skalierbare Nachrichtenverarbeitung
