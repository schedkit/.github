# Contributing to schedkit

Thanks for wanting to help! schedkit is a set of tools to package `sched_ext`
schedulers as OCI images and run them anywhere, from a laptop to a Kubernetes
fleet. Contributions of every kind are welcome: bug reports, docs, packaging
for your distribution, new schedulers in the catalog, reviews and code.

By participating you agree to follow our [Code of Conduct](./CODE_OF_CONDUCT.md).

## Where things live

| Repository | What it is | Language |
| ---------- | ---------- | -------- |
| [schedctl](https://github.com/schedkit/schedctl) | Host-side CLI: pulls, verifies, runs and stops scheduler images via containerd or Podman | Go |
| [sked](https://github.com/schedkit/sked) | Kubernetes operator: `Scheduler` CRD, admission webhook, trust policy, Helm chart | Go |
| [plumbing](https://github.com/schedkit/plumbing) | Catalog of OCI-packaged schedulers built from upstream sources | Dockerfile |
| [docs](https://github.com/schedkit/docs) | The [schedkit.io](https://schedkit.io) website | TypeScript (Docusaurus) |
| [.github](https://github.com/schedkit/.github) | Org-wide community files (this one!) | Markdown |

## Ways to contribute

- **Report a bug.** Open an issue in the relevant repository. For anything
  that touches the kernel, please include `uname -r`, the scheduler image and
  tag, the output of `schedctl doctor` and `schedctl status`, and relevant
  kernel log lines (`dmesg | grep -i sched_ext`). Yes, we really do want the
  kernel logs.
- **Ask a question or propose an idea.** Use
  [GitHub Discussions](https://github.com/orgs/schedkit/discussions).
  Cross-project design topics (naming, scope, shared APIs) belong there too.
- **Find something to work on.** Look for issues labelled
  `good first issue` or `help wanted`.
  <!-- TODO: create these labels in every repo -->
- **Propose a significant change.** New APIs, CRD changes, changes to image
  conventions or signature verification need a short written proposal first;
  see [GOVERNANCE.md](./GOVERNANCE.md#significant-changes).
- **Report a security issue.** Please don't open a public issue; follow
  [SECURITY.md](./SECURITY.md).

## Developer Certificate of Origin (DCO)

Every commit must be signed off, certifying that you wrote the change or
otherwise have the right to submit it under the project's license, as
described in the [Developer Certificate of Origin](https://developercertificate.org/).

Add the sign-off with `-s`:

```sh
git commit -s -m "schedctl: add --output json to versions"
```

which appends a line like:

```
Signed-off-by: Jane Doe <jane@example.com>
```

The name and email must match the commit author. A DCO check runs on every
pull request. If you forgot, fix the last commits with
`git rebase --signoff HEAD~N` and force-push.

## Pull request workflow

1. Fork the repository and create a branch from `main`.
2. Keep pull requests focused: one logical change per PR is much easier to
   review than a grab bag.
3. Add or update tests and documentation alongside the code. Changes to
   commands or CRDs usually need a matching update in the
   [docs](https://github.com/schedkit/docs) repository.
4. Make sure the checks below pass locally.
5. Open the pull request, describe *what* and *why*, and link the issue it
   addresses.
6. A maintainer reviews it. Merge rules are in
   [GOVERNANCE.md](./GOVERNANCE.md#pull-requests).

### Commit messages

Use a short prefix with the area you're touching, an imperative summary, and
a body explaining the reason for the change when it isn't obvious:

```
verify: reject trust policies with an empty identity list

An empty list made the policy match nothing, which looked like a
signature failure. Fail at load time with a clear error instead.

Signed-off-by: Jane Doe <jane@example.com>
```

## Development setup

### schedctl

Requirements: Go (see `go.mod`), `make`, `libbtrfs` headers, and for
integration tests `qemu`, KVM and [mkosi](https://github.com/systemd/mkosi).

```sh
make            # build ./schedctl
make test       # unit tests (go test -race)
make lint       # golangci-lint
make man        # regenerate man pages after changing commands; CI checks them
```

Builds need the `containers_image_openpgp` build tag, which avoids linking
against `libgpgme`:

```sh
go build -tags containers_image_openpgp ./...
```

Integration tests boot an openSUSE Tumbleweed VM image built with mkosi; the
[schedctl README](https://github.com/schedkit/schedctl#qemu-rootfs) explains
how to build the rootfs and kernel image locally. Running scheduler-related
code paths usually needs root.

### sked

sked is a [Kubebuilder](https://book.kubebuilder.io/) project.

```sh
make test           # unit + envtest
make lint           # golangci-lint
make test-e2e       # end-to-end tests on a kind cluster
make test-helm      # lint and test the Helm chart
make verify-generated verify-helm   # CI fails if generated files are stale
```

After changing API types, run `make manifests generate helm-sync` and commit
the result.

### plumbing (adding a scheduler)

Each scheduler is a directory with a `Dockerfile` and a `VERSION` file. To add
one:

1. Read [Packaging a scheduler](https://schedkit.io/docs/schedctl/packaging-a-scheduler).
2. Add a directory with a multi-arch `Dockerfile` (AMD64 and ARM64) that
   builds from a pinned upstream version, plus the OCI labels used by the
   existing images (`org.opencontainers.image.*`,
   `org.schedkit.scheduler.kernel.min-version`).
3. Add it to the build matrix in `.github/workflows/oci-build-publish.yml` and
   to `manifest.json`.
4. In the pull request, link the upstream project and state its license.
   Schedulers must be published under an open source license that allows
   redistribution.

### docs

```sh
npm ci
npm start      # local dev server
npm run build  # what CI runs
```

## Reviewing

Reviews are contributions too, and the main path to becoming a reviewer and
maintainer. Be kind, be specific, and explain the "why" behind requested
changes. Pay particular attention to anything touching privileges, image
verification or the kernel interface.

## License

schedkit is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
By contributing you agree that your contributions are licensed under the same
terms. New dependencies must use a license on the
[CNCF allowlist](https://github.com/cncf/foundation/blob/main/policies-guidance/allowed-third-party-license-policy.md);
if in doubt, ask in the pull request.
