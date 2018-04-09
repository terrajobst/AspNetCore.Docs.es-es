---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usar el calendario HTML5 y jQuery UI Datepicker emergente con ASP.NET MVC - parte 3 | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Usar el calendario HTML5 y jQuery UI Datepicker emergente con ASP.NET MVC - parte 3
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de MVC de ASP.NET.


## <a name="working-with-complex-types"></a>Trabajar con tipos complejos

En esta sección podrá crear una clase de dirección y obtenga información acerca de cómo crear una plantilla para mostrarla.

En el *modelos* carpeta, cree un nuevo archivo de clase denominado *Person.cs* en la que se colocarán dos tipos: un `Person` clase y un `Address` clase. El `Person` clase contendrá una propiedad que se escribe como `Address`. El `Address` tipo es un tipo complejo, lo que significa que no es uno de los tipos integrados como `int`, `string`, o `double`. En su lugar, tiene varias propiedades. El código de las nuevas clases tiene el siguiente aspecto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

En el `Movie` controlador, agregue las siguientes `PersonDetail` acción para mostrar una instancia de la persona:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

A continuación, agregue el código siguiente a la `Movie` controlador para rellenar el `Person` modelo con datos de ejemplo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Abra la *Views\Movies\PersonDetail.cshtml* de archivos y agregue el siguiente marcado para la `PersonDetail` vista.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Presione Ctrl + F5 para ejecutar la aplicación y vaya a *películas/PersonDetail*.

El `PersonDetail` vista no contiene el `Address` tipo complejo, como puede verse en esta captura de pantalla. (No se muestra ninguna dirección).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

El `Address` datos del modelo no se muestran porque es un tipo complejo. Para mostrar la información de dirección, abra el *Views\Movies\PersonDetail.cshtml* archivo nuevo y agregue el siguiente marcado.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

El marcado completo para el `PersonDetail` ahora vista tiene el siguiente aspecto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Ejecute de nuevo la aplicación y mostrar la `PersonDetail` vista. Ahora se muestra la información de dirección:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Crear una plantilla para un tipo complejo

En esta sección creará una plantilla que se usará para representar la `Address` tipo complejo. Cuando se crea una plantilla para el `Address` tipo, ASP.NET MVC automáticamente sirve para dar formato a un modelo de dirección en cualquier parte de la aplicación. Esto proporciona una manera de controlar la representación de la `Address` tipo desde un solo sitio, en la aplicación.

En el *Views\Shared\DisplayTemplates* carpeta, crear una vista parcial fuertemente tipada denominada **dirección**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Haga clic en **agregar**y, a continuación, abra el nuevo *Views\Shared\DisplayTemplates\Address.cshtml* archivo. La nueva vista contiene el siguiente marcado generado:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Ejecute la aplicación y mostrar la `PersonDetail` vista. En esta ocasión, el `Address` plantilla que acaba de crear se usa para mostrar el `Address` tipo complejo, por lo que la pantalla será similar al siguiente:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Resumen: Formas para especificar el formato de presentación de modelo y la plantilla

Ha visto que puede especificar el formato o la plantilla para una propiedad de modelo mediante los métodos siguientes:

- Aplicar el `DisplayFormat` atributo a una propiedad en el modelo. Por ejemplo, el código siguiente hace que la fecha que debe mostrarse sin la hora:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Aplicar un [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo a una propiedad en el modelo y se especifica el tipo de datos. Por ejemplo, el código siguiente hace que la fecha que debe mostrarse sin la hora.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Si la aplicación contiene un *date.cshtml* plantilla en el *Views\Shared\DisplayTemplates* carpeta o el *Views\Movies\DisplayTemplates* carpeta, esa plantilla se usará para representar la `DateTime` propiedad. En caso contrario, el sistema de creación de plantillas ASP.NET integrado muestra la propiedad como una fecha.
- Crear una plantilla de pantalla en el *Views\Shared\DisplayTemplates* carpeta o el *Views\Movies\DisplayTemplates* carpeta cuyo nombre coincida con el tipo de datos que desea dar formato. Por ejemplo, hemos visto que la *Views\Shared\DisplayTemplates\DateTime.cshtml* se usa para representar `DateTime` propiedades en un modelo, sin tener que agregar un atributo para el modelo y sin tener que agregar todas las marcas a vistas.
- Mediante el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo en el modelo para especificar la plantilla para mostrar la propiedad de modelo.
- Agregar explícitamente el nombre de plantilla para mostrar la [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) llama en una vista.

El método utilizado depende de lo que necesita hacer en la aplicación. No es raro combinar estos enfoques para obtener exactamente el tipo de formato que necesita.

En la sección siguiente, cambiará el tercio un poco y mover de personalizar cómo se muestran los datos para personalizar la forma de especificar. Podrá enlazar el datepicker de jQuery a las vistas de edición en la aplicación con el fin de proporcionar una manera fácil de especificar las fechas.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
