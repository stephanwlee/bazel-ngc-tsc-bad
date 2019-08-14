# bazel-tsc-bad
This is a sample code that reproduces the issue I am facing with bazel+ngc (tsc)
This code is derived from
https://github.com/bazelbuild/rules_nodejs/tree/master/examples/bazel_managed_deps

To reproduce the issue, please do
```sh
bazel build :bug
```

Then you will get something like below:
```
ERROR: /usr/local/google/home/stephanlee/dev/bad-ngc/BUILD.bazel:3:1: Compiling TypeScript (devmode) //:bug failed (Exit 1) tsc_wrapped failed: error executing command bazel-out/host/bin/external/npm/@bazel/typescript/bin/tsc_wrapped @@bazel-out/k8-fastbuild/bin/bug_es5_tsconfig.js
on

Use --sandbox_debug to see verbose messages from the sandbox
src/core.actions.ts:17:14 - error TS2742: The inferred type of 'meow' cannot be named without a reference to '../external/npm/node_modules/@ngrx/store/src/models'. This is likely not portable. A type annotation is necessary.

17 export const meow = createAction('[Core] wow');
                ~~~~
src/core.actions.ts:17:14 - error TS2742: The inferred type of 'meow' cannot be named without a reference to '../external/npm/node_modules/@ngrx/store/store'. This is likely not portable. A type annotation is necessary.

17 export const meow = createAction('[Core] wow');
                ~~~~
src/core.reducers.ts:52:14 - error TS2742: The inferred type of 'getActivePlugin' cannot be named without a reference to '../external/npm/node_modules/@ngrx/store/src/selector'. This is likely not portable. A type annotation is necessary.

52 export const getActivePlugin = createSelector(
                ~~~~~~~~~~~~~~~
src/core.reducers.ts:52:14 - error TS2742: The inferred type of 'getActivePlugin' cannot be named without a reference to '../external/npm/node_modules/@ngrx/store/store'. This is likely not portable. A type annotation is necessary.

52 export const getActivePlugin = createSelector(

```

Compare above error with the canonical way of building Angular in OSS:

```sh
npm run tsc
```
