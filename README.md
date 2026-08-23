# Drupal application stack for Kubernetes on Wodby

Deploy Drupal applications on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for Drupal.

<!-- wodby:generated:start -->

## Stack contract

- [Drupal stack on Wodby](https://wodby.com/stacks/drupal)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Drupal stack guide](https://wodby.com/docs/2.0/stacks/catalog/drupal/)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Start from a boilerplate

Use one of the compatible boilerplates exposed by this stack's services to
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
| PHP<br>`php` | required; enabled by default; volumes: `files` 10 GB; links: `db` → `mariadb`, `files` → `files-nfs`, `solr` → `solr`, `redis` → `valkey`, `sendmail` → `mailpit` |
| Vinyl<br>`vinyl` | optional; enabled by default; links: `backend` → `nginx` |
| Nginx<br>`nginx` | required; enabled by default; links: `backend` → `php` |
| MariaDB<br>`mariadb` | optional; enabled by default; volumes: `data` 10 GB |
| Files NFS Storage<br>`files-nfs` | optional; enabled by default; volumes: `data` 15 GB |
| Solr<br>`solr` | optional; disabled by default; links: `zookeeper` → `zookeeper` |
| Valkey<br>`valkey` | optional; enabled by default |
| Gotenberg<br>`gotenberg` | optional; disabled by default |
| Mailpit<br>`mailpit` | optional; enabled by default |
| OpenSMTPD<br>`opensmtpd` | optional; disabled by default |
| ZooKeeper (Solr)<br>`zookeeper` | optional; disabled by default |
| PostgreSQL<br>`postgres` | optional; disabled by default; volumes: `data` 10 GB |
| Cloud MariaDB<br>`cloud-mariadb` | optional; disabled by default |
| Cloud MySQL<br>`cloud-mysql` | optional; disabled by default |

Manifest: [`11/stack.yml`](11/stack.yml)

### Drupal 10

| Component / service | Default configuration |
| --- | --- |
| PHP<br>`php` | required; enabled by default; volumes: `files` 20 GB; links: `db` → `mariadb`, `files` → `files-nfs`, `solr` → `solr`, `redis` → `valkey`, `sendmail` → `opensmtpd` |
| Vinyl<br>`vinyl` | optional; enabled by default; links: `backend` → `nginx` |
| Nginx<br>`nginx` | required; enabled by default; links: `backend` → `php` |
| MariaDB<br>`mariadb` | optional; enabled by default; volumes: `data` 10 GB |
| Files NFS Storage<br>`files-nfs` | optional; enabled by default; volumes: `data` 25 GB |
| Solr<br>`solr` | optional; disabled by default; links: `zookeeper` → `zookeeper` |
| Valkey<br>`valkey` | optional; enabled by default |
| OpenSMTPD<br>`opensmtpd` | optional; enabled by default |
| Gotenberg<br>`gotenberg` | optional; enabled by default |
| ZooKeeper (Solr)<br>`zookeeper` | optional; disabled by default |
| PostgreSQL<br>`postgres` | optional; disabled by default; volumes: `data` 10 GB |
| Cloud MariaDB<br>`cloud-mariadb` | optional; disabled by default |
| Cloud MySQL<br>`cloud-mysql` | optional; disabled by default |

Manifest: [`10/stack.yml`](10/stack.yml)

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifests

```bash
wodby stack validate-manifest 11/stack.yml --org <org-id>
wodby stack validate-manifest 10/stack.yml --org <org-id>
```

<!-- wodby:generated:end -->

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
