# Migrating from the Node.js-only version

We've extended the functionality of this library so that can now run on more JavaScript runtimes other than just Node.js, as long as there is a runtime adapter for it.

To migrate your Node.js app to use this new adaptable version of the API library, you'll need to add an import of the node adapter in your app, _before_ importing the library functions itself.
Note you only need to import this once in your app.

```js
import '@shopify/shopify-api/adapters/node';
import { ... } from '@shopify/shopify-api';
```

This automatically sets the library up to run on the Node.js runtime.

In addition to making the library compatible with multiple runtimes, we've also improved its public interface to make it easier for apps to load only the features they need from the library.
Once you set up your app with the right adapter, you can follow the next sections for instructions on how to upgrade the individual methods that were changed.

## Basic configuration

We've refactored the way many of the objects are exported by this library, to remove the global `Shopify` object.
In general, those changes don't affect any library functionality, unless explicitly mentioned below.
You will probably need to search and replace most of the imports to this library to leverage the new, more idiomatic approach to exporting them.

### Renamed `Context` to `config`

The current `Context` object name doesn't reflect that it mostly just holds configuration settings for the library, and no business logic.
To make it more idiomatic, we've made the following changes to it:

- The `Shopify.Context.initialize()` method is now called `setConfig()`.
- You can search / replace instances of `Shopify.Context` with `config`, as per the example below.
- The config options are now `camelCase` instead of `UPPER_CASE` to better represent them as properties, rather than constants. The settings' names and functionality are still the same, you'll just need to replace e.g. `Shopify.Context.API_KEY` with `config.apiKey`.
- `Shopify.Context.throwIfUnitialized` is now `throwIfConfigNotSet`.
- `UninitializedContextError` is now `ConfigNotSetError`.

<table>
<tr><th>Old code</th><th>New code</th></tr>
<tr>
<td>

```ts
import {Shopify} from '@shopify/shopify-api';
Shopify.Context.initialize({ API_KEY: '...', ... });
console.log(Shopify.Context.API_KEY);
```

</td>
<td>

```ts
import {setConfig, config} from '@shopify/shopify-api';
setConfig({ apiKey: '...', ... });
console.log(config.apiKey);
```

</td>
</tr>
</table>