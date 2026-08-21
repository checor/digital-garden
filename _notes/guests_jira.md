---
title: Adding guest users to a Jira board
---
Creating a guest that only has access to one Jira Board, is a PITA. Depending on how Jira is configured, this may take more time than expected. These instructions are for Jira Cloud.
## Default permission scheme

https://prepmod.atlassian.net/secure/admin/ViewPermissionSchemes.jspa

Each board has a permission scheme. Most schemes are pretty permissive that will allow any user in the default group "jira users" to get access. That means by default, most people invited will see all of the boards.

Modify the schema to don't allow anyone unless invited.

* Don't use the [default group](https://support.atlassian.com/user-management/docs/default-groups-and-permissions/) to allow permissions.
* Create a group for you in-home/trusted users, if required.
* Only allow people invited as member to view/create/edit.

Make sure a new invited accounts doesn't see anything. You can [impersonate](https://support.atlassian.com/user-management/docs/log-in-as-another-user/) users from the admin site, or user the [Admin Helper](https://support.atlassian.com/jira-cloud-administration/docs/use-the-jira-admin-helper/).

### New permission Role & Schema

https://support.atlassian.com/jira-cloud-administration/docs/manage-project-roles/
https://prepmod.atlassian.net/secure/project/ViewProjectRoles.jspa
https://support.atlassian.com/jira-software-cloud/docs/what-are-team-managed-and-company-managed-projects/

This step is only required for company managed projects.

Create a new project role with the name of the external company (or guest).

Create a new permission scheme for the company. Is easier to start copying one. Add the new role to the things they need to see/edit.

Go to the board and add change the schema to use that one.

Add the guests using the role your previously created.

## Team managed project

This one is easier. Just invite the new member(s) as member of the board, they should get access to view/edit on that board only.

When you can, ensure any new project is team managed, that way is easier to have guests. Just create another schema so guests don't have access to other boards.