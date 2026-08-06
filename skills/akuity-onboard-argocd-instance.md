---
name: akuity-onboard-argocd-instance
description: >-
  Stand up a managed Argo CD control plane on the Akuity Platform and attach a
  target Kubernetes cluster to it, using the Akuity Platform REST API.
api: Akuity Platform API — Argo CD
spec: openapi/akuity-argocd.json
base_url: https://akuity.cloud
operations:
  - OrganizationService_ListAuthenticatedUserOrganizations
  - SystemService_ListArgoCDVersions
  - ArgoCDService_CreateInstance
  - ArgoCDService_GetInstance
  - ArgoCDService_ListInstances
  - ArgoCDService_CreateInstanceCluster
  - ArgoCDService_GetInstanceClusterManifests
  - ArgoCDService_GetInstanceClusterInfo
generated: '2026-08-06'
method: generated
source: openapi/akuity-argocd.json + https://docs.akuity.io/argocd/getting-started/
---

# Onboard an Argo CD instance on Akuity

Creates a managed Argo CD control plane, then connects a Kubernetes cluster to it
through the Akuity Agent. This is the platform's core flow — everything else
(Kargo, Intelligence, addons) hangs off an instance with at least one cluster.

## Before you start

- You need an API key pair from the Akuity portal: `AKUITY_API_KEY_ID` and
  `AKUITY_API_KEY_SECRET` (Organization → API Keys → New Key). Assign the key a
  role of **Owner** if it must create instances; a **Member** key will get
  `PERMISSION_DENIED` (code 7).
- Every call authenticates with HTTP Basic:
  `curl -u $AKUITY_API_KEY_ID:$AKUITY_API_KEY_SECRET ...`
- There is **no idempotency key**. A repeated `ArgoCDService_CreateInstance` with
  the same name returns `ALREADY_EXISTS` (code 6), not the existing instance —
  check with `ArgoCDService_ListInstances` before you create.

## Steps

1. **Resolve the organization.**
   `OrganizationService_ListAuthenticatedUserOrganizations`
   → `GET /api/v1/organizations`
   Take the `id` of the organization the key belongs to. Every subsequent path
   needs it as `{organization_id}`.

2. **Pick an Argo CD version.**
   `SystemService_ListArgoCDVersions` → `GET /api/v1/system/cd/versions`
   This endpoint is anonymous — you can call it before you have a key. Choose a
   version the platform currently offers rather than hard-coding one.

3. **Check the instance does not already exist.**
   `ArgoCDService_ListInstances`
   → `GET /api/v1/orgs/{organization_id}/argocd/instances`

4. **Create the instance.**
   `ArgoCDService_CreateInstance`
   → `POST /api/v1/orgs/{organization_id}/argocd/instances`
   The request body is the instance spec. If the organization is at its plan
   quota you get `RESOURCE_EXHAUSTED` (code 8) — a Pro plan includes one Argo CD
   control plane.

5. **Poll until the instance is healthy.**
   `ArgoCDService_GetInstance`
   → `GET /api/v1/orgs/{organization_id}/argocd/instances/{id}`
   Provisioning is asynchronous. Back off between polls; do not tight-loop.

6. **Register the target cluster.**
   `ArgoCDService_CreateInstanceCluster`
   → `POST /api/v1/orgs/{organization_id}/argocd/instances/{instance_id}/clusters`

7. **Fetch the agent manifests and apply them in the cluster.**
   `ArgoCDService_GetInstanceClusterManifests`
   → `GET /api/v1/orgs/{organization_id}/argocd/instances/{instance_id}/clusters/{id}/manifests`
   Apply the returned YAML with `kubectl apply -f -`. This is what makes the
   cluster call home to the Akuity control plane.

8. **Confirm the agent connected.**
   `ArgoCDService_GetInstanceClusterInfo`
   → `GET /api/v1/orgs/{organization_id}/argocd/instances/{instance_id}/clusters/{id}/info`
   The same transition also fires an `agent_health_event` webhook with
   `status: "connected"` if a notification config is registered.

## Workspace-scoped variants

If the organization uses workspaces, every operation above has a `_1` twin with
`/workspaces/{workspace_id}/` inserted after `/orgs/{organization_id}` — e.g.
`ArgoCDService_CreateInstance_1`. Pick one form and stay in it; the two address
different tenancy scopes.

## Errors to handle

| code | HTTP | meaning | what to do |
|---|---|---|---|
| 16 | 401 | UNAUTHENTICATED | Check the Basic header; a trailing newline in the base64 is the usual cause. |
| 7 | 403 | PERMISSION_DENIED | The key's role or custom-role policy does not allow the action. |
| 6 | 409 | ALREADY_EXISTS | An instance or cluster with that name exists. |
| 8 | 429 | RESOURCE_EXHAUSTED | Plan quota reached. See https://akuity.io/pricing |
| 9 | 400 | FAILED_PRECONDITION | e.g. deleting an instance that still has clusters attached. |

Errors are `google.rpc.Status`, not RFC 9457 — branch on `code`, never on
`message`. See `errors/akuity-error-codes.yml`.

## Prefer the declarative path for anything repeatable

For a flow you will run more than once, use
`ArgoCDService_ExportInstance` → edit → `ArgoCDService_ApplyInstance`
(or `akuity argocd export` / `apply` in the CLI). Because the API has no
idempotency keys, desired-state re-application *is* the idempotence mechanism.
See https://docs.akuity.io/akuity-portal/automation/declarative-management
