# Standars and good practices

   - [1- Objective](#1--objective)
   - [2- APP Structure](#2--app-structure)
     - [2.1- Controllers](#21--controllers) 
     - [2.2- Services](#22--services)
     - [2.3- Models](#23--models)
     - [2.4- Helpers](#24--helpers)
     - [2.5- Serializers](#25--serializers)
     - [2.6- Interactors](#26--interactors)
     - [2.7- Middlewares](#27--middlewares)
   - [3- Naming Convention](#3--naming-convention)
     - [3.1- File name](#31--file-name)
     - [3.2- Input and output API parameters (Pending)](#32--parámetros-de-entrada-y-salida-de-api-pendiente)
     - [3.3- Routes (Pendiente)](#33--rutas-pendiente)
     - [3.4- Database](#34--base-de-datos)
     - [3.5- Code](#35--resto-del-código)
   - [4- Conditionals](#4--conditionals)
     - [4.1- IFs](#41--ifs)
     - [4.2- Ternary Operator](#42--ternary-operator)
     - [4.3- AND Operator](#43--and-operator)
     - [4.4- OR Operator](#44--or-operator)
   - [5- API Rest](#5--api-rest)
     - [5.1- Best Practices](#51--best-practices)
     - [5.2- Status Code ERRORs](#52--status-code-errors)
   - [6- Functional Programming](#6--functional-programming)
     - [6.1- Introduction](#61--introduction)
     - [6.2- Map](#62--map)
     - [6.3- Reduce](#63--reduce)
     - [6.4- Filter](#64--filter)
     - [6.5- Others](#65--others)
   - [7- Code Style](#7--code-style)
     - [7.1- Length Limit](#71--length-limit)
     - [7.2- Requires](#7.2--requires)
     - [7.3- Destructuring](#7.3--destructuring)
     - [7.4- Implicit Return](#74--implicit-return)
   - [8- Promise vs Async/Await](#8--promise-vs-asyncawait)
     - [8.1- Promise](#81--promise)
     - [8.2- Async/Await](#82--async/await)
   - [9- Useful Links](#9--useful-links)

## 1- Objective

The purpose of this document is to present the standards we use in Wolox to work with NodeJS. 
It's recommended to read Matias Pizzagalli's post whose link is in the section of useful links.

&nbsp;

## 2- APP Structure

The structure that will be used includes, at least, these folders: (models and interactors if they are necessary for the API). For more information is Gonzalo Escandarani's post in the section of useful links.

### 2.1- Controllers

Every exported controller method should receive a request, a response and the next parameters from express. Its responsibility is validating and formatting the request, calling the services, modeling methods and returning the response formatting if it is necessary. Also, handling errors should be done here. The controllers are grouped by resource such as users, albums and so on.  In the case that there are too many complex parts you should abstract part of the logic to an **interactor**.

### 2.2- Services

Our services are responsible for interacting with external services and database management. Usually we have one file per external service and should be as simple as possible. The services shouldn't interact with each other.

### 2.3- Models

The model should be as simple as possible, just fields and Sequelize configurations. We try to not put logic here but it is permitted to do some specific model validations such as user password validation.

### 2.4- Helpers

Helpful tools with **absolutley no business logic whatsoever**. These include parsers, date formatters, etc. These are just helpers and should be able to be used in other projects.

### 2.5- Serializers

Formats the response of a service or endpoint. These are used when there is too much complexity or repetition.

### 2.6- Interactors

Utilized when business flow is too complex or are many of them. For _complex_ business flows we mean those which utilize many _services_ or perform various calculations. For these cases we create **interactors** and move the different interactions from the controller to it.

### 2.7- Middlewares

Abstraction layers set up before controllers usually, which allow us to perform certain validation steps, for example, authentication or schema validations.

&nbsp;

## 3- Naming Conventions

### 3.1- Files

File names must be **snake_case**, the entity of the file must not be repeated, for example if we have the model user, the file should be named **user.js** instead of **user.model.js** or **user_model.js**. Same thing for to controllers, services, etc.

### 3.2- Input and output API parameters (Pending)

Input and output API parameters will be in **snake_case**.

### 3.3- Routes (Pending)

Routes will be **snake_case**.

### 3.4- Database

Tables and columns should all be **snake_case**.

### 3.5- Code

Code is always **camelCase**.

&nbsp;

## 4- Conditional Statements

### 4.1 - IFs

Up to the possible extent, single statement `ifs` should be placed within the same line without using **{ }**.

```javascript
   if(!user) return next(errors.notFound('User not found');
```

Likewise, the if statement should be wisley selected to avoid using the else statement, for example use early return instead of else statement:

```javascript
   if(!user) return next(errors.notFound('User not found');
    ..
    ..
    ....
    .
    ....
    ..
```
instead of

```javascript
    if(user){
        ..
        ..
        ....
        .
        ....
        ..
    } else {
        next(errors.notFound('User not found');
    };
```
### 4.2- Ternary Operator

When a value should be chosen using a binary condition it is useful to use the ternary operator. 

```javascript
   let variable = condition ? value_if_condition_is_true : value_if_condition_is_false;
```

### 4.3- AND Operator

A simple way to avoid the ternary operator is using the **AND** operator. 

In the following example variable will be set to value if indicator is truthy, if indicator is falsy variable will be set to value;

```javascript
   let variable = indicator && value;
```

### 4.4- OR Operator

When using OR operator between diferent values, the result of evaluation will be the first truthy value from left to right. 

In this example variable will be set to option_1 if its not falsy, in that case it will be set to option_2 value if its not falsy, and on until the last value.

```javascript
   let variable = option_1 || option_2 || option_3;
```

&nbsp;


## 5- Rest API

Typically we use a RESTful design for our web APIs. The concept of REST is to separate the API structure into logical resources. There are used the HTTP methods **GET**, **DELETE**, **POST** and **PUT** to operate with the resources.

### 5.1- Best Practices

These are some of the best practices to design a clean RESTful API:

* **Use plural nouns instead of verbs**: To get all cars perform a GET to _/users_ instead of _getUsers_.
* **GET methods must not alter states**: Must return stuff, not modify it.
* **Use sub-resources for relations**: To obtain driver number 2 of car number 4 use GET _/cars/4/drivers/2_
* **Provide filtering, sorting, field selection and paging for collections**: Use query params to apply different options to alter data retrieval through GET methods.
* **API Version**: API's must be versioned always.

### 5.2- Response Status Codes

There are many _status codes_ to use in request responses. \
Most commonly used are:

* **200 OK**: Base successful response. Depends on currently used HTTP method. 
* **201 CREATED**: Successful response meaning a new resource has been created. Most commonly used with POST and sometimes PUT.
* **204 NO CONTENT**: Successful response without content in body.
* **400 BAD REQUEST**: Request was not formatted correctly and the server cannot interpret it.
* **401 UNAUTHORIZED**: Unauthorized attempt to use a specific resource.
* **403 FORBIDDEN**: Incorrect level of authorization to use a specific resource.
* **404 NOT FOUND**: Specified resource was not found.
* **422 UNPROCESSABLE ENTITY**: Must be used when the server cannot handle the request as is. For example may be a parameter image cannot be read correctly or some parameters are missing.
* **500 INTERNAL SERVER ERROR**: An internal server error has ocurred which it does not know how to handle.

Useful links will include more information about status codes.

## 6- Functional

To the extent to which it can be applied, use fuctional primitives as **Map, Reduce, Filter, etc.**.

### 6.1- Introduction

Functional programming is a _declarative_ paradigm, whilst prodedural and OOP (object oriented programming) are _imperative_ paradigms.

The _declarative_ paradigm, as opposed to _imperative_, avoids describing the **control flow** (loops, conditionals, etc.) and focuses on describing **what** are we doing instead of **how** are we doing it (_imperative_ approach).

### 6.2- Map

Returns a new array resulting of applying the a function to each element of the original array.

```javascript
   const numbers = [1, 4, 9, 16];

   // pass a function to map
   const double = numbers.map(x => x * 2);
   
   console.log(double);
   // expected output: Array [2, 8, 18, 32]
```

### 6.3- Reduce

Effectively reduces the array to one value, iterating through each element where acumulator is the result of the last call.
The starting value of the acumulator is passed a second parameter and the last value returned is effectively the result of the reduce operation.

```javascript
   const numbers = [1, 2, 3, 4];
   const reducer = (accumulator, currentValue) => accumulator + currentValue;

   // 1 + 2 + 3 + 4
   console.log(numbers.reduce(reducer));
   // expected output: 10

   // 5 + 1 + 2 + 3 + 4
   console.log(numbers.reduce(reducer, 5));
   // expected output: 15
```

### 6.4- Filter

Returns an array with all the values from the original array for which the function retured a truthy value.

```javascript
   const isBigEnough = value => value >= 10;
    
   const filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
   // filtered is [12, 130, 44]
```

### 6.5- Others

Other useful methods for a _declarative_ approach.

* **some()**.
* **every()**.
* **forEach()**.
* **split()**.

&nbsp;

## 7- Code style

### 7.1- Line length limit

Line length limit should be between 80 and 100 characters.

### 7.2- Requires

The format we'll adopt for **requires** will be:
* **const** for each require.
* They will be separated in two blocks:
  * The first one will refer to external dependencies.
  * The second one will refer to internal dependencies.

```javascript
   const moment = require('moment');
   const lodash = require('lodash');

   const userService = require('../services/users');
   const userMapper = require('../mappers/users');

```

### 7.3- Destructuring

The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

This is really useful to refer specific values from modules.

```javascript
   const { create, update, delete } = require('../services/user');
```

A comprenhensive approach to destructuring may be found in the useful links section.

### 7.4- Implicit return

When using _arrow functions_ we can make the **return statement implicit** meaning that the result of evaluating the expression right of the arrow will be returned as a value. This saves writing **{}** and **return**.  

Yes, code will be shorter and neater but for more complex code, changing or debugging is noticeably more prone to errors. This is why, we enforce the use of the **implicit return** only in simple or short functions.

&nbsp;

## 8- Promise vs Async/Await

### 8.1- Promise

A _promise_ represents the eventual success or failure of an asynchronous operation. Promises have two methods:
* **then**: Takes a function as parameter which is executed once the promise is resolved with the result as parameter.
* **catch**: Takes a function as parameter which is executed once an exception is thrown with the exception as parameter.

There are many ways to handle correctly promises. Feel free to browse Maykol Purica's post about Promises in the useful links section.

### 8.2- Async/Await

Promise's _syntactic sugar_.
Specifying any function or arrow function as **async** specifies that the return value is a Promise.
**await** may only be used inside **async** functions. Using **await** makes the code flow block until the promise is _resolved_ or _rejected_.
**await** statements are usually within **try/catch** blocks.


## 9- Useful Links

- [Developing Better Node.js Developers][MaPiP] Post by Matias Pizzagalli's post.
- [Bootstrap][GEP] Post by Gonzalo Escandarani.
- [Promises][MaPuP] Post by Maykol Purica.
- [Destructuring][destructuring] Destructuring guide.
- [Status Codes][statusCodes] Status codes guide.

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [MaPiP]: <https://medium.com/wolox-driving-innovation/developing-better-node-js-developers-a176de770539> 
   [GEP]: <https://medium.com/wolox-driving-innovation/nodejs-api-bootstrap-b598a2591a3b>
   [MaPuP]: <https://medium.com/wolox-driving-innovation/how-to-code-better-async-javascript-e59363883c84>
   [destructuring]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment>
   [statusCodes]: <https://developer.mozilla.org/en-US/docs/Web/HTTP/Status>