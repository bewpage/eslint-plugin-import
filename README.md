eslint-plugin-import
---
[![build status](https://travis-ci.org/benmosher/eslint-plugin-import.svg)](https://travis-ci.org/benmosher/eslint-plugin-import)
[![npm](https://img.shields.io/npm/v/eslint-plugin-import.svg)](https://www.npmjs.com/package/eslint-plugin-import)

This plugin intends to support linting of ES6 import syntax, and prevent issues with misspelling of file paths and import names. All the goodness that the ES6 static module syntax intends to provide, marked up in your editor.

**Current support**:

* Ensure imports point to a file/module that can be resolved. ([`no-unresolved`](#no-unresolved))
* Ensure named imports correspond to a named export in the remote file. ([`named`](#named))
* Ensure a default export is present, given a default import. ([`default`](#default))
* Ensure imported namespaces contain dereferenced properties as they are dereferenced. ([`namespace`](#namespace))
* Report assignments (at any scope) to imported names/namespaces. ([`no-reassign`](#no-reassign))

## Rules

### `no-unresolved`

Ensures an imported module can be resolved to a module on the local filesystem,
as defined by standard Node `require.resolve` behavior.

Will attempt to resolve from one or more paths from the `resolve.root` shared setting, i.e.

```
---
settings:
  resolve.root: 'src'
```
or
```
  resolve.root:
    - 'src'
    - 'lib'
```

Paths may be absolute or relative to the package root (i.e., where your `package.json` is).


### `named`

Verifies that all named imports are part of the set of named exports in the referenced module.

### `default`

If a default import is requested, this rule will report if there is no default
export in the imported module.

### `namespace`

Enforces names exist at the time they are dereferenced, when imported as a full namespace (i.e. `import * as foo from './foo'; foo.bar();` will report if `bar` is not exported by `./foo`.).

Will report at the import declaration if there are _no_ exported names found.

Also, will report for computed references (i.e. `foo["bar"]()`).

**Implementation note**: currently, this rule does not check for possible redefinition of the namespace in an intermediate scope. Adherence to either `import/no-reassign` or the ESLint `no-shadow` rule for namespaces will prevent this from being a problem.

### `no-reassign`

Reports on assignment to an imported name (or a member of an imported namespace).
Will also report shadowing (i.e. redeclaration as a variable, function, or parameter);

## Debugging

### `no-errors`

Reports on errors in the attempt to parse the imported module for exports.
Primarily useful for determining why imports are not being reported properly by the other rules.
Pass `include-messages` as an option to include error descriptions in the report.