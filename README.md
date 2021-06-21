# ⚡ Admin Power Pack ⚡

The Admin Power Pack is a tool for Looker administrators to accomplish certain management tasks more efficiently. It is a Looker add-on application built using the Looker extension framework. Currently it offers utilities for extended functionality in two main areas: **schedules** and **users**. Key features include:

- Bulk management of user accounts (select multiple users at once)
- Bulk management of dashboard schedules (edit multiple scheduled plans in one table)
- Execute actions that aren't available in the standard UI (e.g. manage user credential objects)

# Usage

## Common Workflows

### Bulk Creating New Users

1. Use the `Bulk create from mapping` function to upload a CSV of user emails, names, and User Attributes
1. Select the newly created users by using the `Select by email address` function
1. Use the `Add users to groups` and `Set roles for users` functions to add these new users to specific groups or roles
1. Use the `Bulk send email creds` to send the Welcome Email to these users

### Updating Email Addresses For Authentication Integration

1. Follow the steps for enabling user authentication [here](https://docs.looker.com/admin-options/security)
1. If the "Merge by Email" option is not working due to differences in the email credential (e.g. case sensitivity or a different domain), use the `Bulk update from mapping` function to update email addresses from a CSV mapping
1. Finish the authentication setup
1. Use the `Find & Replace Emails` function in the Schedules tab to bulk update the destination email addresses for all schedules

### Switching to a new Idp

1. Notify users not to log in and disable the existing IdP authentication in Admin/Authentication
1. Use the `Auto-fill Email Credentials` function to create email creds (many users may only have IdP or API based creds)
1. Use the `Bulk update from mapping` function to update email creds to new email addresses
1. `Delete Creds` from the old Idp
1. Enable new IdP authentication in Admin/Authentication
1. Notify users to log in
1. Use the `Find & Replace Emails` function in the Schedules tab to bulk update the destination email addresses for all schedules

# Installation

## Requirements

This extension requires Looker release 7.8 or later. Be sure to enable the “Extension Framework” labs feature.

**Note**: Any user who has model access to the "admin_power_pack" model (possibly via the "All" model set) will be able to see the APP in the Browse dropdown menu. However:

- Extensions always run with the permissions of the currently logged-in user and can't escalate permissions or do anything a regular user can't do. A non-Admin cannot be granted additional powers via an extension.
- The APP checks the user roles and displays a friendly message page if the user is not an Admin.

However in order to avoid confusion, it is still recommended that only Looker admins have access to the admin_power_pack model. This may mean that the standard roles will need to be changed to not use the “All” model set.

- There is no need to create new model sets or roles for the APP, since Admins can always see all models.
- Refer to the documentation on [Setting Permissions for Looker Extensions](https://docs.looker.com/data-modeling/extension-framework/permissions) for more details on extension permissions.

## local install

1. Create a New Project
   - Add project with a name such as "admin_power_pack"
   - As the "Starting Point" select "Blank Project". You'll now have a new project with no files
2. Create a project manifest based on this [example](manifest.lkml)
   - Use the `file` parameter to host the application javascript in the LookML repo and include the [javascript file](looker_admin_power_pack.js) in the LookML project
3. Create a new model file in your project named "admin_power_pack.model"
   - Provide a [connection](https://docs.looker.com/r/lookml/types/model/connection) value (see above notes on models and connections)
   - Make sure to [configure the model you created](https://docs.looker.com/r/develop/configure-model) so that it has access to the connection. Develop => Manage LookML Projects => Configure button next to the new model
4. Configure git for the project and deploy to production
   - You can use the "Create Bare Repository" (server only) or create a new repository on GitHub 
   - [Commit your changes](https://docs.looker.com/r/develop/commit-changes) and [deploy them to production](https://docs.looker.com/r/develop/deploy-changes) through the Projects page UI
   - Once deployed, you will now see "Admin Power Pack" in the Browse menu
