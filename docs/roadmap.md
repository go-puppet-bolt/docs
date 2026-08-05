# Roadmap

## Done

- **Inventory (v2)** — Targets, nested groups, per-group / per-target `config` / `facts` / `vars` / `features`, effective-value resolution through the hierarchy (deep merge, closest group wins, target overrides all), and target selection by name / alias / group / glob (`TargetsForQuery`).
- **Tasks** — Parses the Bolt task `*.json` metadata shape (typed `parameters`, `input_method`, `supports_noop`, `implementations`, `files`) and validates arguments against the declared parameter types.
- **YAML plans** — `plan.yaml` `parameters` plus an ordered list of `task` / `command` / `script` / `eval` / `plan` / `resources` / `message` steps with `targets`, per-step parameters and a `return` expression, executed by a step runner.
- **Puppet-language (`.pp`) plans** — `plan name(...) { ... }` manifests run through `github.com/go-puppet/puppet`; the `run_task` / `run_command` / `run_script` / `get_targets` / `apply` plan functions dispatch through the executor's transports and inventory (`Executor.RunPuppetPlan`).
- **Transports** — a `Transport` interface with a host-local `LocalTransport`, a full pure-Go **SSH** transport (`SSHTransport`, over `golang.org/x/crypto/ssh`) and a full pure-Go **WinRM** transport (`WinRMTransport`, WS-Management / MS-WSMV over `net/http`, with `basic` / `negotiate` (NTLM) / `ssl` auth).
- **Executor & `apply`** — an `Executor` runs a command, script or task across a target set into a `ResultSet`; `apply` blocks and the `resources` step compile a catalog via `go-puppet/puppet` and produce an apply `ResultSet` (`Executor.ApplyCatalog`).

## Documented boundaries (not silently capped)

- `apply` execution is compile-and-report only: it does not remotely enforce resources (that needs a Puppet agent on the target — Bolt's `apply_prep` model).
- WinRM Kerberos / CredSSP auth is not implemented (`basic` / `negotiate` / `ssl` are).
- PuppetDB and other `_plugin` inventory resolvers are not implemented.

Quality is a standing gate: 100% coverage including error branches, `gofmt` + `go vet` clean, CI green across the six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x).
