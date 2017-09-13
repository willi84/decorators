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
introduced by AtScript. It adds automatically  some boilerplate code to the class, function or property its attached to.
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

## sources



* https://github.com/arolson101/typescript-decorators
* https://blog.thoughtram.io/angular/2015/05/03/the-difference-between-annotations-and-decorators.html
* https://github.com/wycats/javascript-decorators
* https://www.typescriptlang.org/docs/handbook/decorators.html
* https://gist.github.com/remojansen/16c661a7afd68e22ac6e
* https://www.typescriptlang.org/play/
* https://basarat.gitbooks.io/typescript/docs/types/type-system.html
* https://stackoverflow.com/questions/37317705/what-is-the-difference-between-annotation-and-decorator
