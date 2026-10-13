# Commonly stale patterns

Places where agents (and old tutorials) reliably produce outdated config. This list is **a prompt to go check**, not an authority — it ages too. Before using any "current" column, confirm it against the docs for the pinned version.

## Linux admin

| Stale | Current direction | Check |
|---|---|---|
| `apt-key add` / `apt-key adv` | Keyring in `/etc/apt/keyrings/`, `signed-by=` in the source (deb822 `.sources` on newer releases) | vendor's current install page, `man sources.list` |
| `add-apt-repository` for third-party repos without key pinning | Explicit keyring + `signed-by` | same |
| `yum` | `dnf` on RHEL 8+/Fedora | `dnf --help` |
| `service x restart`, `chkconfig`, init scripts | `systemctl`, unit files, drop-ins via `systemctl edit` | `man systemd.unit` |
| `ifconfig`, `netstat`, `route` | `ip`, `ss` | `man ip`, `man ss` |
| Hand-written `iptables` rules on modern distros | nftables / distro firewall frontend (firewalld, ufw) | distro docs |
| `ntpd` | chrony / systemd-timesyncd | distro docs |
| `sudo pip install` into system Python | venv / pipx / distro packages (PEP 668 "externally managed") | `pip --help`, distro docs |
| `/etc/network/interfaces` on Ubuntu | netplan | `man netplan` |
| EOL distro base (CentOS 7/8, Ubuntu 18.04, Debian 10…) | supported release | https://endoflife.date |

## Containers

| Stale | Current direction | Check |
|---|---|---|
| `docker-compose` (v1, Python) | `docker compose` plugin; top-level `version:` key is obsolete | `docker compose --help` |
| `MAINTAINER` in Dockerfile | `LABEL org.opencontainers.image.*` | Dockerfile reference |
| Classic builder assumptions | BuildKit / `docker buildx` | `docker buildx --help` |
| `latest` / unpinned base images | pinned tag or digest | registry |

## Kubernetes / Helm

| Stale | Current direction | Check |
|---|---|---|
| `extensions/v1beta1`, `apps/v1beta*`, `networking.k8s.io/v1beta1` Ingress, `policy/v1beta1` PDB, `batch/v1beta1` CronJob | GA `v1` APIs | `kubectl api-resources`, deprecated API migration guide |
| PodSecurityPolicy | Pod Security Admission (namespace labels) | k8s docs for cluster version |
| Helm 2 / Tiller, `helm init` | Helm 3 | `helm version` |
| `kubectl run --generator=…` | `kubectl create deployment` / manifests | `kubectl run --help` |
| Chart values from memory | `helm show values <chart> --version <v>` | — |

## Terraform / OpenTofu

| Stale | Current direction | Check |
|---|---|---|
| Inline `acl`/`versioning`/`server_side_encryption_configuration`/`lifecycle_rule` on `aws_s3_bucket` | separate `aws_s3_bucket_*` resources (AWS provider v4+) | provider upgrade guides |
| Provider args from memory for any fast-moving provider (azurerm, aws, google, proxmox…) | the registry page at the pinned version, or `terraform providers schema -json` | — |
| `terraform state mv` / `rm` for refactors | `moved {}` / `removed {}` / `import {}` blocks | Terraform docs for pinned version |
| `count` for keyed collections | `for_each` | — |
| No `required_providers` / no lock file | pinned providers + committed `.terraform.lock.hcl` | — |

## Ansible

| Stale | Current direction | Check |
|---|---|---|
| Short module names (`apt`, `copy`, `ufw`) | FQCN (`ansible.builtin.apt`, `community.general.ufw`) | `ansible-doc <fqcn>` |
| `include:` | `include_tasks` / `import_tasks` / `import_playbook` | — |
| `with_items` etc. everywhere | `loop` (with_* still works; follow repo convention) | — |
| `apt_key` module | `get_url` keyring + `deb822_repository` / `signed-by` | `ansible-doc ansible.builtin.deb822_repository` |
| `sudo: yes` | `become: true` | — |

## GitHub Actions

| Stale | Current direction | Check |
|---|---|---|
| `::set-output`, `::save-state`, `::set-env`, `::add-path` | `$GITHUB_OUTPUT`, `$GITHUB_STATE`, `$GITHUB_ENV`, `$GITHUB_PATH` | GitHub workflow commands docs |
| Old majors of `actions/checkout`, `setup-node`, `cache`, `upload-artifact`… (runtime deprecations) | current major from the action's releases page | `gh release list -R actions/checkout` |
| Mutable `@vN` tags on third-party actions | full commit SHA pin with version comment | action's releases |
| Implicit token permissions | explicit `permissions:` | — |
| Long-lived cloud keys in secrets | OIDC federation | cloud provider docs |
| Retired runner images (`ubuntu-18.04`, `ubuntu-20.04`, …) | currently supported labels | runner-images repo |

## Windows / PowerShell / Microsoft cloud

| Stale | Current direction | Check |
|---|---|---|
| `wmic`, `Get-WmiObject` | `Get-CimInstance` | `Get-Help Get-CimInstance` |
| `AzureRM` module | `Az` | — |
| `MSOnline`, `AzureAD` modules | Microsoft Graph PowerShell SDK | Microsoft Learn retirement notices |
| Windows PowerShell 5.1 assumptions in new scripts | PowerShell 7+ where available (check `$PSVersionTable`) | — |
| `netsh advfirewall` in scripts | `NetSecurity` cmdlets (`New-NetFirewallRule`) | `Get-Help New-NetFirewallRule` |

## Generic tells that you're on stale ground

- You remember the answer from a tutorial with a year in the URL.
- The tool printed a deprecation warning and you kept going.
- The install instructions involve `curl | sh`, a hard-coded version number, or a key fingerprint you didn't fetch.
- The version you're targeting is newer than anything you remember release notes for.
