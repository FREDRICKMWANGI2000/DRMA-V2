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