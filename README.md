# Ansible Role: Cloudflare SSL

An Ansible role that configures SSL/TLS certificates for Cloudflare Authenticated Origin Pulls. This role ensures your origin server only accepts requests from Cloudflare by validating the Cloudflare client certificate.

## Features

- Installs OpenSSL.
- Sets up standard SSL directories (`/etc/ssl/cloudflare`, `/etc/ssl/private`, `/etc/ssl/certs`).
- Retrieves the Cloudflare Origin CA certificate from AWS Secrets Manager.
- Generates a private key and self-signed certificate for the origin server.
- Generates DH parameters (2048-bit).
- Triggers Nginx restart upon certificate or configuration changes.

## Requirements

- **Ansible:** 2.9 or higher.
- **Collections:** `amazon.aws` (if using AWS Secrets Manager).
- **Target OS:** Amazon Linux 2023.
- **Web Server:** Nginx (expected handler: `restart nginx`).

## Role Variables

Variables and their default values are defined in `defaults/main.yml`:

| Variable | Default | Description |
|----------|---------|-------------|
| `ssl_hostname` | `{{ inventory_hostname }}` | The common name for the certificate. |
| `cloudflare_origin_cert_secret_name` | `""` | The AWS Secrets Manager secret name containing the Cloudflare Origin CA certificate. |
| `ssl_aws_region` | `us-east-1` | AWS region where the secret is stored. |

*Note: Ensure `cloudflare_origin_cert_secret_name` is provided to successfully retrieve the Cloudflare CA cert.*

## Dependencies

None.

## Example Playbook

```yaml
- hosts: webservers
  become: yes
  roles:
    - role: ansible-role-cloudflare-ssl
      vars:
        ssl_hostname: "api.example.com"
        cloudflare_origin_cert_secret_name: "prod/cloudflare/origin-ca"
```

## License

MIT

## Author Information

This role was maintained by the DevOps Team.
