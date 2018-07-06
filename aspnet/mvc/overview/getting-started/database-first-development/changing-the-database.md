---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Database First con MVC de ASP.NET: cambiar la base de datos | Microsoft Docs'
author: tfitzmac
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 0176274694426a527ed0862b5138919d4cf5c319
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840947"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>EF Database First con MVC de ASP.NET: cambiar la base de datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en realizar una actualización en la estructura de base de datos y propagar ese cambio a lo largo de la aplicación web.


## <a name="add-a-column"></a>Agregar una columna

Si actualiza la estructura de una tabla en la base de datos, deberá asegurarse de que el cambio se propaga al modelo de datos, vistas y controlador.

Para este tutorial, agregará una nueva columna a la tabla Student para registrar el apellido del alumno. Para agregar esta columna, abra el proyecto de base de datos y abra el archivo Student.sql. Mediante el diseñador o en el código de Transact-SQL, agregue una columna denominada **MiddleName** que es un nvarchar (50) y permite valores NULL.

![agregar el segundo nombre](changing-the-database/_static/image1.png)

Implementar este cambio en la base de datos local, inicie el proyecto de base de datos (o F5). El nuevo campo se agrega a la tabla. Si no se ve en el Explorador de objetos de SQL Server, haga clic en el botón Actualizar en el panel.

![Mostrar la nueva columna](changing-the-database/_static/image2.png)

La nueva columna existe en la tabla de base de datos, pero no existe actualmente en la clase de modelo de datos. Debe actualizar el modelo para incluir la nueva columna. En el **modelos** carpeta, abra el **ContosoModel.edmx** archivo para mostrar el diagrama del modelo. Tenga en cuenta que el modelo Student no contiene la propiedad MiddleName. Haga doble clic en cualquier lugar en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.

![actualización del modelo](changing-the-database/_static/image3.png)

En el asistente, seleccione el **actualizar** ficha y el **estudiante** tabla.

![Asistente para actualizar](changing-the-database/_static/image4.png)

Haga clic en **Finalizar**.

Cuando finalice el proceso de actualización, el diagrama de base de datos incluye el nuevo **MiddleName** propiedad. Guardar el **ContosoModel.edmx** archivo. Debe guardar este archivo para la nueva propiedad se propagará a la **Student.cs** clase. Ahora que ha actualizado la base de datos y el modelo.

Compile la solución.

Lamentablemente, las vistas todavía no contienen la nueva propiedad. Para actualizar las vistas tiene dos opciones: puede generar volver a las vistas mediante la adición de una vez más scaffolding para la clase Student o puede agregar manualmente la nueva propiedad a las vistas existentes. En este tutorial, agregará el scaffolding de nuevo porque no ha realizado los cambios personalizados en las vistas que se genera automáticamente. Considere la posibilidad de agregar manualmente la propiedad cuando ha realizado cambios en las vistas y no desea perder los cambios.

Para asegurarse de que las vistas se vuelven a creadas, eliminar el **estudiantes** carpeta bajo **vistas**y elimine el **StudentsController**. A continuación, haga clic en el **controladores** carpeta y agregar el scaffolding para el **estudiante** modelo. El nombre nuevo, el controlador **StudentsController**. Seleccione **Aceptar**.

Las vistas contienen ahora la propiedad MiddleName.

![Mostrar el segundo nombre](changing-the-database/_static/image5.png)

En la siguiente sección, agregará código para personalizar la vista para mostrar los detalles acerca de un registro de estudiante.

> [!div class="step-by-step"]
> [Anterior](generating-views.md)
> [Siguiente](customizing-a-view.md)
