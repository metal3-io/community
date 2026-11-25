# Metal3 Release & Signal Team

## Overview

This document introduces the concept of Metal3 release team.
The release team serves the following purposes:
- Share information about how the release team is organized, assembled, and how
  one can become a member of the release team.
- Define the release team responsibilities.
- Provide a guide to the community memmbers on how to engange with the release
  activities
- Spreads the workload from few maintainers to bigger task organized group.

**Note**: this document is intended to explain the release team. For information
about the release process, refer to [Releasing in Metal3](https://github.com/metal3-io/metal3-docs/blob/main/processes/releasing.md).

## Duration of Term

The term in release team will last approximately four months and aligns with one
minor release cycle. A cycle starts from previous minor release and ends after
creating a new minor release.

There is no limit to the number of terms a release team member can serve. It is
possible to be a release team member for multiple consecutive terms or re-join
the team again after a period of absence from the team.

## Team Roles
The team has following roles:
- Release lead (only one person per cycle, short form RL)
- Release coordinator(s) (short form: coordinator)
- CI signal lead (only one person per cycle, short form CIL)
- CI signal tracker(s) (short form: tracker)

## Specific Responsibilities

The Release lead is also a Release coordinator. The CI signal lead is also
a CI signal tracker.

The Release lead is responsible for making sure
all the responsibilities of the Release coordinators are being fulfilled and
also acts as a coordinator.

The CI signal lead is responsible for making sure
all the responsibilities of the CI signal trackers are being fulfilled and
also acts as a tracker.

All the roles are responsible to coordinate with all other roles in a timely
manner to ensure the success of the release.

### Release coordinators
- Release patch releases as often as needed. Usually, but not limited to, a week
  after CAPI and/or Ironic release.
- Release a minor version every 4 months, usually following CAPI release cycle.
- Create and publish beta and release candidates.
- Create the release branch before the release is finalized.
- Set up and verify the automation for the release branches.
- Manage and communicate the feature freeze periods leading up to the releases.
- Ensure that only eligible pull requests are cherry picked to release branches.
- Maintain and improve release related documentation, issue tracking etc...
- Maintain and improve release automation, tooling & related developer docs.
- Track tasks needed to ensure compatibility with upstream components for
  the upcoming release(es) e.g. K8s, CAPI, Ironic etc...
- Make sure milestones contain only Issues/PRs relevant for the given release.
- Coordinate with code owners & authors who have pending issues/PRs
  under a milestone that is relevant for the particular release.
  The goal is to reach a compromise where the release can progress on schedule
  but as much of the goals of the milestone are reached as possible.
- Provide written public and transparent post-cycle retrospective report on
  the release activities done during the cycle with improvement suggestions for
  the next cycle there are any. (what went well, what went wrong, what should
  be improved)

### CI Signal trackers
- The responsibility of the trackers is to monitor the CI continuously.
- The goal is to report CI issues in time and to the relevant maintainers and
  contributors so that the CI issues affecting development and release workflows
  would be mitigated as much as possible.
- Setup CI&CD jobs, jobs for new releases, cleanup of old jobs etc.
- If determined that the issue originates from the infrastructure platform
  which hosts the Metal3 CI ecosystem, the trackers report it to the
  lead who has access to further investigate.
- The trackers are not required to have, and won't be granted elevated
  permissions to the infrastructure platform when taking on the tracker role.
- Tracker are not responsible to solve all the CI issues, trackers are
  encouraged to try but their main task is to find people who can fix or
  mitigate the issue and inform them.
- Trackers are expected the monitor the progress of the CI fixes/mitigations.

## Team Repository Permissions

### RL & Coordinators
- Cherry pick commits to research branches.
- Run tests
- Create and publish draft releases.

### Trackers
- No elevated permissions, all contributors should be able to push changes
  to CI pipelines, if some changes require e.g. introduction of new
  credentials or testing in a staging environment, then the CIL will help
  with that.
- Tracker(s) might be granted a limited set of elevated permissions for a
  temporary period in a scenario that is deemed emergency by the CIL
  or other infrastructure owning maintainers (based on who is available).

### CIL
- CIL is a maintainer with elevated priveledges to the Metal3 infra.

## Team Selection
- RL selection happens when the current RL is unable or unwilling to continue
  the role.
- The RL is selected from coordinators of the last two release cycle who
  nominated themselves for the role.
  The new RL is selected  by the prior RL or current CIL or the Maintainers in
  this descending priority order based on who is available to make the decision.
- CIL selection happens when the current CIL is unable or unwilling to continue
  the role.
- The CIL role can be fulfilled only by a maintainer who has elevated
  access rights and understanding of the Metal3 infra. (One of the Infra owners)
- The release & signal team is assembled by the RL and the CIL from volunteers.
  The team is assembled in the very beginning of the release cycle. A call for
  volunteers will be made through the usual communication channels
  (community meeting, Slack, mailing list, etc.).
The team is looking for volunteers who:
- Can commit to the amount of time required across the release cycle.
- Are enthusiastic about being on the release & signal team.
- Have some prior experience contributing to Metal3 or other CNCF project
  releases.

There is no upper limit on the amount of coordinators or trackers that can
participate, but the RL and CIL is allowed to limit the team size in case the
number of volunteers exceed the number of participants that either
the RL or CIL is comfortable working with.

The goal of the RL and the CIL is to make a capable release & signal team
and give the opportunity for a diverse group of people to participate
in each release cycle. In order to give a chance to participate for the
widest possible group of people, the RL and CIL re-assembles the
team at the beginning of each cycle. As part of the re-asembly the RL and CIL
may not re-select trackers and/or coordinators in order to give a chance to
new first time participants or to provide opprtuinty for contributors to re-join
after a considerable period of absence.

- Only human beings are allowed to join the release & signal team, non-human
  intelligence is not welcome.

## Time Commitment
As coordinator you should expect to spend approximately 4-8 hours during patch
release weeks and during the last four weeks of the cycle (when doing
work related to minor release).

As tracker the time commitment is based on the state of the CI, but you
are expected to check the state of the CI every week during the cycle if
possible multiple time and keep track of the issues and make sure the
relevant community members are alerted and kept informed.

Members are not expected to be available every week, but we expect members to
communicate their availability, so the release team can plan accordingly.

Before you volunteer to be part of a Metal3 release & signal team, please make
certain that your other personal or professional obligations don't interfeer
with your commitment to the release team. Unexpected scenarios can happen
but the RL and CIL has the authority to remove members from the team
if the member is not responsive or unreliable. This decision is up to the
individual RL and CIL as ultimately they are responsible for the success of
the release.

## Why Should I Volunteer
- This is a great way to contribute to the project without having deep technical
  knowledge of the internals.
- Create relationships with other members of the community.
- Contribute to the project health.
- If your goal is to become a reviewer, this is a good gateway.
