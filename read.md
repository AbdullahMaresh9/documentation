# دليل المستخدمين - Odoo Documentation

تم تجميع كافة صفحات دليل المستخدمين في هذا الملف بالكامل، مع الحفاظ على محتوى الصفحات الأصلية من مستودع الوثائق دون اختصار.

## فهرس المحتوى

- [Two-factor authentication](#two-factor-authentication)
- [Access rights](#access-rights)
- [Microsoft Azure sign-in authentication](#microsoft-azure-sign-in-authentication)
- [Facebook sign-in authentication](#facebook-sign-in-authentication)
- [Google Sign-In Authentication](#google-sign-in-authentication)
- [Change languages](#change-languages)
- [LDAP authentication](#ldap-authentication)
- [User portals](#user-portals)
  - [Granting portal access](#granting-portal-access)
  - [Updating portal info](#updating-portal-info)

---

# Two-factor authentication

*Two-factor authentication (2FA)* is a security measure that helps prevent unauthorized access to user accounts.

Practically, 2FA means storing a secret in an *authenticator*, usually on a mobile phone, and exchanging a code from the authenticator when logging in.

This means an unauthorized user would need to guess the account password and have access to the authenticator, which is a more difficult proposition.

> Note: Some governments, such as the [Australian](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/finance/fiscal_localizations/australia.rst) government, require 2FA. For these [fiscal localizations](https://github.com/AbdullahMaresh9/documentation/tree/19.0/content/applications/finance/fiscal_localizations), it is not possible to deactivate 2FA.

## Requirements

> Important: These lists are just examples. They are **not** endorsements of any specific software.

Phone-based authenticators are the easiest and most commonly used. Examples include:

- [Authy](https://authy.com/)
- [FreeOTP](https://freeotp.github.io/)
- [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=en)
- [LastPass Authenticator](https://lastpass.com/auth/)
- [Microsoft Authenticator](https://www.microsoft.com/en-gb/account/authenticator?cmp=h66ftb_42hbak)

Password managers are another option. Common examples include:

- [1Password](https://support.1password.com/one-time-passwords/)
- [Bitwarden](https://bitwarden.com/help/article/authenticator-keys/)

> Note: The remainder of this document uses Google Authenticator as an example, as it is one of the most commonly used. This is **not** an endorsement of the product.

## Two-factor authentication setup

After selecting an authenticator, log in to Odoo, then click the profile avatar in the upper-right corner, and select **My Preferences** from the resulting drop-down menu.

Click the *Security* tab, then click **Enable 2FA**.

![The account secuirty tab in a user profile](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/2fa/account-security.png)

This generates an *Access Control* pop-up window that requires password confirmation to continue. Enter the Odoo account password, then click **Confirm Password**. Next, a *Two-Factor Authentication Activation* pop-up window appears, with a QR code.

![The 2fa authentication QR code in Odoo](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/2fa/qr-code.png)

> Using the desired authenticator application, scan the QR code.

> Tip: If scanning the screen is not possible (e.g., the setup is being completed on the same device as the authenticator application), click the provided **Cannot scan it?** link, or copying the code to set up the authenticator manually, is an alternative.
>
> ![A 2fa secret code on an authentication popup](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/2fa/secret-visible.png)

Afterwards, the authenticator should display a *verification code*.

![A view of the Google authenticator app with the six digit code for 2fa](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/2fa/authenticator.png)

Enter the code into the **Verification code** field, then click **Enable Two-Factor Authentication**.

![The success message that appears in a user profile when 2fa is successfully enabled](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/2fa/2fa-enabled.png)

## Logging in

To confirm 2FA setup is complete, log out of Odoo.

On the login page, input the username and password, then click **Log in**. On the **Two-factor Authentication** page, input the code provided by the chosen authenticator in the **Authentication Code** field, then click **Log in**.

![The login page with 2fa enabled](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/2fa/2fa-login.png)

> Danger: If a user loses access to their authenticator, an administrator **must** deactivate 2FA on the account before the user can log in.

## Enforce two-factor authentication

To enforce 2FA for every user in the database, navigate to the **Settings** app. In the **Permissions** section, tick the checkbox labeled **Enforce two-factor authentication**. Then, use the radio buttons to choose whether to apply this setting to **Employees only** or **All users**.

> Note: Selecting **All users** applies the setting to portal users in addition to employees.

Click **Save** to commit any unsaved changes.

![The enforce two factor setting in the Settings application](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/2fa/enforce-settings.png)

> Note: If users are frequently required to log in again and complete two-factor authentication, it may be due to session or inactivity timeout policies configured for one or more of their user groups.
>
> Administrators can configure these policies using the *Timeouts* settings available on user groups when the *auth_timeout* module is installed. See [Session and inactivity timeouts](#session-and-inactivity-timeouts) for more information.

---

# Access rights

*Access rights* are permissions that determine the content and applications users can access and edit. In Odoo, these permissions can be set for individual users or for groups of users. Limiting permissions to only those who need them ensures that users do not modify or delete anything they should not have access to.

**Only** an *administrator* can change access rights.

> Danger: Making changes to access rights can have a detrimental impact on the database. This includes *impotent admin*, which means that no user in the database can make changes to the access rights. For this reason, Odoo recommends contacting an Odoo Business Analyst, or our Support Team, before making changes.

> Tip: A user **must** have the specific *Administration* access rights set on their user profile, in order to make changes on another user's settings for access rights.
>
> To access this setting, navigate to **Settings app --> Manage users --> select a user --> Access Rights tab --> Administration section --> Administration field**.
>
> Once at the setting, an already existing administrator **must** change the setting in the **Administration** field to **Access Rights**.
>
> Once complete, click **Save** to save the changes, and implement the user as an administrator.

## Manage roles

[Individual users](#) are assigned a *Role* when they are added to the database. Roles determine the level of access a user has, while the specific access rights associated with each role are defined by [groups](#create-and-modify-groups).

The four available roles are:

- *Administrator*: An internal user with access to technical features, product creation, export, and other advanced permissions.
- *User*: An internal user that typically has access to the back end and can create and edit records but has less overall access than an administrator.
- *Portal*: A customer or supplier who accesses their own data through the portal.
- *Public*: A website visitor or other external user. They generally have the least access.

## Manage user permissions

The access rights for [individual users](#) are set when the user is added to the database, but they can be adjusted at any point in the user's profile.

To make changes to a user's rights, click the desired user to edit their profile.

![Users menu in the Users & Companies section of the Settings app of Odoo](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/access_rights/navigate-to-users-menu.png)

On the user's profile page, in the **Access Rights** tab, scroll down to view the current permissions.

The permissions available on the *Access Rights* tab are determined by the groups associated with the user's role. Roles provide a predefined set of permissions, while groups define the specific access rights granted to users.

For each app, use the drop-down menu to select what level of permission this user should have. The options vary for each section, yet the most common are: **Blank/None**, **User: Own Documents**, **User: All Documents**, or **Administrator**.

The **Administration** field in the **Access Rights** tab has the following options: **Settings** or **Access Rights**.

![The Sales apps drop-down menu to set the user's level of permissions](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/access_rights/user-permissions-dropdown-menu.png)

### Manage specific permissions

While access rights are typically assigned in bundles under specific roles, they can also be set as explicit permissions.

> Example: When a user is assigned the **Administrator** permission for **Timesheets**, it gives them full access to that app. That user, while holding full access, can *still* have their ability to manage *their own* timesheets restricted — such as in the case of a salaried payroll administrator who does not need to track time.

To manage specific permissions, [developer mode](#) must be enabled.

After that, navigate to the **Settings** app. Then click **Manage Users**, select a user, and go to the **Technical Access Rights** tab. From here, **Groups** can be edited, and specific access rights can be managed across the various sections. If no changes are made to these groups, then their permissions will mirror the selections made in the **Access Rights** tab.

- **Selected groups**: a list of detailed access rights, set by choices made in the **Access Rights** tab.
- **Groups added automatically**: *implied* permissions that are *inherited* with the explicit permissions already granted to the user. The values here will match the values listed under a given *Group*'s form located under the **Users & Companies --> Groups** menu, in the **Inherited** tab.

![The technical access rights tab opened up for a user profile](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/access_rights/tech-access-rights.png)

> Example: When the *Sales Administrator* permission set is assigned to a user, the *Canned Responses Administrator* permissions are inherited automatically. These assignments are reflected across the values listed in the **Selected Groups** and **Groups added automatically** tables, respectively.

To add a permission to this user profile, click **Add a line** in the **Selected groups** table, and then add permissions to this user profile. To remove a permission, click the **(cancel)** icon at the end of that permission's row.

> Warning: Removing permissions from the **Selected Groups** list can impact what permissions are listed in the **Groups added automatically** list, since selected permission groups inform what permission groups are added automatically.

Clicking on the permission itself will open a group management form. Learn more about [managing groups](#create-and-modify-groups).

Any permission in the **Groups added automatically** section is implied or required by the permission shown in the **Selected groups** section. These cannot be removed, but more users can be given these permissions by clicking on the permission itself, and then adding the user to that permission's group.

> Note:
> - Any permission in green is already provided by another permission (for example, setting the **Website** app's permission to **Editor and Designer** will also give that user the **Restricted Editor** permission).
> - Any permissions in red are conflicting and cannot be active at the same time.
> - Any permissions in *italics* are implied by a **Selected group** (these are usually found in the **Groups added automatically**).

## Create and modify groups

*Groups* are app-specific sets of permissions that are used to manage common access rights for a large amount of users. Administrators can modify the existing groups in Odoo, or create new ones to define rules for models within an application.

To access groups, first activate Odoo's [developer mode](#), then go to **Settings app --> Users & Companies --> Groups**.

![Groups menu in the Users & Companies section of the Settings app of Odoo](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/access_rights/click-users-and-companies.png)

To create a new group from the **Groups** page, click **Create**. Then, from the blank group form, select an **Application**, and complete the group form (detailed below).

To modify an existing group, click an existing group from the list displayed on the **Groups** page, and edit the contents of the form.

Enter a **Name** for the group and select the checkbox next to **Share Group**, if this group was created to set access rights for sharing data with some users.

> Important: Always test the settings being changed to ensure they are being applied to the correct users.

The group form contains multiple tabs for managing all elements of the group. In each tab, click **Add a line** to add a new row for users or rules, and click the **(cancel)** icon to remove a row.

![Tabs in the Groups form to modify the settings of the group](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/access_rights/groups-form.png)

- **Users** tab: lists the current users in the group. Users listed in black have administrative rights. Users without administrative access appear in blue. Click **Add a line** to add users to this group.
- **Inherited** tab: Inherited means that users added to this group are automatically added to the groups listed on this tab. Click **Add a line** to add inherited groups.

  > Example: For example, if the group *Sales/Administrator* lists the group *Website/Restricted Editor* in its **Inherited** tab, then any users added to the *Sales/Administrator* group automatically receive access to the *Website/Restricted Editor* group, as well.

- **Menus** tab: defines which models the group can have access to. Click **Add a line** to add a specific menu.
- **Views** tab: lists which views in Odoo the group has access to. Click **Add a line** to add a view to the group.
- **Access Rights** tab: lists the first level of rights (models) that this group has. The **Name** column represents the name for the current group's access to the model selected in the **Model** column.

  To link a new access right to a group, click **Add a line**. Select the appropriate model from the **Model** drop-down, then enter a name for the access right in the **Name** column. For each model, enable the following options as appropriate:

  - **Read**: Users can see the object's existing values.
  - **Write**: Users can edit the object's existing values.
  - **Create**: Users can create new values for the object.
  - **Delete**: Users can delete values for the object.

  > Tip: While there are no conventions for naming access rights, it is advisable to choose a name that identifies its purpose.
  >
  > For example, the access that purchase managers have to the **Contact** model could be named `res.partner.purchase.manager`. This consists of the technical name of the model, followed by a name identifying the group of users in question.
  >
  > ![Name of access rights to a model](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/access_rights/name-field.png)
  >
  > To find the model's technical name from the current view, first enter a placeholder text in the **Name** field, then click the **Model** name, then the **(Internal link)** icon.

- **Record Rules**: lists the second layer of editing and visibility rights. **Record Rules** overwrite, or refine, the group's access rights. Click **Add a line** to add a record rule to this group. For each rule, choose values for the following options:

  - **Apply for Read**
  - **Apply for Write**
  - **Apply for Create**
  - **Apply for Delete**

  > Important: Record rules are written using a *domain*, or conditions that filter data. A domain expression is a list of such conditions. For example:
  >
  > `[('mrp_production_ids', 'in', user.partner_id.commercial_partner_id.production_ids.ids)]`
  >
  > This record rule is to enable MRP consumption warnings for subcontractors.
  >
  > Odoo has a library of preconfigured record rules for ease of use. Users without knowledge of domains (and domain expressions) should consult an Odoo Business Analyst, or the Odoo Support Team, before making changes.

## Session and inactivity timeouts

When the `auth_timeout` module is installed, administrators can configure automatic logout rules for users assigned to a specific user group. This module may be installed automatically by certain localizations (for example, the Australian Payroll localization).

Once installed, a *Timeouts* tab appears on user group forms. This tab allows administrators to define how long users can remain logged in under different conditions.

Two types of timeouts can be configured:

### Inactivity timeout

*Inactivity timeout* controls whether users are automatically logged out after a period of inactivity.

To enable inactivity timeout, click the *Timeouts* tab and select the **Inactivity** checkbox. Next, choose either **Screen lock** or **Screen lock with two-factor authentication** from the drop-down menu. This determines whether the user must complete 2FA verification when logging back in.

Then, enter the desired amount of time before enforcing screen lock, and select a unit of measure from the drop-down menu. Inactivity can be measured in minutes, hours, or days.

### Session timeout

*Session timeout* controls whether users are logged out after a fixed session duration, regardless of activity.

To enable session timeout, click on the *Timeouts* tab, and select the **Session** checkbox. Next, choose either **Logout** or **Logout with two-factor authentication** from the drop-down menu. This determines whether the user must complete 2FA verification when logging back in.

Then, enter the desired amount of time before the user is logged out, and select a unit of measure from the drop-down menu. Session timeouts can be measured in minutes, hours, or days.

## Superuser mode

*Superuser mode* allows the user to bypass record rules and access rights. To activate *Superuser mode*, first, activate [developer mode](#). Then, navigate to the *debug* menu, represented by a **(debug)** icon, located in the top banner.

Finally, towards the bottom of the menu, click **Become Superuser**.

> Important: Only users with *Settings* access for the *Administration* section of the *Access Rights* (in their user profile) are allowed to log in to *Superuser mode*.

> Danger: *Superuser mode* allows for circumvention of record rules and access rights, and therefore, should be exercised with extreme caution.
>
> Upon exiting *Superuser mode*, users may be locked out of the database, due to changes that were made. This can cause *impotent admin*, or an administrator without the ability to change access rights/settings.
>
> In this case contact Odoo Support here: [new help ticket](https://www.odoo.com/help). The support team is able to restore access using a support login.

To leave *Superuser mode*, log out of the account, by navigating to the upper-right corner, and clicking on the **OdooBot** username. Then, select the **Log out** option.

> Tip: An alternative way to activate *Superuser mode* is to log in as a superuser. To do that, navigate to the login screen, and enter the appropriate **Email** and **Password**.
>
> Instead of clicking **Login**, click **Log in as superuser**.

---

# Microsoft Azure sign-in authentication

The Microsoft Azure OAuth sign-in authentication is a useful function that allows Odoo users to sign in to their database with their Microsoft Azure account.

This is particularly helpful if the organization uses Azure Workspace, and wants employees within the organization to connect to Odoo using their Microsoft Accounts.

> Warning: Databases hosted on Odoo.com should not use OAuth login for the owner or administrator of the database as it would unlink the database from their Odoo.com account. If OAuth is set up for that user, the database will no longer be able to be duplicated, renamed, or otherwise managed from the Odoo.com portal.

> See also:
> - [../../productivity/calendar/outlook](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/productivity/calendar/outlook.rst)
> - [../email_communication/azure_oauth](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/general/email_communication/azure_oauth.rst)

## Configuration

Integrating the Microsoft sign-in function requires configuration on Microsoft and Odoo.

### Odoo System Parameter

First activate the [developer mode](#), and then go to **Settings --> Technical --> System Parameters**.

Click **New** and in the blank row that appears, add the following system parameter `auth_oauth.authorization_header` to the **Key** field, and set the **Value** to `1`. Then click **Save** to finish.

### Microsoft Azure dashboard

#### Create a new application

Now that the system parameters in Odoo have been set up, it's time to create a corresponding application inside of Microsoft Azure. To get started creating the new application, go to [Microsoft's Azure Portal](https://portal.azure.com/). Log in with the **Microsoft Outlook Office 365** account if there is one, otherwise, log in with a personal **Microsoft account**.

> Important: A user with administrative access to the *Azure Settings* must connect and perform the following configuration steps below.

Next, navigate to the section labeled **Manage Microsoft Entra ID** (formally *Azure Active Directory*). The location of this link is usually in the center of the page.

Now, click on the **Add (+)** icon, located in the top menu, and then select **App registration** from the drop-down menu. On the **Register an application** screen, rename the **Name** field to `Odoo Login OAuth` or a similarly recognizable title. Under the **Supported account types** section select the option for **Accounts in this organizational directory only (Default Directory only - Single tenant)**.

> Warning: The **Supported account types** can vary by Microsoft account type and end use of the OAuth. For example: Is the login meant for internal users within one organization or is it meant for customer portal access? The above configuration is used for internal users in an organization.
>
> Choose **Personal Microsoft accounts only** if the target audience is meant for portal users. Choose **Accounts in this organizational directory only (Default Directory only - Single tenant)** if the target audience is company users.

Under the **Redirect URL** section, select **Web** as the platform, and then input `https://<odoo base url>/auth_oauth/signin` in the **URL** field. The Odoo base URL is the canonical domain at which your Odoo instance can be reached (e.g. *mydatabase.odoo.com* if you are hosted on Odoo.com) in the **URL** field. Then, click **Register**, and the application is created.

#### Authentication

Edit the new app's authentication by clicking on the **Authentication** menu item in the left menu after being redirected to the application's settings from the previous step.

Next, the type of *tokens* needed for the OAuth authentication will be chosen. These are not currency tokens but rather authentication tokens that are passed between Microsoft and Odoo. Therefore, there is no cost for these tokens; they are used merely for authentication purposes between two APIs. Select the tokens that should be issued by the authorization endpoint by scrolling down the screen and check the boxes labeled: **Access tokens (used for implicit flows)** and **ID tokens (used for implicit and hybrid flows)**.

![Authentication settings and endpoint tokens](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/azure/authentication-tokens.png)

Click **Save** to ensure these settings are saved.

#### Gather credentials

With the application created and authenticated in the Microsoft Azure console, credentials will be gathered next. To do so, click on the **Overview** menu item in the left-hand column. Select and copy the **Application (client) ID** in the window that appears. Paste this credential to a clipboard / notepad, as this credential will be used in the Odoo configuration later.

After finishing this step, click on **Endpoints** on the top menu and click the *copy icon* next to **OAuth 2.0 authorization endpoint (v2)** field. Paste this value in the clipboard / notepad.

![Application ID and OAuth 2.0 authorization endpoint (v2) credentials](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/azure/overview-azure-app.png)

#### API permissions

Odoo needs permission to read the signed-in user's profile information from Microsoft Graph during the OAuth flow. At minimum, ensure the delegated permission **User.Read** is granted on the Azure app registration. Otherwise, the login process may fail or be unable to correctly link the Microsoft account to the Odoo user.

> Note: **User.Read** is often added by default for new app registrations, but it should be explicitly verified here.

To configure this, click on the **Manage** menu item in the left-hand column, then:

1. Click **API permissions** in the left menu.
2. Click **(+) Add a Permission**.
3. Select **Microsoft Graph** under **Commonly Used Microsoft APIs**.
4. Choose **Delegated Permissions**.
5. Search for **User.Read**, select it, and click **Add permissions**.

### Odoo setup

Finally, the last step in the Microsoft Azure OAuth configuration is to complete the setup in Odoo. Navigate to **Settings --> Integrations** and enable the checkbox for **OAuth Authentication**. Click **Save** to save the changes and refresh the page. Scroll down to the **Integrations** section, and click on **OAuth Providers**. Then click **New**.

Enter the **Provider name** as `Azure`. Paste the **Application (client) ID** from the previous section into the **Client ID** field. After completing this, paste the new **OAuth 2.0 authorization endpoint (v2)** value into the **Authorization URL** field.

For the **UserInfo URL** field, paste the following URL: `https://graph.microsoft.com/oidc/userinfo`

In the **Scope** field, paste the following value: `openid profile email`. Next, the Windows logo can be used as the CSS class on the login screen by entering the following value: `fa fa-fw fa-windows`, in the **CSS class** field.

Check the box next to the **Allowed** field to enable the OAuth provider. Finally, add `Microsoft Azure` to the **Login button label** field. This text will appear next to the Windows logo on the login page.

![Odoo provider setup in the Settings application](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/azure/odoo-provider-settings.png)

**Save** the changes to complete the OAuth authentication setup in Odoo.

### User experience flows

For a user to log in to Odoo using Microsoft Azure, the user must be on the **Odoo password reset page**. This is the only way that Odoo is able to link the Microsoft Azure account and allow the user to log in.

> Note: Existing users must reset their password to access the Odoo password reset page. New Odoo users must click the new user invitation link that was sent via email, then click on **Microsoft Azure**. Users should not set a new password.

To sign in to Odoo for the first time using the Microsoft Azure OAuth provider, navigate to the **Odoo password reset page** (using the new user invitation link). A password reset page should appear. Then, click on the option labeled **Microsoft Azure**. The page will redirect to the Microsoft login page.

Enter the **Microsoft Email Address** and click **Next**. Follow the process to sign in to the account. Should 2FA be turned on, then an extra step may be required.

Finally, after logging in to the account, the page will redirect to a permissions page where the user will be prompted to **Accept** the conditions that the Odoo application will access their Microsoft information.

---

# Facebook sign-in authentication

The *Facebook* OAuth sign-in function allows Odoo users to sign in to their database with their Facebook account.

> Danger: Databases housed on Odoo.com should **not** use OAuth login for the owner or administrator of the database, as it would unlink the database from their Odoo.com account. If OAuth is setup for that user, the database can no longer be duplicated, renamed, or otherwise managed from the Odoo.com portal.

## Meta for Developers setup

Go to [Meta for Developers](https://developers.facebook.com/) and log in. Click **My Apps**. On the **Apps** page, click **Create App**.

On the **Use cases** page, select **Authenticate and request data from users with Facebook Login**, then click **Next**.

In the **Add an app name** field, enter `Odoo Login OAuth`, or a similar title.

> Note: The **App contact email** automatically defaults to the email address associated with the Meta account. If this email address is not regularly monitored, it may be wise to use another email address.

Click **Next**. Review the **Publishing requirements**, the **Meta Platform Terms**, and **Developer Policies**. Then, click **Create app**.

> Important: Clicking **Create app** may require password re-entry.

### Customize app

After the new app is created, the **Dashboard** page appears, with a list of steps to be completed before the app can be published. From here, click **Customize adding a Facebook Login button**.

![The App Dashboard in the Meta for developers platform](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/facebook/app-requirements.png)

On the **Customize** page, click **Settings**.

In the **Valid OAuth Redirect URIs** field, enter `https://<odoo base url>/auth_oauth/signin`, replacing `<odoo base url>` with the URL of the applicable database.

> Example: If a database has the URL `https://example.odoo.com`, the URL `https://example.odoo.com/auth_oauth/signin` would be entered in the **Valid OAuth Redirect URIs** field.

Click **Save changes** when finished.

### Configure settings

At the far left of the page, click **App settings --> Basic**. This page contains additional settings that are required before the app can be submitted for approval.

In the **Privacy Policy URL** field, enter `https://www.odoo.com/privacy`.

> Note: `https://www.odoo.com/privacy` is the default privacy policy for databases hosted on Odoo.com.

Click the **App Icon** field to open a file upload window. From here, select and upload an app icon.

In the **User data deletion** field, enter `https://www.odoo.com/documentation/17.0/administration/odoo_accounts.html`.

> Note: This document provides instructions on how a user can delete their Odoo account.

Lastly, click the **Category** field, and select **Business and pages** from the drop-down menu.

Click **Save changes**.

![An example of the Basic Settings page in the Meta for developers platform](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/facebook/app-id.png)

### Capture app ID

After the app is created, and approved, select and copy the **App ID**. Paste this information on a clipboard or notepad file, as it is needed in a later step to complete the setup.

### Publish

On the left side of the page, click **Publish**. Depending on the status of the connected Facebook account, additional verification and testing steps may be required, and are listed on this page.

After reviewing the information, click **Publish**.

> See also: Additional information regarding Meta App Development, including further details on building, testing, and use cases, can be found in the [Meta for developers documentation](https://developers.facebook.com/docs/development).

## Odoo setup

First, activate [Developer mode](#).

Navigate to the **Settings app**, and scroll down to the **Integrations** section. There, tick the checkbox labeled, **OAuth Authentication**. Click **Save**.

![The enable OAuth setting in the Settings app](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/facebook/enable-oauth.png)

Then, sign in to the database once the login screen loads.

After successfully logging in, navigate to **Settings app --> Users & Companies --> OAuth Providers**. Click **Facebook Graph**.

In the **Client ID** field, enter the App ID from the previous section, then tick the **Allowed** checkbox.

![The Facebook Graph record in Odoo](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/facebook/facebook-graph.png)

---

# Google Sign-In Authentication

The *Google Sign-In Authentication* is a useful function that allows Odoo users to sign in to their database with their Google account.

This is particularly helpful if the organization uses Google Workspace, and wants employees within the organization to connect to Odoo using their Google Accounts.

> Warning: Databases hosted on Odoo.com should not use Oauth login for the owner or administrator of the database as it would unlink the database from their Odoo.com account. If Oauth is set up for that user, the database will no longer be able to be duplicated, renamed or otherwise managed from the Odoo.com portal.

> See also:
> - [Calendar with Google](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/productivity/calendar/google.rst)
> - [../email_communication/google_oauth](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/general/email_communication/google_oauth.rst)

## Configuration

The integration of the Google sign-in function requires configuration both on Google *and* Odoo.

### Google API Dashboard

1. Go to the [Google API Dashboard](https://console.developers.google.com/).
2. Make sure the right project is opened. If there isn't a project yet, click on **Create Project**, fill out the project name and other details of the company, and click on **Create**.

   ![Filling out the details of a new project](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/google/new-project-details.png)

   > Tip: Choose the name of the company from the drop-down menu.

### OAuth consent screen

1. On the left side menu, click on **OAuth consent screen**.

   ![Google OAuth consent selection menu](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/google/consent-selection.png)

2. Choose one of the options (**Internal** / **External**), and click on **Create**.

   ![Choice of a user type in OAuth consent](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/google/consent.png)

   > Warning: *Personal* Gmail Accounts are only allowed to be **External** User Type, which means Google may require an approval, or for *Scopes* to be added on. However, using a *Google WorkSpace* account allows for **Internal** User Type to be used.
   >
   > Note, as well, that while the API connection is in the *External* testing mode, then no approval is necessary from Google. User limits in this testing mode is set to 100 users.

3. Fill out the required details and domain info, then click on **Save and Continue**.
4. On the **Scopes** page, leave all fields as is, and click on **Save and Continue**.
5. Next, if continuing in testing mode (*External*), add the email addresses being configured under the **Test users** step by clicking on **Add Users**, and then the **Save and Continue** button. A summary of the app registration appears.
6. Finally, scroll to the bottom, and click on **Back to Dashboard**.

### Credentials

1. On the left side menu, click on **Credentials**.

   ![Credentials button menu](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/google/credentials-button.png)

2. Click on **Create Credentials**, and select **OAuth client ID**.

   ![OAuth client id selection](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/google/client-id.png)

3. Select **Web Application** as the **Application Type**. Now, configure the allowed pages on which Odoo will be redirected.

   In order to achieve this, in the **Authorized redirect URIs** field, enter the database's domain immediately followed by `/auth_oauth/signin`. For example: `https://mydomain.odoo.com/auth_oauth/signin`, then click on **Create**.

4. Now that the *OAuth client* has been created, a screen will appear with the **Client ID** and **Client Secret**. Copy the **Client ID** for later, as it will be necessary for the configuration in Odoo, which will be covered in the following steps.

## Google Authentication on Odoo

### Retrieve the Client ID

Once the previous steps are complete, two keys are generated on the Google API Dashboard: **Client ID** and **Client Secret**. Copy the **Client ID**.

![Google OAuth Client ID generated](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/google/secret-ids.png)

### Odoo activation

1. Go to **Odoo General Settings --> Integrations** and activate **OAuth Authentication**.

   > Note: Odoo may prompt the user to log-in again after this step.

2. Go back to **General Settings --> Integrations --> OAuth Authentication**, activate the selection and **Save**. Next, return to **General Settings --> Integrations --> Google Authentication** and activate the selection. Then fill out the **Client ID** with the key from the Google API Dashboard, and **Save**.

   ![Filling out the client id in Odoo settings](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/google/odoo-client-id.png)

   > Note: Google OAuth2 configuration can also be accessed by clicking on **OAuth Providers** under the **OAuth Authentication** heading in **Integrations**.

## Log in to Odoo with Google

To link the Google account to the Odoo profile, click on **Log in with Google** when first logging into Odoo.

![Reset password screen with "Log in with Google" button](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/google/first-login.png)

Existing users must [reset their password](#) to access the **Reset Password** page, while new users can directly click on **Log in with Google**, instead of choosing a new password.

> See also:
> - [Google Cloud Platform Console Help - Setting up OAuth 2.0](https://support.google.com/cloud/answer/6158849)

---

# Change languages

You select the language of your database upon its creation. However, you can [add](#add-languages) and [install](#change-languages-1) additional languages to allow users to manage the database in another language or to [translate](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/general/websites/website/configuration/translate.rst) your website.

## Add languages

To download additional languages:

- either click the profile icon in the upper-right corner, select **My profile**, and click the globe icon next to the **Language** field;
- or go to the **Settings** app, and click **Add Languages** in the **Languages** section.

You can then select the languages you want from the dropdown menu and click **Add**.

> See also: [Translations](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/general/websites/website/configuration/translate.rst)

## Change languages

To select their preferred language, users can click the profile icon in the upper-right corner, go to **My profile**, and select a **Language** in the dropdown list.

### Change another user's language

To change the database language for a user:

1. Go to the **Settings** app and click **Manage Users** in the **Users** section.
2. Click on the user whose language you want to change.
3. Go to the **Preferences** tab and select a previously installed language from the **Language** dropdown menu.

> Note: Emails and documents will be sent to the user in the selected language.

---

# LDAP authentication

To configure LDAP authentication in Odoo:

1. Open the Settings app, scroll down to the **Integrations** section, and enable **LDAP Authentication**.
2. Click **Save**, then go back to the **Integrations** section and click **LDAP Server**.
3. In the **Set up your LDAP Server** list, click **New**, then select the required company in the dropdown list.
4. In the **Server information** section, enter the server's IP address and port in the **LDAP server address** and **LDAP Server port** fields, respectively.
5. Enable **Use TLS** to request secure TLS/SSL encryption when connecting to the LDAP server, providing the server has StartTLS enabled.
6. In the **Login information** section, enter the ID and password of the account used to query the server in the **LDAP binddn** and **LDAP password** fields, respectively. If the fields are left empty, the server will perform the query anonymously.
7. In the **Process parameter** section, enter:
   - the LDAP server's name in the **LDAP base** field using LDAP format (e.g., `dc=example,dc=com`);
   - `uid=%s` in the **LDAP filter** field.
8. In the **User information** section:
   - Enable **Create user** to create a user profile in Odoo the first time someone logs in using LDAP;
   - Select the **User template** to be used to create the new user profiles. If no template is selected, the administrator's profile is used.

> Note: When using Microsoft Active Directory (AD) for LDAP authentication, if users experience login issues despite using valid credentials, create a new system parameter to disable referral chasing in the LDAP client:
>
> 1. Activate the developer mode.
> 2. Go to **Settings --> Technical --> System Parameters** and click **New**.
> 3. Fill in the fields:
>    - **Key**: `auth_ldap.disable_chase_ref`
>    - **Value**: `True`

---

# User portals

The user portal is a module available by default in Odoo. Users, both customers and vendors, can be granted [access to the portal](#) by businesses in order to view certain documents or information within an Odoo database. For example, some common use cases for providing portal access include allowing customers and vendors to:

- Follow, view, and pay orders
- Follow, download, or pay invoices, including partial payments and down payments
- Manage payment methods
- Manage subscriptions
- Add, remove, or modify their addresses
- Configure connection parameters between Odoo and 3rd-party services

> Note: Portal users only have read/view access, and will **not** be able to edit any documents in the database.

## Granting portal access

For the customers and vendors of a business to begin using a user portal, they must first be granted access by the business. The process to do this begins in the **Contacts** application.

### Adding portal access to a contact

From the main Odoo dashboard, open the **Contacts** application. If the contact to be granted access is not yet in the database, create an entry for them by clicking the **New** button and entering their details. Otherwise, choose an existing contact, then click on the **(Actions)** drop-down menu and select **Grant portal access**.

![Use the Contacts application to give portal access to users](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/user_portals/portal_access/grant-portal-access.png)

This brings up the **Portal Access Management** window, which has three fields:

- **Contact**: This field is automatically populated with the name on the contact.
- **Email**: This field shows the contact's email address used to log into the portal.
- **Latest Authentication**: The last time the user accessed the portal appears on this line. If the user has never accessed the portal, it is blank.

To grant portal access, first enter the email address the contact will use to log into the portal. This may have been automatically entered by the system if an existing contact is being granted access. Then click the **Grant Access** button to finish.

![Grant access in the Portal Access Management window](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/user_portals/portal_access/portal-access-management.png)

> Note: When portal access is granted to a Company, all contacts affiliated with that company are granted portal access. Individual contacts can be removed as needed.

### Granting access to multiple users at once

To grant portal access to multiple users from a single company at once, navigate to the contact listing for that company, then click **Action --> Grant portal access** to view a list of the company's related contacts. Click **Grant Access** for each contact that needs portal access. An email is sent to the specified email address, indicating that the contact is now a portal user for that Odoo database.

![Grant access to multiple users at once](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/user_portals/portal_access/multiple-user-access.png)

### Revoking portal access

Some situations may require revoking an individual's portal access, such as when they retire or leave a company. Portal access can be revoked from a contact at any time by navigating to a contact, clicking **Action --> Grant portal access**, and then clicking **Revoke Access**.

![Revoke access from a user](https://raw.githubusercontent.com/AbdullahMaresh9/documentation/19.0/content/applications/general/users/user_portals/portal_access/revoke-user-access.png)

## Updating portal info

There may be times when a user would like to make changes to their contact, security, or payment information connected to their portal account. Portal users can update most information by logging into their account and making the changes themselves, but some updates may require a user with administrator access rights.

Updating user information begins with logging into the user portal. Once a user has logged in and is on the portal dashboard, they can begin making changes to their information.

> Note: Passwords for portal users and Odoo.com users remain separate, even if the same email address is used.

### User-driven updates

#### Update contact info

From the portal dashboard, click the **Edit information** button under the user contact information. This brings up a new page where contact information can be updated.

On this screen, users can update their:

- **Your name**
- **Email**
- **Phone**
- Company name
- Mailing address
- Invoice delivery preference

Additional fields and descriptions may appear depending on the user's location and the location of the company that created the portal.

Once users have updated their information, they can click **Save Address** to save any changes they've made or **Discard** to return to the portal dashboard. By default, the only fields that need to be filled out to save users' information are their names, emails, phone numbers, and addresses.

> Important: Once the system has generated any documents for a user's account, their country **cannot** be updated. If users need to update their country at that point, they must reach out to a user with administrator access rights.

#### Update payment info

From the portal dashboard, click the **Payment methods** button. This brings up a new page that shows any saved payment methods.

To add a new payment option, choose from the available payment methods, enter the new payment information, and click **Save** to save the details. To remove a payment option, click the **(Delete payment method)** icon, then click the **Confirm Deletion** button.

> Important: Users that need to update payment information **must** delete any existing information and enter their new or updated info. Payment information **cannot** be edited and must be re-entered.

#### Update passwords and manage security

From the portal dashboard, click the **Connection & Security** button. This brings up a new page with five sections. The following sections can each be edited and updated.

##### Change Password

Here the user can enter their current password and a new password they wish to change to. The new password can be saved by clicking **Change Password**.

##### Two-factor authentication

The user's current 2FA status is shown as an alert. If 2FA is not enabled, an **Enable two-factor authentication** link appears. If it is enabled, a link reading **Disable two-factor authentication** appears instead.

> Tip: Clicking the **(Documentation)** icon opens a page that explains how two-factor authentication (2FA) works.

##### Passkeys

Click **Add Passkey** to create a passkey that simplifies the login process. Any previously created passkeys are shown here, alongside information on when they were created and when they were last used. When creating a passkey, Odoo asks for a password to confirm account ownership before allowing a passkey to be set. Once account ownership has been confirmed, the passkey must be named to differentiate it from other passkeys. Clicking **Create** brings up a browser prompt to create the passkey. Complete the passkey creation process in the browser. They can be renamed by clicking the **(pencil)** button or deleted by clicking their **(trash can)** button.

##### Revoke All Sessions

Clicking the **Log out from all devices** button brings up a pop-up prompting the user to enter their password. Once they do so and click **Confirm Password**, the system logs them out of all sessions except for the current one.

##### Delete Account

Clicking the **Delete Account** button brings up a pop-up to completely and irrevocably delete the user's account. Users must enter their password and their login name to complete the process. They can also choose to have their email address and phone number added to an internal block list to prevent any future communication.

### Administrator-driven updates

#### Accessing user information as an administrator

First, navigate to **Settings app --> Users** and click **Manage Users**. Then, click the **(Remove)** icon to clear the **Internal Users** search filter and use the **(Toggle Search Panel)** button to add **Portal Users** as a filter. After making this selection, search for and open the portal user that needs to be edited.

#### Update email address

The **(Email)** field shows the email address the user uses to log into the Odoo database. A user with administrator access rights can make any necessary changes by clicking into the field and updating the user's email address.

> Note: Users **cannot** change their own login usernames. Changing the **(Login)** only changes the *username* on the user's portal login. Users' email addresses can be changed in the **Manage Users** module, the **Contacts** app, or by the users themselves in the portal.

#### Update passwords

Users' security settings can be adjusted under the **Security** tab on their Contact form. Under the **Security** tab, there are options to:

- **Change password**: Click the **Change password** button to bring up a form that shows what logins the user has access to and enter a new password. Click **Change Password** to save the password or click **Cancel** to return to the previous screen.
- **Invite to use 2FA**: Click the **Invite to use 2FA** button to bring up a pop-up notification that an invitation to use two-factor authentication was sent.

> See also:
> - [Access rights](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/general/users/access_rights.rst)
> - [Portal access documentation](https://github.com/AbdullahMaresh9/documentation/blob/19.0/content/applications/general/users/user_portals/portal_access.rst)

---

## ملاحظات التوثيق

تم تجميع هذا الملف من مستودع الوثائق الرسمي لـ Odoo في فرع `19.0`.

إذا رغبت في ذلك، يمكنني أيضًا تجهيز نسخة ثانية من نفس الملف بصيغة:

- Markdown عربي كامل مع ترجمة مناسبة
- ملف README منظم بشكل احترافي
- نسخة مختصرة نسبيًا مع فهرس محتوى وملخصات
- ملف PDF/HTML قابل للطباعة

