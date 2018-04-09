---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Iteración #1: crear la aplicación (VB) | Documentos de Microsoft'
author: microsoft
description: 'En la primera iteración, se creará el póngase en contacto con el Administrador de la manera más sencilla posible. Se agrega compatibilidad para las operaciones de base de datos básicos: crear, leer, actualizar y D...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 52029816bd9f37c3d5c3321d3c5e60599314a33b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="iteration-1--create-the-application-vb"></a>Iteración #1: crear la aplicación (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> En la primera iteración, se creará el póngase en contacto con el Administrador de la manera más sencilla posible. Se agrega compatibilidad para las operaciones de base de datos básicos: crear, lectura, actualización y eliminación (CRUD).


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

En esta primera iteración, se genere la aplicación básica. El objetivo es crear el póngase en contacto con el Administrador de la manera más rápida y más sencilla posible. En las iteraciones posteriores, mejorar el diseño de la aplicación.

La aplicación de Contact Manager es una aplicación básica de bases de datos. Puede usar la aplicación para crear nuevos contactos, editar contactos existentes y eliminar contactos.

En esta iteración se completarán los pasos siguientes:

1. Aplicación de ASP.NET MVC
2. Crear una base de datos para almacenar nuestra contactos
3. Generar una clase de modelo para nuestra base de datos con Microsoft Entity Framework
4. Crear una acción de controlador y la vista que nos permite enumerar todos los contactos de la base de datos
5. Crear acciones del controlador y una vista que nos permite crear un nuevo contacto en la base de datos
6. Crear acciones del controlador y una vista que nos permite editar un contacto ya existente en la base de datos
7. Crear acciones del controlador y una vista que nos permite eliminar un contacto ya existente en la base de datos

## <a name="software-prerequisites"></a>Requisitos previos de software

En las aplicaciones de ASP.NET MVC, debe tener Visual Studio 2008 o Visual Web Developer 2008 instalado en el equipo (Visual Web Developer es una versión gratuita de Visual Studio que no incluye todas las características avanzadas de Visual Studio). Puede descargar la versión de prueba de Visual Studio 2008 o Visual Web Developer desde la siguiente dirección:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Las aplicaciones de MVC de ASP.NET con Visual Web Developer, debe tener Visual Web Developer Service Pack 1 instalado. Sin Service Pack 1, no se puede crear proyectos de aplicación Web.


Marco de ASP.NET MVC. Puede descargar el marco de MVC de ASP.NET desde la siguiente dirección:

[https://www.asp.net/mvc](../../../index.md)

En este tutorial, usamos Microsoft Entity Framework para tener acceso a una base de datos. Entity Framework se incluye con .NET Framework 3.5 Service Pack 1. Puede descargar este service pack desde la siguiente ubicación:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Como alternativa a la realización de cada una de estas descargas uno por uno, puede aprovechar las ventajas del instalador de plataforma Web (Web PI). Puede descargar el instalador de plataforma Web desde la siguiente dirección:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Proyecto de ASP.NET MVC

Proyecto de aplicación Web de ASP.NET MVC. Inicie Visual Studio y seleccione la opción de menú **archivo, nuevo proyecto**. El **nuevo proyecto** aparece de cuadro de diálogo (consulte la figura 1). Seleccione el **Web** tipo de proyecto y la **aplicación Web ASP.NET MVC** plantilla. Nombre para el nuevo proyecto *ContactManager* y haga clic en el botón Aceptar.


Asegúrese de que tiene .NET Framework 3.5 seleccionado en la lista desplegable en la parte superior derecha de la **nuevo proyecto** cuadro de diálogo. En caso contrario, la plantilla de aplicación Web ASP.NET MVC won t aparece.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Figura 01**: cuadro de diálogo de nuevo el proyecto ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image2.png))


Aplicación de ASP.NET MVC, la **crear proyecto de prueba unitaria** aparece el cuadro de diálogo. Puede utilizar este cuadro de diálogo para indicar que desea crear y agregar un proyecto de prueba unitaria a la solución cuando se crea la aplicación de ASP.NET MVC. Aunque ganados t puede generar pruebas unitarias en esta iteración, debe seleccionar la opción **Sí, crear un proyecto de prueba unitaria** como tenemos previsto agregar pruebas unitarias en una iteración posterior. Agregar un proyecto de prueba cuando se crea por primera vez un nuevo proyecto de MVC de ASP.NET es mucho más sencillo que agregar un proyecto de prueba una vez creado el proyecto de MVC de ASP.NET.

> [!NOTE] 
> 
> Dado que Visual Web Developer no admite proyectos de prueba, no se obtiene el cuadro de diálogo Crear proyecto de prueba unitaria cuando se utiliza Visual Web Developer.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Figura 02**: cuadro de diálogo el proyecto de prueba unitaria crear ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image4.png))


Aplicación de ASP.NET MVC se muestra en la ventana Explorador de soluciones de Visual Studio (consulte la figura 3). Si don t ve la ventana Explorador de soluciones, a continuación, puede abrir esta ventana, seleccione la opción de menú **la vista, el Explorador de soluciones**. Tenga en cuenta que la solución contiene dos proyectos: el proyecto de MVC de ASP.NET y el proyecto de prueba. El proyecto de MVC de ASP.NET se denomina ContactManager y el proyecto de prueba se denomina ContactManager.Tests.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Figura 03**: ventana del explorador de soluciones ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Eliminar los archivos de ejemplo de proyecto

La plantilla de proyecto de ASP.NET MVC incluye archivos de ejemplo para controladores y vistas. Antes de crear una nueva aplicación MVC de ASP.NET, debe eliminar estos archivos. Puede eliminar archivos y carpetas en la ventana Explorador de soluciones, con el botón secundario en un archivo o carpeta y seleccione la opción de menú **eliminar**.

Debe eliminar los siguientes archivos desde el proyecto de MVC de ASP.NET:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Y necesita eliminar el siguiente archivo del proyecto de prueba:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Crear la base de datos

La aplicación póngase en contacto con el administrador es una aplicación web controlada por la base de datos. Se usa una base de datos para almacenar la información de contacto.

El marco de MVC de ASP.NET con cualquier base de datos moderna, incluidas bases de datos IBM DB2, Oracle, MySQL y Microsoft SQL Server. En este tutorial, se usa una base de datos de Microsoft SQL Server. Cuando se instala Visual Studio, se le proporcionará la opción de instalar Microsoft SQL Server Express que es una versión gratuita de la base de datos de Microsoft SQL Server.

Crear una nueva base de datos haciendo clic en la aplicación\_carpeta de datos en la ventana Explorador de soluciones y seleccionando la opción de menú **agregar, nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione la **datos** categoría y el **base de datos de SQL Server** plantilla (consulte la figura 4). Nombre de la nueva base de datos ContactManagerDB.mdf y haga clic en el botón Aceptar.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Figura 04**: crear una nueva base de datos Microsoft SQL Server Express ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image8.png))


Después de crear la nueva base de datos, la base de datos aparece en la aplicación\_carpeta de datos en la ventana Explorador de soluciones. Haga doble clic en el archivo ContactManager.mdf para abrir la ventana Explorador de servidores y conectarse a la base de datos.

> [!NOTE] 
> 
> La ventana Explorador de servidores se denomina la ventana del explorador de base de datos en el caso de Microsoft Visual Web Developer.


Puede utilizar la ventana Explorador de servidores para crear nuevos objetos de base de datos como tablas de base de datos, vistas, desencadenadores y procedimientos almacenados. Haga clic en la carpeta de tablas y seleccione la opción de menú **agregar nueva tabla**. El Diseñador de tablas de base de datos aparece (Véase la figura 5).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Figura 05**: el Diseñador de tablas de base de datos ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image10.png))


Es preciso crear una tabla que contiene las columnas siguientes:

<a id="0.2_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | int | False |
| Nombre | nvarchar(50) | False |
| LastName | nvarchar(50) | False |
| Teléfono | nvarchar(50) | False |
| Correo electrónico | nvarchar(255) | False |


La primera columna, la columna Id., es especial. Debe marcar la columna de identificador como una columna de identidad y una columna de clave principal. Indicar que una columna es una columna de identidad mediante la expansión de propiedades de columna (mire en la parte inferior de la figura 6) y desplazarse hacia abajo hasta la propiedad de la especificación de identidad. Establecer el **(identidad)** valor para la propiedad **Sí**.

Marcar una columna como una columna de clave principal, seleccione la columna y haga clic en el botón con el icono de una clave. Una vez se ha marcado una columna como columna de clave principal, aparece un icono de una clave junto a la columna (consulte la figura 6).

Cuando termine de crear la tabla, haga clic en el botón Guardar (el botón con un icono de un disquete) para guardar la nueva tabla. Asigne el nombre de la nueva tabla *contactos*.

Después de finalizar la creación de la tabla de base de datos de contactos, debe agregar algunos registros a la tabla. Haga clic en la tabla de contactos en la ventana Explorador de servidores y seleccione la opción de menú **mostrar datos de tabla**. Escriba uno o más contactos en la cuadrícula que aparece.

## <a name="creating-the-data-model"></a>Crear el modelo de datos

La aplicación de MVC de ASP.NET consta de modelos, vistas y controladores. Comenzamos creando una clase de modelo que representa la tabla de contactos que hemos creado en la sección anterior.

En este tutorial, usamos Microsoft Entity Framework para generar automáticamente una clase de modelo de la base de datos.

> [!NOTE] 
> 
> El marco de MVC de ASP.NET no está vinculado a Microsoft Entity Framework de ninguna manera. Puede usar ASP.NET MVC con tecnologías de acceso de base de datos alternativo incluidos NHibernate, LINQ to SQL o ADO.NET.


Siga estos pasos para crear las clases del modelo de datos:

1. Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione **agregar, nuevo elemento**. El **Agregar nuevo elemento** aparece de cuadro de diálogo (consulte la figura 6).
2. Seleccione el **datos** categoría y el **ADO.NET Entity Data Model** plantilla. El modelo de datos el nombre *ContactManagerModel.edmx* y haga clic en el **agregar** botón. Aparece el Asistente de Entity Data Model (consulte la figura 7).
3. En el **Elegir contenido del modelo** paso, seleccione **generar desde la base de datos** (consulte la figura 7).
4. En el **elegir la conexión de datos** paso a paso, seleccione la base de datos de ContactManagerDB.mdf y escriba el nombre *ContactManagerDBEntities* para la configuración de conexión de entidad (consulte la figura 8).
5. En el **elija los objetos de base de datos** paso a paso, seleccione la casilla de tablas (consulte la figura 9). El modelo de datos incluirá todas las tablas incluidas en la base de datos (hay solo uno, la tabla Contacts). Escriba el espacio de nombres *modelos*. Haga clic en el botón Finalizar para completar al asistente.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Figura 06**: cuadro de diálogo Agregar nuevo elemento ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image12.png))


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Figura 07**: Elegir contenido del modelo ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image14.png))


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Figura 08**: elegir la conexión de datos ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image16.png))


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Figura 09**: elija los objetos de base de datos ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image18.png))


Después de completar al Asistente para Entity Data Model, aparece el Entity Data Model Designer. El diseñador muestra una clase que corresponde a cada tabla que se está modelando. Debería ver una clase denominada contactos.

El Asistente de Entity Data Model genera nombres de las clases basados en nombres de tabla de base de datos. Casi siempre debe cambiar el nombre de la clase generada por el asistente. Haga clic en la clase de contactos en el diseñador y seleccione la opción de menú **cambiar el nombre de**. Cambiar el nombre de la clase de contactos (plurales) para el contacto (singular). Después de cambiar el nombre de clase, la clase debe aparecer como la figura 10.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Figura 10**: clase Contact The ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image20.png))


En este punto, hemos creado nuestro modelo de base de datos. Podemos usar la clase Contact para representar un determinado registro de contacto en nuestra base de datos.

## <a name="creating-the-home-controller"></a>Crear el controlador Home

El siguiente paso es crear nuestro controlador Home. El controlador Home es el controlador predeterminado que se invoca en una aplicación ASP.NET MVC.

Crear la clase de controlador de inicio haciendo clic en la carpeta de controladores en la ventana Explorador de soluciones y seleccionando la opción de menú **agregar, controlador** (consulte la figura 11). Observe la casilla **agregar métodos de acción para los escenarios de creación, actualización y detalles**. Asegúrese de que está activada esta casilla de verificación antes de hacer clic en el **agregar** botón.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Figura 11**: agregar el controlador Home ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image22.png))


Cuando se crea el controlador Home, obtener la clase en la lista 1.

**Lista 1 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Enumerar los contactos

Para mostrar los registros en la tabla de base de datos de contactos, necesitamos crear una acción de Index() y una vista de índice.

El controlador Home ya contiene una acción Index(). Es necesario modificar este método para que se parezca al listado 2.

**La lista 2 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Tenga en cuenta que la clase de controlador de inicio en el listado 2 contiene un campo privado denominado \_entidades. La \_campos de entidades representa las entidades del modelo de datos. Usamos el \_campo entidades para comunicarse con la base de datos.

El método Index() devuelve una vista que representa todos los contactos de la tabla de base de datos de contactos. La expresión \_entidades. ContactSet.ToList() devuelve la lista de contactos como una lista genérica.

Ahora que se ha creado el controlador de índice, que a continuación, necesitamos crear la vista de índice. Antes de crear la vista de índice, compile la aplicación seleccionando la opción de menú **compilar, compilar solución**. Siempre debe compilar el proyecto antes de agregar una vista en orden de la lista de clases de modelo que se mostrará en el **agregar vista** cuadro de diálogo.

Crear la vista de índice haciendo clic en el método Index() y seleccionar la opción de menú **agregar vista** (consulte la figura 12). Al seleccionar esta opción de menú se abre la **agregar vista** cuadro de diálogo (Véase la figura 13).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Figura 12**: agregar la vista de índice ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image24.png))


En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada**. Seleccione la clase de datos de vista ContactManager.Contact y la lista Ver contenido. Al seleccionar estas opciones, genera una vista que muestra una lista de registros de contacto.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Figura 13**: cuadro de diálogo de agregar la vista ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image26.png))


Al hacer clic en el **agregar** botón, la vista de índice en el listado 3 se genera. Observe el &lt;% @ Page %&gt; directiva que aparece en la parte superior del archivo. La vista de índice se hereda de la ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; clase. En otras palabras, la clase del modelo en la vista representa una lista de entidades de contacto.

El cuerpo de la vista de índice contiene un bucle foreach que recorre en iteración cada uno de los contactos representados por la clase del modelo. El valor de cada propiedad de la clase de contacto se muestra dentro de una tabla HTML.

**El listado 3 - Views\Home\Index.aspx (sin cambios)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Es necesario realizar una modificación en la vista de índice. Dado que no vamos a crear una vista de detalles, podemos quitar el vínculo de detalles. Busque y quite el siguiente código desde la vista de índice:

{.id = elemento. % De ID})&gt;

Después de modificar la vista de índice, puede ejecutar la aplicación póngase en contacto con el administrador. Seleccione la opción de menú Depurar, Iniciar depuración o simplemente presione F5. La primera vez que ejecute la aplicación, obtendrá el cuadro de diálogo en la figura 14. Seleccione la opción **modificar el archivo Web.config para habilitar la depuración** y haga clic en el botón Aceptar.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Figura 14**: habilitar la depuración ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image28.png))


La vista de índice se devuelve de forma predeterminada. Esta vista muestra todos los datos de la tabla de base de datos de contactos (consulte la figura 15).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Figura 15**: vista del índice ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image30.png))


Tenga en cuenta que la vista de índice incluye un vínculo con la etiqueta crear nuevo en la parte inferior de la vista. En la siguiente sección, aprenderá a crear nuevos contactos.

## <a name="creating-new-contacts"></a>Crear nuevos contactos

Para habilitar a los usuarios crear nuevos contactos, tenemos que agregar dos acciones Create() para el controlador Home. Es necesario crear una acción de Create() que devuelve un formulario HTML para crear un nuevo contacto. Es necesario crear una segunda acción Create() que realiza la inserción de base de datos real del nuevo contacto.

Los nuevos métodos Create() que tenemos que agregar al controlador principal están contenidos en el listado 4.

**Listado 4 - Controllers\HomeController.vb (con crear métodos)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

El primer método Create() se puede invocar con un GET de HTTP, mientras que el segundo método Create() solo se puede invocar una solicitud HTTP POST. En otras palabras, se puede invocar el segundo método Create() solo al enviar un formulario HTML. El primer método Create() simplemente devuelve una vista que contiene el formulario HTML para crear un nuevo contacto. El segundo método Create() es mucho más interesante: agrega el nuevo contacto a la base de datos.

Tenga en cuenta que el segundo método Create() se ha modificado para aceptar una instancia de la clase Contact. Los valores del formulario registrados en el formulario HTML están enlazados a esta clase póngase en contacto con el marco de MVC de ASP.NET automáticamente. Cada campo de formulario desde el formulario de creación de HTML se asigna a una propiedad del parámetro de contacto.

Tenga en cuenta que el parámetro de contacto se decora con un atributo [Bind]. El atributo [Bind] se usa para excluir la propiedad de Id. de contacto del enlace. Dado que la propiedad Id. representa una propiedad Identity, no queremos t desea establecer la propiedad Id.

En el cuerpo del método Create(), Entity Framework se utiliza para insertar el nuevo contacto en la base de datos. El nuevo contacto se agrega al conjunto existente de contactos y se llama al método SaveChanges() para insertar los cambios de nuevo a la base de datos subyacente.

Puede generar un formulario HTML para crear nuevos contactos haciendo clic en cualquiera de los dos métodos Create() y seleccionar la opción de menú **agregar vista** (Véase la figura 16).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Figura 16**: agregar la vista de creación ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image32.png))


En el **agregar vista** cuadro de diálogo, seleccione la **ContactManager.Contact** clase y la **crear** opción para ver el contenido (vea la figura 17). Al hacer clic en el **agregar** botón Crear vista se genera automáticamente.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Figura 17**: ver una página seccionar ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image34.png))


La vista de creación contiene campos de formulario para cada una de las propiedades de la clase Contact. El código de la vista de creación se incluye en el listado 5.

**Listado 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Después de modificar los métodos Create() y agregar la vista de creación, puede ejecutar la aplicación de administrador de contacto y crear nuevos contactos. Haga clic en el **crear nuevo** vínculo que aparece en la vista de índice para navegar a la vista de creación. Debería ver la vista en la figura 18.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Figura 18**: The Create View ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image36.png))


## <a name="editing-contacts"></a>Editar contactos

Agregar la funcionalidad para editar un registro de contacto es muy similar a agregar la funcionalidad para la creación de nuevos registros de contacto. En primer lugar, necesitamos agregar dos nuevos métodos de edición a la clase del controlador principal. Estos nuevos métodos de Edit() están contenidos en el listado 6.

**Listado 6 - Controllers\HomeController.vb (con los métodos de edición)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

El primer método Edit() se invoca una operación GET de HTTP. Un parámetro de identificador se pasa a este método que representa el identificador de registro de contacto que se está editando. Entity Framework se utiliza para recuperar un contacto que coincida con el identificador. Se devuelve una vista que contiene un formulario HTML para editar un registro.

El segundo método Edit() realiza la actualización real de la base de datos. Este método acepta una instancia de la clase Contact como un parámetro. El marco de ASP.NET MVC enlaza los campos del formulario desde el formulario de edición para esta clase automáticamente. Observe que no t incluyen el atributo [Bind] cuando se edita un contacto (se necesita el valor de la propiedad Id).

Entity Framework se usa para guardar el contacto modificado en la base de datos. El contacto original se debe recuperar de la base de datos en primer lugar. A continuación, se llama al método de Entity Framework ApplyPropertyChanges() para registrar los cambios en el contacto. Por último, se llama al método de Entity Framework SaveChanges() para conservar los cambios a la base de datos subyacente.

Puede generar la vista que contiene el formulario de edición haciendo clic en el método Edit() y seleccionar la opción de menú Agregar vista. En el cuadro de diálogo Agregar vista, seleccione la **ContactManager.Models.Contact** clase y la **editar** ver contenido (Véase la figura 19).


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Figura 19**: agregar una vista de edición ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image38.png))


Al hacer clic en el botón Agregar, se genera automáticamente una nueva vista de edición. El formulario HTML que se genera contiene campos que corresponden a cada una de las propiedades de la clase Contact (consulte el listado 7).

**Listado 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Eliminar contactos

Si desea eliminar los contactos, a continuación, debe agregar dos acciones Delete() a la clase de controlador principal. La primera acción Delete() muestra un formulario de confirmación de eliminación. La segunda acción Delete() lleva a cabo la eliminación real.

> [!NOTE] 
> 
> Más adelante, en la iteración #7, modificamos el póngase en contacto con el administrador para que admita una paso Ajax eliminar.


Los dos nuevos métodos de Delete() están contenidos en el listado 8.

**Enumerar 8 - Controllers\HomeController.vb (métodos de eliminación)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

El primer método Delete() devuelve un formulario de confirmación para eliminar un registro de contacto de la base de datos (vea Figure20). El segundo método Delete() realiza la operación de eliminación real con la base de datos. Después de que el contacto original se ha recuperado de la base de datos, se llama a los métodos de Entity Framework DeleteObject() y SaveChanges() para llevar a cabo la eliminación de la base de datos.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Figura 20**: la vista de confirmación de eliminación ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image40.png))


Es necesario modificar la vista de índice para que contenga un vínculo para eliminar registros de contactos (Véase la figura 21). Debe agregar el código siguiente a la misma celda de tabla que contiene el vínculo de edición:

{.id = elemento. % De ID})&gt;


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Figura 21**: índice de vista con el vínculo de edición ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image42.png))


A continuación, necesitamos crear la vista de confirmación de eliminación. Haga clic en el método Delete() en la clase de controlador de inicio y seleccione la opción de menú Agregar vista. El cuadro de diálogo Agregar vista aparece (Véase la figura 22).

A diferencia de en el caso de las vistas de lista, crear y editar el cuadro de diálogo Agregar vista no contiene una opción para crear una vista de eliminación. En su lugar, seleccione la **ContactManager.Models.Contact** clase de datos y la **vacía** ver el contenido. Seleccionar la vista vacía opción content será necesario crear la vista de nosotros mismos.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Figura 22**: agregar la vista de confirmación de eliminación ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image44.png))


El contenido de la vista de eliminación se incluye en la lista de 9. Esta vista contiene un formulario que se confirma o no debe ser un contacto determinado eliminado (consulte la figura 21).

**Listado 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Cambiar el nombre del controlador predeterminado

Podría le molesta que el nombre de la clase de controlador para trabajar con los contactos se denomina la clase HomeController. ¿No debe t el controlador se denomina ContactController?

Es bastante fácil de corregir este problema. En primer lugar, es necesario refactorizar el nombre del controlador Home. Abra la clase HomeController en el Editor de código de Visual Studio, haga clic en el nombre de la clase y seleccione la opción de menú **cambiar el nombre de**. Al seleccionar esta opción de menú abre el cuadro de diálogo Cambiar el nombre.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Figura 23**: un nombre de controlador de refactorización ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image46.png))


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Figura 24**: mediante el cuadro de diálogo Cambiar nombre ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image48.png))


Si cambia el nombre de la clase de controlador, Visual Studio actualizará el nombre de la carpeta en la carpeta Views así. Visual Studio cambiará el nombre de la carpeta \Views\Home a la carpeta \Views\Contact.

Después de realizar este cambio, la aplicación ya no tendrá un controlador Home. Al ejecutar la aplicación, obtendrá la página de error en la figura 25.


[![El cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Figura 25**: no hay ningún controlador predeterminado ([haga clic aquí para ver la imagen a tamaño completo](iteration-1-create-the-application-vb/_static/image50.png))


Es necesario actualizar la ruta predeterminada en el archivo Global.asax para usar el controlador de contacto en lugar del controlador principal. Abra el archivo Global.asax y modifique el controlador predeterminado usado por la ruta predeterminada (consulte el listado 10).

**Listing 10 - Global.asax.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Después de realizar estos cambios, el Administrador de contacto se ejecutará correctamente. Ahora, usará la clase de controlador de contacto como el controlador predeterminado.

## <a name="summary"></a>Resumen

En esta primera iteración, creamos una aplicación básica de póngase en contacto con el Administrador de la manera más rápida posible. Aprovechamos de Visual Studio para generar automáticamente el código inicial para nuestro controladores y vistas. También aprovechamos de Entity Framework para generar las clases de modelo de base de datos automáticamente.

Actualmente, podemos enumerar, crear, editar y eliminar registros de contactos con la aplicación de ponerse en contacto con el administrador. En otras palabras, se puede realizar todas las operaciones de base de datos básica requeridas por una aplicación web controlada por la base de datos.

Por desgracia, nuestra aplicación tiene algunos problemas. Primero y puede dudar admitirlo, la aplicación póngase en contacto con el administrador no es la aplicación sea más atractiva. Necesita algún trabajo de diseño. En la siguiente iteración, se examinará cómo podemos cambiar la página maestra de la vista predeterminada y la hoja de estilos en cascada para mejorar el aspecto de la aplicación.

En segundo lugar, no hemos implementado ninguna validación del formulario. Por ejemplo, no hay nada para evitar que se envíe el formulario de contacto de creación sin especificar valores para cualquiera de los campos del formulario. Además, puede escribir direcciones de correo electrónico y de números de teléfono no válido. Empezamos a abordar el problema de validación del formulario en la iteración #3.

Por último y lo más importante, la iteración actual de la aplicación póngase en contacto con el administrador no puede modificar ni mantiene fácilmente. Por ejemplo, la lógica de acceso de la base de datos está preparada derecha en las acciones de controlador. Esto significa que no podemos modificar el código de acceso de datos sin modificar nuestro controladores. En las iteraciones posteriores, exploramos patrones de diseño de software que se puede implementar para hacer más resistente a cambiar el Administrador de contacto.

> [!div class="step-by-step"]
> [Anterior](iteration-7-add-ajax-functionality-cs.md)
> [Siguiente](iteration-2-make-the-application-look-nice-vb.md)
