---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Crear una base de datos | Documentos de Microsoft
author: shanselman
description: "Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Crear una aplicación web simple que lee y escribe desde una base de datos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 22f1042555d7cdd0ca8d75f1cbde65fbb65d81cc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="creating-a-database"></a>Crear una base de datos
====================
por [Scott Hanselman](https://github.com/shanselman)

> Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos. Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección, vamos a crear una nueva base de datos SQL Express a objetos que vamos a usar para almacenar y recuperar los datos de la película. En el IDE de Visual Web Developer, seleccione Ver | Explorador de servidores. Haga clic con el botón secundario en las conexiones de datos y haga clic en Agregar conexión...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

En el cuadro de diálogo Elegir origen de datos, seleccione Microsoft SQL Server y seleccione continuar.

![](getting-started-with-mvc-part4/_static/image2.png)

En el cuadro de diálogo Agregar conexión, escriba ". \SQLEXPRESS" para el nombre del servidor y escriba "Películas" como el nombre de la nueva base de datos.

[![Agregar cuadro de diálogo de conexión](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Haga clic en Aceptar y se le preguntará si desea crear la base de datos. Seleccione Sí.

[![¿Crear películas?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Ahora ya tiene una base de datos vacía en el Explorador de servidores.

![Agregar nueva tabla](getting-started-with-mvc-part4/_static/image7.png)

Haga clic con el botón secundario en las tablas y haga clic en Agregar tabla. Aparecerá el Diseñador de tablas. Agregar columnas para Id., título, ReleaseDate, género y precio. Haga clic con el botón secundario en la columna Id. y haga clic en Establecer clave principal. Aquí es qué áreas de mi diseño aspecto.

[![Editor de la tabla de base de datos](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Además, seleccione la columna Id. y en Propiedades de columna a continuación, cambie "Especificación de identidad" en "Sí".

[![IsIdentity - propiedades de columna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Si tienes que hacerlo, haga clic en el icono Guardar en la barra de herramientas o seleccione archivo | Guardar en el menú y el nombre de la tabla "**película**" (simples). Tenemos una base de datos y una tabla.

[![Elegir nombre](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Vuelva al explorador de servidores y haga clic en la tabla de la película, y seleccione "Mostrar datos de tabla". Escriba unos películas para que nuestra base de datos tiene algunos datos.

[![Edición de la tabla en la base de datos](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Creación de un modelo

Ahora, vuelva al explorador de soluciones en el lado derecho del IDE y haga doble clic en la carpeta Models y seleccione Agregar | Nuevo elemento.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Vamos a crear un modelo de entidades de la nueva base de datos. Esto agregará un conjunto de clases al proyecto que hace más fácil para que podamos consultar y manipular los datos dentro de nuestra base de datos. Seleccione el nodo de datos en el lado izquierdo del cuadro de diálogo y, a continuación, seleccione la plantilla de elemento de ADO.NET Entity Data Model. Asígnele el nombre Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Haga clic en el botón "Agregar". A continuación, se iniciará al "Entity Data Model Wizard".

En el nuevo cuadro de diálogo que aparece, seleccione Generar desde la base de datos. Puesto que solo hemos realizado una base de datos, solo necesitaremos y explíquenos la nueva base de datos y su tabla en Entity Framework. Haga clic en siguiente para guardar la conexión de base de datos de configuración de la aplicación web. Ahora, compruebe las tablas y la película y haga clic en finalización.

[![Asistente de Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Ahora podemos ver nuestra nueva tabla de película en el Diseñador de Entity Framework y obtener acceso a él desde el código.

[![Películas - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

En la superficie de diseño puede ver una clase de "Película". Esta clase se asigna a la tabla "Película" en nuestra base de datos, y cada propiedad en él se asigna a una columna con la tabla. Cada instancia de una clase de "Película" corresponderá a una fila dentro de la tabla "Película".

Si no le gusta el nombre predeterminado y asignación convenciones usadas por Entity Framework, puede utilizar el Diseñador de Entity Framework para cambiar o personalizarlos. Para esta aplicación podrá usar los valores predeterminados y solo hay que guardar el archivo como-es.

Ahora, vamos a trabajar con algunos datos reales.

>[!div class="step-by-step"]
[Anterior](getting-started-with-mvc-part3.md)
[Siguiente](getting-started-with-mvc-part5.md)
