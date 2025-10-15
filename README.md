# Current vs New Domain Models Comparison
## The Emirates Government Services Platform

**Document Version:** 1.0  
**Date:** January 2025  
**Purpose:** Comprehensive comparison between current domain models and new generated domain models

---

## Executive Summary

This document provides a detailed analysis of the differences and similarities between the current domain models and the new generated domain models based on the CSV schema. The comparison covers architectural changes, base class evolution, inheritance patterns, conversion requirements, and implementation limitations.

---

## 1. Architectural Overview Comparison

### 1.1 Current Architecture (Domain-Based)

```
TheEmirate.Domain/Entities/
├── BaseEntity.cs (Single base class)
├── EmirateTables/ (11 entities)
├── LogTables/ (2 entities)
├── SettingTables/ (9 entities)
├── UserTables/ (4 entities)
├── Cities Tables/ (2 entities)
├── Copon/ (2 entities)
├── Chat/ (2 entities)
└── AdditionalTables/ (9 entities)
```

### 1.2 New Architecture (Schema-Based)

```
TheEmirate.Domain/Entities/
├── BaseEntity.cs (Original - to be deprecated)
├── Auth/ (4 entities + BaseAuthEntity)
├── DataManagement/ (16 entities + BaseDataManagementEntity)
├── FileManager/ (1 entity)
├── Lookup/ (22 entities + BaseLookupEntity)
├── Request/ (16 entities + BaseRequestEntity)
└── EFMigrationsHistory.cs
```

---

## 2. Base Class Evolution Analysis

### 2.1 Current Base Class

```csharp
public class BaseEntity
{
    [Key] public int Id { get; set; }
    public string NameAr { get; set; }
    public string NameEn { get; set; }
    public bool IsActive { get; set; }
    public bool IsDeleted { get; set; }
    public DateTime Date { get; set; }
    public string ChangeLangName(string lang = "ar") { ... }
}
```

**Characteristics:**
- ✅ Single base class for all entities
- ✅ Simple audit trail (Date only)
- ✅ Soft delete support (IsDeleted)
- ✅ Bilingual support (NameAr, NameEn)
- ✅ Language switching utility
- ❌ No user tracking (CreatedBy, LastModifiedBy)
- ❌ No concurrency control
- ❌ No comprehensive audit trail

### 2.2 New Base Classes

#### 2.2.1 BaseAuthEntity
```csharp
public class BaseAuthEntity
{
    [Key] public int Id { get; set; }
    public int CreatedBy { get; set; }
    public DateTime CreatedDate { get; set; }
    public Guid ConcurrencyStamp { get; set; }
    public int? LastModifiedBy { get; set; }
    public DateTime? LastModifiedDate { get; set; }
    
    // Navigation Properties
    public virtual User CreatedByUser { get; set; }
    public virtual User LastModifiedByUser { get; set; }
}
```

#### 2.2.2 BaseDataManagementEntity
```csharp
public class BaseDataManagementEntity
{
    [Key] public int Id { get; set; }
    public int CreatedBy { get; set; }
    public DateTime CreatedDate { get; set; }
    public Guid ConcurrencyStamp { get; set; }
    public int? LastModifiedBy { get; set; }
    public DateTime? LastModifiedDate { get; set; }
    
    // Navigation Properties
    public virtual Auth.User CreatedByUser { get; set; }
    public virtual Auth.User LastModifiedByUser { get; set; }
}
```

#### 2.2.3 BaseLookupEntity
```csharp
public class BaseLookupEntity
{
    [Key] public int Id { get; set; }
    public int CreatedBy { get; set; }
    public DateTime CreatedDate { get; set; }
    public Guid ConcurrencyStamp { get; set; }
    public int? LastModifiedBy { get; set; }
    public DateTime? LastModifiedDate { get; set; }
    
    // Navigation Properties
    public virtual Auth.User CreatedByUser { get; set; }
    public virtual Auth.User LastModifiedByUser { get; set; }
}
```

#### 2.2.4 BaseRequestEntity
```csharp
public class BaseRequestEntity
{
    [Key] public Guid Id { get; set; }  // Different: Uses Guid instead of int
    public int CreatedBy { get; set; }
    public DateTime CreatedDate { get; set; }
    public Guid ConcurrencyStamp { get; set; }
    public int? LastModifiedBy { get; set; }
    public DateTime? LastModifiedDate { get; set; }
    
    // Navigation Properties
    public virtual Auth.User CreatedByUser { get; set; }
    public virtual Auth.User LastModifiedByUser { get; set; }
}
```

**New Base Class Characteristics:**
- ✅ **Multiple specialized base classes** for different schemas
- ✅ **Comprehensive audit trail** (CreatedBy, CreatedDate, LastModifiedBy, LastModifiedDate)
- ✅ **Concurrency control** (ConcurrencyStamp)
- ✅ **User tracking** (CreatedBy, LastModifiedBy with navigation properties)
- ✅ **Navigation properties** for relationships
- ❌ **No soft delete** (IsDeleted removed)
- ❌ **No language switching utility** (ChangeLangName removed)
- ❌ **No bilingual naming** (NameAr, NameEn removed from base)

---

## 3. Entity Mapping and Transformation

### 3.1 Current Entities → New Entities Mapping

| Current Domain | Current Entity | New Schema | New Entity | Status |
|----------------|----------------|------------|------------|---------|
| **EmirateTables** | CaseType | Lookup | CaseType | ✅ Mapped |
| **EmirateTables** | Center | Lookup | Center | ✅ Mapped |
| **EmirateTables** | DestinationEntity | Lookup | Entity | ✅ Renamed |
| **EmirateTables** | Governorate | DataManagement | Governorate | ✅ Enhanced |
| **EmirateTables** | MediaCenter | DataManagement | EmiratesPrince | ✅ Renamed |
| **EmirateTables** | OrderServiceType | Request | RequestType | ✅ Renamed |
| **EmirateTables** | Prison | Lookup | Prison | ✅ Mapped |
| **EmirateTables** | RelativeRelation | Lookup | RelationPerson | ✅ Renamed |
| **EmirateTables** | Service | DataManagement | Service | ✅ Enhanced |
| **EmirateTables** | TreatmentType | Lookup | TreatmentType | ❌ Removed |
| **EmirateTables** | ViolationType | Lookup | ViolationType | ❌ Removed |
| **LogTables** | Log | DataManagement | ServieNotificationLog | ✅ Renamed |
| **LogTables** | HttpRequestLog | DataManagement | ServieNotificationLog | ✅ Merged |
| **SettingTables** | ComplaintsAndSuggestions | Request | ContactUsMessage | ✅ Renamed |
| **SettingTables** | ContactUs | Request | ContactUsMessage | ✅ Mapped |
| **SettingTables** | ExceptionLog | DataManagement | ServieNotificationLog | ✅ Merged |
| **SettingTables** | HistoryNotify | DataManagement | ServieNotificationLog | ✅ Merged |
| **SettingTables** | Payment | DataManagement | Service | ✅ Merged |
| **SettingTables** | Question | Lookup | SurveyQuestion | ✅ Renamed |
| **SettingTables** | Setting | DataManagement | Service | ✅ Merged |
| **SettingTables** | Slider | DataManagement | Poster | ✅ Renamed |
| **SettingTables** | SocialMedia | DataManagement | SocialMedia | ❌ Removed |
| **UserTables** | ApplicationDbUser | Auth | User | ✅ Enhanced |
| **UserTables** | Device | Auth | User | ✅ Merged |
| **UserTables** | Nationality | Lookup | Nationality | ✅ Mapped |
| **UserTables** | Notification | DataManagement | ServieNotification | ✅ Renamed |
| **Cities Tables** | City | Lookup | City | ❌ Removed |
| **Cities Tables** | Region | Lookup | Region | ❌ Removed |
| **Copon** | Copon | DataManagement | Service | ❌ Removed |
| **Copon** | CoponUsed | DataManagement | Service | ❌ Removed |
| **Chat** | HubConnection | DataManagement | ServieNotification | ❌ Removed |
| **Chat** | Messages | DataManagement | ServieNotification | ❌ Removed |
| **AdditionalTables** | ElectronicSummonOrder | Request | RequestElectronicSummon | ✅ Renamed |
| **AdditionalTables** | ElectronicSummonsforGovernoratesOrder | Request | RequestElectronicSummonsGovernorate | ✅ Renamed |
| **AdditionalTables** | Order | Request | Request | ✅ Renamed |
| **AdditionalTables** | OrderAttachment | Request | RequestAttachmentType | ✅ Renamed |
| **AdditionalTables** | PrisonerOrder | Request | RequestPrisonersService | ✅ Renamed |
| **AdditionalTables** | RoyalCouncilPrinceOrder | Request | RequestElectronicBoard | ✅ Renamed |
| **AdditionalTables** | ShariaJudgmentExecutionOrder | Request | RequestJudgmentExecution | ✅ Renamed |
| **AdditionalTables** | TreatmentOrder | Request | RequestTreatmentRecommendation | ✅ Renamed |
| **AdditionalTables** | ViolationsAgriculturalOrder | Request | RequestLandsInfringement | ✅ Renamed |

### 3.2 New Entities Added

| Schema | New Entity | Purpose |
|--------|------------|---------|
| **Auth** | Role | Role-based access control |
| **Auth** | UserRole | User-role mapping |
| **DataManagement** | DesignEvaluation | User design feedback |
| **DataManagement** | EmiratesPrince | Prince information |
| **DataManagement** | News | News management |
| **DataManagement** | NewsComment | News comments |
| **DataManagement** | OpenDataCateguery | Open data categorization |
| **DataManagement** | OpenDataReport | Open data reports |
| **DataManagement** | Poster | Advertisement management |
| **DataManagement** | ServiceBenefit | Service benefit tracking |
| **DataManagement** | ServiceCondition | Service conditions |
| **DataManagement** | ServiceRate | Service rating |
| **DataManagement** | ServiceStage | Service stage management |
| **DataManagement** | ServieNotification | Service notifications |
| **DataManagement** | ServieNotificationLog | Notification logging |
| **DataManagement** | SurveyRate | Survey rating |
| **FileManager** | UploadedFile | File management |
| **Lookup** | Audience | User audience classification |
| **Lookup** | BuildingType | Building type classifications |
| **Lookup** | ContactUsMessageType | Contact message types |
| **Lookup** | DefendantType | Defendant type classifications |
| **Lookup** | Governorate | Governorate information |
| **Lookup** | Issue | Legal issues |
| **Lookup** | LegalPerson | Legal entity information |
| **Lookup** | MaritalStatus | Marital status options |
| **Lookup** | NewsCateguery | News categorization |
| **Lookup** | OpenDataSubCateguery | Open data sub-categories |
| **Lookup** | Reason | Request rejection reasons |
| **Lookup** | Religion | Religious classifications |
| **Lookup** | Stage | Request stage definitions |
| **Lookup** | SurveyQuestion | Survey questions |
| **Lookup** | SurveySelect | Survey answer options |
| **Request** | ContactSurvey | Contact information surveys |
| **Request** | OpenDataRequest | Open data requests |
| **Request** | RequestElectronicBoard | Electronic board requests |
| **Request** | RequestLandsInfringement | Land infringement requests |
| **Request** | RequestMarriageCertificate | Marriage certificate requests |
| **Request** | RequestPrisonerTempRelease | Temporary release requests |
| **Request** | RequestStageLog | Request stage tracking |
| **dbo** | EFMigrationsHistory | Entity Framework migrations |

---

## 4. Inheritance Patterns Analysis

### 4.1 Current Inheritance Pattern

```csharp
// All entities inherit from single BaseEntity
public class Service : BaseEntity
{
    // Additional properties
}

public class Order : BaseEntity
{
    // Additional properties
}
```

**Current Pattern Characteristics:**
- ✅ **Simple inheritance** - All entities inherit from BaseEntity
- ✅ **Consistent base properties** - All entities have same base properties
- ✅ **Easy to understand** - Single inheritance hierarchy
- ❌ **No specialization** - All entities treated the same
- ❌ **Limited flexibility** - Cannot have different base properties for different entity types

### 4.2 New Inheritance Pattern

```csharp
// Schema-specific base classes
public class Service : BaseDataManagementEntity
{
    // Additional properties
}

public class Request : BaseRequestEntity  // Uses Guid primary key
{
    // Additional properties
}

public class Role : BaseAuthEntity
{
    // Additional properties
}

public class Nationality : BaseLookupEntity
{
    // Additional properties
}
```

**New Pattern Characteristics:**
- ✅ **Specialized inheritance** - Different base classes for different schemas
- ✅ **Flexible base properties** - Each schema can have different base properties
- ✅ **Better organization** - Clear separation of concerns
- ✅ **Enhanced functionality** - Different primary key types (int vs Guid)
- ❌ **More complex** - Multiple inheritance hierarchies
- ❌ **Potential inconsistency** - Different base classes might diverge over time

---

## 5. Required Base Classes and Inheritance

### 5.1 Essential Base Classes

#### 5.1.1 BaseAuthEntity (Required)
**Purpose:** Authentication and authorization entities
**Inherited by:**
- `Role.cs`
- `UserRole.cs`
- `User.cs`

**Key Features:**
- Standard audit trail
- User tracking
- Concurrency control

#### 5.1.2 BaseDataManagementEntity (Required)
**Purpose:** Core data management entities
**Inherited by:**
- `EmiratesPrince.cs`
- `Governorate.cs`
- `News.cs`
- `OpenDataCateguery.cs`
- `OpenDataReport.cs`
- `Poster.cs`
- `Service.cs`
- `ServiceCondition.cs`
- `ServiceStage.cs`
- `ServieNotification.cs`
- `ServieNotificationLog.cs`

**Key Features:**
- Standard audit trail
- User tracking
- Concurrency control

#### 5.1.3 BaseLookupEntity (Required)
**Purpose:** Reference data entities
**Inherited by:**
- `Audience.cs`
- `BuildingType.cs`
- `CaseType.cs`
- `ContactUsMessageType.cs`
- `DefendantType.cs`
- `MaritalStatus.cs`
- `Nationality.cs`
- `NewsCateguery.cs`
- `OpenDataSubCateguery.cs`
- `Prison.cs`
- `Reason.cs`
- `RelationPerson.cs`
- `Religion.cs`
- `Stage.cs`
- `SurveyQuestion.cs`
- `SurveySelect.cs`

**Key Features:**
- Standard audit trail
- User tracking
- Concurrency control

#### 5.1.4 BaseRequestEntity (Required)
**Purpose:** Service request entities
**Inherited by:**
- `RequestAttachmentType.cs`
- `RequestElectronicBoard.cs`
- `RequestElectronicSummon.cs`
- `RequestElectronicSummonsGovernorate.cs`
- `RequestJudgmentExecution.cs`
- `RequestLandsInfringement.cs`
- `RequestMarriageCertificate.cs`
- `RequestPrisonersService.cs`
- `RequestPrisonerTempRelease.cs`
- `Request.cs`
- `RequestStageLog.cs`
- `RequestTreatmentRecommendation.cs`
- `RequestType.cs`

**Key Features:**
- **Guid primary key** (different from others)
- Standard audit trail
- User tracking
- Concurrency control

### 5.2 Non-Inheriting Entities

#### 5.2.1 Standalone Entities
These entities don't inherit from base classes due to specific requirements:

- `DesignEvaluation.cs` - Simple evaluation entity
- `ServiceBenefit.cs` - Simple benefit tracking
- `ServiceRate.cs` - Simple rating entity
- `SurveyRate.cs` - Simple survey rating
- `ContactSurvey.cs` - Simple contact survey
- `ContactUsMessage.cs` - Simple contact message
- `OpenDataRequest.cs` - Simple open data request
- `NewsComment.cs` - Simple news comment
- `Center.cs` - Simple center entity
- `Governorate.cs` (Lookup) - Simple governorate entity
- `Issue.cs` - Simple issue entity
- `LegalPerson.cs` - Simple legal person entity
- `UploadedFile.cs` - File management entity
- `EFMigrationsHistory.cs` - System entity

---

## 6. Conversion Changes Required

### 6.1 Database Schema Changes

#### 6.1.1 Primary Key Changes
```sql
-- Current: All entities use int primary keys
ALTER TABLE [Orders] ALTER COLUMN [Id] uniqueidentifier NOT NULL;

-- New: Request entities use Guid primary keys
-- This is a breaking change requiring data migration
```

#### 6.1.2 Column Additions
```sql
-- Add audit trail columns to existing tables
ALTER TABLE [Services] ADD [CreatedBy] int NOT NULL;
ALTER TABLE [Services] ADD [CreatedDate] datetime2(7) NOT NULL;
ALTER TABLE [Services] ADD [ConcurrencyStamp] uniqueidentifier NOT NULL;
ALTER TABLE [Services] ADD [LastModifiedBy] int NULL;
ALTER TABLE [Services] ADD [LastModifiedDate] datetime2(7) NULL;
```

#### 6.1.3 Column Removals
```sql
-- Remove soft delete columns
ALTER TABLE [Services] DROP COLUMN [IsDeleted];

-- Remove language switching columns
ALTER TABLE [Services] DROP COLUMN [NameAr];
ALTER TABLE [Services] DROP COLUMN [NameEn];
```

### 6.2 Entity Model Changes

#### 6.2.1 Base Class Migration
```csharp
// Current
public class Service : BaseEntity
{
    // Inherits: Id, NameAr, NameEn, IsActive, IsDeleted, Date, ChangeLangName()
}

// New
public class Service : BaseDataManagementEntity
{
    // Inherits: Id, CreatedBy, CreatedDate, ConcurrencyStamp, LastModifiedBy, LastModifiedDate
    // Must add: NameAr, NameEn, IsActive as explicit properties
    // Must add: ChangeLangName() method if needed
}
```

#### 6.2.2 Property Migration
```csharp
// Current Service entity
public class Service : BaseEntity
{
    // Base properties inherited
    public string DescriptionAr { get; set; }
    public string DescriptionEn { get; set; }
    // ... other properties
}

// New Service entity
public class Service : BaseDataManagementEntity
{
    // Must explicitly add base properties
    [Required] [MaxLength(256)] public string NameAr { get; set; }
    [Required] [MaxLength(256)] public string NameEn { get; set; }
    [Required] public bool IsActive { get; set; }
    
    // Enhanced properties
    [Required] [MaxLength(256)] public string SectorAr { get; set; }
    [Required] [MaxLength(256)] public string SectorEn { get; set; }
    [Required] [MaxLength(4000)] public string DescriptionAr { get; set; }
    [Required] [MaxLength(4000)] public string DescriptionEn { get; set; }
    [Required] [MaxLength(256)] public string RequestLink { get; set; }
    [MaxLength(40)] public string WorkDays { get; set; }
    public float? Cost { get; set; }
    [Required] [MaxLength(4000)] public string ImageName { get; set; }
    [Required] public bool IsExternal { get; set; }
    
    // Navigation properties
    public virtual ICollection<ServiceBenefit> ServiceBenefits { get; set; }
    public virtual ICollection<ServiceCondition> ServiceConditions { get; set; }
    // ... other navigation properties
}
```

### 6.3 Business Logic Changes

#### 6.3.1 Language Switching
```csharp
// Current: Built into BaseEntity
public string ChangeLangName(string lang = "ar")
{
    return AAITHelper.HelperMsg.creatMessage(lang, NameAr, NameEn);
}

// New: Must be added to each entity that needs it
public class Service : BaseDataManagementEntity
{
    // ... properties
    
    public string ChangeLangName(string lang = "ar")
    {
        return AAITHelper.HelperMsg.creatMessage(lang, NameAr, NameEn);
    }
}
```

#### 6.3.2 Soft Delete
```csharp
// Current: Built into BaseEntity
public bool IsDeleted { get; set; }

// New: Must be implemented differently
// Option 1: Add to each entity that needs it
public class Service : BaseDataManagementEntity
{
    public bool IsDeleted { get; set; }
}

// Option 2: Use audit trail to track deletion
// Check LastModifiedDate and CreatedBy for deletion logic
```

---

## 7. Limitations and Challenges

### 7.1 Technical Limitations

#### 7.1.1 Breaking Changes
- **Primary Key Changes**: Request entities change from int to Guid
- **Base Class Changes**: All entities must be updated
- **Property Changes**: Some properties removed from base classes
- **Method Changes**: ChangeLangName() method removed from base

#### 7.1.2 Data Migration Challenges
- **Existing Data**: Must migrate existing data to new schema
- **Foreign Key Updates**: All foreign key references must be updated
- **Index Rebuilding**: Primary key changes require index rebuilding
- **Constraint Updates**: All constraints must be updated

#### 7.1.3 Performance Considerations
- **Guid vs Int**: Guid primary keys are larger and slower for indexing
- **Navigation Properties**: Lazy loading can cause N+1 query problems
- **Audit Trail**: Additional columns increase storage requirements
- **Concurrency**: ConcurrencyStamp adds overhead

### 7.2 Functional Limitations

#### 7.2.1 Lost Features
- **Soft Delete**: IsDeleted flag removed from base classes
- **Language Switching**: ChangeLangName() method removed from base
- **Bilingual Naming**: NameAr/NameEn removed from base classes
- **Simple Audit**: Date field replaced with complex audit trail

#### 7.2.2 New Dependencies
- **User Dependency**: All entities now depend on User entity
- **Schema Dependency**: Entities are tightly coupled to their schemas
- **Base Class Dependency**: Multiple base classes create maintenance overhead

### 7.3 Migration Limitations

#### 7.3.1 Data Loss Risks
- **Soft Delete Data**: IsDeleted flag data might be lost
- **Audit Data**: Date field data might be lost
- **Language Data**: NameAr/NameEn data might be lost if not properly migrated

#### 7.3.2 Application Compatibility
- **Existing Code**: All existing code using BaseEntity must be updated
- **API Changes**: API contracts might change due to property changes
- **Database Queries**: All database queries must be updated
- **Business Logic**: Business logic depending on base properties must be updated

---

## 8. Migration Strategy Recommendations

### 8.1 Phase 1: Preparation
1. **Backup Current System**: Complete backup of current database and code
2. **Create Migration Scripts**: Scripts to migrate data from old to new schema
3. **Update Base Classes**: Create new base classes alongside existing ones
4. **Test Migration**: Test migration on development environment

### 8.2 Phase 2: Gradual Migration
1. **Migrate Lookup Entities**: Start with simple lookup entities
2. **Migrate Data Management Entities**: Migrate core data management
3. **Migrate Request Entities**: Migrate request entities (most complex)
4. **Migrate Auth Entities**: Migrate authentication entities last

### 8.3 Phase 3: Cleanup
1. **Remove Old BaseEntity**: Remove old BaseEntity class
2. **Update All References**: Update all code references
3. **Performance Testing**: Test performance with new schema
4. **Documentation Update**: Update all documentation

---

## 9. Conclusion

### 9.1 Key Differences Summary

| Aspect | Current | New | Impact |
|--------|---------|-----|---------|
| **Base Classes** | 1 (BaseEntity) | 4 (Schema-specific) | High |
| **Primary Keys** | All int | Mixed (int/Guid) | High |
| **Audit Trail** | Simple (Date) | Complex (Full audit) | Medium |
| **Soft Delete** | Built-in | Must be added | Medium |
| **Language Support** | Built-in | Must be added | Medium |
| **User Tracking** | None | Full tracking | High |
| **Concurrency** | None | Full support | Medium |

### 9.2 Benefits of New Architecture
- ✅ **Better Organization**: Schema-based organization
- ✅ **Enhanced Audit**: Comprehensive audit trail
- ✅ **User Tracking**: Full user tracking capabilities
- ✅ **Concurrency Control**: Optimistic locking support
- ✅ **Type Safety**: Better type safety with specialized base classes
- ✅ **Scalability**: Better support for distributed systems

### 9.3 Challenges of Migration
- ❌ **Breaking Changes**: Significant breaking changes required
- ❌ **Data Migration**: Complex data migration required
- ❌ **Code Updates**: Extensive code updates required
- ❌ **Testing**: Comprehensive testing required
- ❌ **Downtime**: Potential system downtime during migration

### 9.4 Recommendation
The new architecture provides significant benefits but requires careful planning and execution. A phased migration approach is recommended to minimize risks and ensure system stability during the transition.

---

**End of Document**
