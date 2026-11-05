# Business Requirements Document (BRD)

**Project:** Azure Coffee Enterprise Project  
**Document:** `docs/01-business-requirements.md`  
**Version:** 1.0 (Draft)  
**Status:** Proposed — requirements to validate  
**Last updated:** 2026-11-04

## 1. Executive Summary

Azure Coffee Enterprise is a **fictional** coffee business used to design, deploy, operate, and troubleshoot a realistic Microsoft Azure environment. The organization operates **five coffee shops, one corporate office, one coffee production factory, and one distribution center**. The project will be developed publicly on GitHub as a hands-on cloud engineering portfolio.

The project prioritizes practical Azure administration, identity and access management, networking, storage, automation, security, monitoring, and infrastructure as code. All customer, employee, payment, and operational records will be **synthetic**.

## 2. Business Goals

1. Centralize identity and security controls across all locations.
2. Enable consistent employee onboarding, role changes, and offboarding.
3. Support customer memberships and loyalty rewards across five stores.
4. Track stock from suppliers through manufacturing and distribution to stores.
5. Provide reliable business applications and operational reporting.
6. Demonstrate incident response, recovery, and cloud cost governance.
7. Publish repeatable deployments, tests, and design decisions on GitHub.

## 3. Organization and Scope

| Site | Quantity | Primary operations |
|---|---:|---|
| Retail coffee shops | 5 | Sales, loyalty redemption, stock consumption, staff scheduling |
| Corporate office | 1 | HR, finance, IT, reporting, management |
| Factory | 1 | Roasting/production, batch tracking, quality control |
| Distribution center | 1 | Receiving, warehousing, picking, shipping, replenishment |
| **Total physical sites** | **8** | Shared enterprise services |

**Planning assumptions (not verified facts):** Approximately 100 employees; one central Azure tenant; cloud-hosted custom demonstration applications; no real payment-card processing; store and factory devices are simulated initially. These assumptions can be revised in later design documents.

### Example user roles

- Retail: cashier, shift supervisor, store manager
- Corporate: HR specialist, finance analyst, IT support, cloud administrator, executive
- Factory: production operator, quality technician, factory manager
- Distribution: warehouse associate, dispatcher, distribution manager
- External: customer/member (application identity separate from workforce identity)

## 4. Functional Requirements

Requirement IDs will be referenced in architecture decisions, implementation issues, and acceptance tests.

| ID | Priority | Requirement | Acceptance criterion |
|---|---|---|---|
| BR-001 | Must | Centralized employee identity | Test users authenticate using Microsoft Entra ID and are assigned role-appropriate access. |
| BR-002 | Must | Automated onboarding | A mock approved HR event triggers account provisioning, group assignment, and an auditable completion/failure record. |
| BR-003 | Must | Automated offboarding | A mock termination event blocks sign-in, revokes sessions where supported, removes access, and produces an audit trail. |
| BR-004 | Must | Least-privilege authorization | Cashiers cannot access HR or financial administration resources; authorized staff can access their assigned systems. |
| BR-005 | Must | Multi-location store data | Transactions and inventory events identify their originating store. |
| BR-006 | Must | Membership and loyalty | A synthetic customer can enroll, earn points, and redeem a reward according to documented rules. |
| BR-007 | Must | Central inventory | Stock levels and store replenishment orders can be recorded and queried. |
| BR-008 | Should | Factory production | Simulated production batches track input materials, output quantities, and status. |
| BR-009 | Should | Distribution workflow | Warehouse staff can receive inventory, fulfill store orders, and record shipments. |
| BR-010 | Must | Application and data protection | Access controls, managed identities where appropriate, secrets management, and encryption are documented and tested. |
| BR-011 | Must | Operational monitoring | A simulated failure creates actionable telemetry or an alert and a documented investigation. |
| BR-012 | Must | Recoverability | A documented recovery exercise demonstrates restoration of a supported test workload or dataset. |
| BR-013 | Must | Repeatable infrastructure | Core resources can be provisioned from version-controlled Terraform or Bicep with reviewable configuration. |
| BR-014 | Should | Business reporting | A sample report summarizes store sales, rewards, or inventory using synthetic data. |
| BR-015 | Must | Cost controls | A budget, alerts, resource tags, and teardown instructions are documented before paid deployment. |

## 5. Nonfunctional Requirements and Proposed Targets

These are **initial lab targets**, not claims of production service levels.

| Area | Proposed target or policy |
|---|---|
| Security | Least privilege, MFA for administrators, no credentials in source control |
| Availability | Document dependencies and single points of failure; test at least one outage scenario |
| Performance | Define and measure a baseline for membership lookup and loyalty updates after the application exists |
| Onboarding | Demonstrate successful automated provisioning within 15 minutes of an approved mock HR event |
| Offboarding | Demonstrate sign-in blocking within 5 minutes of a processed mock termination event; measure actual execution |
| Auditability | Record automation outcome, request ID, timestamp, and responsible workflow without exposing secrets |
| Recovery | Define RPO/RTO per workload before selecting backup and replication services |
| Data privacy | Synthetic data only; no real customer information, payment card data, or employee records |
| Maintainability | Document infrastructure changes, tests, rollback, and incident runbooks |
| Cost | Prefer free/low-cost services for early labs; estimate charges before provisioning and destroy idle resources |

## 6. Preliminary Azure Service Mapping

These are **candidate technologies**, not a commitment to deploy all of them.

| Capability | Candidate Azure / Microsoft services |
|---|---|
| Workforce identity | Microsoft Entra ID, security groups, Conditional Access where licensed |
| Role-based authorization | Azure RBAC for resources; application-level roles for business apps |
| Networking | VNets, subnets, NSGs, route tables, private endpoints, DNS |
| Site connectivity | HTTPS to cloud apps initially; site-to-site VPN later if justified and cost-approved |
| Web/API hosting | Azure App Service or Azure Functions |
| Structured data | Azure SQL Database |
| Object and file storage | Azure Blob Storage and, if needed, Azure Files |
| Messaging and workflows | Azure Functions, queues or Service Bus where appropriate |
| Secrets | Azure Key Vault, managed identities |
| Monitoring | Azure Monitor, Application Insights, Log Analytics |
| Infrastructure as code | Terraform or Bicep, GitHub Actions |
| Backup/recovery | Service-native backup/restore features and Azure Backup where applicable |

**Architecture note:** Each physical site does not automatically require its own Azure VNet. Connectivity will be designed around application needs, segmentation, cost, and threat models. Payment processing will remain out of scope; no real card data will be stored.

## 7. Key Business Workflows

### 7.1 Employee onboarding

1. A mock HR system records an approved hire with location and role.
2. Automation validates required fields and checks for duplicate requests.
3. The workflow provisions or stages an employee identity using approved test methods.
4. Location/role groups and application access are assigned.
5. A completion report and audit record are generated; errors are queued for review.

### 7.2 Employee offboarding

1. A mock HR termination event is approved.
2. Automation blocks sign-in and revokes sessions where supported.
3. Group memberships and application access are removed according to policy.
4. Device return and data retention tasks are logged for manual or later automated handling.
5. The workflow records verification, exceptions, and completion.

### 7.3 Customer loyalty

1. A synthetic customer joins the membership program.
2. A simulated POS sale posts a purchase event.
3. The loyalty service validates the event and updates points exactly once.
4. The customer redeems points for an eligible reward.
5. Audit logs and store-level reporting reflect the activity.

### 7.4 Inventory replenishment

1. A store's inventory falls below a threshold.
2. A replenishment request is created.
3. Distribution checks available stock and allocates a shipment.
4. Factory production can replenish distribution stock if necessary.
5. Receipt at the store updates inventory and order status.

## 8. Security, Privacy, and Compliance Boundaries

- Do not commit passwords, access tokens, connection strings, private keys, Terraform state, or sensitive `.tfvars` files.
- Use GitHub secret scanning and appropriate repository protections where available.
- Use synthetic data and fictional locations/identities in screenshots, logs, and demos.
- Apply separate administrative and ordinary user roles; enforce least privilege.
- Define retention and deletion rules for mock employee and customer records.
- Treat production payment processing, PCI DSS implementation, real POS integrations, and industrial control system connectivity as **out of scope** for the initial lab.
- Never connect this lab to real factory equipment or real employee systems.

## 9. Project Delivery Phases

| Phase | Deliverable | Evidence in GitHub |
|---|---|---|
| 1 — Discovery | BRD, assumptions, use cases, high-level architecture | Markdown docs and diagrams |
| 2 — Foundation | Azure governance, identity, network baseline | IaC, screenshots with redactions, validation tests |
| 3 — Employee lifecycle | Onboarding/offboarding workflows | Scripts, test cases, runbooks |
| 4 — Retail and loyalty | Membership API, sample data, storage | Application code, API tests, data model |
| 5 — Supply chain | Inventory, factory, distribution simulations | Workflow diagrams and integration tests |
| 6 — Operations | Monitoring, failure injection, recovery | Incident reports, alerts, recovery exercises |
| 7 — Delivery automation | CI validation, deployment and teardown | GitHub Actions, IaC, cost controls |

## 10. Initial Risks and Dependencies

| Risk | Mitigation |
|---|---|
| Unexpected Azure charges | Budget alerts, pricing checks, small SKUs, teardown scripts |
| Sensitive information leaked to public GitHub | Synthetic data, `.gitignore`, secret scanning, pre-commit review |
| Overly complex first deployment | Start with a minimal vertical slice; add services incrementally |
| Licensing requirements | Verify Entra, Microsoft 365, and Azure feature licensing before use |
| Unrealistic architecture assumptions | Record architecture decision records (ADRs) and validate each design |
| Automation partial failures | Idempotent workflows, retries, audit logs, exception handling |

## 11. Success Criteria

The project will be considered a successful learning portfolio when it includes:

- A documented multi-site business architecture with clear identity and security boundaries.
- At least one working automated employee lifecycle scenario with tests.
- A working mock loyalty transaction from API request to persisted data.
- A demonstrable inventory movement between factory, distribution, and a store.
- At least three documented troubleshooting exercises with root-cause analysis.
- Reproducible infrastructure configuration and safe cleanup instructions.
- Clear GitHub documentation explaining design decisions, limitations, and measured outcomes.

## 12. Open Decisions

1. Choose a fictional company brand and store identifiers.
2. Confirm whether to use Terraform, Bicep, or both (Terraform is a strong starting choice).
3. Choose the first Azure region and a monthly spending cap.
4. Decide which employee identity features are feasible with available licensing.
5. Define sample membership and inventory data schemas.
6. Define which systems are simulated versus actually deployed.
7. Agree on initial RPO/RTO objectives for the loyalty and inventory workloads.

## 13. Next Document

Create `docs/02-high-level-architecture.md` with a component diagram, identity boundaries, data flows, site connectivity options, and an initial resource/cost estimate.

---

*This is a public educational simulation, not a production-ready implementation or an assertion of compliance with any specific regulatory framework.*
