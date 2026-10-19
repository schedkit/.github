# schedkit governance

This document describes how the schedkit project is run: who makes decisions,
how they are made, and how people move into and out of project roles. It
applies to every repository in the [schedkit organization](https://github.com/schedkit).

schedkit is a small project and this document is deliberately lightweight. It
will grow with the community; changes to it follow the process in
[Changing this document](#changing-this-document).

## Values

- **Openness.** Design discussions, decisions and roadmap happen in public:
  GitHub issues, pull requests and Discussions. Private channels are only used
  for security issues and Code of Conduct reports.
- **Vendor neutrality.** No single company owns the project's direction.
  Maintainers act in the interest of the project and its users, not of their
  employer.
- **Safety first.** schedkit loads code into the running kernel. Changes that
  weaken image verification, privilege boundaries or supply-chain guarantees
  get extra scrutiny, and "it works on my machine" is not a review.
- **Kindness.** Everyone participating follows our
  [Code of Conduct](./CODE_OF_CONDUCT.md).

## Roles

### Contributors

Anyone who opens an issue, comments on a discussion, reviews a pull request,
writes documentation, packages schedkit for a distribution or sends code is a
contributor. No permission is needed.

### Reviewers

Reviewers are contributors with a track record in one or more repositories.
They can approve pull requests in those repositories. An approval from a
reviewer counts toward the merge requirements below, but at least one approval
must come from a maintainer.

A maintainer can nominate a contributor as reviewer by opening a pull request
against [MAINTAINERS.md](./MAINTAINERS.md). The nomination passes with lazy
consensus of the maintainers (see below).

### Maintainers

Maintainers are listed in [MAINTAINERS.md](./MAINTAINERS.md). They:

- review and merge pull requests;
- triage issues and keep the backlog honest;
- cut releases and keep the release process (signing, SBOMs, provenance)
  working;
- keep the [roadmap](./ROADMAP.md) up to date;
- make project-wide decisions as described below;
- act as the first point of contact for security reports and Code of Conduct
  reports.

#### Becoming a maintainer

Reviewers and contributors with sustained, high-quality contributions — code,
docs, reviews, packaging, release work or community support all count — can be
nominated by any maintainer. The nomination is a pull request to
`MAINTAINERS.md` and is approved by a **two-thirds supermajority** of current
maintainers, with no objection left unaddressed for at least one week.

While the project has fewer than three maintainers, the existing maintainers
nominate and approve new maintainers by consensus.

#### Stepping down and emeritus status

Maintainers can step down at any time by opening a pull request that moves
them to the emeritus section of `MAINTAINERS.md`. A maintainer who has been
unreachable or inactive for six months can be moved to emeritus by a
two-thirds vote of the other maintainers, after an attempt to contact them.
Emeritus maintainers can return through the normal nomination process.

#### Removing a maintainer

In the rare case a maintainer must be removed for reasons other than
inactivity (for example a Code of Conduct violation), the decision requires a
two-thirds vote of the other maintainers. Code of Conduct matters follow the
process described in [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md).

## Decision making

### Lazy consensus

Most decisions (merging a pull request, accepting a small feature, changing
CI) are made by **lazy consensus**: a proposal is accepted if no maintainer
objects within a reasonable time (at least 72 hours for anything non-trivial,
excluding weekends and public holidays where practical).

### Pull requests

A pull request can be merged when:

1. CI passes;
2. every commit carries a DCO sign-off (see [CONTRIBUTING.md](./CONTRIBUTING.md));
3. it has at least one approval from a maintainer who is not the author.

While the project has a single maintainer, that maintainer may merge their own
changes after leaving them open for review for at least 72 hours; this
exception goes away as soon as a second maintainer is added.

### Significant changes

The following need a written proposal (a GitHub issue or Discussion labelled
`proposal`) before code is merged:

- new or changed APIs: the `sked` CRDs, the `schedctl` CLI surface, the trust
  policy formats;
- changes to the OCI image conventions schedulers are expected to follow;
- changes to how images are signed or verified;
- adding or removing a repository from the organization;
- adding or removing a scheduler from the `plumbing` catalog.

Proposals stay open for at least one week. If consensus cannot be reached, any
maintainer can call a vote.

### Voting

When a vote is needed it happens on the GitHub issue or pull request, is open
for at least one week, and passes with a simple majority of maintainers unless
this document says otherwise. Each maintainer has one vote.

To keep the project vendor-neutral, once the project has at least three
maintainers, if more than half of them are employed by the same organization,
that organization's maintainers collectively hold votes equal to half the
total (rounded down) on any vote.

## Releases

Each repository is released independently using semantic versioning. Releases
are cut by a maintainer from a Git tag; CI builds, signs (Sigstore keyless) and
attaches SBOMs and provenance to the release artifacts. See each repository's
README for details.

## Code of Conduct

schedkit follows the [CNCF Code of Conduct](https://github.com/cncf/foundation/blob/main/code-of-conduct.md).
See [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md) for how to report a problem.

## Security

Security issues are handled as described in [SECURITY.md](./SECURITY.md).

## Changing this document

Changes to this document are made by pull request and need a two-thirds
supermajority of maintainers, with the pull request open for at least two
weeks.
