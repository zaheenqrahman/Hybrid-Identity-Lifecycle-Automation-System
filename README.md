# Network-Aware Zero-Trust Automated IAM Platform/System

A single application platform hosted on Azure with testing and deployment pipelines used to streamline vendor specific uses like ticketing, IAM with authentication features reducing user fatigue and increasing ease of access built with a front end platform that is easy to navigate. The back end will be secured using network security protocols = API's with tools.

**Status:** Planning stage.

## The problem

Many organizations split identity and support across separate vendors: one product for sign-in and access (for example an identity provider such as Okta), another completely separate vendor platform that has to make users log in again to request a ticket that can increase fatigue especially when there is little to no help when something breaks. An agent is also available directly to talk to on the original sign in and access platform page to get to the app's resources successfully for the user's ease of access.

An application built on Microsoft Entra ID, using its sign-in, MFA, groups, and Microsoft Graph API to deliver an Okta-like experience (central sign-in, role-based access, lifecycle changes) together with ticketing. It runs against a test tenant with fake users. It is a project to introduce a solution to fix a problem that should be more streamlined for users to access.

## Planned features

**Identity and access**

- Single sign-on with Entra ID using OpenID Connect (MSAL)
- MFA through Entra ID for test users
- Role-based access in the app: Requester, Helpdesk, Admin
- Group-based access to the application
- Audit log of sign-ins and actions

**Ticketing (built into the same app)**

- Ticket types: new hire, access request, role change, offboarding, password reset
- SLA, ticket's notes, documentation bases
- Approval step before any identity change is made

**User lifecycle (joiner, mover, leaver)**

- Approved tickets trigger changes through Microsoft Graph: create a user and add groups, change group membership, or disable an account
- Results are written back to the ticket

**Safeguards**

- A requester cannot approve their own ticket
- Automation is limited to a designated test group of users
- Least-privilege Graph permissions
- Dry-run mode that logs what would change
- Secrets are kept out of the repository

## Planned architecture

Diagram for how the front end may interact with the back end (to be added).

## Planned tech stack (SDK)

| Layer | Choice |
|---|---|
| App | Python and FastAPI (maybe Okta Node/Python SDKs within it) |
| Authentication | Entra ID with MSAL (OpenID Connect) |
| Identity actions | Microsoft Graph API |
| Storage | SQLite for tickets and the audit log |
| Hosting | Runs locally first; Azure hosting is optional later |
| Front End | React and TypeScript for platform user interface from an Azure-hosted app |
| Agent/Agentic Flow | Anthropic API, Build a MCP, LangGraph, Pydantic |

## Status

The plan for the first steps before I begin to create the back end:

| Step | Status |
|---|---|
| Test users and groups in the Entra ID tenant | Planned |
| App registration and single sign-on | Planned |
| App roles (Requester, Helpdesk, Admin) enforced in the app | Planned |
| Ticket creation, listing, and status changes | Planned |
| Approval workflow, with self-approval blocked | Planned |
| Graph actions triggered by approved tickets | Planned |
| Audit log | Planned |
| Documentation, diagram, and demo | Planned |

## The scope

- Features that need paid Entra ID licenses (such as Conditional Access) are out of scope unless a trial is available.
- Sign-in and audit logs on the free tier are kept for a short time, so screenshots and exports are captured as the work progresses.

## Build plan

The project is built in order so that something works end to end early, then gets stronger. The week-by-week plan is in [docs/roadmap.md](docs/roadmap.md).

1. **Core (weeks 1-3):** test tenant, sign-in, roles, tickets, approval workflow, Graph actions with dry-run
2. **Hardening (weeks 4-5):** safeguard tests, CI, threat model, documented Graph permissions, red team / blue team round, scenario demos
3. **Network-aware access (week 5):** a simple risk score from sign-in details (IP, location, time) that requires a fresh MFA prompt for Admin actions in unusual situations
4. **Stretch (after the core is done):** on-prem Active Directory lab synced to Entra ID (the "hybrid" part), agent triage with a guardrailed proposal flow, React and TypeScript front end, Azure deployment

## Engineering practices

- All changes go through pull requests with a review from the other author
- Automated tests for every safeguard listed above (self-approval blocked, dry-run changes nothing, test-group limit enforced)
- GitHub Actions runs the tests on every pull request
- A short threat model and a list of the exact Graph permissions requested, with the reason for each
- Secret scanning and Dependabot enabled

## Authors

- Zaheen Rahman · [GitHub](https://github.com/zaheenqrahman)
- [Partner name and GitHub link]
