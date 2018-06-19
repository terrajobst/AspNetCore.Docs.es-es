---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Base de datos EF primero con ASP.NET MVC: cambiar la base de datos | Documentos de Microsoft'
author: tfitzmac
description: Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 63ee8768a43dbdac80922e3adbedd3378c10da73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879327"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>Base de datos EF primero con ASP.NET MVC: cambiar la base de datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en realizar una actualización en la estructura de base de datos y la propagación de dicho cambio a lo largo de la aplicación web.


## <a name="add-a-column"></a>Agregar una columna

Si actualiza la estructura de una tabla en la base de datos, debe asegurarse de que el cambio se propaga al modelo de datos, vistas y controlador.

Para este tutorial, agregará una nueva columna a la tabla de estudiantes para registrar el nombre completo de los estudiantes. Para agregar esta columna, abra el proyecto de base de datos y abra el archivo Student.sql. Mediante el diseñador o en el código de T-SQL, agregar una columna denominada **MiddleName** que es un nvarchar (50) y permite valores NULL.

![agregar el segundo apellido](changing-the-database/_static/image1.png)

Implemente este cambio en la base de datos local, inicie el proyecto de base de datos (o F5). El nuevo campo se agrega a la tabla. Si no se ve en el Explorador de objetos de SQL Server, haga clic en el botón Actualizar en el panel.

![Mostrar la nueva columna](changing-the-database/_static/image2.png)

La nueva columna existe en la tabla de base de datos, pero no existe actualmente en la clase de modelo de datos. Debe actualizar el modelo para incluir la nueva columna. En el **modelos** carpeta, abra el **ContosoModel.edmx** archivo para mostrar el diagrama del modelo. Tenga en cuenta que el modelo de estudiantes no contiene la propiedad MiddleName. Haga doble clic en cualquier lugar en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.

![modelo de actualización](changing-the-database/_static/image3.png)

En el asistente, seleccione la **actualizar** ficha y **estudiante** tabla.

![Asistente para actualización](changing-the-database/_static/image4.png)

Haga clic en **Finalizar**.

Cuando finalice el proceso de actualización, el diagrama de base de datos incluye la nueva **MiddleName** propiedad. Guardar el **ContosoModel.edmx** archivo. Debe guardar este archivo para la nueva propiedad se propagará a la **Student.cs** clase. Ahora que ha actualizado la base de datos y el modelo.

Compile la solución.

Desafortunadamente, las vistas aún no contienen la nueva propiedad. Para actualizar las vistas tiene dos opciones: puede generar volver a las vistas mediante la adición de una vez más scaffolding para la clase Student o puede agregar manualmente la nueva propiedad a las vistas existentes. En este tutorial, agregará el scaffolding nuevo porque no se ha realizado algún cambio personalizada a las vistas generados automáticamente. Considere la posibilidad de agregar manualmente la propiedad cuando ha realizado cambios en las vistas y no desea perder los cambios.

Para asegurarse de que las vistas se vuelven a creadas, eliminar el **estudiantes** carpeta bajo **vistas**y elimine la **StudentsController**. A continuación, haga clic en el **controladores** carpeta y agregar el scaffolding para el **estudiante** modelo. El nombre nuevo, el controlador **StudentsController**. Seleccione **Aceptar**.

Las vistas contienen ahora la propiedad MiddleName.

![Mostrar el segundo apellido](changing-the-database/_static/image5.png)

En la siguiente sección, agregará código para personalizar la vista para mostrar detalles acerca de un registro de estudiante.

> [!div class="step-by-step"]
> [Anterior](generating-views.md)
> [Siguiente](customizing-a-view.md)
