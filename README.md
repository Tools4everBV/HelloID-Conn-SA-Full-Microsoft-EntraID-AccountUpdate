# HelloID-Conn-SA-Full-EntraID-AccountUpdate

| :information_source: Information |
| :--- |
| This repository contains connector and configuration code only. The implementer is responsible for obtaining the required connection details, certificates, and permissions. Contact the client's application manager to coordinate connector requirements. |

## Description

HelloID-Conn-SA-Full-EntraID-AccountUpdate is a delegated form designed for use with HelloID Service Automation (SA). It can be imported into HelloID and customized according to your requirements.

This delegated form updates selected user attributes in Microsoft Entra ID. The following options are available:

1. Search for and select an Entra ID user by display name, UserPrincipalName, or mail.
2. Edit the selected user's display name, department, job title, company name, given name, surname, business phone, or mobile phone.
3. Confirm the changes to update the user.

## Getting started

### Requirements

#### App Registration and certificate setup

Configure a Microsoft Entra ID App Registration for application (app-only) access to Microsoft Graph. Assign the required application permissions and configure a certificate for authentication.

For guidance, see Microsoft's documentation on [app-only authentication with a certificate](https://learn.microsoft.com/en-us/powershell/exchange/app-only-auth-powershell-v2?view=exchange-ps#set-up-app-only-authentication).

#### HelloID-specific configuration

Grant admin consent for these Microsoft Graph application permissions:

- `User.Read.All` to search and read user information.
- `User.ReadWrite.All` to update the supported user attributes.

The certificate must be provided as a Base64-encoded string. See [Setting up a certificate for Microsoft Graph API in HelloID connectors](https://forum.helloid.com/forum/helloid-provisioning/5338-instruction-setting-up-a-certificate-for-microsoft-graph-api-in-helloid-connectors#post5338) for instructions.

### Connection settings

Configure these HelloID global variables when importing and setting up the delegated form:

| Setting                          | Description                        | Mandatory |
| -------------------------------- | ---------------------------------- | --------- |
| `EntraIdTenantId`                | The Microsoft Entra ID tenant ID   | Yes       |
| `EntraIdAppId`                   | The App Registration client ID     | Yes       |
| `EntraIdCertificateBase64String` | The Base64-encoded app certificate | Yes       |
| `EntraIdCertificatePassword`     | The certificate password           | Yes       |

### Setup options

Use [All-in-one setup/createform.ps1](All-in-one%20setup/createform.ps1) to create the delegated form, data source, task, and required global variables through the HelloID API. The script assumes these resources do not already exist; it does not provide resource versioning or source control.

Alternatively, import and configure the resources in [Manual resources](Manual%20resources/).

Follow the [HelloID Service Automation GitHub resources documentation](https://docs.helloid.com/hc/en-us/articles/360017556559-Service-automation-GitHub-resources) to run the setup script.

## Remarks

### User lookup and updates

- The data source searches display name, UserPrincipalName, and mail. Enter `*` to return all users, including enabled and disabled accounts.
- The form uses the Entra ID user `id` to update the selected account, so changes to properties such as UserPrincipalName do not affect correlation.
- Only these attributes are updated: `displayName`, `department`, `jobTitle`, `companyName`, `givenName`, `surname`, `businessPhones`, and `mobilePhone`.
- The task sends only changed attributes to Microsoft Graph.

### Certificate-based authentication

The connector uses the configured certificate to generate a JSON Web Token (JWT) for app-only authentication with Microsoft Graph.

## Development resources

### API endpoints

| Endpoint           | Description                                     |
| ------------------ | ----------------------------------------------- |
| `/v1.0/users`      | Search and retrieve users                       |
| `/v1.0/users/{id}` | Update the selected user's supported attributes |

### API documentation

- [List users](https://learn.microsoft.com/en-us/graph/api/user-list)
- [Update user](https://learn.microsoft.com/en-us/graph/api/user-update)

## Getting help

For more information on delegated forms, see the [HelloID documentation](https://docs.helloid.com/en/service-automation/delegated-forms.html). You can also ask questions in the [HelloID forum](https://forum.helloid.com/forum/helloid-connectors/service-automation/216-helloid-sa-azure-ad-update-user).

## HelloID docs

The official HelloID documentation can be found at [docs.helloid.com](https://docs.helloid.com/).