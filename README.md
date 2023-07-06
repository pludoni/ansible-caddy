## USAGE

Simple role to run and configure Caddy as a reverse proxy/static-php site (php must be installed separately).

```yaml
  - role: roles/pludoni.caddy
    caddy_user: www-data
    # to install newer Caddy versions, edit download url
    # caddy_download_url: "https://github.com/caddyserver/caddy/releases/download/v2.7.0-beta.2/caddy_2.7.0-beta.2_linux_amd64.tar.gz"
    # To use letsencrypt staging ACME server:
    # caddy_tls_acme_server: https://acme-staging-v02.api.letsencrypt.org/directory
    caddy_letsencrypt_email: 'admin@mydomain.de'
    caddy_routings:
      - name: first_website.com
        # only activate letsencrypt when the domain name completely resolves to this host
        # or set to 'no' to enable self signed certs
        letsencrypt: yes
        # what domains should this vhost respond to?
        domains:
          - www.first_website.com
          - first_website.com
          - another-alias.com
        # SEO redirects
        redirects:
          - from: mydomain.de
            to:  www.mydomain.de

        # now, what to do with the domain?
        # 1. reverse-Proxy to somewhere else, with all good settings, like websocket, transparency, etc.
        target: '10.10.13.2:3000'

        # 2. static/php hosting?
        root: /home/pages/www.mysite.com
        # uses php7.2 fastcgi socket in /run/php/php-7.2-fpm.sock, must be installed separately
        php: 7.2

        # to only allow access from IPs (internal services, etc.)
        # allowed_ips: "10.0.0.0/8 123.234.123.123"
        # block_ips: "123.123.123.123 23.23.23.23"

        # basic-auth: Add http basic auth (e.g. test servers, internal servers etc.):
        basic_auth:
          username: 'admin'
           # create with: caddy hash-password
          password_hash: 'put-the-bcrypted-password-hash-here'

        # if supporting big file uploads change those
        # NOTE: THIS MUST BE THE SAME VALUES FOR ALLE HOSTS, THE SMALLEST VALUES OF ALL HOSTS WILL WIN
        read_timeout: 1m
        write_timeout: 1m
        header_timeout: 1m
        idle_timeout: 1m


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
