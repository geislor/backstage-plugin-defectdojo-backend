# backstage-plugin-defectdojo-backend

Welcome to the backstage-plugin-defectdojo-backend backend plugin!


## Getting started

## Installation
This plugin needs to be added to an existing backstage instance.

```bash
# From your Backstage root directory
yarn add --cwd packages/backend @geislor/backstage-plugin-defectdojo-backend
```

Create a file called defectdojo.ts inside `packages/backend/src/plugins/` and add the following:

### defectdojo.ts
```typescript
import { createRouter } from '@geislor/backstage-plugin-defectdojo-backend';
import { Router } from 'express';
import { PluginEnvironment } from '../types';

export default async function createPlugin(
  env: PluginEnvironment,
): Promise<Router> {  
  return await createRouter({
    logger: env.logger,
    config: env.config,
  });
}
```

And import the plugin to `packages/backend/src/index.ts`.
### src/index.ts
```diff
diff --git a/packages/backend/src/index.ts b/packages/backend/src/index.ts
index c4736a5..5822302 100644
--- a/packages/backend/src/index.ts
+++ b/packages/backend/src/index.ts
@@ -28,6 +28,7 @@ import scaffolder from './plugins/scaffolder';
 import proxy from './plugins/proxy';
 import techdocs from './plugins/techdocs';
 import search from './plugins/search';
+import defectdojo from './plugins/defectdojo';
 import { PluginEnvironment } from './types';
 import { ServerPermissionClient } from '@backstage/plugin-permission-node';
 import { DefaultIdentityClient } from '@backstage/plugin-auth-node';
@@ -85,6 +86,7 @@ async function main() {
   const techdocsEnv = useHotMemoize(module, () => createEnv('techdocs'));
   const searchEnv = useHotMemoize(module, () => createEnv('search'));
   const appEnv = useHotMemoize(module, () => createEnv('app'));
+  const defectdojoEnv = useHotMemoize(module, () => createEnv('defectdojo'));

   const apiRouter = Router();
   apiRouter.use('/catalog', await catalog(catalogEnv));
@@ -93,6 +95,7 @@ async function main() {
   apiRouter.use('/techdocs', await techdocs(techdocsEnv));
   apiRouter.use('/proxy', await proxy(proxyEnv));
   apiRouter.use('/search', await search(searchEnv));
+  apiRouter.use('/defectdojo', await defectdojo(defectdojoEnv));

   // Add backends ABOVE this line; this 404 handler is the catch-all fallback
   apiRouter.use(notFoundHandler());

```

## Continue with Frontend Plugin
Follow the Docs from [README.md](https://github.com/deepan10/backstage-plugin-defectdojo)

## Configuration

Add the following into your `app-config.yaml`
### Config
```yaml
defectdojo:
  host: https://defectdojo.yourcompany.com/api
  token: YOUR_API_TOKEN
```

Add the following into your catalog
### Catalog
```yaml
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: backstage
  annotations:
    defectdojo/project: YOUR_PROJECT_NAME/YOUR_PROJECT_VERSION
```
