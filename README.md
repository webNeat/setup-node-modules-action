# Better node modules caching

```yml
- uses: actions/cache@v2
  with:
    path: ./node_modules
    key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}
- id: node_modules_exists
  run: 'if [ -d "node_modules" ]; then echo "::set-output name=value::yes"; else echo "::set-output name=value::no"; fi'
- if: steps.node_modules_exists.outputs.value == 'no'
  run: npm install
```
