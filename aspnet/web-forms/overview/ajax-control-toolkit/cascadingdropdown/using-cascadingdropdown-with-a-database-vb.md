---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Usar CascadingDropDown con una base de datos (VB) | Documentos de Microsoft
author: wenz
description: El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores de anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 65b9a499dd9b500338ccdb90e56b742ff50a1024
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>Usar CascadingDropDown con una base de datos (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. En orden para que funcione, debe crearse un servicio web especial.


## <a name="overview"></a>Información general

El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de US Estados y la lista siguiente, a continuación, se rellena con ciudades importantes en ese estado.) En orden para que funcione, debe crearse un servicio web especial.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos de AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y bases de datos de ejemplo (descargue en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae 4bd1 4e3d 94b8 5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar la `AdventureWorks.mdf` archivo de base de datos.

En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada. Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el &lt; `form` &gt; elemento):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

En el paso siguiente, se necesitan dos controles de DropDownList. En este ejemplo, usamos la información de contacto y proveedor de AdventureWorks, por lo tanto, creamos una lista para los proveedores disponibles y otra para los contactos disponibles:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

A continuación, dos extensores CascadingDropDown deben agregarse a la página. Uno rellena la primera lista (proveedores) y el otro llena la segunda lista (contactos). Se deben establecer los siguientes atributos:

- `ServicePath`: Dirección URL de un servicio web entrega las entradas de lista
- `ServiceMethod`: Método web entrega de las entradas de lista
- `TargetControlID`: Id. de la lista desplegable
- `Category`: Información de categoría que se envía al método web cuando se llama
- `PromptText`: Texto que aparece cuando asincrónicamente Cargando datos de la lista desde el servidor
- `ParentControlID`: (opcional) que la carga de los desencadenadores de la lista actual de lista desplegable elemento primario

Según el lenguaje de programación utilizado, cambiará el nombre del servicio web en cuestión, pero todos los demás valores de atributo son los mismos. Este es el elemento CascadingDropDown para la primera lista desplegable:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Los extensores de control para la segunda lista necesitan establecer el `ParentControlID` atributo para que al seleccionar una entrada en los desencadenadores de la lista de proveedores de carga asociadas a elementos de la lista de contactos.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

A continuación, se realiza el trabajo real en el servicio web, que se configura como se indica a continuación. Tenga en cuenta que el `[ScriptService]` se usa el atributo, en caso contrario, AJAX de ASP.NET no se puede crear el proxy de JavaScript para tener acceso a los métodos web desde el código de script del lado cliente.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

La firma de los métodos web llamado a CascadingDropDown es como sigue:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Por lo que el valor devuelto debe ser una matriz de tipo `CascadingDropDownNameValue` que se define mediante el Kit de herramientas de Control. El `GetVendors()` método es bastante fácil de implementar: el código se conecta a la base de datos de AdventureWorks y consulta los primeros 25 proveedores. El primer parámetro de la `CascadingDropDownNameValue` constructor es el título de la entrada de la lista, la segunda se su valor (atributo value del HTML &lt; `option` &gt; elemento). Este es el código:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Obtener los contactos asociados para un proveedor (nombre del método: `GetContactsForVendor()`) es un poco más complicado. En primer lugar, debe determinarse el proveedor que se ha seleccionado en la primera lista desplegable. El Kit de herramientas de Control define un método auxiliar para dicha tarea: el `ParseKnownCategoryValuesString()` método devuelve un `StringDictionary` elemento con los datos de la lista desplegable:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Por motivos de seguridad, estos datos deben validarse en primer lugar. Por tanto, si hay una entrada de proveedor (dado que la `Category` propiedad del primer elemento CascadingDropDown está establecida en `"Vendor"`), se puede recuperar el Id. del proveedor seleccionado:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

El resto del método es bastante sencilla, a continuación. Id. del proveedor se utiliza como un parámetro para una consulta SQL que recupera todos los contactos asociados para ese proveedor. Una vez más, el método devuelve una matriz de tipo `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Cargar la página ASP.NET y, después de un breve período de tiempo se rellena la lista de proveedores con 25 entradas. Seleccione una entrada y observe cómo la segunda lista desplegable se rellena con datos.


[![La primera lista se rellena automáticamente](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

La primera lista se rellena automáticamente ([haga clic aquí para ver la imagen a tamaño completo](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![La segunda lista se rellena según la selección en la primera lista](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

La segunda lista se rellena según la selección en la primera lista ([haga clic aquí para ver la imagen a tamaño completo](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

>[!div class="step-by-step"]
[Anterior](filling-a-list-using-cascadingdropdown-vb.md)
[Siguiente](presetting-list-entries-with-cascadingdropdown-vb.md)
