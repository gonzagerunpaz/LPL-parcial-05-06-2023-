# Laboratorio de Programación y Lenguajes 

## Parcial 05.Junio.2023

Se le solicita a los alumnos del laboratorio de programación y lenguajes de la UNPAZ que realicen el desarrollo de la siguiente api que le perminte gestionar a la universidad la solicitud de viandas en las distittas sedes.

* pedidos (/api/pedidos) 
* viandas (/api/viandas)
* alumnos (/api/alumnos)

Los tres recursos trabajan en conjunto y permiten a la universidad gestionar correctamente las solicitudes de viandas que serán otorgadas a los alumnos que generen los pedidos.

Un alumno podrá generar un pedido de una vianda siempre y cuando se encuentre habilitado para hacerlo, y el sistema le asignará la primer vianda que tenga stock disponible y que además se ajuste al pedido del alumno 

- [x] el alumno se debe encontrar habiltiado para realizar el pedido
- [x] la vianda se debe ajustar al tipo solicitado, también debe ajustarse a la condición del alumno si es celiaco o no y tiene que tener stock disponible (stock > 0)
- [x] si la vianda es asignada al alumno deben ocurrir dos cosas
- 1. El stock de la vianda debe disminuir en 1 unidad.
- 2. El alumno debe pasar a estar deshabilitado para pedir vianda


## Consideraciones Generales

El desarrollo deberá realizarse en **_NodeJs_** utilizando la liberería **_express_**. El código **obligatoriamente**  deberá tener rutas y controladores para gestiona los request y response. Revisar la estructura de directorios propuesta en este repositorio.

Toda la información deberá gestionarse en memoria, es recomendable utilizar la librería **_nodemon_** para ir viendo los cambios sin necesidad de cortar el servidor de express y volverlo a levantar.

La entrega deberá ser en un repositorio de [github](https://github.com) donde el README.md deberá al menos incluir el nombre, apellido y dni del alumno, y escribir todas las cosas que asumieron para resolver el ejercicio o consideraciones que crean conveniente realizar.

### Alumnos

Realizar el endpoint **/api/alumos** que permita realizar la consulta total de los alumnos, más la creación, borrado,  modificacion y consulta de un alumno en particular a través de su número de dni.

* _GET_ **/api/alumnos** - Recupera el array de alumnos.
* _GET_ **/api/alumnos/:dni** - Recupera el alumno a través del dni que es pasado en el path de la URL como parámetro.
* _PUT_ **/api/alumnos/:dni** - Permite la modificación de solo los atributos **_habilitado, celiaco y edad** del alumno con el dni que es pasado en el path de la URL como parámetro.
* _POST_ **/api/alumno** - Registra un nuevo alumno.

En la registración de un nuevo alumnos _POST_ **/api/alumnos** hay que considerar:
1. El dni del alumno debe tener 8 dígitos númericos.
2. Validar que el alumno no se encuentre registrado previamente. La comprobación se debé realizar por el atributo dni.
3. El alumno siempre ingresa habilitado **_no es necesario que pasen el atributo_**. Luego el sistema lo podrá cambiar.
4. La **edad** de la alumno debe ser siempre mayor a 18 y menor a 99 años.
5. Si el atributo **celiaco** no es informado en el POST se deberá asignar el valor false. En caso contrario se deberá asigar el valor que es informado en el body.

### Estructura del objeto alumno

``` JSON
    {
        "dni": Number,
        "nombre": String,
        "habilitado": Boolean,
        "celiaco": Boolan,
        "edad": Number
    }

```
### Viandas

Realizar el endpoint **/api/viandas** que permita realizar la consulta total de las viandas, más la creación y modificacion y consulta de una vianda en particular a través de su código.

* _GET_ **/api/viandas** - Recupera el array de viandas.
* _GET_ **/api/viandas/:codigo** - Recupera la vianda a través del código que es pasado en el path de la URL como parámetro.
* _PUT_ **/api/viandas/:codigo** - Permite la modificación del atributo **aptoCeliaco y stock** de la vianda con el código que es pasado en el path de la URL como parámetro.
* _POST_ **/api/vianda** - Registra una nueva vianda,

En la registración de una nueva vianda _POST_ **/api/viandas** hay que considerar:
1. El código de vianda es un string de 5 caracteres y siempre de comenzar con la letra 'V'.
2. Validar que la vianda no se encuentre registrada previamente. La comprobación se debé realizar por el atributo código.
3. El tipo de vianda solo puede ser uno de los valores disponibles. Los valores disponibles son: CARNE, POLLO, PASTA, PIZZA, EMPANADAS
4. El **stock** debe ser siempre mayor a 0.


### Estructura del vianda

``` JSON
    {
        "codigo": String,
        "tipo": String,
        "aptoCeliaco": Boolean,
        "stock": Numbrer,
        "descripcion": String
    }

```
### Pedidos

Realizar el endpoint **/api/pedidos** que permita consultar el total de los pedidos, la consulta de un pedido puntual a través del identificador único que genera la api y además la creación de un nuevo pedido. 

* _GET_ **/api/pedidos** - Recupera el array de pedidos.
* _GET_ **/api/pedidos/:id** - Recupera la vianda con el id pasado en el path de la URL como parámetro.
* _POST_ **/api/pedido** - Registra un nuevo pedido.


En la registración de un nuevo pedido _POST_ **/api/pedido** hay que considerar:

1. el id (identeficador único del pedido) lo deberá calcular el sistema siguiendo una secuencia incremental.
2. Registrar la fecha del pedido lo deberá calcular el sistema tomando la fecha actual. Ayuda: ```new Date().toISOString().slice(0, 10)```
3. En el body se recibe el dni de alumno y se regista todos los atributos del alumnos.
4. el el body se recibé el tipo de vianda y el sistema asigna la vianda que corresponde 


## Lógica que debe implementar la creación de un pedido
- [x] el alumno se debe encontrar habiltiado para realizar el pedido
- [x] la vianda se debe ajustar al tipo solicitado, también debe ajustarse a la condición del alumno si es celiaco o no y tiene que tener stock disponible (stock > 0)
- [x] si la vianda es asignada al alumno deben ocurrir dos cosas
- 1. El stock de la vianda debe disminuir en 1 unidad.
- 2. El alumno debe pasar a estar deshabilitado para pedir vianda

### Estructura del body para la creación de un pedido
``` JSON
    {
        "dni": Numbrer,
        "tipo": String
    }
```

### Estructura del objeto PEDIDO para los GET
``` JSON
    {
        "id": Number,
        "fecha": String,
        "alumno": {
            "dni": Number,
            "nombre": String,
            "celiaco": Boolan,
            "edad": Number
        },
        "vianda":{
            "codigo": String,
            "tipo": String,
            "aptoCeliaco": Boolean,
            "descripcion": String
        }
    }
```

La vianda deberá registrarse de acuerdo a las conidciones que fueron detalladas. Si existe algún error en los datos enviados (por ejemplo: el los tipos permitidos) o no es posible la creación del pedido el sistema deberá retornar el código correspondiente a un **bad request** indicando un mensajes descriptivo del error.

## PUNTOS BONUS

Adicionalmente también se deberá poder buscar el último pedido realizado por nombre del alumnos

* GET **/api/pedidos/search?nombre=XXXX** - Recupera la ultima el último pedido que realizo el alumno. En caso de no encontrar reservas deberá retornar "No encontrado"

* _DELETE_ **/api/pedidos/:id** - Borra el pedido con el id pasado en el path de la URL como parámetro. Incrementa en una unidad la viana del pedido y pone habilitado a alumno.


## Ejemplos de la creación de un peddio

Si tuvieramos los siguientes alumnos

|dni     |Nombre     |habilitado|celiaco|edad|
|--------|-----------|----------|-------|----|
|39911147|FLORENCIA  |true      |false  |23  |
|37535032|CHRISTIAN  |false     |false  |24  |
|33199201|ROMINA     |false     |true   |28  |
|34701204|JUAN       |true      |true   |27  |


y tuvieramos las siguientes viandas 

|codigo  |tipo       |aptoCeliaco|stock|descripcion              |
|--------|-----------|---------- |-----|-------------------------|
|VCARN   |CARNE      |true       |11   |CARNE                    |
|VPZAP   |PIZZA      |true       |0    |PIZZA APTO CELICO        |
|VPZNA   |PIZZA      |false      |98   |PIZZA NO APTO CELIACO    |
|VEMAP   |EMPANADAS  |true       |75   |EMPANADAS APTO CELIACO   |
|VENOP   |EMPANADAS  |false      |35   |EMPANADAS NO APTO CELIACO|
|VPOLO   |POLLO      |true       |0    |POLLO                    |
|VPAAP   |PASTA      |true       |14   |PASTA APTO CELIACO       |
|VPANP   |PASTA      |false      |0    |PASTA NO APTO CELIACO    |


### Ejemplo 1
Si se hicera el siguiente POST con este body 
``` JSON
{
    {
        "dni": 34701204,
        "tipo": "VENOP"
    }
}
```
El resultado el resultado sería el siguiente
``` JSON
    {
        "id": 1,
        "fecha": "2023-06-05",
        "alumno":{
            "dni": 39911147,
            "nombre": "FLORENCIA",
            "celiaco": false,
            "edad": 23
        },
        "vianda": {
            "codigo": "VENOP",
            "tipo": "EMPANADAS",
            "aptoCeliaco": false,
            "descripcion": "EMPANADAS NO APTO CELIACO"
        }
    }
```
El alunno se encuentra habilitado y no es celiaco, y pidio un codigo de vianda que no es apto para celiacos y que tiene stock. Y además luego de registrar el pedido el alumno debe quedar con **"habilitado": false** y la vianda con una undad menos

#### Alumno con el estado cambiado
consultando **_GET /api/alumnos/39911147_**
``` JSON
   {
        "dni": 39911147,
        "nombre": "FLORENCIA",
        "habilitado": false,
        "celiaco": false,
        "edad": 23
    }
```
#### Vianda con al unidad disminuida
consultando **_GET /api/viandas/VENOP_**
``` JSON
   {
        "codigo": "VENOP",
        "tipo": "EMPANADAS",
        "aptoCeliaco": false,
        "stock": 34,
        "descripcion": "EMPANADAS NO APTO CELIACO"
    }
```
### Ejemplo 2

Si se hicera el siguiente POST con este body 
``` JSON
{
    {
        "dni": 39911147,
        "tipo": "VPAAP"
    }
}
```
El resultado el resultado sería el siguiente
``` JSON
    {
        "id": 2,
        "fecha": "2023-06-05",
        "alumno": {
            "dni": 34701204,
            "nombre": "JUAN",
            "celiaco": true,
            "edad": 27
    },
        "vianda": {
            "codigo": "VPAAP",
            "tipo": "PASTA",
            "aptoCeliaco": true,
            "descripcion": "PASTA APTO CELIACO"
        }
    }
```
El alunno se encuentra habilitado, **es celiaco** y pidio un codigo de vianda que no **apto para celiacos** y que **tiene stock** . Y además luego de registrar el pedido el alumno debe quedar con **"habilitado": false** y la vianda con una undad menos

#### Alumno con el estado cambiado
consultando **_GET /api/alumnos/34701204_**
``` JSON
    {
        "dni": 34701204,
        "nombre": "JUAN",
        "habilitado": false,
        "celiaco": true,
        "edad": 27
    }
```
#### Vianda con al unidad disminuida
consultando **_GET /api/viandas/VPAAP**
``` JSON
     {
        "codigo": "VPAAP",
        "tipo": "PASTA",
        "aptoCeliaco": true,
        "stock": 13,
        "descripcion": "PASTA APTO CELIACO"
    }
```

### Ejemplo 3

Si se hicera el siguiente POST con este body 
``` JSON
{
    {
        "dni": 37535032,
        "tipo": "VENOP"
    }
}
```
Debería dar un **bad request** porque el alumno no se encuentra habilitado para realizar el pedido de vianda

### Ejemplo 4

Si se hicera el siguiente POST con este body 
``` JSON
{
    {
        "dni": 37535032,
        "tipo": "VPZNA"
    }
}
```

Debería dar un **bad request** porque si bien el alumno se encuentra habilitado para realizar el peidido y aunque la vianda tiene stock esa vianda que esta pidiendo no es apta porque el alumno es celiaco.

### Ejemplo 5

Si se hicera el siguiente POST con este body 
``` JSON
{
    {
        "dni": 37535032,
        "tipo": "VPZAP"
    }
}

Debería dar un **bad request** porque si bien el alumno se encuentra habilitado para realizar el peidido y aunque la vianda es apta para celiacos no tiene stock disponible