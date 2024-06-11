# Deploy with zugriff

## Example

> [!IMPORTANT]
> This example assumes you deploy an application built with a zugriff adapter.

```yml
# .github/workflows/zugriff.yml

name: zugriff

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - run: npm ci
      - run: npm run build

      - uses: zugriffcloud/action-deploy@latest
        with:
          deploymentToken: ${{ secrets.ZUGRIFF_TOKEN }}
```

## Inputs

| Argument                         | Description                                                | Required | Example                |
| :------------------------------- | :--------------------------------------------------------- | :------- | :--------------------- |
| `deploymentToken`                | Token to deploy and manage applications                    | Yes      | `<SECRET>`             |
| `cwd`                            | Current working directory                                  | No       | `./my-app/`            |
| `function`                       | Function entry file                                        | No       | `./src/server.js`      |
| `assets`                         | Newline-separated locations of static asset(-s) to include | No       | `./public/favicon.ico` |
| `promotions`                     | Newline-separated domain accociated labels                 | No       | `production`           |
| `name`                           | Deployment name                                            | No       | `<COMMIT-HASH>`        |
| `description`                    | Deployment description                                     | No       | `<COMMIT-MESSAGE>`     |
| `dryRun`                         | Deployment is created but not uploaded                     | No       | -                      |
| `disableStaticRouter`            | Disables the auto-router for static-only deployments       | No       | -                      |
| `enableStaticRouter`             | Enables the auto-router for deployments with functions     | No       | -                      |
| `preferFileRouter`               | Prefer file based routing                                  | No       | -                      |
| `preferPuppets`                  | Prefer puppets over redirects                              | No       | -                      |
| `disableFunctionDiscovery`       | Disable function discovery                                 | No       | -                      |
| `redirects`                      | Newline-separated Redirect Middleware                      | No       | `/:308:/index.html`    |
| `puppets`                        | Newline-separated Asset Alias Middleware                   | No       | `/:/index.html`        |
| `interceptors`                   | Newline-separated interceptors                             | No       | `404:/404.html`        |
| `pack`                           | Re-pack a Next.js or custom application before deploying   | No       | -                      |
| `environmentDeploymentApi`       | Self-hosted: API endpoint                                  | No       | `https://<DOMAIN>`     |
| `environmentGeneralPurposeApi`   | Self-hosted: API endpoint                                  | No       | `https://<DOMAIN>`     |
| `environmentWsGeneralPurposeApi` | Self-hosted: API endpoint                                  | No       | `wss://<DOMAIN>`       |

### Newline-separated arguments

Newline-separated arguments are array-like values. Please see the following example.

```yml
- uses: zugriffcloud/action-deploy@latest
  with:
    assets: |
      ./example.jpg
      ./index.html
```

The example will add `-y --asset ./example.jpg --asset ./index.html` to the deploy command.

## Outputs

| Identifier | Description                                                 | Example                                   |
| :--------- | :---------------------------------------------------------- | :---------------------------------------- |
| domains    | Comma-separated list of domains connected to the deployment | `my-app-123456789.zugriff.app,zugriff.eu` |
