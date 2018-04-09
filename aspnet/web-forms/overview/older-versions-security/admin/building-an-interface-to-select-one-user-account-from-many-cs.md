---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: Creación de una interfaz para seleccionar una cuenta de usuario de muchos (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial crearemos una interfaz de usuario con una cuadrícula paginada, se puede filtrar. En concreto, la interfaz de usuario constará de una serie de LinkButton para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 304505b18e330425ea1dc8df87a552f3d8cd15f3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Creación de una interfaz para seleccionar una cuenta de usuario de muchos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) o [descarga de PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> En este tutorial crearemos una interfaz de usuario con una cuadrícula paginada, se puede filtrar. En concreto, la interfaz de usuario constará de una serie de LinkButton para filtrar los resultados en función de la letra inicial del usuario y un control GridView para mostrar los usuarios coincidentes. Comenzaremos con una lista de todas las cuentas de usuario en un control GridView. A continuación, en el paso 3, agregaremos el filtro LinkButton. Paso 4 se examina los resultados filtrados de paginación. La interfaz que se construyen en los pasos 2 a 4 se utilizará en los tutoriales siguientes para realizar tareas administrativas para una cuenta de usuario determinada.


## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"> </a> [ *asignar Roles a los usuarios* ](../roles/assigning-roles-to-users-cs.md) tutorial, creamos una interfaz rudimentaria para un administrador seleccionar un usuario y administrar sus roles. En concreto, la interfaz aparecerá el Administrador de una lista desplegable de todos los usuarios. Esta interfaz es adecuada cuando hay pero el usuario de una docena de cuentas, pero es difícil de manejar para sitios con cientos o miles de cuentas. Una cuadrícula paginada, se puede filtrar es la interfaz de usuario más adecuada para los sitios Web con bases de usuarios grande.

En este tutorial crearemos una interfaz de usuario de este tipo. En concreto, la interfaz de usuario constará de una serie de LinkButton para filtrar los resultados en función de la letra inicial del usuario y un control GridView para mostrar los usuarios coincidentes. Comenzaremos con una lista de todas las cuentas de usuario en un control GridView. A continuación, en el paso 3, agregaremos el filtro LinkButton. Paso 4 se examina los resultados filtrados de paginación. La interfaz que se construyen en los pasos 2 a 4 se utilizará en los tutoriales siguientes para realizar tareas administrativas para una cuenta de usuario determinada.

Comencemos.

## <a name="step-1-adding-new-aspnet-pages"></a>Paso 1: Agregar nuevas páginas ASP.NET

En este tutorial y los dos siguientes analizaremos diversas funciones relacionadas con la administración y capacidades. Necesitamos una serie de páginas ASP.NET que se va a implementar en los temas que se examinan a lo largo de estos tutoriales. Vamos a crear estas páginas y actualizar el mapa del sitio.

Empiece por crear una nueva carpeta en el proyecto denominado `Administration`. A continuación, agregue dos nuevas páginas ASP.NET a la carpeta, vincular cada página con el `Site.master` página maestra. Nombre de las páginas:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Agregar dos páginas al directorio de raíz del sitio Web: `ChangePassword.aspx` y `RecoverPassword.aspx`.

Estos cuatro páginas en este punto, deberían, tiene dos controles de contenido, uno para cada uno de ContentPlaceHolders la página maestra: `MainContent` y `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Queremos que aparezca el marcado de la página maestra predeterminada para el `LoginContent` ContentPlaceHolder para estas páginas. Por lo tanto, quite el marcado declarativo para la `Content2` control de contenido. Una vez hecho esto, el marcado de las páginas debe contener un solo control de contenido.

Páginas de ASP.NET en el `Administration` carpeta están diseñados únicamente para los usuarios administrativos. Hemos agregado un rol de administradores en el sistema en el <a id="_msoanchor_2"> </a> [ *crear y administrar Roles* ](../roles/creating-and-managing-roles-cs.md) tutorial; restringir el acceso a estas dos páginas a este rol. Para ello, agregue un `Web.config` del archivo a la `Administration` carpeta y configurar sus `<authorization>` elemento para admitir usuarios de la función de los administradores y denegar todos los demás.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

En este momento, el Explorador de soluciones del proyecto debe ser similar a la pantalla se muestra en la figura 1.


[![Se agregaron cuatro nuevas páginas y un archivo Web.config para el sitio Web](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Figura 1**: cuatro nuevas páginas y un `Web.config` archivo se agregaron al sitio Web ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


Por último, actualizar el mapa del sitio (`Web.sitemap`) para incluir una entrada para el `ManageUsers.aspx` página. Agregue el siguiente código XML después de la `<siteMapNode>` hemos agregado para los tutoriales de Roles.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Con el mapa del sitio actualizado, visite el sitio a través de un explorador. Como se muestra en la figura 2, la navegación de la izquierda incluye elementos para los tutoriales de administración.


[![El mapa del sitio incluye un nodo denominado Administración de usuario](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Figura 2**: el mapa del sitio incluye un nodo denominado Administración de usuarios ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Paso 2: Mostrar todas las cuentas de usuario en un control GridView

El objetivo final de este tutorial es crear una cuadrícula paginada, se puede filtrar a través del cual un administrador puede seleccionar una cuenta de usuario para administrar. Empecemos por lista *todos los* a los usuarios en un control GridView. Una vez completada, agregaremos las interfaces y funcionalidad de filtrado y paginación.

Abra la `ManageUsers.aspx` página en el `Administration` carpeta y agregar un control GridView, estableciendo su `ID` a `UserAccounts`. En un momento, se escribirá el código para enlazar el conjunto de cuentas de usuario a la GridView mediante la `Membership` la clase `GetAllUsers` método. Como se describe en los tutoriales anteriores, el método GetAllUsers devuelve un `MembershipUserCollection` objeto, que es una colección de `MembershipUser` objetos. Cada `MembershipUser` en la colección incluye propiedades, como `UserName`, `Email`, `IsApproved`, y así sucesivamente.

Para mostrar la información de la cuenta de usuario deseado en el control GridView, establecer la GridView `AutoGenerateColumns` propiedad en False y agregue BoundFields para el `UserName`, `Email`, y `Comment` propiedades y CheckBoxFields para el `IsApproved`, `IsLockedOut`, y `IsOnline` propiedades. Esta configuración se puede aplicar a través de marcado declarativo del control o mediante el cuadro de diálogo de campos. Figura 3 muestra una captura de pantalla de los campos de cuadro de diálogo después de que se ha desactivado la casilla de verificación Generar automáticamente campos y los BoundFields y CheckBoxFields se han agregado y configurado.


[![Agregar tres BoundFields y tres CheckBoxFields a GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Figura 3**: agregar tres BoundFields y tres CheckBoxFields a GridView ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


Después de configurar el control GridView, asegúrese de que su marcado declarativo es similar al siguiente:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

A continuación, es necesario escribir código que enlaza las cuentas de usuario a la GridView. Crear un método denominado `BindUserAccounts` para realizar esta tarea y, a continuación, llámelo desde el `Page_Load` controlador de eventos en la primera página, visite.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

Tómese un momento para probar la página a través de un explorador. Como se muestra en la figura 4, la `UserAccounts` GridView muestra el nombre de usuario, dirección de correo electrónico y otra información de cuenta pertinentes para todos los usuarios en el sistema.


[![Las cuentas de usuario se muestran en el control GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Figura 4**: las cuentas de usuario se muestran en el control GridView ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Paso 3: Filtrar los resultados por la primera letra del nombre de usuario

Actualmente el `UserAccounts` GridView muestra *todos los* de las cuentas de usuario. Para los sitios Web con cientos o miles de cuentas de usuario, es imperativo que el usuario sea capaz de reducir rápidamente las cuentas mostradas. Esto puede realizarse mediante la adición de filtrado LinkButton a la página. Agreguemos 27 LinkButton a la página: uno denominado todo junto con uno LinkButton para cada letra del alfabeto. Si un visitante hace clic en todos los LinkButton, GridView mostrará todos los usuarios. Si hacen clic en una letra determinada, se mostrará solo los usuarios cuyo nombre de usuario comienza por la letra seleccionada.

La primera tarea consiste en agregar los controles LinkButton 27. Una opción sería crear mediante declaración, la 27 LinkButton uno en uno. Un enfoque más flexible consiste en usar un control de repetidor con un `ItemTemplate` que representa un control LinkButton y, a continuación, enlaza las opciones de filtrado para el repetidor como un `string` matriz.

Comience por agregar un control de repetidor a la página anterior del `UserAccounts` GridView. Establecer el repetidor `ID` propiedad `FilteringUI`. Configurar las plantillas del repetidor para que su `ItemTemplate` representa un LinkButton cuya `Text` y `CommandName` propiedades se enlazan al elemento de matriz actual. Como vimos en el <a id="_msoanchor_3"> </a> [ *asignar Roles a los usuarios* ](../roles/assigning-roles-to-users-cs.md) tutorial, esto puede realizarse mediante el `Container.DataItem` sintaxis de enlace de datos. Use el repetidor `SeparatorTemplate` para mostrar una línea vertical entre cada vínculo.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Para rellenar esta repetidor con las opciones de filtrado deseadas, cree un método denominado `BindFilteringUI`. Asegúrese de llamar a este método desde el `Page_Load` controlador de eventos en la primera carga de página.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Este método especifica las opciones de filtrado como elementos en el `string` matriz `filterOptions`. Para cada elemento de la matriz, repetidor representará un LinkButton con su `Text` y `CommandName` propiedades asignadas al valor del elemento de matriz.

Figura 5 se muestra la `ManageUsers.aspx` página cuando se ve mediante un explorador.


[![Repetidor enumera 27 LinkButton filtrado](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Figura 5**: el repetidor enumera 27 filtrado LinkButton ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> Nombres de usuario pueden empezar con cualquier carácter, incluidos números y signos de puntuación. Para ver estas cuentas, el administrador tendrá que utilizar la opción LinkButton todos los. Como alternativa, puede agregar un control LinkButton para devolver todas las cuentas de usuario que se inician con un número. Dejar este como un ejercicio para el lector.


Al hacer clic en cualquiera de lo LinkButton filtrado provoca una devolución de datos y genera el repetidor `ItemCommand` eventos, pero no hay ningún cambio en la cuadrícula porque hemos todavía para escribir ningún código para filtrar los resultados. El `Membership` clase incluye una [ `FindUsersByName` método](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) que devuelve las cuentas de usuario cuyo nombre de usuario coincide con un patrón de búsqueda especificado. Podemos usar este método para recuperar solo las cuentas de usuario cuyos nombres comienzan por la letra especificada por el `CommandName` de LinkButton filtrado que se hizo clic.

Inicie actualizando el `ManageUser.aspx` clase de código subyacente de la página para que incluya una propiedad denominada `UsernameToMatch`. Esta propiedad conserva la cadena de filtro de nombre de usuario a través de las devoluciones de datos:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

El `UsernameToMatch` propiedad almacena su valor se asigna en el `ViewState` colección utilizando la clave UsernameToMatch. Cuando se lee el valor de esta propiedad, comprueba para ver si existe un valor en el `ViewState` colección; en caso contrario, se devuelve el valor predeterminado, una cadena vacía. El `UsernameToMatch` propiedad exhibe un patrón común, es decir, conservar un valor para ver el estado para que se conserven los cambios a la propiedad a través de las devoluciones de datos. Para obtener más información sobre este patrón, lea [Understanding ASP.NET View State](https://msdn.microsoft.com/library/ms972976.aspx).

A continuación, actualice el `BindUserAccounts` método para que en lugar de llamar al método `Membership.GetAllUsers`, llama a `Membership.FindUsersByName`, pasando el valor de la `UsernameToMatch` propiedad que se anexa con el carácter comodín SQL, %.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Para mostrar únicamente los usuarios cuyo nombre de usuario comienza con la letra A, establezca el `UsernameToMatch` propiedad a una y, a continuación, llame a `BindUserAccounts`. Eso resultaría en una llamada a `Membership.FindUsersByName("A%")`, que devolverá todos los usuarios cuyo nombre de usuario comienza por A. Asimismo, para devolver *todos los* a los usuarios, asigne una cadena vacía para el `UsernameToMatch` propiedad para que la `BindUserAccounts` le (método) invocar `Membership.FindUsersByName("%")`, con lo que se devuelve todas las cuentas de usuario.

Crear un controlador de eventos para el repetidor `ItemCommand` eventos. Este evento se desencadena cuando se hace clic en uno de los filtros LinkButton; se pasa el control LinkButton donde ha hecho clic `CommandName` valor a través de la `RepeaterCommandEventArgs` objeto. Es necesario asignar el valor apropiado para la `UsernameToMatch` propiedad y, después, llame el `BindUserAccounts` método. Si el `CommandName` es, asigne una cadena vacía para `UsernameToMatch` para que se muestren todas las cuentas de usuario. De lo contrario, asigne el `CommandName` valor a `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Con este código en su lugar, pruebe la funcionalidad de filtrado. Cuando primero se visita la página, se muestran todas las cuentas de usuario (hacen referencia a la figura 5). Al hacer clic en LinkButton A provoca una devolución de datos y filtra los resultados, mostrar sólo las cuentas de usuario que comiencen por A.


[![Usar el filtrado LinkButton para mostrar los usuarios cuyo nombre de usuario comienza por una letra determinada](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Figura 6**: usar el filtrado LinkButton para mostrar esos usuarios cuyo nombre de usuario comienza con una letra ciertos ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Paso 4: Actualizar GridView para utilizar la paginación

GridView que se muestra en las figuras 5 y 6 muestra todos los registros devueltos desde la `FindUsersByName` método. Si no hay cientos o miles de cuentas de usuario Esto puede provocar sobrecarga de información al ver todas las cuentas (como es el caso al hacer clic en LinkButton todos o al visitar inicialmente la página). Con el fin de presentar las cuentas de usuario en fragmentos manejables más, vamos a configurar el control GridView para mostrar 10 cuentas de usuario a la vez.

El control GridView ofrece dos tipos de paginación:

- **Paginación predeterminada** : fácil de implementar, pero ineficaz. En pocas palabras, no tiene valor predeterminado paginación GridView espera *todos los* de los registros de su origen de datos. A continuación, solo muestra la página adecuada de registros.
- **Paginación personalizada** -requiere más trabajo para implementar, pero es más eficaz que de forma predeterminada una paginación porque con custom pagine los datos en origen devuelve solo el conjunto de registros que deben mostrarse.

La diferencia de rendimiento entre predeterminados y paginación personalizada puede ser bastante elevada al paginar a través de miles de registros. Dado que estamos creando esta interfaz suponiendo que no haya puede ser cientos o miles de cuentas de usuario, vamos a utilizar la paginación personalizada.

> [!NOTE]
> Para obtener una explicación más exhaustiva sobre las diferencias entre el valor predeterminado y la paginación personalizada, así como los desafíos implicados en la implementación de paginación personalizada, consulte [eficazmente paginar a través de grandes cantidades de datos](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Para un análisis de la diferencia de rendimiento entre predeterminado y la paginación personalizada, vea [paginación personalizada en ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Para implementar la paginación personalizada en primer lugar se necesita algún mecanismo por el que se va a recuperar el subconjunto de registros mostrado por el control GridView preciso. Las buenas noticias son que el `Membership` la clase `FindUsersByName` método tiene una sobrecarga que nos permite especificar el índice de la página y el tamaño de página y devuelve solo las cuentas de usuario que se encuentran dentro de ese intervalo de registros.

En concreto, esta sobrecarga tiene la siguiente firma: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

El *pageIndex* parámetro especifica la página de cuentas de usuario para devolver; *pageSize* indica cuántos registros se muestran por página. El *totalRecords* parámetro es un `out` parámetro que devuelve el número de cuentas de usuario total en el almacén del usuario.

> [!NOTE]
> Los datos devueltos por `FindUsersByName` se ordena por nombre de usuario; no se puede personalizar los criterios de ordenación.


GridView puede configurarse para utilizar la paginación personalizada, pero solo cuando se enlaza a un control ObjectDataSource. Para que el control ObjectDataSource implementar la paginación personalizada, requiere dos métodos: uno que se pasa a un índice de fila inicial y el número máximo de registros que se va a mostrar, y devuelve el subconjunto de registros que se encuentran dentro de ese intervalo; preciso y un método que devuelve el número total de registros que se va a paginar a través de. El `FindUsersByName` sobrecarga acepta un índice de la página y el tamaño de página y devuelve el número total de registros mediante un `out` parámetro. Por lo que no hay una falta de coincidencia de interfaz.

Una opción sería crear una clase de proxy que expone la interfaz ObjectDataSource espera y, a continuación, se llama internamente el `FindUsersByName` método. Otra opción - y el otro que se va a utilizar para este artículo - es crear nuestra propia interfaz de paginación y usarlo en lugar de la interfaz de paginación integrado de GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Creando una primera, anterior, a continuación, la última interfaz de paginación

Vamos a crear una interfaz de paginación con primero, anterior, siguiente y última LinkButton. El primer LinkButton, al hacer clic, se guiará al usuario a la primera página de datos, mientras que el anterior le devolverá a la página anterior. Del mismo modo, siguiente y último moverá al usuario a la página siguiente y última, respectivamente. Agregar los cuatro controles LinkButton bajo la `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

A continuación, cree un controlador de eventos para cada uno de lo LinkButton `Click` eventos.

La figura 7 muestra lo cuatro LinkButton cuando se ven a través de la vista de diseño de Visual Web Developer.


[![Agregar en primer lugar, anterior, a continuación, y el apellido de LinkButton debajo de GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Figura 7**: agregar primera, anterior, siguiente y última LinkButton debajo la GridView ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Realizar el seguimiento del índice de la página actual

Cuando un usuario visita por primera vez la `ManageUsers.aspx` página o hace clic en uno de los filtros botones, deseamos mostrar la primera página de datos en GridView. Sin embargo, cuando el usuario hace clic en uno de los vínculos de desplazamiento, deberá actualizar el índice de la página. Para mantener el índice de la página y el número de registros mostrados por página, agregue las dos propiedades siguientes para la clase de código subyacente de la página:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

Al igual que el `UsernameToMatch` propiedad, el `PageIndex` propiedad conserva su valor para el estado de vista. Sólo lectura `PageSize` propiedad devuelve un valor codificado de forma rígida, 10. Puede invitar a actualizar esta propiedad para utilizar el mismo patrón que el lector interesado `PageIndex`y, a continuación, para aumentar la `ManageUsers.aspx` página de forma que la persona que visita la página puede especificar cuántas cuentas de usuario que se mostrarán por página.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recuperar sólo los registros de la página actual, actualizar el índice de la página y habilitar y deshabilitar el LinkButton de interfaz de paginación

Con la interfaz de paginación en su lugar y el `PageIndex` y `PageSize` agregan propiedades, estamos preparados para actualizar la `BindUserAccounts` método para que use la correspondiente `FindUsersByName` sobrecarga. Además, es necesario hacer que este método habilitar o deshabilitar la interfaz de paginación dependiendo de qué página se está mostrando. Al ver la primera página de datos, se deben deshabilitar los vínculos primero y anterior; A continuación y última debe deshabilitarse al ver la última página.

Actualice el método `BindUserAccounts` con el código siguiente:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Tenga en cuenta que el número total de registros que se va a paginar a través de se determina por el último parámetro de la `FindUsersByName` método. Se trata de un `out` parámetro, por lo que es necesario en primer lugar, declare una variable que contenga este valor (`totalRecords`) y, a continuación, agregarle un prefijo con el `out` palabra clave.

Después de que se devuelva la página especificada de las cuentas de usuario, lo cuatro LinkButton se habilita o deshabilita, dependiendo de si está viendo la primera o última página de datos.

El último paso consiste en escribir el código para LinkButton cuatro `Click` controladores de eventos. Estos controladores de eventos que necesite actualizar el `PageIndex` propiedad y, a continuación, volver a enlazar los datos en GridView mediante una llamada a `BindUserAccounts`. La primera, anterior y siguientes controladores de eventos son muy simples. El `Click` controlador de eventos para el último LinkButton, sin embargo, es un poco más compleja, dado que necesitamos determinar cuántos registros se muestran con el fin de determinar el último índice de la página.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Las figuras 8 y 9 muestran la interfaz de paginación personalizada en acción. La figura 8 muestra la `ManageUsers.aspx` página al ver la primera página de datos para todas las cuentas de usuario. Tenga en cuenta que se muestran solo 10 de las cuentas de 13. Haga clic en el vínculo siguiente o último provoca una devolución de datos, las actualizaciones de la `PageIndex` a 1 y enlaza las cuentas de la segunda página del usuario a la cuadrícula (consulte la figura 9).


[![Se muestran las primeras cuentas de usuario de 10](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Figura 8**: se muestran las cuentas de usuario de 10 primer ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![Haga clic en el vínculo siguiente muestra la segunda página de cuentas de usuario](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Figura 9**: haga clic en el vínculo siguiente muestra la segunda página de las cuentas de usuario ([haga clic aquí para ver la imagen a tamaño completo](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>Resumen

Los administradores necesitan a menudo seleccionar un usuario de la lista de cuentas. En los tutoriales anteriores analizamos con una lista desplegable que se rellena con los usuarios, pero este enfoque no se escala bien. En este tutorial, analizamos una alternativa mejor: una interfaz filtrable cuyos resultados se muestran en un control GridView paginado. Con esta interfaz de usuario, los administradores pueden rápida y eficaz busque y seleccione una cuenta de usuario entre miles.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Paginación personalizada en ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Eficazmente paginar a través de grandes cantidades de datos](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Poner su propia herramienta de administración de sitios Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Alicja Maziarz. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](recovering-and-changing-passwords-cs.md)
