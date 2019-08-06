# bazel-ngc-tsc-bad
This is a sample code that reproduces the issue I am facing with bazel+ngc (tsc)
This code is derived from
https://github.com/bazelbuild/rules_nodejs/tree/master/examples/bazel_managed_deps

To reproduce the issue, please do
```sh
bazel build :core
```

Then you will get something like below:
```
Compiling Angular templates (ngc) //:core failed (Exit 1) ngc-wrapped failed:
error executing command
bazel-out/host/bin/external/npm/@angular/bazel/bin/ngc-wrapped
'--node_options=--expose-gc' '--node_options=--stack-trace-limit=100'
'--node_options=--max-old-space-size=4096' ... (remaining 1 argument(s) skipped)

Use --sandbox_debug to see verbose messages from the sandbox
core.actions.ts(17,14): error TS2742: The inferred type of 'meow' cannot be
named without a reference to
'./external/npm/node_modules/@ngrx/store/src/models'. This is likely not
portable. A type annotation is necessary.  core.actions.ts(17,14): error TS2742:
The inferred type of 'meow' cannot be named without a reference to
'./external/npm/node_modules/@ngrx/store/store'.  This is likely not portable. A
type annotation is necessary.  core.reducers.ts(52,14): error TS2742: The
inferred type of 'getActivePlugin' cannot be named without a reference to
'./external/npm/node_modules/@ngrx/store/src/selector'. This is likely not
portable. A type annotation is necessary.  core.reducers.ts(52,14): error
TS2742: The inferred type of 'getActivePlugin' cannot be named without a
reference to './external/npm/node_modules/@ngrx/store/store'. This is likely not
portable. A type annotation is necessary.

Target //:core failed to build
```

Compare above error with the canonical way of building Angular in OSS:

```sh
# junks in bazel directory seem to cause angular-cli to misbehave.
bazel clean
npx ng build
# or
./node_modules/.bin/ng build
```
