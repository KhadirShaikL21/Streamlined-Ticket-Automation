Streamlining Ticket Assignment for Efficient Support Operations

Automates ticket routing in ServiceNow so that new or updated tickets are assigned to the correct support group (e.g., Platform or Certificates) based on the issue selected—reducing triage time and improving first-response SLAs.

Table of Contents

Project Overview

Architecture & Design

Key Features

Requirements

Repository Structure

Set Up the Project (Step-by-Step)

1) Create Users

2) Create Groups

3) Create Roles

4) Create the Operations Table

5) Configure Choice Values

6) Assign Users & Roles to Groups

7) Secure with Application Access & ACLs

8) Build the Flows (Automation)

9) Test the Automation

How It Works (Logic Summary)

Troubleshooting

Performance & Best Practices

Security & Governance

Extending the Solution

Glossary

License

Project Overview

Goal: Reduce manual triage by auto-assigning tickets to the right group as soon as a record is created or updated.

Scope:

Custom table: Operations related (u_operations_related)

Groups: Platform, Certificates

Roles: platform_role, certification_role

Automation: 2 Flows in Flow Designer that listen for changes on the table and update Assigned to group accordingly.

Value:

Faster routing → faster resolution

Less manual work for dispatchers

Consistent and auditable assignment rules

Architecture & Design

Data Layer: Custom table u_operations_related to capture ticket details.

Access Layer: Application Access + Field/Table ACLs requiring appropriate roles.

Automation Layer: Flow Designer:

Flow A: “Regarding Certificate” → routes to Certificates group.

Flow B: “Regarding Platform” → routes to Platform group (for multiple platform-type issues).

People Layer: Users belong to groups; groups have roles that grant access to the table.

Key Features

✅ Declarative, no-code flows for auto-assignment

✅ Granular security via Application Access & ACLs

✅ Extensible choice-driven routing

✅ Clear audit trail in Flow Execution Details

✅ Production-ready folder/repo structure

Requirements

ServiceNow instance with admin access (to create tables, roles, ACLs).

Permissions to create Flows (Flow Designer).

(Optional) Email/notification rights if you extend with notifications.

Repository Structure
Streamlined-Ticket-Assignment/
├─ docs/                          # Documentation, diagrams, exports
├─ screenshots/                   # Add your screenshots here
├─ update_sets/                   # (Optional) exported update sets
├─ src/
│  ├─ flows/                      # JSON/XML exports of flows (optional)
│  ├─ acls/                       # ACL export artifacts (optional)
│  ├─ table/                      # Table dictionary exports (optional)
│  └─ scripts/                    # Any client/server scripts (if added later)
├─ .gitignore
└─ README.md                      # This file

Set Up the Project (Step-by-Step)
1) Create Users

Path: All → Users (System Security)
Create two users (example):

Niranjan Manne (platform engineer)

Katherine Pierce (certification specialist)

Set first name, last name, email; leave others default or as required.

2) Create Groups

Path: All → Groups (System Security)
Create:

Platform — deals with platform-related issues

Certificates — deals with certification-related issues

3) Create Roles

Path: All → Roles (System Security)
Create:

platform_role — mapped to Platform group

certification_role — mapped to Certificates group

Keep role names lowercase with underscores for consistency.

4) Create the Operations Table

Path: All → Tables (System Definition) → New

Label: Operations related

Check Create module & Create mobile module

Table name: u_operations_related (auto)

Columns (Dictionary Entries):

Column label	Type	Reference	Notes
Service request No	String	—	Default: javascript:getNextObjNumberPadded() (optional)
Name	String	—	Requester name
Assigned to group	Reference	Group	Updated by flows
Assigned to user	Reference	User	Optional manual routing
Priority	Integer	—	1–5 or your scale
issue	Choice	—	Key driver for routing
Comment	String	—	Free-text
Ticket raised date	Date/Time	—	Set default to Now() if desired
(system fields)	Various	—	Created/Updated/By are system fields
5) Configure Choice Values

Path: Table form → Form Designer → issue (Choice)
Add values (examples):

unable to login to platform

404 error

regarding user

regarding certificates

You can add more over time; remember to align Flow conditions.

6) Assign Users & Roles to Groups

Platform group

Group Members: add Niranjan Manne

Group Roles: add platform_role

Certificates group

Group Members: add Katherine Pierce

Group Roles: add certification_role

7) Secure with Application Access & ACLs

Application Access (on table u_operations_related)

Read/Write operations allowed

Requires roles: platform_role, certification_role (as applicable)

Elevate to security_admin before modifying Application Access.

ACLs (Access Control → New)
Create record-level ACLs for u_operations_related:

create / read / write / delete → Allow if user has platform_role or certification_role.

Create field-level ACLs (examples):

u_priority, u_service_request_no, u_ticket_raised_date, u_issue → write allowed for the same roles, read for all authenticated users (adjust to policy).

8) Build the Flows (Automation)

Open Flow Designer → New → Flow

Flow A: Regarding Certificate

Trigger: Created or Updated on table Operations related

Condition: issue is "regarding certificates"

Action: Update Record

Record: From Trigger (Operations related)

Field: Assigned to group = Certificates

Run As: System user

Activate the flow.

Flow B: Regarding Platform

Trigger: Created or Updated on table Operations related

Condition:

issue is "unable to login to platform" OR

issue is "404 error" OR

issue is "regarding user"

Action: Update Record

Field: Assigned to group = Platform

Activate the flow.

9) Test the Automation

Create three test records in Operations related:

issue = regarding certificates → Assigned to group = Certificates

issue = unable to login to platform → Assigned to group = Platform

issue = 404 error → Assigned to group = Platform

Check:

Record updates reflect correct Assigned to group

Flow Designer → Executions show successful runs

How It Works (Logic Summary)

A ticket is created/updated on u_operations_related.

The appropriate Flow trigger evaluates the issue value.

If a condition matches, the Flow updates the record and sets Assigned to group to the mapped support group.

Agents in that group see the ticket in their queue immediately.

Troubleshooting
Symptom	Likely Cause	Fix
Flow never runs	Flow not Activated	Open Flow → Activate
Flow runs but no assignment	Condition doesn’t match exact choice text	Verify issue values and spelling
“Insufficient rights” errors	Missing roles or ACLs	Add platform_role / certification_role, review ACLs
Can’t edit Application Access	Not elevated	Click profile → Elevate role → security_admin
Updated but not visible to agents	Group membership missing	Add users to respective Groups
Performance & Best Practices

Keep Flow conditions specific but readable; group related values with OR.

Use data pills from the Trigger record; avoid unnecessary queries.

Keep choice values short, consistent, and documented.

Add Flow error handlers (Try/Catch) if you extend with lookups or notifications.

Use Update Sets to migrate between instances.

Security & Governance

Restrict table access using Requires role + ACLs.

Audit changes via System Logs and Flow Execution Details.

Store only necessary personal data; follow your organization’s retention policy.

Extending the Solution

Auto-assign to user based on on-call roster or skill.

Notify groups/users via Email/Slack/MS Teams after assignment.

Prioritization rules from impact/urgency or ML (Predictive Intelligence).

Catalog Item front door that writes into u_operations_related.

Dashboards for assignment and SLA metrics.

Glossary

Flow Designer: No/low-code automation engine in ServiceNow.

ACL: Access Control List; authorizes CRUD/field operations.

Application Access: Table-level gate that works with ACLs.

Data Pill: Token representing runtime data inside a Flow.

Choice Field: Field with predefined selectable values.

License

This project is released under the MIT License. You may use, modify, and distribute with attribution.

Maintainers

Project Owner: You / Your Team

Contact: your.email@example.com 
