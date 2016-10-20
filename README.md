# Ansible Role: Consul TLS

[![Build Status](https://travis-ci.org/adriano-di-giovanni/ansible-role-consul-tls.svg?branch=master)](https://travis-ci.org/adriano-di-giovanni/ansible-role-consul-tls)

Generates self-signed OpenSSL certificates to be used by Consul for [RPC encryption with TLS](https://www.consul.io/docs/agent/encryption.html).

## Requirements

* OpenSSL

## Role Variables

```yaml
# Where to put output certificates
consul_tls_out_dir: ~/.consul_tls

# Number of days certificates are valid for
consul_tls_default_days: '{{ 365 * 10 }}'

# Datacenter for consul cluster using generated certificates
consul_datacenter: default
```

## Dependencies

None.

## Example Playbook

This role is meant to be run locally.

```yaml
---
- hosts: localhost
  connection: local
  roles:
    - role: adigiovanni.consul_tls
      consul_tls_out_dir: /tmp/consul_tls
      consul_datacenter: dc1
```

Run the example playbook to create

* `ca.crt.pem`
* `dc1.consul.key.pem`
* `dc1.consul.crt.pem`

in `/tmp/consul_tls`.

Upload the files to consul nodes and change configuration as follows:

```json
{
  "ca_file": "/opt/consul/tls/ca.crt.pem",
  "cert_file": "/opt/consul/tls/dc1.consul.crt.pem",
  "key_file": "/opt/consul/tls/dc1.consul.key.pem",
  "verify_incoming": true,
  "verify_outgoing": true,
  "verify_server_hostname": true
}
```

## License

MIT

## Author Information

Adriano Di Giovanni
