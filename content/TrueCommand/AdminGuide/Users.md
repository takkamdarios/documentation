---
title: "Users and Teams"
description: "Describes the Users and Teams screens and provides information on administering TrueCommand user accounts, groups, and teams."
weight: 20
aliases:
 - /truecommand/administration/users/
tags:
- users
---

{{< toc >}}

TrueCommand has a robust user management system that lets administrators personalize the experience for each user account.
You can create user accounts in the TrueCommand interface.
Alternately, LDAP can automatically create new user accounts when someone logs into TrueCommand with their LDAP credentials.

You can also manage many user accounts simultaneously by organizing them into Teams.

## Adding TrueCommand User Accounts

To create a new TrueCommand user account, click the gear <i class="material-icons" aria-hidden="true" title="Settings">settings</i> icon to open the settings dropdown menu, then click **Users** to open the **Users** screen. 
Click **NEW USER** to open the **New User** screen.

![UsersAdd](/images/TrueCommand/Users/UsersNewUser.png "Adding a new user")

A user can be a person, location, department, or whatever you choose. Enter a descriptive name in **Username**. 
You can also enter a full name for the user but this is not required. 
For example, if adding a person enter *john smith* or if adding a department enter *auditing*.
Enter a password for the user account. 
User names and passwords are case-sensitive.

TrueCommand uses the default authentication method to create unique credentials for logging in to the web interface.
The administrator must provide the user with their login credentials.

If already created, you can assign the new user to a team. Click the **Teams** down arrow and select a team from the dropdown list. 
If not created, you can go to **Teams** to create the team, then return to the **Users** screen and edit the user, or you can add the user when you create the team.
You can assign users to multiple teams.

If creating an administrator user, select **TrueCommand Administrator**.

Click **Create User**. The new user is added to the list of users on the **Users** screen. 

![UsersList](/images/TrueCommand/Users/UsersList.png "Users Screen")

## Editing TrueCommand User Accounts

To edit TrueCommand user account details, click the gear <i class="material-icons" aria-hidden="true" title="Settings">settings</i> icon to open the settings dropdown menu, then click **Users**.
Locate the user on the list, then click on the edit <i class="material-icons" aria-hidden="true" title="Configure">edit</i> icon.
If the user is a non-administrator, the screen opens with options to add the user to groups, teams, and to add systems the user needs access to.

![EditNonAdministratorUserScreen](/images/TrueCommand/Users/EditNonAdministratorUserScreen.png "Edit Non-Administrator User Screen")

Administrator users do not include the option to add teams, system access, or system groups from the edit user screen.

You can configure several different user elements, including the avatar, user personal details, contact email address, team or group membership, and system access permissions. 
Select **TrueCommand Administrator** to make the user an administrator account.
To change the user password, enter and confirm the new password.

Click **SAVE CHANGES** after completing changes to the user.

## User Details (Profile) Screen 
Users can view details about their account by clicking on the avatar menu, then clicking on **Profile**. This opens the same user details screen as clicking on the edit icon for a user on the **Users** screen.
Administrator users can go to the **Users** screen, then click on the edit icon for the user to open the user details screen for the user. 
Non-administrator users do no have access to the **Users** screen.

To discard any changes and revert to the original field contents, click **RESET FORM** before you click **SAVE CHANGES**.

{{< truetable >}}
| Setting | Description |
|---------|-------------|
| **Username** | Enter or change the username. |
| **Full Name** | Enter or change the user full name. If not a person, enter the full name of the location, department, etc. for the user. |
| **Title** | Enter or change the user title. |
| **Email** | Enter or change the user email. If [SMTP]({{< relref "AlertManage.md" >}}) is not set up, an error message displays at the bottom of the screen stating **Failed to send email. Are your SMTP settings configured?**. Admins can click the **CONFIGURE** button to open the SMTP settings window. Before adding a user email, go to **Alert Services** and verify you have set up the SMTP service. |
| **Phone** | Enter or change the user phone number. |
| **Two Factor Authentication** | Enables [Two Factor Authentication]({{< relref "truecommand/tcgettingstarted/useraccounts.md" >}}), which requires the user to enter a validation code emailed to them after they enter their username, password, and click **SIGN IN** on the login screen. |
| **TrueCommand Administrator** | Designates the account as an administrator. |
| **Password** | New user password. |
| **Password Confirm** | Confirms new user password. |
{{< /truetable >}}

### Managing Team Membership
The **CREATE A NEW TEAM** button displays in the **Join Teams** area of the user details screen if a TrueCommand team does not exist.
When teams exist, the **JOIN TEAM** button displays instead.

Click **JOIN TEAM** to display the list of existing teams, then select a team to add to the user.
Users can be members of multiple teams.
TrueCommand applies team permissions to any user added to a team, but setting specific permissions for users can override related team permissions.
Use the **Teams** screen to create new teams or edit existing ones.

### Managing System and Group Access
To limit non-administrative account access to connected systems, administrators can add the system(s) and group(s) on the user details screen.
The user details screen allows administrators to select an existing group with the **ADD GROUP** or system with the **ADD SYSTEM** buttons.

Administrators must first add the system connections and/or system groups. For more information see [Connecting the First TrueNAS System]]({{< relref "ConnectingTrueNAS.md" >}}). 
Add systems from the main **Dashboard**, **Fleet Dashboard**, or the **Systems** screens.

{{< include file="content/_includes/TCPermissionsHierarchy.md" >}}

To add user system access, on the user details screen, click **ADD SYSTEM**, then select a system from the dropdown list. The system name displays in the **System Access** area. 
To restrict access to only viewing system details, select the **read**  on the **Permission** dropdown list.
To remove their access to a particular system, click the delete icon to remove the system. 

If TrueCommand has system groups, the **ADD GROUP** button displays.
To add the user to an existing group, click **ADD GROUP**, then select the group from the dropdown list. A group user member has access to all the systems in that group.
To choose the user group permissions, select **read** or **read/write** from the **Permissions** dropdown list.
To remove their access to a particular system group, click the delete icon for the group.

## Resetting a User Password at Login

{{< include file="content/_includes/TCResettingUserPassword.md" >}}

## Resetting a User Password from the Command Line

The Docker version of TrueCommand allows you to reset user passwords from the command line.
Open the **Shell** on the TrueNAS system running the TrueCommand container and use the following command, replacing the values in brackets with their appropriate values. 

```
docker exec -it [docker instance ID] resetpw [username]
```

## Deleting TrueCommand User Accounts
To delete an account, click the gear <i class="material-icons" aria-hidden="true" title="Settings">settings</i> icon to open the settings dropdown menu, then click **Users**.
On the **Users** screen, click the delete icon <i class="material-icons" aria-hidden="true" title="Delete">delete</i> to the right of the user you want to delete.
A confirmation dialog opens to confirm user deletion.

![Users Delete](/images/TrueCommand/Users/UsersDeleteUser.png "Users Delete")

## Managing Teams
To create a team, click the gear <i class="material-icons" aria-hidden="true" title="Settings">settings</i> icon to open the settings dropdown menu, then click **Teams**.
The **Teams** screen displays a list of existing teams created in TrueCommand.

![TeamsList](/images/TrueCommand/Teams/TeamsList.png "Teams List")

Clicking **NEW TEAM** to open the **New Team** screen.

![TeamsAdd](/images/TrueCommand/Teams/TeamsNewTeam.png "Teams: Add")

Enter a name and select an avatar for the new team.
You can edit team permissions and settings after creating it.

### Editing Teams
To edit a team, click the gear <i class="material-icons" aria-hidden="true" title="Settings">settings</i> icon to open the settings dropdown menu, then click **Teams**.
Click on the edit icon for the team to open the team details screen. 

![Teams Edit](/images/TrueCommand/Teams/TeamsEdit.png "Teams Edit")

You can change a team profile avatar, name, add new team members, add existing systems and groups to a team, or grant team members permission to create new TrueCommand alert rules by selecting **Enable alert creation**.

To change team members, click on the edit icon <i class="material-icons" aria-hidden="true" title="Configure">edit</i> for the team you selected on the list. 
Click **ADD USER** in the **Members** section to add new users to the team. Members of the team display in this area.
To remove users from the team, click the delete icon next to the user(s) you want to remove.

You can configure system permissions the same way as individual user system access.
Note that individual user account permissions can override team permissions.

### Deleting Teams
To delete account details and permissions, click the gear <i class="material-icons" aria-hidden="true" title="Settings">settings</i> icon to open the settings dropdown menu, then click **Teams**.
On the **Teams** page, click the delete icon <i class="material-icons" aria-hidden="true" title="Delete">delete</i> to the right of the team you want to delete.
A confirmation dialog opens to confirm the team deletion.

![Teams Delete](/images/TrueCommand/Teams/TeamsDeleteTeam.png "Teams Delete")

{{< hint type=note >}}
Deleting a team does not remove users or systems assigned to that team from TrueCommand.
{{< /hint >}}
