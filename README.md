# Storefront Deployment

Welcome to Storefront Deployment repository. Here, you can found a Github Actions we produce to simplify deployments of Storefronts to the Console & Cloud.

There are four actions:

- build-middleware
- build-frontend
- deploy

With those four you can easily compose the deployment script for your needs.

## Explanation of the GitHub Actions

### build-frontend

```yaml
uses: vuestorefront/storefront-deployment/build-frontend@v1
with:
  project_name: ${{ secrets.PROJECT_NAME }}
  cloud_username: ${{ secrets.CLOUD_USERNAME }}
  cloud_password: ${{ secrets.CLOUD_PASSWORD }}
  npm_email: ${{ secrets.NPM_EMAIL }}
  npm_user: ${{ secrets.NPM_USER }}
  npm_pass: ${{ secrets.NPM_PASS }}
  frontend: 'next'
  docker_registry_url: 'registry.vuestorefront.cloud'
  npm_registry: 'https://registrynpm.storefrontcloud.io'
  version: '2.3.0'
  image_provider: 'ipx'
```

**Input Parameters:**

- `project_name`: Project name in Console. Required field.
- `cloud_username`: Cloud API and Docker registry username. Required field.
- `cloud_password`: Cloud API and Docker registry password. Required field.
- `npm_email`: Email address required for logging into the private NPM registry. Required field.
- `npm_user`: Username for the private NPM registry. Required field.
- `npm_pass`: Password for the private NPM registry. Required field.
- `frontend`: Frontend choice - Next (`next`) or Nuxt (`nuxt`). Optional field. Defaults to `next`.
- `docker_registry_url`: URL to the Docker image registry. Optional field. Defaults to `registry.vuestorefront.cloud`.
- `npm_registry`: URL to the private NPM registry. Optional field. Defaults to `https://registrynpm.storefrontcloud.io`.
- `version`: Version that will be used for docker image tag. Example: 2.3.0, 3.1.0-rc.1. If not passed, github.sha will be used
- `image_provider`: Select image provider for Nuxt Image. Optional field. Only for Nuxt.

Any environment variables needed by an application is needed to be set in the Alokai Console.

### build-middleware

```yaml
uses: vuestorefront/storefront-deployment/build-middleware@v1
with:
  project_name: ${{ secrets.PROJECT_NAME }}
  cloud_username: ${{ secrets.CLOUD_USERNAME }}
  cloud_password: ${{ secrets.CLOUD_PASSWORD }}
  npm_email: ${{ secrets.NPM_EMAIL }}
  npm_user: ${{ secrets.NPM_USER }}
  npm_pass: ${{ secrets.NPM_PASS }}
  docker_registry_url: 'registry.vuestorefront.cloud'
  npm_registry: 'https://registrynpm.storefrontcloud.io'
  version: 2.3.0
```

**Input Parameters:**

- `project_name`: Project name in Console. Required field.
- `cloud_username`: Cloud API and Docker registry username. Required field.
- `cloud_password`: Cloud API and Docker registry password. Required field.
- `npm_email`: Email address required for logging into the private NPM registry. Required field.
- `npm_user`: Username for the private NPM registry. Required field.
- `npm_pass`: Password for the private NPM registry. Required field.
- `docker_registry_url`: URL to the Docker image registry. Optional field. Defaults to `registry.vuestorefront.cloud`.
- `npm_registry`: URL to the private NPM registry. Optional field. Defaults to `https://registrynpm.storefrontcloud.io`.
- `version`: Version that will be used for docker image tag. Example: 2.3.0, 3.1.0-rc.1. If not passed, github.sha will be used

### deploy

```yaml
uses: vuestorefront/storefront-deployment/deploy@v1
with:
  project_name: ${{ secrets.PROJECT_NAME }}
  cloud_username: ${{ secrets.CLOUD_USERNAME }}
  cloud_password: ${{ secrets.CLOUD_PASSWORD }}
  cloud_region: ${{ secrets.CLOUD_REGION }}
  docker_registry_url: 'registry.vuestorefront.cloud'
  console_api: 'https://api.platform.vuestorefront.io'
  version: 2.3.0
```

**Input Parameters:**

- `project_name`: Project name in Console. Required field.
- `cloud_username`: Cloud API and Docker registry username. Required field.
- `cloud_password`: Cloud API and Docker registry password. Required field.
- `cloud_region`: Region where the environment is set up in the Console. Required field.
- `docker_registry_url`: URL to the Docker image registry. Optional field. Defaults to `registry.vuestorefront.cloud`.
- `console_api_url`: URL to the Console. Optional field. Defaults to `https://api.platform.vuestorefront.io`.
- `version`: Docker image tag that will be deployed. Example: 2.3.0, 3.1.0-rc.1. If not passed, github.sha will be used
