---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: 'Iteración #6: usar el desarrollo controlado por pruebas (C#) | Microsoft Docs'
author: microsoft
description: En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias. En esta iteración,...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 58e1ab5a0a65e65b70a8f8a9f5e45564499efd63
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830074"
---
<a name="iteration-6--use-test-driven-development-c"></a>Iteración #6: usar el desarrollo controlado por pruebas (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargue el código](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias. En esta iteración, se agrega grupos de contactos.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación de MVC de ASP.NET de la administración de contactos (C#)
  

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa desde el principio para finalizar. La aplicación de Contact Manager permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Creamos la aplicación a través de varias iteraciones. Con cada iteración, mejorar gradualmente la aplicación. Es el objetivo de este enfoque de varias iteraciones para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible. Se agrega compatibilidad con las operaciones de base de datos básicos: creación, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación parezca interesante. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.

- Iteración #3: agregar validación de formulario. En la tercera iteración, se agrega una validación de formulario básico. Evitamos personas desde el envío de un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación tenga un acoplamiento. En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y la inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias. Hemos simular nuestras clases de modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración #6: usar el desarrollo controlado por pruebas. En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias. En esta iteración, se agrega grupos de contactos.

- Iteración #7: agregar funcionalidad Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En la iteración anterior de la aplicación de Contact Manager, hemos creado las pruebas unitarias para proporcionar una red de seguridad de nuestro código. La motivación para crear las pruebas unitarias era hacer que nuestro código sea más resistente a cambiar. Con las pruebas unitarias en su lugar, podemos alegremente realizar ningún cambio en nuestro código y saber inmediatamente si se ha interrumpido la funcionalidad existente.

En esta iteración, usamos las pruebas unitarias para un propósito completamente diferente. En esta iteración, usamos las pruebas unitarias como parte de una filosofía de diseño de la aplicación llama a *desarrollo controlado por pruebas*. Al poner en práctica desarrollo controlado por pruebas, escriba pruebas en primer lugar y, a continuación, escribir código en las pruebas.

Más concretamente, al practicar desarrollo controlado por pruebas, hay tres pasos que se completan al crear código (rojo / verde/de refactorización):

1. Escribir una prueba unitaria que se produce un error (rojo)
2. Escribir código que pasa la prueba unitaria (verde)
3. Refactorizar el código (refactorizar)

En primer lugar, escribir la prueba unitaria. La prueba unitaria debe expresar la intención para cómo espera que el código se comportan. Al crear la prueba unitaria en primer lugar, debe generar un error en la prueba unitaria. La prueba debe generar un error porque todavía no ha escrito ningún código de aplicación que cumple la prueba.

A continuación, escribir suficiente código en orden para que pase la prueba unitaria. El objetivo es escribir el código de la manera laziest, sloppiest y más rápido posible. No debe perder tiempo a pensar acerca de la arquitectura de la aplicación. En su lugar, debe centrarse en escribir la cantidad mínima de código necesario para satisfacer la intención expresada por la prueba unitaria.

Por último, después de haber escrito suficiente código, puede retroceder y considerar la arquitectura general de la aplicación. En este paso, vuelva a escribir (refactorizar) patrones de su código aprovechando las ventajas de diseño de software, como el modelo de repositorio para que el código sea más fácil de mantener. Sin miedo puede volver a escribir el código en este paso porque el código está cubierto por las pruebas unitarias.

Hay muchas ventajas resultantes de practicar el desarrollo controlado por pruebas. En primer lugar, controlado por pruebas de desarrollo le obliga a centrarse en el código que realmente debe escribirse. Dado que constantemente se centran en escribir solo suficiente código para pasar una prueba determinada, se impide adentra en las malas hierbas y escribir grandes cantidades de código que nunca va a utilizar.

En segundo lugar, una metodología de diseño "Pruebe primero" obliga a escribir código desde la perspectiva de cómo se utilizará el código. En otras palabras, cuando practica el desarrollo controlado por pruebas, se escribe constantemente las pruebas desde una perspectiva del usuario. Por lo tanto, puede dar lugar a desarrollo controlado por pruebas en las API más limpia y sea más comprensibles.

Por último, desarrollo controlado por pruebas obliga a escribir pruebas unitarias como parte del proceso de escribir una aplicación normal. Cuando se acerca el plazo del proyecto, las pruebas normalmente es lo primero que sale de la ventana. Cuando practica el desarrollo controlado por pruebas, por otro lado, es más probable que sea realmente virtuoso sobre cómo escribir pruebas unitarias porque desarrollo controlado por pruebas realiza las pruebas unitarias central en el proceso de creación de una aplicación.

> [!NOTE] 
> 
> Para más información sobre el desarrollo controlado por pruebas, le recomiendo que lea el libro de Michael Feathers **Working Effectively with Legacy Code**.


En esta iteración, agregamos una nueva característica a nuestra aplicación de Contact Manager. Se agrega compatibilidad con grupos de contactos. Puede usar grupos de contacto para organizar los contactos en categorías, como Business y Friend.

Vamos a agregar esta nueva funcionalidad a nuestra aplicación siguiendo un proceso de desarrollo controlado por pruebas. Vamos a escribir las pruebas unitarias en primer lugar y escribiremos todo nuestro código frente a estas pruebas.

## <a name="what-gets-tested"></a>¿Qué Obtiene probado

Como se explicó en la iteración anterior, normalmente no escribir pruebas unitarias para la lógica de acceso a datos o ver lógica. Cuando no las pruebas unitarias de t escritura lógica del acceso a datos porque el acceso a una base de datos es una operación relativamente lenta. Cuando no las pruebas unitarias de t escritura para la lógica de la vista porque el acceso a una vista requiere poner en marcha un servidor web que es una operación relativamente lenta. T no debe escribir una prueba unitaria a menos que la prueba se puede ejecutar una y otra vez muy rápida

Porque el desarrollo controlado por pruebas está controlado por pruebas unitarias, inicialmente nos centramos en escribir el controlador y la lógica empresarial. Evite la tocar la base de datos o vistas. Hemos ganado t modificar la base de datos o crear nuestras vistas hasta el final de este tutorial. Comenzaremos con lo que se pueden probar.

## <a name="creating-user-stories"></a>Crear casos de usuario

Cuando practica el desarrollo controlado por pruebas, siempre se inicia mediante la escritura de una prueba. Esto genera inmediatamente la pregunta: ¿cómo se decide qué prueba para escribir primero? Para responder esta pregunta, debe escribir un conjunto de [ **casos de usuario**](http://en.wikipedia.org/wiki/User_stories).

Un caso de usuario es una breve descripción de (normalmente una frase) de un requisito de software. Debe ser una descripción técnica de un requisito de escribir desde la perspectiva del usuario.

Este es el conjunto de casos de usuario que se describen las características necesarias para la nueva funcionalidad de grupo de contactos s:

1. Usuario puede ver una lista de grupos de contactos.
2. Usuario puede crear un nuevo grupo de contactos.
3. Usuario puede eliminar un grupo de contactos existente.
4. Usuario puede seleccionar un grupo de contactos al crear un nuevo contacto.
5. Usuario puede seleccionar un grupo de contactos al editar un contacto ya existente.
6. Se muestra una lista de grupos de contactos en la vista de índice.
7. Cuando un usuario hace clic en un grupo de contactos, se muestra una lista de contactos coincidentes.

Tenga en cuenta que esta lista de casos de usuario es perfectamente comprensible por un cliente. No hay ninguna mención de detalles de implementación técnica.

Mientras que en el proceso de compilar la aplicación, el conjunto de casos de usuario puede ser más refinado. Un caso de usuario que puede dividir en varios casos (requisitos). Por ejemplo, podría decidir que crear un nuevo grupo de contactos debe implicar la validación. Envío de un grupo de contactos sin nombre debe devolver un error de validación.

Después de crear una lista de los casos de usuario, está listo para escribir las primeras pruebas unitarias. Comenzaremos por crear una prueba unitaria para ver la lista de grupos de contactos.

## <a name="listing-contact-groups"></a>Enumerar grupos de contactos

Nuestro primer caso de usuario es que un usuario debe ser capaz de ver una lista de grupos de contactos. Es necesario expresar esta historia con una prueba.

Crear una nueva prueba unitaria con el botón secundario de la carpeta controladores en el proyecto ContactManager.Tests, seleccionar **Add, nueva prueba**y seleccionando la **de prueba unitaria** plantilla (consulte la figura 1). Nombre de la nueva unidad GroupControllerTest.cs de prueba y haga clic en el **Aceptar** botón.


[![Adición de la prueba unitaria GroupControllerTest](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Figura 01**: adición de la prueba unitaria GroupControllerTest ([haga clic aquí para ver imagen en tamaño completo](iteration-6-use-test-driven-development-cs/_static/image2.png))


La primera prueba unitaria se encuentra en el listado 1. Esta prueba comprueba que el método Index() del controlador grupo devuelve un conjunto de grupos. La prueba comprueba que se devuelve en la vista una colección de grupos de datos.

**Listado 1 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Cuando se escribe primero el código en el listado 1 en Visual Studio, obtendrá una gran cantidad de líneas onduladas rojas. No hemos creado las clases GroupController o grupo.

En este momento, se puede generar incluso t nuestra aplicación, por lo que se puede ejecutar la primera prueba unitaria. S buena. Que se considera una prueba fallida. Por lo tanto, ahora tenemos permiso para empezar a escribir código de la aplicación. Es necesario escribir código suficiente para ejecutar la prueba.

La clase de controlador de grupo en el listado 2 contiene el mínimo de código necesario para pasar la prueba unitaria. La acción de Index() devuelve una lista codificada de forma estática de grupos (la clase del grupo se define en el listado 3).

**Listado 2 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Listado 3 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Una vez que se agregue las clases GroupController y grupo a nuestro proyecto, nuestra primera prueba unitaria se completa correctamente (consulte la figura 2). Lo que hemos hecho el trabajo mínimo necesario para pasar la prueba. Es el momento para celebrar.


[![¡Success!](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Figura 02**: Success! () [Haga clic aquí para ver imagen en tamaño completo](iteration-6-use-test-driven-development-cs/_static/image4.png))


## <a name="creating-contact-groups"></a>Creación de grupos de contactos

Ahora podemos pasar al segundo caso de usuario. Es necesario que pueda crear nuevos grupos de contactos. Es necesario expresar la intención con una prueba.

La prueba en el listado 4 comprueba que el método con un nuevo grupo agrega el grupo a la lista de grupos devueltos por el método Index() de Create() la llamada. En otras palabras, si creo un nuevo grupo, a continuación, debería poder obtener el nuevo grupo de la lista de grupos devueltos por el método Index().

**Listado 4 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

El método Create() con un nuevo grupo de contacto del controlador de grupo llama a la prueba en el listado 4. A continuación, la prueba comprueba que una llamada del controlador de grupo Index() método devuelve el nuevo grupo en los datos de vista.

El controlador de grupo modificado en el listado 5 contiene el mínimo de los cambios necesarios para pasar la nueva prueba.

**Listado 5 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>El controlador de grupo en el listado 5 tiene una nueva acción Create(). Esta acción agrega un grupo a una colección de grupos. Tenga en cuenta que se ha modificado la acción de Index() para devolver el contenido de la colección de grupos.

Una vez más, hemos realizado la cantidad mínima de trabajo que se le pida que pase la prueba unitaria. Después de realizar estos cambios en el controlador de grupo, todos de nuestra unidad superan las pruebas.

## <a name="adding-validation"></a>Agregar una validación

Este requisito no se ha indicado explícitamente en el caso de usuario. Sin embargo, es razonable exigir que un grupo tiene un nombre. En caso contrario, el organizar los contactos en grupos no sería muy útil.

Listado 6 contiene una nueva prueba que expresa la intención. Esta prueba comprueba se intenta crear un grupo sin proporcionar un nombre da como resultado un mensaje de error de validación en el estado del modelo.

**Listado 6 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Con el fin de satisfacer esta prueba, necesitamos agregar una propiedad de nombre a nuestra clase de grupo (consulte el listado 7). Además, tenemos que agregar una pequeña cantidad de lógica de validación a nuestro controlador grupo s Create() acción (consulte el listado 8).

**Listado 7 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Listado 8 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Tenga en cuenta que el controlador de grupo Create() acción ahora contiene la lógica de validación y base de datos. Actualmente, la base de datos usada por el controlador del grupo consta de nada más que una colección en memoria.

## <a name="time-to-refactor"></a>Tiempo de refactorización

El tercer paso en rojo/verde/de refactorización es la parte de refactorización. En este momento, es necesario el paso de nuestro código y tener en cuenta cómo podemos refactorizar la aplicación para mejorar su diseño. La fase de Refactor es la fase en la que pensamos difíciles sobre la mejor manera de implementar los patrones y principios de diseño de software.

Estamos libres de modificar el código de cualquier forma que elegimos para mejorar el diseño del código. Tenemos una red de seguridad de las pruebas unitarias que nos impiden romper la funcionalidad existente.

En este momento, nuestro controlador de grupo es un lío desde la perspectiva del buen diseño de software. El controlador de grupo contiene un caos complicado de validación y el código de acceso de datos. Para evitar que infringe el principio de responsabilidad única, es necesario separar estas preocupaciones en clases diferentes.

Nuestra clase de controlador refactorizado grupo está contenida en el listado 9. El controlador se ha modificado para utilizar la capa de servicio ContactManager. Se trata de la misma capa de servicio que se usa con el controlador de contacto.

Listado 10 contiene los nuevos métodos que se agrega a la capa de servicio ContactManager para admitir la validación, enumerar y crear grupos. La interfaz IContactManagerService se actualizó para incluir los nuevos métodos.

Listado 11 contiene una nueva clase FakeContactManagerRepository que implementa la interfaz IContactManagerRepository. A diferencia de la clase EntityContactManagerRepository que también implementa la interfaz IContactManagerRepository, nuestra nueva clase FakeContactManagerRepository no se comunica con la base de datos. La clase FakeContactManagerRepository usa una colección en memoria como un proxy para la base de datos. Vamos a usar esta clase en nuestras pruebas de unidad como una capa de repositorio falsa.

**Listado 9 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Listado 10 - Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Listado 11 - Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

Modificar el IContactManagerRepository interfaz requiere utilizar para implementar los métodos CreateGroup() y ListGroups() en la clase EntityContactManagerRepository. La forma laziest y rápida de hacerlo consiste en agregar los métodos auxiliares que tienen un aspecto similar al siguiente:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


Por último, estos cambios en el diseño de nuestra aplicación nos obligan a realizar algunas modificaciones en nuestras pruebas unitarias. Ahora es necesario usar el FakeContactManagerRepository al realizar las pruebas unitarias. La clase GroupControllerTest actualizada se encuentra en el listado 12.

**Listado 12 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Después de realizar todos estos cambios, una vez más, todas nuestras pruebas unitarias superadas. Hemos completado todo el ciclo de rojo/verde/de refactorización. Hemos implementado la primera casos de usuario de dos. Ahora tenemos compatibilidad con pruebas unitarias para los requisitos que se expresa en los casos de usuario. Implementar el resto de los casos de usuario consiste en repetir el mismo ciclo de rojo/verde/de refactorización.

## <a name="modifying-our-database"></a>Modificación de la base de datos

Por desgracia, aunque nos hemos cumple todos los requisitos expresados por nuestras pruebas unitarias, nuestro trabajo no se realiza. Necesitamos modificar nuestra base de datos.

Es necesario crear una nueva tabla de base de datos del grupo. Siga estos pasos:

1. En la ventana Explorador de servidores, haga clic en la carpeta Tables y seleccione la opción de menú **agregar nueva tabla**.
2. Escriba las dos columnas que se describe a continuación, en el Diseñador de tablas.
3. Marque la columna de identificador como una clave principal y una columna de identidad.
4. Guarde la nueva tabla con los nombres de grupos, haga clic en el icono del disquete.

<a id="0.11_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | int | False |
| nombre | nvarchar (50) | False |


A continuación, se debe eliminar todos los datos de la tabla de contactos (en caso contrario, hemos ganado poder crear una relación entre las tablas de contactos y grupos). Siga estos pasos:

1. Haga clic en la tabla Contacts y seleccione la opción de menú **mostrar datos de tabla**.
2. Eliminar todas las filas.

A continuación, se debe definir una relación entre la tabla de base de datos de grupos y la tabla de base de datos de contactos existente. Siga estos pasos:

1. Haga doble clic en la tabla de contactos en la ventana Explorador de servidores para abrir el Diseñador de tablas.
2. Agregue una nueva columna de enteros a la tabla Contactos denominada GroupId.
3. Haga clic en el botón Relationship para abrir el cuadro de diálogo de relaciones de clave externa (consulte la figura 3).
4. Haga clic en el botón Agregar.
5. Haga clic en el botón de puntos suspensivos que aparece junto al botón de tabla y la especificación de columnas.
6. En el cuadro de diálogo tablas y columnas, seleccione los grupos como la tabla de clave principal y el identificador como columna de clave principal. Seleccionar contactos como la tabla de clave externa y GroupId como columna de clave externa (consulte la figura 4). Haga clic en el botón Aceptar.
7. En **especificación de INSERT y UPDATE**, seleccione el valor **Cascade** para **Eliminar regla**.
8. Haga clic en el botón Cerrar para cerrar el cuadro de diálogo de relaciones de clave externa.
9. Haga clic en el botón Guardar para guardar los cambios en la tabla Contactos.


[![Creación de una relación de tabla de base de datos](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Figura 03**: creación de una relación de tabla de base de datos ([haga clic aquí para ver imagen en tamaño completo](iteration-6-use-test-driven-development-cs/_static/image6.png))


[![Especificar las relaciones entre tablas](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Figura 04**: especificar las relaciones entre tablas ([haga clic aquí para ver imagen en tamaño completo](iteration-6-use-test-driven-development-cs/_static/image8.png))


### <a name="updating-our-data-model"></a>Actualización de nuestro modelo de datos

A continuación, necesitamos actualizar nuestro modelo de datos para representar la nueva tabla de base de datos. Siga estos pasos:

1. Haga doble clic en el archivo ContactManagerModel.edmx en la carpeta de modelos para abrir el Diseñador de entidades.
2. Haga clic en la superficie del diseñador y seleccione la opción de menú **actualizar modelo desde base de datos**.
3. En el asistente, seleccione los grupos de la tabla y haga clic en el acabado de botón (consulte la figura 5).
4. Haga clic en la entidad de grupos y seleccione la opción de menú **cambiar el nombre**. Cambiar el nombre de la *grupos* entidad *grupo* (simples).
5. Haga clic en la propiedad de navegación de grupos que aparece en la parte inferior de la entidad Contact. Cambiar el nombre de la *grupos* propiedad de navegación para *grupo* (simples).


[![Actualizar un modelo de Entity Framework desde la base de datos](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Figura 05**: actualizar un modelo de Entity Framework desde la base de datos ([haga clic aquí para ver imagen en tamaño completo](iteration-6-use-test-driven-development-cs/_static/image10.png))


Después de completar estos pasos, el modelo de datos representa tanto los contactos y grupos de tablas. El Diseñador de entidades debería mostrar ambas entidades (consulte la figura 6).


[![Muestra el grupo y póngase en contacto con el Diseñador de entidades](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Figura 06**: muestra el grupo y póngase en contacto con el Diseñador de entidades ([haga clic aquí para ver imagen en tamaño completo](iteration-6-use-test-driven-development-cs/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Creación de nuestras clases de repositorio

A continuación, debemos implementar nuestra clase de repositorio. En el transcurso de esta iteración, hemos agregado varios métodos nuevos a la interfaz IContactManagerRepository al escribir código para satisfacer las pruebas unitarias. La versión final de la interfaz IContactManagerRepository se encuentra en el listado 14.

**Listado 14 - Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

Nos haven t realmente implementa cualquiera de los métodos relacionados con el trabajo con grupos de contactos. Actualmente, la clase EntityContactManagerRepository tiene los métodos auxiliares para cada uno de los métodos de grupo de contactos que aparecen en la interfaz IContactManagerRepository. Por ejemplo, el método ListGroups() actualmente este aspecto:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

Los métodos de código auxiliar nos permitía compilar la aplicación y pasar las pruebas unitarias. Sin embargo, ahora es el momento para implementar estos métodos. La versión final de la clase EntityContactManagerRepository se encuentra en el listado 13.

**Listado 13 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Creación de las vistas

Aplicación de ASP.NET MVC cuando se usa el motor de vistas ASP.NET de forma predeterminada. Por lo tanto, don t crear vistas en respuesta a una prueba unitaria determinada. Sin embargo, dado que una aplicación sería inútil sin vistas, nos t puede completar esta iteración sin la creación y modificación de las vistas de la aplicación de Contact Manager.

Es necesario crear las siguientes nuevas vistas para administrar grupos de contactos (consulte la figura 7):

- Views\Group\Index.aspx - muestra una lista de grupos de contactos
- Views\Group\Delete.aspx - formulario de confirmación de muestra para eliminar un grupo de contactos


[![La vista de índice de grupo](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Figura 07**: ver el índice de grupo ([haga clic aquí para ver imagen en tamaño completo](iteration-6-use-test-driven-development-cs/_static/image14.png))


Es necesario modificar las siguientes vistas existentes para que incluyan grupos de contactos:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Puede ver las vistas modificadas echando un vistazo a la aplicación de Visual Studio que acompaña a este tutorial. Por ejemplo, la figura 8 muestra la vista de índice de contacto.


[![La vista de índice de contacto](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Figura 08**: vista de índice de póngase en contacto con el ([haga clic aquí para ver imagen en tamaño completo](iteration-6-use-test-driven-development-cs/_static/image16.png))


## <a name="summary"></a>Resumen

En esta iteración, hemos agregado nueva funcionalidad a nuestra aplicación de Contact Manager siguiendo una metodología de diseño de la aplicación de desarrollo controlado por pruebas. Comenzamos creando un conjunto de casos de usuario. Hemos creado un conjunto de pruebas unitarias que se corresponde con los requisitos expresados por los casos de usuario. Por último, hemos escrito código suficiente para satisfacer los requisitos expresados por las pruebas unitarias.

Una vez que hemos terminado de escribir código suficiente para satisfacer los requisitos expresados por las pruebas unitarias, hemos actualizado nuestra base de datos y vistas. Hemos agregado una nueva tabla de grupos a nuestra base de datos y actualizado nuestro modelo de datos de Entity Framework. También se crea y se puede modificar un conjunto de vistas.

En la siguiente iteración--la última iteración--se vuelva a escribir la aplicación para aprovechar las ventajas de Ajax. Aprovechando las ventajas de Ajax, se mejorará la capacidad de respuesta y el rendimiento de la aplicación de Contact Manager.

> [!div class="step-by-step"]
> [Anterior](iteration-5-create-unit-tests-cs.md)
> [Siguiente](iteration-7-add-ajax-functionality-cs.md)
