---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Creación de una base de datos | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362361"
---
<a name="creating-a-database"></a>Creación de una base de datos
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe desde una base de datos. Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección vamos a crear una nueva base de datos SQL Express a objetos que vamos a usar para almacenar y recuperar los datos de la película. En el IDE de Visual Web Developer, seleccione Ver | Explorador de servidores. Haga clic con el botón derecho en conexiones de datos y haga clic en Agregar conexión...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

En el cuadro de diálogo Elegir origen de datos, seleccione Microsoft SQL Server y seleccione continuar.

![](getting-started-with-mvc-part4/_static/image2.png)

En el cuadro de diálogo Agregar conexión, escriba ". \SQLEXPRESS" para el nombre del servidor y escriba "Movies" como el nombre de la base de datos.

[![Agregar cuadro de diálogo de conexión](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Haga clic en Aceptar y se le preguntará si desea crear la base de datos. Seleccione Sí.

[![¿Crear películas?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Ahora ya tiene una base de datos vacía en el Explorador de servidores.

![Agregar nueva tabla](getting-started-with-mvc-part4/_static/image7.png)

Haga clic con el botón derecho en las tablas y haga clic en Agregar tabla. Aparecerá el Diseñador de tablas. Agregar columnas para Id., Title, ReleaseDate, género y precio. Haga clic con el botón derecho en la columna de identificador y haga clic en Establecer clave principal. Aquí es lo que mis áreas de diseño similar a.

[![Editor de la tabla de base de datos](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Además, seleccione la columna Id. y en las propiedades de columna siguiente, cambie "Especificación de identidad" en "Sí".

[![IsIdentity - propiedades de columna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Cuando haya entendido listo, haga clic en el icono Guardar en la barra de herramientas o seleccione archivo | Guardar en el menú y el nombre de la tabla "**película**" (simples). Tenemos una base de datos y una tabla.

[![Elegir nombre](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Vuelva al explorador de servidores y haga clic en la tabla Movie, seleccione "Mostrar datos de tabla". Escriba algunas películas para que nuestra base de datos tiene algunos datos.

[![Edición de tablas de base de datos](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Creación de un modelo

Ahora, vuelva al explorador de soluciones en el lado derecho del IDE y haga doble clic en la carpeta Models y seleccione Agregar | Nuevo elemento.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Vamos a crear un modelo de entidades de nuestra nueva base de datos. Esto agregará un conjunto de clases al proyecto que resulta muy fácil para nosotros consultar y manipular los datos dentro de nuestra base de datos. Seleccione el nodo de datos en el lado izquierdo del cuadro de diálogo y, a continuación, seleccione la plantilla de elemento de ADO.NET Entity Data Model. Asígnele el nombre Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Haga clic en el botón "Agregar". A continuación, se iniciará al "modelo de Asistente para Entity Data".

En el nuevo cuadro de diálogo que aparece, seleccione Generar desde la base de datos. Puesto que solo hemos realizado una base de datos, solo se necesitará indicar a Entity Framework sobre nuestra nueva base de datos y su tabla. Haga clic en siguiente para guardar nuestra conexión de base de datos en la configuración de la aplicación web. Ahora, compruebe las tablas y la película y haga clic en finalización.

[![Asistente para Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Ahora podemos ver nuestra nueva tabla de películas en Entity Framework Designer y acceder a él desde el código.

[![Películas - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

En la superficie de diseño puede ver una clase "llamada Movie". Esta clase se asigna a la tabla "Movie" en nuestra base de datos, y cada propiedad en él se asigna a una columna con la tabla. Cada instancia de una clase "Movie" corresponderá a una fila dentro de la tabla "Movie".

Si no le gusta la nomenclatura predeterminada y la asignación convenciones empleadas por Entity Framework, puede utilizar el Diseñador de Entity Framework para cambiar o personalizarlos. Para esta aplicación podrá usar los valores predeterminados y guarde el archivo como de solo-es.

Ahora, vamos a trabajar con algunos datos reales.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part3.md)
> [Siguiente](getting-started-with-mvc-part5.md)
