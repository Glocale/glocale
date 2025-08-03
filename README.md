# glocale

A "good" locale system.

## Json format

Glocale uses JSON as a store for translations. Each locale gets stored in it's own file and can be loaded from any source.

Entries are defined like so:

```js
{ // Simple format
    "my": "Hello!",
    "my.key": "Hello world!"
}

{ // Nested format
    "my": {
        ".": "Hello!",
        "key": "Hello world!"
    }
}
```

In nested format, you can use dot notation to access the value (e.g `my.key`). Key _parts_ cannot contain periods.

In both formats, the following accessors result in:

- `my` -> `"Hello!"`
- `my.key` -> `Hello world!`

`.` being a special key in the Nested format for the root value of each key.

## Placeholders

Glocale defines the following placeholder syntaxes:

```
{argument}
[lookup]
<component>
```

### {argument}

Allows you to pass in a named argument to be replaced.

#### Example

`Hello {name}!` + `{name: "John Doe"}` = `Hello John Doe!`

### \[lookup\]

Looks up the provided local key and interprets it, passing the arguments to it.

#### Example

hello: `Hello {name}!`

`[hello] How are you today?` + `{name: "John Doe"}` = `Hello John Doe! How are you today?`

### \<component\>

Allows UI frameworks to pass inline Components to be rendered.

_Not required to be supported by all libraries._

#### Example

nameInput:

```html
<input type="text" />
```

`Hello! What is your name? <nameInput>` + `{nameInput}` =

```html
Hello! What is your name? <input type="text" />
```

### Escaping

You can use `^` as an escape character.
