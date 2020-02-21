## Certificate Management using Vault

vault is an secret storage solution from Hashicorp. It can be used for storing,
rotating, generating, and auditing secrets. Once started, vault is available
via an HTTP API. This example is a walkthrough of Hashicorp's guide on
certificate management using Vault available here:
https://www.hashicorp.com/blog/certificate-management-with-vault

Note: This is not a production ready setup, this is just for quick experimentation.

### Requirements

- docker
- docker-compose
- vault (will only be using the cli)

### Prep Setup

Open two terminals. In terminal 1, start vault by running

```
make
```

In terminal 2, set the following environment variables

```
export VAULT_ADDR=http://127.0.0.1:8200
export VAULT_TOKEN=root
```

and verify vault can be reached with `vault status`

```
vault status

Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.3.2
Cluster Name    vault-cluster-93ccf4b8
Cluster ID      a3a54e4f-6252-4ed6-9f96-fce0c845d237
HA Enabled      false
```

### Create Root CA

Using terminal 2.

Enable the pki backend.

```
vault secrets enable pki
```

Update the Global TTL to 1 year.

```
vault secrets tune -max-lease-ttl=8760h pki
```

Generate the self-signed root cert

```
vault write -format=json pki/root/generate/exported \
    common_name=my-website.com \
    ttl=8760h > root_cert.json
```

and extract the files from json response.

```
jq -r .data.certificate root_cert.json > ca.pem
jq -r .data.issuing_ca root_cert.json > issuing_ca.pem
jq -r .data.private_key root_cert.json > ca-key.pem
```
