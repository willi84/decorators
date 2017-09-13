# Decorators

This repo shows samples and structurs of decorators in typescript. Its a mixup of different sources

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

Decorators use the form @expression, where expression must evaluate to a function that will be called at runtime with information about the decorated declaration.

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

#### Decorator Factories

If we want to customize how a decorator is applied to a declaration, we can write a decorator factory. A Decorator Factory is simply a function that returns the expression that will be called by the decorator at runtime.

We can write a decorator factory in the following fashion:
```ts
function color(value: string) { // this is the decorator factory
    return function (target) { // this is the decorator
        // do something with 'target' and 'value'...
    }
}
```

#### Decorator Composition

Multiple decorators can be applied to a declaration, as in the following examples:

On a single line:
```ts
@f @g x
```

On multiple lines:
```ts
@f
@g
x
```

#### Decorator Evaluation

There is a well defined order to how decorators applied to various declarations inside of a class are applied:

1. Parameter Decorators, followed by Method, Accessor, or Property Decorators are applied for each instance member.
2. Parameter Decorators, followed by Method, Accessor, or Property Decorators are applied for each static member.
3. Parameter Decorators are applied for the constructor.
4. Class Decorators are applied for the class.

- [Class Decorator](#class-decorator)
- [Property Decorator](#property-decorator)
- [Method Decorator](#method-decorator)
- [Static Method Decorator](#static-method-decorator)
- [Parameter Decorator](#parameter-decorator)


### Class Decorator
[Example use](http://blogs.msdn.com/b/typescript/archive/2015/04/30/announcing-typescript-1-5-beta.aspx): Using the metadata api to store information on a class.

No parameters:
```ts
function ClassDecorator(
    target: Function // The class the decorator is declared on
    ) {
    console.log("ClassDecorator called on: ", target);
}

@ClassDecorator
class ClassDecoratorExample {
}
```
Output
```
    ClassDecorator called on:  function ClassDecoratorExample() {
     }
```

With parameters:
```ts
function ClassDecoratorParams(param1: number, param2: string) {
    return function(
        target: Function // The class the decorator is declared on
        ) {
        console.log("ClassDecoratorParams(" + param1 + ", '" + param2 + "') called on: ", target);
    }
}

@ClassDecoratorParams(1, "a")
@ClassDecoratorParams(2, "b")
class ClassDecoratorParamsExample {
}
```
Output
```
    ClassDecoratorParams(2, 'b') called on:  function ClassDecoratorParamsExample() {
     }
    ClassDecoratorParams(1, 'a') called on:  function ClassDecoratorParamsExample() {
     }
```


### Property Decorator
[Example use](http://stackoverflow.com/a/29706811/188246): Creating a @serialize("serializedName")
decorator and adding the property name the a list of properties to serialize.
```ts
function PropertyDecorator(
    target: Object, // The prototype of the class
    propertyKey: string | symbol // The name of the property
    ) {
    console.log("PropertyDecorator called on: ", target, propertyKey);
}

class PropertyDecoratorExample {
    @PropertyDecorator
    name: string;
}
```
Output
```
    PropertyDecorator called on:  {} name
```


### Method Decorator
```ts
function MethodDecorator(
    target: Object, // The prototype of the class
    propertyKey: string, // The name of the method
    descriptor: TypedPropertyDescriptor<any>
    ) {
    console.log("MethodDecorator called on: ", target, propertyKey, descriptor);
}

class MethodDecoratorExample {
    @MethodDecorator
    method() {
    }
}
```
Output
```
    MethodDecorator called on:  { method: [Function] } method { value: [Function],
      writable: true,
      enumerable: true,
      configurable: true }
```


Restrict to a certain function signature:
```ts
function TypeRestrictedMethodDecorator(
    target: Object, // The prototype of the class
    propertyKey: string, // The name of the method
    descriptor: TypedPropertyDescriptor<(num: number) => number>
    ) {
    console.log("TypeRestrictedMethodDecorator called on: ", target, propertyKey, descriptor);
}

class TypeRestrictedMethodDecoratorExample {
    @TypeRestrictedMethodDecorator
    method(num: number): number {
        return 0;
    }
}
```
Output
```
    TypeRestrictedMethodDecorator called on:  { method: [Function] } method { value: [Function],
      writable: true,
      enumerable: true,
      configurable: true }
```


### Static Method Decorator
```ts
function StaticMethodDecorator(
    target: Function, // the function itself and not the prototype
    propertyKey: string | symbol, // The name of the static method
    descriptor: TypedPropertyDescriptor<any>
    ) {
    console.log("StaticMethodDecorator called on: ", target, propertyKey, descriptor);
}

class StaticMethodDecoratorExample {
    @StaticMethodDecorator
    static staticMethod() {
    }
}
```
Output
```
    StaticMethodDecorator called on:  function StaticMethodDecoratorExample() {
      }
```


### Parameter Decorator
```ts
function ParameterDecorator(
    target: Function, // The prototype of the class
    propertyKey: string | symbol, // The name of the method
    parameterIndex: number // The index of parameter in the list of the function's parameters
    ) {
    console.log("ParameterDecorator called on: ", target, propertyKey, parameterIndex);
}

class ParameterDecoratorExample {
    method(@ParameterDecorator param1: string, @ParameterDecorator param2: number) {
    }
}
```
Output
```
    ParameterDecorator called on:  { method: [Function] } method 1
    ParameterDecorator called on:  { method: [Function] } method 0
```



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
* http://stackoverflow.com/a/29837695/40877
* https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Reflect
