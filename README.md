# env

[![Go Reference](https://pkg.go.dev/badge/cattlecloud.net/go/env.svg)](https://pkg.go.dev/cattlecloud.net/go/env)
[![License](https://img.shields.io/github/license/cattlecloud/env?color=7C00D8&style=flat-square&label=License)](https://github.com/cattlecloud/env/blob/main/LICENSE)
[![Build](https://img.shields.io/github/actions/workflow/status/cattlecloud/env/ci.yaml?style=flat-square&color=0FAA07&label=Tests)](https://github.com/cattlecloud/env/actions/workflows/ci.yaml)

`env` provides a way to parse environment variables using a schema

### Getting Started

The `env` package can be added to a project by running:

```shell
go get cattlecloud.net/go/env@latest
```

```shell
import "cattlecloud.net/go/env"
```

### Examples

##### basics

Use `env.ParseOS` and `env.Schema` for conveniently extracting values from the
operating system environment variables.

```
var (
  home       string
  gomaxprocs int
)

err := env.ParseOS(env.Schema{
  "HOME":       env.String(&home, true),
  "GOMAXPROCS": env.Int(&gomaxprocs, true),
})
```

##### fallbacks

the `StringOr` and `IntOr` variants can be used to provide fallback values in
case the environment variables are not set.

```
err := env.ParseOS(env.Schema{
  "HOME":       env.StringOr(&home, "/doesnotexist"),
  "GOMAXPROCS": env.IntOr(&gomaxprocs, 8),
})
```

##### generics

The `Schema` parsing is compatible with generic types, so if you have custom types
like UUID backed by string or ID backed by integers, they can still be used as
targets for extraction.

```
type (
  uuid      string
  timestamp int64
)

var (
  id       uuid
  creation timestamp
)

err := env.ParseOS(env.Schema{
  "ID":       env.String(&id, true),
  "CREATED":  env.Int(&creation, true),
})
```

##### environments

In addition to `ParseOS`, other parsers include

- `ParseFile` - for parsing a file containing environment variable key-value pairs
- `ParseMap` - for parsing a Go map containing string key-value pairs
- `Parse[Environment]` for parsing arbitrary implementations of the `Environment` type

### License

The `cattlecloud.net/go/env` module is open source under the [BSD](LICENSE) license.
