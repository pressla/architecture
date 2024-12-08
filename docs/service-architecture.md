# Service Interface Architecture

This document provides a comprehensive overview of the system's service architecture, including core business services, gateway and identity management, and support services.

## Core Business Services Architecture

This layer handles the main functionality of the modular house planning system. It shows how different services interact to process house plans, manage configurations, provide visualizations, and calculate costs. The architecture uses a message broker for event-driven communication, ensuring loose coupling between services while maintaining data consistency across the platform.

```plantuml
@startuml Core Business Services Architecture

skinparam component {
    BackgroundColor<<external>> LightGray
    BorderColor<<external>> Gray
}

' External Systems
component "3D Engine\n(Unity/Unreal)" <<external>> as ThreeD

' Core Services
component "House Plan Service" as planService {
    port "upload" as planUpload
    port "analyze" as planAnalyze
}

component "Planning & Configuration Service" as configService {
    port "config" as configPort
    port "modules" as modulesPort
}

component "3D Visualization Service" as vizService {
    port "render" as renderPort
    port "vr" as vrPort
}

component "Cost Estimation Service" as costService {
    port "calculate" as calcPort
    port "offer" as offerPort
}

component "Plan Storage Service" as storageService {
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

## Gateway and Identity Architecture

This layer represents the security and access control layer of the system. It shows how the API Gateway manages all incoming requests and integrates with the Identity & Access Management service to ensure secure access to the platform. The architecture implements a robust authentication and authorization system, supporting both web and VR interfaces while maintaining centralized user management.

```plantuml
@startuml Gateway and Identity Architecture

' External Systems
component "Web Frontend" as webUI
component "VR Frontend" as vrUI

' API Gateway
component "API Gateway" as gateway {
    port "auth" as gwAuth
    port "api" as gwApi
}

' Identity Services
component "Identity & Access Management" as iam {
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

## Support Services Architecture

This layer outlines the architecture of the support services that facilitate collaboration, notification, and system integration within our platform. It ensures scalable, loosely coupled services that can evolve independently while maintaining robust communication channels.

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

### Components Description

#### Core Services
- **House Plan Service**: Manages upload and analysis of house plans
- **Planning & Configuration Service**: Handles module configuration and planning
- **3D Visualization Service**: Provides rendering and VR visualization capabilities
- **Cost Estimation Service**: Calculates costs and generates offers
- **Plan Storage Service**: Manages plan storage and versioning

#### Gateway and Identity Services
- **API Gateway**: Central entry point for all client requests
- **Identity & Access Management**: Handles authentication and user management
- **User DB**: Stores user information and credentials
- **Identity Message Broker**: Manages user-related events

#### Support Services
- **Collaboration Service**: Provides real-time chat and comment functionality
- **Notification Service**: Handles email and push notifications
- **Integration Service**: Manages external system integrations
- **Support Message Broker**: Enables asynchronous communication between services

### Communication Flow
1. All client requests go through the API Gateway for authentication
2. Core services communicate through the Core Message Broker
3. Support services use the Support Message Broker for asynchronous communication
4. External systems integrate through dedicated integration services
5. Services maintain loose coupling through event-driven architecture
