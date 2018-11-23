## USAGE

```yaml
  - role: roles/pludoni.caddy
    caddyfile: "{{lookup('template', '{{playbook_dir}}/files/caddyfile.j2')}}"
```

compile Caddy from source:


```
cd $GOPATH/src
go get -u github.com/mholt/caddy
go get -u github.com/caddyserver/builds
```

## Remove sponsors

```
cd $GOPATH/src/github.com/mholt/caddy
vim caddyhttp/httpserver/server.go
```

## Add Plugins:

```
cd $GOPATH/src
go get github.com/pyed/ipfilter
go get github.com/xuqingfeng/caddy-rate-limit

cd $GOPATH/src/github.com/mholt/caddy
vim caddy/caddymain/run.go
```

Add in import statement after comment

```
"github.com/pyed/ipfilter"
"github.com/xuqingfeng/caddy-rate-limit"
```

## Build

```
cd $GOPATH/src/github.com/mholt/caddy/caddy
go run build.go -goos=linux -goarch=amd64
```

