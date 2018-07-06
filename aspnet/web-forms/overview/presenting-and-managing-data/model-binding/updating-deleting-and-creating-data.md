---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Actualizar, eliminar y crear datos con enlace de modelos y formularios web forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 1cf9873db177b67927b579def1eedd08e3e9a762
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821637"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Actualizar, eliminar y crear datos con enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.
> 
> Este tutorial muestra cómo crear, actualizar y eliminar datos con enlace de modelos. Establecerá las propiedades siguientes:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Estas propiedades reciben el nombre del método que controla la operación correspondiente. Dentro de ese método, debe proporcionar la lógica para interactuar con los datos.
> 
> Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>¿Qué va a crear

En este tutorial, necesitará:

1. Agregar plantillas de datos dinámicos
2. Habilitar la actualización y eliminación de datos a través de los métodos de enlace de modelos
3. Aplicar reglas de validación de datos: habilitación de la creación de un nuevo registro en la base de datos

## <a name="add-dynamic-data-templates"></a>Agregar plantillas de datos dinámicos

Para ofrecer la mejor experiencia de usuario y reducir la repetición del código, deberá usar plantillas de datos dinámicos. Puede integrar fácilmente las plantillas precompiladas datos dinámicos en el sitio existente al instalar un paquete de NuGet.

Desde el **administrar paquetes de NuGet**, instale el **DynamicDataTemplatesCS**.

![plantillas de datos dinámicos](updating-deleting-and-creating-data/_static/image1.png)

Tenga en cuenta que ahora el proyecto incluye una carpeta denominada **DynamicData**. En esa carpeta, encontrará las plantillas que se aplican automáticamente a los controles dinámicos en los formularios web forms.

![carpeta de datos dinámicos](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Habilitar actualización y eliminación

Permitir a los usuarios actualizar y eliminar registros en la base de datos es muy similar al proceso de recuperación de datos. En el **UpdateMethod** y **DeleteMethod** propiedades, especifique los nombres de los métodos que realizan esas operaciones. Con un control GridView, también puede especificar la generación automática de editar y eliminar botones. El código resaltado siguiente muestra las adiciones al código de GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

En el archivo de código subyacente, agregue una mediante declaración para **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

A continuación, agregue la siguiente actualización y elimina métodos.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

El **TryUpdateModel** método aplica los valores enlazados a datos coincidentes del formulario web Forms al elemento de datos. El elemento de datos se recupera en función del valor del parámetro del identificador.

## <a name="enforce-validation-requirements"></a>Exigir los requisitos de validación

Los atributos de validación que aplican a las propiedades FirstName, LastName y año en la clase Student se aplican automáticamente al actualizar los datos. Los controles de DynamicField agregan validadores de cliente y servidor según los atributos de validación. Las propiedades FirstName y LastName son necesarios. FirstName no puede superar los 20 caracteres de longitud y LastName no puede superar los 40 caracteres. Año debe ser un valor válido para la enumeración AcademicYear.

Si el usuario infringe uno de los requisitos de validación, no continúe la actualización. Para ver el mensaje de error, agregue un control ValidationSummary por encima del control GridView. Para mostrar los errores de validación del enlace de modelos, establezca el **ShowModelStateErrors** propiedad establecida en **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Ejecute la aplicación web y actualizar y eliminar cualquiera de los registros.

![actualizar datos](updating-deleting-and-creating-data/_static/image3.png)

Tenga en cuenta que, cuando en el modo de edición, el valor de la propiedad año automáticamente se representa como una lista desplegable. La propiedad de año es un valor de enumeración y la plantilla de datos dinámicos para un valor de enumeración especifica una lista desplegable de edición. Puede encontrar esa plantilla abriendo el **enumeración\_Edit.ascx** de archivos en el **DynamicData**/**FieldTemplates** carpeta.

Si proporciona los valores válidos, la actualización se complete correctamente. Si se infringe uno de los requisitos de validación, la actualización no continúa y se muestra un mensaje de error encima de la cuadrícula.

![mensaje de error](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Agregar nuevos registros

El control GridView no incluye el **InsertMethod** propiedad y, por tanto, no se puede usar para agregar un nuevo registro con el enlace de modelos. Puede encontrar la propiedad InsertMethod en el **FormView**, **DetailsView**, o **ListView** controles. En este tutorial, usará un control FormView para agregar un nuevo registro.

En primer lugar, agregue un vínculo a la nueva página que se va a crear para agregar un nuevo registro. Sobre el control ValidationSummary, agregue:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

El nuevo vínculo aparecerá en la parte superior del contenido de la página Students.

![nuevo vínculo](updating-deleting-and-creating-data/_static/image5.png)

A continuación, agregue un nuevo formulario web con una página maestra y asígnele el nombre **AddStudent**. Seleccione Site.Master como la página maestra.

Se representarán los campos para agregar un nuevo alumno mediante un **DynamicEntity** control. El control de DynamicEntity representa esa propiedades editables de la clase especificada en la propiedad de tipo de elemento. La propiedad StudentID se marcó con el **[scaffoldcolumn (false)]** atributo para que no se represente. En el marcador de posición MainContent de la página AddStudent, agregue el código siguiente.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

En el archivo de código subyacente (AddStudent.aspx.cs), agregue un **mediante** instrucción para el **ContosoUniversityModelBinding.Models** espacio de nombres.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

A continuación, agregue los métodos siguientes para especificar cómo insertar un nuevo registro y un controlador de eventos para el botón Cancelar.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Guarde todos los cambios.

Ejecutar la aplicación web y crear un nuevo alumno.

![Agregar nuevo alumno](updating-deleting-and-creating-data/_static/image6.png)

Haga clic en **insertar** y observe el alumno nuevo se ha creado.

![pantalla nuevo alumno](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, ha habilitado actualizar, eliminar y crear datos. Se garantiza que se aplican las reglas de validación al interactuar con los datos.

En los próximos [tutorial](sorting-paging-and-filtering-data.md) en esta serie, permitirá ordenar, paginar y filtrar datos.

> [!div class="step-by-step"]
> [Anterior](retrieving-data.md)
> [Siguiente](sorting-paging-and-filtering-data.md)
