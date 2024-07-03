# Contributor Ladder

This contributor ladder outlines the different contributor roles within the
Metal3 project, along with the responsibilities and privileges that come with
them. Community members generally start at the first levels of the "ladder"
and advances up it as their involvement in the project grows. Our contributors
are happy to help you advance along the contributor ladder.

Each of the contributor roles below is organized into lists of three types
of things. "Requirements" are things that a contributor is expected to do at
that level. "Qualifications" are qualifications a person needs to meet to be at
that level (may be one or any/all of them), and "Privileges" are things
contributors on that level are entitled to.

As the Metal3 project grows, the current roles may be broken out into new roles
and/or teams and roles may no longer be needed.

The final judgment on whether an individual fulfills the criteria for a role is
up to the Metal3 community. After six months of inactivity or after any behavior
detrimental to the future of the project, any contributor can be removed from
their position(s).

## Community Contributor

A Community Contributor contributes directly to the project and
adds value to it. Contributions need not be code. People at the Community
Contributor level may be new contributors, or they may only contribute
occasionally.

* Responsibilities:

   * Must follow the [Metal3 Code of Conduct](CODE_OF_CONDUCT.md)
   * Follow the contributing guide for each repository, for example see
    Cluster Api Provider Metal3's [CONTRIBUTING.md](https://github.com/metal3-io/cluster-api-provider-metal3/blob/main/CONTRIBUTING.md)

* Qualifications:

  Anybody that is participating in the Metal3 community is welcome. Any
  contribution counts. Some possible forms of contribution are:
   * Participating in community discussions
   * Helping other users in Slack or in person
   * Submitting bug reports
   * Trying out new releases
   * Attending community events
   * Giving talks about Metal3 virtually or in person
   * Report and sometimes resolve issues
   * Occasionally submit Pull Requests(PRs)
   * Writing a Call For Proposal(CFP)
   * Contribute to the documentation
   * Show up at community meetings and/or leads and/or takes notes
   * Answer questions from other community members
   * Submit feedback and comments on issues and PRs
   * Test releases, patches and submit reviews
   * Promote the project in public

* Privileges:

   * Ability to be assigned to Issues and ask for Reviews

Becoming a Community Contributor is the first step towards becoming an
Organization Member.

## Organization Member

An Organization Member is an established contributor who regularly
participates in the project. Organization Members have privileges in project
repositories and as such are expected to act in the interests of the whole
project. Organization Members can take on
[additional roles](CONTRIBUTOR-ROLES.md) within the project and can work more
independently, for example CI tests are automatically triggered in their
PRs.

An Organization Member has all the rights and responsibilities of a Community
Contributor, plus:

* Responsibilities:

   * Continues to contribute regularly
   * Work to accomplish the tasks they volunteer to do within the project

* Qualifications:

   * Must have successful contributions to the project, including at least one
   of the following:
      * Written and/or reviewed PRs,
      * Reviewed PRs,
      * Made contributions that resolved Issues,
      * Or some equivalent combination or contribution
   * Must be actively contributing to at least one project area, where the
   privileges of Organization Member will be beneficial

* Privileges:

   * Can trigger CI
   * Can recommend other contributors to become Metal3 GitHub Org members
   * Can nominate themselves to [other roles](CONTRIBUTOR-ROLES.md) within Metal3

The process for a Contributor to become an Organization Member is as follows:

1. Open an issue in the [community repo](https://github.com/metal3-io/community)
1. Assign the issue to a [community maintainer](OWNERS).

## Reviewer

A Reviewer has responsibility for specific code, documentation,
test, or other project areas. They are collectively responsible, with other
Reviewers, for reviewing all changes to those areas and indicating whether those
changes are ready to merge. They have a track record of contribution and review
in the project.

Reviewers are responsible for specific repositories and/or technical areas.
Most often it is one or more related Git repositories. Reviewers have all the
rights and responsibilities of an Organization Member.

* Responsibilities:

   * Regularly reviewing Pull Requests against their specific areas of
    responsibility, including Pull Requests that are assigned to them
   * Helping other contributors become reviewers

* Requirements:

   * Is an Organization Member
   * Has a track record of constructive and valuable reviews
   * Has demonstrated knowledge of the specific area, for example by resolving
   test failures, or through contributions
   * At least 10 PRs in the specific repository.
   * Is supportive of new and occasional contributors and helps get useful PRs
   in shape to commit

* Privileges:

   * Has rights to `/lgtm` pull requests in specific directories
   * Can recommend and review other Org Members to become Reviewers

The process for an Organization Member to become a Reviewer is as follows:

1. Open a PR in specific repository towards the `OWNERS` file and add the
 persons GitHub handle under `reviewers:` section.

## Approver

An Approver has similar responsibility for specific code, documentation,
test, or other project areas asa Reviewer. An Approver is however expected to
have deeper understanding of the code base of the specific repository or technical
area than a Reviewer and also has shown a substantial track record of
contribution and review in the project.

Approvers are responsible for specific repositories and/or technical areas.
Most often it is one or more related Git repositories. Approvers have all the
rights and responsibilities of a Reviewer.

* Responsibilities:

   * Same as Reviewers
   * Engage/drive in feature proposals and ensure release readiness of the
   repository.
   * Update/modify project goals and governance policies.
   * Define/maintain project [roadmap](ROADMAP.md).

* Requirements:
   * Has a track record of constructive and valuable reviews as a Reviewer
   * Has demonstrated significant knowledge of the specific area, for example by
   introducing new features in the repository and has substantial number of
   commits in the repo or has a considerable amount of understanding of Metal3
   ecosystem in case he/she is being considered as an Approver in a new
   repository is being introduced in the project.

* Privileges:
   * Has rights to `/approve` and `/lgtm` pull requests in specific
   directories
   * Can recommend and review other Org Members to become Approvers

The process for an Organization Member to become an Approver is documented
[here](maintainers/README.md).

**Note**: If the current maintainers of a specific repository identify that an
existing Approver/Reviewer is no longer active in the repository, the maintainer
can discuss the matter with the person and upon agreement move the persons
Github handle under `emeritus_approvers:` or `emeritus_reviewers:` section
accordingly in the `OWNERS` file in the repository by opening up a PR.
