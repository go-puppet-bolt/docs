# Usage & API

```go
import "github.com/go-puppet-bolt/bolt"
```

```go
inv, _ := bolt.ParseInventory(invBytes)
targets, _ := bolt.TargetsForQuery(inv, "web,db1.example.com")

exec := &bolt.Executor{
    Transport: bolt.NewLocalTransport(),
    Inventory: inv,
}
rs := exec.RunCommand(targets, "uptime")
fmt.Println(rs.Ok(), rs.Names())
```

`ParseInventory` reads an `inventory.yaml` v2 document; `TargetsForQuery` selects targets by name, alias, group or glob and resolves their effective config / facts / vars through the group hierarchy. Tasks parse from their `*.json` metadata and validate arguments against declared types; YAML plans parse and run through a step runner, and Puppet-language (`.pp`) plans run through `Executor.RunPuppetPlan`. An `Executor` runs a command, script or task across a target set via a `Transport` — a host-local `LocalTransport`, a full pure-Go `SSHTransport` (over `golang.org/x/crypto/ssh`) or a full pure-Go `WinRMTransport` (WS-Management over `net/http`) — collecting a `ResultSet`.

Over SSH, running a Puppet-language plan:

```go
exec := &bolt.Executor{
    Transport:  bolt.NewSSHTransport(),
    Inventory:  inv,
    TaskLoader: loadTask, // resolve a task by name
}
src := `plan deploy(TargetSpec $nodes) {
  run_command('systemctl stop app', $nodes)
  run_task('app::deploy', $nodes, {'version' => '1.2.3'})
  return apply($nodes) {
    service { 'app': ensure => running, enable => true }
  }
}`
res, err := exec.RunPuppetPlan(src, "deploy", map[string]any{"nodes": "web"})
```

## Command line & builds

The library is `CGO_ENABLED=0` pure Go. Cross-compile it anywhere:

```sh
GOOS=linux   GOARCH=arm64    go build ./...
GOOS=js      GOARCH=wasm     go build ./...
```

It builds and tests on all six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x).
