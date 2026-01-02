## Purpose and Scope

This document covers the plugins responsible for medicine and material management in the HIS system, located in `HIS/Plugins/`. These plugins handle pharmaceutical inventory operations including medicine type definitions, warehouse import/export transactions, stock management, procurement/bidding processes, and blood product operations.

This page focuses on inventory and pharmacy-related plugins. For prescription and medication administration workflows, see [HIS Core Business Plugins](../../02-modules/his-desktop/business-plugins.md). For billing and payment of medicine sales, see [Transaction & Billing Plugins](../../02-modules/his-desktop/business-plugins.md#transaction-billing). For reusable UI components related to medicine selection, see [Medicine & ICD UCs](#1.3.3).

## Plugin Categories Overview

The medicine and material management subsystem consists of approximately 40+ plugins organized into six functional categories:

```mermaid
graph TB
    subgraph "Medicine & Material Plugin System"
        MasterData["Master Data Plugins<br/>MedicineType<br/>MaterialType<br/>MedicineBean"]
        Import["Import Operations<br/>ImpMest* Plugins<br/>80 files (ImpMestCreate)"]
        Export["Export Operations<br/>ExpMest* Plugins<br/>78 files (ExpMestSaleCreate)"]
        Stock["Stock Management<br/>MediStock* Plugins<br/>49 files (MediStockSummary)"]
        Bid["Procurement & Bidding<br/>Bid* Plugins<br/>47 files (BidCreate)"]
        Blood["Blood Management<br/>Import/Export Blood<br/>Blood Product Tracking"]
    end
    
    subgraph "Supporting Systems"
        UC["HIS.UC.MedicineType<br/>HIS.UC.MaterialType<br/>Selection Controls"]
        API["HIS.Desktop.ApiConsumer<br/>Backend Communication"]
        MPS["MPS Print System<br/>Export Documents<br/>Import Documents"]
    end
    
    MasterData --> Import
    MasterData --> Export
    MasterData --> Bid
    Import --> Stock
    Export --> Stock
    Stock --> MPS
    Import --> MPS
    Export --> MPS
    
    MasterData -.->|Uses| UC
    Import -.->|Uses| UC
    Export -.->|Uses| UC
    Bid -.->|Uses| UC
    
    Import --> API
    Export --> API
    Stock --> API
    Bid --> API
```

**Sources:** [`.devin/wiki.json:90-97`](../../../../.devin/wiki.json#L90-L97)

## Master Data Management Plugins

These plugins manage the fundamental definitions of medicines and materials in the system:

| Plugin Name | File Count | Primary Function | Key Features |
|-------------|-----------|------------------|--------------|
| `MedicineType` | ~40-50 | Medicine catalog management | Type definitions, dosage forms, active ingredients |
| `MaterialType` | ~40-50 | Medical material catalog | Equipment, supplies, consumables |
| `MedicineBean` | ~30-40 | Medicine packaging units | Conversion ratios, packaging hierarchies |
| `MedicinePaty` | ~25-35 | Medicine patient type pricing | Patient type-specific pricing rules |
| `ActiveIngredient` | ~20-30 | Drug ingredient management | Active ingredient tracking |

### Medicine Type Structure

```mermaid
graph LR
    Plugin["HIS.Desktop.Plugins.MedicineType"]
    Run["Run/<br/>frmMedicineType.cs<br/>Main Form"]
    ADO["ADO/<br/>MedicineTypeADO.cs<br/>Data Transfer Objects"]
    Processor["Processor/<br/>Business Logic"]
    Base["Base/<br/>RequestUriStore.cs<br/>API Endpoints"]
    
    Plugin --> Run
    Plugin --> ADO
    Plugin --> Processor
    Plugin --> Base
    
    Run --> UCMedicine["HIS.UC.MedicineType<br/>82 files<br/>Grid & Selection UI"]
    ADO --> APIConsumer["HIS.Desktop.ApiConsumer<br/>REST Calls"]
    
    style Plugin fill:#f9f9f9
    style UCMedicine fill:#e8f5e9
```

**Sources:** [`.devin/wiki.json:225-232`](../../../../.devin/wiki.json#L225-L232, high-level architecture diagrams

## Warehouse Import Operations (ImpMest*)

Import operations handle receiving medicines and materials into warehouse inventory. The `ImpMest*` plugin family manages various import scenarios:

### Major Import Plugins

| Plugin Name | Files | Purpose | Workflow Type |
|-------------|-------|---------|---------------|
| `ImpMestCreate` | 80 | General warehouse import | Purchase orders, donations, transfers |
| `ImpMestEdit` | ~50-60 | Modify import records | Edit quantities, expiry dates |
| `ImpMestView` | ~40-50 | View import history | Search, filter, detail view |
| `ImpMestApproval` | ~35-45 | Import approval workflow | Multi-level approval |
| `ImpMestDeduct` | ~30-40 | Import deductions | Damaged goods, returns |

### ImpMest Data Flow

```mermaid
graph TB
    Supplier["Supplier/Vendor<br/>External Entity"]
    
    CreatePlugin["HIS.Desktop.Plugins.ImpMestCreate<br/>80 files<br/>frmImpMestCreate.cs"]
    
    subgraph "Data Layer"
        ADO["ImpMestCreateADO.cs<br/>Import Request DTO"]
        Validation["Validation Logic<br/>Stock Rules<br/>Expiry Checks"]
    end
    
    subgraph "API Communication"
        Consumer["HIS.Desktop.ApiConsumer<br/>ApiConsumerStore.cs"]
        Backend["Backend API<br/>/api/HisImpMest/Create"]
    end
    
    subgraph "Stock Update"
        MediStock["HIS_MEDI_STOCK<br/>Warehouse Entity"]
        MediStockPeriod["HIS_MEDI_STOCK_PERIOD<br/>Period Balance"]
    end
    
    subgraph "Print Output"
        MPS["MPS.Processor.Mps000074<br/>Import Document Print"]
    end
    
    Supplier -->|"Delivery Note"| CreatePlugin
    CreatePlugin --> ADO
    ADO --> Validation
    Validation --> Consumer
    Consumer --> Backend
    Backend -->|"Update"| MediStock
    Backend -->|"Update"| MediStockPeriod
    CreatePlugin --> MPS
```

### ImpMestCreate Plugin Structure

The `ImpMestCreate` plugin follows the standard plugin architecture with 80 files:

- `HIS.Desktop.Plugins.ImpMestCreate.Run/` - Main form implementation
  - [[`frmImpMestCreate.cs`](../../../frmImpMestCreate.cs)](../../../frmImpMestCreate.cs) - Primary UI form
  - [[`frmImpMestCreate.Designer.cs`](../../../frmImpMestCreate.Designer.cs)](../../../frmImpMestCreate.Designer.cs) - UI designer file
  - Grid configuration and event handlers
- `HIS.Desktop.Plugins.ImpMestCreate.ADO/` - Data objects
  - [[`ImpMestCreateADO.cs`](../../../ImpMestCreateADO.cs)](../../../ImpMestCreateADO.cs) - Import request model
  - [[`MedicineImportADO.cs`](../../../MedicineImportADO.cs)](../../../MedicineImportADO.cs) - Medicine line items
  - [[`MaterialImportADO.cs`](../../../MaterialImportADO.cs)](../../../MaterialImportADO.cs) - Material line items
- `HIS.Desktop.Plugins.ImpMestCreate.Processor/` - Business logic
  - Validation processors
  - Calculation logic (quantities, prices)
- `HIS.Desktop.Plugins.ImpMestCreate.Base/` - Configuration
  - [[`RequestUriStore.cs`](../../../RequestUriStore.cs)](../../../RequestUriStore.cs) - API endpoint definitions
  - [[`ResourceLangManager.cs`](../../../ResourceLangManager.cs)](../../../ResourceLangManager.cs) - Localization keys

**Sources:** [`.devin/wiki.json:90-97`](../../../../.devin/wiki.json#L90-L97)

## Warehouse Export Operations (ExpMest*)

Export operations handle dispensing medicines and materials from warehouse inventory. The `ExpMest*` plugin family manages various export scenarios:

### Major Export Plugins

| Plugin Name | Files | Purpose | Workflow Type |
|-------------|-------|---------|---------------|
| `ExpMestSaleCreate` | 78 | Direct pharmacy sales | Over-the-counter sales |
| `ExpMestDepaCreate` | ~60-70 | Department requisitions | Ward/department stock requests |
| `ExpMestChmsCreate` | ~55-65 | Consumables export | Medical supplies, equipment |
| `ExpMestActualCreate` | ~50-60 | Actual dispensing | Fulfill prescriptions |
| `ExpMestEdit` | ~45-55 | Modify export records | Quantity adjustments |

### ExpMestSaleCreate Flow

```mermaid
graph TB
    Patient["Patient/Customer<br/>Walk-in Purchase"]
    
    SalePlugin["HIS.Desktop.Plugins.ExpMestSaleCreate<br/>78 files<br/>frmExpMestSaleCreate.cs"]
    
    subgraph "Medicine Selection"
        UCMed["HIS.UC.MedicineInStock<br/>Available Stock Grid"]
        StockCheck["Stock Availability Check<br/>Batch/Lot Selection"]
    end
    
    subgraph "Transaction Processing"
        ExpADO["ExpMestSaleADO.cs<br/>Sale Transaction DTO"]
        PriceCalc["Price Calculation<br/>VAT, Discounts"]
        StockDeduct["Stock Deduction Logic"]
    end
    
    subgraph "Payment Integration"
        Transaction["HIS.Desktop.Plugins.Transaction<br/>Payment Collection"]
        Invoice["Electronic Invoice Generation"]
    end
    
    subgraph "Print & Records"
        MPSBill["MPS.Processor.Mps000085<br/>Sale Receipt"]
        MPSLabel["MPS.Processor.Mps000086<br/>Medicine Labels"]
    end
    
    Patient --> SalePlugin
    SalePlugin --> UCMed
    UCMed --> StockCheck
    StockCheck --> ExpADO
    ExpADO --> PriceCalc
    PriceCalc --> StockDeduct
    StockDeduct --> Transaction
    Transaction --> Invoice
    SalePlugin --> MPSBill
    SalePlugin --> MPSLabel
```

### Export Document Types

The export plugins support multiple document types mapped to different MPS processors:

| Export Type | Document Code | MPS Processor | Plugin Integration |
|-------------|---------------|---------------|-------------------|
| Prescription Export | `TT` | Mps000078-Mps000082 | `ExpMestActualCreate` |
| Sale Export | `BAN` | Mps000085-Mps000086 | `ExpMestSaleCreate` |
| Department Export | `DEPA` | Mps000088-Mps000090 | `ExpMestDepaCreate` |
| Return Export | `TRA` | Mps000091-Mps000092 | `ExpMestReturnCreate` |

**Sources:** [`.devin/wiki.json:90-97`](../../../../.devin/wiki.json#L90-L97, MPS architecture overview

## Stock Management & Inventory (MediStock*)

The `MediStock*` plugins provide comprehensive warehouse and inventory management:

### Core Stock Management Plugins

| Plugin Name | Files | Primary Function | Key Capabilities |
|-------------|-------|------------------|------------------|
| `MediStockSummary` | 49 | Inventory summary reports | Real-time balance, expiry alerts |
| `MedicalStoreV2` | 49 | Pharmacy store management | Multi-location inventory |
| `MediStockPeriod` | ~40-45 | Period closing/opening | Monthly reconciliation |
| `MediStockBalance` | ~35-40 | Stock balance inquiry | Batch-level tracking |
| `MediStockExport` | ~30-35 | Export stock data | Excel/CSV reporting |

### MediStockSummary Architecture

```mermaid
graph TB
    Plugin["HIS.Desktop.Plugins.MediStockSummary<br/>49 files"]
    
    subgraph "UI Components"
        MainForm["frmMediStockSummary.cs<br/>Main Grid Display"]
        FilterPanel["Filter Panel<br/>Date Range, Stock Type"]
        GridConfig["GridControl Configuration<br/>Columns, Grouping"]
    end
    
    subgraph "Data Aggregation"
        BackendData["HIS.Desktop.LocalStorage.BackendData<br/>Cached Stock Data"]
        Calculator["Stock Calculator<br/>Beginning + Import - Export"]
        ExpiryCheck["Expiry Date Checker<br/>Alert Generation"]
    end
    
    subgraph "Reporting"
        MPSReport["MPS.Processor.Mps000155<br/>Stock Summary Report"]
        ExcelExport["FlexCel Export<br/>Excel Workbook"]
    end
    
    Plugin --> MainForm
    Plugin --> FilterPanel
    MainForm --> GridConfig
    MainForm --> BackendData
    BackendData --> Calculator
    Calculator --> ExpiryCheck
    Plugin --> MPSReport
    MPSReport --> ExcelExport
    
    style Plugin fill:#f9f9f9
    style BackendData fill:#fff4e1
```

### MedicalStoreV2 Multi-Location Support

The `MedicalStoreV2` plugin (49 files) extends stock management to support multiple physical locations:

- **Branch Support**: `HIS.Desktop.LocalStorage.Branch` integration for multi-branch hospitals
- **Stock Transfer**: Inter-location transfer workflows
- **Location-Specific Inventory**: Separate balances per location
- **Consolidated Reporting**: Cross-location inventory views

**Key Classes:**
- [[`HIS/Plugins/HIS.Desktop.Plugins.MedicalStoreV2/Run/frmMedicalStoreV2.cs`](../../../../HIS/Plugins/HIS.Desktop.Plugins.MedicalStoreV2/Run/frmMedicalStoreV2.cs)](../../../../HIS/Plugins/HIS.Desktop.Plugins.MedicalStoreV2/Run/frmMedicalStoreV2.cs) - Main form
- [[`HIS/Plugins/HIS.Desktop.Plugins.MedicalStoreV2/ADO/MediStockADO.cs`](../../../../HIS/Plugins/HIS.Desktop.Plugins.MedicalStoreV2/ADO/MediStockADO.cs)](../../../../HIS/Plugins/HIS.Desktop.Plugins.MedicalStoreV2/ADO/MediStockADO.cs) - Stock location model
- `HIS/Plugins/HIS.Desktop.Plugins.MedicalStoreV2/Processor/` - Multi-location business logic

**Sources:** [`.devin/wiki.json:90-97`](../../../../.devin/wiki.json#L90-L97)

## Bidding & Procurement (Bid*)

The `Bid*` plugin family manages the procurement and bidding process for medicines and materials:

### Bid Management Plugins

| Plugin Name | Files | Purpose | Process Stage |
|-------------|-------|---------|---------------|
| `BidCreate` | 47 | Create bid packages | Planning & specification |
| `BidEdit` | ~35-40 | Modify bid details | Pre-award changes |
| `BidSupplier` | ~30-35 | Supplier management | Vendor selection |
| `BidMedicineType` | ~30-35 | Bid medicine mapping | Item specifications |
| `BidImport` | ~25-30 | Import bid data | Bulk upload |

### Bid Process Flow

```mermaid
graph TB
    Planning["Procurement Planning<br/>Annual Requirements"]
    
    subgraph "Bid Creation"
        BidPlugin["HIS.Desktop.Plugins.BidCreate<br/>47 files<br/>frmBidCreate.cs"]
        BidADO["BidCreateADO.cs<br/>Bid Package Model"]
        ItemSelection["Medicine/Material Selection<br/>Quantities & Specifications"]
    end
    
    subgraph "Supplier Management"
        SupplierPlugin["HIS.Desktop.Plugins.BidSupplier<br/>Vendor Database"]
        SupplierQuote["Quote Collection<br/>Price Comparison"]
    end
    
    subgraph "Award & Contract"
        BidAward["Bid Award Decision<br/>Winner Selection"]
        ContractGen["Contract Generation<br/>Terms & Conditions"]
    end
    
    subgraph "Integration"
        ImpMest["ImpMestCreate<br/>Receive against bid"]
        PriceUpdate["Medicine Price Update<br/>Contract Pricing"]
    end
    
    Planning --> BidPlugin
    BidPlugin --> BidADO
    BidADO --> ItemSelection
    ItemSelection --> SupplierPlugin
    SupplierPlugin --> SupplierQuote
    SupplierQuote --> BidAward
    BidAward --> ContractGen
    ContractGen --> ImpMest
    ContractGen --> PriceUpdate
```

### BidCreate Plugin Components

The `BidCreate` plugin structure (47 files):

- **Bid Header Management**:
  - Bid number, name, year
  - Budget allocation
  - Timeline and deadlines
- **Bid Line Items**:
  - Medicine types with specifications
  - Material types with specifications
  - Requested quantities
  - Estimated prices
- **Supplier Association**:
  - Multiple supplier quotes
  - Comparative analysis
  - Award recommendations
- **Document Generation**:
  - MPS processors for bid documents
  - Export to tender systems

**Sources:** [`.devin/wiki.json:90-97`](../../../../.devin/wiki.json#L90-L97)

## Blood Management Operations

Blood product management requires specialized tracking due to regulatory requirements:

### Blood-Related Plugins

| Plugin Name | Estimated Files | Purpose | Unique Features |
|-------------|----------------|---------|-----------------|
| `ImpMestBlood` | ~35-40 | Import blood products | Blood bank integration |
| `ExpMestBlood` | ~35-40 | Issue blood products | Cross-match verification |
| `BloodType` | ~25-30 | Blood type management | ABO, Rh tracking |
| `BloodTracking` | ~30-35 | Traceability | Donor to recipient chain |

### Blood Product Flow

```mermaid
graph TB
    BloodBank["Blood Bank<br/>External Supplier"]
    
    subgraph "Import Process"
        ImpBlood["HIS.Desktop.Plugins.ImpMestBlood<br/>Blood Receipt"]
        BloodCheck["Quality Checks<br/>Temperature Log<br/>Expiry Verification"]
        BloodStorage["Cold Storage Assignment<br/>Location Tracking"]
    end
    
    subgraph "Cross-Match Process"
        PatientOrder["Blood Order<br/>Patient Request"]
        TypeMatch["Blood Type Matching<br/>ABO & Rh Compatibility"]
        CrossMatch["Cross-Match Test<br/>Lab Verification"]
    end
    
    subgraph "Issue Process"
        ExpBlood["HIS.Desktop.Plugins.ExpMestBlood<br/>Blood Issue"]
        Traceability["Blood Tracking Record<br/>Donor to Recipient"]
        TransfusionDoc["Transfusion Documentation"]
    end
    
    BloodBank --> ImpBlood
    ImpBlood --> BloodCheck
    BloodCheck --> BloodStorage
    
    PatientOrder --> TypeMatch
    BloodStorage --> TypeMatch
    TypeMatch --> CrossMatch
    CrossMatch --> ExpBlood
    ExpBlood --> Traceability
    ExpBlood --> TransfusionDoc
```

**Special Considerations:**
- **Temperature Monitoring**: Integration with cold chain monitoring
- **Expiry Tracking**: Short shelf life (typically 35-42 days)
- **Regulatory Compliance**: MOH blood safety regulations
- **Audit Trail**: Complete donor-to-recipient traceability

**Sources:** [`.devin/wiki.json:90-97`](../../../../.devin/wiki.json#L90-L97, medical domain knowledge

## Integration with Other Systems

### API Consumer Integration

All medicine and material plugins communicate with the backend through `HIS.Desktop.ApiConsumer`:

| API Consumer Class | Endpoints Served | Plugin Usage |
|-------------------|------------------|--------------|
| `ApiConsumerStore` | General REST calls | All plugins |
| `MedicineTypeApiConsumer` | `/api/HisMedicineType/*` | MedicineType, ImpMest*, ExpMest* |
| `MaterialTypeApiConsumer` | `/api/HisMaterialType/*` | MaterialType, ImpMest*, ExpMest* |
| `MediStockApiConsumer` | `/api/HisMediStock/*` | MediStock*, Stock management |
| `ImpMestApiConsumer` | `/api/HisImpMest/*` | ImpMest* plugins |
| `ExpMestApiConsumer` | `/api/HisExpMest/*` | ExpMest* plugins |

**Sources:** [`.devin/wiki.json:54-58`](../../../../.devin/wiki.json#L54-L58, HIS.Desktop.ApiConsumer overview

### Local Storage & Caching

Medicine and material data is heavily cached in `HIS.Desktop.LocalStorage.BackendData`:

```mermaid
graph LR
    subgraph "Backend Data Cache"
        MedCache["BackendData.MedicineTypes<br/>In-Memory Collection"]
        MatCache["BackendData.MaterialTypes<br/>In-Memory Collection"]
        StockCache["BackendData.MediStocks<br/>Warehouse List"]
        SupplierCache["BackendData.Suppliers<br/>Vendor List"]
    end
    
    subgraph "Plugin Access"
        ImpPlugin["ImpMest* Plugins"]
        ExpPlugin["ExpMest* Plugins"]
        StockPlugin["MediStock* Plugins"]
        BidPlugin["Bid* Plugins"]
    end
    
    API["Backend API<br/>Initial Load"]
    
    API -->|"Load on Startup"| MedCache
    API -->|"Load on Startup"| MatCache
    API -->|"Load on Startup"| StockCache
    API -->|"Load on Startup"| SupplierCache
    
    MedCache --> ImpPlugin
    MedCache --> ExpPlugin
    MedCache --> BidPlugin
    
    MatCache --> ImpPlugin
    MatCache --> ExpPlugin
    MatCache --> BidPlugin
    
    StockCache --> StockPlugin
    StockCache --> ImpPlugin
    StockCache --> ExpPlugin
    
    SupplierCache --> BidPlugin
    SupplierCache --> ImpPlugin
```

**Caching Benefits:**
- Reduces API calls during data entry
- Enables offline validation
- Improves autocomplete performance
- Faster grid filtering and sorting

**Cache Refresh:**
- Automatic on application startup
- Manual refresh via `BackendData.Reload()`
- PubSub event-driven updates when data changes

**Sources:** [`.devin/wiki.json:44-53`](../../../../.devin/wiki.json#L44-L53, LocalStorage documentation

### MPS Print Integration

Medicine and material transactions generate various print documents:

| Transaction Type | MPS Processor Range | Document Examples |
|-----------------|-------------------|-------------------|
| Import | Mps000070-Mps000079 | Import voucher, goods receipt |
| Export | Mps000080-Mps000099 | Export voucher, sale receipt, labels |
| Stock | Mps000150-Mps000159 | Stock report, inventory card |
| Bid | Mps000200-Mps000209 | Bid document, contract |

**Common Print Features:**
- Barcode generation for tracking
- QR codes for digital verification
- Multi-copy printing (original, copy, warehouse)
- Electronic signature support

**Sources:** [`.devin/wiki.json:180-198`](../../../../.devin/wiki.json#L180-L198, MPS Print System overview

## Common Plugin Patterns

### Standard Plugin Structure

Each medicine/material plugin typically follows this structure:

```
HIS.Desktop.Plugins.[PluginName]/
├── [PluginName].cs                    # Plugin entry point
├── Run/
│   ├── frm[PluginName].cs            # Main form
│   ├── frm[PluginName].Designer.cs   # Designer file
│   └── frm[PluginName].resx          # Resources
├── ADO/
│   └── [PluginName]ADO.cs            # Data transfer objects
├── Processor/
│   └── [PluginName]Processor.cs      # Business logic
├── Base/
│   ├── RequestUriStore.cs            # API endpoints
│   └── ResourceLangManager.cs        # Localization
├── Resources/
│   └── [Language].resx               # Resource files
└── Properties/
    └── AssemblyInfo.cs               # Assembly metadata
```

### Data Validation Patterns

Common validation across plugins:

- **Stock Availability**: Before export, verify sufficient stock
- **Expiry Date**: Warn on near-expiry, block expired items
- **Duplicate Prevention**: Check for duplicate transactions
- **Price Validation**: Ensure prices within allowed ranges
- **User Permissions**: Validate role-based access
- **Period Closure**: Prevent transactions in closed periods

### Error Handling

Standard error handling via:
- `Inventec.Common.Logging` for logging exceptions
- `MessageBox` for user notifications
- `ValidationResult` objects for business rule violations
- Rollback support for failed transactions

**Sources:** [`.devin/wiki.json:60-68`](../../../../.devin/wiki.json#L60-L68, Plugin System Architecture