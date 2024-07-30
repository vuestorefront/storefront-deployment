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
  cloud_region: ${{ secrets.CLOUD_REGION }}
  npm_email: ${{ secrets.NPM_EMAIL }}
  npm_user: ${{ secrets.NPM_USER }}
  npm_pass: ${{ secrets.NPM_PASS }}
  frontend: 'next'
  docker_registry_url: 'registry.vuestorefront.cloud'
  npm_registry: 'https://registrynpm.storefrontcloud.io'
  api_base_url: format('https://{0}.{1}.{2}/api', secrets.PROJECT_NAME, secrets.CLOUD_REGION, 'gcp.storefrontcloud.io')
  multistore_enabled: false
  image_provider: 'ipx'
  image_provider_upload_url: ${{ secrets.IMAGE_PROVIDER_UPLOAD_URL }}
  image_provider_fetch_url: ${{ secrets.IMAGE_PROVIDER_FETCH_URL }}
  coveo_organization_id: ${{ secrets.NEXT_PUBLIC_COVEO_ORGANIZATION_ID }}
  coveo_access_token: ${{ secrets.NEXT_PUBLIC_COVEO_ACCESS_TOKEN }}
  version: 2.3.0
  default_html_cache_control:  ${{ vars.DEFAULT_HTML_CACHE_CONTROL || 'public, max-age=0, s-maxage=0, must-revalidate' }}
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
- `api_base_url`: URL for Middleware. Optional field. The default value is composed of the Console project name, region, Alokai Cloud domain, and the default subpath which is `/api`.
- `image_provider`: Switch the image provider on the Nuxt frontend. More details in the [Nuxt image docs](https://image.nuxt.com/get-started/providers). Know the differences between the Nuxt & Next image modules and their configuration. Optional field. The default value is undefined because Nuxt automatically falls back to `ipx`
- `image_provider_upload_url`: URL for uploading images to the selected image provider (e.g., Cloudinary). Do not set this value if you are not using any image provider. Any value here will attempt to run the module in the frontend code, resulting in an error and frontend unavailability. Optional field.
- `image_provider_fetch_url`: URL for fetching images from the selected image provider. Optional field.
- `coveo_organization_id`: Coveo organization ID required for coveo integration. Optional field.
- `coveo_access_token`: Coveo organization access token required for coveo integration. Optional field.
- `coveo_tracking_id`: Coveo tracking id required for coveo integration. Optional field.
- `version`: Version that will be used for docker image tag. Example: 2.3.0, 3.1.0-rc.1. If not passed, github.sha will be used
- `default_html_cache_control`: Default cache control header value for SSR pages. If not passed, `public, max-age=0, s-maxage=15, must-revalidate` will be used.

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

### build-stripe-ct

```yaml
uses: vuestorefront/storefront-deployment/build-stripe-ct
with:
  project_name: ${{ secrets.PROJECT_NAME }}
  cloud_username: ${{ secrets.CLOUD_USERNAME }}
  cloud_password: ${{ secrets.CLOUD_PASSWORD }}
  docker_registry_url: 'registry.vuestorefront.cloud'
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
- `extension_module_name`: Extension module name. It’s used as the name of the docker image and path where the api is available. Default value is `ct-stripe-extension`.
- `extension_module_port`: Port of the extension module. Default value is `8080`.
- `extension_module_config`: Configuration for the extension module. Required field.
- `notification_module_name`: Notification module name. It’s used as the name of the docker image and path where the api is available. Default value is `ct-stripe-notification`.
- `notification_module_port`: Port of the notification module. Default value is `8081`.
- `notification_module_config`: Configuration for the notification module. Required field.

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

### deploy/stripe-ct

```yaml
uses: vuestorefront/storefront-deployment/deploy/stripe-ct
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
- `extension_module_name`: Extension module name. It’s used as the name of the docker image and path where the api is available. Default value is `ct-stripe-extension`.
- `extension_module_port`: Port of the extension module. Default value is `8080`.
- `notification_module_name`: Notification module name. It’s used as the name of the docker image and path where the api is available. Default value is `ct-stripe-notification`.
- `notification_module_port`: Port of the notification module. Default value is `8081`.