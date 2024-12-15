# Service-Schnittstellenarchitektur

Dieses Dokument bietet einen umfassenden Überblick über die Service-Architektur des Systems, einschließlich der Kern-Geschäftsdienste, Gateway- und Identitätsverwaltung sowie Unterstützungsdienste.

## Architektur der Kern-Geschäftsdienste

Diese Schicht behandelt die Hauptfunktionalität des modularen Hausplanungssystems. Sie zeigt, wie verschiedene Dienste zusammenarbeiten, um Hauspläne zu verarbeiten, Konfigurationen zu verwalten, Visualisierungen bereitzustellen und Kosten zu berechnen. Die Architektur verwendet einen Message Broker für ereignisgesteuerte Kommunikation und gewährleistet damit eine lose Kopplung zwischen den Diensten bei gleichzeitiger Datenkonsistenz über die gesamte Plattform.

```plantuml
@startuml Core Business Services Architecture

skinparam component {
    BackgroundColor<<external>> LightGray
    BorderColor<<external>> Gray
}

' External Systems
component "3D Engine\n(xeokit))" <<external>> as ThreeD

' Core Services
component "House Plan Service" as planService #lightblue {
    port "upload" as planUpload
    port "analyze" as planAnalyze
}

component "Planning & Configuration Service" as configService #lightblue {
    port "config" as configPort
    port "modules" as modulesPort
}

component "3D Visualization Service" as vizService #lightblue  {
    port "render" as renderPort
    port "vr" as vrPort
}

component "Cost Estimation Service" as costService #lightblue {
    port "calculate" as calcPort
    port "offer" as offerPort
}

component "Plan Storage Service" as storageService #lightblue{
    port "store" as storePort
    port "version" as versionPort
}

' Infrastructure Components
database "Plan DB" as planDB
database "Version DB" as versionDB
queue "Core Message Broker" as coreBroker

' Connections
ThreeD --> vizService

planService --> planDB : "stores plans"
storageService --> versionDB : "version control"

planService --> coreBroker : "publishes updates"
costService --> coreBroker : "subscribes to changes"

' Service Interactions
planService ..> configService : "provides modules"
configService ..> vizService : "sends configuration"
configService ..> costService : "requests calculation"
storageService ..> planService : "version management"

@enduml
```

## Gateway- und Identitätsarchitektur

Diese Schicht repräsentiert die Sicherheits- und Zugriffssteuerungsschicht des Systems. Sie zeigt, wie das API-Gateway alle eingehenden Anfragen verwaltet und mit dem Identitäts- und Zugriffsverwaltungsdienst integriert wird, um einen sicheren Zugriff auf die Plattform zu gewährleisten. Die Architektur implementiert ein robustes Authentifizierungs- und Autorisierungssystem, das sowohl Web- als auch VR-Schnittstellen unterstützt und gleichzeitig eine zentralisierte Benutzerverwaltung beibehält.

```plantuml
@startuml Gateway and Identity Architecture

' External Systems
component "Web Frontend" as webUI
component "VR Frontend" as vrUI

' API Gateway
component "API Gateway" as gateway #lightblue {
    port "auth" as gwAuth
    port "api" as gwApi
}

' Identity Services
component "Identity & Access Management" as iam #lightblue {
    port "auth" as iamAuth
    port "user-mgmt" as iamUser
}

' Infrastructure Components
database "User DB" as userDB
queue "Identity Message Broker" as identityBroker

' Connections
gateway --> iam : "authenticates"
gateway --> webUI : "serves"
gateway --> vrUI : "serves"

iam --> userDB : "persists"
iam ..> identityBroker : "publishes user events"

webUI --> gateway
vrUI --> gateway

@enduml
```

## Architektur der Unterstützungsdienste

Diese Schicht beschreibt die Architektur der Unterstützungsdienste, die die Zusammenarbeit, Benachrichtigung und Systemintegration innerhalb unserer Plattform ermöglichen. Sie gewährleistet skalierbare, lose gekoppelte Dienste, die sich unabhängig voneinander entwickeln können und dabei robuste Kommunikationskanäle aufrechterhalten.

```plantuml
@startuml Support Services Architecture

skinparam component {
    BackgroundColor<<external>> LightGray
    BorderColor<<external>> Gray
}

' External Systems
component "CAD Systems\n(AutoCAD, Revit)" <<external>> as CAD
component "ERP System" <<external>> as ERP

' Support Services
component "Collaboration Service" as collabService #lightblue {
    port "chat" as chatPort
    port "comments" as commentPort
}

component "Notification Service" as notifyService #lightblue {
    port "email" as emailPort
    port "push" as pushPort
}

component "Integration Service" as integrationService #lightblue {
    port "cad" as cadPort
    port "erp" as erpPort
}

' Infrastructure Components
queue "Support Message Broker" as supportBroker

' Connections
CAD --> integrationService
ERP --> integrationService

integrationService --> supportBroker : "publishes changes"
notifyService --> supportBroker : "subscribes to events"
collabService --> supportBroker : "publishes messages"

collabService ..> notifyService : "triggers notifications"

@enduml
```

### Komponentenbeschreibung

#### Kerndienste
- **House Plan Service**: Verwaltet das Hochladen und die Analyse von Hausplänen
- **Planning & Configuration Service**: Behandelt Modulkonfiguration und Planung
- **3D Visualization Service**: Bietet Rendering- und VR-Visualisierungsfähigkeiten
- **Cost Estimation Service**: Berechnet Kosten und erstellt Angebote
- **Plan Storage Service**: Verwaltet Planspeicherung und Versionierung

#### Gateway- und Identitätsdienste
- **API Gateway**: Zentraler Eingangspunkt für alle Client-Anfragen
- **Identity & Access Management**: Behandelt Authentifizierung und Benutzerverwaltung
- **User DB**: Speichert Benutzerinformationen und Anmeldedaten
- **Identity Message Broker**: Verwaltet benutzerbezogene Ereignisse

#### Unterstützungsdienste
- **Collaboration Service**: Bietet Echtzeit-Chat- und Kommentarfunktionalität
- **Notification Service**: Behandelt E-Mail- und Push-Benachrichtigungen
- **Integration Service**: Verwaltet externe Systemintegrationen
- **Support Message Broker**: Ermöglicht asynchrone Kommunikation zwischen Diensten

### Kommunikationsfluss
1. Alle Client-Anfragen durchlaufen das API-Gateway zur Authentifizierung
2. Kerndienste kommunizieren über den Core Message Broker
3. Unterstützungsdienste nutzen den Support Message Broker für asynchrone Kommunikation
4. Externe Systeme integrieren sich über dedizierte Integrationsdienste
5. Dienste behalten durch ereignisgesteuerte Architektur eine lose Kopplung bei
