---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 11f9e0a8602e9ee1feda9d0e7d0d319add7c440c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400776"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 3
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de ASP.NET MVC.


## <a name="working-with-complex-types"></a>Trabajar con tipos complejos

En esta sección podrá crear una clase de dirección y obtenga información sobre cómo crear una plantilla para que se muestre.

En el *modelos* carpeta, cree un nuevo archivo de clase denominado *Person.cs* que colocará dos tipos: un `Person` clase y un `Address` clase. El `Person` clase contendrá una propiedad que se escribe como `Address`. El `Address` tipo es un tipo complejo, lo que significa que no es uno de los tipos integrados como `int`, `string`, o `double`. En su lugar, tiene varias propiedades. El código para las nuevas clases tiene este aspecto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

En el `Movie` controlador, agregue las siguientes `PersonDetail` acción para mostrar una instancia de person:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

A continuación, agregue el código siguiente a la `Movie` controlador para rellenar el `Person` modelar con algunos datos de ejemplo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Abra el *Views\Movies\PersonDetail.cshtml* archivo y agregue el siguiente marcado para el `PersonDetail` vista.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Presione Ctrl + F5 para ejecutar la aplicación y vaya a *películas/PersonDetail*.

El `PersonDetail` vista no contiene el `Address` tipo complejo, como puede ver en esta captura de pantalla. (No se muestra ninguna dirección).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

El `Address` no se muestra el modelo de datos porque es un tipo complejo. Para mostrar la información de dirección, abra el *Views\Movies\PersonDetail.cshtml* archivo nuevo y agregue el marcado siguiente.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

El marcado completo para el `PersonDetail` ahora la vista tiene un aspecto similar al siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Ejecute de nuevo la aplicación y mostrar la `PersonDetail` vista. Ahora se muestra la información de dirección:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Creación de una plantilla para un tipo complejo

En esta sección creará una plantilla que se usará para representar el `Address` tipo complejo. Cuando se crea una plantilla para el `Address` tipo, ASP.NET MVC automáticamente sirve para dar formato a un modelo de dirección en cualquier parte de la aplicación. Esto le ofrece una manera de controlar la representación de la `Address` tipo desde un solo lugar en la aplicación.

En el *Views\Shared\DisplayTemplates* carpeta, cree una vista parcial fuertemente tipada denominada **dirección**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Haga clic en **agregar**y, a continuación, abra el nuevo *Views\Shared\DisplayTemplates\Address.cshtml* archivo. La nueva vista contiene el marcado generado siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Ejecute la aplicación y mostrar la `PersonDetail` vista. Esta vez, el `Address` plantilla que acaba de crear se utiliza para mostrar el `Address` tipo complejo, por lo que la pantalla es similar a la siguiente:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Resumen: Formas de especificar el formato de presentación del modelo y la plantilla

Hemos visto que puede especificar el formato o la plantilla para una propiedad de modelo mediante los siguientes enfoques:

- Aplicar el `DisplayFormat` atributo a una propiedad en el modelo. Por ejemplo, el código siguiente hace que la fecha que se mostrará sin la hora:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Aplicar un [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo a una propiedad en el modelo y especificar el tipo de datos. Por ejemplo, el código siguiente hace que la fecha que se mostrará sin incluir el tiempo.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Si la aplicación contiene un *date.cshtml* plantilla en el *Views\Shared\DisplayTemplates* carpeta o el *Views\Movies\DisplayTemplates* carpeta, esa plantilla se usará para representar el `DateTime` propiedad. En caso contrario, el sistema de creación de plantillas ASP.NET integrado muestra la propiedad como una fecha.
- Creación de una plantilla de visualización en el *Views\Shared\DisplayTemplates* carpeta o el *Views\Movies\DisplayTemplates* carpeta cuyo nombre coincide con el tipo de datos que desea dar formato. Por ejemplo, hemos visto que la *Views\Shared\DisplayTemplates\DateTime.cshtml* se usa para representar `DateTime` propiedades en un modelo, sin agregar un atributo para el modelo y sin agregar ningún marcado para las vistas.
- Mediante el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo en el modelo para especificar la plantilla para mostrar la propiedad del modelo.
- Agregar explícitamente el nombre de plantilla para mostrar el [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) llama en una vista.

El método que se utilice depende de lo que necesita hacer en la aplicación. No es raro combinar ambos métodos para obtener exactamente el tipo de formato que necesita.

En la sección siguiente, tendrá que cambiar un poco de artes y mover de personalizar cómo se muestran los datos para personalizar la forma de especificar. Podrá enlazar datepicker de jQuery a las vistas de edición de la aplicación con el fin de proporcionar una manera sorprendente para especificar las fechas.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
