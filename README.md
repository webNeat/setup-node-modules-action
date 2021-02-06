# Setup node modules

Install node and npm dependencies and cache them for speed.

## Usage

```yml
uses: webNeat/setup-node-modules-action@v1
with:
  node-version: 14 # default: '14'
  cached-paths: | # default: './node_modules'
    ./node_modules
    ~/.cache/Cypress
  cache-key: ${{ hashFiles('./package-lock.json') }} # default: ${{ hashFiles('./package-lock.json') }}
```
