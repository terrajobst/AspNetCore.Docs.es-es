---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Introducción a Entity Framework 6 Database First con MVC 5 | Documentos de Microsoft
author: tfitzmac
description: Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: ae60b5c808d2522c66dc17ccf7d16fefdc65d552
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Introducción a Entity Framework 6 Database First con MVC 5
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos. En la última parte de la serie, se implementará el sitio y la base de datos en Azure.
> 
> Esta parte de la serie se centra en crear una base de datos y rellenarlo con datos.
> 
> Esta serie se escribió con las contribuciones de Tom Dykstra y Rick Anderson. Se ha mejorado en función de los comentarios de los usuarios en la sección Comentarios.


## <a name="introduction"></a>Introducción

Este tema muestra cómo comenzar con otra base de datos y crear rápidamente una aplicación web que permite a los usuarios interactuar con los datos. Usa el Entity Framework 6 y 5 de MVC para compilar la aplicación web. La característica de Scaffolding de ASP.NET permite generar automáticamente código para mostrar, actualizar, crear y eliminar datos. Con las herramientas de publicación en Visual Studio, puede implementar fácilmente el sitio y la base de datos en Azure.

Este tema trata la situación donde tiene una base de datos y desea generar código para una aplicación web basada en los campos de esa base de datos. Este enfoque se denomina desarrollo First de base de datos. Si no dispone de una base de datos existente, puede usar un método denominado desarrollo Code First que consiste en definir las clases de datos y generar la base de datos de las propiedades de clase.

Para obtener un ejemplo de introducción de desarrollo Code First, vea [Introducción a ASP.NET MVC 5](../introduction/getting-started.md). Para obtener un ejemplo más avanzado, vea [crear un modelo de datos de Entity Framework para una aplicación de ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Para obtener instrucciones sobre cómo seleccionar qué enfoque de Entity Framework para utilizar, consulte [enfoques de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Requisitos previos

Visual Studio 2013 o Visual Studio Express 2013 para Web

## <a name="set-up-the-database"></a>Configurar la base de datos

Para imitar el entorno de tener una base de datos existente, creará primero una base de datos con datos previamente rellenadas y, a continuación, crear la aplicación web que se conecta a la base de datos.

Este tutorial se desarrolló con LocalDB con Visual Studio 2013 o Visual Studio Express 2013 para Web. Puede usar un servidor de base de datos existente en lugar de LocalDB, pero dependiendo de la versión de Visual Studio y el tipo de base de datos, todas las herramientas de datos en Visual Studio podrían no admitir. Si las herramientas no están disponibles para la base de datos, debe realizar algunos de los pasos específicos de la base de datos en el conjunto de administración de la base de datos.

Si tiene un problema con las herramientas de base de datos de la versión de Visual Studio, asegúrese de que ha instalado la versión más reciente de las herramientas de base de datos. Para obtener información sobre cómo actualizar o instalar las herramientas de base de datos, vea [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Inicie Visual Studio y cree una **proyecto de base de datos de SQL Server**. Denomine el proyecto **ContosoUniversityData**.

![Crear proyecto de base de datos](setting-up-database/_static/image1.png)

Ahora tiene un proyecto de base de datos vacía. Se implementará esta base de datos en Azure, más adelante en este tutorial, por lo que necesitará establecer la base de datos de SQL Azure como la plataforma de destino para el proyecto. Configuración de la plataforma de destino no implementa realmente la base de datos; sólo significa que el proyecto de base de datos comprobará que el diseño de base de datos es compatible con la plataforma de destino. Para establecer la plataforma de destino, abra el **propiedades** para el proyecto y seleccione **base de datos de Microsoft Azure SQL** para la plataforma de destino.

![plataforma de destino de conjunto](setting-up-database/_static/image2.png)

Puede crear las tablas necesarias para este tutorial mediante la adición de secuencias de comandos SQL que definen las tablas. Haga clic en el proyecto y agregar un nuevo elemento.

![Agregar nuevo elemento](setting-up-database/_static/image3.png)

Agregar una nueva tabla denominada Student.

![Agregar tabla student](setting-up-database/_static/image4.png)

En el archivo de la tabla, reemplace el comando de T-SQL con el código siguiente para crear la tabla.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Tenga en cuenta que la ventana de diseño se sincroniza automáticamente con el código. También puede trabajar con el código o en el diseñador.

![Mostrar código y diseño](setting-up-database/_static/image5.png)

Agregar otra tabla. Esta vez denomínelo curso y utilice el siguiente comando de T-SQL.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Y repita una vez más para crear una tabla denominada inscripción.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Puede rellenar la base de datos con datos a través de una secuencia de comandos que se ejecuta una vez implementada la base de datos. Agregar un Script posterior a la implementación para el proyecto. Puede usar el nombre predeterminado.

![Agregar script posterior a la implementación](setting-up-database/_static/image6.png)

Agregue el código de T-SQL siguiente a la secuencia de comandos posterior a la implementación. Esta secuencia de comandos simplemente agrega datos a la base de datos cuando no se encuentra ningún registro coincidente. No sobrescribir o eliminar los datos que se insertó en la base de datos.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Es importante tener en cuenta que el script posterior a la implementación se ejecuta cada vez que implementa el proyecto de base de datos. Por lo tanto, debe considerar detenidamente los requisitos al escribir esta secuencia de comandos. En algunos casos, puede que desee iniciar desde un conjunto conocido de datos cada vez que se implementa el proyecto. En otros casos, no puede modificar los datos existentes de ninguna manera. Según sus requisitos, puede decidir si necesita un script posterior a la implementación o lo que necesita incluir en la secuencia de comandos. Para obtener más información acerca de cómo rellenar la base de datos con un script posterior a la implementación, consulte [datos incluidos en un proyecto de base de datos de SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Ahora tiene 4 archivos de script SQL, pero ninguna de las tablas real. Está listo para implementar el proyecto de base de datos en localdb. En Visual Studio, haga clic en el botón Inicio (o F5) para compilar e implementar el proyecto de base de datos. Compruebe la ficha salida para comprobar que la compilación e implementación se realizó correctamente.

![Mostrar salida](setting-up-database/_static/image7.png)

Para ver que se ha creado la nueva base de datos, abra **Explorador de objetos de SQL Server** y busque el nombre del proyecto en el servidor de base de datos local correcta (en este caso **\ProjectsV12 (localdb)**).

![Mostrar la nueva base de datos](setting-up-database/_static/image8.png)

Para ver que las tablas se rellenan con datos, haga clic en una tabla y seleccione **ver datos**.

![Mostrar datos de la tabla](setting-up-database/_static/image9.png)

Se muestra una vista editable de los datos de tabla.

![Mostrar resultados de datos de tabla](setting-up-database/_static/image10.png)

La base de datos ahora se configura y se rellena con datos. En el siguiente tutorial, creará una aplicación web para la base de datos.

> [!div class="step-by-step"]
> [Siguiente](creating-the-web-application.md)
