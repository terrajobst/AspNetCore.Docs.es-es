---
uid: whitepapers/aspnet-data-access-content-map
title: Acceso a los datos de ASP.NET - recomienda recursos | Documentos de Microsoft
author: rick-anderson
description: En este tema se proporciona vínculos a recursos de documentación acerca de cómo obtener acceso a datos en aplicaciones web ASP.NET, principalmente mediante Entity Framework y SQL Se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 16364951544dff33c9cdb8c8e330cc93de192c47
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28048264"
---
<a name="aspnet-data-access---recommended-resources"></a>Acceso a los datos de ASP.NET: los recursos recomendado
====================
> Este tema proporciona vínculos a recursos de documentación acerca de cómo obtener acceso a datos en aplicaciones web ASP.NET, principalmente mediante el Entity Framework y SQL Server.
> 
> Si conoce un estupendo blog post, [stackoverflow](http://stackoverflow.com) subproceso, o cualquier otro vínculo a la que podría ser útil, [envíenos un correo electrónico](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) con el vínculo.
> 
> Actualizado 4/3/2014 del último


El tema contiene las siguientes secciones:

- [Introducción al acceso a datos en ASP.NET](#gettingstarted)
- [Con Entity Framework](#ef)

    - [Utilizando el código de Entity Framework primero](#cf)
    - [Con Entity Framework Code First Migrations](#efcfmigrations)
    - [Utilizando la base de datos de Entity Framework primero o el modelo primero (Diseñador de EF)](#efdbf)
    - [Cargar datos relacionados en Entity Framework (la carga diferida, carga diligente y carga explícita)](#efrelateddata)
    - [Optimizar el rendimiento de Entity Framework](#optimizingef)
    - [Control de simultaneidad en una aplicación de Entity Framework](#efconcurrency)
    - [Libros sobre Entity Framework](#efbooks)
    - [Recursos adicionales de Entity Framework](#otherefresources)
- [Las aplicaciones de enlace de datos en ASP.NET Web Forms](#wfdatabinding)

    - [Usar Web Forms el enlace de modelos](#wfmodelbinding)
    - [Usar Web Forms controles de origen de datos](#wfdsc)
    - [Usar Web Forms controles enlazados a datos y expresiones de enlace de datos](#wfdbc)
- [Trabajar con bases de datos SQL Server](#sqlserver)

    - [Trabajar con bases de datos SQL Server Express LocalDB](#sslocaldb)
    - [Trabajar con bases de datos SQL Server Express](#sse)
    - [Trabajar con la base de datos SQL Azure de Windows](#ssdb)
    - [Elegir entre SQL Server y base de datos SQL Azure de Windows](#ssdbchoosing)
- [Trabajar con sistemas de administración de base de datos NoSQL](#nosql)
- [Uso de consultas LINQ en aplicaciones ASP.NET](#linq)
- [Mediante scaffolding de datos dinámicos](#dd)
- [Protección de acceso a datos](#securing)
- [Optimizar rendimiento del acceso a datos](#optimizingdataaccess)
- [Implementar una base de datos](#deploying)
- [Obtener acceso a datos a través de un servicio Web](#webservice)
- [Recursos adicionales](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introducción al acceso a datos en ASP.NET

- [Opciones de almacenamiento de datos (creación de aplicaciones de nube reales con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capítulo de un libro electrónico sobre el desarrollo de la nube. Las bases de datos NoSQL se presenta como una alternativa que muchos desarrolladores familiarizados con las bases de datos relacional suelen pasar por alto. Presenta las directrices sobre lo que debe pensar al elegir relacional o NoSQL o elegir una plataforma concreta.
- [Opciones de acceso de datos de ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Introducción a las opciones de acceso a datos para bases de datos relacionales para ASP.NET e instrucciones sobre cómo elegir las plataformas y obtener acceso a los métodos que son adecuados para su escenario.
- [Base de datos relacional](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Si no ha trabajado con bases de datos relacionales, consulte esta página para ver una introducción a los conceptos y terminología de base de datos relacional. Para obtener una introducción a SQL Server en particular vea [trabajar con bases de datos de SQL Server](#sqlserver) más adelante en este tema.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Con Entity Framework

- [Enfoques de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Instrucciones sobre cómo elegir un Entity Framework desarrollo enfoque Database First, Model First o Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Utilizando el código de Entity Framework primero
  

Los siguientes tutoriales proporcionan aplicaciones de ejemplo descargable:

- [Introducción a 6 de EF mediante MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Abarca una amplia gama de escenarios de Entity Framework Code First, incluyendo las migraciones y 6 de EF características como resistencia de conexión, la interceptación de comando y asincrónico. Se trata de una versión actualizada de la [EF 5 / serie de MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La serie anterior incluye un tutorial en el repositorio y patrones de unidad de trabajo que no se incluye en la nueva serie.
- [Introducción a ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Cubre un escenarios de primer intervalo de Entity Framework código más estrechas pero no un trabajo más completo de la introducción de las características MVC.
- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Usa Code First en una aplicación de formularios Web Forms.
- [Introducción a ASP.NET 4.5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Introducción a los formularios Web Forms con algunos cobertura de Code First. Utiliza el enlace de modelo.
- [Tienda de música de MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Usa Code First en una aplicación de MVC 3 de comercio electrónico que también implementa la pertenencia y la autorización. La versión MVC y el sistema de (autenticación y autorización) de la pertenencia ASP.NET usada aquí están actualizados; Para obtener información más actualizada en la pertenencia a ASP.NET, vea [https://asp.net/identity](https://asp.net/identity).

Otros recursos:

- [Entity Framework: Code primero a una base de datos](https://msdn.microsoft.com/data/jj200620). MSDN. Vídeo y tutorial que muestra cómo usar Code First con una base de datos existente.
- [Centro para desarrolladores de datos - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Para obtener una guía a la documentación de Entity Framework que se ha creado y mantenido por el equipo de Entity Framework, vea la [Introducción](https://msdn.microsoft.com/data/ee712907) vínculo.

Vea también [libros sobre Entity Framework](#efbooks) y [recursos adicionales de Entity Framework](#otherefresources) más adelante en este tema.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Con Entity Framework Code First Migrations
  

Mayoría de los tutoriales de Code First mencionados anteriormente migraciones de portada. Consulte también los siguientes recursos.

- [Implementación de Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). parte 2 serie de tutoriales que se muestra cómo usar migraciones de Code First para implementar una base de datos.
- [Implementar una aplicación de MVC de ASP.NET 5 segura con pertenencia, OAuth y base de datos SQL a un sitio Web de Azure de Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Describe cómo usar las migraciones para implementar datos de pertenencia y aplicación en Azure.
- [Información general de la implementación de Web para Visual Studio y ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Consulte la **configuración de implementación de base de datos en Visual Studio** sección para obtener una explicación de cómo se integra migraciones de Code First en características de implementación web de Visual Studio.
- [Centro para desarrolladores de datos - migraciones de Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Documentación de migraciones del equipo de Entity Framework.
- [Serie de presentación en pantalla de migraciones](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog EF). Vídeos de tres en temas avanzados de migraciones de Code First.
- [Migraciones de Code First con sitios ASP.NET Web Pages](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog de Mikesdotnetting). Muestra cómo utilizar las migraciones de Code First con un sitio de ASP.NET Web Pages colocando el contexto de datos en un proyecto de biblioteca de clases de Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Utilizando la base de datos de Entity Framework primero o el modelo primero (Diseñador de EF)

- [Introducción a Entity Framework 6 Database First con MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Ejecutar un script en el Explorador de servidores para crear una base de datos y, a continuación, utilizar el Diseñador de Entity Framework para crear el modelo de datos. Muestra cómo crear sitio web CRUD simple páginas y otros datos de control de las funciones que puede seguir uno de los tutoriales de Code First debido a que todos los flujos de trabajo EF utilizan las mismas API de DbContext.

Los siguientes recursos sean más antiguos. Son útiles si desea utilizar la versión 4.0 de Entity Framework y desea usar un control de origen de datos para el enlace de datos en una aplicación de formularios Web Forms.

- [Introducción a Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Muestra cómo utilizar el **EntityDataSource** control.
- [Continuar con Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(muestra cómo utilizar el **ObjectDataSource** Control. Incluye un tutorial sobre el control de simultaneidad, un tutorial sobre el rendimiento de EF y un tutorial sobre cuáles son las novedades en EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Control de los datos relacionados de Entity Framework (la carga diferida, carga diligente y carga explícita)

- [Leer datos relacionados con Entity Framework en una aplicación ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Aplicación de ejemplo MVC en primer lugar, el código. Los métodos que se muestran se aplican también a enlace de modelos de formularios Web Forms y la base de datos primer flujo de trabajo.
- [Centro para desarrolladores de datos - cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232) (MSDN). Datos relacionados con la documentación del equipo de Entity Framework sobre la carga.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimizar el rendimiento de Entity Framework

- [Advanced escenarios de Entity Framework para una aplicación ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Muestra cómo ejecutar sus propias instrucciones SQL o llamar a sus propios procedimientos almacenados, cómo deshabilitar la detección de cambios y cómo deshabilitar la validación al guardar los cambios.
- [Consideraciones de rendimiento para Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Consideraciones de rendimiento (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximizar el rendimiento con Entity Framework en una aplicación Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Se aplica a Entity Framework 4.0.
- Vea también [ASP.NET optimizar el acceso a los datos](#optimizingdataaccess) más adelante en este tema.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Control de simultaneidad en una aplicación de Entity Framework

- [Control de simultaneidad con Entity Framework en una aplicación ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). API de DbContext, mediante una aplicación de ejemplo MVC en primer lugar, el código.
- [Centro para desarrolladores de datos: simultaneidad optimista patrones](https://msdn.microsoft.cus/data/jj592904) (MSDN). Documentación de simultaneidad del equipo de Entity Framework.
- [Control de simultaneidad con Entity Framework en una aplicación Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Se aplica a Entity Framework 4.0. En primer lugar, la base de datos API ObjectContext, mediante una aplicación de ejemplo de formularios Web Forms.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Libros sobre Entity Framework

- [Programación de Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman y Rowan Miller.
- [Programación de Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman y Rowan Miller.

Estos dos libros estén actualizados con técnicas recomendadas actuales. Ofrece una introducción más completa pero fácil de seguir a Entity Framework que cualquier disponibles en Internet. Otro libro, [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) Julie Lerman, es más grandes y más completa, pero son más antiguas y muchas de las técnicas que cubre ya no son la manera recomendada para que use Entity Framework. Vea también la lista de libros recomendados por el equipo de Entity Framework en [Centro para desarrolladores de datos - libros](https://msdn.microsoft.com/data/aa937716) en el sitio MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Otros recursos de Entity Framework

- [Blog del equipo de Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Uno de los mejores recursos para anuncios de nuevas mejoras y la información más reciente. Para otros blogs relacionados con EF, consulte el Blogroll en [empezar a trabajar con Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Consulte la **puntos de datos** columna, que suele ser sobre temas relacionados con Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Las aplicaciones de enlace de datos en ASP.NET Web Forms

- [Opciones de acceso de datos de ASP.NET Web Forms](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Usar Web Forms el enlace de modelos

- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Serie de tutoriales mediante EF Code First.
- [Elemento de enlace de modelo de formularios Web 1: Seleccionar datos](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog de Guthrie). En estas entradas de blog anteriores, la propiedad que actualmente se denomina ItemType se llamaba ModelType, pero en caso contrario, la información que contienen es válida.
- [Elemento de enlace de modelo de formularios Web 2: Filtrar datos](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog de Guthrie).
- [Elemento de enlace de modelo de formularios Web 3: Actualizar y validación](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Guthrie).
- [Enlace de modelo de ASP.NET 4.5 Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Parte 1: seleccionar datos de enlace de modelo](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vídeo).
- [Modelo de enlace parte 2: filtrado](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vídeo).
- [Introducción a trabajar con ASP.NET 4.5 Web Forms - mostrar datos de elementos y detalles](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Usar Web Forms controles de origen de datos

- [Controles de servidor Web de origen de datos](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Anuncio de la versión del proveedor de datos dinámicos y de control EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de desarrollo Web de Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Usar Web Forms controles enlazados a datos y expresiones de enlace de datos

- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Serie de tutoriales que usa EF Code First.
- [Introducción a trabajar con ASP.NET 4.5 Web Forms - mostrar datos de elementos y detalles](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Controles datos fuertemente tipados](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Guthrie).
- [Controles datos fuertemente tipados](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [ASP.NET 4.5 seguro escrito datos controles de formularios Web](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [Controles de servidor Web de enlace a datos](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Información general de las expresiones de enlace de datos](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Esta página solo cubre **Eval** y **enlazar**; no se ha actualizado para incluir **elemento** y **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Trabajar con bases de datos SQL Server

- [Características de base de datos SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Para obtener una introducción general a una amplia variedad de temas de SQL Server, vea las entradas debajo de éste en la TDC.
- [Ediciones de SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Un resumen de las ediciones de SQL Server disponibles, con vínculos a más información acerca de cada uno.)
- [Cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Uso de SQL Server Compact para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [De Microsoft SQL Server: Muestras de productos de la base de datos](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Bases de datos de ejemplo AdventureWorks.
- [Instalar bases de datos de ejemplo](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Además de los métodos que se muestra aquí, también puede descargar uno de los archivos .mdf de ejemplo a la aplicación\_carpeta de datos de un proyecto web, convertir la base de datos en LocalDB y crear una cadena de conexión de LocalDB. Para obtener información acerca de cómo hacerlo, consulte [Cómo: actualizar a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Consulte también las siguientes secciones en trabajar con LocalDB y SQL Server Express y elegir entre SQL Server y base de datos SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Trabajar con bases de datos SQL Server Express LocalDB

- [SQL Server Express LocalDB 2012](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). La introducción de MSDN oficial en LocalDB.
- [Cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Cómo: actualizar a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Cómo migrar un archivo .mdf desde una versión anterior de SQL Server Express LocalDB. También tiene que pasar a través de este proceso si descarga uno de los [bases de datos de ejemplo de SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Introducción a LocalDB, un mejor SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog de SQL Server Express). Tiene más información sobre por qué se creó LocalDB que se incluye en MSDN.
- [LocalDB: ¿Dónde está mi base de datos?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog de SQL Server Express). Información sobre dónde se crean archivos de base de datos de LocalDB.
- [Con LocalDB completa de IIS, parte 1: perfil de usuario](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog de SQL Server Express). LocalDB no está diseñado para trabajar con IIS. La siguiente serie de entradas de blog explica los problemas y algunas soluciones alternativas.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Trabajar con bases de datos SQL Server Express

- [Cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Si usa la opción de cadena de conexión AttachDBFileName con SQL Server Express, vea especialmente la sección de la instancia de usuario de esta página.
- [Cómo tomar posesión de su SQL Server Express 2008 local](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog de SQL Server Express). Es un problema común no poder trabajar con bases de datos de SQL Server Express ya no es un administrador en la instancia de SQL Server Express. De forma predeterminada, solo la persona que instaló SQL Server Express es un administrador. Este blog explica cómo realizar manualmente un administrador de SQL Server Express si es un administrador en el equipo.
- [¿Mi aplicación web ASP.NET puede usar una base de datos de SQL Server Express en producción?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Trabajar con la base de datos SQL Azure de Windows

- [Implementar una aplicación de MVC de ASP.NET seguros con pertenencia, OAuth y base de datos SQL a un sitio Web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (sitio de Microsoft Azure).
- [Las bases de datos de SQL](https://docs.microsoft.com/azure/sql-database/) (sitio de Microsoft Azure). Obtener tutoriales y guías de procedimientos.
- [Base de datos SQL Azure de Windows](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). El nodo de nivel superior de la tabla de contenido de la base de datos de SQL en MSDN.
- [Índice de artículos del Wiki de TechNet de Windows Azure SQL base de datos](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (sitio de TechNet de Microsoft).
- [Bloque de aplicación de control de errores transitorios](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Un marco que permite controlar los errores de conexión y errores de red transitorio ese resultado de la limitación. Disponible en un paquete de NuGet: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Introducción a la base de datos SQL y Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit de aprendizaje de Azure Windows](https://www.microsoft.com/download/details.aspx?id=8396) (centro de descarga de Microsoft). Incluye laboratorios de prácticas de base de datos de SQL.
- [Foro de la Comunidad de base de datos de Windows Azure SQL](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Mover a la base de datos SQL Azure de Windows](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un capítulo de un escenario de extremo a extremo completa por el equipo de Microsoft Patterns and Practices. ¿Por qué desea migrar de portadas y cómo migrar desde SQL Server a la base de datos SQL.
- [Migrar bases de datos SQL Server en Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Asistente para migración de base de datos SQL](http://sqlazuremw.codeplex.com/). Una herramienta de código abierto para migrar bases de datos a y desde la base de datos SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Elegir entre SQL Server y base de datos SQL Azure de Windows

- [Comparar SQL Server con Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (sitio de TechNet de Microsoft).
- [Migración de datos a la base de datos SQL Azure de Windows: herramientas y técnicas](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Incluye secciones que comparan SQL Server para la base de datos SQL y ofrecen orientación sobre cuándo migrar de SQL Server a la base de datos de SQL.
- [Guía de entrega de base de datos SQL de Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (sitio de TechNet de Microsoft).
- [Limitaciones de características SQL Server (base de datos SQL Azure de Windows)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [El almacenamiento de Windows Azure de tabla y base de datos SQL Azure de Windows: comparación y diferencias](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Para una aplicación que se implementa en Windows Azure, almacenamiento de tabla de Windows Azure puede ser una alternativa a la base de datos de SQL de Windows Azure. Este tema le ayudará a decidir entre estas alternativas.
- [Base de datos SQL Azure de Windows](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Instrucciones y limitaciones (base de datos SQL Azure de Windows)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Trabajar con sistemas de administración de base de datos NoSQL

- [Servicios de datos de Windows Azure](https://www.windowsazure.com/develop/net/data/) (sitio de Microsoft Azure). Consulte la [Guía de características de servicio tabla](https://docs.microsoft.com/azure/) y **grandes cantidades de datos** sección de la página.
- [Aplicación ASP.NET multinivel usando almacenamiento de tablas, colas y Blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sitio de Microsoft Azure). Tutorial to-end con la aplicación de ejemplo descargable que utiliza las tablas de NoSQL de almacenamiento de Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Uso de consultas LINQ en aplicaciones ASP.NET

- [Opciones de acceso de datos de ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Incluye una introducción a LINQ.
- [Vídeos de aprendizaje de LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [Subproceso del foro de ASP.NET con vínculos a recursos LINQ dinámicos](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Mediante Scaffolding de datos dinámicos

- [Plantillas de proyecto de datos dinámicos](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Orientación sobre cuándo usar proyectos de datos dinámicos.
- [Datos dinámicos de ASP.NET](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protección de acceso a datos

- [Protección de acceso a datos en ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Consideraciones de seguridad (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Cómo: Proteger las cadenas de conexión al utilizar controles de origen de datos](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimizar rendimiento del acceso a datos

- [Información general sobre el rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Almacenamiento en caché de ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Mejorar el rendimiento de ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Hay una advertencia "Retirado contenido" en la parte superior de esta página, pero la mayor parte de la información sigue siendo pertinente y no hay ningún recurso actualizado comparable.
- [Mejorar el rendimiento de SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Mismo comentario como el vínculo anterior.

Vea también [rendimiento optimizar Entity Framework](#optimizingef) anteriormente en este tema.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Implementar una base de datos

- [Implementación de Web de ASP.NET: los recursos recomendados](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Obtener acceso a datos a través de un servicio Web

- [Obtener acceso a datos a través de un servicio Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Orientación sobre cuándo usar API de Web frente a WCF.
- [Introducción a ASP.NET Web API](../web-api/index.md).
- [Servicios de datos WCF](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [Acceso a datos de ASP.NET de preguntas más frecuentes sobre](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Formularios Web Forms ASP.NET tutoriales - datos](../web-forms/overview/data-access/index.md). La mayoría de estos tutoriales son relativamente anterior; Asegúrese de leer [opciones de acceso a datos de ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) y [opciones de almacenamiento de datos (creación reales en la nube de aplicaciones con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) primero para que no obtenga demasiado lejos en un método de acceso de datos que no es correcto para su escenario.
- [Mapa de contenido de ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Tutoriales: datos de ASP.NET Web Pages](../web-pages/overview/data/index.md).
- [Obtener acceso a datos en Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Proporciona una lista de vínculos similar a este mapa de contenido, pero con un enfoque en Visual Studio, en lugar de ASP.NET.
