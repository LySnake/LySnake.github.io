---
title: TypeScript学习
comments: true
categories:
  - archives
  - technology
tags:
  - TypeScript
date: 2018-10-08 22:13:12
updated: {{ updated }}
description: 个人学习官网TypeScript教程
---

## 简介 ##

个人不太喜欢JS，语法方面有太多的特例又不太好解释，在作creator小游戏的过程中，JS在玩家客户机上出现的运行时错误，也不能有很好的错误反馈，仅有混淆后的那一句语句，没有函数栈，错误信息太少。另外就是JS这类弱类型语言，在写代码的过程中，没有很好的代码提示，而且也不能在编写代码时或编译期尽可能多的提供检查。另外，个人认为，对于一个项目的健壮性而言，在写的时候考虑的更多才能更健壮，TypeScript的语法可以让程序员在编写的时候考虑更多，在维护迭代的过程种通过代码本身就能知道更多信息，更利于维护。

[TypeScript官方教程](https://www.tslang.cn/docs/handbook/basic-types.html)

## 类型 ##

**布尔值**：

```TypeScript
let isDone: boolean = false;
```


**数字**：

```TypeScript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

**字符串**：

```TypeScript
let name: string = "bob";
name = "smith";

//模板字符串
let sentence: string = "Hello, my name is " + name + ".\n\n" +
    "I'll be " + (age + 1) + " years old next month.";
```

**数组**：

```TypeScript
let list: number[] = [1, 2, 3];

//数组泛型，Array<元素类型>
let list: Array<number> = [1, 2, 3];
```

**元组 Tuple**：元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 string和number类型的元组。

```TypeScript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

**枚举**：enum类型是对JavaScript标准数据类型的一个补充。 

```TypeScript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;

enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;

enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
```

**Any**：有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 

```TypeScript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

**Void**：void类型像是与any类型相反，它表示没有任何类型。

```TypeScript
function warnUser(): void {
    alert("This is my warning message");
}
```

**Null 和 Undefined**：undefined和null两者各自有自己的类型分别叫做undefined和null。默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量。当你指定了--strictNullChecks标记，null和undefined只能赋值给void和它们各自。 这能避免 很多常见的问题。

```TypeScript
function warnUser(): void {
    alert("This is my warning message");
}
```

**Never**：never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。

```TypeScript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

**类型断言**：类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

```TypeScript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

//当你在TypeScript里使用JSX时，只有 as语法断言是被允许的。
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```