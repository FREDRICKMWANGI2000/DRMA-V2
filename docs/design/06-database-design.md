# Database Design

## 1. Purpose

### 1.1 Overview

This document defines the database design for the Danka Report Management Application Version 2 (DRMA-v2).

It describes how application data will be structured, stored, related, validated, and maintained within PostgreSQL. The database design supports the functional requirements defined in the Software Requirements Specification (SRS) and aligns with the Backend Architecture document.

The database is responsible for storing application metadata, user information, report session records, upload metadata, manual inputs, generated output metadata, audit logs, and future analytics data. Uploaded dataset files and generated report files remain on the filesystem and are referenced by metadata stored in PostgreSQL.

---

### 1.2 Objectives

The database design aims to:

* Provide a reliable and consistent data storage layer.
* Support the complete report generation workflow.
* Preserve data integrity through constraints and relationships.
* Minimize data duplication.
* Support efficient querying and reporting.
* Enable auditability and traceability.
* Support future feature expansion.
* Maintain compatibility with PostgreSQL best practices.

---

### 1.3 Scope

This document covers:

* Database design principles.
* Logical data model.
* Entity definitions.
* Table relationships.
* Primary and foreign keys.
* Constraints and validation rules.
* Indexing strategy.
* Migration strategy.
* Backup and recovery considerations.
* Future extensibility considerations.

Physical database deployment details are documented separately in the Deployment Architecture document.

---

## 2. Database Design Principles

The DRMA-v2 database follows several principles to ensure reliability, maintainability, scalability, and long-term consistency.

---

### 2.1 Relational Integrity

All related data should be connected through explicit foreign key relationships. The database should prevent orphaned records and maintain referential integrity.

---

### 2.2 Normalization

The schema should be normalized to reduce duplication and maintain data consistency. In general, the design should target Third Normal Form (3NF) unless a justified performance optimization requires controlled denormalization.

---

### 2.3 Clear Entity Boundaries

Each table should represent a single business entity or responsibility. Examples include report sessions, uploads, manual inputs, generated outputs, users, and audit logs.

---

### 2.4 Immutable Historical Records

Certain records, such as audit logs and completed report metadata, should preserve historical information even if related operational data changes later.

---

### 2.5 Metadata over Binary Storage

PostgreSQL will store metadata about uploaded files and generated reports. The actual file contents will remain on the filesystem.

This approach reduces database size and improves performance for large binary files.

---

### 2.6 Consistent Naming

The database will use snake_case naming conventions for:

* Table names
* Column names
* Index names
* Constraint names
* Foreign key names

Examples:

* report_sessions
* report_uploads
* created_at
* updated_at
* report_id

---

### 2.7 Auditability

The database should support tracking of important user and system actions, including report creation, uploads, report generation, downloads, deletions, and administrative operations.

---

### 2.8 Extensibility

The schema should accommodate future features such as:

* Additional report types
* AI-generated insights
* Notifications
* Scheduled reports
* Multi-tenancy
* Advanced analytics
* External integrations

---

## 3. Data Model Overview

### 3.1 Core Data Domains

The DRMA-v2 database is organized around several core data domains.

| Domain             | Purpose                                          |
| ------------------ | ------------------------------------------------ |
| User Management    | Authentication, authorization, and user profiles |
| Report Management  | Report sessions and lifecycle tracking           |
| Upload Management  | Uploaded dataset metadata                        |
| Manual Inputs      | Additional report data entered by users          |
| Preview Management | Generated preview metadata                       |
| Output Management  | Final generated report metadata                  |
| Audit Logging      | System activity tracking                         |
| Analytics          | Aggregated operational statistics (future)       |

---

### 3.2 High-Level Entity Relationships

The database is centered around the report session.

```text
users
  │
  ├── created_by
  ▼
report_sessions
  ├── report_uploads
  ├── report_manual_inputs
  ├── report_previews
  ├── report_outputs
  └── audit_logs
```

A single report session can have multiple uploads, previews, outputs, and audit records.

---

### 3.3 Core Entities

The initial database design includes the following primary entities:

| Entity               | Description                     |
| -------------------- | ------------------------------- |
| users                | Application users               |
| roles                | User roles                      |
| user_roles           | User-role assignments           |
| report_sessions      | Report session metadata         |
| report_uploads       | Uploaded dataset metadata       |
| report_manual_inputs | Additional user-entered data    |
| report_previews      | Generated preview metadata      |
| report_outputs       | Final generated report metadata |
| audit_logs           | Audit trail records             |

---

### 3.4 Report Session as Aggregate Root

The report_sessions table acts as the aggregate root for the reporting workflow. Most operational data is associated with a specific report session.

This design provides:

* Clear ownership of related records.
* Simplified report retrieval.
* Easier cleanup of report-related data.
* Better transaction boundaries.
* Improved audit traceability.

---

### 3.5 Storage Strategy

| Data Type              | Storage Location |
| ---------------------- | ---------------- |
| Application metadata   | PostgreSQL       |
| User accounts          | PostgreSQL       |
| Report sessions        | PostgreSQL       |
| Upload metadata        | PostgreSQL       |
| Manual inputs          | PostgreSQL       |
| Preview metadata       | PostgreSQL       |
| Output metadata        | PostgreSQL       |
| Audit logs             | PostgreSQL       |
| Uploaded dataset files | Filesystem       |
| Generated report files | Filesystem       |
| Preview files          | Filesystem       |

---

### 3.6 Design Rationale

Separating metadata from file storage provides several benefits:

* Smaller database size.
* Faster database backups.
* Better query performance.
* Easier file management.
* Simpler migration of storage backends.
* Reduced PostgreSQL storage overhead.

The database remains focused on structured relational data, while the filesystem handles large binary assets.

# 4. Entity Design

## 4.1 Purpose

The Entity Design defines the core business entities that make up the DRMA-v2 database.

Each entity represents a real-world concept within the report management domain and serves as the foundation for the logical and physical database models.

The entity model is designed to:

- Accurately represent business processes.
- Minimize data redundancy.
- Maintain data integrity.
- Support future system expansion.
- Simplify reporting and analytics.
- Align with the Modular Monolith architecture.

---

## 4.2 Design Principles

Each entity should:

- Represent a single business concept.
- Have a unique primary key.
- Minimize duplicated information.
- Store only persistent data.
- Be independent of presentation concerns.
- Support auditing where appropriate.
- Maintain referential integrity.

---

# 4.3 Core Entities

The first release of DRMA-v2 will consist of the following core entities.

| Entity | Purpose |
|---------|---------|
| User | System users |
| Role | User authorization |
| Permission | Fine-grained access control |
| Report | Report session metadata |
| ReportUpload | Uploaded datasets |
| ManualInput | User-entered report values |
| ReportPreview | Generated preview files |
| ReportOutput | Final generated reports |
| AuditLog | Audit trail |
| SystemSetting | Application configuration |

---

# 4.4 User Entity

## Description

Represents every authenticated user of the system.

Examples include:

- Report Officer
- Administrator
- Auditor
- System Administrator

---

### Responsibilities

- Authentication
- User profile
- Account status
- Ownership of reports
- Audit attribution

---

### Suggested Fields

| Field | Type | Description |
|---------|------|-------------|
| id | UUID | Primary Key |
| username | VARCHAR | Unique username |
| full_name | VARCHAR | User's full name |
| email | VARCHAR | Email address |
| password_hash | VARCHAR | Encrypted password |
| role_id | UUID | Assigned role |
| is_active | BOOLEAN | Account status |
| created_at | TIMESTAMP | Creation time |
| updated_at | TIMESTAMP | Last update |

---

# 4.5 Role Entity

## Description

Defines user roles used for authorization.

Roles determine the level of access available to users.

---

### Initial Roles

- Report Officer
- Administrator
- Auditor
- System Administrator

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| name | VARCHAR |
| description | TEXT |
| created_at | TIMESTAMP |

---

# 4.6 Permission Entity

## Description

Stores granular permissions that may be assigned to roles.

This allows future expansion beyond fixed role-based permissions.

---

### Example Permissions

- Create Report
- Delete Report
- Generate Report
- Manage Users
- View Audit Logs
- Manage Settings

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| permission_name | VARCHAR |
| description | TEXT |

---

# 4.7 Report Entity

## Description

Represents an individual report session.

This is the central entity of DRMA-v2.

Every upload, preview, report output, and manual input belongs to a report session.

---

### Responsibilities

- Track report lifecycle.
- Store report metadata.
- Track processing status.
- Associate related resources.

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| report_number | VARCHAR |
| report_type | VARCHAR |
| report_date | DATE |
| created_by | UUID |
| status | VARCHAR |
| remarks | TEXT |
| created_at | TIMESTAMP |
| updated_at | TIMESTAMP |

---

### Possible Status Values

```text
Draft

Uploading

Ready

Generating Preview

Preview Ready

Generating Report

Completed

Archived

Deleted
```

---

# 4.8 ReportUpload Entity

## Description

Represents datasets uploaded for a report session.

The uploaded files remain on the filesystem.

This table stores only metadata.

---

### Responsibilities

- Track uploaded datasets.
- Store upload metadata.
- Monitor validation results.
- Link uploads to reports.

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| report_id | UUID |
| upload_type | VARCHAR |
| original_filename | VARCHAR |
| storage_path | TEXT |
| file_size | BIGINT |
| upload_status | VARCHAR |
| uploaded_at | TIMESTAMP |

---

### Supported Upload Types

- Daily Hour Statistics
- Wideload Register
- Overloaded Register
- Impounded & Prohibited
- Mobile Register
- Traffic Census (Future)

---

# 4.9 ManualInput Entity

## Description

Stores user-entered values that cannot be obtained from uploaded datasets.

---

### Examples

- Weather conditions
- Officer remarks
- Operational notes
- Shift information
- Vehicle counts entered manually

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| report_id | UUID |
| field_name | VARCHAR |
| field_value | TEXT |
| created_at | TIMESTAMP |
| updated_at | TIMESTAMP |

---

# 4.10 ReportPreview Entity

## Description

Tracks preview files generated before the final report.

Preview files remain stored on the filesystem.

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| report_id | UUID |
| preview_type | VARCHAR |
| file_path | TEXT |
| generated_at | TIMESTAMP |

---

# 4.11 ReportOutput Entity

## Description

Represents final generated reports.

Only metadata is stored in PostgreSQL.

Generated documents remain on filesystem storage.

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| report_id | UUID |
| output_type | VARCHAR |
| file_path | TEXT |
| version | INTEGER |
| generated_by | UUID |
| generated_at | TIMESTAMP |

---

# 4.12 AuditLog Entity

## Description

Records significant user and system activities.

Audit records support accountability, security, troubleshooting, and compliance.

---

### Examples

- Login
- Logout
- Report created
- Upload completed
- Preview generated
- Report generated
- Report deleted
- User updated
- Settings changed

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| user_id | UUID |
| report_id | UUID (Nullable) |
| action | VARCHAR |
| resource | VARCHAR |
| details | JSONB |
| ip_address | VARCHAR |
| created_at | TIMESTAMP |

---

# 4.13 SystemSetting Entity

## Description

Stores configurable application settings.

This enables runtime configuration without modifying source code.

---

### Example Settings

- Report retention period
- Maximum upload size
- Allowed file extensions
- Default report template
- System maintenance mode

---

### Suggested Fields

| Field | Type |
|---------|------|
| id | UUID |
| setting_key | VARCHAR |
| setting_value | TEXT |
| description | TEXT |
| updated_by | UUID |
| updated_at | TIMESTAMP |

---

# 4.14 Entity Summary

The first version of DRMA-v2 consists of ten core entities that collectively support authentication, authorization, report management, file tracking, auditing, and system configuration.

The model intentionally separates persistent metadata from uploaded files and generated documents, ensuring that PostgreSQL stores structured business data while the filesystem manages large binary assets. This approach simplifies backups, improves performance, and allows future migration to cloud object storage without significant changes to the domain model.

# 5. Relationship Design

## 5.1 Purpose

The Relationship Design defines how the core entities interact within the DRMA-v2 database.

Relationships establish the business rules that govern data ownership, dependency, and referential integrity. They ensure that related information remains consistent throughout the application's lifecycle.

---

## 5.2 Relationship Design Principles

The database relationships follow these principles:

- Maintain referential integrity through foreign key constraints.
- Use one-to-many relationships where a parent entity owns multiple child records.
- Avoid unnecessary many-to-many relationships.
- Cascade updates where appropriate.
- Restrict deletion of records that would compromise historical integrity.
- Preserve auditability by retaining historical references where required.

---

# 5.3 High-Level Entity Relationships

```text
Role
 │
 └──────────────┐
                │
                ▼
              User
                │
                │ creates
                ▼
             Report
      ┌─────────┼───────────────┬───────────────┬──────────────┐
      │         │               │               │              │
      ▼         ▼               ▼               ▼              ▼
ReportUpload ManualInput ReportPreview ReportOutput AuditLog
                                                   ▲
                                                   │
                                             Performed By
                                                   │
                                                   ▼
                                                 User

SystemSetting
      │
Updated By
      ▼
    User

Permission
      ▲
      │
Assigned To
      │
Role
```

---

# 5.4 User and Role Relationship

## Relationship

```text
Role (1)
      │
      │
      └───────────────< User (Many)
```

### Description

A single role may be assigned to multiple users.

Each user is assigned exactly one role.

### Cardinality

| Parent | Child |
|---------|--------|
| Role | User |
| One | Many |

---

### Foreign Key

```text
users.role_id
    → roles.id
```

---

# 5.5 Role and Permission Relationship

## Relationship

```text
Role
   │
   │
Many-to-Many
   │
   ▼
Permission
```

Since relational databases do not directly support many-to-many relationships, an associative table should be introduced.

### Junction Table

```text
role_permissions
```

---

### Structure

| Field |
|---------|
| role_id |
| permission_id |

---

This design enables permissions to be added or removed without modifying application code.

---

# 5.6 User and Report Relationship

## Relationship

```text
User (1)
      │
      │
      └───────────────< Report (Many)
```

### Description

Each report is created by one user.

A user may create many report sessions.

---

### Foreign Key

```text
reports.created_by
        →
users.id
```

---

# 5.7 Report and ReportUpload Relationship

## Relationship

```text
Report (1)

      │

      └───────────────< ReportUpload (Many)
```

---

### Description

A report session may contain multiple uploaded datasets.

Each uploaded dataset belongs to only one report.

---

### Foreign Key

```text
report_uploads.report_id
          →
reports.id
```

---

# 5.8 Report and ManualInput Relationship

## Relationship

```text
Report (1)

      │

      └───────────────< ManualInput (Many)
```

---

### Description

Each report may contain multiple manually entered values.

Every manual input belongs to one report session.

---

### Foreign Key

```text
manual_inputs.report_id
        →
reports.id
```

---

# 5.9 Report and ReportPreview Relationship

## Relationship

```text
Report (1)

      │

      └───────────────< ReportPreview (Many)
```

---

### Description

A report may have multiple previews generated during editing.

Only one preview may be marked as the latest active preview.

Future versions may maintain complete preview history.

---

### Foreign Key

```text
report_previews.report_id
          →
reports.id
```

---

# 5.10 Report and ReportOutput Relationship

## Relationship

```text
Report (1)

      │

      └───────────────< ReportOutput (Many)
```

---

### Description

A report may have multiple generated outputs.

Examples include:

- PDF
- DOCX
- Future versions

Version history can therefore be maintained without replacing previous outputs.

---

### Foreign Key

```text
report_outputs.report_id
        →
reports.id
```

---

# 5.11 User and AuditLog Relationship

## Relationship

```text
User (1)

      │

      └───────────────< AuditLog (Many)
```

---

### Description

Every audit log entry records the user responsible for the action.

Some system-generated events may have a NULL user reference.

---

### Foreign Key

```text
audit_logs.user_id
      →
users.id
```

---

# 5.12 Report and AuditLog Relationship

## Relationship

```text
Report (1)

      │

      └───────────────< AuditLog (Many)
```

---

### Description

Audit records may optionally reference the report affected by an operation.

Examples include:

- Report created
- Report updated
- Preview generated
- Report generated
- Report deleted

---

### Foreign Key

```text
audit_logs.report_id
      →
reports.id
```

---

# 5.13 User and SystemSetting Relationship

## Relationship

```text
User (1)

      │

      └───────────────< SystemSetting (Many)
```

---

### Description

Each configuration change records the administrator who last modified the setting.

---

### Foreign Key

```text
system_settings.updated_by
        →
users.id
```

---

# 5.14 Relationship Summary

| Parent Entity | Child Entity | Cardinality |
|---------------|--------------|-------------|
| Role | User | One-to-Many |
| Role | Permission | Many-to-Many (via role_permissions) |
| User | Report | One-to-Many |
| Report | ReportUpload | One-to-Many |
| Report | ManualInput | One-to-Many |
| Report | ReportPreview | One-to-Many |
| Report | ReportOutput | One-to-Many |
| User | AuditLog | One-to-Many |
| Report | AuditLog | One-to-Many |
| User | SystemSetting | One-to-Many |

---

# 5.15 Referential Integrity Rules

The following integrity rules shall be enforced:

- Every report must reference a valid user.
- Every uploaded dataset must belong to an existing report.
- Every manual input must belong to an existing report.
- Every preview must belong to an existing report.
- Every report output must belong to an existing report.
- Every user must have a valid role.
- Every audit log shall reference a valid user unless generated automatically by the system.
- System settings must record the administrator responsible for the last update.
- Foreign key constraints shall prevent orphaned records and maintain database consistency.

These relationships form the logical foundation of the DRMA-v2 database and support the application's reporting workflow, user management, auditing, and future extensibility.

# 6. Normalization Strategy

## 6.1 Purpose

The normalization strategy ensures that the DRMA-v2 database minimizes data redundancy while preserving data integrity, consistency, and maintainability.

A properly normalized database simplifies updates, reduces storage duplication, and prevents anomalies during data insertion, modification, and deletion.

---

## 6.2 Normalization Objectives

The database design aims to:

- Eliminate duplicate data.
- Maintain a single source of truth.
- Simplify maintenance.
- Improve data consistency.
- Support scalable growth.
- Preserve referential integrity.

---

## 6.3 Normal Form Target

The DRMA-v2 database is designed to satisfy the **Third Normal Form (3NF)**.

This level of normalization provides a balance between:

- Data integrity.
- Query performance.
- Ease of maintenance.
- Future extensibility.

---

# 6.4 First Normal Form (1NF)

### Rule

Each table must:

- Have a primary key.
- Contain atomic values.
- Avoid repeating groups.
- Store one value per column.

### Example

Instead of:

| Report | Uploaded Files |
|----------|----------------|
| R001 | Daily.xlsx, Mobile.xlsx |

The design uses:

**Report**

| id |
|----|
| R001 |

**ReportUpload**

| id | report_id | upload_type |
|----|-----------|-------------|
| 1 | R001 | Daily |
| 2 | R001 | Mobile |

This ensures each upload is stored as an independent record.

---

# 6.5 Second Normal Form (2NF)

### Rule

Every non-key attribute must depend on the entire primary key.

Because DRMA-v2 primarily uses UUID surrogate keys as primary keys, each non-key attribute depends entirely on its entity's identifier.

Example:

```text
Report

id
report_type
status
created_at
```

Each attribute depends only on `Report.id`.

---

# 6.6 Third Normal Form (3NF)

### Rule

Non-key attributes must not depend on other non-key attributes.

Example:

Instead of storing:

```text
Report

created_by_name

created_by_email
```

The database stores:

```text
created_by
```

The related user information is retrieved through the `User` entity.

This eliminates duplicated user information across reports.

---

# 6.7 Eliminating Data Redundancy

Several design decisions reduce duplication.

### Roles

Rather than storing the role name with every user:

```text
Administrator

Report Officer

Auditor
```

The database stores:

```text
role_id
```

which references the `Role` entity.

---

### Upload Metadata

Only upload metadata is stored in PostgreSQL.

Actual files remain on the filesystem.

Metadata includes:

- Original filename.
- File size.
- Upload type.
- Storage path.
- Upload timestamp.

This prevents unnecessary duplication of large binary files within the database.

---

### Generated Reports

Similarly, generated reports are not stored as binary objects in PostgreSQL.

Instead, the database stores:

- Output type.
- Storage path.
- Version.
- Generation timestamp.

This keeps the database lightweight and improves backup efficiency.

---

# 6.8 Lookup Tables

Lookup tables centralize values that are reused throughout the application.

Examples include:

- Roles.
- Permissions.
- Future report categories.
- Future notification types.

Benefits include:

- Reduced duplication.
- Easier maintenance.
- Improved consistency.
- Simpler reporting.

---

# 6.9 Controlled Denormalization

Although 3NF is the primary design goal, controlled denormalization may be introduced where justified.

Possible future cases include:

- Analytics dashboards.
- Reporting summaries.
- Frequently accessed statistics.
- Materialized views.

Such optimizations should only be introduced after performance analysis demonstrates a clear benefit.

---

# 6.10 Benefits of the Normalization Strategy

The adopted normalization approach provides:

- Consistent data.
- Reduced redundancy.
- Simplified updates.
- Lower storage requirements.
- Improved maintainability.
- Clear relationships between entities.
- Easier future enhancements.

---

# 7. Indexing Strategy

## 7.1 Purpose

Indexes improve query performance by enabling PostgreSQL to locate records efficiently without scanning entire tables.

The indexing strategy focuses on optimizing the application's most common operations while avoiding excessive indexing that could negatively impact write performance.

---

## 7.2 Indexing Principles

Indexes should:

- Support frequently executed queries.
- Optimize search and filtering.
- Improve join performance.
- Minimize duplicate indexes.
- Balance read and write performance.

---

# 7.3 Primary Key Indexes

Every primary key automatically creates a unique index.

Examples:

```text
users.id

roles.id

reports.id

report_uploads.id

manual_inputs.id

report_previews.id

report_outputs.id

audit_logs.id

system_settings.id
```

These indexes support rapid entity retrieval by identifier.

---

# 7.4 Foreign Key Indexes

Foreign key columns should be indexed to improve join performance.

Recommended indexes include:

| Table | Indexed Column |
|---------|----------------|
| users | role_id |
| reports | created_by |
| report_uploads | report_id |
| manual_inputs | report_id |
| report_previews | report_id |
| report_outputs | report_id |
| audit_logs | user_id |
| audit_logs | report_id |
| system_settings | updated_by |

These indexes accelerate relationship-based queries throughout the application.

---

# 7.5 Unique Indexes

Certain fields require uniqueness to maintain data integrity.

Examples include:

| Table | Column |
|---------|--------|
| users | username |
| users | email |
| roles | name |
| permissions | permission_name |
| system_settings | setting_key |

Unique indexes prevent duplicate values for these business-critical fields.

---

# 7.6 Search Indexes

Additional indexes should support common search operations.

Examples:

### Reports

- report_number
- report_date
- report_type
- status

### Users

- full_name
- username

### Audit Logs

- action
- created_at

These indexes improve filtering and search responsiveness.

---

# 7.7 Composite Indexes

Composite indexes optimize queries that filter by multiple columns.

Recommended examples:

| Table | Columns |
|---------|---------|
| reports | report_type, report_date |
| reports | status, created_at |
| report_outputs | report_id, version |
| audit_logs | user_id, created_at |

Composite indexes should reflect actual query patterns identified during implementation.

---

# 7.8 Future Full-Text Search

Future versions may support PostgreSQL Full-Text Search for:

- Report remarks.
- Audit log details.
- Manual input values.
- User search.

Dedicated text indexes can be introduced when advanced search capabilities are implemented.

---

# 7.9 Monitoring Index Usage

Index performance should be reviewed periodically.

Unused or redundant indexes should be removed to:

- Reduce storage consumption.
- Improve insert and update performance.
- Simplify maintenance.

Database performance monitoring tools should guide future indexing decisions.

---

# 7.10 Indexing Summary

The indexing strategy combines:

- Automatic primary key indexes.
- Foreign key indexes.
- Unique indexes.
- Search indexes.
- Composite indexes.

This approach supports efficient report management, user administration, auditing, and analytics while maintaining a balance between query performance and write efficiency.

# 8. Constraints & Integrity Rules

## 8.1 Purpose

Constraints enforce the business rules and maintain the integrity of data stored within the DRMA-v2 database.

They ensure that only valid, consistent, and reliable data can exist throughout the application's lifecycle.

---

## 8.2 Integrity Principles

The database shall enforce the following principles:

- Every record must have a unique identifier.
- Relationships must remain valid through foreign key constraints.
- Required information shall not be left empty.
- Duplicate business-critical values shall be prevented.
- Invalid reference data shall not be stored.
- Historical records shall be preserved whenever appropriate.

---

# 8.3 Primary Key Constraints

Each entity shall contain a primary key.

Example:

```text
users.id

roles.id

permissions.id

reports.id

report_uploads.id

manual_inputs.id

report_previews.id

report_outputs.id

audit_logs.id

system_settings.id
```

Primary keys uniquely identify each record.

---

# 8.4 Foreign Key Constraints

Foreign keys enforce relationships between entities.

Examples:

```text
users.role_id
        →
roles.id
```

```text
reports.created_by
        →
users.id
```

```text
report_uploads.report_id
        →
reports.id
```

```text
manual_inputs.report_id
        →
reports.id
```

```text
report_previews.report_id
        →
reports.id
```

```text
report_outputs.report_id
        →
reports.id
```

```text
audit_logs.user_id
        →
users.id
```

```text
audit_logs.report_id
        →
reports.id
```

```text
system_settings.updated_by
        →
users.id
```

These constraints prevent orphaned records and maintain relational consistency.

---

# 8.5 NOT NULL Constraints

Business-critical fields shall require values.

Examples include:

### Users

- username
- full_name
- email
- password_hash
- role_id

### Reports

- report_type
- report_date
- status
- created_by

### Uploads

- report_id
- upload_type
- storage_path

Fields that are optional, such as remarks or audit references for automated events, may allow NULL values.

---

# 8.6 UNIQUE Constraints

The following values shall remain unique across the system.

| Table | Column |
|---------|--------|
| users | username |
| users | email |
| roles | name |
| permissions | permission_name |
| system_settings | setting_key |
| reports | report_number |

These constraints prevent duplicate business records.

---

# 8.7 CHECK Constraints

Where appropriate, CHECK constraints shall enforce valid values.

Examples include:

### Report Status

Allowed values:

```text
Draft

Uploading

Ready

Generating Preview

Preview Ready

Generating Report

Completed

Archived

Deleted
```

---

### Upload Status

Allowed values:

```text
Pending

Uploaded

Validated

Failed
```

---

### Output Type

Allowed values:

```text
PDF

DOCX
```

Additional formats may be introduced in future releases.

---

# 8.8 Referential Actions

The following referential actions are recommended.

| Relationship | Action |
|--------------|--------|
| Role → User | RESTRICT |
| User → Report | RESTRICT |
| Report → Upload | CASCADE |
| Report → Manual Input | CASCADE |
| Report → Preview | CASCADE |
| Report → Output | CASCADE |
| Report → Audit Log | RESTRICT |
| User → Audit Log | SET NULL (System Events) |

This strategy balances data consistency with preservation of historical records.

---

# 8.9 Business Integrity Rules

The application shall enforce additional business rules, including:

- A report cannot be generated until all required datasets have been uploaded.
- A completed report cannot be modified without appropriate authorization.
- Only authorized users may delete reports.
- Deleted reports shall remain traceable through audit logs.
- Every report shall be associated with a valid creator.
- Uploaded files shall belong to only one report session.
- Generated outputs shall reference an existing report.

---

# 8.10 Data Integrity Summary

Database constraints provide the first layer of validation.

Application-level validation complements these constraints by enforcing workflow-specific business rules before data is written to the database.

Together, these mechanisms ensure the consistency, reliability, and integrity of DRMA-v2 data.

---

# 9. Migration Strategy

## 9.1 Purpose

The migration strategy defines how database schema changes are introduced, versioned, and deployed throughout the project's lifecycle.

A controlled migration process ensures consistency across development, testing, and production environments.

---

## 9.2 Migration Principles

Database migrations should:

- Be version-controlled.
- Be repeatable.
- Be reversible where practical.
- Be automated during deployment.
- Preserve existing data whenever possible.

---

## 9.3 Migration Tool

DRMA-v2 uses **Alembic** as the database migration framework in conjunction with **SQLAlchemy**.

Alembic provides:

- Versioned schema changes.
- Upgrade and downgrade support.
- Change history.
- Team collaboration through migration files.

---

## 9.4 Migration Workflow

The standard migration workflow is:

```text
Update SQLAlchemy Models
        │
        ▼
Generate Migration
        │
        ▼
Review Migration Script
        │
        ▼
Apply Migration
        │
        ▼
Verify Schema
        │
        ▼
Commit Migration
```

Each migration should be reviewed before being committed to the repository.

---

## 9.5 Version Control

Migration scripts shall be stored alongside the application source code.

Each migration represents a single logical schema change and should include:

- A descriptive revision identifier.
- Upgrade logic.
- Downgrade logic where feasible.

---

## 9.6 Deployment

Database migrations should execute automatically as part of the deployment pipeline before the application starts.

If a migration fails, the deployment should halt until the issue is resolved.

---

## 9.7 Migration Best Practices

The project should:

- Keep migrations small and focused.
- Avoid modifying applied migration files.
- Test migrations before deployment.
- Maintain backward compatibility when possible.
- Document significant schema changes.

---

# 10. Backup & Recovery Considerations

## 10.1 Purpose

The backup and recovery strategy ensures that DRMA-v2 data can be restored in the event of hardware failure, software failure, accidental deletion, or disaster.

---

## 10.2 Backup Scope

Regular backups should include:

- PostgreSQL database.
- Uploaded datasets.
- Generated reports.
- Application configuration.
- Migration history.

---

## 10.3 Backup Strategy

Recommended practices include:

- Scheduled automated database backups.
- Filesystem backups for uploaded files.
- Secure off-site backup storage.
- Periodic backup verification.
- Retention policies aligned with organizational requirements.

---

## 10.4 Recovery Objectives

Recovery procedures should aim to:

- Restore database integrity.
- Recover uploaded datasets.
- Recover generated reports.
- Minimize downtime.
- Minimize data loss.

---

## 10.5 Recovery Testing

Backup restoration should be tested periodically to verify:

- Backup completeness.
- Restoration procedures.
- Data consistency.
- Recovery time objectives.

Routine testing ensures backups remain usable when needed.

---

# 11. Future Extensibility

## 11.1 Purpose

The database architecture is designed to accommodate future functional growth while minimizing disruptive schema changes.

---

## 11.2 Planned Extensions

Future releases may introduce:

- Additional report types.
- Notification management.
- Workflow approvals.
- Digital signatures.
- External system integrations.
- Dashboard analytics.
- AI-generated operational insights.
- Multi-tenant deployments.

The current schema provides a flexible foundation for these enhancements.

---

## 11.3 Scalability Considerations

As the application grows, the database can be enhanced through:

- Table partitioning for large datasets.
- Read replicas for reporting workloads.
- Materialized views for analytics.
- Advanced indexing strategies.
- Cloud object storage integration for uploaded files and report outputs.

---

## 11.4 Design Stability

The separation of business entities, metadata, and filesystem assets enables the database to evolve independently from storage infrastructure and presentation components.

This architectural separation reduces maintenance effort and supports long-term scalability.

---

# Document Summary

This document defines the database architecture for DRMA-v2, including its design principles, logical data model, entity definitions, relationships, normalization strategy, indexing approach, integrity constraints, migration process, backup considerations, and future extensibility.

Together with the Software Requirements Specification (SRS), System Architecture, Backend Architecture, Frontend Architecture, and API Design, this document provides the foundation for implementing a reliable, scalable, and maintainable PostgreSQL database that supports the operational and reporting requirements of DRMA-v2.