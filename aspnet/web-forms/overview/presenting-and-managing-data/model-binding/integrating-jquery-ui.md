---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: "Integración de JQuery UI Datepicker con el enlace de modelos y formularios web forms | Documentos de Microsoft"
author: tfitzmac
description: "Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más directa-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: da3c8f347a709a4c9a47fd0ecce5201d9b0cd1b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integración de JQuery UI Datepicker con el enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más sencilla de trabajar con datos de objetos de origen (por ejemplo, ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se mueve al conceptos más avanzados en tutoriales posteriores.
> 
> Este tutorial muestra cómo agregar la JQuery UI [widget Datepicker](http://jqueryui.com/datepicker/) a un formulario Web Forms y el modelo de uso de enlace para actualizar la base de datos con el valor seleccionado.
> 
> Este tutorial se basa en el proyecto creado en el [primer](retrieving-data.md) y [segundo](updating-deleting-and-creating-data.md) partes de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código que se puede descargar funciona con Visual Studio 2012 o Visual Studio 2013. Utiliza la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En este tutorial, deberá:

1. Agregar una propiedad al modelo para registrar la fecha de inscripción de student
2. Permitir al usuario seleccionar la fecha de inscripción mediante el widget Datepicker de JQuery UI
3. Exigir reglas de validación para la fecha de inscripción

El widget Datepicker de JQuery UI permite a los usuarios seleccionar fácilmente una fecha de calendario que aparece cuando el usuario interactúa con el campo. Uso de este widget puede ser más conveniente para los usuarios que escribir manualmente una fecha. Integrar el widget Datepicker en una página que usa el enlace de modelos para las operaciones de datos requiere sólo una pequeña cantidad de trabajo adicional.

## <a name="add-a-new-property-to-the-model"></a>Agregar una nueva propiedad al modelo

En primer lugar, agregará un **Datetime** propiedad a los estudiantes de modelo y migrar ese cambio a la base de datos. Abra **UniversityModels.cs**y agregue el código resaltado en el modelo de estudiante.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

El **RangeAttribute** se incluye para exigir reglas de validación para la propiedad. Para este tutorial, asumiremos que Contoso Universidad se basa en el 1 de enero de 2013 y, por tanto, las fechas de inscripción anteriores no son válidas.

En la ventana de administración de paquetes, agregue una migración, ejecute el comando **AddEnrollmentDate de migración agregar**. Tenga en cuenta que el código de la migración agrega la nueva columna de fecha y hora a la tabla de estudiantes. Para que coincida con el valor especificado en el RangeAttribute, agregue un valor predeterminado para la nueva columna, tal y como se muestra en el código que aparece resaltado a continuación.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Guarde el cambio en el archivo de migración.

No es necesario propagar los datos de nuevo. Por lo tanto, abra **archivo Configuration.cs que** en la carpeta Migrations y quite o marque como comentario el código en el **inicialización** método. Guarde y cierre el archivo.

Ahora, ejecute el comando **Actualizar base de datos**. Observe que ahora existe la columna en la base de datos y todos los registros existentes tienen el valor predeterminado para EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Agregar controles dinámicos para la fecha de inscripción

Ahora va a agregar controles para mostrar y editar la fecha de inscripción. En este momento, se edita el valor a través de un cuadro de texto. Más adelante en el tutorial, cambiará el cuadro de texto para el widget de JQuery Datepicker.

En primer lugar, es importante tener en cuenta que no es necesario realizar ningún cambio a la **AddStudent.aspx** archivo. El control DynamicEntity automáticamente representará la nueva propiedad.

Abra **Students.aspx**y agregue el código resaltado siguiente.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Ejecute la aplicación y observe que puede establecer el valor de la fecha de inscripción, escriba una fecha. Cuando se agrega un nuevo alumno:

![Configurar fecha](integrating-jquery-ui/_static/image1.png)

O bien, editar un valor existente:

![Editar fecha](integrating-jquery-ui/_static/image2.png)

Escriba funciona de la fecha, pero podría no ser la experiencia del cliente que se va a proporcionar. En la siguiente sección le permitirá seleccionar una fecha mediante un calendario.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Instalar el paquete de NuGet para trabajar con JQuery UI

El **interfaz de usuario de zumo** paquete NuGet permite la integración sencilla de los widgets de JQuery UI en la aplicación web. Para usar este paquete, instalarlo a través de NuGet.

![agregar la interfaz de usuario de zumo](integrating-jquery-ui/_static/image3.png)

La versión de interfaz de usuario de zumo que instale puede entrar en conflicto con la versión de JQuery en la aplicación. Antes de continuar con este tutorial, vuelva a ejecutar la aplicación. Si se produce un error de JavaScript, debe reconciliar la versión de JQuery. Puede agregar la versión esperada de JQuery a la carpeta de Scripts (versión 1.8.2 en el momento de escribir este tutorial) o en Site.master, especifique la ruta de acceso al archivo de JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalizar la plantilla de fecha y hora para que incluya el widget Datepicker

El widget Datepicker agregará a la plantilla de datos dinámicos para editar un valor de fecha y hora. Al agregar el widget a la plantilla, automáticamente se representa en forma de agregar un nuevo alumno y en la vista de cuadrícula para estudiantes de edición. Abra **DateTime\_Edit.ascx**y agregue el código resaltado siguiente.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

En el archivo de código subyacente, establecerá las fechas mínimas y máxima para el selector de fechas. Al establecer estos valores, se impide que los usuarios navegar a fechas no válidas. Recuperará los valores mínimos y máximo de la **RangeAttribute** en la propiedad de fecha y hora, si se proporciona uno. Abra **DateTime\_Edit.ascx.cs**y agregue el código que aparece resaltado a la página\_Load (método).

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Ejecutar la aplicación web y navegue a la página AddStudent. Proporcione valores para los campos y tenga en cuenta que al hacer clic en el cuadro de texto para la fecha de inscripción, se muestra el calendario.

![Selector de fecha](integrating-jquery-ui/_static/image4.png)

Seleccionar una fecha y haga clic en **insertar**. El RangeAttribute exige la validación en el servidor. Al establecer la propiedad minDate en el selector de fechas, también se aplican validación en el cliente. El calendario no permite que el usuario navegue a una fecha anterior al valor de minDate.

Al editar un registro en la vista de cuadrícula, también se muestra el calendario.

![DatePicker en GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, aprendió a incorporar un widget de JQuery en un formulario web Forms que utiliza el enlace de modelos.

En la siguiente [tutorial](using-query-string-values-to-retrieve-data.md), usará un valor de cadena de consulta al seleccionar los datos.

>[!div class="step-by-step"]
[Anterior](sorting-paging-and-filtering-data.md)
[Siguiente](using-query-string-values-to-retrieve-data.md)
