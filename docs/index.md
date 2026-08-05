# go-puppet-bolt

**Puppet Bolt core in pure Go — inventory, tasks, YAML and Puppet-language plans, and SSH / WinRM / local transports.**

go-puppet-bolt is a pragmatic, pure-Go (CGO_ENABLED=0) port of the core of Puppet Bolt, the agentless orchestrator. It parses Bolt inventory, tasks and plans and runs them over pluggable transports — with no Ruby runtime and no cgo, so it cross-compiles to every 64-bit Go target and WebAssembly and links into a static binary. Inventory v2 resolves effective config / facts / vars through the group hierarchy and selects targets by name, alias, group or glob; tasks validate arguments against declared parameter types; YAML plans run an ordered sequence of task / command / script / eval / plan / resources / message steps; Puppet-language (`.pp`) plans run through go-puppet/puppet with their plan functions dispatched to real targets; `apply` blocks compile a catalog and report; and an Executor runs work across targets — over a host-local, a full SSH or a full WinRM transport — into a ResultSet. Its non-stdlib dependencies are all pure Go (the fleet's YAML loader, golang.org/x/crypto/ssh, go-ntlmssp and go-puppet/puppet). 100% coverage, six arches.

- **[Why pure Go](why.md)** — a static, cgo-free engine for the Puppet stack.
- **[Inventory, tasks & plans](model.md)** — the capabilities in detail.
- **[Usage & API](api.md)** — the Go API and how to call it.
- **[Roadmap](roadmap.md)** — what is done and what is next.

## Guarantees

- **Pure Go, zero cgo.** Imports the Go standard library and a few pure-Go dependencies (go-ruby-yaml/yaml, golang.org/x/crypto/ssh, go-ntlmssp, go-puppet/puppet); cross-compiles to the six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x) and WebAssembly, linking into a static binary.
- **Faithful to Bolt's inventory v2, task metadata, YAML and Puppet-language plan shapes.**
- **100% test coverage** including error branches, enforced as a CI gate.
