# Drupal application stack for Kubernetes on Wodby

Deploy Drupal applications on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for Drupal.

- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Start from a template

Use one of the compatible source templates exposed by this stack's services to
start with Wodby CI build configuration:

- [Drupal CMS](https://github.com/wodby/drupal-cms-template)
- [Vanilla Drupal](https://github.com/wodby/drupal-vanilla)

## Service definitions

- [PHP (Drupal 11) service](https://github.com/wodby/service-drupal-php)
- [Vinyl (Drupal) service](https://github.com/wodby/service-drupal-vinyl)
- [Nginx (Drupal 11) service](https://github.com/wodby/service-drupal-nginx)
- [MariaDB service](https://github.com/wodby/service-mariadb)
- [Ganesha NFS provisioner service](https://github.com/wodby/service-nfs-provisioner)
- [Solr service](https://github.com/wodby/service-solr)
- [Valkey service](https://github.com/wodby/service-valkey)
- [Gotenberg service](https://github.com/wodby/service-gotenberg)
- [Mailpit service](https://github.com/wodby/service-mailpit)
- [OpenSMTPD service](https://github.com/wodby/service-opensmtpd)
- [ZooKeeper service](https://github.com/wodby/service-zookeeper)
- [PostgreSQL service](https://github.com/wodby/service-postgres)
- [Cloud MariaDB service](https://github.com/wodby/service-cloud-mariadb)
- [Cloud MySQL service](https://github.com/wodby/service-cloud-mysql)

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

## Deploy this stack

Start from [Drupal CMS](https://github.com/wodby/drupal-cms-template), [Vanilla Drupal](https://github.com/wodby/drupal-vanilla), or connect your own
compatible source repository.

Review service versions, storage, links, and optional components when creating
the application. The same stack can be reused across development, staging, and
production environments.

## Maintain a custom version

1. Fork this repository.
2. Edit the stack manifest.
3. Import the repository as a [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target
and derivative reference. Stack-local names and referenced service names are
distinct identifiers.

Validate the manifests with:

```bash
wodby stack validate-manifest 11/stack.yml --org <org-id>
wodby stack validate-manifest 10/stack.yml --org <org-id>
```

See the [stack manifest reference](https://wodby.com/docs/2.0/stacks/template/) and the [managed services index](https://github.com/wodby/services).
