---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Iteración 6 #: Use desarrollo controlado por pruebas (VB) | Documentos de Microsoft'
author: microsoft
description: En esta iteración sexto, se agregan nuevas funciones a nuestra aplicación escribiendo pruebas unitarias en primer lugar y escribir código frente a las pruebas unitarias. En esta iteración...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 71b3425c5ca8cbfc1b89493c7afb26681f8bdc9d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877598"
---
<a name="iteration-6--use-test-driven-development-vb"></a>Iteración 6 #: Use desarrollo controlado por pruebas (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> En esta iteración sexto, se agregan nuevas funciones a nuestra aplicación escribiendo pruebas unitarias en primer lugar y escribir código frente a las pruebas unitarias. En esta iteración, agregamos grupos de contactos.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de MVC de ASP.NET (VB) de administración de contactos
  

En esta serie de tutoriales, creamos una aplicación de administración de contactos toda desde el principio para finalizar. La aplicación de ponerse en contacto con el administrador permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Se compile la aplicación en varias iteraciones. Con cada iteración, se mejora gradualmente la aplicación. El objetivo de este enfoque de iteración varias es para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el póngase en contacto con el Administrador de la manera más sencilla posible. Se agrega compatibilidad para las operaciones de base de datos básicos: crear, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación sea bonito. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de vista de MVC de ASP.NET predeterminada y en cascada hoja de estilos.

- Iteración #3: agregar validación del formulario. En la tercera iteración, agregamos la validación del formulario básico. Es evitar que los usuarios enviar un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación de acoplamiento flexible. En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que resulten más fáciles de mantener y modificar la aplicación póngase en contacto con el administrador. Por ejemplo, se refactoriza nuestra aplicación para que use el modelo de repositorio y el modelo de inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinto, hacemos nuestra aplicación más fácil de mantener y modificar mediante la adición de pruebas unitarias. Hemos simular nuestro clases del modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración 6 #: Use desarrollo controlado por pruebas. En esta iteración sexto, se agregan nuevas funciones a nuestra aplicación escribiendo pruebas unitarias en primer lugar y escribir código frente a las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

- Iteración #7 - agregar funcionalidad de Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad para Ajax.

## <a name="this-iteration"></a>Esta iteración

En las iteraciones anteriores de la aplicación Administrador de contacto, creamos pruebas unitarias para proporcionar una red de seguridad para el código. La motivación para crear las pruebas unitarias fue hacer que el código más resistente a cambiar. Con pruebas unitarias en su lugar, podemos, realizar cualquier cambio en el código y saber inmediatamente si se ha interrumpido la funcionalidad existente.

En esta iteración, utilizamos pruebas unitarias para un propósito completamente diferente. En esta iteración, se utilizan pruebas unitarias como parte de una filosofía de diseño de la aplicación llama a *desarrollo controlado por pruebas*. Cuando practicar el desarrollo controlado por pruebas, escribe pruebas en primer lugar y, a continuación, escribe código en las pruebas.

Más concretamente, al practicando con desarrollo controlado por pruebas, hay tres pasos que seguir al crear el código (rojo / verde/refactorizar):

1. Escribir una prueba unitaria que se produce un error (rojo)
2. Escribir el código que pasa la prueba unitaria (verde)
3. Refactorizar el código (refactorización)

En primer lugar, escribir la prueba unitaria. La prueba unitaria debe formularse de su intención de cómo se espera que el código se comportan. Cuando se crea por primera vez la prueba unitaria, debe generar un error en la prueba unitaria. La prueba debe generar un error porque todavía no ha escrito ningún código de aplicación que supera la prueba.

A continuación, escriba únicamente el código suficiente en orden para que pase la prueba unitaria. El objetivo consiste en escribir el código en la forma laziest, sloppiest y más rápido posible. No debe perder tiempo a pensar acerca de la arquitectura de la aplicación. En su lugar, debe centrarse en escribir la cantidad mínima de código necesario para satisfacer la intención expresada por la prueba unitaria.

Finalmente, después de haber escrito el código suficiente, puede retroceder y considerar la arquitectura global de la aplicación. En este paso, reescriba (refactorización) patrones de su código aprovechando las ventajas de diseño de software, como el modelo de repositorio: para que el código sea más fácil de mantener. Fearlessly puede volver a escribir el código en este paso porque el código está cubierto por las pruebas unitarias.

Hay muchas ventajas resultantes practicando con el desarrollo controlado por pruebas. En primer lugar, controlado por pruebas desarrollo obliga a centrarse en el código que realmente debe escribirse. Dado que constantemente se centra en escribir simplemente el código suficiente para pasar una prueba determinada, no se permite desde adentra en las plantas y escritura de grandes cantidades de código que no utilizará nunca.

En segundo lugar, una metodología de diseño "probar primero", tendrá que escribir código desde la perspectiva de cómo se usará el código. En otras palabras, cuando practicando con desarrollo controlado por pruebas, se escribe constantemente las pruebas desde la perspectiva del usuario. Por lo tanto, puede dar lugar a desarrollo controlado por pruebas en las API más limpia y sea más comprensibles.

Por último, desarrollo controlado por pruebas, tendrá que escribir pruebas unitarias como parte del proceso normal de la escritura de una aplicación. Tal y como se acerca a una fecha límite del proyecto, prueba suele ser lo primero que sale de la ventana. Cuando practicando con el desarrollo controlado por pruebas, por otro lado, es más probable que sean virtuoso sobre cómo escribir pruebas unitarias porque desarrollo controlado por pruebas no realiza pruebas unitarias central en el proceso de creación de una aplicación.

> [!NOTE] 
> 
> Para más información acerca del desarrollo controlado por pruebas, recomienda leer el libro de Michael Feathers **trabajar eficazmente con código heredado**.


En esta iteración, agregamos una nueva característica a nuestra aplicación póngase en contacto con el administrador. Se agregará compatibilidad para grupos de contactos. Puede usar grupos de contactos para organizar los contactos en categorías, como Business y grupos de confianza.

Esta nueva funcionalidad vamos a agregar a nuestra aplicación siguiendo un proceso de desarrollo controlado por pruebas. Escribiremos nuestras pruebas unitarias en primer lugar y se escribirá todo el código en estas pruebas.

## <a name="what-gets-tested"></a>¿Qué Obtiene probado

Como se explicó en la iteración anterior, normalmente no escribir pruebas unitarias para la lógica de acceso a datos, o ver lógica. Cuando no las pruebas de unidad lógica de acceso a datos t escritura porque el acceso a una base de datos es una operación relativamente lenta. Cuando no las pruebas de unidad de lógica de vista t escritura porque el acceso a una vista requiere poner en marcha un servidor web que es una operación relativamente lenta. T no debe escribir una prueba unitaria a menos que la prueba se puede ejecutar una y otra vez muy rápido

Dado el desarrollo controlado por pruebas está controlado por pruebas unitarias, nos centramos inicialmente sobre la escritura de controlador y la lógica empresarial. Se evite tocar la base de datos o vistas. Ganados t modificar la base de datos o crear nuestra vistas hasta el final de este tutorial. Empezaremos con lo que se puede probar.

## <a name="creating-user-stories"></a>Crear casos de usuario

Cuando la práctica de desarrollo controlado por pruebas, siempre se inicia mediante la escritura de una prueba. Esto genera inmediatamente la pregunta: ¿cómo se decide qué prueba de escribir en primer lugar? Para responder a esta pregunta, debe escribir un conjunto de [ *casos de usuario*](http://en.wikipedia.org/wiki/User_stories).

Un caso de usuario es una descripción (normalmente una oración) muy breve de un requisito de software. Debe ser una descripción técnica de un requisito que se escriben desde la perspectiva del usuario.

Aquí s el conjunto de casos de usuario que describen las características requeridas por la nueva funcionalidad de grupo de contactos:

1. Usuario puede ver una lista de grupos de contactos.
2. Usuario puede crear un nuevo grupo de contactos.
3. Usuario puede eliminar un grupo de contactos existente.
4. Usuario puede seleccionar un grupo de contactos al crear un nuevo contacto.
5. Usuario puede seleccionar un grupo de contactos al editar un contacto ya existente.
6. Se muestra una lista de grupos de contactos en la vista de índice.
7. Cuando un usuario hace clic en un grupo de contactos, se muestra una lista de contactos coincidentes.

Tenga en cuenta que esta lista de casos de usuario es completamente comprensible por un cliente. No hay ninguna mención de detalles de la implementación técnica.

Mientras que en el proceso de creación de la aplicación, el conjunto de casos de usuario puede llegar a ser más específicos. Un caso de usuario se podría dividir en varios casos (requisitos). Por ejemplo, podría decidir que crear un nuevo grupo de contactos debe implican la validación. Envío de un grupo de contactos sin un nombre, debe devolver un error de validación.

Después de crear una lista de casos de usuario, está listo para escribir las primeras pruebas unitarias. Comenzaremos por crear una prueba unitaria para ver la lista de grupos de contactos.

## <a name="listing-contact-groups"></a>Lista de grupos de contactos

Nuestro primer caso de usuario es que un usuario pueda ver una lista de grupos de contactos. Es necesario expresar este artículo con una prueba.

Crear una nueva prueba unitaria haciendo clic en la carpeta de controladores en el proyecto ContactManager.Tests, seleccionar **agregar, nueva prueba**y seleccionando la **de prueba unitaria** plantilla (consulte la figura 1). Nombre de la nueva unidad GroupControllerTest.vb de prueba y haga clic en el **Aceptar** botón.


[![Agregar la prueba unitaria de GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Figura 01**: agregar la prueba unitaria de GroupControllerTest ([haga clic aquí para ver la imagen a tamaño completo](iteration-6-use-test-driven-development-vb/_static/image2.png))


La primera prueba unitaria se encuentra en la lista 1. Esta prueba comprueba que el método Index() del controlador al grupo devuelve un conjunto de grupos. La prueba comprueba que una colección de grupos aparece en la vista datos.

**Lista 1 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Cuando se escribe primero el código en el listado 1 en Visual Studio, obtendrá una gran cantidad de líneas onduladas rojas. No hemos creado las clases de GroupController o grupo.

En este momento, podemos compilación incluso t nuestra aplicación, por lo que se puede ejecutar la primera prueba unitaria. S buena. Que se considera una prueba fallida. Por lo tanto, ahora tiene permiso para empezar a escribir el código de la aplicación. Es necesario escribir el código suficiente para ejecutar la prueba.

La clase de controlador de grupo en el listado 2 contiene la configuración mínima de código necesario para pasar la prueba unitaria. La acción de Index() devuelve una lista codificada de forma estática de grupos (la clase del grupo se define en el listado 3).

**La lista 2 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**El listado 3 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Una vez que se agregue las clases GroupController y grupo a nuestro proyecto, la primera prueba unitaria se completa correctamente (consulte la figura 2). Hemos hecho el trabajo mínimo necesario para pasar la prueba. Es el momento de celebrar.


[![Correcto.](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Figura 02**: correcto! () [Haga clic aquí para ver la imagen a tamaño completo](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Crear grupos de contactos

Ahora podemos mover el segundo caso de usuario. Es necesario para que pueda crear nuevos grupos de contactos. Es necesario expresar esta intención con una prueba.

La prueba en el listado 4 comprueba que el método Create() método con un nuevo grupo agrega el grupo a la lista de grupos devuelto por el método Index() de llamada. En otras palabras, si crea un nuevo grupo, a continuación, debería poder obtener el nuevo grupo de la lista de grupos devuelto por el método Index().

**Listado 4 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

La prueba en el listado 4 llama el método Create() con un nuevo grupo de contacto del controlador de grupo. A continuación, la prueba comprueba que el controlador de grupo Index() método de llamada devuelve el nuevo grupo de datos de la vista.

El controlador de grupo modificado en el listado 5 contiene el mínimo estricto de los cambios necesarios para pasar la nueva prueba.

**Listado 5 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>El controlador de grupo en el listado 5 tiene una nueva acción de Create(). Esta acción agrega un grupo a una colección de grupos. Tenga en cuenta que se ha modificado la acción de Index() para devolver el contenido de la colección de grupos.

Una vez más, hemos realizado la cantidad mínima de trabajo requerida para pasar la prueba unitaria. Después de realizar estos cambios en el controlador de grupo, todas las de nuestro unidad pruebas pasada.

## <a name="adding-validation"></a>Agregar una validación

Este requisito no se ha indicado explícitamente en el caso de usuario. Sin embargo, resulta razonable requerir que un grupo tiene un nombre. En caso contrario, organizar los contactos en grupos podría no ser muy útil.

Listado 6 contiene una nueva prueba que expresa esta intención. Esta prueba comprueba que al intentar crear un grupo sin proporcionar un nombre tiene como resultado en un mensaje de error de validación en el estado del modelo.

**Listado 6 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Para satisfacer esta prueba, es necesario agregar una propiedad de nombre a nuestra clase de grupo (consulte el listado 7). Además, tenemos que agregar un poco de lógica de validación a nuestro controlador grupo s Create() acción (consulte el listado 8).

**Listado 7 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Enumerar 8 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Observe que el controlador de grupo Create() acción ahora contiene lógica de validación y base de datos. Actualmente, la base de datos usada por el controlador de grupo está formada por nada más que una colección en memoria.

## <a name="time-to-refactor"></a>Tiempo de refactorización

El tercer paso en rojo/verde/refactorización es la parte de refactorización. En este momento, es necesario el paso desde el código y tener en cuenta cómo se puede refactorizar la aplicación para mejorar su diseño. La fase de refactorización es el escenario en el que creemos duros acerca de la mejor manera de implementar los patrones y principios de diseño de software.

Se está disponible modificar el código de manera que se elija para mejorar el diseño del código. Tenemos una red de seguridad de las pruebas unitarias que nos impiden romper la funcionalidad existente.

En este momento, el controlador de grupo es un lío desde la perspectiva de buen diseño de software. El controlador de grupo contiene un lío complicado de validación y código de acceso de datos. Para evitar infringir el principio de responsabilidad única, es necesario separar estos problemas en clases diferentes.

Nuestra clase de controlador de grupo refactorizado se encuentra en el listado 9. El controlador se ha modificado para utilizar la capa de servicio ContactManager. Se trata de la misma capa de servicio que se usa con el controlador de contacto.

Enumerar 10 contiene los nuevos métodos que se agrega a la capa de servicio ContactManager para admitir la validación, enumerar y crear grupos. La interfaz IContactManagerService se actualizó para incluir los nuevos métodos.

Listado 11 contiene una nueva clase FakeContactManagerRepository que implementa la interfaz IContactManagerRepository. A diferencia de la clase EntityContactManagerRepository que también implementa la interfaz IContactManagerRepository, nuestra nueva clase FakeContactManagerRepository no se comunica con la base de datos. La clase FakeContactManagerRepository utiliza una colección en memoria como un proxy para la base de datos. Vamos a usar esta clase en nuestras pruebas unitarias como una capa de repositorio falsa.

**Listado 9 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Lista de 10 - Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Listado 11 - Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


Modificar el IContactManagerRepository interfaz requiere utilizar para implementar los métodos CreateGroup() y ListGroups() en la clase EntityContactManagerRepository. La forma laziest y más rápida de hacerlo consiste en Agregar métodos de código auxiliar que tengan un aspecto similar al siguiente:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Por último, estos cambios en el diseño de nuestra aplicación tenemos que realizar algunas modificaciones en nuestras pruebas unitarias. Ahora necesitamos que utilizará el FakeContactManagerRepository al realizar las pruebas unitarias. La clase GroupControllerTest actualizada se encuentra en el listado 12.

**Enumerar 12 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Después de realizar todos estos, una vez más, todos los cambios de nuestro superación de las pruebas unitarias. Hemos completado todo el ciclo de rojo/verde/refactorizar. Hemos implementado los casos de dos usuario primeros. Ahora tenemos la compatibilidad con pruebas unitarias para los requisitos expresados en los casos de usuario. Implementar el resto de los casos de usuario consiste en repetir el mismo ciclo de rojo/verde/refactorizar.

## <a name="modifying-our-database"></a>Modificación de nuestra base de datos

Por desgracia, aunque nos hemos cumple todos los requisitos que se expresa mediante nuestras pruebas unitarias, nuestro trabajo no se realiza. Es necesario modificar la base de datos.

Es necesario crear una nueva tabla de base de datos de grupo. Siga estos pasos:

1. En la ventana Explorador de servidores, haga clic en la carpeta tablas y seleccione la opción de menú **agregar nueva tabla**.
2. Escriba las dos columnas que se describe a continuación, en el Diseñador de tablas.
3. Marcar la columna de identificador como una clave principal y una columna de identidad.
4. Guarde la nueva tabla con el nombre de grupos, haga clic en el icono del disquete.

<a id="0.12_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | int | False |
| nombre | nvarchar(50) | False |


A continuación, es necesario eliminar todos los datos de la tabla Contacts (en caso contrario, ganados t que pueda crear una relación entre las tablas de contactos y grupos). Siga estos pasos:

1. Haga clic en la tabla Contacts y seleccione la opción de menú **mostrar datos de tabla**.
2. Eliminar todas las filas.

A continuación, es necesario definir una relación entre la tabla de base de datos de grupos y la tabla de base de datos de contactos existente. Siga estos pasos:

1. Haga doble clic en la tabla de contactos en la ventana Explorador de servidores para abrir el Diseñador de tablas.
2. Agregar una nueva columna de enteros a la tabla Contacts denominada GroupId.
3. Haga clic en el botón de relación para abrir el cuadro de diálogo de relaciones de clave externa (consulte la figura 3).
4. Haga clic en el botón Agregar.
5. Haga clic en el botón de puntos suspensivos que aparece junto al botón de tabla y una especificación de columnas.
6. En el cuadro de diálogo tablas y columnas, seleccione los grupos como la tabla de clave principal y el identificador como columna de clave principal. Seleccionar contactos como la tabla de clave externa y GroupId como columna de clave externa (consulte la figura 4). Haga clic en el botón Aceptar.
7. En **INSERT y UPDATE especificación**, seleccione el valor **Cascade** para **Eliminar regla**.
8. Haga clic en el botón Cerrar para cerrar el cuadro de diálogo de relaciones de clave externa.
9. Haga clic en el botón Guardar para guardar los cambios en la tabla Contacts.


[![Crear una relación de tabla de base de datos](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Figura 03**: crear una relación de tabla de base de datos ([haga clic aquí para ver la imagen a tamaño completo](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Especificar relaciones entre tablas](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Figura 04**: especificar las relaciones entre tablas ([haga clic aquí para ver la imagen a tamaño completo](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>Actualizar el modelo de datos

A continuación, deberá actualizar el modelo de datos para representar la nueva tabla de base de datos. Siga estos pasos:

1. Haga doble clic en el archivo ContactManagerModel.edmx en la carpeta de modelos para abrir el Diseñador de entidades.
2. Haga clic en la superficie del diseñador y seleccione la opción de menú **actualizar modelo desde base de datos**.
3. En el asistente, seleccione los grupos de la tabla y haga clic en el acabado botón (Véase la figura 5).
4. Haga clic en la entidad de grupos y seleccione la opción de menú **cambiar el nombre de**. Cambiar el nombre de la *grupos* entidad a *grupo* (simples).
5. Haga clic en la propiedad de navegación de grupos que aparece en la parte inferior de la entidad Contact. Cambiar el nombre de la *grupos* propiedad de navegación para *grupo* (simples).


[![Actualizar un modelo de Entity Framework de la base de datos](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Figura 05**: actualizar un modelo de Entity Framework de la base de datos ([haga clic aquí para ver la imagen a tamaño completo](iteration-6-use-test-driven-development-vb/_static/image10.png))


Después de completar estos pasos, el modelo de datos representan los contactos y los grupos de tablas. El Diseñador de entidades debería mostrar ambas entidades (consulte la figura 6).


[![Presentación de grupo y póngase en contacto con el Diseñador de entidades](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Figura 06**: Entity Designer mostrar Group y Contact ([haga clic aquí para ver la imagen a tamaño completo](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Crear las clases de repositorio

A continuación, debemos implementar nuestra clase de repositorio. En el transcurso de esta iteración, se agregan varios métodos nuevos a la interfaz de IContactManagerRepository al escribir código para satisfacer nuestras pruebas unitarias. La versión final de la interfaz de IContactManagerRepository se encuentra en el listado 14.

**Enumerar 14 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Se refugio t realmente implementa cualquiera de los métodos relacionados con el trabajo con grupos de contactos en nuestra clase EntityContactManagerRepository real. Actualmente, la clase EntityContactManagerRepository tiene métodos de código auxiliar para cada uno de los métodos de grupo de contactos que se muestran en la interfaz IContactManagerRepository. Por ejemplo, el método ListGroups() actualmente es similar al siguiente:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Los métodos de código auxiliar nos permitido compilar la aplicación y pasar las pruebas unitarias. Sin embargo, ahora es el momento para implementar estos métodos. La versión final de la clase EntityContactManagerRepository se encuentra en el listado 13.

**Lista de 13 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Crear las vistas

Aplicación de ASP.NET MVC cuando se usa el motor de vista ASP.NET de forma predeterminada. Por lo tanto, don t crear vistas en respuesta a una prueba unitaria determinada. Sin embargo, dado que una aplicación podría ser sean inútil sin vistas, pero podemos t completar esta iteración sin tener que crear y modificar las vistas contenidas en la aplicación póngase en contacto con el administrador.

Se necesita crear las siguientes nuevas vistas para administrar grupos de contactos (consulte la figura 7):

- Views\Group\Index.aspx - muestra una lista de grupos de contactos
- Views\Group\Delete.aspx - formulario de confirmación de muestra para eliminar un grupo de contactos


[![La vista de índice de grupo](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Figura 07**: ver el índice de grupo ([haga clic aquí para ver la imagen a tamaño completo](iteration-6-use-test-driven-development-vb/_static/image14.png))


Es preciso modificar las siguientes vistas existentes para que incluyan grupos de contactos:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Puede ver las vistas modificadas examinando la aplicación de Visual Studio que se incluye en este tutorial. Por ejemplo, la figura 8 muestra la vista de índice de contactos.


[![La vista de índice de contactos](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Figura 08**: ver el índice de contactos ([haga clic aquí para ver la imagen a tamaño completo](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Resumen

En esta iteración, hemos agregado nuevas funcionalidades a nuestra aplicación póngase en contacto con el administrador siguiendo una metodología de diseño de aplicaciones de desarrollo controlado por pruebas. Hemos iniciado mediante la creación de un conjunto de casos de usuario. Hemos creado un conjunto de pruebas unitarias que se corresponde con los requisitos de expresar los casos de usuario. Por último, indicamos únicamente el código suficiente para satisfacer los requisitos expresados por las pruebas unitarias.

Una vez finalizada la escritura de código suficiente para satisfacer los requisitos expresados por las pruebas unitarias, hemos actualizado nuestra base de datos y vistas. Se agrega una nueva tabla de grupos a nuestra base de datos y se actualizaron nuestro modelo de datos de Entity Framework. También se crea y se modifica un conjunto de vistas.

En la siguiente iteración--la última iteración--volvemos a escribir la aplicación para aprovechar las ventajas de Ajax. Aprovechando las ventajas de Ajax, se podrá mejorar la capacidad de respuesta y el rendimiento de la aplicación póngase en contacto con el administrador.

> [!div class="step-by-step"]
> [Anterior](iteration-5-create-unit-tests-vb.md)
> [Siguiente](iteration-7-add-ajax-functionality-vb.md)
