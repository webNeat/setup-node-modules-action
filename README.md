# Better node modules caching

```yml
name: Compare
on:
  push:
    branches: [main]
jobs:
  # The recommended way
  normal:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the code
        uses: actions/checkout@v2
      - uses: actions/setup-node@v2-beta
        with:
          node-version: "14"
      - name: Cache node modules
        uses: actions/cache@v2
        env:
          cache-name: cache-node-modules
        with:
          path: ~/.npm
          key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-build-${{ env.cache-name }}-
            ${{ runner.os }}-build-
            ${{ runner.os }}-
      - name: Install Dependencies
        run: npm install
  # The efficient way
  optimized-with-install:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the code
        uses: actions/checkout@v2
      - uses: actions/setup-node@v2-beta
        with:
          node-version: "14"
      - uses: actions/cache@v2
        with:
          path: ./node_modules
          key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}
      - run: npm install
  # Another efficient way (maybe risky)
  optimized-without-install:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the code
        uses: actions/checkout@v2
      - uses: actions/setup-node@v2-beta
        with:
          node-version: "14"
      - uses: actions/cache@v2
        with:
          path: ./node_modules
          key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}
      - id: node_modules_exists
        run: 'if [ -d "node_modules" ]; then echo "::set-output name=value::yes"; else echo "::set-output name=value::no"; fi'
      - if: steps.node_modules_exists.outputs.value == 'no'
        run: npm install
```
