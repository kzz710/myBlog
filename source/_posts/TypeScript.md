---
title: TypeScript
date: 2022-03-09 15:11:31
tags: 
 - ts
 - js
cover: https://s1.ax1x.com/2022/03/09/bR4iGj.png
---

## TypeScript详解

### hello typescript

typescript是js的超集，包括js,类型限制，es6自动转换

```typescript
const hello = (name: string) => {
    console.log(`hello, ${name}`)
}

hello('typescript')
```

### typescript配置文件
配置ts项目配置文件，使用命令行 yarn tsc --init 生成，内部包含文件入口目录、生成目录、sourceMap等配置，配置完成直接运行命令行 yarn tsc 则能编译整个项目
```json
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Projects */
    // "incremental": true,                              /* Enable incremental compilation */
    // "composite": true,                                /* Enable constraints that allow a TypeScript project to be used with project references. */
    // "tsBuildInfoFile": "./",                          /* Specify the folder for .tsbuildinfo incremental compilation files. */
    // "disableSourceOfProjectReferenceRedirect": true,  /* Disable preferring source files instead of declaration files when referencing composite projects */
    // "disableSolutionSearching": true,                 /* Opt a project out of multi-project reference checking when editing. */
    // "disableReferencedProjectLoad": true,             /* Reduce the number of projects loaded automatically by TypeScript. */

    /* Language and Environment */
    "target": "es5",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
     "lib": ["es2015","DOM"],                                        /* Specify a set of bundled library declaration files that describe the target runtime environment. */
    // "jsx": "preserve",                                /* Specify what JSX code is generated. */
    // "experimentalDecorators": true,                   /* Enable experimental support for TC39 stage 2 draft decorators. */
    // "emitDecoratorMetadata": true,                    /* Emit design-type metadata for decorated declarations in source files. */
    // "jsxFactory": "",                                 /* Specify the JSX factory function used when targeting React JSX emit, e.g. 'React.createElement' or 'h' */
    // "jsxFragmentFactory": "",                         /* Specify the JSX Fragment reference used for fragments when targeting React JSX emit e.g. 'React.Fragment' or 'Fragment'. */
    // "jsxImportSource": "",                            /* Specify module specifier used to import the JSX factory functions when using `jsx: react-jsx*`.` */
    // "reactNamespace": "",                             /* Specify the object invoked for `createElement`. This only applies when targeting `react` JSX emit. */
    // "noLib": true,                                    /* Disable including any library files, including the default lib.d.ts. */
    // "useDefineForClassFields": true,                  /* Emit ECMAScript-standard-compliant class fields. */

    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
     "rootDir": "src",                                  /* Specify the root folder within your source files. */
    // "moduleResolution": "node",                       /* Specify how TypeScript looks up a file from a given module specifier. */
    // "baseUrl": "./",                                  /* Specify the base directory to resolve non-relative module names. */
    // "paths": {},                                      /* Specify a set of entries that re-map imports to additional lookup locations. */
    // "rootDirs": [],                                   /* Allow multiple folders to be treated as one when resolving modules. */
    // "typeRoots": [],                                  /* Specify multiple folders that act like `./node_modules/@types`. */
    // "types": [],                                      /* Specify type package names to be included without being referenced in a source file. */
    // "allowUmdGlobalAccess": true,                     /* Allow accessing UMD globals from modules. */
    // "resolveJsonModule": true,                        /* Enable importing .json files */
    // "noResolve": true,                                /* Disallow `import`s, `require`s or `<reference>`s from expanding the number of files TypeScript should add to a project. */

    /* JavaScript Support */
    // "allowJs": true,                                  /* Allow JavaScript files to be a part of your program. Use the `checkJS` option to get errors from these files. */
    // "checkJs": true,                                  /* Enable error reporting in type-checked JavaScript files. */
    // "maxNodeModuleJsDepth": 1,                        /* Specify the maximum folder depth used for checking JavaScript files from `node_modules`. Only applicable with `allowJs`. */

    /* Emit */
    // "declaration": true,                              /* Generate .d.ts files from TypeScript and JavaScript files in your project. */
    // "declarationMap": true,                           /* Create sourcemaps for d.ts files. */
    // "emitDeclarationOnly": true,                      /* Only output d.ts files and not JavaScript files. */
    // "sourceMap": true,                                /* Create source map files for emitted JavaScript files. */
    // "outFile": "./",                                  /* Specify a file that bundles all outputs into one JavaScript file. If `declaration` is true, also designates a file that bundles all .d.ts output. */
     "outDir": "dist",                                   /* Specify an output folder for all emitted files. */
    // "removeComments": true,                           /* Disable emitting comments. */
    // "noEmit": true,                                   /* Disable emitting files from a compilation. */
    // "importHelpers": true,                            /* Allow importing helper functions from tslib once per project, instead of including them per-file. */
    // "importsNotUsedAsValues": "remove",               /* Specify emit/checking behavior for imports that are only used for types */
    // "downlevelIteration": true,                       /* Emit more compliant, but verbose and less performant JavaScript for iteration. */
    // "sourceRoot": "",                                 /* Specify the root path for debuggers to find the reference source code. */
    // "mapRoot": "",                                    /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                          /* Include sourcemap files inside the emitted JavaScript. */
    // "inlineSources": true,                            /* Include source code in the sourcemaps inside the emitted JavaScript. */
    // "emitBOM": true,                                  /* Emit a UTF-8 Byte Order Mark (BOM) in the beginning of output files. */
    // "newLine": "crlf",                                /* Set the newline character for emitting files. */
    // "stripInternal": true,                            /* Disable emitting declarations that have `@internal` in their JSDoc comments. */
    // "noEmitHelpers": true,                            /* Disable generating custom helper functions like `__extends` in compiled output. */
    // "noEmitOnError": true,                            /* Disable emitting files if any type checking errors are reported. */
    // "preserveConstEnums": true,                       /* Disable erasing `const enum` declarations in generated code. */
    // "declarationDir": "./",                           /* Specify the output directory for generated declaration files. */
    // "preserveValueImports": true,                     /* Preserve unused imported values in the JavaScript output that would otherwise be removed. */

    /* Interop Constraints */
    // "isolatedModules": true,                          /* Ensure that each file can be safely transpiled without relying on other imports. */
    // "allowSyntheticDefaultImports": true,             /* Allow 'import x from y' when a module doesn't have a default export. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables `allowSyntheticDefaultImports` for type compatibility. */
    // "preserveSymlinks": true,                         /* Disable resolving symlinks to their realpath. This correlates to the same flag in node. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */

    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                            /* Enable error reporting for expressions and declarations with an implied `any` type.. */
    // "strictNullChecks": true,                         /* When type checking, take into account `null` and `undefined`. */
    // "strictFunctionTypes": true,                      /* When assigning functions, check to ensure parameters and the return values are subtype-compatible. */
    // "strictBindCallApply": true,                      /* Check that the arguments for `bind`, `call`, and `apply` methods match the original function. */
    // "strictPropertyInitialization": true,             /* Check for class properties that are declared but not set in the constructor. */
    // "noImplicitThis": true,                           /* Enable error reporting when `this` is given the type `any`. */
    // "useUnknownInCatchVariables": true,               /* Type catch clause variables as 'unknown' instead of 'any'. */
    // "alwaysStrict": true,                             /* Ensure 'use strict' is always emitted. */
    // "noUnusedLocals": true,                           /* Enable error reporting when a local variables aren't read. */
    // "noUnusedParameters": true,                       /* Raise an error when a function parameter isn't read */
    // "exactOptionalPropertyTypes": true,               /* Interpret optional property types as written, rather than adding 'undefined'. */
    // "noImplicitReturns": true,                        /* Enable error reporting for codepaths that do not explicitly return in a function. */
    // "noFallthroughCasesInSwitch": true,               /* Enable error reporting for fallthrough cases in switch statements. */
    // "noUncheckedIndexedAccess": true,                 /* Include 'undefined' in index signature results */
    // "noImplicitOverride": true,                       /* Ensure overriding members in derived classes are marked with an override modifier. */
    // "noPropertyAccessFromIndexSignature": true,       /* Enforces using indexed accessors for keys declared using an indexed type */
    // "allowUnusedLabels": true,                        /* Disable error reporting for unused labels. */
    // "allowUnreachableCode": true,                     /* Disable error reporting for unreachable code. */

    /* Completeness */
    // "skipDefaultLibCheck": true,                      /* Skip type checking .d.ts files that are included with TypeScript. */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  }
}

```

### typescript的原始基本类型

```typescript
const a: string = 'ts';

const b: number = 123;

const c: boolean = true;

const d: any = '123';

const e: void = undefined; // 非严格模式下能为undefined

const f: undefined = undefined;

const g: null = null;

const h: symbol = Symbol(); // Symbol、Promise为es6的特性，故需使用需将配置文件target设为es2015,或者在配置文件lib新增es2015

export {}; // 形成模块作用域，避免示例命名冲突
```

### typescript的object类型
```typescript
export {};

const a: object = function (){}//{}//[]  // object类型包括对象、数组、方法

const b: {} = {name: 'zs', age: 18}

const c: {name: string, son: string} = {name: 'zs', age: 18} // 如果声明对象成员，则赋值应该严格按照内部成员赋值
```

### typescript的数组类型
```typescript
export {};

const a: Array<number> = [1,2,3];

const b: number[] = [1,2,3];

function add(...args: number[]) { //声明数据类型，无需再做类型判断
    return args.reduce((prev, cur) => {
        return prev + cur;
    }, 0)
}

add(1,2,3);
```

### typescript元组
```typescript
// 元组是数组的扩展，他表示已知元素类型和数量的数组
export {};
const a: [string, number] = ['ts', 210] // 元素的个数和类型都固定

a[0] = 123 // 报错
```

### 枚举
```typescript
export {}
export {};

// 以前用对象表示枚举

// const postStatus = {
//     pending: 0,
//     success: 1,
//     fail: 2
// }

//ts新增枚举

enum postStatus { // 枚举不赋值表示从0开始往后累加
    pending,
    success,
    fail
}

enum postStatus1 { // 第一个元素赋值，后面元素累加
    p=6,
    s,
    f
}

enum postStatus2 { // 如果第一个元素是字符串，则后面元素必须赋值
    p='p',
    s='s',
    f='f'
}

const enum ps { // 常量枚举，不能使用索引的方式访问名称，常量枚举不会被解析
    p,
    s,
    f
}

// ps[0] // 错误

ps.p;

postStatus1[0] // 6

const post = {
    title: 'ts',
    content: '很不错',
    status: postStatus.pending
}
```

### typescript的函数类型
```typescript
export {};

function f1(a: number, b:number): number {
    return a+b;
}

f1(1, 456)

function f2(a: string, b: number): string {
    return '123';
}

f2('ts', 502);

function f3(a: string, b?:number): void { // ?表示可变参数，可有可无，只能放在最后一个参数使用
    console.log(a, b);
}

f3('ts')

function f4(...rest: any[]): string { // any类型不会进行类型检查，会有安全问题
    console.log(rest)
    return '123'
}

f4(1,2, '5')

const f5 = function (a: number, b: number): number {
    return  a+ b;
}

f5(1,2);

function str(value: any) { // any能接受任意类型的参数，但是不会进行类型检查，不安全
    return JSON.stringify(value)
}

str(1);
str('ts');
str(undefined);
```

### 隐式类型装换和类型断言
```typescript
export {};

let a = 100;
// a = '123' // 报错，因为在a赋值时，已经进行了隐式类型转换，将a转换为number类型

let b; // 隐式类型转换，无法明确类型故为 any类型
b=1;
b='ts';
b=undefined;

let arr = [100,200,300]

let num = arr.find(item => item > 1);

// let res = num * num; // ts无法明确num是啥类型 故报错

let num1 = num as number; // 类型断言

let num2 = <number>num; // 推荐使用上一种类型断言方式

let res = num1 * num1;

let res2 = num2 * num2;

```

### typescript接口
接口就是为了约束对象的接口，一个对象实现接口，就必须拥有接口中所有的成员
```typescript
export {}

interface Post { // 接口对对象结构进行约束，对象实现接口，就需实现所有成员
    title: string,
    content: string,
    subtitle?: string // 添加？表示字段可有可无
    readonly summery: string // 只读成员
}

function getPost(post: Post) {
    console.log(post.title, post.content, 'zzzzzz');
}

getPost({
    title: 'ts',
    content: 'ts 666',
    summery: 'good'
})

let post: Post = {
    title: 'ts',
    content: 'ts 666',
    summery: 'good'
}


interface Cache {
    [key: string]: string, // 动态接口成员，对象可以实现多个属性成员
}

const cache: Cache = {};

cache.foo = 'foo'
cache.doo = 'doo'
```

### typescript类
```typescript
export {};

class Person {
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
        this.gender = true;
        this.xg = 'hqg'
    }
    public name: string; // public，表示能够被外部访问
    private age: number; // private, 表示只能被内部访问，外部访问不到
    protected gender: boolean; // protected，表示能在内部和子类访问，外部访问不到
    readonly xg: string; // 只读属性
    say(msg: string): void {
        console.log(msg)
        console.log(this.age, this.gender)
    }
}


const tom = new Person('tom', 18);
tom.say('hello');

class Student extends Person {
    private constructor(name: string, age: number) { //私有构造函数，外部不能使用new生成实例，只能内部使用静态方法生成
        super(name, age);
        console.log(this.gender) // protected属性能在子类中访问
    }
    static getInstance(name: string, age: number) {
        return new Student(name, age);
    }
}

// const xm = new Student()
const xm = Student.getInstance('xm', 18);
```

### typescript类的接口
```typescript
export {};

// 不同的类，可能会具有相同的方法，此时可以将相同的方法作为一个接口

interface Run {
    run(distance: string): void
}

interface Eat {
    eat(food: string): void
}

class Person implements Run, Eat{
    eat(food: string) {
        console.log('person eating' + food)
    }
    run(distance: string) {
        console.log('person run' + distance)
    }
}

class Animal implements Run, Eat{
    eat(food: string) {
        console.log('animal eating' + food)
    }
    run(distance: string) {
        console.log('animal run' + distance)
    }
}
```

### typescript抽象类
```typescript
export {};

abstract class Animal { // 抽象类只能被继承，不能被实例化
    eat(food: string): void {
        console.log('animal eat' + food)
    }
    abstract run(distance: string): void // 子类必须实现父类的抽象方法
}

// let dog = new Animal() // 错误

class Dog extends Animal {
    run(distance: string): void {
        console.log('四脚爬行' + distance)
    }
}

let dog = new Dog()

dog.run('100米')

dog.eat('肉')
```

### 泛型
```typescript
export {};

// 泛型就是当类型不明确时，使用泛型替代，后面在调用时，传入具体的数据类型

function createNumberArray(length: number, value: number): number[] { // 创建数字数组
    return Array<number>(length).fill(value);
}

createNumberArray(3,100);

function createStringArray(length: number, value: string): string[] { // 创建字符串数组
    return Array<string>(length).fill(value);
}

createStringArray(3,'100');

// 上诉方法冗余，正确的做法是使用泛型

function createArray<T>(length: number, value: T): T[] {
    return Array<T>(length).fill(value);
}

createArray<number>(5, 100);

createArray<string>(5,'100');

```