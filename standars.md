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
     - [3.2- Parámetros de entrada y salida de API (Pendiente)](#32--parámetros-de-entrada-y-salida-de-api-pendiente)
     - [3.3- Rutas (Pendiente)](#33--rutas-pendiente)
     - [3.4- Base de datos](#34--base-de-datos)
     - [3.5- Resto del código](#35--resto-del-código)
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

The model should be as simple as possible, just fields and Sequelize configurations. We try to not putting logic here but it is permitted to do an specific model validation as password requirements for an user.

### 2.4- Helpers

Herramientas técnicas como parsers, formatos de fechas, etc. **No debe haber nada con respecto al negocio**, son solo funciones que nos simplifican la vida y que podemos llegar a usar en otros proyectos.

### 2.5- Serializers

Formatea la response de un service o de un endpoint. Se utilizan cuando hay demasiada complejidad o repetición.

### 2.6- Interactors

Utilizado cuando el flujo del negocio es muy complejo o existen diferentes flujos. Con _muy complejos_ hacemos referencia a casos en el que se hacen llamadas a varios _servicies_ o hayan muchos calculos en el medio. Para estos casos crearemos un interactor y movemos las distintas interacciones del controller hacia este interactor.

### 2.7- Middlewares

Capas de abstracción, puesta antes de los controllers generalmente, que nos permiten realizar validaciones de distintos tipos, por ejemplo, validaciones de autenticación o validaciones de esquemas.

&nbsp;


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