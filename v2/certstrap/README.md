# Certstrap

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Install tools](#install-tools)
  - [caddy](#caddy)
  - [certstrap](#certstrap)
  - [certigo](#certigo)
- [Generate certificates](#generate-certificates)
- [Generate Caddyfile](#generate-caddyfile)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Install tools

### caddy

```sh
brew install caddy
```

### certstrap

```sh
mkdir -p bin/

# Brew
brew install certstrap

# Darwin, Non-brew
curl -fsSL "https://github.com/square/certstrap/releases/download/v1.2.0/certstrap-1.2.0-darwin-amd64" -o bin/certstrap
xattr -d com.apple.quarantine bin/certstrap
chmod +x bin/certstrap
bin/certstrap --help
```

### certigo

```sh
mkdir -p bin/

# Brew
brew install certigo
```

## Generate certificates

```sh
mkdir -p certs/
certstrap --depot-path certs init --common-name root_ca --expires "10 years" --passphrase ""
certstrap --depot-path certs request-cert --common-name client --ip "127.0.0.1" --passphrase ""
certstrap --depot-path certs sign client --CA root_ca
rm -rf certs/*.csr certs/*.crl
```

## Generate Caddyfile

```sh
cat <<EOF > Caddyfile
localhost:8080 {
	tls certs/client.crt certs/client.key
	respond "I am 8080"
}
EOF

caddy run

open 'https://localhost:8080'
```
