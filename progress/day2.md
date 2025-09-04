📅 Day 2 Progress – Streamlining Ticket Assignment for Efficient Support Operations
Overview

On Day 2, we successfully moved from the foundation phase (Users, Groups, Roles, Tables, ACLs) into automation and flow implementation. This was a critical milestone where we automated the process of assigning support tickets dynamically to the correct group based on the type of issue raised.

This step directly supports our project’s goal: reducing manual intervention and improving the speed of ticket resolution.

Key Tasks Completed
1️⃣ Created & Configured Flow Designer Flows
Flow 1 – Regarding Certificates

Flow Name: Regarding Certificate

Trigger:

Type: Created or Updated Record

Table: Operations related

Condition: issue is regarding certificates

Action:

Action Type: Update Record

Record: Operations related (from trigger)

Field Updated: Assigned to group = Certificates

Purpose:
Ensures that all tickets with issue = regarding certificates are automatically routed to the Certificates group for faster resolution.

Flow 2 – Regarding Platform

Flow Name: Regarding Platform

Trigger:

Type: Created or Updated Record

Table: Operations related

Condition:

issue is unable to login to platform

OR issue is 404 error

OR issue is regarding user

Action:

Action Type: Update Record

Record: Operations related (from trigger)

Field Updated: Assigned to group = Platform

Purpose:
Ensures that all platform-related issues (login problems, 404 errors, user issues) are assigned to the Platform group automatically.

2️⃣ Testing & Verification

Created multiple test records with different issues.

Verified that records are automatically routed to the correct group.

Observed successful execution in Flow Designer Execution Logs.
