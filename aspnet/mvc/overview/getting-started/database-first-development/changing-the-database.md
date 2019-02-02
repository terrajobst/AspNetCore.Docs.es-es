---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutorial: Cambiar la base de datos para EF Database First con la aplicación de ASP.NET MVC'
description: En este tutorial se centra en realizar una actualización en la estructura de base de datos y propagar ese cambio a lo largo de la aplicación web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667640"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Tutorial: Cambiar la base de datos para EF Database First con la aplicación de ASP.NET MVC

Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

En este tutorial se centra en realizar una actualización en la estructura de base de datos y propagar ese cambio a lo largo de la aplicación web.

En este tutorial ha:

> [!div class="checklist"]
> * Agregar una columna
> * Agregue la propiedad a las vistas

## <a name="prerequisites"></a>Requisitos previos

* [Generación de vistas](generating-views.md)

## <a name="add-a-column"></a>Agregar una columna

Si actualiza la estructura de una tabla en la base de datos, deberá asegurarse de que el cambio se propaga al modelo de datos, vistas y controlador.

Para este tutorial, agregará una nueva columna a la tabla Student para registrar el apellido del alumno. Para agregar esta columna, abra el proyecto de base de datos y abra el archivo Student.sql. Mediante el diseñador o en el código de Transact-SQL, agregue una columna denominada **MiddleName** que es un nvarchar (50) y permite valores NULL.

Implementar este cambio en la base de datos local, inicie el proyecto de base de datos (o F5). El nuevo campo se agrega a la tabla. Si no se ve en el Explorador de objetos de SQL Server, haga clic en el botón Actualizar en el panel.

![Mostrar la nueva columna](changing-the-database/_static/image2.png)

La nueva columna existe en la tabla de base de datos, pero no existe actualmente en la clase de modelo de datos. Debe actualizar el modelo para incluir la nueva columna. En el **modelos** carpeta, abra el **ContosoModel.edmx** archivo para mostrar el diagrama del modelo. Tenga en cuenta que el modelo Student no contiene la propiedad MiddleName. Haga doble clic en cualquier lugar en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.

En el asistente, seleccione el **actualizar** pestaña y, a continuación, seleccione **tablas** > **dbo** > **estudiante**. Haga clic en **Finalizar**.

Cuando finalice el proceso de actualización, el diagrama de base de datos incluye el nuevo **MiddleName** propiedad. Guardar el **ContosoModel.edmx** archivo. Debe guardar este archivo para la nueva propiedad se propagará a la **Student.cs** clase. Ahora que ha actualizado la base de datos y el modelo.

Compile la solución.

## <a name="add-the-property-to-the-views"></a>Agregue la propiedad a las vistas

Lamentablemente, las vistas todavía no contienen la nueva propiedad. Para actualizar las vistas tiene dos opciones: puede generar volver a las vistas mediante la adición de una vez más scaffolding para la clase Student o puede agregar manualmente la nueva propiedad a las vistas existentes. En este tutorial, agregará el scaffolding de nuevo porque no ha realizado los cambios personalizados en las vistas que se genera automáticamente. Considere la posibilidad de agregar manualmente la propiedad cuando ha realizado cambios en las vistas y no desea perder los cambios.

Para asegurarse de que las vistas se vuelven a creadas, eliminar el **estudiantes** carpeta bajo **vistas**y elimine el **StudentsController**. A continuación, haga clic en el **controladores** carpeta y agregar el scaffolding para el **estudiante** modelo. El nombre nuevo, el controlador **StudentsController**. Seleccione **Agregar**.

Compile la solución de nuevo. Las vistas contienen ahora la propiedad MiddleName.

![Mostrar el segundo nombre](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Se ha agregado una columna
> * Agrega la propiedad a las vistas

Avance al siguiente tutorial para aprender a personalizar la vista para mostrar los detalles acerca de un registro de estudiante.
> [!div class="nextstepaction"]
> [Personalizar una vista](customizing-a-view.md)