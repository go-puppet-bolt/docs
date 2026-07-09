# Roadmap

## Done

- **Inventory (v2)** — Targets, nested groups, per-group / per-target `config` / `facts` / `vars` / `features`, effective-value resolution through the hierarchy (deep merge, closest group wins, target overrides all), and target selection by name / alias / group / glob (`TargetsForQuery`).
- **Tasks** — Parses the Bolt task `*.json` metadata shape (typed `parameters`, `input_method`, `supports_noop`, `implementations`, `files`) and validates arguments against the declared parameter types.
- **YAML plans** — `plan.yaml` `parameters` plus an ordered list of `task` / `command` / `script` / `eval` / `plan` / `resources` / `message` steps with `targets`, per-step parameters and a `return` expression, executed by a step runner.
- **Transports & executor** — A `Transport` interface with a host-local `LocalTransport` driven through an injectable `CommandRunner` seam; an `Executor` runs a command, script or task across a set of targets, collecting a per-target `Result` into a `ResultSet`.

## Next

- **SSH / WinRM & Puppet-language plans** — Only `LocalTransport` ships (`Transport` is the extension point); SSH / WinRM transports, Puppet-language (`.pp`) plans, `apply` / `resources` steps and PuppetDB inventory references are documented deferrals.

Quality is a standing gate: 100% coverage including error branches, `gofmt` + `go vet` clean, CI green across the six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x).
