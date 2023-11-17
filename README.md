# Storefront Deployment

Welcome to Storefront Deployment repository. Here, you can found a Github Actions we produce to simplify deployments of Storefronts to the Console & Cloud.

There are four actions:

- build-middleware
- build-frontend
- reuse-middleware
- deploy

With those four you can easily compose the deployment script for your needs.

## Explanation of the GitHub Actions

### build-frontend

```yaml
uses: vuestorefront/storefront-deployment/build-frontend
with:
  project_name: ${{ secrets.PROJECT_NAME }}
  cloud_username: ${{ secrets.CLOUD_USERNAME }}
  cloud_password: ${{ secrets.CLOUD_PASSWORD }}
  cloud_region: ${{ secrets.CLOUD_REGION }}
  npm_email: ${{ secrets.NPM_EMAIL }}
  npm_user: ${{ secrets.NPM_USER }}
  npm_pass: ${{ secrets.NPM_PASS }}
  frontend: 'next'
  docker_registry_url: 'registry.vuestorefront.cloud'
  npm_registry: 'https://registrynpm.storefrontcloud.io'
  api_base_url: format('{0}.{1}.{2}', secrets.PROJECT_NAME, secrets.CLOUD_REGION, 'gcp.storefrontcloud.io')
  api_subpath: '/api'
  api_protocol: 'https'
  multistore_enabled: false
  image_provider: 'ipx'
  image_provider_upload_url: ${{ secrets.IMAGE_PROVIDER_UPLOAD_URL }}
  image_provider_fetch_url: ${{ secrets.IMAGE_PROVIDER_FETCH_URL }}
```

**Input Parameters:**

- `project_name`: Project name in Console. Required field.
- `cloud_username`: Cloud API and Docker registry username. Required field.
- `cloud_password`: Cloud API and Docker registry password. Required field.
- `cloud_region`: Region where the environment is set up in the Console. Required field.
- `npm_email`: Email address required for logging into the private NPM registry. Required field.
- `npm_user`: Username for the private NPM registry. Required field.
- `npm_pass`: Password for the private NPM registry. Required field.
- `frontend`: Frontend choice - Next (`next`) or Nuxt (`nuxt`). Optional field. Defaults to `next`.
- `docker_registry_url`: URL to the Docker image registry. Optional field. Defaults to `registry.vuestorefront.cloud`.
- `npm_registry`: URL to the private NPM registry. Optional field. Defaults to `https://registrynpm.storefrontcloud.io`.
- `api_base_url`: Hostname (URL part) for Middleware. Optional field. The default value is composed of the Console project name, region, and VSF Cloud domain.
- `api_subpath`: Subpath to the Middleware. Optional field. Default value: `/api`.
- `api_protocol`: Communication protocol between frontend and Middleware. Optional field. Default value: `https`.
- multistore_enabled: Switch to enable Multistore support on the frontend side. Optional field. Default value: `false`.
- `image_provider`: Switch the image provider on the Nuxt frontend. More details in the [Nuxt image docs](https://image.nuxt.com/get-started/providers). Know the differences between the Nuxt & Next image modules and their configuration. Optional field. The default value is undefined because Nuxt automatically falls back to `ipx`
- `image_provider_upload_url`: URL for uploading images to the selected image provider (e.g., Cloudinary). Do not set this value if you are not using any image provider. Any value here will attempt to run the module in the frontend code, resulting in an error and frontend unavailability. Optional field.
- `image_provider_fetch_url`: URL for fetching images from the selected image provider. Optional field.

### build-middleware

```yaml
uses: vuestorefront/storefront-deployment/build-middleware
with:
  project_name: ${{ secrets.PROJECT_NAME }}
  cloud_username: ${{ secrets.CLOUD_USERNAME }}
  cloud_password: ${{ secrets.CLOUD_PASSWORD }}
  npm_email: ${{ secrets.NPM_EMAIL }}
  npm_user: ${{ secrets.NPM_USER }}
  npm_pass: ${{ secrets.NPM_PASS }}
  docker_registry_url: 'registry.vuestorefront.cloud'
  npm_registry: 'https://registrynpm.storefrontcloud.io'
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

### reuse-middleware

```yaml
uses: vuestorefront/storefront-deployment/reuse-middleware
with:
  source_project_name: 'another-console-project'
  project_name: ${{ secrets.PROJECT_NAME }}
  cloud_username: ${{ secrets.CLOUD_USERNAME }}
  cloud_password: ${{ secrets.CLOUD_PASSWORD }}
  docker_registry_url: 'registry.vuestorefront.cloud'
```

**Input Parameters:**

- `source_project_name`: Project name in Console from you want to copy middleware image. This project should be accessible with the same Cloud API username & password and be built for the same commit hash — required field.
- `project_name`: Project name in Console. Required field.
- `cloud_username`: Cloud API and Docker registry username. Required field.
- `cloud_password`: Cloud API and Docker registry password. Required field.
- `docker_registry_url`: URL to the Docker image registry. Optional field. Defaults to `registry.vuestorefront.cloud`.

### deploy

```yaml
uses: vuestorefront/storefront-deployment/deploy
with:
  project_name: ${{ secrets.PROJECT_NAME }}
  cloud_username: ${{ secrets.CLOUD_USERNAME }}
  cloud_password: ${{ secrets.CLOUD_PASSWORD }}
  cloud_region: ${{ secrets.CLOUD_REGION }}
  console_api_url: 'https://api.platform.vuestorefront.io'
  cloud_api_url: 'https://farmer.vuestorefront.cloud'
  docker_registry_url: 'registry.vuestorefront.cloud'
```

**Input Parameters:**

- `project_name`: Project name in Console. Required field.
- `cloud_username`: Cloud API and Docker registry username. Required field.
- `cloud_password`: Cloud API and Docker registry password. Required field.
- `cloud_region`: Region where the environment is set up in Console. Required field.
- `console_api_url`: Console API URL. Optional field. Defaults to `https://api.platform.vuestorefront.io`.
- `cloud_api_url`:  Cloud API URL. Optional field. Defaults to `https://farmer.vuestorefront.cloud`.
- `docker_registry_url`: URL to the Docker image registry. Optional field. Defaults to `registry.vuestorefront.cloud`.
