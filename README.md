# Entity Comparison Table - Previous vs Emirates14 Models

## 📊 Quick Reference Comparison

| Aspect | Previous Models | Emirates14 Models |
|--------|----------------|-------------------|
| **Total Entities** | ~25 | 72 |
| **Schemas** | 1 (dbo) | 6 (Auth, DataManagement, Lookup, Request, FileManager, Dbo) |
| **DbContext** | ApplicationDbContext (with Identity) | Emirates14DbContext (standalone) |
| **Inheritance Pattern** | Table-per-hierarchy (Orders) | Table-per-type (Requests) |
| **Navigation Properties** | Basic | Comprehensive |
| **Authentication** | ASP.NET Identity | Custom implementation |

---

## 🔄 Entity Mapping Table

### User Management
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| ApplicationDbUser | User (Auth) | Different approach: Identity vs standalone |
| - | IamLoginHistory | New: Login tracking |
| - | IamResponse | New: IAM integration |
| - | Role (Auth) | New: Role management |
| - | UserRole | New: User-role mapping |

### Geographic Entities
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| City | - | Not present in Emirates14 |
| Region | - | Not present in Emirates14 |
| Governorate (EmirateTables) | Governorate (DataManagement) | Enhanced with more properties |
| - | GovernorateLookup (Lookup) | New: Separate lookup table |
| - | Nationality (Lookup) | New: Nationality management |

### Service Management
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| Service (EmirateTables) | Service (DataManagement) | Enhanced version |
| - | ServiceAudience | New: Service-audience mapping |
| - | ServiceBenefit | New: Service benefits |
| - | ServiceCondition | New: Service conditions |
| - | ServiceRate | New: Service rates |
| - | ServiceStage | New: Service stages |
| - | ServieNotification | New: Service notifications |
| - | ServieNotificationLog | New: Notification logs |

### Order/Request Processing
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| Order (AdditionalTables) | Request (Request) | Different terminology |
| PrisonerOrder | RequestPrisonersService | Renamed and restructured |
| TreatmentOrder | RequestTreatmentRecommendation | Renamed and restructured |
| ShariaJudgmentExecutionOrder | RequestJudgmentExecution | Renamed and restructured |
| ElectronicSummonOrder | RequestElectronicSummon | Renamed and restructured |
| ElectronicSummonsforGovernoratesOrder | RequestElectronicSummonsGovernorate | Renamed and restructured |
| ViolationsAgriculturalOrder | RequestLandsInfringement | Renamed and restructured |
| RoyalCouncilPrinceOrder | - | Not present in Emirates14 |
| - | RequestMarriageCertificate | New: Marriage certificate requests |
| - | RequestPrisonerTempRelease | New: Temporary release requests |
| - | RequestForeignersRealtyOwner | New: Foreigners realty requests |
| - | RequestElectronicBoard | New: Electronic board requests |
| - | RequestType | New: Request type management |
| - | RequestStageLog | New: Request stage tracking |
| - | RequestAttachmentType | New: Attachment type management |

### Content Management
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| - | News (DataManagement) | New: News system |
| - | NewsComment | New: News commenting |
| - | NewsCateguery (Lookup) | New: News categories |
| - | Poster (DataManagement) | New: Poster management |
| - | PageContent (DataManagement) | New: Page content |
| - | MainPagePoint (DataManagement) | New: Main page points |

### Communication
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| ContactUs (SettingTables) | ContactUsMessage (Request) | Enhanced version |
| - | ContactUsMessageType (Lookup) | New: Message type management |

### File Management
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| - | UploadedFile (FileManager) | New: File upload system |

### Survey System
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| - | Survey (Dbo) | New: Survey management |
| - | SurveyQuestion (Lookup) | New: Survey questions |
| - | SurveySelect (Lookup) | New: Survey options |
| - | SurveyRate (DataManagement) | New: Survey ratings |
| - | ContactSurvey (Request) | New: Contact surveys |

### Open Data System
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| - | OpenDataCateguery (DataManagement) | New: Open data categories |
| - | OpenDataReport (DataManagement) | New: Open data reports |
| - | OpenDataRequest (Request) | New: Open data requests |
| - | OpenDataSubCateguery (Lookup) | New: Open data sub-categories |

### Lookup Tables
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| CaseType (EmirateTables) | CaseType (Lookup) | Same entity, different schema |
| Center (EmirateTables) | Center (Lookup) | Same entity, different schema |
| Prison (EmirateTables) | Prison (Lookup) | Same entity, different schema |
| TreatmentType (EmirateTables) | - | Not present in Emirates14 |
| ViolationType (EmirateTables) | - | Not present in Emirates14 |
| DestinationEntity (EmirateTables) | Entity (Lookup) | Renamed |
| RelativeRelation (EmirateTables) | RelationPerson (Lookup) | Renamed |
| MediaCenter (EmirateTables) | - | Not present in Emirates14 |
| OrderServiceType (EmirateTables) | - | Not present in Emirates14 |
| - | Audience (Lookup) | New: Audience management |
| - | BuildingType (Lookup) | New: Building types |
| - | CommentStage (Lookup) | New: Comment stages |
| - | DefendantType (Lookup) | New: Defendant types |
| - | Issue (Lookup) | New: Issues |
| - | MaritalStatus (Lookup) | New: Marital statuses |
| - | Reason (Lookup) | New: Reasons |
| - | Religion (Lookup) | New: Religions |
| - | Stage (Lookup) | New: Stages |

### Logging & Monitoring
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| Log (LogTables) | - | Not present in Emirates14 |
| HttpRequestLog (LogTables) | - | Not present in Emirates14 |
| ExceptionLog (SettingTables) | - | Not present in Emirates14 |

### Settings & Configuration
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| Setting (SettingTables) | - | Not present in Emirates14 |
| Slider (SettingTables) | - | Not present in Emirates14 |
| SocialMedia (SettingTables) | - | Not present in Emirates14 |
| Question (SettingTables) | - | Not present in Emirates14 |
| ComplaintsAndSuggestions (SettingTables) | - | Not present in Emirates14 |

### Notification System
| Previous | Emirates14 | Notes |
|----------|------------|-------|
| Notification (UserTables) | - | Not present in Emirates14 |
| HistoryNotify (SettingTables) | - | Not present in Emirates14 |
| Device (UserTables) | - | Not present in Emirates14 |

---

## 📈 Statistics Summary

### Entity Count by Schema (Emirates14)
- **Auth**: 5 entities
- **DataManagement**: 18 entities
- **Lookup**: 20 entities
- **Request**: 15 entities
- **FileManager**: 1 entity
- **Dbo**: 1 entity
- **Views**: 1 entity
- **Total**: 72 entities

### Previous Models Count
- **AdditionalTables**: 9 entities
- **Cities Tables**: 2 entities
- **EmirateTables**: 11 entities
- **LogTables**: 2 entities
- **SettingTables**: 9 entities
- **UserTables**: 4 entities
- **Total**: ~41 entities (including BaseEntity)

---

## 🎯 Key Takeaways

1. **Emirates14 is more comprehensive** with 72 vs ~41 entities
2. **Different architectural approach** - schema-based vs domain-based
3. **Enhanced functionality** in Emirates14 (surveys, open data, notifications)
4. **Request-based vs Order-based** terminology and structure
5. **Standalone vs Identity-based** authentication approach
