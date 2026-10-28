# Akuity

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Akuity is the enterprise software delivery company founded by the creators of Argo CD and Kargo. The Akuity Platform is its commercial, fully-managed offering: hosted, enterprise-grade Argo CD control planes for GitOps continuous delivery, managed Kargo for multi-stage progressive promotion, the Akuity Agent for connecting target Kubernetes clusters, and Akuity Intelligence — an AI layer adding multi-cluster insight dashboards, on-call and promotion advisor agents, and AI-assisted remediation. The platform is controlled by a REST API at https://akuity.cloud/api/v1/, an `akuity` CLI, a Terraform provider and a Crossplane provider, all of which speak the same grpc-gateway service surface. Akuity runs on AWS with US and EU data residency and maintains SOC 2 Type II, ISO 27001:2022, PCI DSS 4.0.1, HIPAA-aligned and CSA STAR Level 1 posture.

**Areas:** GitOps · Continuous Delivery · Kubernetes · ArgoCD · Kargo · Platform Engineering · DevOps · Progressive Delivery · Cloud-Native · AIOps · Developer Tools

## Akuity on APIs.io

Everything profiled for Akuity is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/akuity/](https://apis.io/providers/akuity/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [Akuity Argo CD Service API](https://apis.io/apis/akuity/akuity-argocdservice-api/) — The ArgoCDService API from Akuity — 111 operation(s) for argocdservice.
- [Akuity Custom Role Service API](https://apis.io/apis/akuity/akuity-customroleservice-api/) — The CustomRoleService API from Akuity — 2 operation(s) for customroleservice.
- [Akuity Extension Service API](https://apis.io/apis/akuity/akuity-extensionservice-api/) — The ExtensionService API from Akuity — 6 operation(s) for extensionservice.
- [Akuity Kargo Service API](https://apis.io/apis/akuity/akuity-kargoservice-api/) — The KargoService API from Akuity — 39 operation(s) for kargoservice.
- [Akuity Organization Service API](https://apis.io/apis/akuity/akuity-organizationservice-api/) — The OrganizationService API from Akuity — 153 operation(s) for organizationservice.
- [Akuity System Service API](https://apis.io/apis/akuity/akuity-systemservice-api/) — The SystemService API from Akuity — 14 operation(s) for systemservice.
- [Akuity Api Key Service API](https://apis.io/apis/akuity/akuity-api-key-service-api/) — The Api Key Service API from Akuity — 4 operation(s) for api key service.
- [Akuity Auth Service API](https://apis.io/apis/akuity/akuity-auth-service-api/) — The Auth Service API from Akuity — 4 operation(s) for auth service.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Akuity Notifications Webhooks](https://apis.io/asyncapis/akuity/akuity-notifications-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [akuity-mcp.yml](https://apis.io/servers/akuity/akuity-mcp-yml/)

#### Collections (8)

Postman and Open Collection request collections.

- [Akuity Platform API — API Keys API Key Service API](https://apis.io/collections/akuity/open-akuity-apikeyservice-api/)
- [Akuity Platform API — Argo CD Argo CD Service API](https://apis.io/collections/akuity/open-akuity-argocdservice-api/)
- [Akuity Platform API — Auth Auth Service API](https://apis.io/collections/akuity/open-akuity-authservice-api/)
- [Akuity Platform API — Custom Roles Custom Role Service API](https://apis.io/collections/akuity/open-akuity-customroleservice-api/)
- [Akuity Platform API — Extension Extension Service API](https://apis.io/collections/akuity/open-akuity-extensionservice-api/)
- [Akuity Platform API — Kargo Kargo Service API](https://apis.io/collections/akuity/open-akuity-kargoservice-api/)
- [Akuity Platform API — Organization Organization Service API](https://apis.io/collections/akuity/open-akuity-organizationservice-api/)
- [Akuity Platform API — System System Service API](https://apis.io/collections/akuity/open-akuity-systemservice-api/)

#### Overlays (9)

OpenAPI overlays, including conversational phrasing for each operation.

- [Akuity Api Key Service Api Phrasing Overlay](https://apis.io/overlays/akuity/akuity-api-key-service-api-phrasing-overlay/)
- [Akuity Argocdservice Api Phrasing Overlay](https://apis.io/overlays/akuity/akuity-argocdservice-api-phrasing-overlay/)
- [Akuity Auth Service Api Phrasing Overlay](https://apis.io/overlays/akuity/akuity-auth-service-api-phrasing-overlay/)
- [Akuity Customroleservice Api Phrasing Overlay](https://apis.io/overlays/akuity/akuity-customroleservice-api-phrasing-overlay/)
- [Akuity Extensionservice Api Phrasing Overlay](https://apis.io/overlays/akuity/akuity-extensionservice-api-phrasing-overlay/)
- [Akuity Kargoservice Api Phrasing Overlay](https://apis.io/overlays/akuity/akuity-kargoservice-api-phrasing-overlay/)
- [Akuity Organizationservice Api Phrasing Overlay](https://apis.io/overlays/akuity/akuity-organizationservice-api-phrasing-overlay/)
- [Akuity Platform Overlay](https://apis.io/overlays/akuity/akuity-platform-overlay/)
- [Akuity Systemservice Api Phrasing Overlay](https://apis.io/overlays/akuity/akuity-systemservice-api-phrasing-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Akuity Authentication](https://apis.io/security/akuity/akuity-authentication/)
- [Akuity Domain Security](https://apis.io/security/akuity/akuity-domain-security/)
- [Akuity Trust Center](https://apis.io/security/akuity/akuity-trust-center/)
- [Akuity Vulnerability Disclosure](https://apis.io/security/akuity/akuity-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Akuity Plans](https://apis.io/plans/akuity/akuity-plans/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://akuity.io)
- [Documentation](https://docs.akuity.io)
- [Getting started](https://docs.akuity.io/argocd/getting-started/)
- [API reference](https://docs.akuity.io/akuity-portal/reference/api)
- [Pricing](https://akuity.io/pricing)
- [Sign up](https://akuity.cloud)
- [Support](https://akuity.io/connect-with-akuity)
- [Blog](https://akuity.io/blog)
- [Compliance](https://akuity.io/security-compliance)
- [Terms of service](https://akuity.io/terms-of-service)
- [Privacy policy](https://akuity.io/privacy-policy)
- [GitHub](https://github.com/akuity)
- [Status](https://status.akuity.io/)

## API Evangelist rating

**Kin Score 73.0** — exemplar · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 58.3 |
| Contract governance | 18.2 |
| Operational transparency | 52.6 |
| Developer ergonomics | 73.2 |
| Access clarity | 92.1 |

**Agent readiness 26.6** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | — |

**Regulatory alignment 35.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/akuity/](https://apis.io/providers/akuity/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Akuity: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/akuity/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
