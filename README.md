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
cd $GOPATH/src
go get -u github.com/mholt/caddy
go get -u github.com/caddyserver/builds
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
