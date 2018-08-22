---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Agregar capa de lógica empresarial a un proyecto que usa el enlace de modelos y formularios web forms | Microsoft Docs
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 2c54c0a635c662c8740aa7f9b6bd02cd71913e24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837995"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Agregar capa de lógica empresarial a un proyecto que usa el enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.
> 
> Este tutorial muestra cómo utilizar el enlace de modelos con una capa de lógica empresarial. Establecerá el miembro OnCallingDataMethods para especificar que un objeto distinto de la página actual se usa para llamar a los métodos de datos.
> 
> Este tutorial se basa en el proyecto creado en el [anteriores](retrieving-data.md) partes de la serie.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.


## <a name="what-youll-build"></a>¿Qué va a crear

Enlace de modelos le permite colocar el código de interacción de datos en el archivo de código subyacente para una página web o en una clase de lógica de negocio independientes. Los tutoriales anteriores se muestra cómo usar los archivos de código subyacente para el código de interacción de datos. Este enfoque funciona para sitios pequeños, pero que puede dar lugar a código repetición y mayor dificultad al mantener un sitio grande. También puede ser muy difícil de probar código que reside en los archivos de código subyacente porque no hay ninguna capa de abstracción de mediante programación.

Para centralizar el código de interacción de datos, puede crear una capa de lógica de negocios que contiene toda la lógica para interactuar con datos. A continuación, llame a la capa de lógica de negocios desde las páginas web. Este tutorial muestra cómo mover todo el código que ha escrito en los tutoriales anteriores en una capa de lógica empresarial y, a continuación, usar dicho código desde las páginas.

En este tutorial, necesitará:

1. Mover el código de archivos de código subyacente a una capa de lógica empresarial
2. Cambiar los controles enlazados a datos para llamar a los métodos en la capa de lógica de negocios

## <a name="create-business-logic-layer"></a>Crear la capa de lógica empresarial

Ahora, creará la clase que se llama desde las páginas web. Los métodos de esta clase tener un aspecto similares a los métodos que usó en los tutoriales anteriores e incluyen los atributos de proveedor de valor.

En primer lugar, agregue una nueva carpeta denominada **BLL**.

![Agregar carpeta](adding-business-logic-layer/_static/image1.png)

En la carpeta BLL, cree una nueva clase denominada **SchoolBL.cs**. Contendrá todas las operaciones de datos que residían originalmente en los archivos de código subyacente. Los métodos son prácticamente los mismos que los métodos en el archivo de código subyacente, pero incluyen algunos cambios.

El cambio más importante a tener en cuenta es que ya no se ejecuta el código desde una instancia de **página** clase. La clase de página contiene el **TryUpdateModel** método y el **ModelState** propiedad. Cuando este código se traslada a una capa de lógica empresarial, ya no tiene una instancia de la clase de página para llamar a estos miembros. Para solucionar este problema, debe agregar un **ModelMethodContext** parámetro a cualquier método que tiene acceso a TryUpdateModel o ModelState. Utilice este parámetro ModelMethodContext para llamar a TryUpdateModel o recuperar ModelState. No es necesario cambiar nada en la página web para tener en cuenta este nuevo parámetro.

Reemplace el código en SchoolBL.cs con el código siguiente.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Revise las páginas existentes para recuperar datos de la capa de lógica empresarial

Por último, convertirá las páginas Students.aspx, AddStudent.aspx y Courses.aspx del uso de las consultas en el archivo de código subyacente al uso de la capa de lógica empresarial.

En los archivos de código subyacente para estudiantes, AddStudent y cursos, elimine o comente los siguientes métodos de consulta:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Ahora no debería tener ningún código en el archivo de código subyacente que pertenecen a las operaciones de datos.

El **OnCallingDataMethods** controlador de eventos le permite especificar un objeto que se usará para los métodos de datos. En Students.aspx, agregue un valor para ese controlador de eventos y cambiar los nombres de los métodos de datos a los nombres de los métodos de la clase de lógica de negocios.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

En el archivo de código subyacente para Students.aspx, definir el controlador de eventos para el evento CallingDataMethods. En este controlador de eventos, especifique la clase de lógica de negocios para las operaciones de datos.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

En AddStudent.aspx, realizar modificaciones similares.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

En Courses.aspx, realizar modificaciones similares.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Ejecute la aplicación y observe que todas las páginas funcionan que tenían anteriormente. La lógica de validación también funciona correctamente.

## <a name="conclusion"></a>Conclusión

En este tutorial, estructurado volver a la aplicación para usar una capa de acceso a datos y la capa de lógica empresarial. Especifica que los controles de datos utilizan un objeto que no es la página actual para las operaciones de datos.

> [!div class="step-by-step"]
> [Anterior](using-query-string-values-to-retrieve-data.md)
