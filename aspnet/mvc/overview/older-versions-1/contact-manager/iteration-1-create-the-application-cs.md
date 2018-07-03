---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Iteración #1: crear la aplicación (C#) | Microsoft Docs'
author: microsoft
description: 'En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible. Se agrega compatibilidad con las operaciones de base de datos básicos: creación, lectura, actualización y D...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: aa79379c721456a62ee62e37781e51f61e16d3bc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403050"
---
<a name="iteration-1--create-the-application-c"></a>Iteración #1: crear la aplicación (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargue el código](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible. Se agrega compatibilidad con las operaciones de base de datos básicos: creación, lectura, actualización y eliminación (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de ASP.NET MVC (VB) de administración de contactos

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

En esta primera iteración, creamos la aplicación básica. El objetivo es crear el Administrador de contactos de la manera más rápida y sencilla posible. En posteriores iteraciones, mejorar el diseño de la aplicación.

La aplicación de Contact Manager es una aplicación básica de bases de datos. Puede usar la aplicación para crear nuevos contactos, editar contactos existentes y eliminar contactos.

En esta iteración, se complete los pasos siguientes:

1. Aplicación de ASP.NET MVC
2. Crear una base de datos para almacenar nuestros contactos
3. Generar una clase de modelo para nuestra base de datos con Microsoft Entity Framework
4. Crear una acción de controlador y vista que nos permite enumerar todos los contactos de la base de datos
5. Crear acciones de controlador y una vista que nos permite crear un nuevo contacto en la base de datos
6. Crear acciones de controlador y una vista que permite editar un contacto ya existente en la base de datos
7. Crear acciones de controlador y una vista que nos permite eliminar un contacto ya existente en la base de datos

## <a name="software-prerequisites"></a>Requisitos previos de software

En las aplicaciones de ASP.NET MVC, debe tener Visual Studio 2008 o Visual Web Developer 2008 instalado en el equipo (Visual Web Developer es una versión gratuita de Visual Studio que no incluya todas las características avanzadas de Visual Studio). Puede descargar la versión de prueba de Visual Studio 2008 o Visual Web Developer desde la siguiente dirección:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Para las aplicaciones de ASP.NET MVC con Visual Web Developer, debe tener Visual Web Developer Service Pack 1 instalado. Sin Service Pack 1, no se puede crear proyectos de aplicación Web.


Marco de ASP.NET MVC. Puede descargar el marco de MVC de ASP.NET desde la siguiente dirección:

[https://www.asp.net/mvc](../../../index.md)

En este tutorial, usamos Microsoft Entity Framework para tener acceso a una base de datos. Entity Framework se incluye con .NET Framework 3.5 Service Pack 1. Puede descargar este service pack desde la ubicación siguiente:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Como alternativa a realizar cada una de estas descargas uno por uno, puede aprovechar el instalador de plataforma Web (Web PI). Puede descargar el instalador de plataforma Web desde la siguiente dirección:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Proyecto de ASP.NET MVC

Proyecto de aplicación Web de ASP.NET MVC. Inicie Visual Studio y seleccione la opción de menú **archivo, nuevo proyecto**. El **nuevo proyecto** cuadro de diálogo aparece (consulte la figura 1). Seleccione el **Web** tipo de proyecto y el **aplicación Web ASP.NET MVC** plantilla. Nombre del nuevo proyecto *ContactManager* y haga clic en el botón Aceptar.


Asegúrese de que haya seleccionado en la lista desplegable en la parte superior de .NET Framework 3.5 derecha de la **nuevo proyecto** cuadro de diálogo. En caso contrario, la plantilla de aplicación Web ASP.NET MVC ganó t aparecen.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figura 01**: cuadro de diálogo nuevo proyecto ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image2.png))


Aplicación de ASP.NET MVC, la **crear proyecto de prueba unitaria** aparece el cuadro de diálogo. Puede usar este cuadro de diálogo para indicar que desea crear y agregar un proyecto de prueba unitaria a la solución al crear la aplicación de ASP.NET MVC. Aunque hemos ganado t puede generar pruebas unitarias en esta iteración, debe seleccionar la opción **Sí, crear un proyecto de prueba unitaria** porque tenemos previsto agregar pruebas unitarias en una iteración posterior. Agregar un proyecto de prueba al crear un nuevo proyecto de ASP.NET MVC es mucho más sencillo que agregar un proyecto de prueba una vez creado el proyecto de MVC de ASP.NET.

> [!NOTE] 
> 
> Dado que Visual Web Developer no admite proyectos de prueba, no podrá obtener el cuadro de diálogo Crear proyecto de prueba unitaria al usar Visual Web Developer.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figura 02**: cuadro de diálogo de la creación de proyecto de prueba de unidad ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image4.png))


Aplicación de ASP.NET MVC se muestra en la ventana Explorador de soluciones de Visual Studio (consulte la figura 3). Si don t ve la ventana del explorador de soluciones, a continuación, puede abrir esta ventana, seleccione la opción de menú **ver, Explorador de soluciones**. Tenga en cuenta que la solución contiene dos proyectos: el proyecto de ASP.NET MVC y el proyecto de prueba. El proyecto de ASP.NET MVC se denomina ContactManager y el proyecto de prueba se denomina ContactManager.Tests.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figura 03**: ventana del explorador de soluciones ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Eliminando los archivos de proyecto de ejemplo

La plantilla de proyecto de ASP.NET MVC incluye los archivos de ejemplo para controladores y vistas. Antes de crear una nueva aplicación MVC de ASP.NET, debe eliminar estos archivos. Puede eliminar archivos y carpetas en la ventana Explorador de soluciones, con el botón secundario en un archivo o carpeta y seleccione la opción de menú **eliminar**.

Es preciso eliminar los siguientes archivos desde el proyecto de ASP.NET MVC:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Y, deberá eliminar el siguiente archivo del proyecto de prueba:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Creación de la base de datos

La aplicación de Contact Manager es una aplicación web controlada por base de datos. Se usa una base de datos para almacenar la información de contacto.

El marco de MVC de ASP.NET con cualquier base de datos modernas, incluidas bases de datos IBM DB2, MySQL, Oracle y Microsoft SQL Server. En este tutorial, se usa una base de datos de Microsoft SQL Server. Cuando instala Visual Studio, se proporcionan con la opción de instalar Microsoft SQL Server Express que es una versión gratuita de la base de datos de Microsoft SQL Server.

Crear una nueva base de datos con el botón secundario de la aplicación\_carpeta de datos en la ventana Explorador de soluciones y seleccionando la opción de menú **agregar, nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **datos** categoría y el **base de datos de SQL Server** plantilla (consulte la figura 4). Nombre de la nueva base de datos ContactManagerDB.mdf y haga clic en el botón Aceptar.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figura 04**: crear una nueva base de datos Microsoft SQL Server Express ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image8.png))


Después de crear la nueva base de datos, la base de datos aparece en la aplicación\_carpeta de datos en la ventana Explorador de soluciones. Haga doble clic en el archivo ContactManager.mdf para abrir la ventana Explorador de servidores y conectarse a la base de datos.

> [!NOTE] 
> 
> La ventana del explorador de servidores se llama a la ventana del explorador de base de datos en el caso de Microsoft Visual Web Developer.


Puede usar la ventana del explorador de servidores para crear nuevos objetos de base de datos como procedimientos almacenados, desencadenadores, vistas y tablas de base de datos. Haga clic en la carpeta Tables y seleccione la opción de menú **agregar nueva tabla**. El Diseñador de tablas de base de datos aparece (consulte la figura 5).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figura 05**: el Diseñador de tablas de base de datos ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image10.png))


Es necesario crear una tabla que contiene las siguientes columnas:

<a id="0.1_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | int | False |
| Nombre | nvarchar (50) | False |
| LastName | nvarchar (50) | False |
| Teléfono | nvarchar (50) | False |
| Correo electrónico | nvarchar (255) | False |


La primera columna, la columna de identificador es especial. Deberá marcar la columna de identificador como una columna de identidad y una columna de clave principal. Indica que una columna es una columna de identidad mediante la expansión de propiedades de columna (examine la parte inferior de la figura 6) y desplácese hacia abajo hasta la propiedad de la especificación de identidad. Establecer el **(identidad)** valor para la propiedad **Sí**.

Marcar una columna como una columna de clave principal, seleccione la columna y haga clic en el botón con el icono de una clave. Después de una columna está marcada como una columna de clave principal, aparece un icono de una clave junto a la columna (consulte la figura 6).

Cuando termine de crear la tabla, haga clic en el botón Guardar (el botón con el icono de un disquete) para guardar la nueva tabla. Asigne el nombre de la nueva tabla *contactos*.

Después de finalizar la creación de la tabla de base de datos de contactos, debe agregar algunos registros a la tabla. Haga clic en la tabla de contactos en la ventana del explorador de servidores y seleccione la opción de menú **mostrar datos de tabla**. Escriba uno o más contactos en la cuadrícula que aparece.

## <a name="creating-the-data-model"></a>Crear el modelo de datos

La aplicación de ASP.NET MVC consta de los modelos, vistas y controladores. Comenzamos creando una clase de modelo que representa la tabla de contactos que creamos en la sección anterior.

En este tutorial, usamos Microsoft Entity Framework para generar automáticamente una clase de modelo de la base de datos.

> [!NOTE] 
> 
> El marco de MVC de ASP.NET no está asociado a Entity Framework de Microsoft de ninguna manera. Puede usar ASP.NET MVC con tecnologías de acceso de base de datos alternativo, como NHibernate, LINQ to SQL o ADO.NET.


Siga estos pasos para crear las clases del modelo de datos:

1. Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione **agregar, nuevo elemento**. El **Agregar nuevo elemento** cuadro de diálogo aparece (consulte la figura 6).
2. Seleccione el **datos** categoría y el **ADO.NET Entity Data Model** plantilla. El modelo de datos el nombre *ContactManagerModel.edmx* y haga clic en el **agregar** botón. Aparece el Asistente de Entity Data Model (consulte la figura 7).
3. En el **Elegir contenido de Model** paso, seleccione **generar desde la base de datos** (consulte la figura 7).
4. En el **elegir la conexión de datos** paso, seleccione la base de datos ContactManagerDB.mdf y escriba el nombre *ContactManagerDBEntities* para la configuración de conexión de entidad (consulte la figura 8).
5. En el **elija los objetos de base de datos** paso, seleccione la casilla de las tablas (consulte la figura 9). El modelo de datos incluirá todas las tablas incluidas en la base de datos (solo hay uno, la tabla Contactos). Escriba el espacio de nombres *modelos*. Haga clic en el botón Finalizar para completar al asistente.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figura 06**: cuadro de diálogo Agregar nuevo elemento ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image12.png))


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figura 07**: Elegir contenido de Model ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image14.png))


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figura 08**: elegir la conexión de datos ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image16.png))


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figura 09**: elija los objetos de base de datos ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image18.png))


Después de completar al Asistente para Entity Data Model, aparece el Entity Data Model Designer. El diseñador muestra una clase que corresponde a cada tabla que se está modelando. Debería ver una clase denominada contactos.

El Asistente de Entity Data Model genera nombres de clase basados en nombres de tabla de base de datos. Casi siempre, deberá cambiar el nombre de la clase generada por el asistente. Haga clic en la clase Contacts en el diseñador y seleccione la opción de menú **cambiar el nombre**. Cambie el nombre de la clase de contactos (en plural) para el contacto (singular). Después de cambiar el nombre de clase, la clase debe aparecer como la figura 10.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figura 10**: clase Contact The ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image20.png))


En este momento, hemos creado nuestro modelo de base de datos. Podemos usar la clase Contact para representar un registro de contacto concreto en nuestra base de datos.

## <a name="creating-the-home-controller"></a>Crear el controlador Home

El siguiente paso es crear nuestro controlador Home. El controlador principal es el controlador predeterminado que se invoca en una aplicación ASP.NET MVC.

Crear la clase del controlador principal haciendo clic en la carpeta controladores en la ventana Explorador de soluciones y seleccionar la opción de menú **Add, controlador** (consulte la figura 11). Tenga en cuenta la casilla **agregar métodos de acción para los escenarios Create, Update y detalles**. Asegúrese de que está activada esta casilla de verificación antes de hacer clic en el **agregar** botón.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figura 11**: agregar el controlador Home ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image22.png))


Cuando se crea el controlador Home, obtenga la clase en el listado 1.

**Listado 1 - controllers\homecontroller. cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Enumerar los contactos

Para mostrar los registros de la tabla de base de datos de contactos, necesitamos crear una acción de Index() y una vista de índice.

El controlador Home ya contiene una acción Index(). Es necesario modificar este método para que quede como el listado 2.

**Listado 2 - controllers\homecontroller. cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Tenga en cuenta que la clase de controlador Home en el listado 2 contiene un campo privado denominado \_entidades. El \_campo entidades representa las entidades del modelo de datos. Usamos el \_entidades campo para comunicarse con la base de datos.

El método Index() devuelve una vista que representa todos los contactos de la tabla de base de datos de contactos. La expresión \_entidades. ContactSet.ToList() devuelve la lista de contactos como una lista genérica.

Ahora que se ve creó el controlador de índice, a continuación, debe crear la vista de índice. Antes de crear la vista de índice, compile la aplicación seleccionando la opción de menú **crear, compilar solución**. Siempre debe compilar el proyecto antes de agregar una vista en orden de la lista de clases de modelo que se mostrará en el **agregar vista** cuadro de diálogo.

Crear la vista de índice haciendo clic en el método Index() y seleccionar la opción de menú **agregar vista** (consulte la figura 12). Al seleccionar esta opción de menú se abre el **agregar vista** cuadro de diálogo (consulte la figura 13).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figura 12**: adición de la vista de índice ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image24.png))


En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada**. Seleccione la clase de datos de vista ContactManager.Models.Contact y la lista Ver contenido. Al seleccionar estas opciones, genera una vista que muestra una lista de registros de contacto.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figura 13**: cuadro de diálogo de agregar la vista ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image26.png))


Al hacer clic en el **agregar** se genera el botón, la vista de índice en el listado 3. Tenga en cuenta la &lt;% @ Page %&gt; directiva que aparece en la parte superior del archivo. La vista de índice se hereda de la ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; clase. En otras palabras, la clase del modelo en la vista representa una lista de entidades de contacto.

El cuerpo de la vista de índice contiene un bucle foreach que itera por cada uno de los contactos representados por la clase del modelo. El valor de cada propiedad de la clase de contacto se muestra en una tabla HTML.

**Listado 3 - Views\Home\Index.aspx (sin cambios)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Es necesario realizar una modificación a la vista de índice. Dado que no vamos a crear una vista de detalles, podemos quitar el vínculo de detalles. Buscar y quitar el código siguiente de la vista de índice:

{id = el elemento. % De identificador})&gt;

Después de modificar la vista de índice, puede ejecutar la aplicación de Contact Manager. Seleccione la opción de menú Depurar, Iniciar depuración o simplemente presione F5. La primera vez que ejecute la aplicación, obtenga el cuadro de diálogo en la figura 14. Seleccione la opción **modificar el archivo Web.config para habilitar la depuración** y haga clic en el botón Aceptar.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figura 14**: habilitar la depuración ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image28.png))


De forma predeterminada, se devuelve la vista de índice. Esta vista muestra todos los datos de la tabla de base de datos de contactos (consulte la figura 15).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figura 15**: vista de índice de la ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image30.png))


Tenga en cuenta que la vista de índice incluye un vínculo Crear nuevo en la parte inferior de la vista de la etiqueta. En la siguiente sección, aprenderá a crear nuevos contactos.

## <a name="creating-new-contacts"></a>Crear nuevos contactos

Para habilitar a los usuarios crear nuevos contactos, necesitamos agregar dos acciones Create() al controlador de inicio. Es necesario crear una acción Create() que devuelve un formulario HTML para crear un nuevo contacto. Es necesario crear una segunda acción Create() que realiza la inserción de base de datos real del nuevo contacto.

Los nuevos métodos Create() que necesitamos para agregar al controlador de inicio se encuentran en el listado 4.

**Listado 4 - controllers\homecontroller. cs (con métodos Create)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

El primer método Create() se puede invocar con un HTTP GET, mientras que el segundo método Create() puede invocarse únicamente por una solicitud HTTP POST. En otras palabras, el segundo método Create() puede invocarse únicamente al registrar un formulario HTML. El primer método Create() simplemente devuelve una vista que contiene el formulario HTML para crear un nuevo contacto. El segundo método Create() es mucho más interesante: agrega el nuevo contacto a la base de datos.

Tenga en cuenta que el segundo método Create() se ha modificado para aceptar una instancia de la clase Contact. Los valores de formulario publicados desde el formulario HTML están enlazados a esta clase póngase en contacto con el marco de ASP.NET MVC automáticamente. Cada campo de formulario de la forma de crear HTML se asigna a una propiedad del parámetro de contacto.

Tenga en cuenta que el parámetro de contacto se decora con un atributo [Bind]. El atributo [Bind] se usa para excluir la propiedad de identificador de contacto del enlace. Dado que la propiedad de identificador representa una propiedad Identity, no queremos t desea establecer la propiedad Id.

En el cuerpo del método Create(), Entity Framework se usa para insertar el nuevo contacto en la base de datos. El nuevo contacto se agrega al conjunto existente de contactos y el método SaveChanges() se llama para devolver estos cambios a la base de datos subyacente.

Puede generar un formulario HTML para crear nuevos contactos haciendo clic en cualquiera de los dos métodos Create() y seleccionar la opción de menú **agregar vista** (vea la figura 16).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figura 16**: adición de la vista de creación ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image32.png))


En el **agregar vista** cuadro de diálogo, seleccione el **ContactManager.Models.Contact** clase y el **crear** opción para ver el contenido (consulte la figura 17). Al hacer clic en el **agregar** botón Crear vista se genera automáticamente.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figura 17**: puede ver una página explode ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image34.png))


La vista de creación contiene campos de formulario para cada una de las propiedades de la clase Contact. El código de la vista de creación se incluye en el listado 5.

**Listado 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Después de modificar los métodos Create() y agregar la vista de creación, puede ejecutar la aplicación de Contact Manager y crear nuevos contactos. Haga clic en el **crear nuevo** vínculo que aparece en la vista de índice para navegar a la vista de creación. Debería ver la vista en la figura 18.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figura 18**: The Create View ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>Editar contactos

Agregar la funcionalidad para editar un registro de contacto es muy similar a agregar la funcionalidad para crear nuevos registros de contacto. En primer lugar, necesitamos agregar dos nuevos métodos de edición a la clase de controlador Home. Estos nuevos métodos Edit() están contenidos en el listado 6.

**Listado 6 - controllers\homecontroller. cs (con los métodos de edición)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

El primer método Edit() invoca una operación HTTP GET. Un parámetro de identificador se pasa a este método que representa el identificador del registro de contacto que se está editando. Entity Framework se usa para recuperar un contacto que coincida con el identificador. Se devuelve una vista que contiene un formulario HTML para editar un registro.

El segundo método Edit() realiza la actualización real de la base de datos. Este método acepta una instancia de la clase Contact como un parámetro. El marco ASP.NET MVC enlaza los campos del formulario desde el formulario de edición a esta clase automáticamente. Tenga en cuenta que t no incluyen el atributo [Bind] cuando se edita un contacto (se necesita el valor de la propiedad Id).

Entity Framework se usa para guardar el contacto modificado en la base de datos. El contacto original se debe recuperar de la base de datos en primer lugar. A continuación, se llama al método de Entity Framework ApplyPropertyChanges() para registrar los cambios en el contacto. Por último, el método SaveChanges() de Entity Framework se llama para conservar los cambios en la base de datos subyacente.

Puede generar la vista que contiene el formulario de edición haciendo clic en el método Edit() y seleccionar la opción de menú Agregar vista. En el cuadro de diálogo Agregar vista, seleccione el **ContactManager.Models.Contact** clase y el **editar** ver el contenido (consulte la figura 19).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figura 19**: adición de una vista de edición ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image38.png))


Al hacer clic en el botón Agregar, se genera automáticamente una nueva vista de edición. El formulario HTML que se genera contiene campos que corresponden a cada una de las propiedades de la clase Contact (consulte el listado 7).

**Listado 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Eliminación de contactos

Si desea eliminar los contactos, a continuación, deberá agregar dos acciones Delete() a la clase de controlador Home. La primera acción Delete() muestra un formulario de confirmación de eliminación. La segunda acción Delete() lleva a cabo la eliminación real.

> [!NOTE] 
> 
> Más adelante, en iteración #7, modificamos el Administrador de contactos para que admita una paso eliminar Ajax.


Los dos nuevos métodos de Delete() están contenidos en el listado 8.

**Listado 8 - controllers\homecontroller. cs (métodos de eliminación)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

El primer método Delete() devuelve un formulario de confirmación para eliminar un registro de contacto de la base de datos (consulte Figure20). El segundo método Delete() realiza la operación de eliminación real con la base de datos. Después de que el contacto original se ha recuperado de la base de datos, se denominan los métodos Entity Framework DeleteObject() y SaveChanges() para llevar a cabo la eliminación de la base de datos.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figura 20**: la vista de confirmación de eliminación ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image40.png))


Es necesario modificar la vista de índice para que contenga un vínculo para eliminar registros de contactos (Véase la figura 21). Deberá agregar el código siguiente a la misma celda de tabla que contiene el vínculo de edición:

Html.ActionLink ({id = el elemento. % De identificador})&gt;


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figura 21**: índice de la vista con el vínculo de edición ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image42.png))


A continuación, necesitamos crear la vista de confirmación de eliminación. Haga clic en el método Delete() en la clase del controlador principal y seleccione la opción de menú Agregar vista. El cuadro de diálogo Agregar vista aparece (consulte la figura 22).

A diferencia de en el caso de las vistas de lista, crear y editar el cuadro de diálogo Agregar vista no contiene una opción para crear una vista de eliminación. En su lugar, seleccione el **ContactManager.Models.Contact** clase de datos y el **vacía** ver el contenido. Seleccionar la vista vacía contenido opción necesitará crear la vista de nosotros mismos.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Figura 22**: adición de la vista de confirmación de eliminación ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image44.png))


El contenido de la vista de eliminación está contenido en el listado 9. Esta vista contiene un formulario que se confirma o no un contacto concreto debe eliminarse (consulte la figura 21).

**Listado 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Cambiar el nombre del controlador predeterminado

Es posible que le molesta que el nombre de nuestra clase de controlador para trabajar con contactos se denomina la clase HomeController. ¿No debe t el controlador se denomina ContactController?

Es bastante fácil de corregir este problema. En primer lugar, es necesario refactorizar el nombre del controlador Home. Abra la clase HomeController en el Editor de código de Visual Studio, haga clic el nombre de la clase y seleccione la opción de menú **refactorizar cambiar nombre**. Al seleccionar esta opción de menú abre el cuadro de diálogo de cambio de nombre.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figura 23**: refactorización de un nombre de controlador ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image46.png))


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figura 24**: mediante el cuadro de diálogo Cambiar nombre ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image48.png))


Si cambia el nombre de la clase de controlador, Visual Studio actualizará el nombre de la carpeta en la carpeta Views también. Visual Studio cambiará el nombre de la carpeta \Views\Home a la carpeta \Views\Contact.

Después de realizar este cambio, la aplicación dejará de tener un controlador Home. Al ejecutar la aplicación, aparecerá la página de error en la ilustración 25.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figura 25**: no hay ningún controlador predeterminado ([haga clic aquí para ver imagen en tamaño completo](iteration-1-create-the-application-cs/_static/image50.png))


Es necesario actualizar la ruta predeterminada en el archivo Global.asax para usar el controlador de contacto en lugar del controlador principal. Abra el archivo Global.asax y modifique el controlador predeterminado utilizado por la ruta predeterminada (consulte el listado 10).

**Listado 10 - Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Después de realizar estos cambios, el Administrador de contactos se ejecutará correctamente. Ahora, usará la clase de controlador de contacto como el controlador predeterminado.

## <a name="summary"></a>Resumen

En esta primera iteración, hemos creado una aplicación básica de Contact Manager en la forma más rápida posible. Se aprovechó de Visual Studio para generar automáticamente el código inicial para nuestros controladores y vistas. También se aprovechó de Entity Framework para generar las clases de modelo de base de datos automáticamente.

Actualmente, nos podemos enumerar, crear, editar y eliminar registros de contactos con la aplicación de Contact Manager. En otras palabras, podemos realizar todas las operaciones de base de datos básica requeridas por una aplicación web controlada por base de datos.

Por desgracia, nuestra aplicación tiene algunos problemas. Primero y me dude admitirlo, la aplicación de Contact Manager no es la aplicación más atractiva. Necesita algún trabajo de diseño. En la siguiente iteración, daremos un vistazo a cómo se puede cambiar la página maestra de la vista predeterminada y la hoja de estilos en cascada para mejorar la apariencia de la aplicación.

En segundo lugar, no hemos implementado ninguna validación de formulario. Por ejemplo, no hay nada para evitar que se envíe el formulario de contacto de creación sin escribir valores para cualquiera de los campos del formulario. Además, puede escribir direcciones de correo electrónico y números de teléfono no válido. Empezamos a abordar el problema de validación de formulario en iteración #3.

Por último y lo más importante, la iteración actual de la aplicación de Contact Manager no puede modificar ni mantiene fácilmente. Por ejemplo, la lógica de acceso de la base de datos está incluida directamente en las acciones del controlador. Esto significa que no podemos modificar nuestro código de acceso a datos sin modificar de nuestros controladores. En las iteraciones posteriores, se exploran patrones de diseño de software que se puede implementar para que el Administrador de contactos sean más resistentes a cambiar.

> [!div class="step-by-step"]
> [Siguiente](iteration-2-make-the-application-look-nice-cs.md)
