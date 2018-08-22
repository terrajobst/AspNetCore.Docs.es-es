---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrar Datepicker de JQuery UI con enlace de modelos y formularios web forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 81cc5f010f2c6c7707223679717e34108a5a7aa3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827743"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrar Datepicker de JQuery UI con enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.
> 
> Este tutorial muestra cómo agregar el JQuery UI [widget Datepicker](http://jqueryui.com/datepicker/) a un formulario Web Forms y el modelo de uso de enlace para actualizar la base de datos con el valor seleccionado.
> 
> Este tutorial se basa en el proyecto creado en el [primera](retrieving-data.md) y [segundo](updating-deleting-and-creating-data.md) partes de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>¿Qué va a crear

En este tutorial, necesitará:

1. Agregar una propiedad al modelo para registrar la fecha de inscripción de student
2. Permitir que el usuario seleccionar la fecha de inscripción con el widget Datepicker de JQuery UI
3. Exigir reglas de validación para la fecha de inscripción

El widget Datepicker de JQuery UI permite a los usuarios seleccionar una fecha fácilmente de un calendario emergente cuando el usuario interactúa con el campo. Uso de este widget puede ser más conveniente para los usuarios que escribir manualmente una fecha. Integrar el widget Datepicker en una página que usa el enlace de modelos para las operaciones de datos requiere sólo una pequeña cantidad de trabajo adicional.

## <a name="add-a-new-property-to-the-model"></a>Agregar una nueva propiedad al modelo

En primer lugar, agregará un **Datetime** propiedad a los estudiantes de modelo y migrar ese cambio a la base de datos. Abra **UniversityModels.cs**y agregue el código resaltado para el modelo Student.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

El **RangeAttribute** se incluye para exigir reglas de validación para la propiedad. Para este tutorial, asumiremos que Contoso University se fundó en 1 de enero de 2013 y, por tanto, las fechas de inscripción anteriores no son válidas.

En la ventana de administración de paquetes, agregue una migración, ejecute el comando **AddEnrollmentDate migración agregar**. Tenga en cuenta que el código de la migración agrega la nueva columna de fecha y hora a la tabla Student. Para que coincida con el valor especificado en el RangeAttribute, agregue un valor predeterminado para la nueva columna, tal como se muestra en el código resaltado siguiente.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Guarde los cambios en el archivo de migración.

No es necesario inicializar nuevamente los datos. Por lo tanto, abra **Configuration.cs** en la carpeta de migraciones y quitar o marque como comentario el código en el **inicialización** método. Guarde y cierre el archivo.

Ahora, ejecute el comando **Actualizar base de datos**. Tenga en cuenta que la columna ya existe en la base de datos y todos los registros existentes tienen el valor predeterminado para EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Agregar controles dinámicos para la fecha de inscripción

Ahora, agregará controles para mostrar y editar la fecha de inscripción. En este momento, se edita el valor a través de un cuadro de texto. Más adelante en el tutorial, cambiará el cuadro de texto para el widget Datepicker de JQuery.

En primer lugar, es importante tener en cuenta que no es necesario realizar ningún cambio a la **AddStudent.aspx** archivo. El control DynamicEntity representará de forma automática la nueva propiedad.

Abra **Students.aspx**y agregue el siguiente código resaltado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Ejecute la aplicación y tenga en cuenta que puede establecer el valor de la fecha de inscripción, escriba una fecha. Al agregar un nuevo alumno:

![Configurar fecha](integrating-jquery-ui/_static/image1.png)

O bien, editar un valor existente:

![Editar fecha](integrating-jquery-ui/_static/image2.png)

Escribir las obras de fecha, pero podría no ser que desee proporcionar la experiencia del cliente. En la siguiente sección, permitirá seleccionar una fecha mediante un calendario.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Instalar el paquete de NuGet para trabajar con JQuery UI

El **la interfaz de usuario del zumo** paquete NuGet permite una integración fácil de los widgets de JQuery UI en la aplicación web. Para usar este paquete, instalarlo a través de NuGet.

![agregar la interfaz de usuario del zumo](integrating-jquery-ui/_static/image3.png)

La versión de interfaz de usuario del zumo que instale podría entrar en conflicto con la versión de JQuery en su aplicación. Antes de continuar con este tutorial, pruebe a ejecutar la aplicación. Si se produce un error de JavaScript, debe reconciliar la versión de JQuery. Puede agregar la versión esperada de JQuery en su carpeta de Scripts (versión 1.8.2 en momento de redactar este tutorial), o en Site.master, especifique la ruta de acceso al archivo de JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalizar la plantilla de fecha y hora para incluir el widget Datepicker

El widget Datepicker se agregará a la plantilla de datos dinámicos para editar un valor de fecha y hora. Al agregar el widget a la plantilla, automáticamente se representa en forma de agregar un nuevo alumno y en la vista de cuadrícula para estudiantes de edición. Abra **DateTime\_Edit.ascx**y agregue el siguiente código resaltado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

En el archivo de código subyacente, establecerá las fechas mínimas y máximos para el objeto DatePicker. Al establecer estos valores, se impide que los usuarios navegar a las fechas no válidas. Se recuperarán los valores mínimos y máximo de la **RangeAttribute** en la propiedad de fecha y hora, si se proporciona uno. Abra **DateTime\_Edit.ascx.cs**y agregue el código resaltado siguiente a la página\_Load (método).

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Ejecute la aplicación web y vaya a la página AddStudent. Proporcione valores para los campos y tenga en cuenta que al hacer clic en el cuadro de texto para la fecha de inscripción, se muestra el calendario.

![Selector de fecha](integrating-jquery-ui/_static/image4.png)

Seleccionar una fecha y haga clic en **insertar**. El RangeAttribute exige la validación en el servidor. Al establecer la propiedad minDate en Datepicker, también se aplican validación en el cliente. El calendario no permite que el usuario navegue a una fecha anterior al valor de minDate.

Cuando se edita un registro en la vista de cuadrícula, también se muestra el calendario.

![DatePicker en GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, ha aprendido cómo incorporar un widget de JQuery en un formulario web Forms que usa el enlace de modelos.

En los próximos [tutorial](using-query-string-values-to-retrieve-data.md), usará un valor de cadena de consulta al seleccionar datos.

> [!div class="step-by-step"]
> [Anterior](sorting-paging-and-filtering-data.md)
> [Siguiente](using-query-string-values-to-retrieve-data.md)
