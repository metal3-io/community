# Metal3 Roadmap

The Metal3 Roadmap is maintained as a GitHub project and can be found
[here](https://github.com/orgs/metal3-io/projects/8).

## Description

Each column in the project represents the work items for either
Baremetal Operator, Cluster API Provider Metal3, IP Address Manager,
Ironic image and other Metal3 components. In addition there is
a `No status` column that contains items that have not yet been
accepted and the `Backlog` column that contains items that have been accepted
but work has not started on them.

An issue can be planned for a specific release if someone volunteers to take
ownership of the issue. The owner will then be assigned the issue. An owner
does not have to carry the whole design and implementation processes on her own
but must instead make sure that the feature is being worked on and will be
completed by the release date of the related milestone.

## Community Meetup

The high level direction of the Metal3 project is discussed mainly in yearly
community meetups. Meetups are usually long discussions within the community
and the specific focus is to discuss long term goals and Roadmap items for the
project.

First Metal3 meetup was arranged on 15th of May 2023. Here is the
[link](https://docs.google.com/document/d/1Exm5k6JB2JlD9_FOaM68R2ASHi7ZsryF_xEqLG1j4zA)
of the meeting minutes and discussion topics for all meetups ever since the
first.

## Community Meetings

The priorities of the project are iteratively refined and adjusted during
weekly meetings that are open to all community members. During the weekly
community meetings contributors can propose and discuss new features,
bugs, adjustments to release timelines, thus the roadmap of the project is
under constant maintenance.

## Proposing a feature

Proposing a new feature for one of the components is done by
opening an issue in the [metal3-docs](https://github.com/metal3-io/metal3-docs)
repository, describing the feature and which components and release are
targeted. The new issue will automatically appear in the "No status"
column of the roadmap and will stay there until it is triaged and accepted.
After an issue gets accepted by default it should be placed to the backlog
unless work has already started on it or will start shortly.

If a feature is proposed for a specific release then the contributor has
to mention it in the issue description and during triaging it  will be
evaluated whether the topic of the issue is a good fit for the given release
and a milestone will be assigned accordingly.

## Issue triaging

Issues are constantly reviewed and triaged every day by the maintainers and in
addition to that there is a bi-weekly event on the Community Meetings where the
meeting participants triage all the issues that haven't been triaged by that
time.

If an accepted issue is specifically targeting a release then the issue
will be assigned to the relevant milestone, if the issue is already being
worked on it will receive the appropriate status in the roadmap.

An inactive issue (marked as stale) can be moved back to the `Backlog` column,
and issues in the `Feature Requests` column that are not actual feature
proposals but issues related to metal3-docs repository can be removed from the
project.

## Ownership of the Roadmap

The Roadmap update and maintenance is performed by "Roadmap editors" team of the
Metal3 project.
