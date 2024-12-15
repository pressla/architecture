# Kubernetes Deployment Architektur

## Zweck

Dieses Dokument beschreibt die Kubernetes Deployment-Architektur für unser Airflow-basiertes Verarbeitungssystem. Die Architektur wurde entwickelt, um eine skalierbare, zuverlässige Aufgabenverarbeitung mit dynamischer Ressourcenzuweisung basierend auf der Arbeitslast zu gewährleisten, organisiert in einem dedizierten Kubernetes Namespace für Workflow-Management.

## Architekturübersicht

```plantuml
!include ../archArchitekturSystem/kubernetes_deployment.puml
```

Das obige Diagramm veranschaulicht unsere Kubernetes Deployment-Architektur innerhalb des "Kubernetes workflows" Namespace, bestehend aus drei Hauptknoten:

### Node Controller: Infrastruktur-Ebene
- **Nginx**: Fungiert als Eingangspunkt, verarbeitet eingehende Anfragen und leitet sie an das Airflow Frontend weiter
- **Rancher**: Bietet Container-Management und Orchestrierungsfunktionen
- **Prometheus**: Überwacht Systemmetriken und Performance
- **SecOps**: Setzt Sicherheitsrichtlinien und Zugriffskontrolle durch

### Node Coordinator: Airflow Kern-Ebene
- **Airflow Frontend**: Weboberfläche für DAG-Verwaltung und Überwachung
- **Airflow Scheduler**: Orchestriert die Aufgabenausführung und verwaltet die Worker-Skalierung
- **MySQL**: Speichert Airflow-Metadaten und Aufgabenstatus
- **DAGs Storage**: Persistenter Speicher für DAG-Definitionen und Aufgabencode

### Node Workers: Worker-Ebene
- **Worker Pods**: Dynamisch skaliertes Replica Set für die Aufgabenausführung

## Dynamischer Skalierungsmechanismus

Das System verwendet dynamische Skalierung durch die Interaktion zwischen dem Airflow Scheduler des Coordinator-Knotens und dem Workers-Knoten:

1. **Task-Queue Überwachung**
   - Airflow Scheduler überwacht kontinuierlich die Aufgabenwarteschlange
   - Analysiert Warteschlangentiefe und Aufgabenprioritäten
   - Bestimmt die erforderliche Worker-Kapazität

2. **Worker Pod Lebenszyklus**
   - Bei Aufgaben in der Warteschlange:
     - Scheduler signalisiert Kubernetes, neue Worker-Pods im Workers-Knoten zu erstellen
     - Kubernetes übernimmt Pod-Platzierung und Ressourcenzuweisung innerhalb des Namespace
   - Nach Aufgabenabschluss:
     - Scheduler identifiziert inaktive Worker
     - Signalisiert Kubernetes, überschüssige Pods kontrolliert zu beenden
     - Ressourcen werden an den Cluster zurückgegeben

3. **Ressourcenoptimierung**
   - Worker-Pods werden mit spezifischen Ressourcenanforderungen/-limits erstellt
   - Kubernetes gewährleistet effiziente Verteilung über den Workers-Knoten
   - Verhindert Ressourcenkonflikte bei maximaler Auslastung

## Modulspezifische Verarbeitung

### Aufgabenverteilung
1. Aufgaben werden über das Airflow Frontend im Coordinator-Knoten eingereicht
2. Scheduler bewertet Aufgabenanforderungen und Warteschlangenstatus
3. Worker-Pods werden im Workers-Knoten dynamisch erstellt basierend auf:
   - Aufgabenpriorität
   - Ressourcenanforderungen
   - Aktuelle Cluster-Kapazität
   - Warteschlangenrückstand

### Scheduler-Intelligenz
- Überwacht Aufgabenausführungsmuster
- Prognostiziert Ressourcenbedarf basierend auf historischen Daten
- Erhält minimalen Worker-Pool für häufige Aufgaben
- Skaliert proaktiv während Spitzenzeiten
- Skaliert graduell herunter zur Vermeidung von Thrashing

### Worker Pod Verwaltung
- Jeder Pod führt jeweils eine Aufgabe aus
- Pods sind vergänglich und zustandslos
- Zustand wird in MySQL-Datenbank innerhalb des Coordinator-Knotens gespeichert
- Aufgabenergebnisse werden im persistenten Speicher abgelegt
- Fehlgeschlagene Aufgaben werden automatisch neu geplant

### Ressourceneffizienz
- Pods fordern nur notwendige Ressourcen an
- Kurzlebige Pods für schnelle Aufgaben
- Langlebige Pods für dauerhafte Arbeitslasten
- Automatische Bereinigung abgeschlossener Pods
- Ressourcenquoten verhindern Namespace-Ressourcenerschöpfung

Diese Architektur gewährleistet eine optimale Ressourcennutzung bei gleichzeitiger Aufrechterhaltung der Systemreaktionsfähigkeit und Zuverlässigkeit. Die dynamische Skalierungsfähigkeit ermöglicht es dem System, unterschiedliche Arbeitslasten effizient zu bewältigen, während Spitzenzeiten hochzuskalieren und in ruhigen Zeiten herunterzuskalieren, um Ressourcenkosten zu minimieren. Dabei wird eine klare Trennung der Zuständigkeiten zwischen Controller-, Coordinator- und Worker-Knoten beibehalten.
