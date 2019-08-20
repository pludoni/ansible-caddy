## USAGE

Simple role to run Caddy as a reverse proxy.

```yaml
  - role: roles/pludoni.caddy
    caddy_routings:
      - name: first_website.com
        domains:
          - www.first_website.com
          - first_website.com
          - another-alias.com
        target: '10.10.13.2:3000'
        letsencrypt: yes

```

## Update compile Caddy from source with plugins:

```
mkdir caddy
cd caddy
vim main.go
```

```
cd package main

import (
	"github.com/caddyserver/caddy/caddy/caddymain"

  _ "github.com/pyed/ipfilter"
  _ "github.com/xuqingfeng/caddy-rate-limit"
	// plug in plugins here, for example:
	// _ "import/path/here"
)

func main() {
	// optional: disable telemetry
	// caddymain.EnableTelemetry = false
	caddymain.Run()
}
```

```
export GO111MODULE=on
go mod init caddy
go get github.com/caddyserver/caddy
go get github.com/pyed/ipfilter
go get github.com/xuqingfeng/caddy-rate-limit
GOOS=linux GOARCH=amd64 go build -o caddy_amd64
GOOS=linux GOARCH=386 go build -o caddy_386
```

## Build

```
cd $GOPATH/src/github.com/mholt/caddy/caddy
go run build.go -goos=linux -goarch=amd64
```
