---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Introducción a Entity Framework 6 Database First con MVC 5 | Microsoft Docs
author: Rick-Anderson
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 7fcb2b82dfa27ae192e1890c0c771d68658760a4
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021149"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Introducción a Entity Framework 6 Database First con MVC 5
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos. En la última parte de la serie, se implementará el sitio y la base de datos en Azure.
> 
> Esta parte de la serie se centra en crear una base de datos y rellenarlo con datos.
> 
> Esta serie se escribió con las contribuciones de Tom Dykstra y Rick Anderson. Se ha mejorado en función de los comentarios de los usuarios en la sección Comentarios.


## <a name="introduction"></a>Introducción

En este tema se muestra cómo comenzar con una existente de base de datos y crear rápidamente una aplicación web que permite a los usuarios interactuar con los datos. Usa el Entity Framework 6 y MVC 5 para compilar la aplicación web. La característica de Scaffolding de ASP.NET le permite generar automáticamente código para mostrar, actualizar, crear y eliminar datos. Con las herramientas de publicación en Visual Studio, puede implementar fácilmente el sitio y la base de datos en Azure.

En este tema se aborda la situación donde tiene una base de datos y desea generar código para una aplicación web basada en los campos de esa base de datos. Este enfoque se denomina desarrollo Database First. Si no dispone de una base de datos existente, puede usar en su lugar un enfoque de desarrollo Code First que implica definir clases de datos y generar la base de datos de las propiedades de clase.

Para obtener un ejemplo introductorio de desarrollo Code First, consulte [Introducción a ASP.NET MVC 5](../introduction/getting-started.md). Para obtener un ejemplo más avanzado, consulte [crear un modelo de datos de Entity Framework para una aplicación de ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Para obtener instrucciones sobre cómo seleccionar el enfoque de Entity Framework para utilizar, consulte [enfoques de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Requisitos previos

Visual Studio 2013 o Visual Studio Express 2013 para Web

## <a name="set-up-the-database"></a>Configurar la base de datos

Para imitar el entorno de tener una base de datos existente, creará primero una base de datos con algunos datos previamente rellenadas y, a continuación, cree la aplicación web que se conecta a la base de datos.

En este tutorial se desarrolló con LocalDB con Visual Studio 2013 o Visual Studio Express 2013 para Web. Puede usar un servidor de base de datos existente en lugar de LocalDB, pero según la versión de Visual Studio y el tipo de base de datos, todas las herramientas de datos en Visual Studio podrían no admitirse. Si las herramientas no están disponibles para la base de datos, deberá realizar algunos de los pasos específicos de la base de datos dentro del paquete de administración de la base de datos.

Si tiene un problema con las herramientas de base de datos en su versión de Visual Studio, asegúrese de que ha instalado la versión más reciente de las herramientas de base de datos. Para obtener información sobre cómo actualizar o instalar las herramientas de base de datos, vea [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Inicie Visual Studio y cree un **el proyecto de base de datos de SQL Server**. Denomine el proyecto **ContosoUniversityData**.

![Crear proyecto de base de datos](setting-up-database/_static/image1.png)

Ahora tiene un proyecto de base de datos vacía. Se implementará esta base de datos en Azure, más adelante en este tutorial, por lo que necesitará establecer la base de datos de SQL Azure como la plataforma de destino para el proyecto. Configuración de la plataforma de destino no implementa realmente la base de datos; sólo significa que el proyecto de base de datos comprobará que el diseño de la base de datos es compatible con la plataforma de destino. Para establecer la plataforma de destino, abra el **propiedades** para el proyecto y seleccione **Microsoft Azure SQL Database** para la plataforma de destino.

![Establezca la plataforma de destino](setting-up-database/_static/image2.png)

Puede crear las tablas necesarias en este tutorial, agregando scripts SQL que definen las tablas. Haga clic en el proyecto y agregar un nuevo elemento.

![Agregar nuevo elemento](setting-up-database/_static/image3.png)

Agregar una nueva tabla denominada Student.

![Agregue la tabla student](setting-up-database/_static/image4.png)

En el archivo de la tabla, reemplace el comando de T-SQL con el código siguiente para crear la tabla.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Tenga en cuenta que la ventana de diseño se sincroniza automáticamente con el código. Puede trabajar con el código o el diseñador.

![Mostrar código y diseño](setting-up-database/_static/image5.png)

Agregar otra tabla. Esta vez asígnele el curso y use el siguiente comando de T-SQL.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Además, repita una vez más para crear una tabla denominada inscripción.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Puede rellenar la base de datos con datos a través de una secuencia de comandos que se ejecuta una vez implementada la base de datos. Agregar un Script posterior a la implementación para el proyecto. Puede usar el nombre predeterminado.

![Agregar script posterior a la implementación](setting-up-database/_static/image6.png)

Agregue el código de T-SQL siguiente al script posterior a la implementación. Esta secuencia de comandos simplemente agrega datos a la base de datos cuando no se encuentra ningún registro coincidente. No sobrescribir ni elimina ningún dato que escribió en la base de datos.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Es importante tener en cuenta que el script posterior a la implementación se ejecuta cada vez que implementa el proyecto de base de datos. Por lo tanto, debe considerar detenidamente los requisitos al escribir esta secuencia de comandos. En algunos casos, puede empezar desde un conjunto conocido de datos cada vez que se implementa el proyecto. En otros casos, es posible que no desee alterar los datos existentes de ninguna manera. Según sus requisitos, puede decidir si necesita un script posterior a la implementación o lo que deberá incluir en la secuencia de comandos. Para obtener más información acerca de cómo rellenar la base de datos con un script posterior a la implementación, consulte [datos incluidos en un proyecto de base de datos de SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Ahora tiene 4 archivos de script SQL, pero no hay tablas reales. Está listo para implementar el proyecto de base de datos en localdb. En Visual Studio, haga clic en el botón Iniciar (o F5) para compilar e implementar el proyecto de base de datos. Compruebe la pestaña de salida para comprobar que la compilación e implementación se realizó correctamente.

![Mostrar salida](setting-up-database/_static/image7.png)

Para ver que se ha creado la nueva base de datos, abra **Explorador de objetos de SQL Server** y busque el nombre del proyecto en el servidor de base de datos local correcto (en este caso **(localdb) \ProjectsV12**).

![Mostrar la nueva base de datos](setting-up-database/_static/image8.png)

Para ver que las tablas se rellenan con datos, haga clic en una tabla y seleccione **ver datos**.

![Mostrar datos de tabla](setting-up-database/_static/image9.png)

Se muestra una vista editable de los datos de tabla.

![Mostrar resultados de datos de tabla](setting-up-database/_static/image10.png)

Ahora, la base de datos se configura y rellena con datos. En el siguiente tutorial, creará una aplicación web para la base de datos.

> [!div class="step-by-step"]
> [Siguiente](creating-the-web-application.md)
