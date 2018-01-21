---
title: Componentes de la vista
author: rick-anderson
description: "Componentes de la vista pretenden en cualquier lugar que tiene lógica de representación reutilizable."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 2d93dcee102009661af708b9a9066e8af0bdbb17
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="view-components"></a>Componentes de la vista

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="introducing-view-components"></a>Componentes de la vista de presentación

Nuevo en MVC de ASP.NET Core, ver componentes son similares a las vistas parciales, pero son mucho más eficaces. Componentes de la vista no usar enlace de modelos y sólo dependen de los datos proporcionados al llamar a en él. Un componente de vista:

* Representa un fragmento en lugar de una respuesta toda
* Incluye la misma separación de problemas y los beneficios de capacidad de prueba se encuentra entre un controlador y la vista
* Puede tener parámetros y lógica de negocios
* Normalmente se invoca desde una página de diseño

Componentes de la vista se han diseñado en cualquier lugar que tiene lógica de representación reutilizable que es demasiado compleja para una vista parcial, como:

* Menús de navegación dinámica
* Nube de etiquetas (que consulta la base de datos)
* Panel de inicio de sesión
* Carro de la compra
* Artículos publicados recientemente
* Contenido de Sidebar en un blog típico
* Un panel de inicio de sesión que se representan en cada página y mostrar los vínculos entre ambos para cerrar la sesión o de registro, según el registro de estado del usuario

Un componente de vista consta de dos partes: la clase (normalmente derivado de [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) y lo devuelve (normalmente una vista). Al igual que los controladores, un componente de vista puede ser un POCO, pero la mayoría de los desarrolladores desee aprovechar las ventajas de los métodos y propiedades disponibles derivando de `ViewComponent`.

## <a name="creating-a-view-component"></a>Crear un componente de vista

Esta sección contiene los requisitos de alto nivel para crear un componente de vista. Más adelante en el artículo, se deberá examinar cada paso en detalle y crear un componente de vista.

### <a name="the-view-component-class"></a>La clase de componente de vista

Una clase de componente de vista se puede crear por cualquiera de las siguientes acciones:

* Derivar de *ViewComponent*
* Al decorar una clase con el `[ViewComponent]` atributo o derivar de una clase con el `[ViewComponent]` atributo
* Crear una clase donde el nombre termina con el sufijo *ViewComponent*

Como controladores, componentes de la vista deben ser clases públicas, no anidada y no abstracta. El nombre del componente de vista es el nombre de clase con el sufijo "ViewComponent" quitado. También se puede especificar explícitamente mediante el `ViewComponentAttribute.Name` propiedad.

Una clase de componente de vista:

* Es totalmente compatible con constructor [inyección de dependencia](../../fundamentals/dependency-injection.md)

* No participar en el ciclo de vida de controlador, lo que significa que no se puede utilizar [filtros](../controllers/filters.md) en un componente de vista

### <a name="view-component-methods"></a>Ver los métodos de componente

Un componente de vista define su lógica en una `InvokeAsync` método que devuelve un `IViewComponentResult`. Parámetros proceden directamente de invocación del componente de vista, no desde el enlace de modelos. Un componente de vista nunca directamente controla una solicitud. Por lo general, inicializa un modelo de un componente de vista y la pasa a una vista mediante una llamada a la `View` método. En resumen, ver los métodos de componente:

* Definir una `InvokeAsync` método que devuelva una`IViewComponentResult`
* Inicializa un modelo normalmente y la pasa a una vista mediante una llamada a la `ViewComponent` `View` (método)
* Parámetros provienen del método que realiza la llamada, y no HTTP, no hay ningún enlace de modelo
* Son accesibles directamente como un extremo HTTP, que se invoquen desde el código (normalmente en una vista). Un componente de vista nunca controla una solicitud
* Están sobrecargados en la firma en lugar de los detalles de la solicitud HTTP actual

### <a name="view-search-path"></a>Ruta de acceso de búsqueda de vista

El tiempo de ejecución busca la vista en las rutas de acceso siguientes:

   * Views/\<controller_name>/Components/\<view_component_name>/\<view_name>
   * Views/Shared/Components/\<view_component_name>/\<view_name>

El nombre de vista predeterminado para un componente de vista es *predeterminado*, lo que significa que el archivo de vista se suele denominar *Default.cshtml*. Puede especificar un nombre de vista diferente al crear el resultado del componente de vista o cuando se llama a la `View` método.

Se recomienda que un nombre al archivo de vista *Default.cshtml* y use la *vistas / / componentes de uso compartido/\<view_component_name > /\<view_name >* ruta de acceso. El `PriorityList` componente de vista utilizado en este ejemplo utiliza *Views/Shared/Components/PriorityList/Default.cshtml* para la vista de componentes de la vista.

## <a name="invoking-a-view-component"></a>Invocar un componente de vista

Para utilizar el componente de vista, llame a los siguientes dentro de una vista:

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Los parámetros se pasará a la `InvokeAsync` método. El `PriorityList` desarrollado en el artículo del componente de vista se invoca desde la *Views/Todo/Index.cshtml* ver el archivo. En la siguiente tabla, el `InvokeAsync` se denomina método con dos parámetros:

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Invocar un componente de vista como una aplicación auxiliar de etiqueta

Para ASP.NET Core 1.1 y versiones posteriores, puede invocar un componente de vista como una [etiqueta auxiliar](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Parámetros de clase y método la grafía Pascal para aplicaciones auxiliares de etiquetas se convierten en sus [reducir caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). La aplicación auxiliar de etiquetas para invocar un componente de vista utiliza la `<vc></vc>` elemento. El componente de vista se especifica como sigue:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Nota: Para poder usar un componente de vista como una aplicación auxiliar de etiquetas, debe registrar el ensamblado que contiene el componente de vista mediante el `@addTagHelper` directiva. Por ejemplo, si el componente de la vista está en un ensamblado denominado "MyWebApp", agregue la siguiente directiva a la `_ViewImports.cshtml` archivo:

```cshtml
@addTagHelper *, MyWebApp
```

Puede registrar un componente de vista como una aplicación auxiliar de etiqueta a cualquier archivo que hace referencia al componente de vista. Vea [administración de ámbito de aplicación auxiliar de la etiqueta](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para obtener más información sobre cómo registrar aplicaciones auxiliares de etiquetas.

El `InvokeAsync` método utilizado en este tutorial:

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

En el marcado de la aplicación auxiliar de etiqueta:

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

En el ejemplo anterior, el `PriorityList` del componente vista se convierte en `priority-list`. Los parámetros para el componente de vista se pasan como atributos en minúsculas kebab.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Invocar un componente de vista directamente desde un controlador

Ver componentes normalmente se invocan desde una vista, pero se puede invocar directamente desde un método de controlador. Mientras los componentes de la vista no definen como controladores, puede implementar fácilmente una acción de controlador que devuelve el contenido de un `ViewComponentResult`.

En este ejemplo, el componente de vista se denomina directamente desde el controlador:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Tutorial: Crear un componente de vista simple

[Descargar](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compilar y probar el código de inicio. Es un proyecto simple con un `Todo` controlador que muestra una lista de *tareas* elementos.

![Lista de ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Agregue una clase ViewComponent

Crear un *ViewComponents* carpeta y agregue las siguientes `PriorityListViewComponent` clase:

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Notas sobre el código:

* Clases de componentes de vista pueden estar contenidas en **cualquier** carpeta del proyecto.
* Dado que el nombre de clase PriorityList**ViewComponent** termina con el sufijo **ViewComponent**, el tiempo de ejecución utilizará la cadena "PriorityList" cuando se hace referencia el componente de la clase de una vista. Explicaremos con más detalle más adelante.
* El `[ViewComponent]` atributos pueden cambiar el nombre utilizado para hacer referencia a un componente de vista. Por ejemplo, podríamos podríamos cambiar el nombre la clase `XYZ` y aplica la `ViewComponent` atributo:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* El `[ViewComponent]` atributo anterior indica el selector de componente de vista para utilizar el nombre `PriorityList` al buscar las vistas asociadas con el componente y utilizar la cadena "PriorityList" cuando se hace referencia el componente de la clase de una vista. Explicaremos con más detalle más adelante.
* El componente usa [inyección de dependencia](../../fundamentals/dependency-injection.md) para que esté disponible el contexto de datos.
* `InvokeAsync`expone un método que se puede llamar desde una vista y puede tomar un número arbitrario de argumentos.
* El `InvokeAsync` método devuelve el conjunto de `ToDo` los elementos que cumplen el `isDone` y `maxPriority` parámetros.

### <a name="create-the-view-component-razor-view"></a>Crear la vista de Razor del componente de vista

* Crear el *vistas/Shared/componentes* carpeta. Esta carpeta **debe** denominarse *componentes*.

* Crear el *vistas/Shared/componentes/PriorityList* carpeta. Este nombre de carpeta debe coincidir con el nombre de la clase de componente de vista o el nombre de la clase menos el sufijo (si se siguió convención y usa el *ViewComponent* sufijo en el nombre de clase). Si ha usado la `ViewComponent` atributo, necesitaría el nombre de clase para que coincida con la designación de atributo.

* Crear un *Views/Shared/Components/PriorityList/Default.cshtml* vista Razor:[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   La vista Razor toma una lista de `TodoItem` y los muestra. Si el componente de vista `InvokeAsync` método no pasa el nombre de la vista (como en nuestro ejemplo), *predeterminado* por convención, se utiliza para el nombre de vista. Más adelante en el tutorial, mostraré cómo pasar el nombre de la vista. Para reemplazar el estilo predeterminado de un controlador concreto, agregar una vista a la carpeta de la vista específica del controlador (por ejemplo *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Si el componente de vista es específicas del controlador, puede agregarlo a la carpeta específica del controlador (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Agregar un `div` que contiene una llamada al componente de lista de prioridad para la parte inferior de la *Views/Todo/index.cshtml* archivo:

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

El marcado `@await Component.InvokeAsync` muestra la sintaxis para llamar a los componentes de la vista. El primer argumento es el nombre del componente que desea invocar o llamar a. Los parámetros siguientes se pasan al componente. `InvokeAsync`puede tomar un número arbitrario de argumentos.

Probar la aplicación. La siguiente imagen muestra la lista de tareas y los elementos de prioridad:

![elementos de lista y la prioridad de tareas](view-components/_static/pi.png)

También puede llamar al componente de vista directamente desde el controlador:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![elementos de prioridad de acción IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Especificar un nombre de vista

Un componente de vista complejas que necesite especificar una vista no predeterminado en determinadas condiciones. El código siguiente muestra cómo especificar la vista de "Conexión virtual permanente" desde el `InvokeAsync` método. Actualización de la `InvokeAsync` método en la `PriorityListViewComponent` clase.

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Copia la *Views/Shared/Components/PriorityList/Default.cshtml* archivo a una vista denominada *Views/Shared/Components/PriorityList/PVC.cshtml*. Agregar un encabezado para indicar que se está usando la vista de circuito virtual permanente.

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Actualización *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Ejecute la aplicación y comprobar la vista de circuito virtual permanente.

![Componente de vista de prioridad](view-components/_static/pvc.png)

Si no se representa la vista de circuito virtual permanente, compruebe que está llamando el componente de vista con una prioridad de 4 o posterior.

### <a name="examine-the-view-path"></a>Examine la ruta de acceso de vista

* Cambie el parámetro de prioridad a tres o menos, por lo que no se devuelve la vista priority.
* Cambiar temporalmente el nombre del *Views/Todo/Components/PriorityList/Default.cshtml* a *1Default.cshtml*.
* Probar la aplicación, obtendrá el siguiente error:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Copia *Views/Todo/Components/PriorityList/1Default.cshtml* a *Views/Shared/Components/PriorityList/Default.cshtml*.
* Agregar algún marcado para la *Shared* proviene de la vista de componentes de vista de lista de tareas para indicar la vista la *Shared* carpeta.
* Prueba la **Shared** vista de componentes.

![Salida de la lista de tareas con vista de componentes compartidos](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Evitar cadenas mágicas

Si desea que la seguridad de tiempo de compilación, puede reemplazar el nombre del componente vista codificado de forma rígida con el nombre de clase. Crear el componente de vista sin el sufijo "ViewComponent":

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Agregar un `using` instrucción para su Razor Ver archivo y usar el `nameof` operador:

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>Recursos adicionales

* [Inserción de dependencias en vistas](dependency-injection.md)
