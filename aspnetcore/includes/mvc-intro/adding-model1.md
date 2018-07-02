# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Agregar un modelo a una aplicación de ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Tom Dykstra](https://github.com/tdykstra)

En esta sección, agregará algunas clases para administrar películas en una base de datos. Estas clases serán el elemento "**M**odel" de la aplicación **M**VC.

Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos. EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos que se debe escribir. [EF Core es compatible con muchos motores de base de datos](/ef/core/providers/).

Las clases de modelo que se crean se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core. Simplemente definen las propiedades de los datos que se almacenan en la base de datos.

En este tutorial, se escriben primero las clases de modelo y EF Core creará la base de datos. Hay un enfoque alternativo, que no se trata aquí, que consiste en generar clases de modelo a partir de una base de datos existente. Para más información sobre este enfoque, vea [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db) (ASP.NET Core - base de datos existente).

## <a name="add-a-data-model-class"></a>Agregar una clase de modelo de datos
