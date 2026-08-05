# Inventory, tasks & plans

This page walks the engine's capabilities. Each is complete unless explicitly marked as a documented deferral.

## Inventory (v2)

Targets, nested groups, per-group / per-target `config` / `facts` / `vars` / `features`, effective-value resolution through the hierarchy (deep merge, closest group wins, target overrides all), and target selection by name / alias / group / glob (`TargetsForQuery`).

## Tasks

Parses the Bolt task `*.json` metadata shape (typed `parameters`, `input_method`, `supports_noop`, `implementations`, `files`) and validates arguments against the declared parameter types.

## YAML plans

`plan.yaml` `parameters` plus an ordered list of `task` / `command` / `script` / `eval` / `plan` / `resources` / `message` steps with `targets`, per-step parameters and a `return` expression, executed by a step runner.

## Puppet-language (`.pp`) plans

`plan name(...) { ... }` manifests run through [`github.com/go-puppet/puppet`](https://github.com/go-puppet/puppet): the `run_task` / `run_command` / `run_script` / `get_targets` / `apply` plan functions dispatch through this executor's transports and inventory to real targets (`Executor.RunPuppetPlan`).

## Transports

A `Transport` interface with three implementations:

- **`LocalTransport`** — a host-local transport driven through an injectable `CommandRunner` seam.
- **`SSHTransport`** — a full pure-Go SSH transport over `golang.org/x/crypto/ssh` that honours the target's `config.ssh` (user, port, key/password auth, host-key-check, run-as/sudo, tty, tmpdir) and uploads-then-runs scripts and tasks.
- **`WinRMTransport`** — a full pure-Go WS-Management / MS-WSMV transport (SOAP over `net/http`) with `basic`, `negotiate` (NTLM via `github.com/Azure/go-ntlmssp`) and `ssl` (TLS client-certificate) auth, honouring `config.winrm`, that uploads-then-runs scripts and tasks on Windows.

## Executor & `apply`

An `Executor` runs a command, script or task across a set of targets, collecting a per-target `Result` into a `ResultSet`. For `apply` blocks and the YAML `resources` step, the manifest is compiled to a catalog via `go-puppet/puppet` and an apply `ResultSet` is produced (`Executor.ApplyCatalog`).

## Documented boundaries

- **`apply` execution** — `ApplyCatalog` compiles the catalog and reports the resources it *would* enforce per target; it does not remotely enforce resources (that needs a Puppet agent on the target, Bolt's `apply_prep` model — out of scope for the agentless core).
- **WinRM Kerberos / CredSSP** — only `basic`, `negotiate` and `ssl` auth are implemented; Kerberos (`realm`) is rejected with a clear error.
- **PuppetDB and other `_plugin` inventory resolvers** — not implemented.
