## <a name="overview"></a>Información general

En este tutorial se crea la siguiente API:

|API | Descripción | Cuerpo de la solicitud | Cuerpo de la respuesta |
|--- | ---- | ---- | ---- |
|GET /api/todo | Obtener todas las tareas pendientes | Ninguna | Matriz de tareas pendientes|
|GET /api/todo/{id} | Obtener un elemento por identificador | Ninguna | Tarea pendiente|
|POST /api/todo | Agregar un nuevo elemento | Tarea pendiente | Tarea pendiente |
|PUT /api/todo/{id} | Actualizar un elemento existente &nbsp; | Tarea pendiente | Ninguna |
|DELETE /api/todo/{id} &nbsp; &nbsp; | Eliminar un elemento &nbsp; &nbsp; | Ninguna | Ninguna|

<br>

El siguiente diagrama muestra el diseño básico de la aplicación.

![El cliente está representado por un cuadro a la izquierda, envía una solicitud y recibe una respuesta de la aplicación, representada por un cuadro en la parte derecha. En el cuadro de la aplicación, hay tres cuadros que representan el controlador, el modelo y la capa de acceso a datos. La solicitud entra en el controlador de la aplicación y se producen operaciones de lectura/escritura entre el controlador y la capa de acceso a datos. El modelo se serializa y se devuelve al cliente en la respuesta.](../../tutorials/first-web-api/_static/architecture.png)

* El cliente es todo lo que consume la API web (aplicación móvil, explorador, etcétera). En este tutorial no se crea ningún cliente. [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) se utilizan como el cliente para probar la aplicación.

* Un *modelo* es un objeto que representa los datos de la aplicación. En este caso, el único modelo es una tarea pendiente. Los modelos se representan como clases de C#, también conocidas como clases POCO (del inglés **P**lain **O**ld **C**# **O**bject, objetos CRL antiguos sin formato).

* Un *controlador* es un objeto que controla solicitudes HTTP y crea la respuesta HTTP. Esta aplicación tiene un único controlador.

* Para simplificar el tutorial, la aplicación no usa una base de datos persistente. La aplicación de ejemplo almacena tareas pendientes en una base de datos en memoria.
