# React / Redux Style Guide

This is an opinionated style guide for developing applications in ES6+ with React and/or Redux.

These styles are based on current best practices in the [React] and [Redux] communities, as well as real life experience
with these tools in the field. It puts great focus on writing readable and maintainable code and using new JavaScript
features in a responsible manner.

[React]: https://facebook.github.io/react/
[Redux]: redux.js.org

## Credit

Much of this style guide is based on the Redux documentation and ideas from [Dan Abramov] in particular.

[Dan Abramov]: https://github.com/gaearon

## Contributing

This guide is opinionated. You'll probably disagree on some points. Feel free to [open an issue] or file a pull request
if you think something should be rephrased or changed. Please include a detailed reasoning about why your way is better,
preferably with links to other sources. I change my mind often (for the better), so please convince me.

## Table of Contents

- [Javascript/ES6](#javascript-or-es6)
  - [Variables: var, let and const](#variables-var-let-and-const)
  - [Variables: declaration and usage](#variables-naming-declaration-and-usage)
  - [Naming: files and variables](#naming-files-and-variables)
  - [Imports](#imports)
  - [Spacing and Line length](#spacing-and-line-length)
  - [Commenting and documentation](#commenting-and-documentation)
  - [Core language rules](#core-language-rules)
  - [Arrow functions](#arrow-functions)
  - [Pure functions](#pure-functions)
- [React](#react)
  - [Components and containers](#components-and-containers)
  - [Dealing with props](#dealing-with-props)
- [Redux](#redux)
  - [Action creators](#action-creators)
  - [Reducers](#reducers)
  - [Connecting React components](#connecting-react-components)
- [Services](#services)
- [Utils](#utils)

## Javascript / ES6

Developers which have recently discovered the power of ES6 often feel compelled to use them on every occasion. This is
very good because it keeps us upstream with the community and also at the same time it can quickly lead to unreadable code.
With great power comes great responsibility.

### Variables: var, let and const

- `var` deprecated. There is no use case for it anymore.
- Use `const` by default. Immutable bindings are a good starting point. Code is easier to read and comprehend because
  you can be sure the variable is never re-assigned.
- When your logic demands re-assignment use `let`.

```js
// deprecated:
var foo = 'foo';

// avoid:
let bar = 'bar';

// avoid:
const foo = 'foo',
  bar = 'bar',
  baz = 'baz';

// recommended:
const foo = 'foo';
const bar = 'bar';
const baz = 'baz';
```

### Variables: declaration and usage
- Declare variables where you use them.
- Declare each variable as a full statement. It avoids the dangling comma discussion.
- Separate logical chunks of code with an empty line.

```js
const foo = 'foo';
const bar = 'bar';
// use foo and bar

const baz = 'baz';
// use baz

const qux = 'qux';
// use qux
```

Common practice with `var` was to put all declarations at the top of their scope, even if they would only be used much
further down. The reason for this is that `var` declarations are automatically hoisted to the top of their scope, so
putting them there yourself makes the code more predictable(keep in mind `var` is deprecated in ES6).

However, `let` and `const` are not hoisted. Therefore there is no reason to declare them top of the file/scope. it's better to declare `let/const` just-in-time, right before the spot
where you are going to use them. This makes the code easier to follow. You won't have to go looking for usages before they are defined, because they can't be used there. 
It also makes it easier to extract a bunch of statements into a function, which is a great way to improve readability even more.

### Naming: files and variables


```js
file-names-like-this
ClassNamesLikeThis
methodNamesLikeThis
variableNamesLikeThis
parameterNamesLikeThis
propertyNamesLikeThis
SYMBOLIC_CONSTANTS_LIKE_THIS
```

### Imports
Use ES6 imports `import foo from 'foo'`. 
use CommonJS imports if the ES6 import isn't supported by the library and use-cases like dynamically fetching image assests inside the code
`Grouping` There should be 3 clusters of imports: third-party (aka vendor) libraries, first-party libraries, and local imports.
React/Redux is "Third party" because we didn't write it. Astro is "First party" because it lives their own repository.

```js
import React, {Component} from "react";
import {connect} from redux;

import {Button, CheckBox} from "Astro";

import Util from "../common/util.js";
import mainThing, {
    firstOtherThing,
    secondOtherThing,
    thirdOtherThing,
} from '../somewhere';
```

### Spacing and Line length
Don't insert extra spaces between parens, brackets, or braces.

Yes:
```js
// Literals:
const fancyPants = pants.map((pant) => ({...pant, isFancy: true}));
const toCartesian = (r, theta) => [r * cos(theta), r * sin(theta)];

// Destructuring:
const {StyleSheet, css} = valueObject;
const [x, y] = coordinates;


// Template strings:
const mission = `A ${price}, ${quality} education for ${clientele}.`;

// Parens:
if ((a === b) || (b === c)) {...}
```

No:
```js
// Literals:
const fancyPants = pants.map((pant) => ({ ...pant, isFancy: true }));
const toCartesian = (r, theta) => [ r * cos(theta), r * sin(theta) ];

// Destructuring:
const { StyleSheet, css } = require('aphrodite');
const [ x, y ] = coordinates;

// Template strings:
const mission = `A ${ price }, ${ quality } education for ${ clientele }.`;

// Parens:
if ( ( a === b ) || ( b === c ) ) {...}
```

##### Line length
Lines should not exceed 79 characters. (This is called the "80 character rule," leaving 1 character for the newline.)
This is consistent with our other language(python,...) style guide.

### Comments and documentation

#### Inline Comments
Inline style comments should be of the `//` variety, not the `/* */` variety.

#### Top level file and class comments
All files and classes should have JSDoc comments. JSDoc can be parsed by a number of open source tools, and must be well-formed.
Syntax:
```js
/**
 * A JSDoc comment should begin with a slash and 2 asterisks.
 */
```
Top-level (top-of-file) comments are designed to orient readers unfamiliar with the code to what is in this file and any other disclaimers clients of the code should be given. It should provide a description of the file's contents and any dependencies or compatibility information. As an example:

/**
 * Various utility functions to handle manage the commonly used functions across
 * the application.
 *
 * These utilities were written to be a general purpose utility
 * for the entire code base.
 */
 
 Class comments should be used for every class, and give a description along with appropriate type tags (see "Methods and properties" comments for more information on types on the constructor).
 
 ```js
 /**
 * Class making something fun and easy.
 *
 * @param {string} arg1 An argument that makes this more interesting.
 * @param {Array.<number>} arg2 List of numbers to be processed.
 */
function SomeFunClass(arg1, arg2) {
  // ...
}
 ```
 
 #### Methods and properties comments
 All non-trivial methods and properties should also have JSDoc comments.

Type annotations are strongly encouraged; if there is even a slight chance that the type will be ambiguous to future readers, put in a type annotation.

Type annotations are based on the ES4/JS2 type system, and are documented in the [Google JavaScript style guide](https://google.github.io/styleguide/javascriptguide.xml).

`@param` and `@return` type annotations that have comments that do not fit on one line wrap to the next line and indent 4 spaces.

/**
 * A UI component allows users to select badges from their full list
 * of earned badges, displaying them in a container.
 * Expects a Badges.BadgeList as a model.
 */
```js 
Class FeedTable extends Component{
    /**
     * Whether or not this is currently in edit mode
     */
    editable: false,

    /**
     * The feed list to show in table .
     * @type {{id: number, name: string}}
     */
    data: null,

    /**
     * @param {rowIdx: number}.
     * @return 
     */
    edit(rowIdx){
    …
    },
   ...
};
```
#### Note:
Don't over comment and avoid commenting by having self-explanatory names for the variables and methods. Avoid inline comments for methods, commenting only in method header is a good practice.

### Core language rules

#### Equality:

Prefer `===` (strict equality) to `==` due to the [numerous oddities
related to JavaScript's type coercion](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/).

The only valid use of `==` is for comparing against null and undefined
at the same time:

```js
// Check null and undefined, but distinguish between other falsey values
if (someVariable == null) {
```

#### Array and Object literals

Always use `[]` and `{}` style literals to initialize arrays and
objects, not the `Array` and `Object` constructors.

Array constructors are error-prone due to their arguments: `new
Array(3)` yields `[undefined, undefined, undefined]`, not `[3]`.

To avoid these kinds of weird cases, always use the more readable
array literal.

Object constructors don't have the same problems, but follow the same
rule for consistency with arrays.  Plus, `{}` is more readable.

In some cases, we do not yet allow a new language feature, if it's
expensive to polyfill.  In others, we require using the newer language
feature and avoiding the old:

#### Utility methods

| Construct | Use...                                | ...instead of |
| --------- | ------------------------------------- | ---------------------- |
| copy objects | use clone methods of `ramda`/`underscore`/... libraries | `JSON.parse(JSON.stringify(...))/Object.assign({}...)` |
| backticks | `` `http://${host}/${path}` `` | `"http://" + host + "/" + path` |
| destructuring | `var {x, y} = a;` | `var x = a.x; var y = a.y;` |
| fat arrow | `foo(() => {...})` | `foo(function() {...}.bind(this))` |
| let/const | `let a = 1; const b = "4EVAH"; a++;` | `var a = 1; var b = "4EVAH"; a++;` |
| includes | `array.includes(item)` | `array.indexOf(item) !== -1` |
| map/forEach/reduce/filter | `list.forEach() / list.map` | `for(var i=0,i<lidt.length:i++){...}` |
| for/of | `for (const [key, value] of Object.entries(obj)) {...}` | `_.each(obj, function(value, key) {...})` |
| spread | `{...a, ...b, c: d}` | `_.extend({}, a, b, {c: d})` |
| rest params | `function(bar, ...args) {foo(...args);}` | `function(bar) {var args = Array.prototype.slice.call(arguments, 1); foo.apply(null, args);}` |

#### Use `=>` instead of `bind(this)`

Arrow functions are easier to read (and with Babel, more efficient)
than calling `bind` manually.

#### Use rest params instead of `arguments`

The magic `arguments` variable has some odd quirks. It's simpler to
use rest params like `(...args) => foo(args)`.

#### Use backticks for string interpolation

`+` is not forbidden, but backticks are encouraged!

### Arrow functions

- Consider anonymous functions deprecated. We have arrow functions for that now.
- Use arrow functions only as part of a larger (named) entity, not as a stand-alone entity.

```js
// deprecated
function (string) {
  return string.toLocaleUpperCase();
}

// recommended
function upper(string) {
  return string.toLocaleUpperCase();
}

// use wisely
string => string.toLocaleUpperCase()

// if it fits your style
const upper = string => string.toLocaleUpperCase()
```

With the addition of the arrow function syntax, we have gained a very elegant and convenient way to define anonymous
functions. Because they use 'lexical this', usage of `this` is more predictable. Therefore you should use arrows instead
of regular anonymous functions.


### Pure functions

- Don't use shared mutable state or access variables from outer scope.
- Don't cause any side effects by running a function.
- Don't mutate any passed arguments, return a new value instead.

```js
// avoid side-effects
let x;
function inc() {
  x++;
}

// recommended
function inc(x) {
  return x + 1;
}

// avoid mutation
function inc(object) {
  object.value++;
}

// recommended
function inc(object) {
  return { ...object, value: object.value + 1 };
}
```

A sure way to reduce code complexity and enhance testability and readability is to keep your functions [pure]. This is
one of the primary aspects of functional programming. It involves writing functions in a purely input-output fashion.
Pure functions only work with the data they are given through their arguments and return some new data, without causing
side effects. That means they cannot access outside scope and they cannot mutate their arguments.

[pure]: https://en.wikipedia.org/wiki/Pure_function


## React

### Components and containers

- Separate 'components' from 'containers'.
- Prefer using a function for components, a class for containers.
- Use PascalCase for component names.
- Put each component in its own directory, inside index.js.
- Expose components as the default export.
- Expose secondary functions as named exports for unit testing.
- Never export anonymous (arrow) functions.

```js
// components/TodoItem/index.js
export default function TodoItem(props, context) {
  return <li onClick={() => context.onTodoClick()}>{props.label}</li>;
}

// containers/TodoList/index.js
export default class TodoList extends React.Component {
  render() {
    return <ul>{this.props.todos.map(todo => <TodoItem {...todo} />)}</ul>;
  }
}
```

React components come in two flavors: presentation components and container components, also known as [dumb vs smart
components]. We simply refer to them as 'components' and 'containers'.

React v0.14 introduced a new way to write components. In the past we would write every component as a class. However,
since presentation components are very lightweight, they don't need all the power that a class provides (i.e. lifecycle
methods and state).

The new way to write components uses a function instead of a class. Components defined that way are known as
'[functional stateless components][fsc]' because they cannot have any state and they are pure functions. This means they
will receive their props as an argument and must return the rendered component. It basically boils down to having only a
render method, which receives the props as an argument rather than accessing it as a class property. They also receive
context as a second argument.

Each component or container should be inside its own directory. This reduces the clutter when components use CSS and
other related files. By using `index.js` you won't have to change any import statements when you migrate from a file to
a folder with the same name. If you import a folder, `index.js` is inferred.

[dump vs smart components]: https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0
[fsc]: https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html#stateless-functional-components

### Dealing with props

- Start your render method with a destructuring assignment on props and/or context.
- Use the spread operator to assign multiple attributes in one go.

```js
const { label, completed } = this.props;
const { onTodoClick } = this.context;

return <TodoItem {...{ label, completed, onTodoClick }} />
```

## Redux

### Action creators

- Group related action creators in one file.
- Use [redux-promise] for asynchronous actions, or [redux-thunk] if you need more flexibility.
- Expose each action creator as a named export.
- Don't use a default export.

```js
// actions/something.js
export function doSomething() {
  return {
    type: 'DO_SOMETHING',
    payload: { foo: 'bar' },
    meta: { baz: 'qux' }
  };
}
```

Action creators are functions which return an action object. An action object contains a type and optionally a payload
and metadata. Action objects should follow the [Flux Standard Action] schema.

We could also use the createAction helper of redux-actions, but other than enforcing FSA it doesn't do much here in
terms of reducing boilerplate. In fact createAction is less readable and doesn't let us easily export named functions
(exporting anonymous or arrow functions should be avoided). Note that only `type` is mandatory.

['Ducks' is a proposal][ducks] to bundle action creators with their reducers into a single file. Such a bundle is called
a 'duck'. The goal is to move towards a modular application structure. So far the common practice is to group files by
type rather than by feature. A [similar transition][modular angularjs] has happened in the Angular world. This style
guide still follows the group-by-type pattern, but may move towards using ducks in the future.

[redux-promise]: https://github.com/acdlite/redux-promise
[redux-thunk]: https://github.com/gaearon/redux-thunk
[Flux Standard Action]: https://github.com/acdlite/flux-standard-action
[redux-actions]: https://github.com/acdlite/redux-actions
[ducks]: https://github.com/erikras/ducks-modular-redux
[modular angularjs]: https://medium.com/opinionated-angularjs/scalable-code-organization-in-angularjs-9f01b594bf06

### Reducers

- Expose reducers as the default export.
- Expose handlers as named exports for unit testing.
- Use constants instead of inline strings for action types.
- Always define an `initialState` variable.

```js
// using redux-actions

// reducers/something.js
import { handleActions } from 'redux-actions';
import { Todo } from '../constants/actionTypes';

const initialState = {};

export default handleActions({
  [Todo.add]: addTodo
}, initialState);

export function addTodo(state, action) {
  return { ...state, todo: action.payload.todo };
}
```

A reducer handles incoming actions. All reducers are triggered for all actions, so it's up to each reducer to decide
whether or not to act on the incoming action. This is commonly done by switching on the action type. Reducers receive
a current state and an action object and must return a new (updated or not) state. Their function signature is as
follows:

```js
(state, action) => state
```

Reducers in Redux are pure functions, which means they cannot access outside information other than the arguments they
are given. It's also not allowed to mutate outside state, be it by referencing outer scope or by mutating objects which
they receive as arguments. In other words they must take the data they are given and return a new state without using
mutation. This can often be achieved by using the array and object spread operators.

The simplest way to create a reducer is to use the handleActions method of redux-actions. This avoids having to write a
switch statement and a default handler. Another benefit is that is enforces the use of FSA-compliant action objects. The
final reducer should be exposed as the default export. Individual handler functions should be exposed as named exports
in order to simplify unit testing.

### Connecting React components

- Always use a state selector function that returns the minimal state subset you need.
- Expose the connected component as default export.
- Expose the unconnected component as named export for unit testing.

```js
export class TodoList extends React.Component { ... }

const stateSelector = ({ todos, filters }) => ({ todos, filters });
export default connect(stateSelector)(TodoList);
```

The [reselect] library works well for more complex selectors and to achieve better performance.

[reselect]: https://github.com/rackt/reselect

## Services

- All methods should return a promise.
- Expose the entire service object as a default export.
- Expose Individual service methods as named exports for unit testing.
- Declare methods in alphabetical order.
- Declare helper functions underneath the methods, in chronological order.
- Never export anonymous (arrow) functions.

```js
// services/SomethingService.js
export default {
  getSomething
};

export function getSomething() {
  return new Promise(resolve => resolve(helper()));
}

function helper() {
  return 'let me help you with that';
}
```

A service is where you commonly do AJAX requests to the back-end. All service methods should return a promise, even if
the result is calculated synchronously. This ensures a consistent and predictable API across all services and allows
changing to an asynchronous implementation later, without having to update service method calls all over the codebase.

Services should expose themselves via a default export to allow consumers to import the service as a whole. The reason
for this is that multiple services may expose the same generic method names, which can lead to name collisions. Services
should also expose each method individually using a named export in order to aid unit testing.

Service methods should be listed in alphabetical order. This keeps things tidy and avoids merge conflicts. Any helper
functions should be defined underneath all exported methods and listed in the order in which they are used (i.e.
chronological order, see 'Arrow functions'). Consider moving helper functions to utils and reusing them between
services.

## Utils

- Group related util functions under a common name.
- List util functions in alphabetical order.
- Expose each function as named export.
- Don't use a default export.
- Util functions must be pure.
- Util functions should be reusable, but have a single purpose.

```js
// utils/serialization.js
export function upper(string) {
  return string.toLocaleUpperCase();
}
```

Utils is a collection of various supporting functions. It is a good idea to extract a function to a util when you are
doing the same thing in multiple places throughout the codebase. If you do are not using the function in several files,
it probably should not be a util function.

Exported functions should be defined in alphabetical order to make them easy to locate and avoid merge issues. Each
function should be exposed using a named export. A utils file should not expose a default export, as it does not
represent an entity as a whole but is merely a bag of related functions.

Utility functions may never keep internal state, nor can they use services. They should be pure functions. Each exported
function should be suitable for multiple uses (i.e. generic enough to be reused), but they should have a single well
defined purpose (i.e. do one thing, and do it well).
