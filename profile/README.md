> OCI-packaged `sched_ext` schedulers, plus the tools to run them anywhere — from your laptop to a Kubernetes cluster.

## The pitch

Recent Linux kernels ship `sched_ext`, a framework that lets you write CPU schedulers in eBPF and swap them in at runtime. The kernel side is solved. **Distribution, lifecycle, and fleet management are not** — and that's the gap schedkit fills.

We treat schedulers like any other piece of software you'd ship today: they live in OCI registries, they're signed, they have versions, and they're orchestrated with tooling your platform team already understands.

## Projects

### [schedctl](https://github.com/schedkit/schedctl) — the host-side CLI

Pulls an OCI-packaged `sched_ext` scheduler, attaches it to the running kernel, and manages its lifecycle. Works with both Podman and containerd as the backing runtime.

Available on openSUSE Tumbleweed (`zypper in schedctl`) and the AUR (`paru -S schedctl`).

### [sked](https://github.com/schedkit/sked) — the Kubernetes operator

Distributes schedulers across cluster nodes via a CRD-based API, with node selectors, tolerations, and continuous reconciliation. If schedctl answers *"which scheduler should run on this machine?"*, sked answers *"which scheduler should run on which node pool, across the whole fleet?"*.

## Why OCI?

Because reinventing supply-chain primitives for kernel-loadable code in 2026 would be… a choice. OCI gives us content addressing, mirrors, mature tooling, and signature verification via sigstore/cosign — all things every ops team already knows how to operate. Schedulers are software; software lives in registries.

## Status

Early but real. We run schedkit ourselves and we welcome early adopters who are okay filing issues with kernel logs attached.

## Getting involved

Each project has its own issue tracker and discussions. For broader design questions about the schedkit ecosystem as a whole — naming, scope, cross-project APIs — this repository's *Discussions* tab is the right place.

## License

All schedkit projects are licensed under Apache-2.0.
