# vps-config

VPS setup for personal stuff

## Run the configure playbook

### Required variables

| Variable | Purpose |
| --- | --- |
| `email_address` | SSL certificate and upgrade email |
| `certbot_ovh_application_key` | OVH API application key |
| `certbot_ovh_application_secret` | OVH API application secret |
| `certbot_ovh_consumer_key` | OVH API consumer key with access to the certificate domain's DNS zone |
| `ansible_port` | SSH port to connect to on the target system |
| `webdav_auth_password_hash` | Hashed password for WebDAV service, generated with `htpasswd -nB -C 12 $webdav_auth_username` (remove the *username:* part) |
| `foundry_vtt_username` | Foundry account username or email with a purchased software license |
| `foundry_vtt_password` | Foundry account password used to download the server |
| `foundry_vtt_admin_key` | Password protecting the Foundry administration interface |

Configure `ansible_host` and `ansible_user` in
[ansible/inventory/hosts.yml](ansible/inventory/hosts.yml) for the target VPS.

### Supply the variables securely

From the repository root, create an encrypted variables file:

```sh
ansible-vault create ~/vps-config.vault.yml
```

Enter a Vault password, then put the following YAML in the editor, replacing
all example values with your own:

```yaml
---
ansible_port: 22

email_address: "you@example.com"

certbot_domain: "example.com"
certbot_ovh_application_key: "YOUR_OVH_APPLICATION_KEY"
certbot_ovh_application_secret: "YOUR_OVH_APPLICATION_SECRET"
certbot_ovh_consumer_key: "YOUR_OVH_CONSUMER_KEY"

webdav_auth_password_hash: "HASHED_PASSWORD"

foundry_vtt_username: "YOUR_FOUNDRY_USERNAME"
foundry_vtt_password: "YOUR_FOUNDRY_PASSWORD"
foundry_vtt_admin_key: "YOUR_FOUNDRY_ADMIN_PASSWORD"
```

### Dependencies

Install the collections declared in [ansible/requirements.yml](ansible/requirements.yml),
then run the playbook from the Ansible directory so its configuration is loaded:

1. Install collections with `ansible-galaxy collection install -r requirements.yml`.
2. Run `ansible-playbook playbooks/configure.yml --extra-vars @group_vars/all.yml --extra-vars "@$HOME/vps-config.vault.yml" --ask-vault-pass`.
