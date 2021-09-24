---
title: 'Javascript Limitations in vRealize Orchestrator 8.5'
date: 2021-09-14T21:03:57-04:00
draft: false
feature_image: 'https://images.unsplash.com/photo-1517694712202-14dd9538aa97?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDEwfHxqYXZhc2NyaXB0fGVufDB8fHx8MTYyOTc1MDgyNQ&ixlib=rb-1.2.1&q=80&w=2000'
toc: false
images:
tags:
  - untagged
---

{{< figure src="https://images.unsplash.com/photo-1517694712202-14dd9538aa97?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDEwfHxqYXZhc2NyaXB0fGVufDB8fHx8MTYyOTc1MDgyNQ&ixlib=rb-1.2.1&q=80&w=2000">}}

One of the selling points for vRealize Orchestrator is the ability to run
JavaScript inside of a workflow using \"Scriptable Tasks\". With the release of
vRealize Automation (vRA) 8, was the addition of [polyglot scripting](https://code.vmware.com/samples/7325/vro-polyglot-scripts). While this is a
great development for the project, I would have much preferred them to implement
modern JavaScript ([ES6 +](https://www.w3schools.com/js/js_es6.asp)) instead.

As of vRealize Orchestrator 8.5, VMware is using a modified ES5 engine. Here
are some things that work in this implementation.

```js
// Create a constant object
const student = {
	name: 'Harry Potter',
	house: 'Gryffindor',
	address: {
		streetNumber: 4,
		streetName: 'Privet Drive',
		confirmed: true,
	},
};

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

// Assigning multiple variables
const colors = ['red', 'green', 'blue'];
var [x, y, z, a] = colors;
System.log(x); //red
System.log(y); //green
System.log(z); //blue
System.log(a); //undefined

// do while
var count = 0;
do {
	count++;
	System.log('count is:' + count);
} while (count < 10);

// for loop
for (var counter = 1; counter < 5; counter++) {
	System.log('Inside the loop:' + counter);
}
System.log('Outside the loop:' + counter);

// label
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

// ternary operator
var age = 19;
var canDrive = age > 16 ? 'yes' : 'no';
System.log(canDrive); // yes

// isArray
var seas = ['Black Sea', 'Caribbean Sea', 'North Sea', 'Baltic Sea'];
var index = seas.indexOf('North Sea');
System.log(Array.isArray(seas)); // true

//sort
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

Now here are some things that you might expect to work in vRO, but will return
validation errors.

```javascript
// Let does not work
let x = 10;

// .includes does not work
const colors = ['red', 'green', 'blue'];
const result = colors.includes('red');

//  Using backticks ``
System.log(`This would have been nice: ${variable}`);

//  Optional Chaining
if (!student.address?.confirmed) return;

//  Spread Operator
const copyOfStudent = { ...student };

//  padStart
var number = 4;
System.log(number.padStart(2, 0));
```
