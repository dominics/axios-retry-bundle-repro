## axios-retry non-ESM export change reproduction

The two subdirectories here are exactly the same, except:

- In the 3.2.0/ directory, the dependencies include `"axios-retry": "3.2.0"`
- In the 3.2.1/ directory, the dependencies include `"axios-retry": "3.2.1"`

Each has a Webpack install, with a very simple configuration:

```js
module.exports = {
    entry: "./index.js",
    output: {
        filename: "bundle.js",
        libraryTarget: "commonjs",
    },
};
```

There's a `yarn test` script in each directory.

To reproduce the issue, I recommend running:

```bash
pushd 3.2.0 && yarn install && yarn test && popd
pushd 3.2.1 && yarn install && yarn test && popd
```

### Spoilers

`3.2.0` returns the following function:

```
[Function: O] {
  isNetworkError: [Function: i],
  isSafeRequestError: [Function: a],
  isIdempotentRequestError: [Function: c],
  isNetworkOrIdempotentRequestError: [Function: R],
  exponentialDelay: [Function: l],
  isRetryableError: [Function: f]
}
```

**However** `3.2.1` returns a module object (not callable):

```
Object [Module] {
  isNetworkError: [Getter],
  isRetryableError: [Getter],
  isSafeRequestError: [Getter],
  isIdempotentRequestError: [Getter],
  isNetworkOrIdempotentRequestError: [Getter],
  exponentialDelay: [Getter],
  default: [Getter]
}
```

This was originally reported at https://github.com/softonic/axios-retry/issues/181 but reproduction instructions weren't
provided.
