---
title: JavaScript let vs var vs const — The Key Differences
description: Learn when to use const, let, and why to avoid var in JavaScript. Covers scope, hoisting, reassignment, redeclaration, and common pitfalls with examples.
date: "2025-08-24"
image: "/images/posts/post-1.jpg"
tags:
  - JavaScript
  - ES6
  - Basics
---

Choosing between const, let, and var impacts how predictable and safe your code is. Here’s a quick guide with examples.

## TL;DR rules

- Use const by default. It prevents accidental reassignment.
- Use let only when you must reassign (counters, state updates, loops).
- Avoid var. It’s function-scoped, hoisted differently, and can cause subtle bugs.

## Scope differences

- var → function-scoped (or global if declared at top level)
- let/const → block-scoped ({}), safer in loops and conditionals

```js
function demo() {
  if (true) {
    var a = 1;     // function-scoped
    let b = 2;     // block-scoped
    const c = 3;   // block-scoped
  }
  console.log(a); // 1
  // console.log(b); // ReferenceError
  // console.log(c); // ReferenceError
}
demo();
```

## Hoisting and the Temporal Dead Zone (TDZ)

- var is hoisted and initialized to undefined.
- let/const are hoisted but uninitialized until the declaration line (TDZ). Accessing before that throws a ReferenceError.

```js
console.log(a); // undefined (var is hoisted)
// console.log(b); // ReferenceError (TDZ)
// console.log(c); // ReferenceError (TDZ)
var a = 1;
let b = 2;
const c = 3;
```

## Reassignment and redeclaration

- const: no reassignment; object contents can still be mutated.
- let: can reassign; cannot redeclare in the same scope.
- var: can reassign and redeclare (another reason to avoid).

```js
const person = { name: "Ada" };
// person = {}; // ❌ TypeError (no reassignment)
person.name = "Lovelace"; // ✅ allowed (mutation)

let count = 0;
count = 1; // ✅ reassignment allowed
// let count = 2; // ❌ SyntaxError (redeclaration in same scope)

var x = 1;
var x = 2; // ✅ redeclaration allowed (can hide bugs)
```

## Loops: why let/const shine

```js
// Using var leaks the index outside the loop and shares one binding across iterations
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log("var i:", i), 0);
}
// → prints 3, 3, 3

// Using let creates a new binding each iteration (expected values)
for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log("let j:", j), 0);
}
// → prints 0, 1, 2
```

## Practical guidelines

- Prefer const for values that shouldn’t change (config, imported data, functions).
- Use let for counters, accumulators, or state that changes over time.
- Don’t use var in modern codebases.

## Summary

Use const by default, let when you must reassign, and avoid var. This leads to safer, more predictable JavaScript.
