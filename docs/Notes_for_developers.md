# Notes for Developers

We use `commitizen` for better standardized commit messages. We use `semantic-release` for auto publishing the release to github and to npm.

## `package.json` necessary fields and values for `semantic-release` and `commitizen`:

```json
{
  "name": "temp-git",
  "version": "0.0.0-development",
  "main": "./dist/js/main.js",
  "module": "./dist/js/main.js",
  "type": "module",
  "files": ["dist/js/**"],
  "scripts": {
    "test": "echo \"No test specified\"",
    "build": "mkdir -p dist/js && cp src/main.js dist/js/",
    "commit": "cz",
    "semantic-release": "semantic-release"
  },
  "devDependencies": {
    "commitizen": "^4.2.5",
    "cz-conventional-changelog": "^3.3.0",
    "semantic-release": "^19.0.5",
    "@semantic-release/exec": "^6.0.3"
  },
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    [
      "@semantic-release/exec",
      {
        "prepareCmd": "npm run build"
      }
    ],
    "@semantic-release/npm",
    "@semantic-release/github"
  ],
  "config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  },
  "engines": {
    "node": ">=12"
  }
}
```

Note that the `package.json` version is prepared before publishing, so `semantic-release` adds the `nextRelease.version` env variable with the next release version.

We can access this variable in our npm scripts directly like `${nextRelease.version}`.

## `publish.yml` workflow:

```yaml
# This workflow will do a clean installation of node dependencies, cache/restore them, build the source code and run tests across different versions of node
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-nodejs-with-github-actions

name: Node.js CI

on:
  push:
    branches: ["main"]
  pull_request:
    branches: "*"

jobs:
  quality-checks:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        # See supported Node.js release schedule at https://nodejs.org/en/about/releases/
        node-version: [12.x, 14.x, 16.x]
        os: [ubuntu-latest, windows-latest]

    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: "npm"
      - run: npm ci
      - run: npm test

  publish:
    needs: [quality-checks]
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: "npm"
      - run: npm ci
      - run: npm run semantic-release
        env:
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

The above workflow contains two stages namely, `quality-checks` and `publish`.

- `quality-checks` runs the test, lint and other quality-checks of the code as applicable.
- `publish` runs th `npm run semantic-release` which runs the `build` npm script and then publishes to the npm and to github releases.
