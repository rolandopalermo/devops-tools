# Devops Tools
A curated list of development and operational tools for DevOps on DigitalOcean and Contabo with Linux-like operating systems.

## SSH

### SSH Keys
To create a new SSH key, we should follow the format `id_rsa_[project]_[module]_[env]`, for example for Sandbox Veronica API key, it should be named as `id_rsa_veronica_api_sbox`.
```bash
ssh-keygen -t rsa
```

## SCP
- Copy remote file to local system

```bash
scp <ssh-username>@<db-server>:<remote-path-to-backup-file.gz> <local-path-to-backup-file>
```

- Copy local file to remote system
```bash
scp backup-file <ssh-username>@<db-server>:<remote-path-to>
```