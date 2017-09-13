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

## Annotations
introduced by AtScript
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


## sources



* https://github.com/arolson101/typescript-decorators
* https://blog.thoughtram.io/angular/2015/05/03/the-difference-between-annotations-and-decorators.html
* https://github.com/wycats/javascript-decorators
* https://www.typescriptlang.org/docs/handbook/decorators.html
* https://gist.github.com/remojansen/16c661a7afd68e22ac6e
* https://www.typescriptlang.org/play/
* https://basarat.gitbooks.io/typescript/docs/types/type-system.html
