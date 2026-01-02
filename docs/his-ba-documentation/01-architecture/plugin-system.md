## Purpose and Scope

This document describes the plugin-based architecture that forms the core extensibility mechanism of the HIS Desktop application. It covers plugin discovery, lifecycle management, communication patterns, and organizational structure. The system contains **956 plugins** distributed across seven domain-specific namespaces.

For information about the main application entry point and initialization, see [HIS Desktop Core](../01-architecture/overview.md). For plugin-specific business domain details, see the child pages for [HIS Core Business Plugins](../02-modules/his-desktop/business-plugins.md), [Transaction & Billing](../02-modules/his-desktop/business-plugins.md#transaction-billing), and other specialized plugin categories.

---

## Plugin Discovery and Loading

The HIS Desktop application uses a dynamic plugin discovery mechanism to locate and load plugins at runtime. The plugin system is managed by the `HIS.Desktop.Modules.Plugin` project and the `Inventec.Desktop.Core` framework.

### Plugin Discovery Process

```mermaid
graph TB
    Entry["HIS.Desktop Application Entry"]
    Scanner["Plugin Scanner<br/>(Inventec.Desktop.Core)"]
    Registry["Plugin Registry"]
    Loader["Plugin Loader"]
    Cache["CacheClient<br/>(HIS.Desktop.Library.CacheClient)"]
    
    Entry --> Scanner
    Scanner --> |"Scan HIS/Plugins/"| Registry
    Registry --> |"Metadata"| Cache
    Registry --> Loader
    Loader --> |"Instantiate"| PluginInstance["Plugin Instance"]
    
    subgraph "Plugin Namespaces - 956 Total"
        HIS["HIS.Desktop.Plugins.*<br/>~600 plugins"]
        ACS["ACS.Desktop.Plugins.*<br/>13 plugins"]
        EMR["EMR.Desktop.Plugins.*<br/>16 plugins"]
        LIS["LIS.Desktop.Plugins.*<br/>12 plugins"]
        SAR["SAR.Desktop.Plugins.*<br/>15 plugins"]
        SDA["SDA.Desktop.Plugins.*<br/>14 plugins"]
        TYT["TYT.Desktop.Plugins.*<br/>17 plugins"]
    end
    
    Loader --> HIS
    Loader --> ACS
    Loader --> EMR
    Loader --> LIS
    Loader --> SAR
    Loader --> SDA
    Loader --> TYT
```

**Sources:** `HIS/HIS.Desktop/`, `HIS/HIS.Desktop.Modules.Plugin/`, `HIS/HIS.Desktop.Library.CacheClient/`, `Common/Inventec.Desktop/Inventec.Desktop.Core/`

### Plugin Registration

Each plugin is registered with metadata that describes its capabilities, dependencies, and entry point. The registration process occurs during application startup.

| Component | Responsibility |
|-----------|---------------|
| `Inventec.Desktop.Core` | Plugin discovery engine, scanning plugin assemblies |
| `HIS.Desktop.Modules.Plugin` | HIS-specific plugin management and metadata handling |
| `HIS.Desktop.Library.CacheClient` | Caching plugin metadata and configuration for performance |
| Plugin Assembly | Self-registration via attributes or manifest files |

**Sources:** `Common/Inventec.Desktop/Inventec.Desktop.Core/`, `HIS/HIS.Desktop.Modules.Plugin/`, `HIS/HIS.Desktop.Library.CacheClient/`

---

## Plugin Organization and Structure

Each plugin follows a standardized directory structure to ensure consistency and maintainability across the 956 plugins in the system.

### Standard Plugin Structure

```mermaid
graph TB
    PluginRoot["[PluginName]/<br/>Plugin Root Directory"]
    
    Entry["[PluginName].cs<br/>Entry Point Class"]
    Run["Run/<br/>Execution Logic"]
    ADO["ADO/<br/>Application Data Objects"]
    Base["Base/<br/>Base Classes & Interfaces"]
    Props["Properties/<br/>Assembly Info & Resources"]
    
    PluginRoot --> Entry
    PluginRoot --> Run
    PluginRoot --> ADO
    PluginRoot --> Base
    PluginRoot --> Props
    
    Run --> RunForm["Form Classes"]
    Run --> RunProcessor["Business Logic Processors"]
    Run --> RunValidator["Validation Logic"]
    
    ADO --> ADOModel["Data Transfer Objects"]
    ADO --> ADOSDO["Service Data Objects"]
    
    Base --> BaseForm["Base Form Classes"]
    Base --> BaseInterface["Interfaces"]
```

**Sources:** `HIS/Plugins/HIS.Desktop.Plugins.*/`, `HIS/Plugins/ACS.Desktop.Plugins.*/`, `HIS/Plugins/EMR.Desktop.Plugins.*/`

### Typical Plugin Components

| Component | File/Folder | Purpose |
|-----------|-------------|---------|
| **Entry Point** | [`[PluginName].cs`](../../[PluginName].cs) | Plugin initialization and module interface implementation |
| **Run Folder** | `Run/` | Contains forms, user controls, and business logic |
| **ADO Folder** | `ADO/` | Application Data Objects for internal plugin data transfer |
| **Base Folder** | `Base/` | Base classes and interfaces used within the plugin |
| **Properties** | `Properties/` | Assembly information and resource files |

### Example: Large Plugin Structure

Larger plugins like `AssignPrescriptionPK` (203 files) follow an extended structure:

```
HIS.Desktop.Plugins.AssignPrescriptionPK/
├── AssignPrescriptionPK.cs              (Entry point)
├── Run/
│   ├── frmAssignPrescriptionPK.cs      (Main form)
│   ├── Validation/                      (Input validators)
│   ├── Processor/                       (Business logic)
│   └── UserControl/                     (Embedded controls)
├── ADO/
│   ├── MedicineADO.cs
│   ├── MaterialADO.cs
│   └── PrescriptionADO.cs
├── Base/
│   ├── ResourceMessage.cs
│   └── ResourceLangManager.cs
└── Properties/
```

**Sources:** `HIS/Plugins/HIS.Desktop.Plugins.AssignPrescriptionPK/`, `HIS/Plugins/HIS.Desktop.Plugins.ServiceExecute/`, `HIS/Plugins/HIS.Desktop.Plugins.TreatmentFinish/`

---

## Plugin Lifecycle

Plugins follow a defined lifecycle from discovery through execution to disposal. The lifecycle is managed by the `Inventec.Desktop.Core` framework.

### Lifecycle Stages

```mermaid
stateDiagram-v2
    [*] --> Discovery: Application Startup
    Discovery --> Registration: Plugin Found
    Registration --> Cached: Metadata Stored
    Cached --> Instantiation: User Action/Menu Click
    Instantiation --> Initialization: Constructor Called
    Initialization --> Execution: Run() Method
    Execution --> Active: Form Displayed
    Active --> Execution: User Interaction
    Active --> Disposal: Form Closed
    Disposal --> [*]
    
    note right of Discovery
        Scan plugin assemblies
        Load metadata
    end note
    
    note right of Instantiation
        Create plugin instance
        Inject dependencies
    end note
    
    note right of Execution
        Execute business logic
        Display UI
    end note
```

**Sources:** `Common/Inventec.Desktop/Inventec.Desktop.Core/`, `HIS/HIS.Desktop.Modules.Plugin/`

### Plugin Interface Contract

Plugins implement a module interface that defines their lifecycle methods:

| Method | Stage | Purpose |
|--------|-------|---------|
| Constructor | Instantiation | Initialize plugin instance, receive dependencies |
| `Run()` | Execution | Entry point for plugin logic, typically creates and shows a form |
| `Dispose()` | Disposal | Clean up resources, unsubscribe from events |

### Module Data Flow

```mermaid
sequenceDiagram
    participant User
    participant MainForm as "Main Application"
    participant PluginCore as "Plugin Core"
    participant Plugin as "Plugin Instance"
    participant Form as "Plugin Form"
    
    User->>MainForm: Click Menu/Action
    MainForm->>PluginCore: Request Plugin
    PluginCore->>Plugin: Instantiate(moduleData)
    Plugin->>Plugin: Constructor(moduleData)
    PluginCore->>Plugin: Run()
    Plugin->>Form: Create Form
    Plugin->>Form: Initialize Data
    Form->>User: Display UI
    User->>Form: Interact
    Form->>Plugin: Process Business Logic
    User->>Form: Close
    Form->>Plugin: Trigger Disposal
    Plugin->>PluginCore: Dispose()
```

**Sources:** `Common/Inventec.Desktop/Inventec.Desktop.Core/`, `HIS/Plugins/HIS.Desktop.Plugins.*/`

---

## Inter-Plugin Communication

The plugin architecture supports both tight and loose coupling patterns through two primary communication mechanisms: **DelegateRegister** and **PubSub**.

### Communication Architecture

```mermaid
graph TB
    subgraph "Communication Layer"
        DR["DelegateRegister<br/>(HIS.Desktop.DelegateRegister)"]
        PS["PubSub<br/>(HIS.Desktop.LocalStorage.PubSub)"]
        LS["LocalStorage<br/>(HIS.Desktop.LocalStorage.*)"]
    end
    
    subgraph "Plugin A"
        PA_Logic["Business Logic"]
        PA_Publisher["Event Publisher"]
        PA_Caller["Direct Call"]
    end
    
    subgraph "Plugin B"
        PB_Logic["Business Logic"]
        PB_Subscriber["Event Subscriber"]
        PB_Handler["Delegate Handler"]
    end
    
    PA_Caller --> |"Register Delegate"| DR
    DR --> |"Invoke Delegate"| PB_Handler
    PB_Handler --> PB_Logic
    
    PA_Publisher --> |"Publish Event"| PS
    PS --> |"Notify Subscribers"| PB_Subscriber
    PB_Subscriber --> PB_Logic
    
    PA_Logic --> LS
    PB_Logic --> LS
    
    style DR fill:#f9f9f9
    style PS fill:#f9f9f9
    style LS fill:#f9f9f9
```

**Sources:** `HIS/HIS.Desktop.LocalStorage.PubSub/`, `HIS/HIS.Desktop/`

### DelegateRegister Pattern

The `DelegateRegister` provides direct, synchronous communication between plugins through registered delegates. This pattern is used when one plugin needs to invoke specific functionality in another plugin with immediate response.

**Use Cases:**
- Opening a specific form from another plugin
- Requesting data refresh in another module
- Triggering validation or calculation in another component
- Navigating to a specific record in another plugin

**Communication Flow:**

```mermaid
sequenceDiagram
    participant PluginA as "Plugin A<br/>(Caller)"
    participant Registry as "DelegateRegister"
    participant PluginB as "Plugin B<br/>(Handler)"
    
    Note over PluginB: Initialization
    PluginB->>Registry: RegisterDelegate("OpenPatient", handler)
    
    Note over PluginA: User Action
    PluginA->>Registry: InvokeDelegate("OpenPatient", patientId)
    Registry->>PluginB: Execute handler(patientId)
    PluginB->>PluginB: Open Patient Form
    PluginB->>Registry: Return result
    Registry->>PluginA: Return result
```

**Sources:** `HIS/HIS.Desktop/`, `HIS/Plugins/HIS.Desktop.Plugins.*/`

### PubSub Pattern

The `PubSub` (Publish-Subscribe) pattern provides loose-coupled, asynchronous communication through events. Multiple plugins can subscribe to events without the publisher knowing about subscribers.

**Key Components:**

| Component | File Location | Responsibility |
|-----------|---------------|----------------|
| Publisher | Plugin raising events | Publishes events when state changes occur |
| Subscriber | Plugins interested in events | Subscribes to specific event types |
| Event Bus | `HIS.Desktop.LocalStorage.PubSub` | Routes events from publishers to subscribers |
| Event Types | Various ADO classes | Define event data structures |

**Use Cases:**
- Broadcasting data changes (patient updated, prescription saved)
- Notifying multiple plugins of system events
- Cache invalidation across plugins
- UI refresh triggers

**Communication Flow:**

```mermaid
sequenceDiagram
    participant Pub as "Publisher Plugin"
    participant PubSub as "PubSub Event Bus"
    participant Sub1 as "Subscriber Plugin 1"
    participant Sub2 as "Subscriber Plugin 2"
    participant Sub3 as "Subscriber Plugin 3"
    
    Note over Sub1,Sub3: Initialization
    Sub1->>PubSub: Subscribe("PatientUpdated")
    Sub2->>PubSub: Subscribe("PatientUpdated")
    Sub3->>PubSub: Subscribe("PatientUpdated")
    
    Note over Pub: Business Event
    Pub->>PubSub: Publish("PatientUpdated", patientData)
    
    par Broadcast to All Subscribers
        PubSub->>Sub1: Notify(patientData)
        PubSub->>Sub2: Notify(patientData)
        PubSub->>Sub3: Notify(patientData)
    end
    
    Sub1->>Sub1: Refresh UI
    Sub2->>Sub2: Update Cache
    Sub3->>Sub3: Log Event
```

**Sources:** `HIS/HIS.Desktop.LocalStorage.PubSub/`, `Common/Inventec.Common/Inventec.Common.WSPubSub/`

### LocalStorage Integration

Both communication patterns integrate with the `LocalStorage` system for shared state management:

```mermaid
graph LR
    subgraph "LocalStorage System"
        BackendData["BackendData<br/>(Cache)"]
        Config["ConfigApplication"]
        HisConfig["HisConfig"]
    end
    
    subgraph "Communication"
        DelegateReg["DelegateRegister"]
        PubSub["PubSub"]
    end
    
    subgraph "Plugins"
        PluginA["Plugin A"]
        PluginB["Plugin B"]
        PluginC["Plugin C"]
    end
    
    PluginA --> DelegateReg
    PluginA --> PubSub
    PluginA --> BackendData
    
    PluginB --> DelegateReg
    PluginB --> PubSub
    PluginB --> Config
    
    PluginC --> PubSub
    PluginC --> HisConfig
    
    BackendData -.-> |"Data Change Event"| PubSub
    PubSub -.-> |"Invalidate Cache"| BackendData
```

**Sources:** `HIS/HIS.Desktop.LocalStorage.BackendData/`, `HIS/HIS.Desktop.LocalStorage.ConfigApplication/`, `HIS/HIS.Desktop.LocalStorage.HisConfig/`

---

## Plugin Categories

The 956 plugins are organized into seven domain-specific namespaces, each serving a distinct functional area of the hospital information system.

### Category Overview

| Category | Namespace | Plugin Count | Primary Function | Child Page |
|----------|-----------|--------------|------------------|------------|
| **HIS Core** | `HIS.Desktop.Plugins.*` | ~600 | Core hospital operations: registration, treatment, prescriptions, billing | [#1.1.3.1](../02-modules/his-desktop/business-plugins.md) |
| **Access Control** | `ACS.Desktop.Plugins.*` | 13 | User management, roles, permissions, module access control | [#1.1.3.5](../03-business-domains/administration/access-control.md) |
| **EMR** | `EMR.Desktop.Plugins.*` | 16 | Electronic medical records, digital signatures, approval workflows | [#1.1.3.6](../02-modules/his-desktop/business-plugins.md#emr) |
| **Laboratory** | `LIS.Desktop.Plugins.*` | 12 | Lab sample management, machine integration, test results | [#1.1.3.7](../03-business-domains/laboratory/lis-plugins.md) |
| **Reporting** | `SAR.Desktop.Plugins.*` | 15 | Report templates, custom print types, report configuration | [#1.1.3.8](../02-modules/his-desktop/business-plugins.md#reporting) |
| **System Data** | `SDA.Desktop.Plugins.*` | 14 | Master data administration: locations, ethnic groups, fields | [#1.1.3.10](../03-business-domains/administration/system-data.md) |
| **Commune Health** | `TYT.Desktop.Plugins.*` | 17 | Community health programs: tuberculosis, malaria, maternal care | [#1.1.3.9](../02-modules/his-desktop/business-plugins.md#commune-health) |

### HIS Core Plugin Subcategories

The largest category, `HIS.Desktop.Plugins.*`, is further divided into functional subcategories:

```mermaid
graph TB
    HISCore["HIS Core Plugins<br/>~600 plugins"]
    
    Register["Patient Registration<br/>RegisterV2, Reception, etc.<br/>81-102 files each"]
    Treatment["Treatment & Exam<br/>Treatment, Exam, Tracking<br/>56-101 files each"]
    Prescription["Prescriptions<br/>AssignPrescription*<br/>117-203 files each"]
    Transaction["Billing & Transactions<br/>Transaction*, Invoice*<br/>31-48 files each"]
    Medicine["Medicine & Materials<br/>MedicineType*, ImpMest*, ExpMest*<br/>49-85 files each"]
    PatientCall["Patient Call Systems<br/>CallPatient*<br/>24-52 files each"]
    Library["Helper Libraries<br/>Library.*<br/>43-101 files each"]
    
    HISCore --> Register
    HISCore --> Treatment
    HISCore --> Prescription
    HISCore --> Transaction
    HISCore --> Medicine
    HISCore --> PatientCall
    HISCore --> Library
```

**Sources:** `HIS/Plugins/HIS.Desktop.Plugins.*/`

For detailed documentation of each plugin category, refer to the child pages:
- [HIS Core Business Plugins](../02-modules/his-desktop/business-plugins.md)
- [Transaction & Billing Plugins](../02-modules/his-desktop/business-plugins.md#transaction-billing)
- [Medicine & Material Plugins](../03-business-domains/pharmacy/medicine-material.md)
- [Patient Call & Display Plugins](../03-business-domains/patient-management/patient-call-display.md)
- [Library Helper Plugins](../04-integrations/helper-plugins.md)

---

## Plugin Development Guidelines

### Creating a New Plugin

The typical workflow for developing a new plugin:

```mermaid
graph TB
    Start["Start: Define Plugin Purpose"]
    Structure["Create Plugin Directory Structure"]
    Entry["Implement Entry Point Class"]
    Module["Implement Module Interface"]
    Logic["Develop Business Logic in Run/"]
    ADO["Define Data Objects in ADO/"]
    Register["Register with Plugin System"]
    Test["Test Integration"]
    Deploy["Deploy to HIS/Plugins/"]
    
    Start --> Structure
    Structure --> Entry
    Entry --> Module
    Module --> Logic
    Logic --> ADO
    ADO --> Register
    Register --> Test
    Test --> |"Issues"| Logic
    Test --> |"Success"| Deploy
```

**Sources:** `HIS/Plugins/HIS.Desktop.Plugins.*/`, `Common/Inventec.Desktop/Inventec.Desktop.Core/`

### Plugin Naming Conventions

| Component | Convention | Example |
|-----------|-----------|---------|
| **Namespace** | `[Domain].Desktop.Plugins.[PluginName]` | `HIS.Desktop.Plugins.AssignPrescriptionPK` |
| **Entry Class** | [`[PluginName].cs`](../../[PluginName].cs) | [[`AssignPrescriptionPK.cs`](../../AssignPrescriptionPK.cs)](../../AssignPrescriptionPK.cs) |
| **Main Form** | [`frm[PluginName].cs`](../../frm[PluginName].cs) | [[`frmAssignPrescriptionPK.cs`](../../frmAssignPrescriptionPK.cs)](../../frmAssignPrescriptionPK.cs) |
| **ADO Classes** | [`[Entity]ADO.cs`](../../[Entity]ADO.cs) | [[`MedicineADO.cs`](../../MedicineADO.cs)](../../MedicineADO.cs), [[`PrescriptionADO.cs`](../../PrescriptionADO.cs)](../../PrescriptionADO.cs) |
| **Base Classes** | Descriptive names in Base/ | [[`ResourceMessage.cs`](../../ResourceMessage.cs)](../../ResourceMessage.cs), [[`GlobalConfig.cs`](../../GlobalConfig.cs)](../../GlobalConfig.cs) |

### Plugin Dependencies

Plugins typically depend on the following core components:

| Dependency | Location | Purpose |
|------------|----------|---------|
| `Inventec.Desktop.Core` | `Common/Inventec.Desktop/` | Plugin framework and module interface |
| `HIS.Desktop.Common` | `HIS/HIS.Desktop.Common/` | Shared interfaces and base classes |
| `HIS.Desktop.LocalStorage.*` | `HIS/HIS.Desktop.LocalStorage.*/` | Configuration, caching, and event system |
| `HIS.Desktop.Utility` | `HIS/HIS.Desktop.Utility/` | Helper functions and utilities |
| `HIS.Desktop.ApiConsumer` | `HIS/HIS.Desktop.ApiConsumer/` | Backend API communication |
| `HIS.Desktop.ADO` | `HIS/HIS.Desktop.ADO/` | Shared data models |
| `UC Components` | `UC/HIS.UC.*/` | Reusable user controls |

**Sources:** `HIS/HIS.Desktop/`, `HIS/HIS.Desktop.Common/`, `HIS/HIS.Desktop.Utility/`, `Common/Inventec.Desktop/`

### Best Practices

1. **Separation of Concerns**: Keep UI logic in `Run/` folder, data objects in `ADO/`, and shared utilities in `Base/`
2. **Event-Driven Communication**: Prefer PubSub for loose coupling when plugins don't need immediate response
3. **Delegate Registration**: Use DelegateRegister for direct plugin-to-plugin calls requiring return values
4. **Cache Utilization**: Leverage `LocalStorage.BackendData` for frequently accessed data
5. **Resource Management**: Properly dispose of resources and unsubscribe from events in plugin disposal
6. **User Controls**: Reuse existing UC components from `UC/` instead of creating duplicate functionality

**Sources:** `HIS/Plugins/HIS.Desktop.Plugins.*/`, `HIS/HIS.Desktop.LocalStorage.*/`, `UC/HIS.UC.*/`

---

## Summary

The HIS plugin system provides a flexible, extensible architecture for the hospital information system:

- **956 plugins** organized into 7 domain-specific namespaces
- **Standardized structure** with entry point, Run/, ADO/, Base/, and Properties/ folders
- **Lifecycle management** through Inventec.Desktop.Core framework
- **Dual communication patterns**: DelegateRegister for synchronous calls, PubSub for asynchronous events
- **Integration with LocalStorage** for shared state and configuration
- **Modular design** enabling independent development and deployment of features

For specific plugin category implementations, refer to the child pages listed in the Plugin Categories section above.

**Sources:** `HIS/Plugins/`, `HIS/HIS.Desktop.Modules.Plugin/`, `HIS/HIS.Desktop.LocalStorage.PubSub/`, `Common/Inventec.Desktop/Inventec.Desktop.Core/`