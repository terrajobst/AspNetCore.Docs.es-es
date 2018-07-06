---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: Editar formularios y plantillas | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 5 cubre editar formularios y plantillas.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: acb4a4c427870e375ff19823f0bdfa208438e899
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836006"
---
<a name="part-5-edit-forms-and-templating"></a>Parte 5: Editar formularios y plantillas
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 5 cubre editar formularios y plantillas.


En el capítulo anterior, nos estábamos al cargar datos de nuestra base de datos y mostrarlos. En este capítulo, le permitiremos también editar los datos.

## <a name="creating-the-storemanagercontroller"></a>Crear el StoreManagerController

Empezaremos creando un nuevo controlador denominado **StoreManagerController**. Para este controlador, se toma las ventajas de las características de Scaffolding de ASP.NET MVC 3 Tools Update. Establecer las opciones para el cuadro de diálogo Agregar controlador tal como se muestra a continuación.

![](mvc-music-store-part-5/_static/image1.png)

Al hacer clic en el botón Agregar, verá que el mecanismo de scaffolding de ASP.NET MVC 3 hace una buena cantidad de trabajo por usted:

- Crea el nuevo StoreManagerController con una variable local de Entity Framework
- Se agrega una carpeta StoreManager a la carpeta Views del proyecto
- Agrega la vista Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml e Index.cshtml, fuertemente tipada a la clase de álbum

![](mvc-music-store-part-5/_static/image2.png)

La nueva clase de controlador StoreManager incluye CRUD (crear, leer, actualizar y eliminar) las acciones de controlador que saben cómo trabajar con el álbum, clase de modelo y usar nuestro contexto de Entity Framework para el acceso a la base de datos.

## <a name="modifying-a-scaffolded-view"></a>Modificación de una vista con scaffold

Es importante recordar que, aunque este código fue generado por nosotros, es código estándar de ASP.NET MVC, al igual que nos hemos estado escribiendo a lo largo de este tutorial. Se ha diseñado para ahorrar el tiempo que pasaría en escribir código de controlador reutilizable y creación manual de las vistas fuertemente tipadas, pero esto no es el tipo del código generado puede haber visto precedido advertencias nefastas en los comentarios acerca de cómo no debe cambiar la código. Este es su código y se espera que cambiarlo.

Por lo tanto, vamos a comenzar con una edición rápida a la vista de índice StoreManager (/ Views/StoreManager/Index.cshtml). Esta vista muestra una tabla que enumera los álbumes en nuestra tienda con Editar / detalles / eliminar vínculos e incluye las propiedades públicas del álbum. Se quitará el campo AlbumArtUrl, ya que no es muy útil en esta presentación. En &lt;tabla&gt; sección del código de vista, quite el &lt;th&gt; y &lt;td&gt; elementos relacionados con referencias AlbumArtUrl, según se indica en las líneas resaltadas a continuación:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

El código de vista modificado aparecerá como sigue:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Un primer vistazo en el Administrador de Store

Ahora ejecute la aplicación y vaya a/StoreManager /. Esto muestra el índice de administrador de Store hemos modificado, que muestra una lista de los álbumes en el almacén con vínculos a Edit, Details y Delete.

![](mvc-music-store-part-5/_static/image3.png)

Al hacer clic en el vínculo de edición, se muestra un formulario de edición con campos para el álbum, incluidas las listas desplegables para el género y el intérprete.

![](mvc-music-store-part-5/_static/image4.png)

Haga clic en el vínculo "Volver a la lista" en la parte inferior y, a continuación, haga clic en el vínculo detalles de un álbum. Esto muestra la información de detalle de un álbum individual.

![](mvc-music-store-part-5/_static/image5.png)

De nuevo, haga clic en el vínculo volver al lista, a continuación, haga clic en un vínculo de eliminación. Esto muestra un cuadro de diálogo de confirmación, que muestra los detalles del álbum y le pregunta si se está seguro de que desea eliminarlo.

![](mvc-music-store-part-5/_static/image6.png)

Al hacer clic en el botón Eliminar en la parte inferior se elimina el álbum y volver a la página de índice, que muestra el álbum eliminado.

No hemos terminado con el Administrador de Store, pero tenemos que trabajar el controlador y vista de código para las operaciones CRUD iniciar desde.

## <a name="looking-at-the-store-manager-controller-code"></a>Examinar el código de controlador de Store Manager

El controlador de Store Manager contiene una gran cantidad de código. Analicemos esto de arriba a abajo. El controlador incluye algunos espacios de nombres estándar para un controlador MVC, así como una referencia a nuestro espacio de nombres de los modelos. El controlador tiene una instancia privada de MusicStoreEntities, utilizado por cada una de las acciones de controlador para el acceso a datos.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Store Manager Index y Details acciones

La vista de índice recupera una lista de álbumes, incluida la información que se hace referencia de género y el intérprete de cada álbum, como hemos visto anteriormente, cuando se trabaja en el método Store Examinar. La vista de índice sigue las referencias a los objetos vinculados para que pueda mostrar cada álbum nombre género y el nombre del intérprete, por lo que el controlador se va a consultar esta información en la solicitud original y eficaz.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Acción del controlador del controlador StoreManager detalles funciona exactamente igual que la acción de detalles del controlador de Store hemos escrito anteriormente - consulta el álbum por Id. mediante el método Find(), a continuación, lo devuelve a la vista.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>La creación de los métodos de acción

Los métodos de acción Create son un poco diferentes de las que hemos visto hasta ahora, ya que controlan la entrada de formulario. Cuando un usuario visita primero /StoreManager/crear / se mostrará un formulario vacío. Esta página HTML contendrá un &lt;formulario&gt; elementos donde pueden escribir los detalles del álbum de entrada del elemento que contiene la lista desplegable y cuadro de texto.

Después de que el usuario rellena los valores de formulario de álbum, presione el botón "Guardar" para enviar que estos cambios de vuelta a nuestra aplicación para guardar en la base de datos. Cuando el usuario presiona el botón "Guardar" la &lt;formulario&gt; realizará una solicitud HTTP POST a la URL/crear//StoreManager y enviar el &lt;formulario&gt; valores como parte de la operación HTTP POST.

ASP.NET MVC nos permite dividir fácilmente la lógica de estos dos escenarios de invocación de la dirección URL, por lo que nos permite implementar dos métodos de acción "Crear" independientes dentro de nuestra clase StoreManagerController: uno para controlar el inicial HTTP-GET, vaya a la /StoreManager/Create / Dirección URL y otro para controlar el método HTTP POST de los cambios enviados.

### <a name="passing-information-to-a-view-using-viewbag"></a>Pasar información a una vista con ViewBag

Se ha usado el elemento ViewBag anteriormente en este tutorial, pero aún no lo ha hablado mucho sobre él. El elemento ViewBag nos permite pasar información a la vista sin usar un objeto de modelo fuertemente tipado. En este caso, la acción de controlador HTTP-GET editar necesita pasar una lista de géneros y artistas al formulario para rellenar las listas desplegables y la manera más sencilla de hacerlo es devolverlos como elementos ViewBag.

El elemento ViewBag es un objeto dinámico, lo que significa que puede escribir ViewBag.Foo o ViewBag.YourNameHere sin necesidad de escribir código para definir las propiedades. En este caso, el código de controlador usa ViewBag.GenreId y ViewBag.ArtistId para que los valores de lista desplegable enviados con el formulario será GenreId y ArtistId, que son las propiedades de álbum que va a establecer.

Estos valores de lista desplegable se devuelven al formulario mediante el objeto SelectList, que es creado solo para ese propósito. Esto se realiza mediante código similar al siguiente:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Como puede ver en el código del método de acción, los tres parámetros se emplean para crear este objeto:

- La lista de elementos que se va a mostrar la lista desplegable. Tenga en cuenta que esto no es simplemente una cadena, estamos pasando una lista de géneros.
- El siguiente parámetro que se pasan a la SelectList es el valor seleccionado. Este cómo el SelectList sabe cómo seleccionar previamente un elemento en la lista. Esto será más fácil de entender cuando nos centramos en el formulario de edición, que es bastante similar.
- El parámetro final es la propiedad que se mostrará. En este caso, esto indica que la propiedad Genre.Name es lo que se mostrará al usuario.

Con eso en mente, a continuación, la acción Create HTTP-GET es bastante simple: se agregan dos SelectLists a ViewBag y ningún objeto de modelo se pasa al formulario (ya que no se ha creado todavía).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Aplicaciones auxiliares HTML para mostrar las listas desplegables de colocar en la vista crear

Puesto que hemos hablado sobre cómo la lista desplegable de valores se pasan a la vista, echemos un vistazo rápido a la vista para ver cómo se muestran los valores. En el código de vista (/ Views/StoreManager/Create.cshtml), verá que se realiza la llamada siguiente para mostrar la lista de género hacia abajo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Esto se conoce como una aplicación auxiliar HTML: un método de utilidad que lleva a cabo una tarea común de vista. Las aplicaciones auxiliares HTML son muy útiles de mantener nuestra ver código conciso y legible. La aplicación auxiliar de Html.DropDownList proporcionada por ASP.NET MVC, pero como verá más adelante es posible crear nuestras propio aplicaciones auxiliares para ver código, que se podrá volver a usar en nuestra aplicación.

La llamada Html.DropDownList solo debe indicarse las dos cosas: dónde conviene get la lista para mostrar y qué valor (si existe) debe estar preseleccionado. El primer parámetro, GenreId, indica la DropDownList para buscar un valor denominado GenreId en el modelo o ViewBag. El segundo parámetro se utiliza para indicar el valor para mostrar inicialmente seleccionada en la lista desplegable. Puesto que este formulario es un formulario de creación, no hay ningún valor para ser preseleccionada y se pasa String.Empty.

### <a name="handling-the-posted-form-values"></a>Controlar los valores de formulario publicados

Como se explicó antes, hay dos métodos de acción asociados con cada forma. La primera gestiona la solicitud HTTP-GET y muestra el formulario. El segundo administra la solicitud HTTP-POST, que contiene los valores de formulario enviado. Tenga en cuenta que la acción de controlador tiene un atributo [HttpPost], lo que indica a ASP.NET MVC que solo debe responder a solicitudes HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Esta acción tiene cuatro responsabilidades:

- 1. Leer los valores de formulario
- 2. Compruebe si los valores de formulario pasan cualquier regla de validación
- 3. Si el envío del formulario es válido, guardar los datos y mostrar la lista actualizada
- 4. Si el envío del formulario no es válido, volver a mostrar el formulario con errores de validación

#### <a name="reading-form-values-with-model-binding"></a>Leer valores de formulario con el enlace de modelos

La acción de controlador está procesando un envío de formulario que incluya valores GenreId y ArtistId (en la lista desplegable) y los valores del cuadro de texto de título, precio y AlbumArtUrl. Aunque es posible acceder directamente a los valores del formulario, un mejor enfoque es usar las capacidades de enlace de modelos integradas en ASP.NET MVC. Cuando una acción de controlador toma un tipo de modelo como un parámetro, ASP.NET MVC intentará rellenar un objeto de ese tipo mediante entradas de formulario (así como los valores de ruta y la cadena de consulta). Esto lo consigue buscando valores cuyos nombres coincidan con las propiedades del modelo de objetos, por ejemplo, al establecer el valor de álbum del nuevo objeto GenreId, busca una entrada con el nombre GenreId. Al crear vistas utilizando los métodos estándares de ASP.NET MVC, los formularios siempre se representará mediante los nombres de propiedad como nombres de campo de entrada, por lo que los nombres de campo sólo coincidirá.

#### <a name="validating-the-model"></a>Validar el modelo

El modelo se valida con una simple llamada a ModelState.IsValid. Las reglas de validación no hayamos agregado a nuestra clase álbum aún - lo haremos en un poco, así que ahora esta comprobación no tiene mucho que ver. Lo importante es que esta comprobación ModelStat.IsValid se adaptará a las reglas de validación que se colocaron en nuestro modelo, por lo que los cambios futuros en las reglas de validación no requieren ninguna actualización en el código de acción de controlador.

#### <a name="saving-the-submitted-values"></a>Guardar los valores enviados

Si el envío del formulario pasa la validación, es momento de guardar los valores en la base de datos. Con Entity Framework, que solo necesita agregar el modelo a la colección de álbumes y llamar a SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework genera los comandos SQL adecuados para conservar el valor. Después de guardar los datos, se redireccionar a la lista de álbumes para que podamos ver nuestra actualización. Esto se hace devolviendo RedirectToAction con el nombre de la acción del controlador que desea mostrar. En este caso, es el método de índice.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Visualización de los envíos de formularios no válido con errores de validación

En el caso de entrada de formulario no válido, se agregan los valores de lista desplegable para el elemento ViewBag (como en el caso de HTTP GET) y se pasan los valores del modelo enlazado a la vista para su presentación. Errores de validación se muestran automáticamente mediante el @Html.ValidationMessageFor aplicación auxiliar HTML.

#### <a name="testing-the-create-form"></a>Probar el formulario de creación

Para probar esto, ejecute la aplicación y vaya a /StoreManager/crear / - Esto mostrará el formulario en blanco que se devolvió el método StoreController crear HTTP-GET.

Rellene algunos valores y haga clic en el botón Crear para enviar el formulario.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Control de cambios

El par de acción de edición (HTTP-GET y HTTP-POST) es muy similar a los métodos de acción Create que acabamos de ver. Puesto que el escenario de edición implica trabajar con un álbum existente, el método carga el álbum basándose en el parámetro "id", de edición que HTTP-GET pasa a través de la ruta. Este código para recuperar un álbum de AlbumId es el mismo que anteriormente hemos visto en la acción de controlador de detalles. Igual que con la creación o el método GET de HTTP, la lista desplegable de valores se devuelven mediante el elemento ViewBag. Esto nos permite devolver un álbum como el objeto del modelo a la vista (que está fuertemente tipada a la clase de álbum) al pasar datos adicionales (por ejemplo, una lista de géneros) mediante el elemento ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

La acción de editar HTTP-POST es muy similar a la acción Create HTTP-POST. La única diferencia es en lugar de agregar un nuevo álbum de la base de datos. Colección de álbumes, estamos encontrando la instancia actual del álbum mediante la base de datos. Entry(Album) y establecer su estado en Modified. Esto indica a Entity Framework que se está modificando un álbum existente en lugar de crear uno nuevo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Podemos probar esto mediante la ejecución de la aplicación y vaya a/StoreManger /, al hacer clic en el vínculo de edición para un álbum.

![](mvc-music-store-part-5/_static/image9.png)

Esto muestra el formulario de edición que se muestra el método GET de HTTP editar. Rellene algunos valores y haga clic en el botón Guardar.

![](mvc-music-store-part-5/_static/image10.png)

Esto envía el formulario, guarda los valores y nos devuelve a la lista de álbumes, que muestra que se actualizaron los valores.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Eliminación de control

Eliminación sigue el mismo patrón que editar y crear, mediante un controlador acción para mostrar el formulario de confirmación y otra acción de controlador para controlar el envío del formulario.

La acción del controlador HTTP-GET Delete es exactamente igual que la acción de controlador Store Manager detalles anterior.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Se muestra un formulario que está fuertemente tipado a un tipo de álbum, mediante la plantilla de contenido de la vista de eliminación.

![](mvc-music-store-part-5/_static/image12.png)

La plantilla de eliminación muestra todos los campos para el modelo, pero podemos simplificarlo un poco abajo. Cambie el código de vista de /Views/StoreManager/Delete.cshtml al siguiente.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Esto muestra una confirmación de eliminación simplificada.

![](mvc-music-store-part-5/_static/image13.png)

Al hacer clic en el botón Eliminar hace que el formulario se envía al servidor, que ejecuta la acción DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

La acción de controlador HTTP-POST eliminar realiza las siguientes acciones:

- 1. Carga el álbum por Id.
- 2. Elimina el álbum y guarde los cambios
- 3. Redirige al índice, que muestra que se quitó el álbum de la lista

Para probar esto, ejecute la aplicación y vaya a /StoreManager. Seleccione un álbum de la lista y haga clic en el vínculo Eliminar.

![](mvc-music-store-part-5/_static/image14.png)

Esto muestra que nuestra pantalla de confirmación de eliminación.

![](mvc-music-store-part-5/_static/image15.png)

Al hacer clic en el botón Eliminar quita el álbum y nos devuelve a la página de índice de Store Manager, que muestra que se ha eliminado el álbum.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Uso de una aplicación auxiliar HTML personalizada para truncar el texto

Tenemos un problema potencial con nuestra página de índice de Store Manager. Nuestras propiedades del título del álbum y el nombre del intérprete tanto pueden ser lo suficientemente largo que se producen fuera de nuestro formato de tabla. Vamos a crear una aplicación auxiliar HTML personalizada para permitirnos truncar fácilmente estas y otras propiedades en nuestras vistas.

![](mvc-music-store-part-5/_static/image17.png)

De Razor @helper sintaxis hizo bastante fácil crear sus propias funciones auxiliares para su uso en las vistas. Abra la vista /Views/StoreManager/Index.cshtml y agregue el código siguiente directamente después del @model línea.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Este método auxiliar toma una cadena y una longitud máxima para permitir. Si el texto proporcionado es menor que la longitud especificada, la aplicación auxiliarlo envía como-es. Si es mayor, se trunca el texto y representa "..." para el resto.

Ahora podemos usar nuestra aplicación auxiliar de truncamiento para asegurarse de que las propiedades del título del álbum y el nombre del artista son menos de 25 caracteres. El código de la visión completa con nuestra nueva aplicación auxiliar de truncar aparece debajo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Ahora cuando se examina la dirección URL de /StoreManager/, los álbumes y títulos porque se mantenga por debajo de nuestro longitudes máximas.

![](mvc-music-store-part-5/_static/image18.png)

Nota: Esto muestra un caso simple de crear y usar una aplicación auxiliar en una vista. Para más información acerca de cómo crear aplicaciones auxiliares que puede usar en todo el sitio, consulte la entrada de mi blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-4.md)
> [Siguiente](mvc-music-store-part-6.md)
