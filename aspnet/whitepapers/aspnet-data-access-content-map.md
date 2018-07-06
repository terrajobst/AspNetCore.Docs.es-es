---
uid: whitepapers/aspnet-data-access-content-map
title: 'Recursos recomendados de acceso a datos de ASP.NET: | Microsoft Docs'
author: rick-anderson
description: Este tema proporcionan vínculos a recursos de documentación acerca de cómo obtener acceso a datos en aplicaciones web ASP.NET, principalmente mediante el uso de Entity Framework y SQL Se...
ms.author: aspnetcontent
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: fb0cea94d82cc8f59ec56a5445ee84d38325995e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832505"
---
<a name="aspnet-data-access---recommended-resources"></a>Acceso a los datos de ASP.NET - recursos recomendado
====================
> Este tema proporcionan vínculos a recursos de documentación acerca de cómo obtener acceso a datos en aplicaciones web ASP.NET, principalmente mediante el uso de Entity Framework y SQL Server.
> 
> Si necesita registrar, tengo un estupendo blog [stackoverflow](http://stackoverflow.com) subproceso, o cualquier otro vínculo que sería útil, [nos envíe un correo electrónico](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) con el vínculo.
> 
> Actualizado 4/3/2014 del último


El tema contiene las siguientes secciones:

- [Introducción al acceso de datos en ASP.NET](#gettingstarted)
- [Uso de Entity Framework](#ef)

    - [Mediante Entity Framework Code First](#cf)
    - [Uso de migraciones de Entity Framework Code First](#efcfmigrations)
    - [Mediante Entity Framework Database First o Model First (EF Designer)](#efdbf)
    - [Carga de datos relacionados en Entity Framework (la carga diferida, la carga diligente y la carga explícita)](#efrelateddata)
    - [Optimizar el rendimiento de Entity Framework](#optimizingef)
    - [Controlar la simultaneidad en una aplicación de Entity Framework](#efconcurrency)
    - [Libros acerca de Entity Framework](#efbooks)
    - [Recursos adicionales de Entity Framework](#otherefresources)
- [Las aplicaciones de enlace de datos en ASP.NET Web Forms](#wfdatabinding)

    - [Usar Web Forms el enlace de modelos](#wfmodelbinding)
    - [Uso de Web Forms controles de origen de datos](#wfdsc)
    - [Uso de Web Forms controles enlazados a datos y las expresiones de enlace de datos](#wfdbc)
- [Trabajar con bases de datos SQL Server](#sqlserver)

    - [Trabajar con bases de datos SQL Server Express LocalDB](#sslocaldb)
    - [Trabajar con bases de datos SQL Server Express](#sse)
    - [Trabajar con Microsoft Azure SQL Database](#ssdb)
    - [Elección entre SQL Server y Microsoft Azure SQL Database](#ssdbchoosing)
- [Trabajar con sistemas de administración de base de datos NoSQL](#nosql)
- [Uso de consultas LINQ en aplicaciones ASP.NET](#linq)
- [Uso de scaffolding de datos dinámicos](#dd)
- [Protección de acceso a datos](#securing)
- [Optimizar el rendimiento de Data Access](#optimizingdataaccess)
- [Implementación de una base de datos](#deploying)
- [Acceso a datos a través de un servicio Web](#webservice)
- [Recursos adicionales](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introducción al acceso de datos en ASP.NET

- [Opciones de almacenamiento de datos (creación de aplicaciones de nube del mundo Real con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capítulo de un libro electrónico sobre el desarrollo para la nube. Presenta las bases de datos NoSQL como una alternativa que muchos desarrolladores familiarizados con las bases de datos relacionales suelen pasar por alto. Se muestran instrucciones sobre qué se debe pensar al elegir relacional o NoSQL o elegir una plataforma concreta.
- [Opciones de acceso a datos ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Introducción a las opciones de acceso a datos para bases de datos relacionales para ASP.NET e instrucciones sobre cómo elegir las plataformas y tener acceso a los métodos que son adecuados para su escenario.
- [Base de datos relacional](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Si no ha trabajado con bases de datos relacionales, consulte esta página para ver una introducción a los conceptos y terminología de la base de datos relacional. Para obtener una introducción a SQL Server en particular vea [trabajar con bases de datos de SQL Server](#sqlserver) más adelante en este tema.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Uso de Entity Framework

- [Enfoques de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Instrucciones sobre cómo elegir un Entity Framework desarrollo enfoque Database First, Model First o Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Mediante Entity Framework Code First
  

Los siguientes tutoriales ofrecen aplicaciones de ejemplo descargable:

- [Introducción a EF 6 mediante MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Abarca una amplia gama de escenarios de Entity Framework Code First, incluidas las migraciones y EF 6 características como asincrónica, intercepción de comandos y resistencia de conexión. Se trata de una versión actualizada de la [EF 5 / 4 MVC serie](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La serie anterior incluye un tutorial en el repositorio y patrones de unidad de trabajo que no se incluye en la nueva serie.
- [Introducción a ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Abarca una escenarios de intervalo de Entity Framework Code First más estrechas pero realiza un trabajo más completo de la introducción de las características MVC.
- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Utiliza Code First en una aplicación de formularios Web Forms.
- [Introducción a ASP.NET 4.5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Introducción a formularios Web Forms con algunos cobertura de Code First. Usa el enlace de modelos.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utiliza Code First en una aplicación de MVC 3 de comercio electrónico que también implementa la pertenencia y autorización. La versión MVC y el sistema de (autenticación y autorización) de la pertenencia ASP.NET usado aquí están actualizados; Para obtener más información actualizada en la pertenencia a ASP.NET, vea [ https://asp.net/identity ](https://asp.net/identity).

Otros recursos:

- [Entity Framework: Code First para una base de datos](https://msdn.microsoft.com/data/jj200620). MSDN. Vídeo y tutorial que muestra cómo usar Code First con una base de datos existente.
- [Centro para desarrolladores de datos: Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Para obtener una guía de documentación de Entity Framework que se ha creado y mantenido por el equipo de Entity Framework, vea el [comenzar](https://msdn.microsoft.com/data/ee712907) vínculo.

Vea también [libros acerca de Entity Framework](#efbooks) y [recursos adicionales de Entity Framework](#otherefresources) más adelante en este tema.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Uso de migraciones de Entity Framework Code First
  

Mayoría de los tutoriales de Code First enumerados anteriormente las migraciones de portada. Consulte también los siguientes recursos.

- [Implementación Web de ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 2 serie de tutoriales que se muestra cómo usar migraciones de Code First para implementar una base de datos.
- [Implementar una aplicación ASP.NET MVC 5 segura con pertenencia, OAuth y SQL Database en un sitio Web de Azure de Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Cómo usar las migraciones para implementar datos de pertenencia y la aplicación en Azure.
- [Información general de la implementación de Web para Visual Studio y ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Consulte la **configuración de implementación de base de datos en Visual Studio** sección para obtener una explicación de cómo se integra las migraciones de Code First en características de implementación web de Visual Studio.
- [Centro para desarrolladores de datos - migraciones de Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Documentación de las migraciones del equipo de Entity Framework.
- [Serie de presentación en pantalla de migraciones](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog EF). Tres vídeos sobre temas avanzados de migraciones de Code First.
- [Migraciones de Code First con ASP.NET Web Pages](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog de Mikesdotnetting). Muestra cómo usar migraciones de Code First con un sitio de ASP.NET Web Pages colocando el contexto de datos en un proyecto de biblioteca de clases de Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Mediante Entity Framework Database First o Model First (EF Designer)

- [Introducción a Entity Framework 6 Database First con MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Ejecutar un script en el Explorador de servidores para crear una base de datos y, a continuación, utilizar el Diseñador de Entity Framework para crear el modelo de datos. Muestra cómo crear web CRUD simple de páginas y otros datos que puede seguir uno de los tutoriales de Code First puesto que todos los flujos de trabajo EF usan la misma API DbContext de funciones de control.

Los siguientes recursos sean más antiguos. Son útiles si desea utilizar la versión 4.0 de Entity Framework y desea usar un control de origen de datos para el enlace de datos en una aplicación de formularios Web Forms.

- [Introducción a Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Se muestra cómo usar el **EntityDataSource** control.
- [Continuando con Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(se muestra cómo usar el **ObjectDataSource** Control. Incluye un tutorial sobre el control de simultaneidad, un tutorial sobre el rendimiento de EF y un tutorial sobre cuáles son las novedades de EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Control de los datos relacionados de Entity Framework (la carga diferida, la carga diligente y la carga explícita)

- [Leer datos relacionados con Entity Framework en una aplicación ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, la aplicación de ejemplo MVC. Los métodos que se muestran se aplican también al enlace de modelos de formularios Web Forms y la base de datos primer flujo de trabajo.
- [Centro para desarrolladores de datos: cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232) (MSDN). Datos relacionados con la documentación del equipo de Entity Framework acerca de la carga.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimizar el rendimiento de Entity Framework

- [Advanced escenarios de Entity Framework para una aplicación ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Muestra cómo ejecutar sus propias instrucciones SQL o llamar a sus propios procedimientos almacenados, cómo deshabilitar la detección de cambios y cómo deshabilitar la validación al guardar los cambios.
- [Consideraciones de rendimiento para Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Consideraciones de rendimiento (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximizar el rendimiento con Entity Framework en una aplicación Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Se aplica a Entity Framework 4.0.
- Vea también [acceso a datos ASP.NET optimizar](#optimizingdataaccess) más adelante en este tema.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Controlar la simultaneidad en una aplicación de Entity Framework

- [Controlar la simultaneidad con Entity Framework en una aplicación ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, la API de DbContext, mediante una aplicación de ejemplo MVC.
- [Centro para desarrolladores de datos: modelos de simultaneidad optimista](https://msdn.microsoft.cus/data/jj592904) (MSDN). Documentación de simultaneidad del equipo de Entity Framework.
- [Controlar la simultaneidad con Entity Framework en una aplicación Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Se aplica a Entity Framework 4.0. En primer lugar, la base de datos API de ObjectContext, mediante una aplicación de ejemplo de formularios Web Forms.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Libros acerca de Entity Framework

- [Programming Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) por Julie Lerman y Rowan Miller.
- [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) por Julie Lerman y Rowan Miller.

Estos libros están actualizados con las técnicas recomendadas actuales. Proporcionan una introducción más completa pero muy fácil de seguir a Entity Framework que cualquier disponibles en Internet. Otro libro, [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) , de Julie Lerman, es más grande y más completa, pero son más antiguos y muchas de las técnicas que cubre ya no son el método recomendado para usar Entity Framework. Consulte también la lista de libros recomendados por el equipo de Entity Framework en [Centro para desarrolladores de datos: los libros en pantalla](https://msdn.microsoft.com/data/aa937716) en el sitio MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Otros recursos de Entity Framework

- [Blog del equipo de Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Uno de los mejores recursos para los anuncios de nuevas mejoras y la información más reciente. Para otros blogs relacionadas con EF, consulte el Blogroll en [empezar a trabajar con Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Consulte la **puntos de datos** columna, que es con frecuencia sobre temas relacionados con Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Las aplicaciones de enlace de datos en ASP.NET Web Forms

- [Opciones de acceso de datos de ASP.NET Web Forms](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Usar Web Forms el enlace de modelos

- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Serie de tutoriales con EF Code First.
- [Web Forms modelo enlace parte 1: Seleccionar datos](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie). En estas entradas de blog anterior, la propiedad que se denomina actualmente ItemType se denominaba ModelType pero, en caso contrario, la información que contienen es válida.
- [Web Forms modelo enlace parte 2: Filtrar datos](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Web Forms modelo enlace parte 3: Actualizar y validación](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Scott Guthrie).
- [Enlace de modelos de ASP.NET 4.5 Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vídeo).
- [Parte 1: seleccionar datos de enlace de modelos](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vídeo).
- [Modelo de enlace, parte 2: filtrado](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vídeo).
- [Introducción a ASP.NET 4.5 Web Forms, mostrar datos de elementos y detalles](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Uso de Web Forms controles de origen de datos

- [Controles de servidor Web de origen de datos](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Anuncio de la versión del proveedor de datos dinámicos y de control EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de desarrollo Web de Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Uso de Web Forms controles enlazados a datos y las expresiones de enlace de datos

- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Serie de tutoriales que usa EF Code First.
- [Introducción a ASP.NET 4.5 Web Forms, mostrar datos de elementos y detalles](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Controles datos fuertemente tipados](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Controles datos fuertemente tipados](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [ASP.NET 4.5 seguro escrito datos controles de formularios Web](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [Enlazado a datos controles de servidor Web](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Información general de las expresiones de enlace de datos](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Esta página solo se tratan **Eval** y **enlazar**; no se actualizó para incluir **elemento** y **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Trabajar con bases de datos SQL Server

- [Características de base de datos SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Para obtener una introducción general a una amplia variedad de temas de SQL Server, vea las entradas debajo de éste en la tabla de contenido.
- [Ediciones de SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Un resumen de las ediciones de SQL Server disponibles, con vínculos a información adicional sobre cada uno de ellos).
- [Cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Usar SQL Server Compact para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [De Microsoft SQL Server: Muestras de productos de la base de datos](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Bases de datos de ejemplo AdventureWorks.
- [Instalar bases de datos de ejemplo](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Además de los métodos que se muestra aquí, también puede descargar uno de los archivos .mdf de ejemplo a la aplicación\_carpeta de datos de un proyecto web, convertir la base de datos en LocalDB y crear una cadena de conexión LocalDB. Para obtener información acerca de cómo hacerlo, consulte [Cómo: actualizar a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Consulte también las siguientes secciones para trabajar con SQL Server Express y LocalDB y elegir entre SQL Server y SQL Database.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Trabajar con bases de datos SQL Server Express LocalDB

- [SQL Server Express LocalDB 2012](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). La introducción de MSDN oficial en LocalDB.
- [Cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Cómo: actualizar a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Cómo migrar un archivo .mdf desde una versión anterior de SQL Server Express LocalDB. También tiene que realizar este proceso si descarga uno de los [bases de datos de ejemplo de SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Introducción a LocalDB, una instrucción SQL Express mejorado](https://go.microsoft.com/fwlink/?LinkId=234375) (blog de SQL Server Express). Tiene más información acerca de por qué se creó LocalDB que se incluye en MSDN.
- [LocalDB: ¿Dónde está mi base de datos?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog de SQL Server Express). Información acerca de dónde se crean archivos de base de datos LocalDB.
- [Usar LocalDB con IIS completo, parte 1: perfil de usuario](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog de SQL Server Express). LocalDB no está diseñado para trabajar con IIS. Esta serie de entradas de blog explican los problemas y algunas soluciones alternativas.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Trabajar con bases de datos SQL Server Express

- [Cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Si usa el valor de cadena de conexión AttachDBFileName con SQL Server Express, vea especialmente la sección de la instancia de usuario de esta página.
- [Cómo tomar posesión de su SQL Server Express 2008 local](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog de SQL Server Express). Es un problema común de no poder trabajar con bases de datos de SQL Server Express ya no es un administrador en la instancia de SQL Server Express. De forma predeterminada, solo la persona que instaló SQL Server Express es un administrador. Esta entrada de blog explica cómo realizar manualmente un administrador de SQL Server Express si es un administrador en el equipo.
- [¿Mi aplicación web ASP.NET puede usar una base de datos de SQL Server Express en producción?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Trabajar con Microsoft Azure SQL Database

- [Implementar una aplicación ASP.NET MVC segura con suscripción, OAuth y SQL Database a un sitio Web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (sitio de Microsoft Azure).
- [Las bases de datos SQL](https://docs.microsoft.com/azure/sql-database/) (sitio de Microsoft Azure). Obtención de tutoriales de introducción y guías de procedimientos.
- [Microsoft Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). El nodo de nivel superior de la tabla de contenido de la base de datos de SQL en MSDN.
- [Índice de Windows Azure SQL Database TechNet Wiki artículos](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (sitio de Microsoft TechNet).
- [Bloque de aplicaciones de control de errores transitorios](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Un marco que permite controlar los errores de red transitorios y errores de conexión de ese resultado de la limitación. Disponible en un paquete de NuGet: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Introducción a base de datos SQL y Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit de aprendizaje de Azure Windows](https://www.microsoft.com/download/details.aspx?id=8396) (centro de descarga de Microsoft). Incluye laboratorios prácticos para la base de datos SQL.
- [Foro de la Comunidad de Windows Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Mover a Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un capítulo de un escenario de extremo a otro completo por el equipo de Microsoft Patterns and Practices. ¿Por qué desea migrar de portadas y cómo migrar de SQL Server a SQL Database.
- [Migrar bases de datos SQL Server a Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Asistente para migración de base de datos SQL](http://sqlazuremw.codeplex.com/). Una herramienta de código abierto para migrar bases de datos hacia y desde la base de datos SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Elección entre SQL Server y Microsoft Azure SQL Database

- [Comparar SQL Server con Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (sitio de Microsoft TechNet).
- [Migración de datos a Microsoft Azure SQL Database: herramientas y técnicas](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Incluye secciones que comparan SQL Server a SQL Database y ofrecen orientación sobre cuándo migrar de SQL Server a SQL Database.
- [Guía de entrega de Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (sitio de Microsoft TechNet).
- [Limitaciones de características SQL Server (Microsoft Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Azure Table Storage de Microsoft y Windows Azure SQL Database: comparación y diferencias](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Para una aplicación que se implementa en Windows Azure, Windows Azure Table storage podría ser una alternativa a Windows Azure SQL Database. Este tema le ayudará a decidir entre estas alternativas.
- [Microsoft Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Instrucciones y limitaciones (Microsoft Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Trabajar con sistemas de administración de base de datos NoSQL

- [Data Services de Windows Azure](https://www.windowsazure.com/develop/net/data/) (sitio de Microsoft Azure). Consulte la [Guía de características de Table Service](https://docs.microsoft.com/azure/) y **Macrodatos** sección de la página.
- [Aplicación ASP.NET multinivel usando almacenamiento de tablas, colas y Blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sitio de Microsoft Azure). Tutorial to-end con la aplicación de ejemplo descargable que usa las tablas de NoSQL de almacenamiento de Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Uso de consultas LINQ en aplicaciones ASP.NET

- [Opciones de acceso a datos ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Incluye una introducción a LINQ.
- [Vídeos de aprendizaje de LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [Foro de ASP.NET con vínculos a los recursos dinámicos de LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Uso de Scaffolding de datos dinámicos

- [Plantillas de proyecto de datos dinámicos](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Orientación sobre cuándo usar proyectos de datos dinámicos.
- [Datos dinámicos de ASP.NET](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protección de acceso a datos

- [Protección de acceso a datos en ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Consideraciones de seguridad (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Cómo: Proteger las cadenas de conexión al usar controles de origen de datos](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimizar el rendimiento de Data Access

- [Información general sobre el rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Almacenamiento en caché de ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Mejorar el rendimiento de ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Hay una advertencia "Contenido retirado" en la parte superior de esta página, pero la mayoría de la información sigue siendo pertinente y no hay ningún recurso actualizado comparable.
- [Mejorar el rendimiento de SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Mismo comentario como el vínculo anterior.

Vea también [rendimiento de la optimización de Entity Framework](#optimizingef) anteriormente en este tema.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Implementación de una base de datos

- [Implementación Web de ASP.NET - recursos recomendados](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Acceso a datos a través de un servicio Web

- [Acceso a datos a través de un servicio Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Orientación sobre cuándo usar las API de Web frente a WCF.
- [Introducción a ASP.NET Web API](../web-api/index.md).
- [Data Services de WCF](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [P+F de acceso a datos ASP.NET](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Tutoriales: datos de ASP.NET Web Forms](../web-forms/overview/data-access/index.md). La mayoría de estos tutoriales son relativamente antigua; Asegúrese de leer [opciones de acceso a datos ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) y [opciones de almacenamiento de datos (crear aplicaciones de nube de mundo Real con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) primero para que no obtenga demasiado lejos en un método de acceso a datos que no es correcto para su escenario.
- [Mapa de contenido de ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Tutoriales: datos de ASP.NET Web Pages](../web-pages/overview/data/index.md).
- [Acceso a datos en Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Proporciona una lista de vínculos similar a este mapa de contenido, pero con un enfoque en Visual Studio, en lugar de ASP.NET.
