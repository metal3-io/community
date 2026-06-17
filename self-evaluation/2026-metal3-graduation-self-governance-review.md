# Metal3.io Graduation Self-Governance Review

<!--
Template: https://raw.githubusercontent.com/cncf/toc/refs/heads/main/toc_subprojects/project-reviews-subproject/governance-review-template.md
-->

## Summary and Assessment

**Project:** Metal3.io (Metal³)
**Level:** Graduation
**Date:** April 2026

Metal3 is a CNCF project providing Kubernetes-native bare metal
host provisioning and lifecycle management. It integrates with the
Cluster API for declarative cluster management and uses OpenStack
Ironic as its default provisioning backend. The project entered
CNCF Sandbox in September 2020 and achieved Incubation in 2025.

**Status:** Self-Assessment (for Graduation application)

---

## Review

**The following review primarily consists of the project's
self-assessment for the Graduation application.**

### Governance Summary

Metal3 produces Kubernetes controllers (Baremetal Operator, Cluster
API Provider Metal3, IP Address Manager) and supporting container
images for declarative bare metal lifecycle management. The project
and its repositories are governed by their maintainers as documented
in the [github.com/metal3-io/community][community-repo] repository. The
[GOVERNANCE.md][governance] document links to all key governance artifacts
including Code of Conduct, Contribution Guide, Contributor Ladder,
Contributor Roles, DCO, License, Maintainers Guide, Roadmap, and
Adopters list.

### Governance Evolution

**Governance has continuously been iterated upon by the project as
a result of their experience applying it, with the governance
history demonstrating evolution of maturity alongside the project's
maturity evolution.**

**Incubating:** Suggested | **Graduated:** Suggested

Metal3 formalized its governance prior to incubation and has
iteratively evolved it since sandbox entry (September 2020) by
adding [vendor neutrality documentation][vendor-neutrality],
defining a five-level [contributor ladder][contributor-ladder],
establishing dedicated Security, CI, and Release teams with
documented onboarding processes ([CONTRIBUTOR-ROLE.md][roles]),
and consolidating per-repo maintainer lists into a
[centralized maintainers directory][maintainers-dir]. Governance
policy changes require 2/3 maintainer approval, ensuring
deliberate and consensus-driven evolution.

### Discoverability

**Clear and discoverable project governance documentation.**

**Incubating:** Suggested | **Graduated:** Required

All governance documentation is consolidated in the
[metal3-io/community][community-repo] repository and cross-linked
from the [GOVERNANCE.md][governance] document. The project website
([metal3.io](https://metal3.io)) and main repository README files
link back to the community repo.

### Accuracy and Clarity

**Governance is up to date with actual project activities,
including any meetings, elections, leadership, or approval
processes.**

**Incubating:** Suggested | **Graduated:** Required

Governance documents reflect current practices: community meetings
occur Wednesdays at 14:00 UTC on Zoom (documented in the
[README](https://github.com/metal3-io#contacts) and
[Google Doc][meeting-notes]), maintainer lists are kept current
in the [maintainers directory][maintainers-dir], and OWNERS files
across all repositories are actively updated. Maintainer elections
follow the nomination and 2/3 approval process documented in
[maintainers/README.md][maintainers-readme].

**Governance clearly documents
[vendor-neutrality](https://contribute.cncf.io/maintainers/community/vendor-neutrality/)
of project direction.**

**Incubating:** Suggested | **Graduated:** Required

[VENDOR_NEUTRALITY.md][vendor-neutrality] explicitly covers three
dimensions: hardware vendor neutrality (supporting Redfish/IPMI
without vendor lock-in), feature vendor neutrality (no dependency
on any commercial provider), and development vendor neutrality
(CI/CD runs on CNCF-provided infrastructure). The project's CI
infrastructure uses CNCF resources to avoid dependency on any
single organization.

### Decisions and Role Assignments

**Document how the project makes decisions on leadership roles,
contribution acceptance, requests to the CNCF, and changes to
governance or project goals.**

**Incubating:** Suggested | **Graduated:** Required

Changes to governance require 2/3 approval from existing
maintainers as documented in
[maintainers/README.md][maintainers-readme]. Contribution
acceptance follows the OWNERS-based `/lgtm` and `/approve`
workflow (Prow-compatible), with each repository defining its own
approvers and reviewers. Roadmap direction is discussed in weekly
community meetings and annual meetups, with the
[Roadmap editors team][roadmap] maintaining the
[GitHub Project board](https://github.com/orgs/metal3-io/projects/8).

**Document how role, function-based members, or sub-teams are
assigned, onboarded, and removed for specific teams (example:
Security Response Committee).**

**Incubating:** Suggested | **Graduated:** Required

All function-based teams are documented in
[CONTRIBUTOR-ROLE.md][roles] with explicit onboarding and
removal processes:

- **Security Team:** Led by Tuomo Tanskanen, reachable at
  <metal3-security@googlegroups.com>. Nomination by a security
  team member plus Org Admin approval, onboarding via shadowing.
- **CI Team:** Reachable at <metal3-ci@googlegroups.com>. Must
  be an Organization Member.
- **Release Team:** Reachable at
  <metal3-release@googlegroups.com>. Nomination by a community
  member, seconded by Org Admin, onboarding via shadowing an
  existing release team member.

Off-boarding from any role follows the process described in the
[Contributor Ladder][contributor-ladder]: after six months of
inactivity or detrimental behavior, maintainers can discuss with
the person and move them to emeritus status.

### Maintainers and Maintainer Lifecycle

**Document a complete maintainer lifecycle process (including
roles, onboarding, offboarding, and emeritus status).**

**Incubating:** Suggested | **Graduated:** Required

The maintainer lifecycle is fully documented in
[maintainers/README.md][maintainers-readme]: nomination is by
an existing maintainer via private email to all maintainers,
followed by discussion and a 2/3 approval vote. Offboarding
occurs after six months of inactivity or detrimental behavior,
with inactive contributors moved to `emeritus_approvers` or
`emeritus_reviewers` sections in OWNERS files.

**Demonstrate usage of the maintainer lifecycle with outcomes,
either through the addition or replacement of maintainers as
project events have required.**

**Incubating:** Suggested | **Graduated:** Required

The [history of the community repo][community-commits]
demonstrates active use of the lifecycle: maintainers have been
added as the project grew (e.g., kashifest, Rozzii), reviewers
have been promoted, and emeritus status has been applied (e.g.,
mboukhalfa moved to emeritus reviewers). The
[ALL-OWNERS][all-owners] file is regenerated from per-repo
OWNERS to keep the consolidated view up to date.

**Document complete list of current maintainers, including names,
contact information, domain of responsibility, and affiliation.**

**Incubating:** Required | **Graduated:** Required

The [ALL-OWNERS][all-owners] file lists each approver with name,
affiliation, GitHub handle, and email.

**A number of active maintainers which is appropriate to the size
and scope of the project.**

**Incubating:** Required | **Graduated:** Required

Metal3 has 17 approvers across all repositories (as listed in
[ALL-OWNERS][all-owners]), with 6 community-level maintainers
and 3 reviewers. This is appropriate for a project spanning
13+ repositories with 175+ individual contributors in the past
year per [LFX Insights][lfx].

**Project maintainers from at least 2 organizations that
demonstrates survivability.**

**Incubating:** N/A | **Graduated:** Required

Maintainers span two organizations: Ericsson Software Technology
and Red Hat. Both organizations have approvers in the core
repositories (BMO, CAPM3, IPAM, IrSO, Ironic-image), ensuring
no single vendor controls the project's direction.

### Ownership

**Code and Doc ownership in GitHub and elsewhere matches
documented governance roles.**

**Incubating:** Required | **Graduated:** Required

Each repository under the metal3-io organization maintains OWNERS
files that define approvers and reviewers, consistent with the
roles documented in the community governance. The community
repository's [maintainers directory][maintainers-dir] aggregates
per-repo OWNERS via the [all-owners.sh][all-owners-script]
script, ensuring alignment between documented governance roles
and actual GitHub permissions.

### Code of Conduct

**Document adoption and adherence to the CNCF Code of Conduct or
the project's CoC which is based off the CNCF CoC and not in
conflict with it.**

**Incubating:** Required | **Graduated:** Required

Metal3 adopts the CNCF Code of Conduct as documented in
[CODE_OF_CONDUCT.md][coc], which directly references and links to
the [CNCF Code of Conduct][cncf-coc].

**CNCF Code of Conduct is cross-linked from other governance
documents.**

**Incubating:** Required | **Graduated:** Required

The Code of Conduct is cross-linked from
[GOVERNANCE.md][governance], referenced in the
[Contributor Ladder][contributor-ladder]
(Community Contributor responsibilities).

### Repositories

**All subprojects, if any, are listed.**

**Incubating:** Required | **Graduated:** Required

Metal3 does not have formal subprojects. The project is organized
as a single governance unit with multiple repositories under the
[metal3-io](https://github.com/metal3-io) GitHub organization,
each serving as a distinct component:

| Repository | Purpose | Maturity | Leadership |
| :----------- | :-------- | :--------- | :----------- |
| [baremetal-operator](https://github.com/metal3-io/baremetal-operator) | Core controller managing BareMetalHost CRDs and Ironic interaction | Stable (v0.13.x) | 7 approvers (Red Hat, Ericsson) |
| [cluster-api-provider-metal3](https://github.com/metal3-io/cluster-api-provider-metal3) | CAPI infrastructure provider for bare metal | Stable (v1.13.x) | 5 approvers (Red Hat, Ericsson) |
| [ip-address-manager](https://github.com/metal3-io/ip-address-manager) | IP address allocation for bare metal hosts | Stable (v1.13.x) | 4 approvers (Red Hat, Ericsson) |
| [ironic-standalone-operator](https://github.com/metal3-io/ironic-standalone-operator) | Kubernetes operator for Ironic deployment | Beta (v0.x) | 3 approvers (Red Hat, Ericsson) |
| [ironic-image](https://github.com/metal3-io/ironic-image) | Container image for OpenStack Ironic | Stable | 4 approvers (Red Hat, Ericsson) |
| [ironic-ipa-downloader](https://github.com/metal3-io/ironic-ipa-downloader) | Ironic Python Agent image downloader | Stable | 3 approvers (Red Hat, Ericsson) |
| [mariadb-image](https://github.com/metal3-io/mariadb-image) | Container image for MariaDB (Ironic backend) | Stable | 2 approvers (Ericsson) |
| [metal3-dev-env](https://github.com/metal3-io/metal3-dev-env) | Development and testing environment | Stable | 5 approvers (Red Hat, Ericsson) |
| [metal3-docs](https://github.com/metal3-io/metal3-docs) | Design documents and proposals | N/A | 4 approvers (Red Hat, Ericsson) |
| [metal3-io.github.io](https://github.com/metal3-io/metal3-io.github.io) | Project website and blog | N/A | 3 approvers (Red Hat, Ericsson) |
| [project-infra](https://github.com/metal3-io/project-infra) | CI/CD infrastructure and jobs | Stable | 5 approvers (Red Hat, Ericsson) |
| [utility-images](https://github.com/metal3-io/utility-images) | Utility container images for CI | Stable | 2 approvers (Ericsson) |
| [community](https://github.com/metal3-io/community) | Governance, policies, maintainer info | N/A | 6 approvers (Red Hat, Ericsson) |

**If the project has subprojects: subproject leadership,
contribution, maturity status documented, including add/remove
process.**

**Incubating:** Suggested | **Graduated:** Required

Metal3 does not use a formal subproject model. All repositories
share a single governance structure, contributor ladder, and
maintainer body defined in the [community repo][community-repo].
Each repository maintains its own OWNERS file defining approvers
and reviewers. The community repository's
[maintainers directory][maintainers-dir] consolidates per-repo
ownership with name, affiliation, GitHub handle, and email.
Contribution processes are uniform across all repositories,
following the same OWNERS-based `/lgtm` and `/approve` workflow.
New repositories are added to the metal3-io GitHub organization
by maintainer consensus (2/3 approval), and the
[all-owners.sh][all-owners-script] script is updated to track
the new repo.

### Contributors and Community

**Contributor ladder with multiple roles for contributors.**

**Incubating:** Suggested | **Graduated:** Suggested

Metal3 defines a five-level contributor ladder in
[CONTRIBUTOR-LADDER.md][contributor-ladder]: Community
Contributor → Organization Member → Reviewer → Approver →
Maintainer, each with clear requirements, qualifications,
and privileges.

**Clearly defined and discoverable process to submit issues
or changes.**

**Incubating:** Required | **Graduated:** Required

Contributors can submit issues via GitHub Issues on any project
repository and propose changes through pull requests, with the
contribution workflow documented in
[CONTRIBUTING.md][contributing]. The project actively labels
issues with
[`good-first-issue`](https://github.com/search?q=org%3Ametal3-io+label%3A%22good+first+issue%22+is%3Aissue+is%3Aopen&type=issues)
and
[`help-wanted`](https://github.com/search?q=org%3Ametal3-io%20label%3A%22help%20wanted%22%20is%3Aissue%20is%3Aopen&type=issues)
to recruit new contributors.

**Project must have, and document, at least one public
communications channel for users and/or contributors.**

**Incubating:** Required | **Graduated:** Required

Metal3 maintains multiple public communication channels: the
[#cluster-api-baremetal][slack] channel on Kubernetes Slack,
the [metal3-dev mailing list][mailing-list], weekly community
meetings on Wednesdays at 14:00 UTC via Zoom, and GitHub
Issues/PRs across all repositories.

**List and document all project communication channels, including
subprojects (mail list/slack/etc.). List any non-public
communications channels and what their special purpose is.**

**Incubating:** Required | **Graduated:** Required

| Channel | Type | Purpose |
| :-------- | :----- | :-------- |
| [#cluster-api-baremetal][slack] (Kubernetes Slack) | Public | General discussion, user support |
| [metal3-dev@googlegroups.com][mailing-list] | Public | Announcements, discussions |
| [Weekly community meeting][meeting-notes] (Wednesdays 14:00 UTC, Zoom) | Public | Project planning, issue triage, feature discussion |
| Annual Metal3 community meetup ([notes][meetup-notes]) | Public | Long-term roadmap, in-person collaboration |
| GitHub Issues/PRs across all repos | Public | Bug reports, feature requests, code review |
| <metal3-security@googlegroups.com> | Private | Security vulnerability reports and coordinated disclosure |
| <metal3-ci@googlegroups.com> | Private | CI infrastructure issues |
| <metal3-release@googlegroups.com> | Private | Release coordination |

**Up-to-date public meeting schedulers and/or integration with
CNCF calendar.**

**Incubating:** Required | **Graduated:** Required

Weekly community meetings are documented in the
[project README](https://github.com/metal3-io#contacts) with
Zoom link, calendar invites, and notes in a
[Google Doc][meeting-notes]. Meetings are published on the
[CNCF community calendar](https://www.cncf.io/calendar/).
Meeting recordings are available on the
[Metal3 YouTube playlist][youtube].

**Documentation of how to contribute, with increasing detail as
the project matures.**

**Incubating:** Required | **Graduated:** Required

The project maintains a comprehensive
[CONTRIBUTING.md][contributing] covering DCO sign-off, finding
issues, contributing patches, testing, code review guidelines,
versioning and release semantics, branching strategy,
backporting, merge approval, and the release process. Each
repository also has a repo-specific CONTRIBUTING.md with
additional details. The contributing guide has grown
significantly since sandbox entry, recently adding sections
on AI agent guidelines, proposal process, and issue triaging.

**Demonstrate contributor activity and recruitment.**

**Incubating:** Required | **Graduated:** Required

Per [LFX Insights][lfx], the project had over 175 individual
contributors in the past year. The project actively recruits
through `good-first-issue` and `help-wanted` labels, KubeCon
presentations, and community meetups. Recent releases have
included contributions from new contributors across multiple
organizations.

<!-- Reference links -->

[community-repo]: https://github.com/metal3-io/community
[governance]: https://github.com/metal3-io/community/blob/main/GOVERNANCE.md
[vendor-neutrality]: https://github.com/metal3-io/community/blob/main/VENDOR_NEUTRALITY.md
[contributor-ladder]: https://github.com/metal3-io/community/blob/main/CONTRIBUTOR-LADDER.md
[roles]: https://github.com/metal3-io/community/blob/main/CONTRIBUTOR-ROLE.md
[maintainers-dir]: https://github.com/metal3-io/community/tree/main/maintainers
[maintainers-readme]: https://github.com/metal3-io/community/blob/main/maintainers/README.md
[all-owners]: https://github.com/metal3-io/community/blob/main/maintainers/ALL-OWNERS
[all-owners-script]: https://github.com/metal3-io/community/blob/main/maintainers/all-owners.sh
[community-commits]: https://github.com/metal3-io/community/commits/main/
[contributing]: https://github.com/metal3-io/community/blob/main/CONTRIBUTING.md
[coc]: https://github.com/metal3-io/community/blob/main/CODE_OF_CONDUCT.md
[cncf-coc]: https://github.com/cncf/foundation/blob/main/code-of-conduct.md
[roadmap]: https://github.com/metal3-io/community/blob/main/ROADMAP.md
[meeting-notes]: https://docs.google.com/document/d/1IkEIh-ffWY3DaNX3aFcAxGbttdEY_symo7WAGmzkWhU/edit
[meetup-notes]: https://docs.google.com/document/d/1Exm5k6JB2JlD9_FOaM68R2ASHi7ZsryF_xEqLG1j4zA
[slack]: https://kubernetes.slack.com/archives/CHD49TLE7
[mailing-list]: https://groups.google.com/g/metal3-dev
[lfx]: https://insights.linuxfoundation.org/project/metal3/contributors
[youtube]: https://www.youtube.com/playlist?list=PL2h5ikWC8viJY4SCSaG2RpuIL5J_0vGJJ
