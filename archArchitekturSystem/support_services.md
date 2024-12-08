```PlantUML
@startuml Support Services Architecture

skinparam component {
    BackgroundColor<<external>> LightGray
    BorderColor<<external>> Gray
}

' External Systems
component "CAD Systems\n(AutoCAD, Revit)" <<external>> as CAD
component "ERP System" <<external>> as ERP

' Support Services
component "Collaboration Service" as collabService {
    port "chat" as chatPort
    port "comments" as commentPort
}

component "Notification Service" as notifyService {
    port "email" as emailPort
    port "push" as pushPort
}

component "Integration Service" as integrationService {
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