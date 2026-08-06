---
name: akuity-manage-api-keys-and-roles
description: >-
  Issue, scope, rotate and revoke Akuity Platform API keys, and bind them to
  custom roles — the credential lifecycle behind every other Akuity API call.
api: Akuity Platform API — API Keys and Custom Roles
spec: openapi/akuity-apikey.json
base_url: https://akuity.cloud
operations:
  - OrganizationService_ListOrganizationAPIKeys
  - OrganizationService_CreateOrganizationAPIKey
  - OrganizationService_ListWorkspaceAPIKeys
  - OrganizationService_CreateWorkspaceAPIKey
  - APIKeyService_GetAPIKey
  - APIKeyService_RegenerateAPIKeySecret
  - APIKeyService_DeleteAPIKey
  - APIKeyService_GetWorkspaceAPIKey
  - APIKeyService_RegenerateWorkspaceAPIKeySecret
  - APIKeyService_DeleteWorkspaceAPIKey
  - CustomRoleService_ListCustomRoles
  - CustomRoleService_CreateCustomRole
  - CustomRoleService_GetCustomRole
  - CustomRoleService_UpdateCustomRole
  - CustomRoleService_DeleteCustomRole
  - OrganizationService_GetOrganizationPermissions
generated: '2026-08-06'
method: generated
source: >-
  openapi/akuity-apikey.json + openapi/akuity-customrole.json +
  https://docs.akuity.io/akuity-portal/organizations/api-keys
---

# Manage Akuity API keys and custom roles

Akuity has exactly one machine credential: an `AKUITY_API_KEY_ID` /
`AKUITY_API_KEY_SECRET` pair presented as HTTP Basic. There is no bearer token,
no OAuth client-credentials flow, and no per-request scope negotiation. Blast
radius is controlled by which role the key carries and, on paid plans, by scoping
the key to a workspace.

## Setup

Authenticate with an existing key that has sufficient role:
`curl -u $AKUITY_API_KEY_ID:$AKUITY_API_KEY_SECRET https://akuity.cloud/api/v1/organizations`

## Issue a key

1. `OrganizationService_ListOrganizationAPIKeys`
   → `GET /api/v1/organizations/{id}/apikeys` — see what already exists.

2. `OrganizationService_CreateOrganizationAPIKey`
   → `POST /api/v1/organizations/{id}/apikeys`
   Set a description, an expiry (`s`, `m`, `h`, `d`, `w`, or `0` for none) and a
   role (Owner or Member).

   **The secret is returned once.** There is no read-back operation that returns
   it. Capture it at creation or you must regenerate.

3. Workspace-scoped keys use `OrganizationService_CreateWorkspaceAPIKey`
   → `POST /api/v1/organizations/{organization_id}/workspaces/{workspace_id}/apikeys`.
   Scoped API keys are a Pro/Enterprise feature.

## Rotate a key

`APIKeyService_RegenerateAPIKeySecret`
→ `POST /api/v1/apikeys/{id}/regenerate`

This mints a new secret for the same key id. Deploy the new secret before the old
one stops working — there is no documented overlap window, so treat rotation as
a cutover, not a grace period. The workspace twin is
`APIKeyService_RegenerateWorkspaceAPIKeySecret`.

## Revoke a key

`APIKeyService_DeleteAPIKey` → `DELETE /api/v1/apikeys/{id}`
(workspace twin: `APIKeyService_DeleteWorkspaceAPIKey`).

Callers using the deleted key immediately receive
`{"code":16,"message":"unauthenticated"}` with HTTP 401.

## Narrow what a key can do

Owner/Member is coarse. Use custom roles for anything finer:

1. `CustomRoleService_ListCustomRoles` → `GET /api/v1/customroles`
2. `CustomRoleService_CreateCustomRole` → `POST /api/v1/customroles`
   Body: `{name, description, policy, organization_id}`. The `policy` field is a
   free-form policy document — model it on an existing role from
   `CustomRoleService_GetCustomRole` rather than authoring one blind.
3. `CustomRoleService_UpdateCustomRole` → `PATCH /api/v1/customroles/{id}`
4. `OrganizationService_GetOrganizationPermissions`
   → `GET /api/v1/organizations/{id}/permissions` — verify the effective
   permission set after a change.

Note: `/api/v1/customroles` returned `{"code":5,"message":"Not Found"}` on an
anonymous probe on 2026-08-06, which is consistent with the route requiring an
authenticated, entitled caller. Confirm routing against your own org before
building on it.

## Human login is a different flow

`akuity login` uses the OAuth 2.0 Device Authorization Grant
(`GET /api/v1/auth/device-code`, `POST /api/v1/auth/device-token`,
`POST /api/v1/auth/refresh-token`). Those endpoints exist in the published
descriptors but Akuity does not document them for third-party clients — do not
build an integration on them. Machine access is API keys.

## Hygiene

- Set an expiry on every key. `0` (no expiry) should be the exception.
- One key per integration, so revocation is surgical.
- Key creation, rotation and deletion all land in the audit trail
  (`OrganizationService_GetAuditLogs`) and can be alerted on via the
  `audit_event` webhook.
- Store secrets in a real secret manager. Akuity never redisplays them.
