# Technologie-Stack Übersicht

## Zusammenfassung
Die Systemarchitektur verwendet einen modernen, Cloud-nativen Technologie-Stack, der für Skalierbarkeit, Zuverlässigkeit und Wartbarkeit ausgelegt ist. Die gewählten Technologien repräsentieren Industriestandards und Best Practices, die eine robuste Enterprise-Performance gewährleisten und gleichzeitig Flexibilität für zukünftiges Wachstum bieten.

## Architekturauswahl

| Komponente | Technologie/Produkt | Begründung |
|------------|-------------------|------------|
| Print2CAD | Print2CAD | - kommerzieller Dienst zur Vektorisierung<br> - angeblich viel Erfahrung in der KI Anwendung<br> - Zeitersparnis und Risikominimierung Planqualität|
| 3D Viewer | xeokit | - Breite Browser-Unterstützung<br>- Optimierte 3D-Rendering-Leistung<br>- Umfassende API für Integration<br>- Unterstützung verschiedener 3D-Formate<br>- Integrierte Annotations- und Sharing-Funktionen<br> [xeokit Browser Demo](https://xeokit.github.io/xeokit-bim-viewer/app/index.html?projectId=OTCConferenceCenter&tab=storeys) |
| BIM Server | BIMserver.org | - Industrietaugliche implementierung<br> - Bauteile und Gebäude<br> -Merge and Match plugins<br> [BIM Server implementierung open source](https://github.com/opensourceBIM/BIMserver) |
| Frontend Framework | Angular | - Robustes TypeScript-basiertes Framework<br>- Umfassende Entwicklungswerkzeuge (Angular CLI)<br>- Modulare Struktur für große Anwendungen<br>- Integrierte State-Management-Lösungen<br>- Starker Enterprise-Fokus |
| API Gateway & Auth | OAuth2-Proxy mit Nginx | - Zentralisierte Authentifizierung<br>- Effizientes Load Balancing<br>- Hohe Performance und Stabilität<br>- Flexible Konfigurationsoptionen<br>- Bewährte Sicherheitsstandards |
| Datenbank | PostgreSQL | - Robuste relationale Datenbank<br>- Exzellente JSON/JSONB-Unterstützung<br>- Hohe Datenkonsistenz<br>- Gute Skalierbarkeit<br>- Umfassende SQL-Funktionalität |
|Workflow Engine|Apache Airflow|- asynchronous pipelines<br> - dynamically scaleable worker pods<br> - graphical web UI to see all parallel workflows and intervene manually|
| Message Broker | Apache Kafka | - Hohe Skalierbarkeit und Durchsatz<br>- Zuverlässige Nachrichtenpersistenz<br>- Bewährte Event-Streaming-Plattform<br>- Event-Sourcing-Unterstützung<br>- Große Community und Enterprise-Support |
|Collaboration Hub|LobeHub|- local db, integrateable<br> - progressive web app<br> - local LLM<br> https://lobehub.com/docs/usage/start|
| Service Mesh | Istio | - Fortgeschrittene Service-Discovery<br>- Verkehrsmanagement<br>- Integrierte Sicherheitsfunktionen<br>- Beobachtbarkeit<br>- Microservices-optimiert |
| Container-Orchestrierung | Kubernetes | - Industriestandard für Container-Orchestrierung<br>- Automatische Skalierung<br>- Selbstheilende Fähigkeiten<br>- Deklarative Konfiguration<br>- Große Community-Unterstützung |
| CI/CD | GitLab CI/CD | - Integrierte Pipeline-Funktionalität<br>- Container-Registry-Integration<br>- Automatisierte Testmöglichkeiten<br>- Infrastructure-as-Code-Unterstützung<br>- Umfassende DevOps-Funktionen |
| Monitoring | Prometheus & Grafana | - De-facto Standard für Kubernetes-Monitoring<br>- Flexible Metrikerfassung<br>- Leistungsstarke Visualisierung<br>- Umfassende Alarmierungsfunktionen<br>- Gute Integration mit anderen Tools |
| API-Dokumentation | Swagger/OpenAPI | - Industriestandard für API-Dokumentation<br>- Automatische Dokumentationsgenerierung<br>- Interaktives API-Testing<br>- Code-Generator-Unterstützung<br>- Einfache Integration |




