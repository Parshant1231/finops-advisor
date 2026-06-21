API BLUEPRINT
These are the endpoints we will build. Phase 2 builds Auth. Phase 3 builds AWS. Phase 4 builds Analysis.
Auth
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout
GET    /api/auth/me
Organizations
GET    /api/organizations/me
PATCH  /api/organizations/me
AWS Accounts
POST   /api/aws-accounts              ← Connect an AWS account
GET    /api/aws-accounts              ← List connected accounts
DELETE /api/aws-accounts/:id          ← Disconnect
POST   /api/aws-accounts/:id/scan     ← Trigger a resource scan
GET    /api/aws-accounts/:id/scan-status
Resources
GET    /api/resources                 ← All resources (filtered by org)
GET    /api/resources?type=ec2        ← Filter by type
GET    /api/resources?idle=true       ← Filter idle only
GET    /api/resources/:id
Recommendations
GET    /api/recommendations
GET    /api/recommendations?priority=critical
PATCH  /api/recommendations/:id/dismiss
PATCH  /api/recommendations/:id/resolve
Analytics
GET    /api/analytics/summary         ← Total waste, savings potential
GET    /api/analytics/by-service      ← Breakdown by EC2/EBS/RDS
GET    /api/analytics/trends          ← Cost over time