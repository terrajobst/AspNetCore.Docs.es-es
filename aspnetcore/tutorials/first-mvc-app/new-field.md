---
title: "Adición de un nuevo campo"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 16efbacf-fe7b-4b41-84b0-06a1574b95c2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 52fe9ee7bf006553c7dcb17c00ff659d3797b204
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="adding-a-new-field"></a>Adición de un nuevo campo

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En esta sección se usa Migraciones de [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First para agregar un nuevo campo al modelo y migrar ese cambio a la base de datos.

Cuando se usa EF Code First para crear una base de datos de forma automática, Code First agrega una tabla a la base de datos para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo a partir del que se ha generado. Si no está sincronizado, EF produce una excepción. Esto facilita la detección de problemas de código o base de datos incoherentes.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

Compile la aplicación (Ctrl + Mayús + B).

Dado que ha agregado un nuevo campo a la clase `Movie`, también debe actualizar la lista blanca de enlaces para que se incluya esta nueva propiedad. En *MoviesController.cs*, actualice el atributo `[Bind]` de los métodos de acción `Create` y `Edit` para incluir la propiedad `Rating`:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

También debe actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.

Edite el archivo */Views/Movies/Index.cshtml* y agregue un campo `Rating`:

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Actualice */Views/Movies/Create.cshtml* con un campo `Rating`. Puede copiar o pegar el elemento "form group" anterior y permitir que IntelliSense le ayude a actualizar los campos. IntelliSense funciona con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro). Nota: En la versión RTM de Visual Studio 2017 debe instalar [Servicios de lenguaje Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) para Razor IntelliSense. Esto se resolverá en la próxima versión.

![El desarrollador ha escrito la letra R para el valor del atributo de asp-for en el segundo elemento de etiqueta de la vista. Ha aparecido un menú contextual de IntelliSense que muestra los campos disponibles, incluido Rating, que se resalta en la lista automáticamente. Cuando el programador hace clic en el campo o presiona Entrar en el teclado, el valor se establece en Rating.](new-field/_static/cr.png)

La aplicación no funcionará hasta que la base de datos se actualice para incluir el nuevo campo. Si la ejecuta ahora, se producirá la siguiente `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Este error aparece porque la clase del modelo Movie actualizado es diferente al esquema de la tabla Movie de la base de datos existente. (No hay ninguna columna Rating en la tabla de la base de datos).

Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque resulta muy conveniente al principio del ciclo de desarrollo cuando se está realizando el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema del modelo y la base de datos juntos. La desventaja es que se pierden los datos existentes en la base de datos, así que no use este enfoque en una base de datos de producción. Usar un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una manera productiva de desarrollar una aplicación.

2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.

3. Use Migraciones de Code First para actualizar el esquema de la base de datos.

Para este tutorial se usa Migraciones de Code First.

Actualice la clase `SeedData` para que proporcione un valor para la nueva columna. A continuación se muestra un cambio de ejemplo, aunque es conveniente realizar este cambio para cada `new Movie`.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Compile la solución.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.

  ![Menú de PMC](adding-model/_static/pmc.png)

En la PMC, escriba los siguientes comandos:

```PMC
Add-Migration Rating
Update-Database
```

El comando `Add-Migration` indica el marco de trabajo de migración para examinar el modelo `Movie` actual con el esquema de base de datos `Movie` actual y para crear el código con el que se migrará la base de datos al nuevo modelo. El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración. Resulta útil emplear un nombre descriptivo para el archivo de migración.

Si elimina todos los registros de la base de datos, el inicializador inicializa la base de datos e incluye el campo `Rating`. Puede hacerlo con los vínculos de eliminación en el explorador o desde SSOX.

Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`. También debe agregar el campo `Rating` a las plantillas de vista `Edit`, `Details` y `Delete`.

>[!div class="step-by-step"]
[Anterior](search.md)
[Siguiente](validation.md)  
