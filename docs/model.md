# Inventory, tasks & plans

This page walks the engine's capabilities. Each is complete unless explicitly marked as a documented deferral.

## Inventory (v2)

Targets, nested groups, per-group / per-target `config` / `facts` / `vars` / `features`, effective-value resolution through the hierarchy (deep merge, closest group wins, target overrides all), and target selection by name / alias / group / glob (`TargetsForQuery`).

## Tasks

Parses the Bolt task `*.json` metadata shape (typed `parameters`, `input_method`, `supports_noop`, `implementations`, `files`) and validates arguments against the declared parameter types.

## YAML plans

`plan.yaml` `parameters` plus an ordered list of `task` / `command` / `script` / `eval` / `plan` / `resources` / `message` steps with `targets`, per-step parameters and a `return` expression, executed by a step runner.

## Transports & executor

A `Transport` interface with a host-local `LocalTransport` driven through an injectable `CommandRunner` seam; an `Executor` runs a command, script or task across a set of targets, collecting a per-target `Result` into a `ResultSet`.

## SSH / WinRM & Puppet-language plans _( planned )_

Only `LocalTransport` ships (`Transport` is the extension point); SSH / WinRM transports, Puppet-language (`.pp`) plans, `apply` / `resources` steps and PuppetDB inventory references are documented deferrals.
