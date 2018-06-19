---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Agregar capa de lógica empresarial a un proyecto que usa el enlace de modelos y formularios web forms | Documentos de Microsoft
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más directa-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892756"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Agregar capa de lógica empresarial a un proyecto que usa el enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más sencilla de trabajar con datos de objetos de origen (por ejemplo, ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se mueve al conceptos más avanzados en tutoriales posteriores.
> 
> Este tutorial muestra cómo utilizar el enlace de modelos a una capa de lógica de negocios. Establecerá el miembro OnCallingDataMethods para especificar que un objeto distinto de la página actual se usa para llamar a los métodos de datos.
> 
> Este tutorial se basa en el proyecto creado en el [anteriores](retrieving-data.md) partes de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código que se puede descargar funciona con Visual Studio 2012 o Visual Studio 2013. Utiliza la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>Lo que vamos a compilar

Enlace de modelos le permite colocar el código de interacción de datos en el archivo de código subyacente para una página web o en una clase de lógica de negocios independientes. Los tutoriales anteriores han demostrado cómo usar los archivos de código subyacente para el código de interacción de datos. Este enfoque funciona para sitios pequeños, pero se pueden producir en código repetición y mayor dificultad al mantener un sitio grande. También puede ser muy difícil probar mediante programación el código que se encuentra en archivos de código subyacente porque no hay ninguna capa de abstracción.

Para centralizar el código de interacción de datos, puede crear una capa de lógica de negocios que contiene toda la lógica para interactuar con datos. A continuación, llamar a la capa de lógica de negocios desde las páginas web. Este tutorial muestra cómo mover todo el código que ha escrito en los tutoriales anteriores en una capa de lógica de negocios y, a continuación, usar ese código desde las páginas.

En este tutorial, deberá:

1. Mueva el código de archivos de código subyacente a una capa de lógica de negocios
2. Cambiar los controles enlazados a datos para llamar a los métodos en la capa de lógica de negocios

## <a name="create-business-logic-layer"></a>Crear la capa de lógica de negocios

Ahora, se creará la clase que se llama desde las páginas web. Los métodos de esta clase un aspecto similares a los métodos utilizados en los tutoriales anteriores e incluyen los atributos de proveedor de valor.

En primer lugar, agregue una nueva carpeta denominada **BLL**.

![Agregar carpeta](adding-business-logic-layer/_static/image1.png)

En la carpeta BLL, cree una nueva clase denominada **SchoolBL.cs**. Contendrá todas las operaciones de datos que residían originalmente en archivos de código subyacente. Los métodos son prácticamente los mismos que los métodos en el archivo de código subyacente, pero incluyen algunos cambios.

El cambio más importante tener en cuenta es que ya no se está ejecutando el código desde dentro de una instancia de **página** clase. La clase de página contiene el **TryUpdateModel** método y **ModelState** propiedad. Cuando este código se traslada a una capa de lógica de negocios, ya no tiene una instancia de la clase de página para llamar a estos miembros. Para solucionar este problema, debe agregar una **ModelMethodContext** parámetro a cualquier método que tiene acceso a TryUpdateModel o ModelState. Este parámetro ModelMethodContext se usa para llamar a TryUpdateModel o recuperar ModelState. No es necesario cambiar nada en la página web para tener en cuenta este nuevo parámetro.

Reemplace el código en SchoolBL.cs con el código siguiente.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Revise las páginas existentes para recuperar datos de capa de lógica de negocios

Por último, convertirá las páginas Students.aspx, AddStudent.aspx y Courses.aspx del uso de consultas en el archivo de código subyacente al uso de la capa de lógica de negocios.

En los archivos de código subyacente para estudiantes, AddStudent y cursos, eliminar o marque como comentario los métodos de consulta siguiente:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Ahora no debería tener ningún código en el archivo de código subyacente que pertenece a las operaciones de datos.

El **OnCallingDataMethods** controlador de eventos le permite especificar un objeto que se usará para los métodos de datos. En Students.aspx, agregue un valor para ese controlador de eventos y cambiar los nombres de los métodos de datos a los nombres de los métodos de la clase de la lógica de negocios.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

En el archivo de código subyacente para Students.aspx, definir el controlador de eventos para el evento CallingDataMethods. En este controlador de eventos, se especifica la clase de la lógica de negocios para las operaciones de datos.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

En AddStudent.aspx, realizar modificaciones similares.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

En Courses.aspx, realizar modificaciones similares.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Ejecute la aplicación y observe que todas las páginas funcionen como que tenían anteriormente. La lógica de validación también funciona correctamente.

## <a name="conclusion"></a>Conclusión

En este tutorial, volver a estructurados de su aplicación para que utilice una capa de acceso a datos y la capa de lógica empresarial. Especifica que los controles de datos utilizan un objeto que no es la página actual para las operaciones de datos.

> [!div class="step-by-step"]
> [Anterior](using-query-string-values-to-retrieve-data.md)
