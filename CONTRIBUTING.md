# How to Contribute to Metal3

Metal3 projects are [Apache 2.0 licensed](LICENSE) and accept contributions via
GitHub pull requests.

This guide provides common contributing guidelines for all Metal3 projects.
Individual repositories may have additional specific requirements documented
in their own CONTRIBUTING.md files.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Certificate of Origin](#certificate-of-origin)
   - [Git commit Sign-off](#git-commit-sign-off)
- [Finding Things That Need Help](#finding-things-that-need-help)
- [Contributing a Patch](#contributing-a-patch)
- [Testing](#testing)
- [Code Review Guidelines](#code-review-guidelines)
- [Versioning and Release Semantics](#versioning-and-release-semantics)
   - [What Goes in a Release](#what-goes-in-a-release)
- [Branches](#branches)
   - [Branch Structure](#branch-structure)
   - [Release Branch Support](#release-branch-support)
- [Breaking Changes](#breaking-changes)
   - [Examples of Breaking Changes](#examples-of-breaking-changes)
   - [Exceptions](#exceptions)
- [Backporting](#backporting)
   - [Backport Criteria](#backport-criteria)
   - [Backport Process](#backport-process)
   - [Maintained Versions](#maintained-versions)
- [Merge Approval](#merge-approval)
- [Issue and Pull Request Management](#issue-and-pull-request-management)
- [Commands and Workflow](#commands-and-workflow)
- [Release Process](#release-process)
   - [Repo-Specific Release Steps](#repo-specific-release-steps)
- [Documentation Changes](#documentation-changes)
- [Proposal Process](#proposal-process)
- [Triaging Issues](#triaging-issues)
- [AI Agent Guidelines](#ai-agent-guidelines)
- [Google Doc Access](#google-doc-access)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Certificate of Origin

By contributing to this project you agree to the Developer Certificate of
Origin (DCO). This document was created by the Linux Kernel community and is a
simple statement that you, as a contributor, have the legal right to make the
contribution. See the [DCO](DCO) file for details.

### Git commit Sign-off

Commit message should contain signed off section with full name and email. For example:

```text
Signed-off-by: John Doe <jdoe@example.com>
```

When making commits, include the `-s` flag and `Signed-off-by` section will be
automatically added to your commit message. If you want GPG signing too, add
the `-S` flag alongside `-s`.

```bash
# Signing off commit
git commit -s

# Signing off commit and also additional signing with GPG
git commit -s -S
```

## Finding Things That Need Help

If you're new to the project and want to help, but don't know where to start,
look for issues labeled with `good first issue` and `help wanted` in the Metal3
repositories. Here's a consolidated link of good first issues and help wanted
across the Metal3.io org.

- [Good first issues](https://github.com/search?q=org%3Ametal3-io+label%3A%22good+first+issue%22+is%3Aissue+is%3Aopen&type=issues)
- [Help wanted](https://github.com/search?q=org%3Ametal3-io%20label%3A%22help%20wanted%22%20is%3Aissue%20is%3Aopen&type=issues)

Alternatively, read the documentation, try to write your own code or
examples, and file issues for any problems or gaps in documentation that you
encounter!

## Contributing a Patch

1. If working on an issue, signal other contributors that you are actively
   working on it by assigning it to yourself.
1. Fork the desired repo, develop and test your code changes.
1. Submit a pull request.
    1. All code PR must be labeled with one of
        - ⚠️ (`:warning:`, major or breaking changes)
        - ✨ (`:sparkles:`, feature additions)
        - 🐛 (`:bug:`, patch and bugfixes)
        - 📖 (`:book:`, documentation or proposals)
        - 🌱 (`:seedling:`, minor or other)
1. If your PR has multiple commits related to the same change, you should
   [squash them into a single commit](https://kubernetes.io/docs/contribute/new-content/open-a-pr/#squashing-commits)
   before merging your PR. When in doubt ask the reviewers to help you determine
   the commit structure.

Individual commits should not be tagged separately with emojis, but will
generally be assumed to match the PR. The emoji label should match the most
significant change in the PR. Test updates, refactoring, and other minor
changes should use 🌱 (`:seedling:`).

For instance, if you have a bugfix along with a breaking change, it's generally
encouraged to submit the bugfix separately, but if you must put them in one PR,
the PR should be labeled with the most significant change (⚠️ in this case).

All changes must be code reviewed. Coding conventions and standards are
explained in the official [developer docs](https://git.k8s.io/community/contributors/devel).

## Testing

In Metal3.io, automated tests are managed by 3 CI (Continuous Integration)
orchestration service:

- [Self hosted Kubernetes Prow](https://prow.apps.test.metal3.io/)
- [Jenkins](jenkins.nordix.org)
- GitHub actions per repository.

Prow tests and Github action tests usually run automatically and jenkins
tests are triggered manually. You can read more on the [Metal3.io prow configuration](https://github.com/metal3-io/project-infra/blob/main/prow/README.md)
and [Metal3.io jenkins setup](https://github.com/metal3-io/project-infra/blob/main/jenkins/README.md).

All tests are expected to be pass before merging. There are quite a few handy
scripts and tools (linters, formatters etc) under the `/hack` folder in each
repo. Please make use of those to run the tests locally before creating a PR
upstream.

## Code Review Guidelines

All changes must be code reviewed. Coding conventions and standards are
explained in the official [Kubernetes developer
docs](https://github.com/kubernetes/community/tree/master/contributors/devel).

Expect reviewers to request that you avoid common [Go style
mistakes](https://github.com/golang/go/wiki/CodeReviewComments) in your PRs.

You will find more information on the review process in [Merge Approval](#merge-approval),
[Issue and Pull Request Management](#issue-and-pull-request-management),
and [Commands and Workflow](#commands-and-workflow) sections.

## Versioning and Release Semantics

### What Goes in a Release

Metal3 projects generally follow these versioning semantics:

- A (**minor**) release CAN include:
   - Introduction of new API versions, or new Kinds
   - Compatible API changes like field additions, deprecation notices, etc.
   - Breaking API changes for deprecated APIs, fields, or code
   - Features, promotion or removal of feature gates
   - And more!

A (**patch**) release SHOULD only include backwards compatible set of:

- bugfixes
- ci/testing fixes
- version bumps

## Branches

### Branch Structure

Metal3 repositories follow a common branch structure:

- The **main** branch is where development happens. All the latest and greatest
  code, including breaking changes, happens on main.

- The **release-X.Y** branches contain stable, backwards compatible code. On every
  major or minor release, a new branch is created. It is from these branches that
  minor and patch releases are tagged. Branches are created usually when we do
  a release candidate (RC) release for a minor release.

- In some cases, it may be necessary to open PRs for bugfixes directly against
  stable branches, but this should generally not be the case.

### Release Branch Support

Each project maintains specific support matrices for their release branches.
Check [version support](https://book.metal3.io/version_support.html) for :

- Maintained versions and EOL dates
- API version support policies
- Test coverage policies (if any)

## Breaking Changes

Breaking changes are generally allowed in the `main` branch, as this is the
branch used to develop the next minor release. We encourage to merge such changes
in the beginning of a minor release cycle if possible so that the changes are
well tested.

There may be times, however, when `main` is closed for breaking changes. This
is likely to happen as we near the release of a new minor version.

Breaking changes are **not allowed** in release branches, as these represent
minor versions that have already been released. These versions have consumers
who expect the APIs, operations logic, etc. to remain stable during the lifetime
of the patch stream for the minor release.

### Examples of Breaking Changes

- Removing or renaming a field in a CRD
- Removing or renaming a CRD
- Removing or renaming an exported constant, variable, type, or function
- Updating the version of critical libraries such as controller-runtime,
  client-go, apimachinery, etc.
   - Patch versions of these libraries are generally not considered breaking changes
   - Some version updates may be acceptable for picking up bug fixes, but
    maintainers must exercise caution when reviewing

### Exceptions

There may be times when breaking changes are allowed in release branches.
These are at the discretion of the project's maintainers and must be carefully
considered before merging. An example of an acceptable breaking change might be
a fix for a behavioral/functional bug that was released in an initial minor
version. Vulnerability fixes may also be done in exceptional circumstances.

## Backporting

We generally do **not** accept PRs directly against release branches. However,
we do accept backports of fixes/changes already merged into the main branch.

### Backport Criteria

We generally allow backports of the following kinds of changes to all
maintained branches:

- Critical bug fixes, security issue fixes, or fixes for bugs without easy workarounds
- Dependency bumps
- Changes required to support new Kubernetes patch versions, when possible
- Changes to use the latest Go patch version to build controller images
- Changes to bump the Go minor version used to build controller images, if the
  Go minor version of a supported branch goes out of support (e.g., to pick up
  bug and CVE fixes)
- Improvements to existing documentation
- Improvements to the test framework and CI

### Backport Process

Like any other activity in the project, backporting a fix/change is a
community-driven effort and requires that someone volunteers to own the task.

In most cases, the cherry-pick bot can (and should) be used to automate
opening a cherry-pick PR. To use it, add a comment on the PR you want to
backport:

```text
/cherry-pick release-X.Y
```

### Maintained Versions

We generally do not accept backports to release branches that are not maintained
(including tested only branches).
Check the [Version Support](https://github.com/metal3-io/metal3-docs/blob/main/docs/user-guide/src/version_support.md)
guide for each project's specific support policy.

## Merge Approval

Please see the [Kubernetes community document on pull
requests](https://git.k8s.io/community/contributors/guide/pull-requests.md) for
more information about the merge process.

Metal3 follows the standard Kubernetes workflow: any PR needs `lgtm` and
`approved` labels, and PRs must pass all tests before being merged. Tests may
be overwritten by maintainers in case the tests are not relevant for a
particular PR or fail for unrelated CI issues.

## Issue and Pull Request Management

Anyone is welcome to comment on issues and pull requests. Assignment to issues
or pull requests, as well as submission of official pull request reviews, is
limited to members of the [Metal3-io organization](https://github.com/metal3-io)
on GitHub.

Metal3 maintainers can assign you an issue or pull request by leaving a
`/assign <your Github ID>` comment on the issue or pull request.

## Commands and Workflow

Metal3 projects follow the standard Kubernetes workflow and use Prow for CI/CD.

We use the same priority and kind labels as Kubernetes. See the labels tab in
GitHub for the full list in each repository.

The standard Kubernetes comment commands work in Metal3 projects. See
[Prow command help](https://prow.apps.test.metal3.io/command-help) for a
command reference.

Common commands include:

- `/lgtm` - Adds the `lgtm` label (Looks Good To Me)
- `/lgtm cancel` - Removes the `lgtm` label
- `/approve` - Adds the `approve` label
- `/approve cancel` - Removes the `approve` label
- `/hold` - Adds the `do-not-merge/hold` label
- `/hold cancel` or `/unhold` - Removes the hold
- `/retest` - Reruns failed tests
- `/assign @username` - Assigns an issue or PR
- `/cc @username` - Requests a review
- `/test ?` - Lists all the test that one can run on a PR
- `/ok-to-test`- to let tests run for external contributors

## Release Process

[Releasing in Metal3](https://github.com/metal3-io/metal3-docs/blob/main/processes/releasing.md)
contains the high-level release process for Metal3.io org and possible follow-up
actions.

### Repo-Specific Release Steps

Each repository has specific release procedures and automation. Refer to each
repository's documentation:

- [baremetal-operator releasing docs](https://github.com/metal3-io/baremetal-operator/blob/main/docs/releasing.md)
- [cluster-api-provider-metal3 releasing docs](https://github.com/metal3-io/cluster-api-provider-metal3/blob/main/docs/releasing.md)
- [ip-address-manager releasing docs](https://github.com/metal3-io/ip-address-manager/blob/main/docs/releasing.md)
- [ironic-standalone-operator releasing docs](https://github.com/metal3-io/ironic-standalone-operator/blob/main/docs/releasing.md)
- [ironic-image releasing docs](https://github.com/metal3-io/ironic-image/blob/main/docs/releasing.md)
- Check individual project repos for their specific release documentation

## Documentation Changes

The Metal3 documentation is published in book form at:
<https://book.metal3.io>

The source for the book is the [metal3-docs repository](https://github.com/metal3-io/metal3-docs)
containing markdown files, and we use [mdBook](https://github.com/rust-lang/mdBook)
to build it into a static website.

When submitting a documentation PR, remember to label it with the 📖 (:book:)
icon.

## Proposal Process

For significant changes to Metal3 projects, we encourage opening and discussing
proposals in the [metal3-docs repository](https://github.com/metal3-io/metal3-docs)
before implementation.

**When to create a proposal:**

- New features or capabilities
- Changes to existing user-facing behavior
- Significant architectural changes that affect users

**When a proposal is NOT needed:**

- Internal workflow changes
- Bug fixes
- Documentation improvements
- Refactoring that doesn't change behavior

**Proposal process:**

- Proposals should be associated with an issue in the relevant repository.
- Proposals should be introduced and discussed during the weekly Metal3 community
 meetings or on the [metal3-dev mailing list](https://groups.google.com/forum/#!forum/metal3-dev).
- Submit and discuss proposals using a collaborative writing platform,
 preferably Google Docs, and share documents with edit permissions with the
  [metal3-dev mailing list](https://groups.google.com/forum/#!forum/metal3-dev).
  This is not a mandatory step as such. If consensus is reached already, a
  contributor can push a pull request instead as described in the next bullet.
- Once consensus is reached, the proposal should be documented as a Pull Request
in the [metal3-docs repository](https://github.com/metal3-io/metal3-docs/tree/main/design).

Note: Unlike some Kubernetes projects, Metal3 does not use a formal
EP (Enhancement Proposal) process, but we do value thorough design discussion
for significant changes.

## Triaging Issues

Issue triage in Metal3 follows similar best practices to Kubernetes projects
while being adapted to the size and needs of the Metal3 community.

While the maintainers play an important role in the triage process described
below, the help of the community is crucial to ensure that this task is
performed in a timely manner and is sustainable long term.

| Phase                | Responsible | What is required to move forward                                           |
|----------------------|-------------|----------------------------------------------------------------------------|
| Initial triage       | Maintainers | The issue MUST have: a [kind/*] label                                     |
| Milestone assignment | Maintainers | Milestones assigned during bi-weekly community meetings                    |
| Triage finalization  | Everyone    | Consensus on the way forward and actionable details                        |
| Triage finalization  | Maintainers | `triage/accepted` label; optional: `help-wanted` or `good-first-issue`    |
| Actionable           | Everyone    | Contributors with time and reviewers/approvers with bandwidth              |

[kind/*]: https://github.com/search?q=org%3Ametal3-io+label%3Akind&type=issues

**Important notes:**

- **Milestones** are assigned during bi-weekly community meetings, not during
  initial triage. This allows the community to discuss and reach consensus on
  priorities collectively.
   - When assigning milestones, several factors are taken into consideration,
     including impact on users, relevance for upcoming releases, and maturity
     of the issue (consensus + completeness).
   - Assigning a milestone is not a commitment to execute within a certain time
     frame, because implementation depends on contributors volunteering time to
     do the work and on reviewers/approvers bandwidth.
   - If a milestone is not assigned manually, then the bot will do it during
   the merge process.

- Closing inactive issues which are stuck in the "triage" phases is crucial
  for maintaining an actionable backlog. The following automation applies to
  issues in the "triage" or "refinement" phase:
   - After 90 days of inactivity, issues will be marked with the
     `lifecycle/stale` label
   - After 30 days of inactivity from when `lifecycle/stale` was applied,
     issues will be marked with the `lifecycle/rotten` label
   - After 30 days of inactivity from when `lifecycle/rotten` was applied,
     issues will be closed. Closed issues remain a valuable part of the
     knowledge base about the Metal3 project.
   - Note:
      - Maintainers can apply the `lifecycle/frozen` label to exclude an issue
        from the automation above
      - Issues excluded from the automation will be re-triaged periodically

- If you care about an issue stuck in the "triage" phases, you can engage
  with the community or try to figure out what is holding back the issue by
  yourself, e.g.:
   - Issue too generic or not yet actionable
   - Lack of consensus or the issue is not relevant for other contributors
   - Lack of contributors; finding ways to help and free up maintainers/other
     contributors time from other tasks can really help to unblock your issues.

- Issues in the "actionable" state are not subject to the stale/rotten/closed
  process; however, they should be re-assessed periodically as the project
  changes quickly.

## AI Agent Guidelines

Metal3 repositories include an `AGENTS.md` file that provides specific
instructions for AI coding agents. These files contain:

- Repository-specific structure and patterns
- Testing standards and make targets
- Code conventions and linting rules
- Key workflows for common development tasks
- Security requirements and best practices

If you're using AI tools to contribute to Metal3 projects, consult the
`AGENTS.md` file in the specific repository (if available) for detailed
guidance on that project's structure and conventions.

## Google Doc Access

To gain access to Google Docs used in Metal3 projects, please
join the [metal3-dev](https://groups.google.com/forum/#!forum/metal3-dev)
Google group.
