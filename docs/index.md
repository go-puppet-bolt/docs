# go-puppet-bolt

**Puppet Bolt core in pure Go — inventory, tasks and YAML plans with a transport abstraction.**

go-puppet-bolt is a pragmatic, pure-Go (CGO_ENABLED=0) port of the core of Puppet Bolt, the agentless orchestrator. It parses Bolt inventory, tasks and YAML plans and runs them through a pluggable transport — with no Ruby runtime and no cgo, so it cross-compiles to every 64-bit Go target and WebAssembly and links into a static binary. Inventory v2 resolves effective config / facts / vars through the group hierarchy and selects targets by name, alias, group or glob; tasks validate arguments against declared parameter types; YAML plans run an ordered sequence of task / command / script / eval / plan steps; and an Executor runs work across targets into a ResultSet. Its only dependency is the fleet's pure-Go YAML loader. 100% coverage, six arches.

- **[Why pure Go](why.md)** — a static, cgo-free engine for the Puppet stack.
- **[Inventory, tasks & plans](model.md)** — the capabilities in detail.
- **[Usage & API](api.md)** — the Go API and how to call it.
- **[Roadmap](roadmap.md)** — what is done and what is next.

## Guarantees

- **Pure Go, zero cgo.** Imports the Go standard library plus the fleet's pure-Go YAML loader (go-ruby-yaml/yaml); cross-compiles to the six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x) and WebAssembly, linking into a static binary.
- **Faithful to Bolt's inventory v2, task metadata and YAML plan shapes.**
- **100% test coverage** including error branches, enforced as a CI gate.
