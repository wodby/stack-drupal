# Drupal stacks for Wodby

Deploy Drupal on Kubernetes with Wodby. This repository contains the stack
manifests used by the public Drupal stack entries in the Wodby catalog.

- [Drupal 11 stack in the Wodby catalog](https://wodby.com/stacks/drupal11)
- [Drupal 10 stack in the Wodby catalog](https://wodby.com/stacks/drupal10)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Stack entries

### Drupal 11

| Component / service | Default configuration |
| --- | --- |
| PHP<br>`drupal11-php` | required; enabled by default; versions: `8.4` by default; also available: `8.5`, `8.3`; volumes: `files` 10 GB; links: `db` → `mariadb`, `files` → `files-nfs`, `solr` → `solr`, `redis` → `valkey`, `sendmail` → `mailpit`; derivatives: `sshd` → `drupal11-php-sshd`; 1 workload overrides |
| Vinyl<br>`drupal-vinyl` | optional; enabled by default; links: `backend` → `nginx` |
| Nginx<br>`drupal11-nginx` | required; enabled by default; links: `backend` → `php` |
| MariaDB<br>`mariadb` | optional; enabled by default; volumes: `data` 10 GB; 1 environment overrides |
| Files NFS Storage (`files-nfs`)<br>`nfs-provisioner` | optional; enabled by default; volumes: `data` 15 GB |
| Solr<br>`solr` | optional; disabled by default; links: `zookeeper` → `zookeeper` |
| Valkey<br>`valkey` | optional; enabled by default |
| Gotenberg<br>`gotenberg` | optional; disabled by default |
| Mailpit<br>`mailpit` | optional; enabled by default |
| OpenSMTPD<br>`opensmtpd` | optional; disabled by default |
| ZooKeeper (Solr) (`zookeeper`)<br>`zookeeper` | optional; disabled by default |
| PostgreSQL (`postgres`)<br>`postgres` | optional; disabled by default; volumes: `data` 10 GB |
| Cloud MariaDB (`cloud-mariadb`)<br>`cloud-mariadb` | optional; disabled by default; versions: `10.3` by default |
| Cloud MySQL (`cloud-mysql`)<br>`cloud-mysql` | optional; disabled by default; versions: `8` by default |

Manifest: [`11/stack.yml`](11/stack.yml)

### Drupal 10

| Component / service | Default configuration |
| --- | --- |
| PHP<br>`drupal10-php` | required; enabled by default; versions: `8.3` by default; also available: `8.2`, `8.1`; volumes: `files` 20 GB; links: `db` → `mariadb`, `files` → `files-nfs`, `solr` → `solr`, `redis` → `valkey`, `sendmail` → `opensmtpd`; derivatives: `sshd` → `drupal10-php-sshd`; 1 workload overrides |
| Vinyl<br>`drupal-vinyl` | optional; enabled by default; links: `backend` → `nginx` |
| Nginx<br>`drupal10-nginx` | required; enabled by default; links: `backend` → `php` |
| MariaDB<br>`mariadb` | optional; enabled by default; volumes: `data` 10 GB; 1 environment overrides |
| Files NFS Storage (`files-nfs`)<br>`nfs-provisioner` | optional; enabled by default; volumes: `data` 25 GB |
| Solr<br>`solr` | optional; disabled by default; links: `zookeeper` → `zookeeper` |
| Valkey<br>`valkey` | optional; enabled by default |
| OpenSMTPD<br>`opensmtpd` | optional; enabled by default |
| Gotenberg<br>`gotenberg` | optional; enabled by default |
| ZooKeeper (Solr) (`zookeeper`)<br>`zookeeper` | optional; disabled by default |
| PostgreSQL (`postgres`)<br>`postgres` | optional; disabled by default; volumes: `data` 10 GB |
| Cloud MariaDB (`cloud-mariadb`)<br>`cloud-mariadb` | optional; disabled by default; versions: `10.3` by default |
| Cloud MySQL (`cloud-mysql`)<br>`cloud-mysql` | optional; disabled by default; versions: `5.7` by default; also available: `8` |

Manifest: [`10/stack.yml`](10/stack.yml)

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Use this stack

The simplest path is to add the public stack from the Wodby catalog. Review the
enabled services, versions, storage sizes, links, and other defaults when
creating your app.

To maintain your own version of this stack:

1. Fork this repository.
2. Edit the stack manifest.
3. Import the repository as a
   [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

Wodby imports the manifest from the selected Git branch or tag and creates a new
stack revision when the Git-backed stack is updated.

## Customize the stack

Common changes include selecting different service versions, enabling or
disabling optional components, changing persistent volume sizes, adjusting
stack-level environment and workload overrides, or replacing one linked service
with another compatible service.

When replacing or renaming a stack service, update every corresponding
`services[].links` target and derivative reference. Stack-local names and
referenced service names are distinct identifiers and do not need to match.

Validate customized manifests with the Wodby CLI before importing them:

```bash
wodby stack validate-manifest 11/stack.yml --org <org-id>
wodby stack validate-manifest 10/stack.yml --org <org-id>
```

See the [stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)
for every supported field and the [managed services
index](https://github.com/wodby/services) for available service references.
