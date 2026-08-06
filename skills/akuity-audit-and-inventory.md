---
name: akuity-audit-and-inventory
description: >-
  Pull an organization's audit trail and its cross-cluster Kubernetes inventory
  out of the Akuity Platform — who changed what, and what is actually running —
  for compliance evidence, drift review or an incident timeline.
api: Akuity Platform API — Organizations
spec: openapi/akuity-organization.json
base_url: https://akuity.cloud
operations:
  - OrganizationService_ListAuthenticatedUserOrganizations
  - OrganizationService_GetAuditLogs
  - OrganizationService_ListAuditLogsArchives
  - OrganizationService_GetAuditLogsInCSV
  - OrganizationService_ListKubernetesEnabledClusters
  - OrganizationService_ListKubernetesResources
  - OrganizationService_ListKubernetesImages
  - OrganizationService_ListKubernetesDeprecatedAPIs
  - OrganizationService_GetKubernetesSummary
  - OrganizationService_GetKubernetesResourceDetail
  - OrganizationService_ListKubernetesTimelineEvents
generated: '2026-08-06'
method: generated
source: openapi/akuity-organization.json + https://docs.akuity.io/intelligence/insights-dashboards/
---

# Audit trail and cluster inventory

Two read-only surfaces that answer different questions:

- **Audit logs** — what a human or key *did* in the organization.
- **Kubernetes inventory (KubeVision)** — what is *running* across every
  connected cluster.

Both are read-only. Nothing in this skill mutates delivery infrastructure.

## Setup

Basic auth with `AKUITY_API_KEY_ID` / `AKUITY_API_KEY_SECRET`. Resolve the
organization id first with
`OrganizationService_ListAuthenticatedUserOrganizations`
(`GET /api/v1/organizations`).

## Audit trail

1. `OrganizationService_GetAuditLogs`
   → `GET /api/v1/organizations/{id}/audit-logs`
   Paginate with the `offset` and `limit` query parameters. Retention is
   6 months searchable on Pro, 1 year on Enterprise.

2. `OrganizationService_ListAuditLogsArchives`
   → `GET /api/v1/organizations/{id}/audit-logs-archives`
   Older records live in archives (1 year Pro / 2 years Enterprise).

3. `OrganizationService_GetAuditLogsInCSV`
   → `GET /api/v1/organizations/{id}/csv-audit-logs`
   **Server-streaming.** The gateway returns newline-delimited JSON envelopes
   wrapping `google.api.HttpBody` chunks, not one document — consume it as a
   stream, do not buffer-and-parse-once.

Each audit record carries an `actor` (type, id, ip), an `object`
(type — `team_member`, `team`, `custom_role`, `kargo_instance`, … — with a
name/kind/group id and a parent id) and `details` (message, patch, action_type).
The same shape arrives live as the `audit_event` webhook; see
`asyncapi/akuity-notifications-webhooks.yml`.

## Cluster inventory

1. `OrganizationService_ListKubernetesEnabledClusters`
   → `GET /api/v1/orgs/{organization_id}/k8s/clusters`
   Which clusters are reporting inventory at all.

2. `OrganizationService_GetKubernetesSummary`
   → `GET /api/v1/orgs/{organization_id}/k8s/summary`
   The rollup behind the overview dashboard.

3. `OrganizationService_ListKubernetesResources`
   → `GET /api/v1/orgs/{organization_id}/k8s/resources`
   Every tracked resource across every cluster. `offset`/`limit` apply.

4. `OrganizationService_ListKubernetesImages`
   → `GET /api/v1/orgs/{organization_id}/k8s/images`
   Running container images — the input to image/CVE review.

5. `OrganizationService_ListKubernetesDeprecatedAPIs`
   → `GET /api/v1/orgs/{organization_id}/k8s/deprecated-apis`
   Kubernetes API versions you are still using that upstream has deprecated.
   This is the highest-value call in the set for upgrade planning.

6. `OrganizationService_GetKubernetesResourceDetail`
   → `GET /api/v1/orgs/{organization_id}/k8s/resources/{resource_id}/detail`
   and `OrganizationService_ListKubernetesTimelineEvents`
   → `GET /api/v1/orgs/{organization_id}/k8s/timeline-events`
   for a per-resource incident timeline.

CSV variants of the resource, image, container and deprecated-API listings exist
under `/api/v1/stream/...` and are all server-streaming.

## Cautions

- The `/ext-api/v1/argocd/extensions/kubevision/...` twins of these operations
  serve the same data to the in-Argo-CD UI extension. Use the `/api/v1/` form
  from a backend integration.
- Inventory is a **read model**. There are no create/update/delete operations on
  `k8s/*` — do not expect to remediate through this surface. Remediation goes
  through Git and Argo CD.
- Audit records are the compliance artifact behind Akuity's SOC 2 / ISO 27001 /
  PCI DSS posture (`conformance/akuity-conformance.yml`). Treat exported CSVs as
  containing customer identifiers.
