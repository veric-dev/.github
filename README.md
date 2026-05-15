# veric-dev/.github

Org-level shared resources for the **veric-dev** fleet:

- **`ai-fundamentals`** — substrate (`ai-kernel`, `ai-program`, `ai-tower`,
  `ai-veric-analyses`, …)
- **`veric-engine`** — analyser engine + CLI; the `veric-ag` (silver)
  workspace; the `veric-saas-types` exporter
- **`veric-saas`** — TypeScript Next.js front-end; SARIF ingest
- **`veric-aws`** — OpenTofu infrastructure-as-code; CI ops jobs
  (kellnr-gc, stale-sweep aggregator, OIDC bootstrap)

## Layout

```
.
├── actions/                     # Composite GitHub Actions, callable as
│   │                            #   uses: veric-dev/.github/actions/<name>@main
│   ├── branch-prerelease-version/
│   ├── cargo-registry-auth/
│   ├── sccache-stats/
│   ├── setup-rust/
│   └── setup-sccache/
└── .github/workflows/           # Reusable workflows (GitHub requires
                                 # this exact path), callable as
                                 #   uses: veric-dev/.github/.github/workflows/<name>.yml@main
```

`actions/` is at repo root (not `.github/actions/`) so the `uses:`
reference reads as `veric-dev/.github/actions/<name>@main` rather than
the awkward `veric-dev/.github/.github/actions/<name>@main`.

Reusable workflows are required by GitHub to live under
`.github/workflows/`, so the double-`.github` cannot be avoided there.

## Versioning

Consumers reference `@main` for the rolling head. There is no tagged
release stream yet — the four consumer repos all run on a tightly
synchronised cadence and the cost of a stale tag would exceed the
benefit.

If a breaking change is introduced (input renamed, default flipped,
output removed), audit the four repos before merging:

```bash
gh search code "uses: veric-dev/.github/actions/<name>@" --owner veric-dev
```

## Adding new shared content

A pattern is a candidate for hoisting when **at least two of the four
fleet repos carry the same YAML** (with at most cosmetic drift). The
audit that produced the initial set is in
`ai-fundamentals/cron-triage-2026-05-14.md` (B3 task #140).

Open a PR against this repo first; cut the consumer-repo migration PRs
in a single batch once it lands so the version drift window is short.
