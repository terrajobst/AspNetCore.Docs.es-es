---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Creación de una interfaz para seleccionar una cuenta de usuario de muchos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial vamos a crear una interfaz de usuario con una cuadrícula por página, puede filtrar. En concreto, la interfaz de usuario constará de una serie de LinkButtons para...
ms.author: aspnetcontent
ms.date: 04/01/2008
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: af2a9692e03f147dfc1389f8c13b7d4fd758d62e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803264"
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Creación de una interfaz para seleccionar una cuenta de usuario de muchos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) o [descargar PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> En este tutorial vamos a crear una interfaz de usuario con una cuadrícula por página, puede filtrar. En concreto, la interfaz de usuario constará de una serie de LinkButtons para filtrar los resultados según la letra inicial del nombre de usuario y un control GridView para mostrar los usuarios coincidentes. Comenzaremos con una lista de todas las cuentas de usuario en un control GridView. A continuación, en el paso 3, se agregará el filtro LinkButtons. Paso 4 se examina los resultados filtrados de paginación. La interfaz construida en los pasos 2 a 4 se usará en los tutoriales siguientes para realizar tareas administrativas para una cuenta de usuario determinada.


## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"> </a> [ *asignar Roles a los usuarios* ](../roles/assigning-roles-to-users-vb.md) tutorial, creamos una interfaz rudimentaria para que un administrador seleccionar un usuario y administrar sus roles. En concreto, la interfaz presenta el Administrador con una lista desplegable de todos los usuarios. Esta interfaz es adecuada cuando hay, pero las cuentas de una docena de usuario, pero es difícil de manejar para sitios con cientos o miles de cuentas. Una cuadrícula por página, puede filtrar es la interfaz de usuario más adecuado para sitios Web con bases de usuarios grande.

En este tutorial vamos a crear una interfaz de usuario de este tipo. En concreto, la interfaz de usuario constará de una serie de LinkButtons para filtrar los resultados según la letra inicial del nombre de usuario y un control GridView para mostrar los usuarios coincidentes. Comenzaremos con una lista de todas las cuentas de usuario en un control GridView. A continuación, en el paso 3, se agregará el filtro LinkButtons. Paso 4 se examina los resultados filtrados de paginación. La interfaz construida en los pasos 2 a 4 se usará en los tutoriales siguientes para realizar tareas administrativas para una cuenta de usuario determinada.

Comencemos.

## <a name="step-1-adding-new-aspnet-pages"></a>Paso 1: Agregar nuevas páginas ASP.NET

En este tutorial y los dos siguientes analizaremos diversas funciones relacionadas con la administración y capacidades. Necesitamos una serie de páginas ASP.NET para implementar los temas que se examinan a lo largo de estos tutoriales. Vamos a crear estas páginas y actualizar el mapa del sitio.

Empiece por crear una nueva carpeta en el proyecto denominado `Administration`. A continuación, agregue dos nuevas páginas ASP.NET a la carpeta, vincular cada página con el `Site.master` página maestra. Nombre de las páginas:

- `ManageUsers.aspx`
- `UserInformation.aspx`

También agrega dos páginas al directorio de raíz del sitio Web: `ChangePassword.aspx` y `RecoverPassword.aspx`.

Estas cuatro páginas en este punto, deberían, tiene dos controles de contenido, uno para cada uno de ContentPlaceHolders la página maestra: `MainContent` y `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Queremos que aparezca el marcado de la página maestra predeterminada para el `LoginContent` ContentPlaceHolder para estas páginas. Por lo tanto, quite el marcado declarativo para el `Content2` control de contenido. Una vez hecho esto, el marcado de las páginas debe contener un solo control de contenido.

Páginas de ASP.NET en el `Administration` carpeta está diseñada únicamente para los usuarios administrativos. Se ha agregado un rol de administradores en el sistema en el <a id="_msoanchor_2"> </a> [ *creación y administración de funciones* ](../roles/creating-and-managing-roles-vb.md) tutorial; restringir el acceso a estas dos páginas para este rol. Para ello, agregue un `Web.config` del archivo a la `Administration` carpeta y configurar su `<authorization>` elemento para admitir usuarios de la función de los administradores y denegar todos los demás.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

En este momento, el Explorador de soluciones del proyecto debe ser similar a la pantalla se muestra en la figura 1.


[![Se han agregado cuatro nuevas páginas y un archivo Web.config para el sitio Web](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Figura 1**: cuatro nuevas páginas y una `Web.config` archivo se han agregado al sitio Web ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Por último, actualice el mapa del sitio (`Web.sitemap`) para incluir una entrada para el `ManageUsers.aspx` página. Agregue el siguiente código XML después de la `<siteMapNode>` se ha agregado para los tutoriales de Roles.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Con el mapa del sitio actualizado, visite el sitio mediante un explorador. Como se muestra en la figura 2, el panel de navegación de la izquierda ahora incluye elementos para los tutoriales de administración.


[![El mapa del sitio incluye un nodo denominado Administración de usuarios](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Figura 2**: el mapa del sitio incluye un nodo denominado Administración de usuarios ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Paso 2: Mostrar todas las cuentas de usuario en un control GridView

Nuestro objetivo final de este tutorial es crear una cuadrícula por página, puede filtrar a través del cual un administrador puede seleccionar una cuenta de usuario para administrar. Empecemos por lista *todas* a los usuarios en un control GridView. Una vez completado, se agregará la funcionalidad y las interfaces de filtrado y paginación.

Abra el `ManageUsers.aspx` página en el `Administration` carpeta y agregar un control GridView, estableciendo su `ID` a `UserAccounts` en unos instantes, escribiremos código para enlazar el conjunto de cuentas de usuario a la GridView mediante el `Membership` la clase `GetAllUsers` método. Como se describe en los tutoriales anteriores, el `GetAllUsers` método devuelve un `MembershipUserCollection` objeto, que es una colección de `MembershipUser` objetos. Cada `MembershipUser` en la colección incluye propiedades como `UserName`, `Email`, `IsApproved`, y así sucesivamente.

Con el fin de mostrar la información de cuenta de usuario deseado en el control GridView, establezca la GridView `AutoGenerateColumns` propiedad en False y agregue BoundFields para el `UserName`, `Email`, y `Comment` propiedades y CheckBoxFields para el `IsApproved`, `IsLockedOut`, y `IsOnline` propiedades. Esta configuración se puede aplicar a través de marcado declarativo del control o a través del cuadro de diálogo campos. Figura 3 muestra una captura de pantalla de los campos de cuadro de diálogo después de que se activó la casilla de verificación Generar automáticamente los campos y se han agregado y configurado el BoundFields y CheckBoxFields.


[![Agregar tres BoundFields y tres CheckBoxFields en GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Figura 3**: agregar tres BoundFields y tres CheckBoxFields en GridView ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


Después de configurar el control GridView, asegúrese de que su marcado declarativo es similar al siguiente:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

A continuación, se debe escribir código que enlaza las cuentas de usuario en el control GridView. Cree un método denominado `BindUserAccounts` para realizar esta tarea y, a continuación, llámelo desde el `Page_Load` controlador de eventos en la primera visita de página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Dedique un momento para probar la página a través de un explorador. Como se muestra en la figura 4, el `UserAccounts` GridView muestra el nombre de usuario, dirección de correo electrónico y otra información pertinente de cuenta para todos los usuarios en el sistema.


[![Las cuentas de usuario se muestran en el control GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Figura 4**: se enumeran las cuentas de usuario en el control GridView ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Paso 3: Filtrar los resultados por la primera letra del nombre de usuario

Actualmente la `UserAccounts` GridView muestra *todas* las cuentas de usuario. Para los sitios Web con cientos o miles de cuentas de usuario, es imperativo que el usuario sea capaz de reducir rápidamente a las cuentas mostradas. Esto puede realizarse mediante la adición de filtrado LinkButtons a la página. Vamos a agregar 27 LinkButtons a la página: uno denominado todo junto con un control LinkButton para cada letra del alfabeto. Si un visitante hace clic en LinkButton todas, el control GridView mostrará todos los usuarios. Si hace clic en una letra determinada, se mostrará únicamente los usuarios cuyo nombre de usuario comienza por la letra seleccionada.

Nuestra primera tarea consiste en agregar los controles LinkButton 27. Sería una opción crear el 27 LinkButtons mediante declaración, una vez. Un enfoque más flexible es utilizar un control Repeater con un `ItemTemplate` que representa un control LinkButton y, a continuación, enlaza las opciones de filtrado para el control Repeater como un `String` matriz.

Comience por agregar un control Repeater a la página anterior del `UserAccounts` GridView. Establecer el repetidor `ID` propiedad `FilteringUI` configurar plantillas de Repeater para que su `ItemTemplate` representa un control LinkButton cuyo `Text` y `CommandName` propiedades se enlazan al elemento de matriz actual. Como hemos visto en el <a id="_msoanchor_3"> </a> [ *asignar Roles a los usuarios* ](../roles/assigning-roles-to-users-vb.md) tutorial, esto puede realizarse mediante el `Container.DataItem` sintaxis de enlace de datos. Utilice el repetidor `SeparatorTemplate` para mostrar una línea vertical entre cada vínculo.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Para rellenar este Repeater con las opciones de filtrado deseadas, cree un método denominado `BindFilteringUI`. No olvide llamar a este método desde el `Page_Load` controlador de eventos en la primera carga de página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Este método especifica las opciones de filtrado como elementos en el `String` matriz `filterOptions` para cada elemento de la matriz, el control Repeater representará un control LinkButton con su `Text` y `CommandName` asignadas al valor de la matriz de propiedades elemento.

La figura 5 muestra el `ManageUsers.aspx` página cuando se ve mediante un explorador.


[![El control Repeater enumera 27 LinkButtons filtrados](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Figura 5**: el control Repeater enumera 27 filtrado LinkButtons ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> Los nombres de usuario pueden empezar con cualquier carácter, incluidos números y signos de puntuación. Con el fin de ver estas cuentas, el administrador tendrá que usar la opción LinkButton todas. Como alternativa, puede agregar un control LinkButton para devolver todas las cuentas de usuario que comienzan con un número. Dejar como un ejercicio para el lector.


Al hacer clic en cualquiera de los LinkButtons filtrados provoca una devolución de datos y genera el repetidor `ItemCommand` eventos, pero no hay ningún cambio en la cuadrícula porque aún no hemos escribir ningún código para filtrar los resultados. El `Membership` clase incluye un [ `FindUsersByName` método](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) que devuelve esas cuentas de usuario cuyo nombre de usuario coincide con un patrón de búsqueda especificado. Podemos usar este método para recuperar sólo a las cuentas de usuario cuyos nombres de usuario comiencen con la letra especificada por el `CommandName` de LinkButton filtrado que se hizo clic.

Empiece actualizando el `ManageUser.aspx` clase de código subyacente de la página para que incluya una propiedad denominada `UsernameToMatch` esta propiedad mantiene la cadena de filtro de nombre de usuario en las devoluciones de datos:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

El `UsernameToMatch` propiedad almacena su valor se asigna a la `ViewState` colección mediante la clave UsernameToMatch. Cuando se lee el valor de esta propiedad, comprueba para ver si existe un valor en el `ViewState` colección; en caso contrario, se devuelve el valor predeterminado, una cadena vacía. El `UsernameToMatch` propiedad exhibe un patrón común, es decir, conservar un valor de estado de vista para que los cambios a la propiedad se conservan entre las devoluciones de datos. Para obtener más información sobre este patrón, lea [Understanding ASP.NET View State](https://msdn.microsoftn-us/library/ms972976.aspx).

A continuación, actualice el `BindUserAccounts` método así que, en lugar de llamar a `Membership.GetAllUsers`, llama a `Membership.FindUsersByName`, pasando el valor de la `UsernameToMatch` propiedad anexado con el carácter comodín SQL, %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Para mostrar solo aquellos usuarios cuyo nombre de usuario se inicia con la letra A, establezca el `UsernameToMatch` propiedad a la A y, a continuación, llame a `BindUserAccounts` esto daría lugar a una llamada a `Membership.FindUsersByName("A%")`, que devolverá todos los usuarios cuyo nombre de usuario comienza por A. Asimismo, para devolver *todos los* de los usuarios, asignar una cadena vacía para el `UsernameToMatch` propiedad para que el `BindUserAccounts` método invocará `Membership.FindUsersByName("%")`, lo que devuelve todas las cuentas de usuario.

Crear un controlador de eventos para el repetidor `ItemCommand` eventos. Este evento se desencadena cuando se hace clic en uno de los filtros LinkButtons; se pasa el control LinkButton donde ha hecho clic `CommandName` valor a través de la `RepeaterCommandEventArgs` objeto. Se debe asignar el valor adecuado para el `UsernameToMatch` propiedad y, después, llame el `BindUserAccounts` método. Si el `CommandName` es, asigna una cadena vacía a `UsernameToMatch` para que se muestren todas las cuentas de usuario. De lo contrario, asigne el `CommandName` valor `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Con este código en su lugar, pruebe la funcionalidad de filtrado. Cuando primero se visita la página, se muestran todas las cuentas de usuario (consulte la vuelta a la figura 5). Al hacer clic en LinkButton A produce un postback y filtra los resultados, mostrar solo esas cuentas de usuario que empiezan por A.


[![Use el filtrado LinkButtons para mostrar aquellos usuarios cuyo nombre de usuario se inicia con una letra determinada](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Figura 6**: usar el filtrado LinkButtons para mostrar esos usuarios cuyo nombre de usuario comience con una letra ciertos ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Paso 4: Actualización de GridView para usar la paginación

El control GridView se muestra en las figuras 5 y 6 muestra todos los registros devueltos desde el `FindUsersByName` método. Si hay cientos o miles de cuentas de usuario Esto puede conducir a la sobrecarga de información cuando se ven todas las cuentas (como es el caso al hacer clic en LinkButton todas o cuando se visita inicialmente la página). Para ayudar a presentar las cuentas de usuario en fragmentos más manejables, vamos a configurar el control GridView para mostrar 10 cuentas de usuario a la vez.

El control GridView ofrece dos tipos de paginación:

- **Paginación predeterminada** : fácil de implementar, pero ineficaz. En pocas palabras, no tiene valor predeterminado de paginación del control GridView espera *todas* los registros de su origen de datos. A continuación, solo muestra la página adecuada de registros.
- **Paginación personalizada** -requiere más trabajo para implementar, pero es más eficaz que una paginación predeterminada porque con personalizada que pagine los datos en origen devuelve solo el conjunto de registros que deben mostrarse.

La diferencia de rendimiento entre el valor predeterminado y la paginación personalizada puede ser bastante considerable al paginar a través de miles de registros. Dado que estamos creando esta interfaz suponiendo que no haya puede ser de cientos o miles de cuentas de usuario, vamos a usar paginación personalizada.

> [!NOTE]
> Para obtener una explicación más exhaustiva sobre las diferencias entre el valor predeterminado y la paginación personalizada, así como los desafíos implicados en implementar paginación personalizada, consulte [eficazmente paginar a través de grandes cantidades de datos](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Para un análisis de la diferencia de rendimiento entre el valor predeterminado y la paginación personalizada, vea [paginación personalizada en ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Para implementar la paginación personalizada, que primero es necesario algún mecanismo por el que se va a recuperar el subconjunto de registros mostrados por el control GridView preciso. La buena noticia es que la `Membership` la clase `FindUsersByName` método tiene una sobrecarga que nos permite especificar el índice de la página y el tamaño de página y devuelve solo esas cuentas de usuario que se encuentran dentro de ese intervalo de registros.

En concreto, esta sobrecarga tiene la siguiente firma: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

El *pageIndex* parámetro especifica la página de cuentas de usuario para devolver; *pageSize* indica el número de registros que se muestran por página. El *totalRecords* parámetro es un `ByRef` parámetro que devuelve el número de cuentas de usuario total en el almacén del usuario.

> [!NOTE]
> Los datos devueltos por `FindUsersByName` se ordena por nombre de usuario; no se pueden personalizar los criterios de ordenación.


El control GridView puede configurarse para utilizar la paginación personalizada, pero solo cuando se enlaza a un control ObjectDataSource. Para que el control ObjectDataSource implementar la paginación personalizada, necesita dos métodos: uno que se pasa a un índice de fila inicial y el número máximo de registros que deben mostrarse, y devuelve el subconjunto de registros que se encuentran dentro de ese intervalo; preciso y un método que devuelve el número total de registros que se va a paginar a través. El `FindUsersByName` sobrecarga acepta un índice de la página y el tamaño de página y devuelve el número total de registros a través de un `ByRef` parámetro. Por tanto, hay una falta de coincidencia de interfaz.

Una opción sería crear una clase de proxy que expone la interfaz espera ObjectDataSource y, a continuación, se llama internamente el `FindUsersByName` método. Otra opción - y uno que para este artículo usaremos - consiste en crear nuestra propia interfaz de paginación y usarla en lugar de la interfaz de paginación integrada de GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Crear una primera, anterior, a continuación, última interfaz de paginación

Vamos a crear una interfaz de paginación con el primero, anterior, siguiente y última LinkButtons. El primer LinkButton, al hacer clic, llevará al usuario a la primera página de datos, mientras que el anterior le devolverá a la página anterior. Del mismo modo, siguiente y último moverá al usuario a la página siguiente y última, respectivamente. Agregue los cuatro controles LinkButton debajo el `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

A continuación, cree un controlador de eventos para cada uno de lo LinkButton `Click` eventos.

Figura 7 muestra los cuatro LinkButtons cuando se ven a través de la vista de diseño de Visual Web Developer.


[![Agregar en primer lugar, anterior, a continuación, y la última LinkButtons bajo el control GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Figura 7**: agregar la primera, anterior, siguiente y última LinkButtons debajo de GridView ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Realizar el seguimiento del índice de la página actual

Cuando un usuario visita por primera vez el `ManageUsers.aspx` página o hace clic en uno de filtrado de los botones, deseamos mostrar la primera página de datos en GridView. Sin embargo, cuando el usuario hace clic en uno de los vínculos de desplazamiento, se debe actualizar el índice de página. Para mantener el índice de página y el número de registros que se muestran por página, agregue las dos propiedades siguientes a la clase de código subyacente de la página:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Al igual que el `UsernameToMatch` propiedad, el `PageIndex` propiedad conserva su valor para el estado de vista. Solo lectura `PageSize` propiedad devuelve un valor codificado de forma rígida, 10. Se puede invitar a sitúa para actualizar esta propiedad para utilizar el mismo patrón que `PageIndex`y, a continuación, para aumentar la `ManageUsers.aspx` página tal que la persona que visita la página puede especificar cuántas cuentas de usuario que se muestran por página.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recuperar sólo los registros de la página actual, actualizando el índice de página y habilitar y deshabilitar el LinkButtons de interfaz de paginación

Con la interfaz de paginación en su lugar y `PageIndex` y `PageSize` agregan propiedades, estamos preparados para actualizar el `BindUserAccounts` método para que use adecuado `FindUsersByName` sobrecargar. Además, es necesario que este método habilita o deshabilita dependiendo de qué página se muestra la interfaz de paginación. Al ver la primera página de datos, se deben deshabilitar los vínculos primero y anterior; A continuación y última debe deshabilitarse al ver la última página.

Actualice el método `BindUserAccounts` con el código siguiente:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Tenga en cuenta que el número total de registros que se va a paginar a través de viene determinada por el último parámetro de la `FindUsersByName` método. Después de que se devuelven a la página de cuentas de usuario especificada, el cuatro LinkButtons habilitados o deshabilitados, dependiendo de si se está viendo la primera o última página de datos.

El último paso es escribir el código para 'LinkButtons cuatro `Click` controladores de eventos. Estos controladores de eventos que necesite actualizar el `PageIndex` propiedad y, a continuación, volver a enlazar los datos en el control GridView a través de una llamada a `BindUserAccounts` controladores de eventos de la primera, anterior y siguiente son muy sencillos. El `Click` controlador de eventos para el último control LinkButton, sin embargo, es un poco más complejo porque es necesario determinar cuántos registros se muestran con el fin de determinar el último índice de página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Las figuras 8 y 9 muestran la interfaz de paginación personalizada en acción. La figura 8 muestra el `ManageUsers.aspx` página al ver la primera página de datos para todas las cuentas de usuario. Tenga en cuenta que se muestran sólo el 10 de las cuentas de 13. Al hacer clic en el vínculo siguiente o último produce un postback, las actualizaciones de la `PageIndex` a 1 y se enlaza a la cuadrícula de cuentas de la segunda página del usuario (consulte la figura 9).


[![Se muestran las primeras cuentas de usuario de 10](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Figura 8**: se muestran las cuentas de usuario de 10 primera ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Al hacer clic en el vínculo siguiente muestra la segunda página de cuentas de usuario](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Figura 9**: al hacer clic en el vínculo siguiente muestra la segunda página de las cuentas de usuario ([haga clic aquí para ver imagen en tamaño completo](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Resumen

A menudo, los administradores necesitan seleccionar un usuario de la lista de cuentas. En los tutoriales anteriores hemos examinado con una lista desplegable que se rellena con los usuarios, pero este enfoque no se escala bien. En este tutorial hemos explorado una alternativa mejor: una interfaz filterable cuyos resultados se muestran en un control GridView paginado. Con esta interfaz de usuario, los administradores pueden rápida y eficaz busque y seleccione una cuenta de usuario entre miles.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Paginación personalizada en ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Página de forma eficaz a través de grandes cantidades de datos](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Implementar su propia herramienta de administración del sitio Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Alicja Maziarz. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en

> [!div class="step-by-step"]
> [Anterior](unlocking-and-approving-user-accounts-cs.md)
> [Siguiente](recovering-and-changing-passwords-vb.md)
