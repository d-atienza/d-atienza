This document has notes for javascript notes:

**JSDoc Notes**

/** Write a function that, given two numbers, returns the smaller of the two numbers.
*
*function:
*=========
*name: min
*
*parameters:
*	- A: number
*	- B: number
*
*return: number
*
*pseudo code:
*
*if A is smaller than B
*	return A
*else
*	return B
**?

----
**Writing out logic for .find() array method**

With regular jsdoc and adding in a predicate function into the tests
<img width="752" alt="Screenshot 2024-01-18 102317" src="https://github.com/d-atienza/notes/assets/153103146/964e0787-2f62-43cf-a9bf-93afbfc84add">

With template T if we wanted to put in either a number or str
<img width="759" alt="Screenshot 2024-01-18 104416" src="https://github.com/d-atienza/notes/assets/153103146/aa6de54a-6268-4917-ac43-78a0c715126f">
- you can put in @param {(element: T) => boolean} then vscode will assess what was inputted for @param {T[]} and adjusted the hover-over popup accordingly. ie. if the array is [1,2,3] then it'll change element:T to number that's required as an input
- this is a **generic function (findFirst)** that can work with any type of data

