---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Database First con MVC de ASP.NET: creación de la aplicación Web y los modelos de datos | Microsoft Docs'
author: tfitzmac
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 343131d45ed0b2442f1b0b557c5b63f3877e5d0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838481"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>EF Database First con MVC de ASP.NET: creación de la aplicación Web y los modelos de datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en crear la aplicación web y generar los modelos de datos basados en las tablas de base de datos.


## <a name="create-a-new-aspnet-web-application"></a>Cree una nueva aplicación Web de ASP.NET

En una nueva solución o en la misma solución que el proyecto de base de datos, cree un nuevo proyecto en Visual Studio y seleccione el **aplicación Web ASP.NET** plantilla. Denomine el proyecto **ContosoSite**.

![Crear proyecto](creating-the-web-application/_static/image1.png)

Haga clic en **Aceptar**.

En la ventana nuevo proyecto ASP.NET, seleccione la **MVC** plantilla. Puede borrar el **Host en la nube** opción por ahora, ya que se implementará la aplicación en la nube. Haga clic en **Aceptar** para crear la aplicación.

![Seleccione la plantilla mvc](creating-the-web-application/_static/image2.png)

El proyecto se crea con los archivos predeterminados y las carpetas.

En este tutorial, usará Entity Framework 6. Puede comprobar la versión de Entity Framework en el proyecto a través de la ventana Administrar paquetes de NuGet. Si es necesario, actualice su versión de Entity Framework.

![Mostrar la versión](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generar los modelos

Ahora va a crear modelos de Entity Framework desde las tablas de base de datos. Estos modelos son clases que se va a utilizar para trabajar con los datos. Cada modelo refleja una tabla en la base de datos y contiene propiedades que corresponden a las columnas de la tabla.

Haga clic en el **modelos** carpeta y seleccione **agregar** y **nuevo elemento**.

![Agregar nuevo elemento](creating-the-web-application/_static/image4.png)

En la ventana Agregar nuevo elemento, seleccione **datos** en el panel izquierdo y **ADO.NET Entity Data Model** entre las opciones en el panel central. Asigne al nuevo archivo de modelo **ContosoModel**.

![Crear modelo](creating-the-web-application/_static/image5.png)

Haga clic en **Agregar**.

En el Asistente de Entity Data Model, seleccione **EF Designer de base de datos**.

![generar a partir de la base de datos](creating-the-web-application/_static/image6.png)

Haga clic en **Siguiente**.

Si tiene conexiones de base de datos definidas dentro de su entorno de desarrollo, es posible que vea una de estas conexiones previamente seleccionadas. Sin embargo, desea crear una nueva conexión a la base de datos que creó en la primera parte de este tutorial. Haga clic en el **nueva conexión** botón.

![conectarse a la base de datos](creating-the-web-application/_static/image7.png)

En la ventana Propiedades de conexión, proporcione el nombre del servidor local donde se creó la base de datos (en este caso **(localdb) \ProjectsV12**). Después de proporcionar el nombre del servidor, seleccione el ContosoUniversityData de las bases de datos disponibles.

![establecer propiedades de conexión](creating-the-web-application/_static/image8.png)

Haga clic en **Aceptar**.

Ahora se muestran las propiedades de conexión correcto. Puede usar el nombre predeterminado para la conexión en el archivo Web.Config

![configuración de conexión](creating-the-web-application/_static/image9.png)

Haga clic en **Siguiente**.

Seleccione **tablas** para generar modelos para las tres tablas.

![Seleccionar tablas](creating-the-web-application/_static/image10.png)

Haga clic en **Finalizar**.

Si recibes una advertencia de seguridad, seleccione **Aceptar** para continuar la ejecución de la plantilla.

Los modelos se generan a partir de las tablas de base de datos y se muestra un diagrama que muestra las propiedades y relaciones entre las tablas.

![diagrama del modelo](creating-the-web-application/_static/image11.png)

La carpeta Models ahora incluye muchos nuevos archivos relacionados con los modelos que se generaron a partir de la base de datos.

![Mostrar los nuevos archivos de modelo](creating-the-web-application/_static/image12.png)

El **ContosoModel.Context.cs** archivo contiene una clase que deriva el **DbContext** clase y proporciona una propiedad para cada clase de modelo que corresponde a una tabla de base de datos. El **Course.cs**, **Enrollment.cs**, y **Student.cs** archivos contienen las clases de modelo que representan las tablas de bases de datos. La clase de contexto y las clases del modelo se usará cuando se trabaja con la técnica scaffolding.

Antes de continuar con este tutorial, compile el proyecto. En la siguiente sección, generará código basado en los modelos de datos, pero esa sección no funcionará si no se ha compilado el proyecto.

> [!div class="step-by-step"]
> [Anterior](setting-up-database.md)
> [Siguiente](generating-views.md)
