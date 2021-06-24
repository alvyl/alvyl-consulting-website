+++
title = "Map, Filter in JavaScript"
date = 2020-06-25T15:57:26+05:30
description = ""
author = "Priyanka Murahari"
imageUrl = "/images/priyanka.png"
+++
*This article is for beginner*

**Map** and **filter** are  array methods in JavaScript. Each One, will iterate over an array and perform a transformation or computation.

Generally we use **for loop** for iterate over an array of elements.But **for-loops** can quickly become a bit messy and large.Most of the times it can be much easier to use the map, filter or reduce operator. Your code stays cleaner and is easier to read.Let's dive in

### **Map:**

The **map()** method is used to apply a function on every element in an array. A new array is then returned.

Syntax:
```
let result = array.map(callback);
```
- `callback` - The callback argument is a function that will be called once for every item in the array. 

Our callback function can take three arguments:

- `element`- The current element of the array

- `index` — The current index of the value being processed

- `array` — the original array

```
let newArray = array.map(function(element,index,array){
    //return an element
});
```

**Example**:

Using **for()**:

```
let array = [1, 4, 9, 16];
let result = [];
for(var i=0 ; i < array.length ; i++){
    let value = array[i] * 2;
    result.push(value);
}
console.log(result); // [2,8,18,32]
```
Using **map()**:

```
const result = [1, 4, 9, 16];

//ES5 Syntax
const result = array.map(function(item){
    return item * 2;
});
console.log(result); // [2,8,18,32]

//ES6 Syntax
const result = array.map(item => item * 2 );
console.log(result); // [2,8,18,32]
 
```

Now **map** has the job of taking an array, running a function on every item in that array and that function should return a new element for every element in that array.So here,the above code doubles the all of the values in the result array.

### **Filter**:

This method allows us to filter the elements in an array. The **filter()** method takes each element in an array and it applies a conditional statement against it. If this condition returns true, the element gets pushed into an output array. If the condition returns false, the element does not get pushed to the output array.

**Syntax**:

```
let result = array.filter(callback);
``` 
- `callback` - The callback argument is a function that will be called once for every item in the array. 

Our callback function can take three arguments:

- `element` - The current element of the array

- `index` - The current index of the value being processed

- `arr` - The original array

**Example**:

with **for()**:

```
let numbers = [1,2,-3,4,-5,6,-7];
let result = [];

for(var i=0 ; i < numbers.length ; i++){
    if(numbers[i] > 0){
       result.push(numbers[i])
    }
}
console.log(result) // 1,2,4,6
```
with **filter()**:

```
let numbers = [1,2,-3,4,-5,6,-7];

//ES5 Syntax
let result = numbers.filter(function(item){
    return item > 0
});
console.log(result) // 1,2,4,6

//ES6 Syntax
let result = numbers.filter(item => item > 0 );
console.log(result) // 1,2,4,6

```
When we call the **filter** method,this method will look through this numbers array and execute the callback function for each element in an array.

In the above example, if the element matches the **condition** means if the value is greater than 0, it will add element to the result array and finally we can get the all the positive numbers in the result array.

*__Thank you!__* 