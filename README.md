# Webships Project

A ready-to-use Drupal API project.

The site serves a JSON:API from `/api`, with authentication, authorization and
OpenAPI documentation rendered by Swagger UI at `/api-docs`. Everything comes
from drupal.org.

The project requires only the [Webships](https://www.drupal.org/project/webships)
installer and two site templates. The installer lists them:

- [Webships Starter](https://www.drupal.org/project/webships_starter): the
  default site template for Webships, with the apps and organizations content
  model and its API resources.
- [WebAPI Starter](https://www.drupal.org/project/webapi_starter): the basic
  default Drupal API site template, with the standard content model.

Both use the Drupal core administration theme, for the back end and the front
end.

## Create a project

Composer runs inside DDEV, so nothing is needed on your machine but DDEV itself:
DDEV 1.25 or newer, for PHP 8.5 (DDEV 1.24 does not offer it).

```shell
mkdir my-api && cd my-api
ddev config --project-type=drupal --docroot=web
ddev start
ddev composer create-project drupal/webships_project:~12.0
ddev restart
```

`ddev restart` picks up the `.ddev/config.yaml` the template ships (PHP 8.5,
Node.js 24, MariaDB 10.11), which lands in the project during
`ddev composer create-project` and replaces the one `ddev config` wrote. The
project name is not in that file: DDEV takes it from the directory, so the site
is at `https://<directory>.ddev.site`.

## Install the site

Open the site and follow the installer, where you pick the site template:

```shell
ddev launch
```

Or install from the command line, with the Webships Starter site template:

```shell
ddev drush site:install ../recipes/webships_starter -y --account-name=webmaster --site-name="My API"
```

The path is relative to the Drupal root. Use `webships_starter` or `webapi_starter`.

## Project layout

| Path | Content |
|---|---|
| `web/` | Drupal root (core, contrib modules and themes) |
| `web/profiles/contrib/webships` | The Webships installer |
| `web/libraries/swagger-ui` | Swagger UI (`swagger-api/swagger-ui`): the API documentation loads its `dist` folder |
| `recipes/webships_starter` | The Webships Starter site template |
| `recipes/webapi_starter` | The WebAPI Starter site template |
| `vendor/` | Composer dependencies, Drush |

## What is inside

- Drupal core 11.4. Drupal 12 (beta, PHP 8.5) is allowed too; see below.
- [Web API](https://www.drupal.org/project/webapi): JSON:API from `/api`,
  read-only, with OAuth 2.0 and consumers.
- OpenAPI documents for JSON:API and REST, rendered with Swagger UI.

## Drupal 12 (beta)

The template installs the latest Drupal 11 by default. It also allows Drupal 12,
which needs PHP 8.5, the PHP version of the template's DDEV configuration. To try it:

```shell
ddev composer require --no-update drupal/core-recommended:^12 drupal/search:^1
ddev composer update -W
```

Search left Drupal core in Drupal 12; Webships Starter uses it, so the contrib
`drupal/search` module comes with the upgrade.

Contributed modules that don't declare Drupal 12 support yet are allowed by the
`mglaman/composer-drupal-lenient` plugin (`extra.drupal-lenient.allowed-list`).
Some of them still fail on Drupal 12.

## Write operations

JSON:API is read-only. Turn write operations on at
`/admin/config/services/jsonapi` only when the site needs them, and rebuild the
cache afterwards.

## Cross-origin requests

CORS lives in `sites/default/services.yml` under `cors.config`. No module can
ship it, so set it per site.

## Maintainers

- Rajab Natshah: [RajabNatshah](https://www.drupal.org/u/rajabnatshah)
