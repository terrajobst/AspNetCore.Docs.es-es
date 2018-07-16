Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En esta sección, agregará las clases para administrar películas en una base de datos. Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos. EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos que se debe escribir.

Las clases de modelo que se crean se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core. Definen las propiedades de los datos que se almacenan en la base de datos.

En este tutorial, se escriben primero las clases del modelo y EF Core crea la base de datos. Hay un enfoque alternativo, que no se trata aquí, que consiste en [generar clases del modelo a partir de una base de datos existente](/ef/core/get-started/aspnetcore/existing-db).

[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) un ejemplo.
