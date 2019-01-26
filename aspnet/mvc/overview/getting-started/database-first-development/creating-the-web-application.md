---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutorial: Crear la base de datos de la aplicación Web y los modelos de datos para EF primero con ASP.NET MVC'
description: En este artículo se centra en crear la aplicación web y generar los modelos de datos basados en las tablas de base de datos.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 095d355866c9ab8fba3759f3e05e2a521992f3d6
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889774"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Tutorial: Crear la base de datos de la aplicación Web y los modelos de datos para EF primero con ASP.NET MVC

 Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.

En este artículo se centra en crear la aplicación web y generar los modelos de datos basados en las tablas de base de datos.

En este tutorial ha:

> [!div class="checklist"]
> * Crear una aplicación web ASP.NET
> * Generar los modelos

## <a name="prerequisites"></a>Requisitos previos

* [Introducción a Entity Framework 6 Database First con MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Crear una aplicación web ASP.NET

En una nueva solución o en la misma solución que el proyecto de base de datos, cree un nuevo proyecto en Visual Studio y seleccione el **aplicación Web ASP.NET** plantilla. Denomine el proyecto **ContosoSite**.

![Crear proyecto](creating-the-web-application/_static/image1.png)

Haga clic en **Aceptar**.

En la ventana nuevo proyecto ASP.NET, seleccione la **MVC** plantilla. Puede borrar el **Host en la nube** opción por ahora, ya que se implementará la aplicación en la nube. Haga clic en **Aceptar** para crear la aplicación.

El proyecto se crea con los archivos predeterminados y las carpetas.

En este tutorial, usará Entity Framework 6. Puede comprobar la versión de Entity Framework en el proyecto a través de la ventana Administrar paquetes de NuGet. Si es necesario, actualice su versión de Entity Framework.

![Mostrar la versión](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generar los modelos

Ahora va a crear modelos de Entity Framework desde las tablas de base de datos. Estos modelos son clases que se va a utilizar para trabajar con los datos. Cada modelo refleja una tabla en la base de datos y contiene propiedades que corresponden a las columnas de la tabla.

Haga clic en el **modelos** carpeta y seleccione **agregar** y **nuevo elemento**.

En la ventana Agregar nuevo elemento, seleccione **datos** en el panel izquierdo y **ADO.NET Entity Data Model** entre las opciones en el panel central. Asigne al nuevo archivo de modelo **ContosoModel**.

Haga clic en **Agregar**.

En el Asistente de Entity Data Model, seleccione **EF Designer de base de datos**.

Haga clic en **Siguiente**.

Si tiene conexiones de base de datos definidas dentro de su entorno de desarrollo, es posible que vea una de estas conexiones previamente seleccionadas. Sin embargo, desea crear una nueva conexión a la base de datos que creó en la primera parte de este tutorial. Haga clic en el **nueva conexión** botón.

En la ventana Propiedades de conexión, proporcione el nombre del servidor local donde se creó la base de datos (en este caso **(localdb) \Projects13**). Después de proporcionar el nombre del servidor, seleccione el ContosoUniversityData de las bases de datos disponibles.

![establecer propiedades de conexión](creating-the-web-application/_static/image8.png)

Haga clic en **Aceptar**.

Ahora se muestran las propiedades de conexión correcto. Puede usar el nombre predeterminado para la conexión en el archivo Web.Config.

Haga clic en **Siguiente**.

Seleccione la versión más reciente de Entity Framework.

Haga clic en **Siguiente**.

Seleccione **tablas** para generar modelos para las tres tablas.

Haga clic en **Finalizar**.

Si recibes una advertencia de seguridad, seleccione **Aceptar** para continuar la ejecución de la plantilla.

Los modelos se generan a partir de las tablas de base de datos y se muestra un diagrama que muestra las propiedades y relaciones entre las tablas.

![diagrama del modelo](creating-the-web-application/_static/image11.png)

La carpeta Models ahora incluye muchos nuevos archivos relacionados con los modelos que se generaron a partir de la base de datos.

El **ContosoModel.Context.cs** archivo contiene una clase que deriva el **DbContext** clase y proporciona una propiedad para cada clase de modelo que corresponde a una tabla de base de datos. El **Course.cs**, **Enrollment.cs**, y **Student.cs** archivos contienen las clases de modelo que representan las tablas de bases de datos. La clase de contexto y las clases del modelo se usará cuando se trabaja con la técnica scaffolding.

Antes de continuar con este tutorial, compile el proyecto. En la siguiente sección, generará código basado en los modelos de datos, pero esa sección no funcionará si no se ha compilado el proyecto.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Crea una aplicación web ASP.NET
> * Genera los modelos

Avance al siguiente artículo para obtener información sobre cómo crear generar código basado en los modelos de datos.
> [!div class="nextstepaction"]
> [Generación de vistas](generating-views.md)