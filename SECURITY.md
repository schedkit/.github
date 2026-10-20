# Security policy

schedkit loads third-party code into the running kernel with elevated
privileges, so we take security reports seriously. Thank you for helping keep
schedkit and its users safe.

## Reporting a vulnerability

**Please do not report security vulnerabilities through public GitHub issues,
pull requests or Discussions.**

Use one of these private channels:

1. **GitHub private vulnerability reporting (preferred).** Open the
   repository affected, go to the *Security* tab and choose
   *Report a vulnerability*. If you're not sure which repository is affected,
   use [schedctl](https://github.com/schedkit/schedctl/security/advisories/new).
   <!-- TODO: enable private vulnerability reporting in every repository -->
2. **Email** **security@schedkit.io**.

Please include as much of the following as you can:

- affected repository, component and version (tag, image digest or commit);
- the kind of issue (for example signature verification bypass, privilege
  escalation, container escape, denial of service of the host scheduler);
- step-by-step instructions to reproduce, and a proof of concept if you have
  one;
- kernel version and container runtime, where relevant;
- the impact as you understand it.

## What happens next

| Step | Target time |
| ---- | ----------- |
| Acknowledge the report | 3 business days |
| Initial assessment and severity | 10 business days |
| Fix or mitigation for critical and high severity issues | 30 days, where possible |
| Public advisory | when a fix is available, coordinated with the reporter |

We will keep you informed throughout, credit you in the advisory unless you'd
prefer to stay anonymous, and agree on a disclosure date with you. We aim for
coordinated disclosure within 90 days of the report.

Fixes are published as GitHub Security Advisories in the affected repository,
with a CVE requested through GitHub when appropriate.

## Scope

In scope:

- **schedctl**: signature verification and trust policy handling, privilege
  handling, interaction with containerd and Podman.
- **sked**: the controller, the admission webhook, the trust policy, RBAC and
  the Helm chart defaults.
- **plumbing**: the build and signing pipeline and the published images under
  `ghcr.io/schedkit/*` (for example a compromised build or an image that is not
  what its labels claim).
- **docs**: the schedkit.io website and its install instructions.

Out of scope, please report upstream:

- bugs in the scheduler code itself (for example
  [sched-ext/scx](https://github.com/sched-ext/scx) or
  [Gthulhu](https://github.com/gthulhu/gthulhu)), unless our packaging
  introduces the problem;
- bugs in the Linux kernel `sched_ext` framework (see the kernel's
  [security bug process](https://docs.kernel.org/process/security-bugs.html));
- bugs in containerd, Podman, Kubernetes or Sigstore.

If you're not sure, report to us and we'll help route it.

Running a scheduler with `--allow-unsigned`, or with a trust policy that
trusts an attacker-controlled identity, loads code you have chosen not to
verify; that is expected behavior, not a vulnerability.

## Supported versions

Security fixes are made on the latest minor release of each component.

| Component | Supported |
| --------- | --------- |
| schedctl  | latest minor release (currently 1.3.x) |
| sked      | latest minor release (currently 0.3.x) |
| plumbing images | latest published tag of each scheduler |

## Verifying what you run

The sked image and the scheduler images in `ghcr.io/schedkit/*` are signed
with Sigstore keyless signing from GitHub Actions and ship SBOMs and SLSA
provenance. See the
[sked README](https://github.com/schedkit/sked#verify-signatures-and-attestations)
for verification commands. `schedctl` verifies scheduler image signatures by
default before loading anything into the kernel.
<!-- TODO: once schedctl release binaries are built and signed in CI, add
     verification instructions for them here. -->
