Odoo Record-Level Security: Personal Document Restriction
Purpose
The goal of this configuration is to restrict non-administrative users so they can only access documents (Contacts or Sales Orders) that they own or are assigned to. This prevents users from viewing sensitive data belonging to other salespeople or departments.

Core Components
1. Security Grouping
Restricted Group: A custom security group (e.g., "User: Own Documents Only") is created for standard users.

Inheritance: This group inherits from Internal User to provide basic system access without granting broad data permissions.

2. Record Rules (The Filtering Logic)
The heart of the process is a Record Rule applied to specific models like res.partner (Contacts) or sale.order (Sales Orders).

Domain Force: The rule uses a Python-based domain to filter the database query.

Logic: ['|', ('user_id', '=', user.id), ('user_id', '=', False)].

Explanation: This allows the user to see records where they are the assigned salesperson OR records that have no salesperson assigned yet (unassigned/public).

Global Flag: The "Global" setting is unchecked to ensure this restriction only targets the restricted group and does not block the System Administrator.

3. Administrative Override
To ensure managers can still see all data, a separate rule with the domain [(1, '=', 1)] is assigned to the Sales / Administrator group.

Since Odoo security is additive, the Administrator's "See All" permission takes precedence over any restrictions.

Key Technical Takeaways
Avoid Global Rules for Restrictions: Global rules apply to everyone; unchecking "Global" allows for group-specific logic.

Additive Permissions: If a user can see everything, check if they belong to a default Odoo group (like Sales / All Documents) that provides a [(1, '=', 1)] override.

youtube link : https://youtu.be/DfLykEuxWZg
