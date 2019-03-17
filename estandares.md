# Estandáres y buenas practicas

## 1- Objetivo

El objetivo del presente documento es presentar los estandares que usamos en Wolox para trabajar con NodeJS.
Se recomienda leer el post de Matias Pizzagalli [Developing Better Node.js Developers][MaPiP]

## 2- Estructura de APP

La estructura que se usará incluyen, como mínimo, estas carpetas: (models e interactors si son necesarias para la API). Para mayor información se encuentra el post de Gonzalo Escandarani: [Bootstrap][GEP].

### 2.1- Controllers

Lugar donde se encuentra la lógica correspondiente a la request y response. Aquí se puede observar la lógica del negocio paso a paso a través de las interacciones con los **services, models, helpers, etc.** En el caso de que haya partes demasiados complejas se debería abstraer parte de la lógica a un **interactor**. Por último, utilizaremos un **serializer** para formatear la respuesta que daremos.

### 2.2- Services

Interactúa con servicios externos o con la base de datos. Debe ser lo más simple posible. Los services no deben interactuar entre ellos.

### 2.3- Models

Aquí se encuentra el esquema del modelo, junto a sus asociaciones.

### 2.4- Helpers

Herramientas técnicas como parsers, formatos de fechas, etc. No debe haber nada con respecto al negocio, son solo funciones que nos simplifican la vida.

### 2.5- Serializers

Formatea la response de un service o de un endpoint.

### 2.6- Interactors

Utilizado cuando el flujo del negocio es muy complejo o existen diferentes flujos. Para estos casos crearemos un interactor y movemos las distintas interacciones del controller hacia este interactor.

### 2.7- Middlewares

Capas de abstracción, puesta antes de los controllers generalmente, que nos permiten realizar validaciones de distintos tipos, por ejemplo, validaciones de autenticación o validaciones de esquemas.

## 3- Convención de nombres

### 3.1- Nombre de archivos

Los nombres de los archivos deben ir en **snake_case**, no se debe repetir la entidad en el nombre del archivo, por ejemplo si tenemos el modelo user, el archivo deberá llamarse **user.js** en lugar de **user.model.js** o de **user_model.js**. 
Lo mismo para controllers, services, etc.

### 3.2- Parámetros de entrada y salida de API (Pendiente)

Los parámetros de entrada / salida de una api sera en **snake_case**.

### 3.3- Rutas (Pendiente)

Las rutas serán en **snake_case**.

### 3.4- Base de datos

El nombre de las tablas y de las columnas serán en **snake_case**.

### 3.5- Resto del código

Para el resto de código se usará **camelCase**.

## 4- Condicionales



## 5- Rest API

## 6- Funcional

## 7- Promise vs Async/Await



[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [MaPiP]: <https://medium.com/wolox-driving-innovation/developing-better-node-js-developers-a176de770539> 
   [GEP]: <https://medium.com/wolox-driving-innovation/nodejs-api-bootstrap-b598a2591a3b>

   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>