---
title: "Inyección de dependencia en las vistas"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: 4586f50bc663b7269914dfff28b61342e3991a48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-into-views"></a>Inyección de dependencia en las vistas

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core es compatible con [inyección de dependencia](xref:fundamentals/dependency-injection) en vistas. Esto puede ser útil para los servicios específicos de la vista, como localización o solo es necesarios para rellenar los elementos de vista de datos. Debe intentar mantener [separación de intereses](http://deviq.com/separation-of-concerns/) entre los controladores y vistas. La mayoría de los datos que se muestran las vistas se debe pasar desde el controlador.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Un ejemplo sencillo

También puede insertar un servicio en una vista mediante el `@inject` directiva. Puede pensar en `@inject` como agregar una propiedad a la vista y rellenar la propiedad mediante DI.

La sintaxis de `@inject`:`@inject <type> <name>`

Un ejemplo de `@inject` en acción:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Esta vista muestra una lista de `ToDoItem` instancias, junto con un resumen de estadísticas generales. El resumen se rellena desde la insertado `StatisticsService`. Este servicio está registrado para la inyección de dependencia en `ConfigureServices` en *Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

El `StatisticsService` realiza algunos cálculos en el conjunto de `ToDoItem` instancias, que tiene acceso a través de un repositorio:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

El repositorio de ejemplo utiliza una colección en memoria. La implementación se muestra arriba (que funciona en todos los datos en memoria) no se recomienda para conjuntos de datos grandes, con acceso de forma remota.

El ejemplo muestra los datos del modelo enlazado a la vista y el servicio que se insertan en la vista:

![Para ver la lista de elementos total, completa elementos de prioridad Media y una lista de tareas con sus niveles de prioridad y los valores de tipo boolean que indica la finalización.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Rellenar los datos de búsqueda

Inyección de código de la vista puede ser útil para rellenar las opciones de elementos de interfaz de usuario, como listas desplegables. Considere la posibilidad de un formulario de perfil de usuario que incluye opciones para especificar el género, estado y otras preferencias. Representar este tipo un formulario mediante un enfoque MVC estándar requeriría el controlador para solicitar servicios de acceso de datos para cada uno de estos conjuntos de opciones y, a continuación, rellenar un modelo o `ViewBag` con cada conjunto de opciones para enlazar.

Un enfoque alternativo inserta services directamente en la vista para obtener las opciones. Esto reduce la cantidad de código necesario para el controlador, mover esta lógica de construcción del elemento de vista en la propia vista. La acción del controlador para mostrar un formulario de edición de perfil sólo necesita pasar el formulario de la instancia de perfil:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

El formulario HTML que se utiliza para actualizar estas preferencias incluye listas desplegables para tres de las propiedades:

![Actualizar vista del perfil con un formulario que permite la entrada de nombre, sexo, estado y su Color favorito.](dependency-injection/_static/updateprofile.png)

Estas listas se rellenan con un servicio que se ha insertado en la vista:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

El `ProfileOptionsService` es un servicio de nivel de interfaz de usuario diseñado para proporcionar solo los datos necesarios para este formulario:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> No olvide registrar tipos solicitará a través de la inyección de dependencia en el `ConfigureServices` método *Startup.cs*.

## <a name="overriding-services"></a>Reemplazar servicios

Además de insertar nuevos servicios, esta técnica también puede usarse para reemplazar servicios previamente insertados en una página. La siguiente ilustración muestra todos los campos disponibles en la página que se utiliza en el primer ejemplo:

![Menú contextual de IntelliSense en una lista de campos de Html, componente, StatsService y dirección Url del símbolo @](dependency-injection/_static/razor-fields.png)

Como puede ver, los campos predeterminados incluyen `Html`, `Component`, y `Url` (así como el `StatsService` que hemos insertado). Si para la instancia que desea reemplazar las aplicaciones auxiliares HTML de forma predeterminada por el suyo propio, puede hacerlo fácilmente mediante `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Si desea ampliar los servicios existentes, simplemente puede utilizar esta técnica al heredar o ajuste la implementación existente por el suyo propio.

## <a name="see-also"></a>Vea también

* Blog de Simon Timms: [obtener datos de búsqueda en la vista](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
