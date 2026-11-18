# Webships Project — agent guide

Composer project template (`drupal/webships_project`, type `project`) for an API
site: Drupal core 11.4, the `webships` installer profile and two site template
recipes. Part of the Webship Workspace (`~/workspace/products`): DDEV only.

## Rules

- `require` holds only the Drupal project scaffolding, the `webships` installer,
  the site templates `drupal/webships_starter` and `drupal/webapi_starter`, and
  the Swagger UI library. No other packages: each site template brings its own
  dependencies.
- Everything comes from drupal.org. The Swagger UI library is the one
  exception: `swagger-api/swagger-ui` (the package Web API requires), from a
  `package` repository as a `drupal-library`, installed once to
  `web/libraries/swagger-ui`, whose `dist` folder the API documentation loads.
  It belongs here rather than in a module: repositories and `installer-paths`
  only work in a root package.
- The site templates are kept in `recipes/` (not unpacked), so the installer
  finds them without Composer.
- Both site templates use the Drupal core administration theme, for the back end
  and the front end.
- JSON:API ships read-only. Never set `read_only: false` here.

## Test

Build a throwaway DDEV sandbox from the local branch through a path repository:

```shell
ddev composer create-project --stability=dev \
  --repository='{"type":"path","url":"/var/www/html/.ddev/webships_project"}' drupal/webships_project:dev-1.0.x
ddev drush site:install ../recipes/webships_starter -y
```
