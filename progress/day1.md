Project Title

Streamlining Ticket Assignment for Efficient Support Operations

Objective

To automate ticket routing within ServiceNow by accurately assigning incoming support tickets to the appropriate group based on predefined conditions (roles, categories, issues).
This reduces manual effort, speeds up resolution, and improves customer satisfaction.

Progress Achieved Today

1. Created Users

Users Added:

Niranjan Manne (Platform related issues)

Katherine Pierce (Certification related issues)

Configured First Name, Last Name, and Email for both users.

Verified successful creation from System Security → Users.

2. Created Groups

Groups Added:

Platform Group – Handles platform-related issues.

Certification Group – Handles certification-related issues.

Added descriptions for better understanding of responsibilities.

3. Created Roles

Roles Added:

platform_role – Assigned to Platform Group.

certification_role – Assigned to Certification Group.

4. Created Table

Table Name: Operations related (u_operations_related)

Columns Added:
Assigned to Group, Assigned to User, Service Request No, Name, Ticket Raised Date, Priority, Issue (Choice field), Comment, Created By, Updated By, etc.

Enabled Create Module and Create Mobile Module.

Created Choices for Issue Field:

Unable to login to platform

404 Error

Regarding certificates

Regarding user expired

📸 Image Placeholder: Insert Screenshot of Table Columns
(Use: Create Table.png)

5. Assigned Users & Roles to Groups

Added Katherine Pierce to Certification Group and assigned certification_role.

Added Niranjan Manne to Platform Group and assigned platform_role.

6. Assigned Roles to Table

Opened Application Access for u_operations_related.

Elevated Security Role → Added platform_role and certification_role to both Read and Write operations.

7. Created ACLs

Created Access Control List (ACL) rules for:

Read

Write

Delete

Create

Ensured admin role was assigned under Requires Role.

📸 Image Placeholder: Insert Screenshot of ACL List
(Use: Created Acls.png)

Status

✅ Completed: User creation, Group creation, Role creation, Table creation, Role & User assignment, ACL configuration.
⏳ Next Step:

Implement Flow Designer logic for automated ticket assignment based on Issue field → Assign tickets dynamically to appropriate groups.
