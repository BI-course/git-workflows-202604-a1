# Data Governance and Access to PII

## What is Data Governance?
Data governance is the set of policies, roles, processes, and
standards that ensure data is accurate, consistent, secure,
and used responsibly across an organisation.

## Core Components
- **Data Stewardship** — assigning ownership of datasets to
  accountable individuals or teams
- **Data Cataloguing** — maintaining a searchable inventory
  of all datasets, their schemas, and their sensitivity level
- **Access Control** — enforcing who can read, write, or delete
  specific data based on their role (RBAC)
- **Audit Logging** — recording all access and modification
  events for regulatory review

## Personally Identifiable Information (PII)
PII is any data that can identify a specific individual:
- Direct identifiers: full name, national ID, passport number
- Indirect identifiers: date of birth + postcode + gender

## Legal Frameworks Governing PII
| Framework | Region | Key Requirement |
|---|---|---|
| GDPR | European Union | Consent, right to erasure, data minimisation |
| HIPAA | United States | Protected health information controls |
| POPIA | South Africa | Lawful processing, data subject rights |

## Best Practices for PII Access
1. Apply the **principle of least privilege** — grant only the
   access a role strictly requires
2. **Anonymise or pseudonymise** PII in non-production environments
3. Set **data retention policies** — delete PII when no longer needed
4. Conduct regular **access reviews** to revoke stale permissions
EOF
