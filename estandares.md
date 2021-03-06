# Estandáres y buenas practicas

   - [1- Objetivo](#1--objetivo)
   - [2- Estructura de APP](#2--estructura-de-app)
     - [2.1- Controllers](#21--controllers) 
     - [2.2- Services](#22--services)
     - [2.3- Models](#23--models)
     - [2.4- Helpers](#24--helpers)
     - [2.5- Serializers](#25--serializers)
     - [2.6- Interactors](#26--interactors)
     - [2.7- Middlewares](#27--middlewares)
     - [2.8- Mappers](#28--mappers)
   - [3- Convención de nombres](#3--convención-de-nombres)
     - [3.1- Nombre de archivos](#31--nombre-de-archivos)
     - [3.2- Parámetros de entrada y salida de API](#32--parámetros-de-entrada-y-salida-de-api)
     - [3.3- Rutas](#33--rutas)
     - [3.4- Base de datos](#34--base-de-datos)
     - [3.5- Resto del código](#35--resto-del-código)
   - [4- Condicionales](#4--condicionales)
     - [4.1- IFs](#41--ifs)
     - [4.2- Operador Ternario](#42--operador-ternario)
     - [4.3- Operador AND](#43--operador-and)
     - [4.4- Operador OR](#44--operador-or)
   - [5- Rest API](#5--rest-api)
     - [5.1- Mejores Prácticas](#51--mejores-prácticas)
     - [5.2- Status Code ERRORs](#52--status-code-errors)
   - [6- Funcional](#6--funcional)
     - [6.1- Introducción](#61--introducción)
     - [6.2- Map](#62--map)
     - [6.3- Reduce](#63--reduce)
     - [6.4- Filter](#64--filter)
     - [6.5- Otros](#65--otros)
   - [7- Estilo de código](#7--estilo-de-código)
     - [7.1- Límite del largo de las lineas](#71--límite-del-largo-de-las-lineas)
     - [7.2- Requires](#7.2--requires)
     - [7.3- Destructuring](#7.3--destructuring)
     - [7.4- Return implícito](#74--return-implícito)
   - [8- Promise vs Async/Await](#8--promise-vs-asyncawait)
     - [8.1- Promise](#81--promise)
     - [8.2- Async/Await](#82--async/await)
     - [8.3- ¿Cuándo usar cuál?](#83--cuándo-usar-cuál)
   - [9- Manejo de errores](#9--manejo-de-errores)
     - [9.1- Lanzamiento de errores](#91--lanzamiento-de-errores)
     - [9.2- Captura de errores](#92--captura-de-errores)
   - [10- Utilidades](#10--utilidades)
   - [11- Links Utiles](#11--links-utiles)

## 1- Objetivo

El objetivo del presente documento es presentar los estandares que usamos en Wolox para trabajar con NodeJS.
Se recomienda leer el post de Matias Pizzagalli cuyo link se encuentra en la sección de links útiles.

&nbsp;

## 2- Estructura de APP

La estructura que se usará incluyen, como mínimo, estas carpetas: (models e interactors si son necesarias para la API). De esta manera, podemos separar en distintas funcionalidades y responsabilidades. Para mayor información se encuentra el post de Gonzalo Escandarani en la sección de links útiles.

### 2.1- Controllers

Lugar donde se encuentra la lógica correspondiente a la request y response. Aquí se puede observar la lógica del negocio paso a paso a través de las interacciones con los **services, models, helpers, etc.** En el caso de que haya partes demasiados complejas se debería abstraer parte de la lógica a un **interactor**. Por último, utilizaremos un **serializer** para formatear la respuesta que daremos.

### 2.2- Services

Interactúa con servicios externos o con la base de datos. Debe ser lo más simple posible. Los services no deben interactuar entre ellos.

### 2.3- Models

Aquí se encuentra el esquema del modelo, junto a sus asociaciones.
Este debe ser lo más simple posible, solo los campos y las configuraciones del ORM utilizado. Intentamos no incluir la lógica aquí, pero está permitido realizar una validación de modelo específica como requisitos de contraseña para un usuario.

### 2.4- Helpers

Herramientas técnicas como parsers, formatos de fechas, etc. **No debe haber nada con respecto al negocio**, son solo funciones que nos simplifican la vida y que podemos llegar a usar en otros proyectos.

### 2.5- Serializers

Formatea la response de un service o de un endpoint. Se utilizan cuando hay demasiada complejidad o repetición.

### 2.6- Interactors

Utilizado cuando el flujo del negocio es muy complejo o existen diferentes flujos. Con _muy complejos_ hacemos referencia a casos en el que se hacen llamadas a varios _servicies_ o hayan muchos calculos en el medio. Para estos casos crearemos un interactor y movemos las distintas interacciones del controller hacia este interactor.

### 2.7- Middlewares

Capas de abstracción, puesta antes de los controllers generalmente, que nos permiten realizar validaciones de distintos tipos, por ejemplo, validaciones de autenticación o validaciones de esquemas.

### 2.8- Mappers

Se utilizan para centralizar la lógica correspondiente a convertir los datos que nos llegan en los objetos que manejamos en nuestra aplicación. Como ventaja también tenemos la reutilización de los mismos en los diferentes puntos de entrada donde apliquen.

&nbsp;

## 3- Convención de nombres

En este punto, definiremos una convencion para los distintos casos. La mayoria estan hechos para respetar el estandar http o el estandar de las bases de datos.

### 3.1- Nombre de archivos

Los nombres de los archivos deben ir en **snake_case**, no se debe repetir la entidad en el nombre del archivo, por ejemplo si tenemos el modelo user, el archivo deberá llamarse **user.js** en lugar de **user.model.js** o de **user_model.js**. 
Lo mismo para controllers, services, etc.

### 3.2- Parámetros de entrada y salida de API

Los parámetros de entrada / salida de una api serán en **camelCase**.

### 3.3- Rutas

Las rutas serán en **snake_case**.

### 3.4- Base de datos

El nombre de las tablas y de las columnas serán en **snake_case**.

### 3.5- Resto del código

Para el resto de código se usará **camelCase** por default. Esta fue una decisión de la mayoría de los desarrolladores, quedando en libertad de cada equipo cambiar al case que mas le convenga, siendo consistente entre todas las apis que se encuentren en el proyecto.

&nbsp;

## 4- Condicionales

### 4.1 - IFs

En el caso de que no sea necesario loguear y solamente sea una línea lo que se realiza dentro del **if** se debería evitar el uso de **{ }** y realizarlo en una única línea, por ejemplo:

```javascript
   if(!user) return next(errors.notFound('User not found');
```

Si la linea es afectada por el linter haciendo que sea más de una linea se deberá hacer uso de **{ }**.

Asimismo, se debe elegir correctamente la condición del if y evitar el uso del else, por ejemplo pasar de esto:

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
a esto:

```javascript
   if(!user) return next(errors.notFound('User not found');
    ..
    ..
    ....
    .
    ....
    ..
```
### 4.2- Operador Ternario

Cuando debemos asignar a una variable un determinado valor en base a una condición podemos utilizar el operador ternario de esta manera:

```javascript
   let variable = condicion ? valor_si_condicion_es_verdadera : valor_si_condicion_es_falsa;
```

Este operador es conveniente cuando la condicion no es dada directamente por un truthy o falsy value o cuando queremos asignar valores distintos de _undefined_.

Ejemplos:

```javascript
  const age = 26;
  const beverage = age >= 21 ? 'Beer' : 'Juice';
  console.log(beverage); // "Beer"

  const isStudent = true;
  const price = isStudent ? 8 : 12;
  console.log(price); // 8
```

En cambio, cuando tenemos estos casos es conveniente usar el operador **AND** o el operador **OR**.

```javascript
  let myObj;
  let value = myObj ? myObj.myKey : undefined; // Aqui conviene usar el operador AND.

  let myVar1;
  let myVar2;
  let myVar3 = 'myVar';
  let value = myVar1 ? myVar1 : myVar2 ? myVar2 : myVar3 ? myVar3 : undefined; // Aqui conviene el operador OR

```

### 4.3- Operador AND

Una manera de evitarnos la utilización del operador ternario es con el uso del operador **AND**. Este operador es conveniente cuando la condicion es dada directamente por un truthy o falsy value. 

```javascript
   let variable = indicator && value;

   /* Variable tomara el valor de value si indicator es verdadero. Si el valor de indicator es 0, false, Nan, null, undefined entonces variable tendra el valor de indicator.*/
```

Ejemplo:

```javascript
let myObj;

let value = myObj && myObj.myKey;
console.log(value) //undefined porque myObj es un falsy value, entonces asigna este valor.

myObj = {};
let value = myObj && myObj.myKey;
console.log(value) //undefined porque myObj es un truthy value, entonces asigna el valor de myObj.myKey el cual es undefined.

myObj.myKey = myValue;
let value = myObj && myObj.myKey;
console.log(value) //myValue porque myObj es un truthy value, entonces asigna el valor de myObj.myKey el cual es myValue.
```

### 4.4- Operador OR

Cuando debemos asignar a una variable un valor entre distintas posibilidades dependiendo de si son _truthy_ o _falsy_ podemos usar el operador **OR**. Este operador es conveniente cuando el valor a asignar depende directamente de su valor booleano asociado.

```javascript
   let variable = opcion_1 || opcion_2 || opcion_3;
    
   /* Variable tomara el valor de opcion_1 al menos que sea null o undefined, en ese caso tomara el valor de opcion_2, pero si este tambien es null o undefined tomara el valor de opcion_3 independientemente de su valor.*/
```

Ejemplo:

```javascript
  let foo, bar, baz;
  const name = foo || bar || baz || 'Sunshine';
  console.log(name); // Sunshine porque foo, bar y baz son undefined.
```

Este ejemplo se podria escribir con `if/else` y observamos que es mucho mas largo.

```javascript
  let foo, bar, baz, name;

  if (foo) name = foo;
  else if (bar) name = bar;
  else if (baz) name = baz;
  else name = 'Sunshine';

  console.log(name); // Sunshine porque foo, bar y baz son undefined.
```

&nbsp;

## 5- Rest API

Normalmente usamos un diseño RESTful para nuestras APIs. El concepto de REST es separar la estructura de la API en recursos lógicos. Se utilizan los métodos HTTP **GET**, **DELETE**, **POST** y **PUT** para operar con los recursos.

### 5.1- Mejores Prácticas

Para realizar una buena API debemos respetar las siguientes practicas:

* **Usar sustantivos (plural) en lugar de verbos**: Con el método GET utilizar _/users_ para obtener el listado de usuarios en lugar de _getUsers_.
* **Metodo GET no deben alterar estados**: Solo debe devolver cosas, no modificarlas.
* **Si un recurso está relacionado con otro, usar sub-recursos**: Por ejemplo, para obtener la informacion del conductor 2 del auto 4 usar GET _/cars/4/drivers/2_
* **Proporcionar filtrado, clasificación, selección de campos y paginación**: Utilizar query params para aplicar distintas opciones a la obtención de datos a través del método GET.
* **Versionar la API**: La versión de la API debe ser obligatoria.


### 5.2- Status Code ERRORs

Existen diversos _status codes_ para describir la respuesta de nuestras requests. \
A continuación dejaremos un listado de los mas usados:

* **200 OK**: Respuesta estandar para una request exitosa. Depende del método HTTP que se utiliza. 
* **201 CREATED**: La request fue exitosa, se creó un nuevo recurso. Usado generalmente en POST y algunas veces con PUT.
* **204 NO CONTENT**: La request fue exitosa pero no se envio contenido en la respuesta.
* **400 BAD REQUEST**: Esta respuesta significa que el servidor no pudo entender la solicitud debido a una sintaxis no válida.
* **401 UNAUTHORIZED**: La request falló debido a que no esta autenticado para realizar esa acción.
* **403 FORBIDDEN**: La request falló debido a que no esta autorizado para realizar esa acción.
* **404 NOT FOUND**: El servidor no puede encontrar el recurso solicitado.
* **422 UNPROCESSABLE ENTITY**: Debe usarse si el servidor no puede procesar la request, por ejemplo, si una imagen no puede ser formateada o faltan campos obligatorios (body, query, params) en la request.
* **500 INTERNAL SERVER ERROR**: El servidor ha encontrado una situación que no sabe cómo manejar.

En la sección de links útiles se encontrará mas información respecto a los status codes.

&nbsp;

## 6- Funcional

En la medida que se pueda, debemos utilizar funcionalidades provenientes del paradigma funcional como **Map, Reduce, Filter, etc.**

### 6.1- Introducción

La programación funcional es un paradigma _declarativo_ mientras la programación por procedimientos y OOP (programación orientada a objetos) son paradigmas _imperativos_.

En el paradigma _declarativo_, a diferencia del _imperativo_, expresamos nuestra lógica sin describir el **control de flujo** (bucles, condicionales, etc.), enfocándonos en **qué** estamos haciendo, en vez de **cómo** se está haciendo, que sería el enfoque _imperativo_.

### 6.2- Map

Devuelve un nuevo array con el resultado de aplicar una función a cada elemento de un array.

```javascript
   const numbers = [1, 4, 9, 16];

   // pass a function to map
   const double = numbers.map(x => x * 2);
   
   console.log(double);
   // expected output: Array [2, 8, 18, 32]
```

### 6.3- Reduce

Ejecuta una función reductora en cada elemento del array que resulta en un solo valor de salida.
El valor devuelto por la función reductora se asigna al acumulador, cuyo valor es recordado durante la iteración del array y, en última instancia, se convierte en el valor final.

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

Aplica una función a cada elemento del array, crea y devuelve un nuevo array con los elementos que cumplen con la condición impuesta.

```javascript
   const isBigEnough = value => value >= 10;
    
   const filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
   // filtered is [12, 130, 44]
```

### 6.5- Otros

Otros métodos importantes son:

* **some()**.
* **every()**.
* **forEach()**.
* **split()**.

&nbsp;

## 7- Estilo de código

### 7.1- Límite del largo de las lineas

El limite del largo de las lineas será entre 80 y 100 caracteres.

### 7.2- Requires

El formato que utilizaremos para los **requires** será:
* **const** para cada require.
* Estarán separados en dos bloques:
  * El primero contará con los requires correspondientes a dependencias externas.
  * El segundo contará con los requires correspondientes a dependencias internas.

```javascript
   const moment = require('moment');
   const lodash = require('lodash');

   const userService = require('../services/users');
   const userMapper = require('../mappers/users');

   // Otro bloque con las demas constantes.

```

### 7.3- Destructuring

La sintaxis de desestructuración es una expresión de JavaScript que hace posible desempaquetar valores de arrays, o propiedades de objetos, en variables distintas.

Tambien podemos aprovechar esto para traernos constantes o funciones especificas en un require.

```javascript
   const { create, update, delete } = require('../services/user');
```

En la sección de links útiles se encuentra una buena guia de destructuring.

### 7.4- Return implícito

Al utilizar _arrow functions_ tenemos la posibilidad de utilizar **returns implícitos** esto significa que no es necesario encerrar la función entre **{ }** y colocar el correspondiente **return**.

Si bien esto nos hace tener un código un poco mas corto, si necesitamos hacer cambios podemos cometer errores los cuales nos consumirá tiempo al debuguear el código.

Por lo tanto, solo se utlizará el **return implícito** en funciones sencillas.

&nbsp;

## 8- Promise vs Async/Await

### 8.1- Promise

La _promise_ representa la finalización (o falla) eventual de una operación asíncrona y su valor resultante. Consta de dos métodos:
* **then**: Este método es ejecutado si la promise fue resuelta.
* **catch**: Este método es ejecutado si la promise fue rechazada.

Existen diversas formas de utilizar las promises, una buena guía se encuentra en el siguiente post de Maykol Purica, ver sección de links útiles.

### 8.2- Async/Await

Es una _syntactic sugar_ de las promises.
Agregar **async** delante de una función hace que esta devuelva siempre una promise.
**await** solamente puede ser usado dentro de una **async function**, esta espera hasta que la promise sea resuelta para continuar con la ejecución del codigo. Se la suele usar dentro de un bloque **try/catch**.

### 8.3- ¿Cuándo usar cuál?

Siempre priorizar el uso de **promises**.  
Hay un caso especial donde conviene usar async/await. El mismo es cuando una promise se ejecuta dentro de un if y luego de ese if se continúa con el flujo principal de la función. Usando promises tendríamos:

```javascript
  if (order.state === CANCELLED) {
    return deleteProduct(order.product)
      .then(() => sendEmail({
        id: order.id,
        state: order.state
      }))
      .then(response => ...)
  }
 
  return sendEmail({
    id: order.id,
    state: order.state
  })
    .then(response => ...)
```

Notar que quedó repetida la cadena de promises. A medida que esta crezca el código duplicado será cada vez más grande.  
En cambio usando **async/await**:

```javascript
  if (order.state === CANCELLED) {
    try {
      await deleteProduct(order.product);
    } catch (e) {
      ...
    }
  }
 
  return sendEmail({
    id: order.id,
    state: order.state
  })
    .then(response => ...)
```

Cuando usamos este approach, dejamos todas las promises de la función con async/await para que el código sea uniforme.

```javascript
  try {
    if (order.state === CANCELLED) {
      await deleteProduct(order.product);
    }
 
    const response = await sendEmail({ id: order.id, state: order.state });
    ...
  } catch (e) {
    ...
  }
```

&nbsp;

## 9- Manejo de errores

### 9.1- Lanzamiento de errores

Hay dos maneras de hacerlo. Las mismas son casi idénticas salvo por lo mencionado [acá](https://stackoverflow.com/questions/33445415/javascript-promises-reject-vs-throw).

```javascript
   throw errors.notFound('User not found');
```
ó
```javascript
   return Promise.reject(errors.notFound('User not found'));
```

Preferir el uso de **Promise.reject** cuando el error se lance dentro de una promise ya que es más Node-friendly.  
Ser uniforme con una opción para lograr la prolijidad del código.

### 9.2- Captura de errores

Se deberá utilizar un **catch** para handlear el error.

En el caso que se quiera realizar la response del request con el error, se deberá ejecutar la _función de middleware de error_.

```javascript
   return next(errors.notFound('User not found'));
```
Cuando a la función **next** se le pasa un parámetro, Express ya sabe que debe ir a la función de middleware de error, sin importar las demás funciones que haya entre medio.

## 10- Utilidades

En esta seccion dejaremos unos cuantos **packages** que nos ayudaran a resolver muchos de nuestros problemas.

* **[LODASH][lodash]**: Una moderna biblioteca de utilidades de JavaScript que ofrece modularidad, rendimiento y extras.
* **[LODASH FP][lodash-fp]**: Lodash orientado a la programacion funcional.
* **[RAMBDA][rambda]**: Libreria orientada a la programacion funcional.
* **[MOMENT][moment]**: Analice, valide, manipule y muestre fechas y horas en JavaScript.

## 11- Links Utiles

- [Developing Better Node.js Developers][MaPiP] Post de Matias Pizzagalli.
- [Bootstrap][GEP] Post de Gonzalo Escandarani.
- [Promises][MaPuP] Post de Maykol Purica.
- [Destructuring][destructuring] Guia sobre destructuring.
- [Status Codes][statusCodes] Guia de status codes.

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [MaPiP]: <https://medium.com/wolox-driving-innovation/developing-better-node-js-developers-a176de770539> 
   [GEP]: <https://medium.com/wolox-driving-innovation/nodejs-api-bootstrap-b598a2591a3b>
   [MaPuP]: <https://medium.com/wolox-driving-innovation/how-to-code-better-async-javascript-e59363883c84>
   [destructuring]: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment>
   [statusCodes]: <https://developer.mozilla.org/en-US/docs/Web/HTTP/Status>

   [lodash]: <https://lodash.com/>
   [lodash-fp]: <https://github.com/lodash/lodash/wiki/FP-Guide>
   [rambda]: <https://ramdajs.com/>
   [moment]: <https://momentjs.com/>