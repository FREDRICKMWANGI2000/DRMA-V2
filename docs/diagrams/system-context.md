# System Context Diagram

This diagram illustrates the external actors and major systems interacting with DRMA-v2.

```mermaid
flowchart LR

User[Report Officer]
Admin[Administrator]

Browser[Web Browser]

DRMA[DRMA-v2 Web Application]

DB[(PostgreSQL)]

Storage[(Filesystem Storage)]

Templates[Official Word & Excel Templates]

Reports[Generated Reports]

SMS[SMS Summary]

User --> Browser
Admin --> Browser

Browser --> DRMA

DRMA --> DB
DRMA --> Storage
DRMA --> Templates

DRMA --> Reports
DRMA --> SMS
```

## Purpose

This diagram provides a high-level overview of the DRMA-v2 ecosystem, showing the main actors and external resources that interact with the application.