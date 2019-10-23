# Code style

When writing code for Cryb, we would like you to follow these guidelines so we can keep our code clean and consistent across all projects.

## General Rules

* No semicolons unless required
* Use single quotes instead of double quotes

## Variables

Use `const` when the variable isn't mutable. Otherwise, use `let`.

Example:
```ts
const product = 'Cryb'
const mutableVariable = 'Yo'
```

## Conditional statements

Use the `===` operand for comparing values.

Always have the brackets up against the statement and away from the curly braces.

Don't use brackets if they are not needed.

Example:
```ts
const product = 'Cryb';

if(product === 'Cryb') {
    console.log("Hey look, it's Cryb!");
} else if(product === 'Some other product name')
    console.log("Oh, it's not Cryb.")
else return
```

## Imports

Sort imports by the following in terms of priority:

1. Packages
1. Models
1. Schemas
1. Interfaces
1. Utils

Please also sort by line width.

Example:
```ts
import fs from 'fs'

import User from './models/user'

import IUser from './models/user/defs'
import StoredUser from './schemas/user.schema'

import { extractUserId } from './utils/helpers.utils'
```