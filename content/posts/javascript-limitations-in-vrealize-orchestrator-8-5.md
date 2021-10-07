---
title: 'Javascript Limitations in vRealize Orchestrator 8.5'
date: 2021-09-14T21:03:57-04:00
draft: false
images:
tags:
  - vRA
  - JavaScript
---

{{< figure src="https://images.unsplash.com/photo-1517694712202-14dd9538aa97?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDEwfHxqYXZhc2NyaXB0fGVufDB8fHx8MTYyOTc1MDgyNQ&ixlib=rb-1.2.1&q=80&w=2000">}}

One of the selling points for vRealize Orchestrator is the ability to run
JavaScript inside of a workflow using \"Scriptable Tasks\". With the release of
vRealize Automation (vRA) 8.1, was the addition of [polyglot scripting](https://code.vmware.com/samples/7325/vro-polyglot-scripts). While this is a
great development for the project, I would have much preferred them to implement
modern JavaScript ([ES6 +](https://www.w3schools.com/js/js_es6.asp)) instead.

As of vRealize Orchestrator 8.5, VMware is using [ECMAScript 5.1](https://262.ecma-international.org/5.1/) and this comes with quite a few limitations for those who are used to the features of modern JS.

## Work in vRA

Create a constant object

```javascript
const student = {
	name: 'Harry Potter',
	house: 'Gryffindor',
	address: {
		streetNumber: 4,
		streetName: 'Privet Drive',
		confirmed: true,
	},
};
```

Build a function

```javascript
function sendLetterIfConfirmed(student) {
	if (!student.address || !student.address.confirmed) return;
	System.log(
		'Sending welcome letter to ' +
			student.name +
			' at ' +
			student.address.streetNumber +
			' ' +
			student.address.streetName
	);
}
sendLetterIfConfirmed(student);
```

Assign multiple variables at once

```javascript
const colors = ['red', 'green', 'blue'];
var [x, y, z, a] = colors;
System.log(x); //red
System.log(y); //green
System.log(z); //blue
System.log(a); //undefined
```

Do/While loop

```javascript
var count = 0;
do {
	count++;
	System.log('count is:' + count);
} while (count < 10);
```

For loop

```javascript
for (var counter = 1; counter < 5; counter++) {
	System.log('Inside the loop:' + counter);
}
System.log('Outside the loop:' + counter);
```

Labels

```javascript
var iterations = 0;
top: for (var i = 0; i < 5; i++) {
	for (var j = 0; j < 5; j++) {
		iterations++;
		if (i === 2 && j === 2) {
			break top;
		}
	}
}
System.log(iterations); // 13
```

Ternary operator

```javascript
var age = 19;
var canDrive = age > 16 ? 'yes' : 'no';
System.log(canDrive); // yes
```

isArray

```javascript
var seas = ['Black Sea', 'Caribbean Sea', 'North Sea', 'Baltic Sea'];
var index = seas.indexOf('North Sea');
System.log(Array.isArray(seas)); // true
```

Sort

```javascript
var employees = [
	{
		firstName: 'John',
		lastName: 'Doe',
		age: 27,
		joinedDate: 'December 15, 2017',
	},
	{
		firstName: 'Ana',
		lastName: 'Rosy',
		age: 25,
		joinedDate: 'January 15, 2019',
	},
	{
		firstName: 'Zion',
		lastName: 'Albert',
		age: 30,
		joinedDate: 'February 15, 2011',
	},
];
System.log(employees.sort);
```

## Things that do not work in vRA 8.5

Now here are some things that you might expect to work in vRO, but will return
validation errors.

Let

```javascript
let x = 10;
```

.includes

```javascript
const colors = ['red', 'green', 'blue'];
const result = colors.includes('red');
```

Using backticks ``

```javascript
System.log(`This would have been nice: ${variable}`);
```

Optional Chaining

```javascript
if (!student.address?.confirmed) return;
```

Spread Operator

```javascript
const copyOfStudent = { ...student };
```

padStart

```javascript
var number = 4;
System.log(number.padStart(2, 0));
```
