---
subcategory: "Service Principals"
---

# Resource: azuread_service_principal

Manages a Service Principal associated with an Application within Azure Active Directory.

-> **NOTE:** If you're authenticating using a Service Principal then it must have permissions to both `Read and write all applications` and `Sign in and read user profile` within the `Windows Azure Active Directory` API. Please see The [Granting a Service Principal permission to manage AAD](../guides/service_principal_configuration.html) for the required steps.

## Example Usage

```hcl
resource "azuread_application" "example" {
  name                       = "example"
  homepage                   = "http://homepage"
  identifier_uris            = ["http://uri"]
  reply_urls                 = ["http://replyurl"]
  available_to_other_tenants = false
  oauth2_allow_implicit_flow = true
}

resource "azuread_service_principal" "example" {
  application_id               = azuread_application.example.application_id
  app_role_assignment_required = false

  tags = ["example", "tags", "here"]
}
```

## Argument Reference

The following arguments are supported:

* `app_role_assignment_required` - (Optional) Does this Service Principal require an AppRoleAssignment to a user or group before Azure AD will issue a user or access token to the application? Defaults to `false`.
* `application_id` - (Required) The ID of the Azure AD Application for which to create a Service Principal.
* `tags` - (Optional) A list of tags to apply to the Service Principal.

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `app_role_assignment_required` - Whether this Service Principal requires an AppRoleAssignment to a user or group before Azure AD will issue a user or access token to the application.
* `application_id` - The Application ID (appId) for the Service Principal.
* `display_name` - The Display Name of the Azure Active Directory Application associated with this Service Principal.
* `id` - The Object ID (internal ID) for the Service Principal.
* `oauth2_permissions` - A collection of OAuth 2.0 permissions exposed by the associated application. Each permission is covered by a `oauth2_permission` block as documented below.
* `object_id` - The Service Principal's Object ID.

---

`oauth2_permission` block exports the following:

* `admin_consent_description` - The description of the admin consent.
* `admin_consent_display_name` - The display name of the admin consent.
* `id` - The unique identifier for one of the `OAuth2Permission`.
* `is_enabled` - Is this permission enabled?
* `type` - The type of the permission.
* `user_consent_description` - The description of the user consent.
* `user_consent_display_name` - The display name of the user consent.
* `value` - The name of this permission.

## Import

Azure Active Directory Service Principals can be imported using the `object id`, e.g.

```shell
terraform import azuread_service_principal.test 00000000-0000-0000-0000-000000000000
```