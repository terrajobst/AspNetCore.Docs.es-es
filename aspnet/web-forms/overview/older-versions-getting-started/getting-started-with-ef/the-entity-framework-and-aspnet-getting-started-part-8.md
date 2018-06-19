---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introducción a la base de datos de Entity Framework 4.0 en primer lugar y ASP.NET 4 formularios Web Forms - parte 8 | Documentos de Microsoft
author: tdykstra
description: La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. Es la aplicación de ejemplo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 035cce022d1b3697b825a96487529dbc9675d90e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887894"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Introducción a la base de datos de Entity Framework 4.0 en primer lugar y ASP.NET 4 Web Forms - parte 8
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Utilizando la funcionalidad de datos dinámicos para dar formato y validar datos

En el tutorial anterior implementa procedimientos almacenados. Este tutorial le mostrará cómo la funcionalidad de datos dinámicos puede proporcionar las siguientes ventajas:

- Campos automáticamente se da formato de presentación de acuerdo con su tipo de datos.
- Campos se validan automáticamente según el tipo de datos.
- Puede agregar metadatos al modelo de datos para personalizar el comportamiento de formato y validación. Al hacer esto, puede agregar las reglas de validación y formato en un solo lugar y automáticamente se aplican en cualquier lugar de tener acceso a los campos mediante controles de datos dinámicos.

Para ver cómo funciona esto, podrá cambiar los controles que usar para mostrar y editar campos existente *Students.aspx* página y agregará los metadatos de validación y formato a los campos de nombre y la fecha de la `Student` tipo de entidad.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Uso de DynamicField y controles de DynamicControl

Abra la *Students.aspx* página y en el `StudentsGridView` control reemplazar la **nombre** y **fecha de inscripción** `TemplateField` elementos con el siguiente marcado:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Este marcado utiliza `DynamicControl` controla en lugar de `TextBox` y `Label` campo de la plantilla de nombre de controles de los estudiantes y usa un `DynamicField` control para la fecha de inscripción. No se especificó ninguna cadena de formato.

Agregar un `ValidationSummary` controle tras el `StudentsGridView` control.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

En el `SearchGridView` control reemplace el marcado para la **nombre** y **fecha de inscripción** columnas como se hacían en el `StudentsGridView` controlar, salvo que omite el `EditItemTemplate` elemento. El `Columns` elemento de la `SearchGridView` control contiene ahora el siguiente marcado:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Abra *Students.aspx.cs* y agregue el siguiente `using` instrucción:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Agregue un controlador para la página `Init` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Este código especifica que proporcione datos dinámicos de formato y validación en estos controles enlazados a datos para los campos de la `Student` entidad. Si recibe un mensaje de error similar al ejemplo siguiente cuando ejecute la página, normalmente significa que ha olvidado llamar a la `EnableDynamicData` método `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Ejecute la página.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

En el **fecha de inscripción** columna, se muestra la hora junto con la fecha porque el tipo de propiedad es `DateTime`. Lo corregirá más adelante.

Por ahora, tenga en cuenta que los datos dinámicos proporciona automáticamente una validación de datos básicos. Por ejemplo, haga clic en **editar**, borre el campo de fecha, haga clic en **actualización**, y verá que los datos dinámicos automáticamente lo hace un campo obligatorio porque el valor no es que aceptan valores NULL en el modelo de datos. La página muestra un asterisco después el campo y un mensaje de error en el `ValidationSummary` control:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Puede omitir el `ValidationSummary` controlar, ya que también puede mantener el puntero del mouse sobre el asterisco para ver el mensaje de error:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Datos dinámicos también validarán que los datos especificados en el **fecha de inscripción** campo es una fecha válida:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Como puede ver, este es un mensaje de error genérico. En la siguiente sección verá cómo personalizar los mensajes, así como la validación y reglas de formato.

## <a name="adding-metadata-to-the-data-model"></a>Agregar metadatos al modelo de datos

Normalmente, desea personalizar la funcionalidad proporcionada por los datos dinámicos. Por ejemplo, puede cambiar cómo se muestran los datos y el contenido de mensajes de error. Normalmente también personalizar las reglas de validación de datos para proporcionar más funcionalidad que los datos dinámicos proporcionan automáticamente en función de los tipos de datos. Para ello, cree las clases parciales que corresponden a tipos de entidad.

En **el Explorador de soluciones**, haga clic en el **ContosoUniversity** proyecto, seleccione **Agregar referencia**y agregue una referencia a `System.ComponentModel.DataAnnotations`.

[![image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

En el *DAL* carpeta, cree un nuevo archivo de clase, asígnele el nombre *Student.cs*y reemplace el código de plantilla en ella con el código siguiente.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Este código crea una clase parcial para la `Student` entidad. El `MetadataType` atributo aplicado a esta clase parcial identifica la clase que va a utilizar para especificar los metadatos. La clase de metadatos puede tener cualquier nombre, pero usa el nombre de la entidad junto con "Metadatos" es una práctica común.

Los atributos aplicados a las propiedades de la clase de metadatos especifican el formato, los mensajes de error, reglas y validación. Los atributos que se muestran aquí tendrá los siguientes resultados:

- `EnrollmentDate` se mostrará como una fecha (sin una hora).
- Los campos de nombre deben ser de 25 caracteres o menor en longitud y un mensaje de error personalizado se ha proporcionado.
- Los campos de nombre son necesarios y se proporciona un mensaje de error personalizado.

Ejecute el *Students.aspx* página nuevo, y verá que ahora se muestran las fechas sin veces:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Editar una fila e intente borrar los valores de los campos de nombre. Los asteriscos que indican errores de campo que aparecen en cuanto se deja un campo, antes de hacer clic **actualización**. Al hacer clic en **actualización**, la página muestra el texto del mensaje de error especificado.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Intenta escribir nombres que tengan más de 25 caracteres, haga clic en **actualización**, y la página muestra el texto del mensaje de error especificado.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Ahora que ha configurado estas reglas de validación y formato en los metadatos del modelo de datos, las reglas se aplicarán automáticamente en cada página que muestra o permite que los cambios en estos campos, por lo que siempre que se use `DynamicControl` o `DynamicField` controles. Esto reduce la cantidad de código redundante deberá escribir, lo que facilita la programación y realización de pruebas, y se asegura de que la validación y formato de datos son coherentes en toda una aplicación.

## <a name="more-information"></a>Más información

Esto concluye la siguiente serie de tutoriales de introducción a Entity Framework. Para que obtener más recursos para ayudarle a aprender a usar Entity Framework, continúe con [el primer tutorial de la siguiente serie de tutoriales de Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) o visite los siguientes sitios:

- [Preguntas más frecuentes de Entity Framework](http://www.ef-faq.org/introduction.html)
- [El Blog del equipo de Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework en MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework en el centro de desarrollo de datos de MSDN](https://msdn.microsoft.com/data/ef.aspx)
- [Información general del Control EntityDataSource Web Server en MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [Control EntityDataSource referencia de API en MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Foros de Entity Framework en MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog de Julie Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-7.md)
