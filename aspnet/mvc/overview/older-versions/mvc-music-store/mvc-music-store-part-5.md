---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: Editar formularios y plantillas | Documentos de Microsoft'
author: jongalloway
description: "Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 5 cubre editar formularios y plantillas."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: cde6fe133291254531a797a434a4b2cdd226dd5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-5-edit-forms-and-templating"></a>Parte 5: Editar formularios y plantillas
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 5 cubre editar formularios y plantillas.


En el capítulo anterior, se nos ha cargar datos de nuestra base de datos y mostrarlos. En este capítulo, se le permiten modificar los datos.

## <a name="creating-the-storemanagercontroller"></a>Crear el StoreManagerController

Comenzaremos creando un nuevo controlador denominado **StoreManagerController**. Para este controlador, se puede sacar partido de las las características de Scaffolding disponibles en ASP.NET MVC 3 Tools Update. Establecer las opciones para el cuadro de diálogo Agregar controlador tal y como se muestra a continuación.

![](mvc-music-store-part-5/_static/image1.png)

Al hacer clic en el botón Agregar, verá que el mecanismo de scaffolding de ASP.NET MVC 3 realiza una gran cantidad de trabajo automáticamente:

- Crea el nuevo StoreManagerController con una variable local de Entity Framework
- Agrega una carpeta StoreManager a la carpeta del proyecto vistas
- Agrega la vista Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml y Index.cshtml, establecimiento inflexible de tipos a la clase de álbum

![](mvc-music-store-part-5/_static/image2.png)

La nueva clase de controlador de StoreManager incluye CRUD (crear, leer, actualizar y eliminar) acciones del controlador que saben cómo trabajar con el álbum, clase de modelo y usar nuestro contexto de Entity Framework para el acceso a la base de datos.

## <a name="modifying-a-scaffolded-view"></a>Modificación de una vista con scaffolding

Es importante recordar que, mientras que generó este código para que podamos, es código de MVC de ASP.NET estándar, tal como se ha estado escribiendo a lo largo de este tutorial. Se ha diseñado para ahorrar el tiempo que tardaría en hacerlo en escribir código repetitivo del controlador y la creación manual de las vistas fuertemente tipadas, pero esto no es del tipo de código generado es podrán que haya visto prologa con advertencias nefastas en comentarios acerca de cómo no debe cambiar el código. Éste es el código y se espera que cambiarlo.

Por lo tanto, puede empezar con una operación de edición a la vista de índice StoreManager rápida (/ Views/StoreManager/Index.cshtml). Esta vista muestra una tabla que enumera los álbumes en nuestra tienda edite / detalles / eliminar vínculos e incluye las propiedades públicas del álbum. Se quitará el campo AlbumArtUrl, ya que no es muy útil en esta presentación. En &lt;tabla&gt; sección del código de la vista, quite el &lt;th&gt; y &lt;td&gt; elementos que rodean las referencias de AlbumArtUrl, como se indica en las líneas resaltadas a continuación:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

El código de la vista modificada aparecerá como sigue:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Un primer vistazo a Store Manager

Ahora ejecute la aplicación y busque/StoreManager /. Esto muestra el índice de administrador de almacén que modificamos, que muestra una lista de los álbumes en el almacén con vínculos para obtener más información, editar y eliminar.

![](mvc-music-store-part-5/_static/image3.png)

Al hacer clic en el vínculo de edición, se muestra un formulario con campos de edición para el álbum, incluida listas desplegables de género y el intérprete.

![](mvc-music-store-part-5/_static/image4.png)

Haga clic en el vínculo "Volver a la lista" en la parte inferior, a continuación, haga clic en el vínculo detalles de un álbum. Esto muestra la información de detalle de un álbum individual.

![](mvc-music-store-part-5/_static/image5.png)

De nuevo, haga clic en la parte posterior para el vínculo de la lista, a continuación, haga clic en un vínculo de eliminación. Esto muestra un cuadro de diálogo de confirmación, que muestra los detalles del álbum y le pregunta si se está seguro de que desea eliminarlo.

![](mvc-music-store-part-5/_static/image6.png)

Haga clic en el botón Eliminar en la parte inferior eliminará el álbum y volver a la página de índice, que se muestra en el álbum eliminado.

No terminamos con el administrador del almacén, pero tenemos que funciona el controlador y el código de la vista para las operaciones CRUD iniciar desde.

## <a name="looking-at-the-store-manager-controller-code"></a>Examinando el código de controlador de administrador de almacén

El controlador de administrador de almacén contiene una gran cantidad de código. Analicemos esto de arriba a abajo. El controlador incluye algunos espacios de nombres estándar para un controlador MVC, así como una referencia a nuestro espacio de nombres de modelos. El controlador tiene una instancia privada de MusicStoreEntities, utilizado por cada una de las acciones de controlador para el acceso a datos.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Almacenar acciones de administrador de índice y detalles

La vista de índice recupera una lista de álbumes, incluida la información que se hace referencia de género y el intérprete del cada álbum, tal y como hemos visto anteriormente cuando se trabaja en el método de examinar el almacén. La vista de índice es posterior a las referencias a los objetos vinculados para que pueda mostrar cada álbum nombre género y el nombre del intérprete, por lo que el controlador está siendo eficaz y consultar esta información en la solicitud original.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Acción de controlador de detalles del controlador StoreManager funciona exactamente igual que la acción de detalles del controlador de almacén indicamos previamente - consulta el álbum con el identificador mediante el método Find(), a continuación, lo devuelve a la vista.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>El crear métodos de acción

Los métodos de acción de crear son un poco diferentes de los que hemos visto hasta ahora, ya que controlan una entrada de formulario. Cuando un usuario visita por primera vez /StoreManager/crear / se mostrará un formulario vacío. Esta página HTML contendrá un &lt;formulario&gt; elementos en el que pueden especificar los detalles del álbum de entrada del elemento que contiene la lista desplegable y cuadro de texto.

Después de que el usuario rellena los valores de formulario de álbum, presione el botón "Guardar" para enviar que estos cambios de nuevo a nuestra aplicación para guardar en la base de datos. Cuando el usuario presiona el botón "Guardar" el &lt;formulario&gt; realizará una solicitud HTTP POST a la dirección /StoreManager/crear/URL y enviar el &lt;formulario&gt; valores como parte de la publicación HTTP.

ASP.NET MVC permite dividir fácilmente la lógica de los dos escenarios de invocación de dirección URL, por lo que nos permite implementar dos métodos de acción "Crear" independientes dentro de nuestra clase StoreManagerController: uno para controlar el HTTP-GET inicial, examine el /StoreManager/Create / URL y la otra para controlar el HTTP-POST de los cambios enviados.

### <a name="passing-information-to-a-view-using-viewbag"></a>Pasar información a una vista con ViewBag

Se ha usado el elemento ViewBag anteriormente en este tutorial, pero aún no lo ha hablado mucho sobre él. El elemento ViewBag nos permite pasar información a la vista sin usar un objeto de modelo fuertemente tipado. En este caso, la acción del controlador HTTP-GET editar necesita pasar una lista de géneros y artistas al formulario para rellenar las listas desplegables y la manera más sencilla de hacerlo es devolverlos como elementos de elemento ViewBag.

El elemento ViewBag es un objeto dinámico, lo que significa que puede escribir ViewBag.Foo o ViewBag.YourNameHere sin tener que escribir código para definir las propiedades. En este caso, el código del controlador utiliza ViewBag.GenreId y ViewBag.ArtistId para que los valores de lista desplegable que se envía con el formulario serán GenreId y ArtistId, que son las propiedades de álbum que va a establecer.

Estos valores de lista desplegable se devuelven al formulario utilizando el objeto SelectList, que se basa solo para ese fin. Esto se realiza mediante código similar al siguiente:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Como puede ver desde el código del método de acción, tres parámetros que se sirven para crear este objeto:

- La lista de elementos que se va a mostrar la lista desplegable. Tenga en cuenta que esto no es una cadena, estamos pasando una lista de géneros.
- El siguiente parámetro que se pasan a la SelectList es el valor seleccionado. Este modo el SelectList sabe cómo previamente seleccione un elemento en la lista. Esto será más fácil de entender, cuando se examina en el formulario de edición, que es muy similar.
- El último parámetro es la propiedad que se mostrará. En este caso, esto es que indica que la propiedad Genre.Name es lo que se mostrará al usuario.

Teniendo esto en cuenta, a continuación, la acción Create HTTP-GET es bastante sencilla: dos SelectLists se agregan al elemento ViewBag y ningún objeto de modelo se pasa al formulario (ya que no se ha creado todavía).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Aplicaciones auxiliares HTML para mostrar las exploraciones le Drop en Create View

Puesto que hemos hablado acerca de cómo la lista desplegable de valores se pasan a la vista, echemos un vistazo rápido a la vista para ver cómo se muestran los valores. En el código de vista (/ Views/StoreManager/Create.cshtml), verá que se realiza la llamada siguiente para mostrar la lista de género hacia abajo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Esto se conoce como una aplicación auxiliar de HTML: un método de utilidad que lleva a cabo una tarea común de la vista. Aplicaciones auxiliares HTML son muy útiles en la conservación de nuestro código vista concisas y legibles. ASP.NET MVC proporciona la aplicación auxiliar Html.DropDownList pero, como verá más adelante es posible crear nuestras propia aplicaciones auxiliares para el código de la vista que se podrá volver a usar en nuestra aplicación.

La llamada Html.DropDownList solo debe indicarse dos cosas: dónde conviene get de la lista para mostrar y qué valor (si existe) debe estar seleccionado previamente. El primer parámetro, GenreId, indica la DropDownList para buscar un valor denominado GenreId en el modelo o el elemento ViewBag. El segundo parámetro se utiliza para indicar el valor para mostrar como inicialmente se seleccionan en la lista desplegable. Puesto que este formulario es una forma de crear, no hay ningún valor para ser preseleccionadas y String.Empty se ha pasado.

### <a name="handling-the-posted-form-values"></a>Controlar los valores de formulario expuestos

Como se explicó antes, hay dos métodos de acción asociados con cada forma. El primero controla la solicitud HTTP GET y muestra el formulario. El segundo administra la solicitud HTTP-POST, que contiene los valores de formulario enviado. Tenga en cuenta que la acción de controlador tiene un atributo [HttpPost], que indica a ASP.NET MVC que solo debería responder a las solicitudes HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Esta acción tiene cuatro responsabilidades:

- 1. Leer los valores de formulario
- 2. Compruebe si los valores del formulario pasan cualquier regla de validación
- 3. Si el envío del formulario es válido, guardar los datos y mostrar la lista actualizada
- 4. Si el envío del formulario no es válido, volver a mostrar el formulario con errores de validación

#### <a name="reading-form-values-with-model-binding"></a>Leer valores de formulario con el enlace de modelos

La acción del controlador está procesando el envío de un formulario que incluya los valores para GenreId y ArtistId (en la lista desplegable) y valores de cuadro de texto de título, el precio y AlbumArtUrl. Aunque es posible tener acceso directo a los valores del formulario, un mejor enfoque es usar las capacidades de enlace de modelos integradas en ASP.NET MVC. Cuando una acción de controlador toma un tipo de modelo como un parámetro, ASP.NET MVC intentará rellenar un objeto de ese tipo con entradas de formulario (así como los valores de ruta y la cadena de consulta). Esto consigue mediante buscando valores cuyos nombres coincidan con las propiedades del objeto de modelo, por ejemplo, al establecer el nuevo álbum valor del objeto GenreId, busca una entrada con el nombre GenreId. Al crear vistas utilizando los métodos estándares en ASP.NET MVC, los formularios siempre se representará mediante nombres de propiedad como nombres de campo de entrada, por lo que esto los nombres de campo sólo coincidirá con.

#### <a name="validating-the-model"></a>Validar el modelo

El modelo se valida con una llamada simple a ModelState.IsValid. No hemos agregamos cualquier regla de validación a nuestra clase álbum aún - haremos todo lo que en un bit, así que ahora esta comprobación no tiene mucho que hacer. Lo importante es que esta comprobación ModelStat.IsValid adaptará a las reglas de validación que colocamos en nuestro modelo, por lo que los futuros cambios en las reglas de validación no requieren actualizaciones para el código de acción del controlador.

#### <a name="saving-the-submitted-values"></a>Guardar los valores enviados

Si el envío del formulario pasa la validación, es el momento de guardar los valores en la base de datos. Con Entity Framework, que solo requiere agregar el modelo a la colección de álbumes y una llamada a SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework genera los comandos SQL adecuados para conservar el valor. Después de guardar los datos, se redireccionar a la lista de álbumes, vemos la actualización. Para ello, devolver RedirectToAction con el nombre de la acción de controlador que desea mostrar. En este caso, es el método de índice.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Mostrar los envíos de formulario no válido con errores de validación

En el caso de entrada de formulario no válido, se agregan los valores de lista desplegable para el elemento ViewBag (como en el caso de HTTP GET) y los valores del modelo dependiente se pasan a la vista para mostrar. Errores de validación se muestran automáticamente mediante el @Html.ValidationMessageFor aplicación auxiliar HTML.

#### <a name="testing-the-create-form"></a>Probar el formulario de creación

Para probar esto, ejecute la aplicación y vaya a /StoreManager/crear / - Esto mostrará el formulario en blanco que se ha devuelto por el método StoreController crear HTTP-GET.

Rellene algunos valores y haga clic en el botón Crear para enviar el formulario.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Control de cambios

El par de acciones de edición (HTTP-GET y HTTP-POST) es muy similar a los métodos de acción de crear que acabamos de ver. Puesto que el escenario de edición implica trabajar con un álbum existente, el método carga el álbum basándose en el parámetro "id", de edición que HTTP-GET pasa a través de la ruta. Este código para recuperar un álbum por AlbumId es el mismo como, hemos analizado anteriormente en la acción de controlador de detalles. Al igual que con la función Create / método GET de HTTP, la lista desplegable de valores se devuelven a través de ViewBag. Esto nos permite devolver un álbum como nuestro objeto de modelo a la vista (que está fuertemente tipada en la clase de álbum) al pasar datos adicionales (por ejemplo, una lista de géneros) mediante el elemento ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

La acción de editar HTTP-POST es muy similar a la acción Create HTTP-POST. La única diferencia es en lugar de agregar un nuevo álbum a la base de datos. Colección de álbumes, estamos encontrando la instancia actual del álbum mediante la base de datos. Entry(Album) y establecer su estado como modificado. Esto indica a Entity Framework que se está modificando un álbum existente en lugar de crear uno nuevo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Podemos probar este comando al ejecutar la aplicación y examinar/StoreManger/y, a continuación, haga clic en el vínculo de edición para un álbum.

![](mvc-music-store-part-5/_static/image9.png)

Esto muestra el formulario de edición que se muestra en el método GET de HTTP editar. Rellene algunos valores y haga clic en el botón Guardar.

![](mvc-music-store-part-5/_static/image10.png)

Esto envía el formulario, guarda los valores y nos devuelve a la lista de álbumes, que muestra que se actualizaron los valores.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Control de eliminación

Eliminación sigue el mismo patrón que editar y crear, mediante la acción de un controlador para mostrar el formulario de confirmación y otra acción de controlador para controlar el envío del formulario.

La acción de controlador HTTP-GET Delete es exactamente igual que la acción de controlador de detalles del Administrador de almacén anterior.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Se muestra un formulario que está fuertemente tipado para un tipo de álbum, mediante la plantilla de contenido de la vista de eliminación.

![](mvc-music-store-part-5/_static/image12.png)

La plantilla de eliminación muestra todos los campos para el modelo, pero podemos simplificar un poco ese abajo. Cambie el código de vista de /Views/StoreManager/Delete.cshtml al siguiente.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Esto muestra una confirmación de eliminación simplificada.

![](mvc-music-store-part-5/_static/image13.png)

Haga clic en el botón Eliminar hace que el formulario se envía al servidor, que ejecuta la acción DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

La acción de controlador HTTP-POST eliminar realiza las siguientes acciones:

- 1. Carga el álbum por Id.
- 2. Elimina el álbum y guardar los cambios
- 3. Redirige al índice, que muestra que se quitó el álbum de la lista

Para ello, ejecute la aplicación y vaya a /StoreManager. Seleccione un álbum de la lista y haga clic en el vínculo Eliminar.

![](mvc-music-store-part-5/_static/image14.png)

Esto muestra la pantalla de confirmación de eliminación.

![](mvc-music-store-part-5/_static/image15.png)

Haga clic en el botón Eliminar quita el álbum y nos devuelve a la página de índice del Administrador de almacenamiento, que muestra que se ha eliminado el álbum.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Uso de una aplicación auxiliar HTML personalizado para truncar el texto

Tenemos un problema potencial con nuestra página de índice del Administrador de almacén. Nuestro propiedades del título del álbum y el nombre del intérprete tanto pueden ser lo suficientemente largo que se producen fuera de nuestro formato de tabla. Vamos a crear una aplicación auxiliar de HTML personalizado para permitir que nos truncar fácilmente estas y otras propiedades en nuestros puntos de vista.

![](mvc-music-store-part-5/_static/image17.png)

De Razor @helper sintaxis hizo bastante fácil de crear sus propias funciones auxiliares para su uso en las vistas. Abra la vista /Views/StoreManager/Index.cshtml y agregue el código siguiente directamente después de la @model línea.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Este método auxiliar toma una cadena y una longitud máxima para permitir. Si el texto proporcionado es menor que la longitud especificada, la aplicación auxiliar genera como-es. Si es más larga, se trunca el texto y representa "..." para el resto.

Ahora podemos usar nuestro truncar auxiliar para asegurarse de que el título del álbum y el nombre del intérprete propiedades son menos de 25 caracteres. El código de la vista completa con nuestro nuevo auxiliar de truncar aparece debajo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Ahora cuando se examina la dirección URL /StoreManager/, los álbumes y títulos se mantengan por debajo de la longitud máxima.

![](mvc-music-store-part-5/_static/image18.png)

Nota: Esto muestra el caso más sencillo de crear y usar una aplicación auxiliar en una vista. Para más información acerca de cómo crear aplicaciones auxiliares que puede utilizar en todo el sitio, consulte mi entrada de blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


>[!div class="step-by-step"]
[Anterior](mvc-music-store-part-4.md)
[Siguiente](mvc-music-store-part-6.md)
