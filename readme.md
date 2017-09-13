# Decorators

This repo shows samples and structurs of decorators in typescript.

## Basics
decorators are still in TS experimentell, so you need do allow them.

**Command Line:**
```ts
tsc --target ES5 --experimentalDecorators
```
**tsconfig.json**
```ts
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true
    }
}
```
## Annotation vs. Decorators
A decorator corresponds to a function that is called on the class whereas annotations are "only" metadata set on the class using the Reflect Metadata library.

With TypeScript and ES7, @Something is a decorator. In the context of Angular2, decorators like @Component, @Injectable, ... define metadata for the decorated element using the Reflect.defineMetadata method.

## Annotations
Annotations was an proposed by the Google AtScript team but annotations are not a standard. It adds automatically  some boilerplate code to the class, function or property its attached to.
* Type Annotations
* Field Annotations
* MetaData Annotations

### type annotation
```ts
var num: number = 123;
function identity(num: number): number {
    return num;
}
```

### metadata annotation
```ts
import { ComponentMetadata as Component } from '@angular/core';

@Component({
  selector: 'tabs',
  template: `<div>foobar</div>`
})
export class Tabs {

}
```

### field annoation
?

## Decorator
Decorators are a proposed standard for ECMAScript 7 by Yehuda Katz, to annotate and modify classes and properties at design time.Looks the same like AtScripts annotation but **we** are in charge of what our decorator does to our code.

A decorator is:

* an expression
* that evaluates to a function
* that takes the target, name, and decorator descriptor as arguments
* and optionally returns a decorator descriptor to install on the target object

Consider a simple class definition:

```js
class Person {
  name() { return `${this.first} ${this.last}` }
}
```

Evaluating this class results in installing the `name` function onto
`Person.prototype`, roughly like this:

```js
Object.defineProperty(Person.prototype, 'name', {
  value: specifiedFunction,
  enumerable: false,
  configurable: true,
  writable: true
});
```

A decorator precedes the syntax that defines a property:

```js
class Person {
  @readonly
  name() { return `${this.first} ${this.last}` }
}
```

Now, before installing the descriptor onto `Person.prototype`, the engine first
invokes the decorator:

```js
let description = {
  type: 'method',
  initializer: () => specifiedFunction,
  enumerable: false,
  configurable: true,
  writable: true
};

description = readonly(Person.prototype, 'name', description) || description;
defineDecoratedProperty(Person.prototype, 'name', description);

function defineDecoratedProperty(target, { initializer, enumerable, configurable, writable }) {
  Object.defineProperty(target, { value: initializer(), enumerable, configurable, writable });
}
```

The has an opportunity to intercede before the relevant `defineProperty` actually occurs.

## figures
![ ](https://svbtleusercontent.com/pxffvvbj2iho8w_small.jpg)

## sources



* https://github.com/arolson101/typescript-decorators
* https://blog.thoughtram.io/angular/2015/05/03/the-difference-between-annotations-and-decorators.html
* https://github.com/wycats/javascript-decorators
* https://www.typescriptlang.org/docs/handbook/decorators.html
* https://gist.github.com/remojansen/16c661a7afd68e22ac6e
* https://www.typescriptlang.org/play/
* https://basarat.gitbooks.io/typescript/docs/types/type-system.html
* https://stackoverflow.com/questions/37317705/what-is-the-difference-between-annotation-and-decorator
* https://spin.atomicobject.com/2017/04/24/typescript-modular-typesafe-metadata/
* http://blog.wolksoftware.com/decorators-metadata-reflection-in-typescript-from-novice-to-expert-part-4
* https://www.npmjs.com/package/reflect-annotations
* https://stackoverflow.com/questions/38085883/how-to-create-a-simple-typescript-metadata-annotation
