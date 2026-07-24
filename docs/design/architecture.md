# Architecture Design

**Project:** DRMA-v2 (Danka Report Management Application v2)

**Version:** 1.0

**Status:** Draft

**Author:** Wade

**Last Updated:** 2026-07-22

---

# 1. Purpose

This document defines the architectural design of the Danka Report Management Application Version 2 (DRMA-v2).

It describes how the system is organized into logical components, how those components interact, and the engineering principles that guide implementation.

The objective is to establish a shared architectural vision for the development team before implementation begins.

This document complements the Software Requirements Specification (SRS) by describing **how** the system will be engineered to satisfy the documented requirements.

It also serves as the primary reference for future design decisions, ensuring that new features follow a consistent architectural approach.

# 2. Architectural Goals

The architecture of DRMA-v2 is designed to achieve the following goals:

- Maintainability through modular and well-defined components.
- Scalability to support future report types, integrations, and increased usage.
- Reliability by ensuring predictable system behavior and fault isolation.
- Extensibility for introducing new modules without major redesign.
- Testability through clear separation of responsibilities.
- Security by enforcing centralized authentication, authorization, and audit logging.
- Performance through efficient processing of uploaded datasets and report generation.
- Consistency by standardizing APIs, error handling, logging, and data management.

# 3. Architecture Principles

The architectural design of DRMA-v2 is guided by a set of engineering principles that promote maintainability, scalability, security, and consistency throughout the system. These principles apply to all modules and should be followed by every contributor.

---

## 3.1 Separation of Concerns

Each component of the system shall have a clearly defined responsibility.

Business logic, presentation, data persistence, report generation, and infrastructure concerns shall remain independent wherever possible.

This separation improves readability, maintainability, and testability.

---

## 3.2 Single Responsibility Principle (SRP)

Every module, service, class, and function should have one primary responsibility.

Changes to one business capability should affect only the corresponding module and not unrelated parts of the system.

---

## 3.3 Modular Design

The application shall be organized into independent business modules rather than technical layers alone.

Examples of modules include:

- Authentication
- Report Sessions
- File Uploads
- Report Processing
- Report Builder
- SMS Summaries
- Analytics
- Administration
- AI Assistant
- Audit Logging

Each module owns its business logic and exposes only the interfaces required by other modules.

---

## 3.4 Loose Coupling

Modules should depend on well-defined interfaces instead of implementation details.

Direct dependencies between unrelated modules should be minimized.

This allows components to evolve independently and simplifies testing and future refactoring.

---

## 3.5 High Cohesion

Closely related functionality should remain within the same module.

Each module should represent a complete business capability rather than a collection of unrelated utilities.

---

## 3.6 Layered Responsibility

The application follows a layered architecture internally.

Each layer has a specific responsibility:

- Presentation Layer – User interface and API endpoints.
- Application Layer – Coordinates business workflows and use cases.
- Domain Layer – Contains business rules and report-generation logic.
- Infrastructure Layer – Provides access to databases, file storage, logging, and external services.

Dependencies shall flow inward toward the domain layer.

---

## 3.7 Interface-Driven Communication

Communication between modules shall occur through clearly defined service interfaces.

Modules should avoid directly accessing another module's internal implementation.

This promotes loose coupling and simplifies future architectural evolution.

---

## 3.8 Fail Fast and Validate Early

Input validation shall occur as early as possible.

Invalid uploads, malformed requests, missing data, and unsupported formats shall be rejected immediately with descriptive error messages.

Early validation reduces unnecessary processing and improves user feedback.

---

## 3.9 Security by Design

Security shall be considered throughout the architecture rather than added after implementation.

Examples include:

- Role-based access control.
- Authentication for protected endpoints.
- Authorization for administrative actions.
- Secure password handling.
- Audit logging of sensitive operations.
- Input validation to reduce security risks.

---

## 3.10 Observability

The system shall provide sufficient visibility into its operation.

Important activities such as uploads, report generation, authentication events, deletions, and system failures shall generate structured logs.

Audit logs shall record sensitive administrative actions separately from operational logs.

---

## 3.11 Extensibility

The architecture shall support future expansion without requiring major redesign.

Examples include:

- New report types.
- Additional upload formats.
- AI-powered analysis.
- Notification services.
- Cloud storage providers.
- External integrations.

Future functionality should be introduced by extending existing modules or adding new modules rather than modifying unrelated components.

---

## 3.12 Consistency

Coding standards, API responses, validation behavior, logging formats, naming conventions, and error handling shall remain consistent throughout the application.

Consistency improves maintainability, developer productivity, and user experience.

# 4. High-Level Architecture

## 4.1 Architectural Style

DRMA-v2 adopts a **Modular Monolith** architecture with a **Layered Architecture** internally.

The application is developed and deployed as a single system while being organized into independent business modules with clearly defined responsibilities.

This approach balances maintainability, scalability, and deployment simplicity, while allowing future extraction of modules into independent services if required.

---

## 4.2 Architectural Layers

The system is organized into four logical layers:

- **Presentation Layer** – Provides the web interface and REST API endpoints.
- **Application Layer** – Coordinates workflows and orchestrates business operations.
- **Domain Layer** – Implements business rules, validations, and report-generation logic.
- **Infrastructure Layer** – Integrates with PostgreSQL, filesystem storage, logging, and external services.

Dependencies flow inward, ensuring that business rules remain independent of infrastructure concerns.

---

## 4.3 Feature Modules

The system is divided into the following business modules:

- Authentication
- Report Session Management
- File Upload Management
- Data Processing
- Report Builder
- Preview Generation
- SMS Summary
- Analytics
- Administration
- AI Assistant (Future)
- Audit Logging

Each module encapsulates its own responsibilities and exposes only the interfaces required by other modules.

---

## 4.4 Module Communication

Modules communicate through well-defined service interfaces within the application layer.

Direct access to another module's internal implementation is discouraged. Shared functionality is provided through reusable services or infrastructure components where appropriate.

This approach promotes loose coupling, high cohesion, and easier maintenance.

# 5. Architecture Constraints

The following constraints define mandatory engineering rules for the DRMA-v2 project. These constraints are intended to preserve architectural consistency, improve maintainability, and ensure that future enhancements remain aligned with the overall system design.

Failure to adhere to these constraints should be considered an architectural violation during code review.

---

## 5.1 Module Independence

Each business module shall own its responsibilities and internal implementation.

Modules shall communicate only through well-defined interfaces or application services.

Direct dependencies on another module's internal classes, repositories, or implementation details are prohibited.

---

## 5.2 Business Logic Placement

Business rules shall reside within the Domain and Application layers.

API controllers, route handlers, and frontend components shall not contain business logic.

Their responsibility is limited to receiving requests, validating input, invoking the appropriate service, and returning responses.

---

## 5.3 Database Access

All database operations shall be performed through the repository layer.

Direct SQL queries from controllers, services outside the repository layer, or frontend components are prohibited.

Repositories shall abstract persistence details from the rest of the application.

---

## 5.4 API Design Standards

All REST endpoints shall follow a consistent structure for:

- Request validation
- Response formatting
- Error handling
- HTTP status codes
- Pagination
- Authentication and authorization

API contracts shall be documented before implementation.

Breaking changes shall require versioning or a documented migration strategy.

---

## 5.5 File Storage

Uploaded files, generated reports, previews, and temporary processing artifacts shall be stored on the filesystem or approved storage providers.

PostgreSQL shall store metadata only.

Binary report files shall not be stored inside the relational database.

---

## 5.6 Report Generation

Report generation shall remain modular.

Each report section shall be implemented as an independent reusable component capable of appending its content to an existing report document.

Report templates shall remain immutable during generation.

Generated reports shall be assembled from reusable sections rather than standalone report builders.

---

## 5.7 Security

Administrative operations shall require appropriate authorization.

Sensitive operations including report deletion, configuration changes, and administrative management shall be protected.

Secrets, passwords, API keys, and connection strings shall never be hardcoded.

Security-sensitive events shall be recorded in the audit log.

---

## 5.8 Audit Logging

The system shall record an audit trail for significant actions, including but not limited to:

- User authentication
- Report creation
- Report deletion
- Report generation
- Upload failures
- Administrative actions
- Configuration changes

Audit records shall be immutable and available for future review.

---

## 5.9 AI Integration

Artificial Intelligence capabilities shall remain optional system components.

The core report generation workflow shall continue functioning even when AI services are unavailable.

External AI providers shall be accessed through dedicated integration interfaces rather than directly throughout the application.

---

## 5.10 Documentation

Major architectural decisions shall be documented using Architecture Decision Records (ADRs).

New features shall include updates to the relevant design documentation before implementation.

Public APIs, database schema changes, and deployment changes shall be reflected in the project documentation.

---

## 5.11 Coding Standards

The project shall follow agreed coding conventions, naming standards, formatting rules, and repository structure.

Code reviews shall verify compliance with these standards before changes are merged.

---

## 5.12 Future Compatibility

The architecture shall support future extension without requiring major redesign.

New report types, analytics modules, notification services, AI capabilities, and storage providers should be added through extension rather than modification of unrelated modules.

Backward compatibility should be maintained where practical.

# 6. Major Architectural Components

DRMA-v2 follows a modular architecture in which each subsystem is responsible for a specific part of the application's functionality. This separation of concerns reduces coupling, improves maintainability, and allows individual modules to evolve independently.

## 6.1 Frontend Application

### Purpose

Provides the user interface through which users interact with the system.

### Responsibilities

- User authentication
- Report session creation
- File uploads
- Manual data entry
- Progress tracking
- Preview display
- Final report download
- Report history
- Dashboard visualization
- Administrative pages

The frontend communicates exclusively through the Backend REST API and does not access the database directly.

---

## 6.2 Backend API

### Purpose

Acts as the central application layer coordinating all business operations.

### Responsibilities

- Request validation
- Authentication and authorization
- Business rule enforcement
- Report session management
- Upload orchestration
- Report generation
- Preview generation
- SMS summary generation
- Analytics
- Audit logging
- Error handling

The backend is the only component permitted to interact with the database and filesystem.

---

## 6.3 Report Processing Engine

### Purpose

Transforms uploaded operational datasets into standardized report sections.

### Responsibilities

- Data validation
- Data cleaning
- Data normalization
- Business calculations
- Statistical summaries
- Section generation
- Report assembly

The processing engine is independent of the web framework and focuses solely on report generation logic.

---

## 6.4 Document Generation Module

### Purpose

Produces professional Word, Excel, PDF, and image outputs.

### Responsibilities

- Word document creation
- Excel workbook creation
- PDF preview generation
- PNG preview generation
- Page formatting
- Tables
- Charts
- Headers and footers
- Page numbering

This module ensures all generated documents conform to Danka reporting standards.

---

## 6.5 File Storage Service

### Purpose

Manages persistent storage of uploaded files and generated outputs.

### Responsibilities

- Upload storage
- Preview storage
- Final report storage
- Temporary working files
- Storage cleanup
- File retrieval

The storage layer manages file paths while PostgreSQL stores only metadata.

---

## 6.6 PostgreSQL Database

### Purpose

Stores all persistent application metadata.

### Responsibilities

- Report metadata
- Upload metadata
- Manual input data
- Preview metadata
- Output metadata
- User accounts
- Roles
- Permissions
- Audit logs
- Application settings

Large binary files are intentionally excluded from database storage.

---

## 6.7 Authentication & Authorization Service

### Purpose

Controls system access based on user identity and assigned permissions.

### Responsibilities

- User authentication
- Session management
- Password security
- Role-based authorization
- Permission validation
- Administrative access control

Every protected backend endpoint validates the authenticated user's permissions before processing requests.

---

## 6.8 Audit Logging Service

### Purpose

Maintains a permanent record of significant application events for accountability and traceability.

### Responsibilities

- Login tracking
- Report creation
- Report modification
- Report deletion
- File uploads
- Report generation
- Administrative actions
- Failed operations
- Security events

Audit logs are immutable and accessible only to authorized administrators.

---

## 6.9 Notification Service

### Purpose

Provides user feedback regarding application events.

### Responsibilities

- Upload completion notifications
- Processing status updates
- Validation errors
- Success confirmations
- Report completion notifications
- Background task status

Initially, notifications will be delivered within the web application. Future versions may support email or SMS notifications.

---

## 6.10 Analytics Module

### Purpose

Provides operational insights derived from stored reporting data.

### Responsibilities

- Dashboard statistics
- Historical trends
- Report counts
- Performance metrics
- Operational summaries

Analytics uses report metadata stored in PostgreSQL without directly accessing uploaded datasets.

---

## Component Interaction Overview

The components collaborate according to the following flow:

1. Users interact with the Frontend Application.
2. The Frontend sends requests to the Backend API.
3. The Backend authenticates the user and validates the request.
4. Uploaded files are stored by the File Storage Service.
5. The Report Processing Engine transforms uploaded data.
6. The Document Generation Module creates previews and final reports.
7. Metadata is persisted in PostgreSQL.
8. Audit Logging records significant events.
9. Notifications inform users of processing progress.
10. Analytics reads metadata to generate dashboards and reports.

This layered interaction ensures clear separation of responsibilities and supports scalability, maintainability, and future extensibility.

# 7. Data Flow

## 7.1 Overview

This section describes how data moves through DRMA-v2 during normal system operation.

Rather than focusing on implementation details, the purpose of this section is to illustrate how information travels between users, application components, storage services, and generated outputs.

Understanding these flows helps developers reason about system behaviour, identify dependencies between components, and design scalable processing pipelines.

The primary business flows within DRMA-v2 include:

- Report Creation Flow
- Upload & Processing Flow
- Preview Generation Flow
- Final Report Generation Flow
- Dashboard & Analytics Flow
- Audit Logging Flow
- AI Assistance Flow (Future)

---

## 7.2 Report Creation Flow

The report creation flow initializes a new reporting session before any operational data is uploaded.

Workflow:

User
    │
    ▼
Frontend
    │
POST /report-sessions
    │
    ▼
Backend API
    │
    ▼
PostgreSQL
(Store Report Metadata)
    │
    ▼
Return report_id
    │
    ▼
Frontend Opens Upload Workspace

Stored Metadata

- Report Date
- Station
- Bound
- Report Type
- Created By
- Session Status
- Timestamp

Output

A unique Report Session is created and becomes the parent record for all subsequent uploads, previews, reports, and audit logs.

## 7.3 Upload & Processing Flow

Operational datasets are uploaded independently and processed as separate report sections.

Workflow

User
    │
Select CSV/XLSX
    │
    ▼
Frontend Upload Component
    │
    ▼
Upload API
    │
    ▼
Validation Engine
    │
    ▼
Processing Engine
    │
    ├────────► PostgreSQL Metadata
    │
    ├────────► File Storage
    │
    └────────► Section Status Updated

Processing Steps

1. Upload file
2. Validate file format
3. Validate required columns
4. Validate business rules
5. Clean data
6. Generate processed dataset
7. Save processed metadata
8. Update report session

Output

Each successfully processed upload becomes available for preview generation and final report assembly.

## 7.4 Preview Generation Flow

After a report section has been successfully processed, preview documents may be generated for user review.

Workflow

Processed Dataset
      │
      ▼
Preview Generator
      │
      ├────────► DOCX
      ├────────► PDF
      └────────► PNG

      │
      ▼
Filesystem Storage

      │
      ▼
Preview Metadata

      │
      ▼
Frontend Preview Panel

Users can review generated previews before building the complete report.

## 7.5 Final Report Generation Flow

Once all mandatory report sections are available, the user may generate the complete report.

Workflow

Frontend
      │
Build Final Report
      │
      ▼
Final Report Builder

      │
      ▼
Load Required Sections

      │
      ▼
Assemble Word Document

      │
      ▼
Generate Excel Workbook

      │
      ▼
Update Page Numbers

      │
      ▼
Save Generated Files

      │
      ▼
Update PostgreSQL Metadata

      │
      ▼
Return Download URL

Output

The system generates standardized Microsoft Word and Excel reports following the official Danka reporting templates.

## 7.6 Dashboard & Analytics Flow

Operational statistics are continuously aggregated as report sessions are processed.

Workflow

Processed Reports
        │
        ▼
Analytics Service

        │
        ▼
Dashboard Queries

        │
        ▼
PostgreSQL

        │
        ▼
Dashboard API

        │
        ▼
Frontend Dashboard

Displayed Information

- Total Reports
- Total Weighed
- Total Overloaded
- Wide Loads
- Special Releases
- Report History
- Daily Statistics

## 7.7 Audit Logging Flow

All significant user and system activities are recorded in the Audit Logging Service.

Workflow

User Action
      │
      ▼
Application Service
      │
      ▼
Audit Logger
      │
      ▼
PostgreSQL Audit Tables

Logged Events

- User Login
- User Logout
- Report Created
- File Uploaded
- Upload Failed
- Validation Error
- Manual Input Updated
- Preview Generated
- Final Report Generated
- SMS Generated
- Report Deleted
- Administrative Actions

Purpose

Audit logs provide accountability, security monitoring, troubleshooting, and historical traceability.

## 7.8 AI Assistance Flow (Future)

Future releases may introduce AI-powered assistance for report officers and administrators.

Potential Workflow

User
      │
      ▼
AI Assistant

      │
      ├────────► Explain Validation Errors
      ├────────► Detect Data Anomalies
      ├────────► Suggest Corrections
      ├────────► Answer User Questions
      └────────► Generate Operational Insights

The AI module will consume processed report data and system metadata while respecting user permissions and organizational security policies.

AI recommendations will remain advisory; final operational decisions will always require user confirmation.

