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

`ParseInventory` reads an `inventory.yaml` v2 document; `TargetsForQuery` selects targets by name, alias, group or glob and resolves their effective config / facts / vars through the group hierarchy. Tasks parse from their `*.json` metadata and validate arguments against declared types; YAML plans parse and run through a step runner. An `Executor` runs a command, script or task across a target set via a `Transport` (host-local `LocalTransport` ships; the interface is the extension point), collecting a `ResultSet`.

## Command line & builds

The library is `CGO_ENABLED=0` pure Go. Cross-compile it anywhere:

```sh
GOOS=linux   GOARCH=arm64    go build ./...
GOOS=js      GOARCH=wasm     go build ./...
```

It builds and tests on all six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x).
