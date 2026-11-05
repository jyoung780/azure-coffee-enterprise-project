# Azure Coffee Enterprise Project - by Jeffrey Young

> A public, hands-on Microsoft Azure cloud engineering lab simulating the technology needs of a multi-location coffee business.

**Repository:** `azure-coffee-enterprise-project`  
**Status:** Planning and design — infrastructure has not yet been deployed.

## Project overview

This project models a fictional coffee company operating **five coffee shops, one corporate office, one manufacturing facility, and one distribution center**. The goal is to design, deploy, automate, secure, monitor, and troubleshoot a realistic Azure environment while documenting decisions and lessons learned in public.

This is a **learning and portfolio project**, not a production business system. All employees, customers, transactions, and operational records will be synthetic.

## Business locations and requirements

| Location | Example technology needs |
| --- | --- |
| Five retail coffee shops | Point-of-sale (POS) simulation, customer loyalty, employee access, inventory, guest Wi-Fi segmentation |
| Corporate office | HR, finance, identity management, employee onboarding/offboarding, reporting |
| Manufacturing facility | Production orders, raw-material inventory, quality checks, equipment data simulation |
| Distribution center | Warehouse stock, fulfillment, shipping, transfers, delivery tracking |

### Core business workflows

1. **Employee lifecycle:** A manager submits a hire or departure request; approved automation provisions or revokes the appropriate access and records the outcome.
2. **Customer loyalty:** A customer earns and redeems points across any of the five stores.
3. **Inventory replenishment:** Retail sales reduce store inventory; reorder thresholds trigger requests to the distribution center.
4. **Manufacturing and distribution:** The factory produces goods, the distribution center receives stock, and stores request replenishment.
5. **Operations and support:** IT detects incidents, investigates logs and metrics, restores service, and documents root causes.

## Proposed Azure architecture

The architecture will evolve as requirements and costs are validated. Planned components include:

| Area | Candidate services / tools | Purpose |
| --- | --- | --- |
| Identity and access | Microsoft Entra ID, Azure RBAC, Microsoft Graph | Staff identities, group-based access, employee lifecycle |
| Networking | Azure Virtual Network, subnets, NSGs, routing, DNS | Segmentation and private cloud networking |
| Connectivity | HTTPS APIs; VPN Gateway only if justified | Secure access from simulated sites |
| Applications | Azure App Service or Azure Functions | Loyalty, inventory, and HR workflow prototypes |
| Structured data | Azure SQL Database | Customers, loyalty balances, products, orders |
| Object and file storage | Azure Blob Storage, Azure Files | Documents, images, exports, shared files |
| Secrets | Azure Key Vault | Application secrets and credentials |
| Observability | Azure Monitor, Log Analytics, Application Insights | Logs, metrics, alerts, troubleshooting |
| Automation and IaC | PowerShell, Terraform or Bicep, GitHub Actions | Repeatable deployments and operational workflows |

**Design principle:** Not every location needs its own Azure VNet or permanent VPN. Retail sites may access centrally hosted applications securely over HTTPS. Connectivity choices will be documented based on requirements, security, and cost.

## Project roadmap

- [ ] **Phase 1 — Requirements and architecture:** Define roles, data flows, site topology, threat model, budget, and architecture diagrams.
- [ ] **Phase 2 — Azure foundation:** Create resource groups, networking, naming conventions, tagging, access controls, and cost alerts.
- [ ] **Phase 3 — Identity and employee automation:** Model employees, groups, RBAC, joiner/mover/leaver workflows, approvals, and audit logs.
- [ ] **Phase 4 — Customer loyalty and storage:** Build a synthetic customer membership system with SQL and object storage.
- [ ] **Phase 5 — Manufacturing and distribution:** Simulate stock movements, production, replenishment, and fulfillment.
- [ ] **Phase 6 — Monitoring, security, and incident response:** Configure observability, backups where appropriate, alerts, and failure scenarios.
- [ ] **Phase 7 — Infrastructure as code and CI/CD:** Make deployments reproducible and document validation, teardown, and recovery.

## Hands-on troubleshooting scenarios

Examples of intentionally introduced lab incidents:

- A store cannot reach the loyalty application.
- A new employee receives the wrong permissions.
- An offboarding workflow fails halfway through.
- A storage account rejects an authorized application's request.
- Inventory updates are delayed or duplicated.
- A deployment fails due to configuration drift or insufficient permissions.

Each incident will include **symptoms, impact, diagnostic steps, root cause, fix, and prevention**.

## Planned repository structure

```text
azure-coffee-enterprise-project/
├── README.md
├── docs/                  # Requirements, diagrams, decisions, runbooks
├── networking/            # Network designs and configurations
├── identity/              # Identity and access designs
├── automation/            # Onboarding, offboarding, operational scripts
├── applications/          # Loyalty and inventory prototypes
├── storage/               # Database and storage designs
├── infrastructure/        # Terraform / Bicep
├── troubleshooting/       # Incident reports and resolutions
└── .github/workflows/     # CI validation and deployment workflows
```

Folders will be added as work begins.

## Security and responsible lab practices

- Use **fictional employee and customer data only**.
- Never commit passwords, tokens, connection strings, private keys, or real customer information.
- Do not commit Terraform state files or environment-specific secrets.
- Use least-privilege access, managed identities where supported, and secure secret storage.
- Use `.gitignore`, secret scanning, and reviews before publishing.
- Configure Azure budgets and cost alerts before provisioning paid resources; delete idle lab resources.
- Avoid connecting real payment systems or processing actual cardholder data.

## Learning outcomes

This project is intended to demonstrate practical skills in **Azure administration (AZ-104 topics), cloud networking, identity, storage, automation, monitoring, incident response, PowerShell, infrastructure as code, and technical documentation**.

## Current progress

**Phase 1: Planning.** The company model and initial project scope have been defined. Architecture decisions, diagrams, code, and deployment evidence will be added incrementally. No deployed infrastructure is claimed at this stage.

## License

This project is licensed under the MIT License.
See the [LICENSE](LICENSE) file for details.
