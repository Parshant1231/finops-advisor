DECISIONS
Before I give you tasks, I want you to understand the three most important architectural decisions for this project. Everything else flows from these.

Decision 1 — Multi-Tenancy Model
FinOps Advisor serves multiple organizations. Each organization connects their AWS account and sees only their own data.
The question is: how do you isolate tenant data in the database?
There are three industry approaches:
ApproachDescriptionProsConsSeparate databases per tenantEach org gets its own DBMaximum isolationExpensive, complex opsSeparate schemas per tenantOne DB, one schema per orgGood isolationComplex migrationsShared tables with tenant IDOne DB, one schema, org_id on every tableSimple, cheap, scalableMust be disciplined with queries
My recommendation for FinOps Advisor: Shared tables with org_id.
Here's why: You're building a portfolio SaaS. You don't have 500 enterprise clients. The operational complexity of separate databases would kill you. Stripe, Linear, Notion — they all started with shared tables. The discipline of always filtering by org_id is enforced by your service layer. This is the right call at this stage.

Decision 2 — AWS Credential Strategy
This is the most security-critical decision in the entire project.
Your users need to give FinOps Advisor permission to read their AWS resources. There are two approaches:
ApproachDescriptionSecurity LevelStore Access KeysUser pastes AWS_ACCESS_KEY_ID + SECRET into your UIPoor — keys sit in your DBCross-Account IAM RoleUser creates a role in their account, your app assumes itExcellent — no keys stored
My recommendation: Cross-Account IAM Role with STS AssumeRole.
This is how AWS-native tools like Datadog, CloudHealth, and Spot.io work. Your app never stores credentials. Instead, the user creates an IAM Role in their AWS account with read-only permissions, and your backend assumes that role on-demand using STS. This is the professional, production-grade approach. It also makes a great interview talking point.
We implement this in Phase 3, but the database schema needs to be designed for it now.

Decision 3 — Monorepo vs Separate Repos
You have a NestJS backend and a Next.js frontend.
ApproachProsConsMonorepo (single repo)Shared types, one place, easier coordinationSlightly more complex setupSeparate reposFull independenceShared types become painful, two repos to manage
My recommendation: Monorepo using a simple /apps structure.
No need for Nx or Turborepo at this stage — that's overengineering. A simple folder structure is enough and keeps things manageable for a solo developer.
