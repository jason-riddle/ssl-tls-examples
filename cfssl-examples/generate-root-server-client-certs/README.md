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
