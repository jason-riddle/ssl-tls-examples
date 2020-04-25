## Generate Root, Server, and Client Certs using CFSSL

cfssl is an easy-to-use tool for generating tls certificates. This example
generates a root, server, and client certificate.

### Requirements

- cfssl

### Usage

```
make root
make server
make client
```

The current directory will be populated with a root, server, and client
certificate.

### Validate

cfssl can also be used to validate certificates have required fields. For
example, if you wanted to validate the server.pem subject organization matched
`ACME`, you could first inspect the server.pem file as JSON and pipe to a json
processor tool like [`jq`](https://github.com/stedolan/jq).

```sh
# `.pem` contains the certificate, but to avoid exposure the key is not shown
SERVER_PEM_JSON=$(cfssl certinfo -cert server.pem | jq 'del(.pem)')
echo $SERVER_PEM_JSON
```

```json
{
  "subject": {
    "common_name": "CFSSL Server",
    "country": "US",
    "organization": "ACME",
    "organizational_unit": "Server",
    "names": [
      "US",
      "ACME",
      "Server",
      "CFSSL Server"
    ]
  },
  "issuer": {
    "common_name": "CFSSL Root CA",
    "country": "US",
    "organization": "ACME",
    "organizational_unit": "ROOT CA",
    "names": [
      "US",
      "ACME",
      "ROOT CA",
      "CFSSL Root CA"
    ]
  },
  "serial_number": "291724464001278347162500109903921170236547384230",
  "sans": [
    "127.0.0.1"
  ],
  "not_before": "2020-02-21T06:13:00Z",
  "not_after": "2021-02-20T06:13:00Z",
  "sigalg": "SHA256WithRSA",
  "authority_key_id": "A3:53:11:38:1B:A3:F4:4F:CF:1C:B8:97:37:8F:DC:30:03:23:87:9E",
  "subject_key_id": "71:F9:17:3C:8A:D0:22:12:24:2B:78:E9:C0:A0:FE:8C:81:4A:06:B9"
}
```

Then inspect subject -> organization and assert it matches `ACME`.

```sh
got=$(echo $SERVER_PEM_JSON | jq -r '.subject.organization')
want='ACME'
[ "$got" = "$want" ] && echo equal || echo not-equal
# equal
```
