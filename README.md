# when-clause

This package lets you write a simple boolean expression in a string then evaluate it at runtime. This is useful for defining conditional logic in static configuration files like JSON or YAML. For example, this VSCode keybinding definition allows you conditionally bind keys using the "when" property. This package was inspired by this very example.

```json
{ 
    "key": "escape",
    "command": "cancelSelection",
    "when": "editorHasSelection && textInputFocus"
}
```

This "when" property accepts a string value that will be evaluated at runtime whenever the escape key is pressed. This expression can be parsed with this package like this.

```js
import {evaluate} from "when-clause"

const string = "editorHasSelection && textInputFocus"
const context = {
    editorHasSelection: true,
    textInputFocus: false
}

evaluate(string, context) // false
```

## API

There are only two function this package exports. You'll only really need to use `evaluate()`.

```ts
function evalute(expression: string, context: object): boolean
```

You also have access to the parse function which returns the AST.

```ts
function parse(clause: string): object
```

## What Syntax is Available?

You can use most of the common JavaScript operators: logical, relational, equality, addition, multiplication, and groupings. The available literal values are boolean, integer, and string. More could be added.

These are example expressions you can write.

```js
"1 + 1"
"1 - 1"
"1 * 2"
"2 / 2"
"1 > 0"
"1 < 0"
"1 <= 1"
"1 >= 1"
"1 && 1"
"isLeaf"
"isRoot"
"isRoot || count > 0"
```

## How Does It Work?

The package uses [peggy.js](https://peggyjs.org/) to create the parser. It's generated from the grammar.peggy file in this repo.


## Why?

You could just use eval, or new Function, but this is safer.
