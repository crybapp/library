![Cryb logo](../.github/cryb.png)

_**Code Style**_

When writing code for Cryb, we would like you to follow these guidelines so we can keep our code clean and consistent across all projects.

This applies for any JavaScript/TypeScript-based project. For Rust, we stick to `rustfmt`/`clippy` default rules.

These rules are generally enforced via our `@cryb/eslint-config` package.
If there's a mismatch between the guidelines outlined here, please open an issue.

## Table of Contents

* [General Rules](#general-rules)
* [Variables](#variables)
  * [Naming Convention](#naming-convention)
* [Conditional Statements](#conditional-statements)
* [Imports](#imports)
* [Method Chaining](#method-chaining)
* [Comments](#comments)

## General Rules

* Indent size should be two spaces
* Files should end in `LF` with a newline, encoded in `UTF-8`
* No semicolons unless required
* Use single quotes instead of double quotes

## Variables

Use `const` when the variable isn't mutable. Otherwise, use `let`. Never use `var`.

Ensure that there is a space between different types of variable declarations. There should be room to breathe.

Example:

```ts
const product = 'Cryb'

let mutableVariable = 'Yo'
```

### Naming Convention

Please view the following examples for how variables should be named.

#### JavaScript / TypeScript

```ts
const name = 'William'
const appName = 'Cryb'
```

#### TypeScript Interface

```ts
interface Config {
  deleteOnExit: boolean
}
```

#### CSS

```css
div.profile-wrapper {
  background-color: red
}
```

#### Environment Property

```env
DELETE_ON_EXIT=1
```

#### JavaScript Config

```ts
export default {
  delete_on_exit: process.env.DELETE_ON_EXIT
}
```

## Conditional Statements

Use the `===` operand for comparing values.

Statements should be written on new lines, away from the same line as the `if` statement.

Always have the brackets up against the statement and away from the curly braces.

Don't use brackets if they are not needed.

Example:

```ts
const product = 'Cryb'

if (product === 'Cryb')
  console.log('Hey look, it\'s Cryb!')
else if (product === 'Some other product name') {
  console.log('Oh, it\'s not Cryb.')

  destroyService(product)
} else
  return
```

## Imports

Sort imports by the following in terms of priority:

1. Vendor Packages
2. Models
3. Schemas
4. Interfaces
5. Utils

Please also sort by line width.

Example:

```ts
import fs from 'fs'

import User from './models/user'

import IUser from './models/user/defs'
import StoredUser from './schemas/user.schema'

import { extractUserId } from './utils/helpers.utils'
```

## Method Chaining

Method chaining is fine&mdash;infact we recommend it&mdash;but to prevent long lines, please split your method calls up into multiple lines.

Example:

```ts
// ❌ This is too long.
functionCall().then(() => console.log('hey!')).catch(err => console.log('error!')).finally(() => console.log('finally!'))

// ✔️ This is better and is easier to read.
functionCall()
  .then(() =>
    console.log('hey!')
  )
  .catch(err =>
    console.log('error!')
  )
  .finally(() =>
    console.log('finally!')
  )
```

## Comments

### Singleline Comments

```ts
// This is my demo method.
const myMethod = () => 'Hello World!'
```

### Multiline Comments

```ts
/**
 * This method is for demo purposes.
 **/
const myMethod = () => 'Hello World!'
```
