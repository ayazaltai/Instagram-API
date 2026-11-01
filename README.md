# Instagram API

A ZIP snapshot of **instagram-private-api**, a Node.js client written in TypeScript for interacting with Instagram's private API. The included package metadata identifies version **1.46.1** and the upstream project as [dilame/instagram-private-api](https://github.com/dilame/instagram-private-api).

## Repository layout

The source is stored in [instagram-private-api-master.zip](./instagram-private-api-master.zip). Extract it before using the package; the root of this repository is not an npm package.

| Path inside `instagram-private-api-master/` | Purpose |
| --- | --- |
| `src/` | Client, account state, API repositories, paginated feeds, services, and response types |
| `examples/` | TypeScript usage examples |
| `docs/` | Generated API documentation |
| `package.json` | Dependencies, package metadata, and build/check scripts |
| `LICENSE` | Upstream MIT license |

## Extract and build

Prerequisites: Git, an unzip utility, Node.js, and npm. The archived manifest declares Node.js `>=8.0.0`; this is historical metadata, not a recommendation to use an unsupported runtime. Compatibility of the old dependency set with current runtimes must be verified in your environment.

```bash
git clone https://github.com/ayazaltai/Instagram-API.git
cd Instagram-API
unzip instagram-private-api-master.zip
cd instagram-private-api-master
npm install
npm run build
npm run typecheck
```

The `prepare` script also builds during installation. Compiled JavaScript and type declarations are written to `dist/`. The upstream README recommends the optional `re2` peer dependency for safer URL matching; consult its guidance and your platform's native build requirements.

## Basic usage

Run this example from the extracted package directory after building. Supply `IG_USERNAME` and `IG_PASSWORD` through your local environment.

```javascript
const { IgApiClient } = require('./dist');

async function main() {
  const username = process.env.IG_USERNAME;
  const password = process.env.IG_PASSWORD;
  if (!username || !password) {
    throw new Error('Set IG_USERNAME and IG_PASSWORD before running.');
  }

  const ig = new IgApiClient();
  ig.state.generateDevice(username);
  const user = await ig.account.login(username, password);
  const posts = await ig.feed.user(user.pk).items();
  console.log(`Fetched ${posts.length} posts.`);
}

main().catch(error => {
  console.error(error.message);
  process.exitCode = 1;
});
```

The client exposes individual API operations through repositories, pagination through feeds, and higher-level operations through services. Consult the extracted `docs/README.md`, `docs/classes/index/IgApiClient.md`, and `examples/` for details.

## Checks and limitations

- `npm run typecheck` checks the TypeScript source without emitting output.
- `npm run build` compiles the package.
- The manifest defines `npm test` using `tests/run.js`, but this archive does not include a `tests/` directory. The test command therefore cannot run from this snapshot as stored.

This is an unofficial private API client. Instagram can change endpoints, require account challenges, or reject requests. Use it only with accounts you are authorized to access and handle credentials and session data privately. This repository stores a snapshot rather than tracking upstream updates automatically. The build commands above describe the package scripts; live login and current API compatibility are not verified by this documentation.

## Attribution and license

The original project is [dilame/instagram-private-api](https://github.com/dilame/instagram-private-api), originally authored by Richard Hutta. The archived `LICENSE` is MIT, copyright 2016 huttarichard. Retain the included license and attribution when redistributing the source.
