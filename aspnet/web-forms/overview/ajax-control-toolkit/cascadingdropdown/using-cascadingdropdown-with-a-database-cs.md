---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Usar CascadingDropDown con una base de datos (C#) | Microsoft Docs
author: wenz
description: El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 9930319d2f9443ef3b50a87c7dd3d42b879168c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818753"
---
<a name="using-cascadingdropdown-with-a-database-c"></a>Usar CascadingDropDown con una base de datos (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. En orden para que funcione, debe crearse un servicio web especial.


## <a name="overview"></a>Información general

El control CascadingDropDown de AJAX Control Toolkit amplía un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de nosotros y la lista siguiente, a continuación, se rellena con las principales ciudades en ese estado.) En orden para que funcione, debe crearse un servicio web especial.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos.

Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada. Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del &lt; `form` &gt; elemento):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

En el paso siguiente, se necesitan dos controles de DropDownList. En este ejemplo, usamos el proveedor e información de contacto de AdventureWorks, por lo tanto, creamos una lista para los proveedores disponibles y otra para los contactos disponibles:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

A continuación, dos de los extensores de CascadingDropDown deben agregarse a la página. Uno rellena la primera lista (proveedores), y el otro rellena la segunda lista (contactos). Se deben establecer los siguientes atributos:

- `ServicePath`: Dirección URL de un servicio web entrega las entradas de lista
- `ServiceMethod`: Método web entrega de las entradas de lista
- `TargetControlID`: Id. de la lista desplegable
- `Category`: Información de categoría que se envía al método web cuando se llama
- `PromptText`: Texto que aparece al cargar asincrónicamente los datos de la lista desde el servidor
- `ParentControlID`: (opcional) lista desplegable de primarios muestran que la carga de los desencadenadores de la lista actual

Según el lenguaje de programación usado, cambiará el nombre del servicio web en cuestión, pero todos los demás valores de atributo son los mismos. Este es el elemento CascadingDropDown para la primera lista desplegable:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

Los extensores de control para la segunda lista deben establecer el `ParentControlID` atributo para que seleccione una entrada en los desencadenadores de la lista de proveedores al cargar asociados a elementos de la lista de contactos.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

A continuación, se realiza el trabajo real en el servicio web, que se define como sigue. Tenga en cuenta que el `[ScriptService]` se usa el atributo, en caso contrario, ASP.NET AJAX no se puede crear el proxy de JavaScript para tener acceso a los métodos web desde el código de script de cliente.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

La firma de los métodos web llamado a CascadingDropDown es como sigue:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Por lo que el valor devuelto debe ser una matriz de tipo `CascadingDropDownNameValue` que se define mediante el Kit de herramientas de Control. El `GetVendors()` método es bastante fácil de implementar: el código se conecta a la base de datos AdventureWorks y consulta los primeros 25 proveedores. El primer parámetro de la `CascadingDropDownNameValue` constructor es el título de la entrada de lista, la segunda se su valor (atributo de valor en HTML &lt; `option` &gt; elemento). Este es el código:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

Introducción a los contactos asociados para un proveedor (nombre del método: `GetContactsForVendor()`) es un poco más complicado. En primer lugar, debe determinarse el proveedor que se ha seleccionado en la primera lista desplegable. El Kit de herramientas de Control define un método auxiliar para que la tarea: el `ParseKnownCategoryValuesString()` método devuelve un `StringDictionary` elemento con los datos de la lista desplegable:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Por motivos de seguridad, primero se deben validar estos datos. Por tanto, si hay una entrada de proveedor (dado que el `Category` propiedad del primer elemento CascadingDropDown está establecida en `"Vendor"`), se puede recuperar el identificador del proveedor seleccionado:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

El resto del método es bastante directa, a continuación. Id. del proveedor se utiliza como un parámetro para una consulta SQL que recupera todos los contactos asociados de ese proveedor. Una vez más, el método devuelve una matriz de tipo `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

Cargar la página ASP.NET y, después de un breve período de tiempo se rellena la lista de proveedores con 25 entradas. Seleccione una entrada y observe cómo la segunda lista desplegable se rellena con datos.


[![La primera lista se rellena automáticamente](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

La primera lista se rellena automáticamente ([haga clic aquí para ver imagen en tamaño completo](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![La segunda lista se rellena según la selección en la primera lista](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

La segunda lista se rellena según la selección en la primera lista ([haga clic aquí para ver imagen en tamaño completo](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](filling-a-list-using-cascadingdropdown-cs.md)
> [Siguiente](presetting-list-entries-with-cascadingdropdown-cs.md)
