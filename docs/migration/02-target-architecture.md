# Target Architecture – GENAPP Modernised Platform

This document describes the target Java microservices architecture that will implement the CM-board user stories and replace the COBOL GENAPP behaviour.

```mermaid
graph LR
  subgraph Users
    Agent[Contact Centre Agent]
    Servicing[Policy Servicing Agent]
    Analyst[Operations Analyst]
  end

  subgraph External
    IdP[Identity Provider / SSO]
    Legacy[Existing GENAPP COBOL (mainframe)]
    OtherSys[External Integrating Systems]
  end

  Agent -->|HTTPS| APIGW
  Servicing -->|HTTPS| APIGW
  Analyst -->|HTTPS| APIGW
  OtherSys -->|HTTPS| APIGW

  APIGW[API Gateway / Edge Service]

  subgraph Services
    CUST[Customer Service]
    POL[Policy Service]
    BILL[Billing Service]
    REP[Reporting Service]
  end

  APIGW --> CUST
  APIGW --> POL
  APIGW --> REP

  POL --> BILL

  CUST --> CDB[(Customer DB)]
  POL --> PDB[(Policy DB)]
  BILL --> BDB[(Billing DB)]
  REP --> CDB
  REP --> PDB

  APIGW -->|AuthN/AuthZ| IdP
  POL -->|Coexistence / Regression Calls| Legacy
```

The services are:

- Customer Service – customer master data and search.
- Policy Service – full policy lifecycle and inquiry.
- Billing Service – billing schedules and payments (later phase).
- Reporting Service – statistics and reports.
- API Gateway – unified external REST API and security.
