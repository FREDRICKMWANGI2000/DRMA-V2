# Software Requirements Specification (SRS)

**Project:** DRMA-v2 (Danka Report Management Application v2)

**Version:** 1.0

**Status:** Draft

**Author:** Wade

**Last Updated:** 2026-07-20

# 1. Introduction

## 1.1 Purpose

This Software Requirements Specification (SRS) defines the functional and non-functional requirements for the Danka Report Management Application Version 2 (DRMA-v2).

The document serves as the primary reference for software design, implementation, testing, and future maintenance. It establishes a common understanding of what the application must accomplish without prescribing how the solution will be implemented.

This document should be read together with the Product Overview, which describes the business vision, objectives, stakeholders, and overall product goals.

## 1.2 Scope

DRMA-v2 is a web-based report management platform designed to automate the generation of weighbridge operational reports.

The system enables authorized users to create report sessions, upload operational datasets, validate uploaded data, generate previews, build standardized reports, manage report history, and produce SMS summaries.

The application replaces manual report preparation with an automated workflow that improves consistency, reduces processing time, minimizes human error, and centralizes report management.

The application supports multiple report types including static weighbridge reports and mobile weighbridge reports while maintaining a consistent reporting workflow.

## 1.3 Related Project Documents

This document references the following project documentation:

- Product Overview
- System Architecture
- Database Design
- API Specification
- Frontend Architecture
- Backend Architecture
- AI Design
- Security Design
- Testing Strategy
- Deployment Architecture
- Development Guidelines

The Product Overview remains the authoritative source for business objectives, stakeholders, user roles, and project vision.

# 2. References
The following documents and resources were used during the requirements gathering and system analysis process for DRMA v2.

## 2.1 Existing DRMA Backend

The existing DRMA backend serves as the reference implementation for understanding the business workflow, report-generation process, API structure, and existing features.

**Purpose**
- Understand the current report generation workflow
- Identify reusable concepts
- Discover existing limitations
- Preserve business rules during redesign

## 2.2 Backend–Frontend Integration Guide
Describes:
- Upload workflow
- Report session lifecycle
- Preview generation
- Final report generation
- API endpoints
- Error handling
- Future planned features

**Purpose**
Primary source for defining frontend functional requirements.

## 2.3 PostgreSQL Integration Guide

This document describes the persistence strategy adopted by the reference implementation, including metadata storage, report history, analytics, and deployment considerations.

**Purpose**
Provides the basis for designing the persistence layer of DRMA v2.

## 2.4 Existing Report Templates
Includes the official Word and Excel report templates currently used by Danka Services.

**Purpose**
Defines the required report layout, formatting standards, and generated outputs.

## 2.5 User Interviews and Domain Knowledge
Requirements collected from:

- Report preparation workflow
- Daily operational procedures
- Manual reporting practices
- Administrative requirements

**Purpose**
Capture real-world business processes not represented in software.

## 2.6 Project Documentation
Internal project documentation including:

- Product Overview
- Architecture Design
- Database Design
- API Specification
- UI/UX Design
- Project Journal
- Session Handoff
- Changelog

**Purpose**

Provide design decisions and implementation guidance for the development team.

## 3.1 Product Perspective

DRMA-v2 (Danka Report Management Application Version 2) is a complete redesign of the existing Danka report generation platform.

The current DRMA implementation serves as a reference system for business workflows and reporting requirements. However, DRMA-v2 is treated as a new software product with its own architecture, documentation, implementation strategy, and long-term maintenance plan.

The application will provide a centralized platform for:

- Report session management
- Data upload and validation
- Automated report generation
- Report previewing
- Historical report retrieval
- Administrative management
- Dashboard analytics
- SMS summary generation

The platform will consist of:

### Frontend Application

A web-based user interface responsible for:

- User interaction
- Session management
- File uploads
- Preview visualization
- Report downloads
- Administrative operations

### Backend Application

A REST API responsible for:

- Business logic
- Data validation
- Report generation
- File management
- Database operations
- Analytics calculations

### PostgreSQL Database

Persistent storage for:

- Report metadata
- Upload metadata
- Manual inputs
- Generated outputs
- Audit information

### File Storage Layer

Filesystem-based storage for:

- Uploaded source files
- Processed datasets
- Preview documents
- Generated reports

Future versions may support cloud object storage.

### Reporting Engine

Responsible for:

- Word report generation
- Excel workbook generation
- PDF preview generation
- SMS summary generation

### Analytics Module

Provides:

- Operational statistics
- Historical trends
- Dashboard metrics
- Reporting insights

The system is intended to support future expansion without major architectural redesign.

## 3.2 Product Functions

The primary functions of DRMA-v2 include:

### Report Session Management

The system shall allow users to:

- Create report sessions
- Edit report metadata
- Save draft reports
- Resume existing report sessions
- Delete report sessions (authorized users only)

### File Upload Management

The system shall allow users to:

- Upload required datasets
- Upload optional datasets
- Replace uploaded files
- Validate uploaded files
- View upload status

### Data Processing

The system shall:

- Parse uploaded files
- Validate file structure
- Validate required columns
- Detect missing values
- Process datasets for report generation

### Report Preview Generation

The system shall:

- Generate section previews
- Generate PDF previews
- Generate image previews
- Allow preview retrieval without rebuilding reports

### Report Generation

The system shall:

- Generate complete Word reports
- Generate Excel outputs where required
- Generate SMS summaries
- Assemble reports from validated sections

### Dashboard Analytics

The system shall:

- Display operational statistics
- Display historical trends
- Display report completion metrics
- Display upload statistics

### Administrative Management

The system shall:

- Provide report history
- Allow report deletion
- Allow report searching
- Provide system monitoring tools

### Audit and Tracking

The system shall:

- Record report creation activity
- Record upload activity
- Record report generation activity
- Maintain historical metadata

### Future AI-Assisted Features

Future releases may include:

- Automated anomaly detection
- Report quality validation
- Natural language report summaries
- Predictive operational analytics

## 3.3 User Classes and Characteristics

The system will support multiple categories of users.

### Report Officer

Primary user responsible for generating reports.

Responsibilities:

- Creating report sessions
- Uploading datasets
- Reviewing validation results
- Generating reports
- Downloading completed outputs

Technical Skill Level:

- Basic computer literacy
- Limited technical knowledge

### Supervisor

Responsible for reviewing generated reports.

Responsibilities:

- Reviewing report outputs
- Monitoring reporting performance
- Approving operational results

Technical Skill Level:

- Moderate computer literacy

### Administrator

Responsible for system management.

Responsibilities:

- Managing report history
- Deleting invalid reports
- Monitoring system health
- Supporting users

Technical Skill Level:

- Advanced system knowledge

### System Developer

Responsible for maintaining and extending the application.

Responsibilities:

- Bug fixes
- Feature implementation
- Performance optimization
- Infrastructure maintenance

Technical Skill Level:

- Software engineering expertise

## 3.4 Operating Environment

DRMA-v2 is designed as a web-based client-server application accessible through modern web browsers. The system will initially be deployed within the Danka Services operational environment but should remain portable enough to support deployment in cloud or on-premises infrastructure.

### Client Environment

Users access the application through a supported web browser.

Supported browsers include:

- Google Chrome
- Microsoft Edge
- Mozilla Firefox

The frontend should provide a responsive interface suitable for desktop and laptop computers. Mobile device support may be introduced for administrative and monitoring features in future releases.

### Application Server

The backend application will run as a RESTful API service built with FastAPI.

Responsibilities include:

- Processing uploaded files
- Executing business logic
- Generating reports
- Managing report sessions
- Serving preview files
- Communicating with PostgreSQL

### Database Environment

Persistent application data shall be stored in PostgreSQL.

The database stores metadata only and does not store uploaded files or generated reports.

### Storage Environment

Uploaded datasets, generated reports, previews, and temporary processing artifacts shall be stored on persistent filesystem storage.

Future versions may replace filesystem storage with cloud object storage without requiring significant architectural changes.

### Deployment Environment

Development environments may use:

- Linux
- Windows (via WSL)
- macOS

Production deployments should support containerized environments using Docker and cloud-hosted infrastructure where appropriate.

### Network Requirements

The frontend communicates with the backend over HTTPS.

The backend communicates securely with PostgreSQL and any future external services.

All communication should support authenticated and encrypted connections in production environments.

## 3.5 Design and Implementation Constraints

The design and implementation of DRMA-v2 shall comply with the following constraints.

### Business Constraints

- Generated reports must conform to the official Danka Services reporting templates.
- Report calculations must preserve existing business rules unless formally approved for change.
- Report layouts must remain consistent across all generated outputs.

### Technical Constraints

- Backend development shall use Python and FastAPI.
- Frontend development shall use React with TypeScript.
- PostgreSQL shall serve as the primary relational database.
- Uploaded files and generated reports shall remain on filesystem storage during Version 2.
- The application shall expose RESTful APIs for frontend communication.

### Security Constraints

- Administrative functions shall require elevated authorization.
- Sensitive configuration values shall be managed through environment variables.
- User input shall be validated before processing.
- Uploaded files shall be validated before storage and processing.

### Performance Constraints

- File uploads should complete without blocking the user interface.
- Long-running report generation tasks should expose processing status.
- Report previews should be cached where appropriate to improve response time.

### Maintainability Constraints

The application shall adopt a modular architecture that separates:

- Presentation logic
- Business logic
- Data access
- Report generation
- Infrastructure concerns

The codebase shall prioritize readability, testability, and extensibility over premature optimization.

## 3.6 Assumptions and Dependencies

The following assumptions apply to Version 2 of DRMA-v2.

### Assumptions

- Users possess the necessary operational knowledge to prepare weighbridge reports.
- Required datasets will be exported in supported CSV or XLSX formats.
- Official report templates remain stable during initial development.
- Network connectivity is available while generating reports.
- Users have permission to access the uploaded operational data.

### External Dependencies

The application depends on:

- PostgreSQL for metadata persistence.
- Filesystem storage for uploaded files and generated reports.
- FastAPI for backend API services.
- React and TypeScript for frontend development.
- Python report-generation libraries for Word, Excel, and PDF outputs.

### Future Dependencies

Future releases may introduce additional integrations, including:

- Cloud object storage
- Authentication providers
- AI-assisted validation services
- Notification services
- Business intelligence and analytics platforms

These future integrations should not require major architectural changes due to the modular design adopted by DRMA-v2.

# 4. System Features (Functional Requirements)

This section defines the functional capabilities that DRMA-v2 shall provide.

Each feature represents a complete business capability that delivers value to the user.

Detailed workflows, validations, business rules, and API interactions for each feature will be documented in later design documents where appropriate.

---

## 4.1 Report Session Management

### Description

The system shall allow authenticated users to create, manage, retrieve, update, and delete report sessions.

A report session represents a single reporting exercise for a specific weighbridge, report date, and traffic direction.

All uploaded datasets, manual inputs, generated previews, SMS summaries, and final reports shall belong to a report session.

### Functional Requirements

FR-SESSION-001

The system shall allow users to create a new report session.

FR-SESSION-002

Each report session shall be assigned a globally unique identifier (UUID).

FR-SESSION-003

The system shall capture the following metadata during session creation:

- Report Date
- Station
- Bound
- Report Type
- Weighbridge Name
- Prepared By
- Approved By (default locked to Faith Njani)

FR-SESSION-004

The system shall maintain the lifecycle status of each report session.

Possible statuses include:

- Draft
- Uploading
- Processing
- Ready
- Completed
- Failed
- Deleted

FR-SESSION-005

The system shall allow users to retrieve an existing report session.

FR-SESSION-006

The system shall allow users to continue editing incomplete report sessions.

FR-SESSION-007

The system shall automatically update the session timestamp whenever data changes.

FR-SESSION-008

The system shall prevent duplicate sessions for the same station, report date, report type, and bound unless explicitly overridden by an administrator.

FR-SESSION-009

The system shall maintain an audit trail for important session events.

Examples include:

- Session created
- Upload completed
- Preview generated
- Final report built
- Session deleted

---

## 4.2 File Upload Management

### Description

The system shall provide a secure mechanism for uploading operational datasets required for report generation.

Each upload shall be validated before processing begins.

### Functional Requirements

FR-UPL-001

The system shall support uploading CSV and XLSX files.

FR-UPL-002

The system shall validate uploaded file formats.

FR-UPL-003

The system shall reject unsupported file types.

FR-UPL-004

The system shall validate required spreadsheet columns before processing.

FR-UPL-005

The system shall reject empty datasets.

FR-UPL-006

The system shall associate every uploaded file with its report session.

FR-UPL-007

The system shall support the following upload categories:

- Daily Hour Statistics
- Wideload
- Vehicle Inspection
- Impounded & Prohibited
- Overloaded
- Mobile Register
- Traffic Census (future)
- Transgressions (future)

FR-UPL-008

The system shall store uploaded files on the filesystem.

FR-UPL-009

The system shall record upload metadata in PostgreSQL.

FR-UPL-010

The system shall expose upload processing status to the frontend.

---

## 4.3 Data Processing

### Description

The system shall transform uploaded operational datasets into validated structured data suitable for report generation.

### Functional Requirements

FR-DAT-001

The system shall automatically process uploaded datasets.

FR-DAT-002

The system shall clean inconsistent data before report generation.

FR-DAT-003

The system shall normalize spreadsheet values.

FR-DAT-004

The system shall validate business rules before calculations.

FR-DAT-005

The system shall calculate report statistics automatically.

FR-DAT-006

The system shall generate reusable processed datasets for report builders.

FR-DAT-007

The system shall preserve processing errors for user review.

---

## 4.4 Preview Generation

### Description

The system shall generate previews of report sections before the final report is produced.

### Functional Requirements

FR-PRV-001

The system shall generate preview documents for completed sections.

FR-PRV-002

The system shall support PNG previews.

FR-PRV-003

The system shall support PDF previews.

FR-PRV-004

The system shall support DOCX previews.

FR-PRV-005

The system shall cache generated previews.

FR-PRV-006

The frontend shall retrieve preview URLs through the Report Session API.

FR-PRV-007

The system shall regenerate previews whenever source data changes.

---

## 4.5 Final Report Generation

### Description

The system shall generate standardized operational reports by combining all completed report sections into a single document. The generated report shall conform to the official Danka Services reporting template.

### Functional Requirements

FR-RPT-001

The system shall validate that all mandatory uploads and required manual inputs are available before report generation.

FR-RPT-002

The system shall prevent report generation when required data is missing.

FR-RPT-003

The system shall assemble report sections in the predefined order.

FR-RPT-004

The system shall generate reports in Microsoft Word (.docx) format.

FR-RPT-005

The generated report shall use A4 Landscape page orientation.

FR-RPT-006

The system shall apply standardized document formatting across all sections.

FR-RPT-007

The system shall automatically update page numbering fields before finalizing the report.

FR-RPT-008

The system shall store generated report metadata in PostgreSQL.

FR-RPT-009

The generated document shall remain stored on the filesystem.

FR-RPT-010

The frontend shall provide users with a download link once report generation is complete.

---

## 4.6 SMS Summary Generation

### Description

The system shall generate SMS-ready summaries derived from report data for operational communication.

### Functional Requirements

FR-SMS-001

The system shall generate an SMS summary for every completed report session.

FR-SMS-002

The SMS summary shall follow the approved Danka Services template.

FR-SMS-003

The system shall calculate all SMS statistics automatically.

FR-SMS-004

The formula for prohibited vehicles shall be:

P = Z + R

Where:

- Z = Charged & Prohibited Trucks
- R = Court Cases Released

FR-SMS-005

The system shall allow retrieval of SMS summaries by report date.

FR-SMS-006

The system shall return SMS summaries through the Report Session API.

FR-SMS-007

The frontend shall allow users to copy SMS summaries.

---

## 4.7 Report History Management

### Description

The system shall maintain historical records of generated reports for retrieval, auditing, and administrative management.

### Functional Requirements

FR-HIS-001

The system shall maintain a searchable history of completed report sessions.

FR-HIS-002

The system shall allow filtering by:

- Report Date
- Station
- Report Type
- Status

FR-HIS-003

The system shall support paginated history retrieval.

FR-HIS-004

The system shall display upload completion status.

FR-HIS-005

The system shall indicate download availability.

FR-HIS-006

The system shall allow administrators to permanently delete report sessions.

FR-HIS-007

Deleting a report session shall remove:

- Metadata
- Uploaded files
- Generated previews
- Final reports
- Cached processed datasets

FR-HIS-008

Deleted reports shall no longer appear in report history.

---

## 4.8 Dashboard & Analytics

### Description

The system shall provide operational dashboards summarizing reporting activities and weighbridge statistics.

### Functional Requirements

FR-DSH-001

The system shall calculate dashboard statistics from completed report sessions.

FR-DSH-002

Dashboard metrics shall include:

- Total Reports
- Total Vehicles Weighed
- Total Overloaded Vehicles
- Total Wideload Vehicles
- Total Special Releases

FR-DSH-003

Dashboard statistics shall support filtering by date range.

FR-DSH-004

Dashboard statistics shall support filtering by station.

FR-DSH-005

Dashboard data shall be retrieved through dedicated API endpoints.

---

## 4.9 Administration

### Description

The system shall provide administrative functionality for managing report sessions and protected operations.

### Functional Requirements

FR-ADM-001

Administrative operations shall require administrator authentication.

FR-ADM-002

Administrative endpoints shall require the configured administrator password.

FR-ADM-003

Only administrators shall delete report sessions.

FR-ADM-004

Administrative actions shall be logged.

FR-ADM-005

The system shall support future migration to role-based access control (RBAC).

---

## 4.10 Authentication and Authorization

### Description

The system shall restrict access to protected resources based on user permissions.

### Functional Requirements

FR-AUTH-001

Users shall authenticate before accessing protected features.

FR-AUTH-002

The system shall distinguish between normal users and administrators.

FR-AUTH-003

Protected API endpoints shall reject unauthorized requests.

FR-AUTH-004

Future versions shall support organizational user accounts.

FR-AUTH-005

Authentication shall be designed to support JWT-based authentication in future releases.

---

## 4.11 Notifications and Error Handling

### Description

The system shall provide informative feedback whenever processing succeeds or fails.

### Functional Requirements

FR-ERR-001

The system shall return standardized API error responses.

FR-ERR-002

Validation errors shall identify the affected upload section.

FR-ERR-003

The system shall clearly indicate missing spreadsheet columns.

FR-ERR-004

The system shall report invalid file formats.

FR-ERR-005

The system shall report invalid dates.

FR-ERR-006

The system shall report empty uploads.

FR-ERR-007

The frontend shall display section-specific validation messages.

FR-ERR-008

Unexpected system errors shall be logged without exposing sensitive implementation details to end users.

FR-ERR-009

Successful operations shall return descriptive confirmation messages.

FR-ERR-010

Long-running operations shall expose processing status through the Report Session API.

# 5. Non-Functional Requirements

This section defines the quality attributes that DRMA-v2 shall satisfy. Unlike functional requirements, these requirements describe how the system should perform rather than the specific business functions it provides.

---

## 5.1 Performance

### NFR-PERF-001

The system shall create a new report session within **2 seconds** under normal operating conditions.

### NFR-PERF-002

The system shall validate uploaded files before processing begins.

### NFR-PERF-003

The system shall begin processing uploaded datasets immediately after successful validation.

### NFR-PERF-004

Generation of individual report section previews should complete within **10 seconds** for typical operational datasets.

### NFR-PERF-005

Generation of the complete report should normally complete within **30 seconds**.

### NFR-PERF-006

The frontend shall display upload and processing progress without requiring manual page refreshes.

---

## 5.2 Reliability

### NFR-REL-001

The system shall preserve uploaded files and generated reports unless explicitly deleted by an administrator.

### NFR-REL-002

Unexpected application failures shall not corrupt previously generated reports.

### NFR-REL-003

The system shall maintain report session consistency throughout the report generation lifecycle.

### NFR-REL-004

Processing failures shall affect only the current report session and shall not impact other active sessions.

### NFR-REL-005

Generated reports shall remain downloadable after successful generation.

---

## 5.3 Availability

### NFR-AVL-001

The application shall be designed for continuous daily operational use.

### NFR-AVL-002

Backend services shall expose health-check endpoints for deployment monitoring.

### NFR-AVL-003

The application shall recover gracefully after service restarts.

---

## 5.4 Security

### NFR-SEC-001

Administrative operations shall require authentication.

### NFR-SEC-002

Administrative credentials shall never be stored in source code.

### NFR-SEC-003

Uploaded files shall be validated before processing.

### NFR-SEC-004

The application shall reject unsupported file formats.

### NFR-SEC-005

Sensitive configuration values shall be stored as environment variables.

### NFR-SEC-006

The application shall avoid exposing internal server paths or implementation details through API responses.

---

## 5.5 Maintainability

### NFR-MNT-001

The application shall follow a modular architecture.

### NFR-MNT-002

Frontend and backend components shall remain independently maintainable.

### NFR-MNT-003

Business logic shall be separated from presentation logic.

### NFR-MNT-004

The system shall use documented APIs between frontend and backend components.

### NFR-MNT-005

All major modules shall be accompanied by developer documentation.

---

## 5.6 Scalability

### NFR-SCL-001

The application architecture shall support additional report types without requiring major redesign.

### NFR-SCL-002

The system shall support future integration with cloud object storage for uploaded files.

### NFR-SCL-003

The database schema shall support future multi-user environments.

### NFR-SCL-004

The application shall support future background job processing for long-running tasks.

---

## 5.7 Usability

### NFR-USA-001

The application shall provide a consistent user interface across all report workflows.

### NFR-USA-002

Users shall receive clear validation messages when errors occur.

### NFR-USA-003

Users shall be able to identify upload status for every required dataset.

### NFR-USA-004

The application shall provide visual progress indicators during report generation.

### NFR-USA-005

The application shall minimize manual data entry wherever automated extraction is possible.

---

## 5.8 Portability

### NFR-PRT-001

The backend shall support deployment using Docker.

### NFR-PRT-002

The application shall support deployment to cloud hosting platforms.

### NFR-PRT-003

Configuration shall be environment-based.

### NFR-PRT-004

The application shall support PostgreSQL as its primary production database.

### NFR-PRT-005

Filesystem storage locations shall be configurable through environment variables.

# 6. External Interface Requirements

This section defines the interfaces through which DRMA-v2 communicates with users, external systems, storage services, and future integrations.

---

## 6.1 User Interface

### Description

The primary user interface shall be a responsive web application accessible through modern web browsers.

### Requirements

UI-001

The application shall provide a dashboard displaying recent report sessions and operational statistics.

UI-002

The application shall provide guided workflows for creating report sessions.

UI-003

The application shall provide dedicated upload interfaces for each required dataset.

UI-004

The application shall display upload progress and processing status for each dataset.

UI-005

The application shall provide preview panels for generated report sections.

UI-006

The application shall provide report history and search capabilities.

UI-007

Administrative functionality shall be accessible only to authorized users.

UI-008

The user interface shall support desktop and tablet screen sizes.

---

## 6.2 Backend API Interface

### Description

The frontend shall communicate with the backend through RESTful HTTP APIs.

### Requirements

API-001

The backend shall expose REST endpoints for report session management.

API-002

The backend shall expose endpoints for file uploads.

API-003

The backend shall expose endpoints for preview generation.

API-004

The backend shall expose endpoints for final report generation.

API-005

The backend shall expose endpoints for SMS summary retrieval.

API-006

The backend shall expose endpoints for report history.

API-007

The backend shall return JSON responses using consistent response structures.

API-008

The backend shall return standardized error responses.

API-009

Future versions shall support API versioning.

---

## 6.3 Database Interface

### Description

The backend shall persist application metadata using PostgreSQL.

### Requirements

DB-001

The backend shall access PostgreSQL through an ORM layer.

DB-002

Report metadata shall be stored in PostgreSQL.

DB-003

Upload metadata shall be stored in PostgreSQL.

DB-004

Manual input metadata shall be stored in PostgreSQL.

DB-005

Preview metadata shall be stored in PostgreSQL.

DB-006

Generated report metadata shall be stored in PostgreSQL.

DB-007

Uploaded file contents shall not be stored in PostgreSQL.

---

## 6.4 File Storage Interface

### Description

The application shall store uploaded files and generated reports on the filesystem.

### Requirements

FS-001

Uploaded files shall be stored in configurable storage directories.

FS-002

Generated previews shall be stored separately from uploaded datasets.

FS-003

Generated reports shall be stored separately from previews.

FS-004

Storage locations shall be configurable through environment variables.

FS-005

The backend shall verify file availability before exposing download links.

---

## 6.5 External Service Interfaces

### Description

The system shall support integration with external services where required.

### Requirements

EXT-001

Future versions shall support AI-assisted report validation.

EXT-002

Future versions shall support AI-generated operational insights.

EXT-003

Future versions shall support cloud object storage.

EXT-004

Future versions shall support email notification services.

EXT-005

Future versions shall support SMS gateway integration.

EXT-006

Future versions shall support external authentication providers.

# 7. System Constraints

This section defines the technical, business, and operational constraints under which DRMA-v2 shall be developed and deployed.

---

## 7.1 Technical Constraints

TC-001

The application shall be developed as a web-based system.

TC-002

The frontend and backend shall be developed as independent applications communicating through REST APIs.

TC-003

The backend shall persist application metadata in PostgreSQL.

TC-004

Uploaded datasets, generated previews, and final reports shall remain stored on the filesystem.

TC-005

All application configuration shall be supplied through environment variables.

TC-006

The application shall support containerized deployment using Docker.

TC-007

The system architecture shall support future cloud deployment without requiring significant redesign.

---

## 7.2 Business Constraints

BC-001

Generated reports shall conform to the official Danka Services reporting templates.

BC-002

The approved signatory shall default to "Faith Njani" and remain non-editable unless future business policies require otherwise.

BC-003

Business calculations shall conform to the approved reporting formulas.

BC-004

The application shall preserve historical report records for auditing purposes unless explicitly deleted by an administrator.

BC-005

The report generation workflow shall follow the approved operational process used by Danka Services.

---

## 7.3 Operational Constraints

OC-001

The application shall support daily report generation activities.

OC-002

The application shall tolerate incomplete report sessions by allowing users to save progress and resume later.

OC-003

Only administrators shall perform destructive operations such as permanent report deletion.

OC-004

The application shall maintain compatibility with Microsoft Word (.docx) report outputs.

OC-005

The application shall be capable of operating in environments with intermittent internet connectivity, provided the backend remains accessible.

# 8. Assumptions and Dependencies

This section documents assumptions made during requirements analysis and identifies external dependencies required for successful operation.

---

## 8.1 Assumptions

ASM-001

Users possess the necessary operational knowledge to prepare the required reporting datasets.

ASM-002

Uploaded datasets follow the approved Danka Services spreadsheet formats.

ASM-003

Authorized users have access to the required report templates.

ASM-004

The hosting environment provides sufficient storage for uploaded datasets and generated reports.

ASM-005

Reliable backups of PostgreSQL and filesystem storage will be managed by deployment administrators.

ASM-006

Users have access to modern web browsers supporting current web standards.

---

## 8.2 Dependencies

DEP-001

PostgreSQL is required for persistent metadata storage.

DEP-002

Filesystem storage is required for uploaded files and generated reports.

DEP-003

Microsoft Word document generation libraries are required for report creation.

DEP-004

Spreadsheet processing libraries are required for CSV and XLSX parsing.

DEP-005

Future AI-powered features depend on the availability of approved AI service providers.

DEP-006

Cloud deployment depends on the availability of supported hosting infrastructure.

# 9. Acceptance Criteria

DRMA-v2 shall be considered functionally complete when the following criteria have been satisfied.

---

## AC-001 Report Session Management

Users can create, retrieve, update, and complete report sessions successfully.

---

## AC-002 File Uploads

All required datasets can be uploaded, validated, processed, and associated with a report session.

---

## AC-003 Preview Generation

The system successfully generates section previews for processed datasets.

---

## AC-004 Final Report Generation

The application generates complete reports matching the approved Danka Services template.

---

## AC-005 SMS Summary

SMS summaries are generated using approved business rules and formulas.

---

## AC-006 Report History

Completed reports appear in the report history with accurate metadata and download availability.

---

## AC-007 Administration

Administrative users can securely perform report deletion and other protected operations.

---

## AC-008 Dashboard

Dashboard metrics accurately reflect generated report data.

---

## AC-009 Security

Protected operations require appropriate authentication and authorization.

---

## AC-010 System Stability

The application completes the end-to-end report generation workflow without data loss or unexpected failures.


# Appendix A — Glossary

| Term | Definition |
|------|------------|
| Report Session | A single report generation workflow from creation to completion. |

| Report Officer | User responsible for preparing and generating reports. |

| Administrator | User responsible for system administration, monitoring, user management, and audit review. |

| Upload Module | Component responsible for validating and storing uploaded datasets. |

| Report Builder | Service responsible for assembling the final report document. |

| Preview | Intermediate document generated before the final report. |

| SMS Summary | Text summary generated from report statistics for operational communication. |

| Audit Log | Immutable record of important system and user actions. |

| Validation Engine | Service responsible for checking uploaded datasets before processing. |

| Analytics Dashboard | Interface displaying report statistics and operational insights. |

| Metadata | Descriptive information stored in PostgreSQL about reports, uploads, previews, and outputs. |

| Persistent Storage | Long-term storage used for uploaded files and generated reports. |

# Appendix B — Acronyms

| Acronym | Meaning |
|----------|---------|
| API | Application Programming Interface |
| CSV | Comma-Separated Values |
| DRMA | Danka Report Management Application |
| GUI | Graphical User Interface |
| HTTP | Hypertext Transfer Protocol |
| JSON | JavaScript Object Notation |
| JWT | JSON Web Token |
| PDF | Portable Document Format |
| REST | Representational State Transfer |
| RBAC | Role-Based Access Control |
| SDD | Software Design Document |
| SDLC | Software Development Life Cycle |
| SRS | Software Requirements Specification |
| UI | User Interface |
| UX | User Experience |
| UUID | Universally Unique Identifier |
| XLSX | Microsoft Excel Open XML Workbook |

# Appendix C — Requirement Identifier Convention

| Prefix | Description |
|---------|-------------|
| FR | Functional Requirement |
| NFR | Non-Functional Requirement |
| UPL | Upload Requirement |
| REP | Report Generation Requirement |
| API | API Requirement |
| DB | Database Requirement |
| SEC | Security Requirement |
| LOG | Logging Requirement |
| SMS | SMS Summary Requirement |
| ADM | Administration Requirement |
| AI | Artificial Intelligence Requirement |

# Appendix D — Project Documentation Map

| Document | Purpose |
|----------|---------|
| Product Overview | Business vision and objectives |
| Software Requirements Specification | System requirements |
| Software Design Document | Technical design |
| Database Design | Database architecture |
| API Specification | REST API definitions |
| Backend Architecture | Backend component design |
| Frontend Architecture | Frontend component design |
| Security Design | Authentication, authorization, audit logging |
| AI Integration Design | AI-assisted features |
| Testing Strategy | Testing approach |
| Deployment Guide | Production deployment |
| Development Guidelines | Coding standards and workflow |
| User Manual | End-user documentation |
| Administrator Guide | Administrative procedures |

# Appendix E — SDLC Mapping

| SDLC Phase | Deliverables |
|------------|--------------|
| Planning | Product Overview |
| Requirements | SRS |
| Design | SDD, Database Design, API Design |
| Implementation | Source Code |
| Testing | Test Reports |
| Deployment | Deployment Guide |
| Maintenance | Changelog, Project Journal |

# Appendix F — Change Control Process

Any proposed changes to the requirements shall follow the process below:

1. Document the proposed change.
2. Assess its impact on architecture and implementation.
3. Review with the development team.
4. Update the SRS if approved.
5. Reflect the change in the SDD and related documentation.
6. Record the change in the project changelog.

No implementation shall proceed based on undocumented requirements.