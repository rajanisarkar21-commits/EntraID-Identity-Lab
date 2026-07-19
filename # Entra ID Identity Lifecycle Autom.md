README.md

An end-to-end UiPath automation that manages the full identity lifecycle of users in Microsoft Entra ID (Azure AD) — provisioning, role assignment, group membership, MFA enforcement, and SSO app access — driven by a simple Excel config file, and fully idempotent (safe to re-run without creating duplicates).

## What it does

| Milestone | Description |
|---|---|
| 1 | Connects to the tenant via OAuth2 client credentials flow (Azure AD Application Scope) |
| 2 | Provisions users from a config-driven data source (create-if-not-exists) |
| 3 | Assigns directory roles (e.g. User Administrator) and manages security group membership |
| 4 | Enforces Multi-Factor Authentication via Microsoft Graph (`/beta/` endpoint) and validates registered MFA methods |
| 5 | Resolves an Enterprise Application's Service Principal and grants SSO access via app role assignment |
| 5.5 | Generates a consolidated audit report (`SSO_Verification_Report.xlsx`) summarizing role, group, MFA, and SSO status per user |

## Tech stack

- **UiPath Studio** (`UiPath.AzureActiveDirectory.Activities` for core AD operations)
- **Microsoft Graph API** (REST calls via HTTP Request activities, for operations not covered by the AD activity package — MFA and app role assignments)
- **OAuth2 client credentials flow** for app-only Graph authentication

## Setup

1. Register an app in **Azure Portal → App registrations** and grant it the required Graph API application permissions (`User.ReadWrite.All`, `Directory.ReadWrite.All`, `Policy.ReadWrite.AuthenticationMethod`, `AppRoleAssignment.ReadWrite.All`).
2. Open `Data/config.xlsx` and fill in your own:
   - `ClientSecret`, `TenantId`, `ClientId` (from your App Registration)
   - `DomainSuffix` (your reference Object ID)
   - Target group name and admin role name
   - Test user details in the `Users` sheet
3. Run `Main.xaml` via UiPath Studio (F5).
4. Check `Data/SSO_Verification_Report.xlsx` for the generated audit report.

> ⚠️ `Data/config.xlsx` is git-ignored. Never commit real Client Secrets or tenant IDs — see `.gitignore`.



## Project structure

```
EntraID_Identity_Lab/
├── Main.xaml                  # Main workflow
├── Data/
│   └── config.xlsx            # Settings + test user data (not committed — see .gitignore)
├── project.json
└── README.md
```
