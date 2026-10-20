# schedkit roadmap

Recent Linux kernels ship `sched_ext`, which lets CPU schedulers be written in
eBPF and swapped at runtime. The kernel side is solved; distributing,
verifying and operating schedulers across machines is not. schedkit's goal is
to make a CPU scheduler as easy to ship, trust and roll out as any other piece
of software: pulled from an OCI registry, verified with Sigstore, and managed
by tools platform teams already use.

This roadmap is a statement of direction, not a promise of dates. It is
reviewed by the maintainers at least once per quarter. To propose a change,
open a Discussion or a pull request against this file.

_Last updated: October 2026._

## Now (next ~3 months)

### Project and community

- Written governance, maintainer list, security policy, contributing guide and
  Code of Conduct across the organization.
- DCO enforced on every repository.
- Onboard at least one additional maintainer, ideally from a different
  organization.
- Apply to the CNCF Sandbox and present schedkit to CNCF TAG Workloads
  Foundation.
- OpenSSF Scorecard on every repository and an OpenSSF Best Practices badge.

### schedctl

- Automated, reproducible releases: signed binaries (Sigstore keyless), SBOM
  and provenance attached to every GitHub release.
- Continue distribution packaging beyond openSUSE and the AUR (Debian, Fedora,
  Nix, Gentoo). <!-- TODO: update with current status of each -->

### sked

- Harden the path to a stable `v1.0`: upgrade and CRD migration story in the
  Helm chart, documented compatibility matrix (Kubernetes and kernel
  versions). (proposed)

### plumbing

- Pin every base and upstream image by digest and automate upstream version
  bumps. (proposed)
- Document a license and provenance check for every scheduler in the catalog.

## Next (3–9 months)

- **Shared trust policy.** schedctl and sked currently each have their own
  signature verifier and trust-policy format. Converge on one format and one
  library so a policy can move between a laptop and a cluster unchanged.
  (proposed)
- **Scheduler image conventions.** Turn the OCI labels used today (for example
  `org.schedkit.scheduler.kernel.min-version`) into a documented, versioned
  convention that third-party scheduler authors can target, with a
  conformance check in `schedctl doctor`/`schedctl info`. (proposed)
- **Observability.** Expose per-node scheduler state (active scheduler,
  version, sched_ext kernel state, failures and fallbacks to the default
  scheduler) as Prometheus metrics and Kubernetes events. (proposed)
- **Safer rollouts in sked.** Progressive rollout across node pools with
  automatic rollback when the kernel ejects a scheduler. (proposed)

## Later / ideas

- Integration with node-level resource management (NRI plugins, CPU manager
  policies) so workloads can express scheduling intent. (proposed)
- A browsable catalog of verified scheduler images on schedkit.io. (proposed)
- Support for additional container runtimes (for example CRI-O). (proposed)

## Non-goals

- Writing or maintaining CPU schedulers ourselves. schedkit packages,
  verifies, distributes and operates schedulers developed upstream (for
  example in [sched-ext/scx](https://github.com/sched-ext/scx)).
- Replacing the Kubernetes pod scheduler (`kube-scheduler`). schedkit is about
  the kernel CPU scheduler on each node, not about placing pods on nodes.
