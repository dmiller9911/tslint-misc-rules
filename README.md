[![Build Status](https://travis-ci.org/jwbay/tslint-misc-rules.svg?branch=master)](https://travis-ci.org/jwbay/tslint-misc-rules)
[![npm version](https://img.shields.io/npm/v/tslint-misc-rules.svg?style=flat-square)](https://www.npmjs.com/package/tslint-misc-rules)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/jwbay/tslint-misc-rules/pulls)
# misc-tslint-rules

Collection of miscellaneous TSLint rules

1. `$ npm install tslint-misc-rules --save-dev`
2. Add the path to it to your tslint.json and add some rules:

```json
{
  "rules": {
    "sort-imports": true,
    "prefer-es6-imports": [
      true,
      "module1",
      "module2"
    ],
    "class-method-newlines": true,
    "declare-class-methods-after-use": true,
    "no-property-initializers": true,
    "jsx-attribute-spacing": true,
    "jsx-expression-spacing": true,
    "jsx-no-braces-for-string-attributes": true,
    "react-lifecycle-order": true,
    "prefer-or-operator-over-ternary": true,
    "camel-case-local-functions": true
  },
  "rulesDirectory": [
    "./node_modules/tslint-misc-rules/rules"
  ]
}
```

# Rules
## "sort-imports"
Fails:
```ts
import b from "b";
import a from "a";
```
Passes:
```ts
import a from "a";
import b from "b";
```
Blocks are grouped, so this also passes:
```ts
import a from "a";
import z from "z";
testSetup();
import c from "c";
import d from "d";
```
Precedence is essentially `*`, `{`, then alpha, so:
```ts
import * as a from "a";
import { b, c } from "bc";
import d from "d";
```

## "prefer-es6-imports"
With configuration (required):
```json
{
  "prefer-es6-imports": [
    true,
    "module-name"
  ]
}
```
Fails:
```ts
import mod = require("module-name");
import mod = require("path/to/module-name");
import mod = require("../module-name");
```

## "class-method-newlines"
Ensure each method in class is preceded by a newline.  

Fails:
```ts
class foo {
    propertyOne: any;
    propertyTwo: any;
    one() {
    }
    two() {
    }
}
```

Passes:
```ts
class foo {
    propertyOne: any;
    propertyTwo: any;

    one() {
    }

    two() {
    }
}
```

The first method is exempt, so this also passes:
```ts
class foo {
    one() {
    }

    two() {
    }
}
```


## "jsx-attribute-spacing"
Fails:
```jsx
<div prop = { value }/>
<div prop= { value }/>
<div prop ={ value }/>
```
Passes:
```jsx
<div prop={ value }/>
```

## "jsx-expression-spacing"
Fails:
```jsx
<div prop={value}/>
<div prop={ value}/>
<div prop={value }/>
<div>
  {value}
  { value}
  {value }
</div>
```
Passes:
```jsx
<div prop={ value }/>
<div>
  { value }
</div>
```

## "jsx-no-closing-bracket-newline"
Fails:
```jsx
  <a className="asdf"
      href="/foo"
  />

  <div
      className="qwer"
      id="asdf"
  >
      text
  </div>
```

Passes:
```jsx
  <a className="asdf"
      href="/foo" />

  <div
      className="qwer"
      id="asdf">
      text
  </div>
```
## "jsx-no-braces-for-string-attributes"
Fails:
```jsx
<div prop={ "value" }/>
```
Passes:
```jsx
<div prop="value"/>
```

## "react-lifecycle-order"
With configuration (optional):
```json
{
  "react-lifecycle-order": [
    true,
    "componentWillMount",
    "render",
    "componentWillUnmount"
  ]
}
```
Fails:
```ts
class extends React.Component {
    componentWillMount() {
    }

    componentWillUnmount() {
    }

    render() {
    }
}
```

Passes:
```ts
class extends React.Component {
    componentWillMount() {
    }

    render() {
    }

    componentWillUnmount() {
    }
}
```

If configuration is not specified, React's [invocation order](https://facebook.github.io/react/docs/component-specs.html#lifecycle-methods) is used.

## "prefer-or-operator-over-ternary"
Fails:
```ts
const maybeFoo = foo ? foo : bar;
```

Passes:
```ts
const maybeFoo = foo || bar;
```

## "no-property-initializers"
Fails:
```ts
class foo {
  bar = 42;
}
```

Passes:
```ts
class foo {
  bar: number;
}
```

## "camel-case-local-functions"
Due to React's Stateless Functional Components, this rule checks callsites rather than declarations.  
Fails:
```ts
import FooImport from 'foo';

function FooDeclaration() { }

FooImport();
FooDeclaration();
```

Passes:
```jsx
import fooImport from 'foo';

function fooDeclaration() { }
function SomeSFC(props) { return null; }

fooImport();
fooDeclaration();
const el = </SomeSFC>;
```

## "declare-class-methods-after-use"
Fails:
```ts
class foo {
    bar() {
    }

    foo() {
      this.bar();
    }
}
```

Passes:
```ts
class foo {
    foo() {
      this.bar();
    }

    bar() {
    }
}
```

## "no-braces-for-single-line-arrow-functions"
Fails:
```ts
const add = (x, y) => { return x + y };
const btn = <button onClick={ e => { console.log(e); } } />;
```
Passes:
```ts
const add = (x, y) => x + y;
const btn = <button onClick={ e => console.log(e) } />;
```
## "no-unnecessary-parens-for-arrow-function-arguments"
Fails:
```ts
const log = (x) => console.log(x);
```
Passes:
```ts
const log = x => console.log(x);
```