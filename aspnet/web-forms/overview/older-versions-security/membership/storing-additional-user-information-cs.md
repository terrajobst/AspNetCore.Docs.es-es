---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Almacenar información de usuario adicional (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial se responderá a esta pregunta mediante la creación de una aplicación de libro de visitas muy rudimentarios. Si lo hace, veremos distintas opciones para modeli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e484f63a82ad9ecf1f376143bdc1924e231e0801
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="storing-additional-user-information-c"></a>Almacenar información de usuario adicional (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> En este tutorial se responderá a esta pregunta mediante la creación de una aplicación de libro de visitas muy rudimentarios. Si lo hace, se consultamos diferentes opciones para el modelado de información de usuario en una base de datos y, a continuación, consulte how to associate estos datos con las cuentas de usuario creado por el marco de trabajo de pertenencia.


## <a name="introduction"></a>Introducción

ASP. Framework de pertenencia de .NET ofrece una interfaz flexible para administrar los usuarios. La API de pertenencia incluye métodos para validar las credenciales, recuperar información sobre el usuario ha iniciado sesión actualmente, crear una nueva cuenta de usuario y eliminar una cuenta de usuario, entre otros. Cada cuenta de usuario en el marco de trabajo de pertenencia contiene solo las propiedades necesarias para validar las credenciales y realizar tareas relacionadas con la cuenta de usuario esenciales. Esto se hace patente en los métodos y propiedades de la [ `MembershipUser` clase](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que modela una cuenta de usuario en el marco de trabajo de pertenencia. Esta clase tiene propiedades como [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), y [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), y métodos como [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) y [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

A menudo, las aplicaciones deben almacenar información de usuario adicional que no se incluye en el marco de trabajo de pertenencia. Por ejemplo, necesite un comerciante en línea para que cada usuario pueda almacenar sus direcciones de envío y facturación, información de pago y las preferencias de entrega y póngase en contacto con el número de teléfono. Además, cada pedido en el sistema está asociado a una cuenta de usuario determinada.

El `MembershipUser` clase no incluye las propiedades como `PhoneNumber` o `DeliveryPreferences` o `PastOrders`. Entonces, ¿cómo se realizar un seguimiento de la información de usuario necesaria para la aplicación y hacer que se integran con el marco de trabajo de la pertenencia a? En este tutorial se responderá a esta pregunta mediante la creación de una aplicación de libro de visitas muy rudimentarios. Si lo hace, se consultamos diferentes opciones para el modelado de información de usuario en una base de datos y, a continuación, consulte how to associate estos datos con las cuentas de usuario creado por el marco de trabajo de pertenencia. Comencemos.

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Paso 1: Crear el modelo de datos de la aplicación libro de visitas

Hay diversas técnicas que se pueden emplear para capturar la información de usuario en una base de datos y asociarlo con las cuentas de usuario creado por el marco de trabajo de pertenencia. Para ilustrar estas técnicas, necesitaremos intensificar la aplicación web tutorial para que capture a algún tipo de datos relacionados con el usuario. (Actualmente, el modelo de datos de la aplicación contiene solo la aplicación de servicios de las tablas que necesita la `SqlMembershipProvider`.)

Vamos a crear una aplicación de libro de visitas muy simple donde un usuario autenticado puede dejar un comentario. Además de almacenar el libro de visitas comentarios, vamos a permitir que cada usuario almacenar su ciudad natal, la página principal y la firma. Si se proporciona, ciudad natal del usuario, página principal y la firma aparecerá en cada mensaje que ha dejado en el libro de visitas.

### <a name="adding-theguestbookcommentstable"></a>Agregar el`GuestbookComments`tabla

Para capturar los comentarios del libro de visitas, necesitamos crear una tabla de base de datos denominada `GuestbookComments` que tiene columnas como `CommentId`, `Subject`, `Body`, y `CommentDate`. También es necesario que cada registro en el `GuestbookComments` referencia al usuario que deja el comentario de la tabla.

Para agregar esta tabla a nuestra base de datos, vaya al explorador de base de datos en Visual Studio y explorar en profundidad el `SecurityTutorials` base de datos. Haga doble clic en la carpeta de tablas y elija Agregar nueva tabla. Se abrirá una interfaz que nos permite definir las columnas para la nueva tabla.


[![Agregar una nueva tabla a la base de datos SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Figura 1**: agregar una nueva tabla a la `SecurityTutorials` base de datos ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image3.png))


A continuación, defina el `GuestbookComments`de columnas. Empiece por agregar una columna denominada `CommentId` de tipo `uniqueidentifier`. Esta columna se identificar de forma exclusiva cada comentario en el libro de visitas, por lo que no permitir `NULL` s y marcarla como clave principal de la tabla. En lugar de proporcionar un valor para el `CommentId` campo en cada `INSERT`, podemos indicar que un nuevo `uniqueidentifier` valor se debe generar automáticamente para este campo en `INSERT` estableciendo el valor predeterminado de la columna en `NEWID()`. Después de agregar este primer campo, marcarlo como la clave principal y la configuración de su valor predeterminado, la pantalla debe ser similar a la pantalla se muestra en la figura 2.


[![Agregar una columna principal denominada CommentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Figura 2**: agregar una columna principal denomina `CommentId` ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image6.png))


A continuación, agregue una columna denominada `Subject` de tipo `nvarchar(50)` y una columna denominada `Body` de tipo `nvarchar(MAX)`, desautorizar `NULL` s en ambas columnas. A continuación, agregar una columna denominada `CommentDate` de tipo `datetime`. No permitir `NULL` s y establezca el `CommentDate` valor predeterminado de la columna a `getdate()`.

Todo lo que queda es agregar una columna que se asocia a una cuenta de usuario de cada libro de visitas comentario. Sería una opción Agregar una columna denominada `UserName` de tipo `nvarchar(256)`. Esto es una opción apropiada cuando se usa un proveedor de pertenencia distinto de la `SqlMembershipProvider`. Pero si se usa el `SqlMembershipProvider`, tal y como se encuentran en esta serie de tutoriales, la `UserName` columna en el `aspnet_Users` tabla no se garantiza que sea único. El `aspnet_Users` clave principal de la tabla es `UserId` y es del tipo `uniqueidentifier`. Por lo tanto, la `GuestbookComments` tabla tiene una columna denominada `UserId` de tipo `uniqueidentifier` (no se permiten `NULL` valores). Continúe y agregar esta columna.

> [!NOTE]
> Como se explicó en la [ *crear el esquema de pertenencia en SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) tutorial, el marco de trabajo de pertenencia está diseñado para permitir que varias aplicaciones web con distintas cuentas de usuario para que compartan el mismo almacén de usuario. Hace esto mediante la partición de las cuentas de usuario en aplicaciones diferentes. Y mientras se garantiza que cada nombre de usuario que ser únicos dentro de una aplicación, puede usar el mismo nombre de usuario en diferentes aplicaciones usando el mismo almacén de usuario. Hay un compuesto `UNIQUE` restricción en la `aspnet_Users` de tabla en la `UserName` y `ApplicationId` campos, pero no en simplemente el `UserName` campo. Por lo tanto, es posible que aspnet\_tabla a los usuarios tener dos (o más) de los registros con el mismo `UserName` valor. Sin embargo, hay un `UNIQUE` restricción en la `aspnet_Users` la tabla `UserId` campo (ya que es la clave principal). A `UNIQUE` restricción es importante porque sin se puede establecer una restricción de clave externa entre la `GuestbookComments` y `aspnet_Users` tablas.


Después de agregar la `UserId` columna, guarde la tabla haciendo clic en el icono Guardar en la barra de herramientas. Llame a la nueva tabla `GuestbookComments`.

Tenemos un último problema que ocuparse de con el `GuestbookComments` tabla: se necesita crear un [restricción foreign key](https://msdn.microsoft.com/library/ms175464.aspx) entre el `GuestbookComments.UserId` columna y la `aspnet_Users.UserId` columna. Para ello, haga clic en el icono de relación en la barra de herramientas para iniciar el cuadro de diálogo de relaciones de clave externa. (Como alternativa, puede iniciar este cuadro de diálogo, en el menú Diseñador de tablas y elija relaciones.)

Haga clic en el botón Agregar en la esquina inferior izquierda del cuadro de diálogo de relaciones de clave externa. Esto agregará una nueva restricción foreign key, aunque todavía necesitamos definir las tablas que participan en la relación.


[![Utilice el cuadro de diálogo de relaciones de clave externa para administrar las restricciones de clave externa de una tabla](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Figura 3**: usar el cuadro de diálogo de relaciones de clave externa para administrar las restricciones de clave externa de una tabla ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image9.png))


A continuación, haga clic en el icono de botón de puntos suspensivos en la fila "Especificaciones de tablas y columnas" a la derecha. Esto iniciará el cuadro de diálogo tablas y columnas, desde el que se puede especificar la tabla de clave principal y de columna y de la columna de clave externa de la `GuestbookComments` tabla. En particular, seleccione `aspnet_Users` y `UserId` como la tabla de clave principal y la columna, y `UserId` desde el `GuestbookComments` tabla como columna de clave externa (consulte la figura 4). Después de definir las tablas de clave principales y externas y columnas, haga clic en Aceptar para volver al cuadro de diálogo de relaciones de clave externa.


[![Establecer un Foreign Key restricción entre el aspnet_Users y tablas GuesbookComments](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Figura 4**: establecer un Foreign Key restricción entre el `aspnet_Users` y `GuesbookComments` tablas ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image12.png))


En este momento se ha establecido la restricción foreign key. La presencia de esta restricción garantiza [integridad relacional](http://en.wikipedia.org/wiki/Referential_integrity) entre las dos tablas al garantizar que nunca habrá una entrada de libro de visitas que hace referencia a una cuenta de usuario no existe. De forma predeterminada, una restricción foreign key prohibir un registro principal para eliminarse si hay registros secundarios correspondientes. Es decir, si un usuario realiza uno o más comentarios de libro de visitas y, a continuación, se intenta eliminar la cuenta de usuario, se producirá un error en la eliminación a menos que sus comentarios de libro de visitas se eliminan primero.

Restricciones de clave externa se pueden configurar para que elimine automáticamente los registros secundarios asociados cuando se elimina un registro primario. En otras palabras, se puede configurar esta restricción de clave externa para que las entradas de libro de visitas de un usuario se eliminan automáticamente cuando se elimina su cuenta de usuario. Para lograr esto, expanda la sección "INSERCIÓN y actualización especificación" y establezca la propiedad "Eliminar la regla" en cascada.


[![Configurar la restricción Foreign Key para las eliminaciones en cascada](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Figura 5**: configurar la restricción de clave externa para eliminaciones en cascada ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image15.png))


Para guardar la restricción foreign key, haga clic en el botón Cerrar para salir de las relaciones de clave externa. A continuación, haga clic en el icono Guardar en la barra de herramientas para guardar la tabla y esta relación.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Almacenar Pueblo principal del usuario, página de inicio y firma

El `GuestbookComments` tabla ilustra cómo almacenar la información que comparte una relación uno a varios con cuentas de usuario. Dado que cada cuenta de usuario puede tener un número arbitrario de comentarios asociados, esta relación se modela mediante la creación de una tabla que contenga el conjunto de comentarios que incluye una columna que vínculos copia cada comentario a un usuario determinado. Cuando se usa el `SqlMembershipProvider`, este vínculo esté establecido mejor mediante la creación de una columna denominada `UserId` de tipo `uniqueidentifier` y una restricción foreign key entre esta columna y `aspnet_Users.UserId`.

Ahora necesitamos asociar tres columnas con cada cuenta de usuario para almacenar la ciudad natal, página de inicio y firma, que aparecerá en sus libro de visitas comentarios del usuario. Hay números de diferentes formas de lograr esto:

- <strong>Agregar nuevas columnas a la</strong><strong>`aspnet_Users`</strong><strong>o</strong><strong>`aspnet_Membership`</strong><strong>tablas.</strong> No recomendamos este enfoque porque modifica el esquema utilizado por el `SqlMembershipProvider`. Esta decisión puede reanudarse en su contra cabeza. Por ejemplo, ¿qué ocurre si una versión futura de ASP.NET usa otra `SqlMembershipProvider` esquema. Microsoft puede incluir una herramienta para migrar la versión 2.0 de ASP.NET `SqlMembershipProvider` datos al nuevo esquema, pero si ha modificado la versión 2.0 de ASP.NET `SqlMembershipProvider` esquema, este tipo de conversión puede no ser posible.

- **Use ASP. Framework de perfil de .NET, definir una propiedad de perfil para la ciudad natal, la página principal y la firma.** ASP.NET incluye un marco de perfil que está diseñado para almacenar datos específicos de los usuarios adicionales. Al igual que el marco de trabajo de pertenencia, el marco de trabajo de perfil se crea sobre el modelo de proveedor. .NET Framework se suministra con un `SqlProfileProvider` grados almacenan los datos de perfil en una base de datos de SQL Server. De hecho, nuestra base de datos ya tiene la tabla utilizada por el `SqlProfileProvider` (`aspnet_Profile`), tal y como se agregó al agregar los servicios de aplicación de nuevo en el <a id="_msoanchor_2"> </a> [ *crear el esquema de pertenencia en SQL Servidor* ](creating-the-membership-schema-in-sql-server-cs.md) tutorial.   
  La principal ventaja del marco de trabajo de perfil es que permite a los programadores definir las propiedades del perfil en `Web.config` : ningún código debe escribirse para serializar los datos del perfil a y desde el almacén de datos subyacente. En resumen, es muy fácil para definir un conjunto de propiedades de perfil y trabajar con ellos en el código. Sin embargo, el sistema del perfil deja mucho que se desee en cuanto a control de versiones, por lo que si tiene una aplicación que esperar nuevas propiedades específicas del usuario para agregarse a un momento posterior, o las existentes para quitar o modificar, a continuación, el marco de trabajo de perfil no puede ser el  mejor opción. Además, la `SqlProfileProvider` almacena las propiedades de perfil en un modo sin normalizar alta, lo que casi imposible ejecutar consultas directamente en los datos de perfil (por ejemplo, ¿cuántos usuarios tienen una versión particular ciudad de Nueva York).   
  Para obtener más información sobre el marco de trabajo de perfil, consulte la sección "Lecturas adicionales" al final de este tutorial.

- <strong>Agregue estas tres columnas a una nueva tabla en la base de datos y establecer una relación uno a uno entre esta tabla y</strong><strong>`aspnet_Users`</strong><strong>.</strong> Este enfoque implica un poco más trabajo con el marco de trabajo de perfil, pero proporciona la máxima flexibilidad en cómo se modelan las propiedades de usuario adicionales en la base de datos. Esta es la opción que se usará en este tutorial.

Se creará una nueva tabla denominada `UserProfiles` para guardar la ciudad natal, la página de inicio y la firma para cada usuario. Haga doble clic en la carpeta de tablas en la ventana Explorador de base de datos y elija crear una nueva tabla. Nombre de la primera columna `UserId` y establezca su tipo en `uniqueidentifier`. No permitir `NULL` valores y marcar la columna como una clave principal. A continuación, agregue las columnas denominadas: `HomeTown` de tipo `nvarchar(50)`; `HomepageUrl` de tipo `nvarchar(100)`; y la firma del tipo `nvarchar(500)`. Cada una de estas tres columnas puede aceptar un `NULL` valor.


[![Crear la tabla UserProfiles](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Figura 6**: crear el `UserProfiles` tabla ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image18.png))


Guarde la tabla y asígnele el nombre `UserProfiles`. Por último, establecer una restricción de clave externa entre la `UserProfiles` la tabla `UserId` campo y `aspnet_Users.UserId` campo. Igual que hicimos con la restricción de clave externa entre la `GuestbookComments` y `aspnet_Users` tablas, tienen esta restricción eliminaciones en cascada. Puesto que la `UserId` campo `UserProfiles` es el principal clave, esto garantiza que habrá no más de un registro en el `UserProfiles` tabla para cada cuenta de usuario. Este tipo de relación se conoce como uno a uno.

Ahora que tenemos el modelo de datos creado, estamos preparados para utilizarlo. En los pasos 2 y 3, veremos cómo puede ver y editar su información principal de ciudad, la firma y la página principal del usuario con sesión iniciada actualmente. En el paso 4, se creará la interfaz para los usuarios autenticados enviar comentarios nuevos en el libro de visitas y ver los archivos existentes.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Paso 2: Mostrar Pueblo principal del usuario, página de inicio y firma

Hay varias maneras de permitir que el usuario ha iniciado sesión actualmente ver y editar su información principal de la ciudad, la firma y la página principal. Podríamos crear manualmente la interfaz de usuario con el cuadro de texto y controles de etiqueta o se podríamos usar uno de los controles Web, como el control DetailsView de datos. Para realizar la base de datos `SELECT` y `UPDATE` instrucciones podríamos escribir ADO.NET en la clase de código subyacente de nuestra página de código o, alternativamente, emplean un enfoque declarativo con SqlDataSource. Lo ideal es que nuestra aplicación contendría una arquitectura en capas, que se pudo invocar mediante programación desde la clase de código subyacente de la página o de manera declarativa mediante el control ObjectDataSource.

Puesto que esta serie de tutoriales se centra en la autenticación de formularios, autorización, cuentas de usuario y roles, no habrá una discusión detallada de estas opciones de acceso de datos diferente o por qué una arquitectura en capas es preferible ejecutar instrucciones SQL directamente en la página ASP.NET. Voy a recorrer mediante un DetailsView y SqlDataSource: la opción más rápida y sencilla: pero por supuesto, se pueden aplicar los conceptos tratados alternativo Web controles y datos de lógica de acceso. Para obtener más información sobre cómo trabajar con datos en ASP.NET, consulte mi *[trabajar con datos en ASP.NET 2.0](../../data-access/index.md)* serie de tutoriales.

Abra la `AdditionalUserInfo.aspx` página en el `Membership` carpeta y agregar un control DetailsView a la página de configuración de su `ID` propiedad a `UserProfile` y borrando su `Width` y `Height` propiedades. Expanda la etiqueta inteligente de DetailsView y elija enlazar a un nuevo control de origen de datos. Se iniciará el Asistente para configuración de origen de datos (consulte la figura 7). El primer paso en el que se le pide que especifique el tipo de origen de datos. Puesto que vamos a conectar directamente a la `SecurityTutorials` base de datos, elija el icono de la base de datos, especificar el `ID` como `UserProfileDataSource`.


[![Agregar un nuevo Control SqlDataSource denominado UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Figura 7**: agregar un nuevo Control SqlDataSource denominado `UserProfileDataSource` ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image21.png))


La siguiente pantalla solicita la base de datos. Ya hemos definido una cadena de conexión en `Web.config` para el `SecurityTutorials` base de datos. Este nombre de la cadena de conexión: `SecurityTutorialsConnectionString` : debe estar en la lista desplegable. Seleccione esta opción y haga clic en siguiente.


[![Elija SecurityTutorialsConnectionString en la lista desplegable](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Figura 8**: elija `SecurityTutorialsConnectionString` en la lista desplegable ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image24.png))


La pantalla posterior en el que se le pide que especifique la tabla y columnas de la consulta. Elija la `UserProfiles` de la tabla en la lista desplegable y compruebe todas las columnas.


[![Poner hacer copia de todas las columnas de la tabla UserProfiles](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Figura 9**: poner todo en espera de las columnas de la `UserProfiles` tabla ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image27.png))


La consulta actual en la figura 9 devuelve *todos los* de los registros de `UserProfiles`, pero solo estamos interesados en el registro del usuario ha iniciado sesión actualmente. Para agregar una `WHERE` cláusula, haga clic en el `WHERE` botón para que aparezca el complemento `WHERE` cuadro de diálogo de la cláusula (consulte la figura 10). Aquí puede seleccionar la columna que se va a filtrar, el operador y el origen del parámetro filter. Seleccione `UserId` como la columna y "=" como el operador.

Lamentablemente, no hay ningún origen de los parámetros integrados para devolver el usuario ha iniciado sesión actualmente `UserId` valor. Necesitamos agarre este valor mediante programación. Por tanto, establecer la lista desplegable de origen en "None", haga clic en Agregar botón para agregar el parámetro y, a continuación, haga clic en Aceptar.


[![Agregar un parámetro de filtro en la columna de identificador de usuario](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Figura 10**: agregar un parámetro de filtro en el `UserId` columna ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image30.png))


Tras hacer clic en Aceptar volverá a la pantalla que se muestra en la figura 9. Esta vez, sin embargo, la consulta SQL en la parte inferior de la pantalla debe incluir un `WHERE` cláusula. Haga clic en siguiente para pasar a la pantalla de "Consulta de prueba". Aquí puede ejecutar la consulta y ver los resultados. Haga clic en Finalizar para completar al asistente.

Al finalizar al Asistente de configuración de origen de datos, Visual Studio crea el control SqlDataSource según la configuración especificada en el asistente. Además, agrega manualmente BoundFields a DetailsView para cada columna devuelta por la SqlDataSource `SelectCommand`. No es necesario para mostrar el `UserId` campo DetailsView, puesto que el usuario no necesita conocer este valor. Puede quitar este campo directamente desde el marcado declarativo del control DetailsView o haciendo clic en "Editar campos" vincular desde su etiqueta inteligente.

En este momento marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Es necesario establecer mediante programación el control SqlDataSource `UserId` parámetro para el usuario conectado actualmente `UserId` antes de que los datos están seleccionados. Esto puede realizarse mediante la creación de un controlador de eventos para el SqlDataSource `Selecting` eventos y agregar el siguiente código no existe:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

El código anterior se inicia mediante la obtención de una referencia al usuario con sesión iniciada actualmente mediante una llamada a la `Membership` la clase `GetUser` método. Esto devuelve un `MembershipUser` objeto cuya `ProviderUserKey` propiedad contiene el `UserId`. El `UserId` , a continuación, se asigna el valor a la SqlDataSource `@UserId` parámetro.

> [!NOTE]
> El `Membership.GetUser()` método devuelve información sobre el usuario ha iniciado sesión actualmente. Si un usuario anónimo visita la página, devolverá un valor de `null`. En tal caso, esto dará lugar a un `NullReferenceException` en la siguiente línea de código al intentar leer la `ProviderUserKey` propiedad. Por supuesto, no es necesario preocuparse de `Membership.GetUser()` devolver un `null` valor en el `AdditionalUserInfo.aspx` página porque hemos configurado autorizaciones de direcciones URL en un tutorial anterior, por lo que sólo los usuarios autenticados podrían tener acceso a los recursos ASP.NET en esta carpeta. Si necesita tener acceso a información sobre el usuario ha iniciado sesión actualmente en una página donde se permite el acceso anónimo, asegúrese de comprobar que no`null MembershipUser` objeto se devuelve desde el `GetUser()` método antes de hacer referencia a sus propiedades.


Si visita el `AdditionalUserInfo.aspx` página a través de un explorador verá una página en blanco porque todavía tenemos que agregar ninguna fila para el `UserProfiles` tabla. En el paso 6, veremos cómo personalizar el control CreateUserWizard para agregar automáticamente una nueva fila a la `UserProfiles` cuando se crea una nueva cuenta de usuario de la tabla. Por ahora, sin embargo, se tendrá que crear manualmente un registro en la tabla.

Navegue hasta el Explorador de base de datos en Visual Studio y expanda la carpeta tablas. Haga doble clic en el `aspnet_Users` de tabla y elegir "Mostrar datos de tabla" para ver los registros en la tabla; hacer lo mismo la `UserProfiles` tabla. La figura 11 muestra estos resultados cuando se coloca en mosaico vertical. En mi base de datos hay actualmente `aspnet_Users` registros para Bruce, Fred y Tito, pero no hay registros en la `UserProfiles` tabla.


[![Se muestran el contenido de la aspnet_Users y tablas UserProfiles](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Figura 11**: el contenido de la `aspnet_Users` y `UserProfiles` se mostrarán las tablas ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image33.png))


Agregue un nuevo registro a la `UserProfiles` tabla escribiendo manualmente en los valores para la `HomeTown`, `HomepageUrl`, y `Signature` campos. La manera más fácil de obtener válido `UserId` valor en el nuevo `UserProfiles` registro consiste en seleccionar la `UserId` arrastrándolo desde una cuenta de usuario determinada en el `aspnet_Users` de tabla y copiar y pegar en el `UserId` campo `UserProfiles`. La figura 12 muestra el `UserProfiles` tabla después de que se ha agregado un nuevo registro para Bruce.


[![Se agregó un registro a UserProfiles para Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Figura 12**: se agregó un registro a `UserProfiles` para Bruce ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image36.png))


Vuelva a la `AdditionalUserInfo.aspx` página, ha registrado en Bruce. Tal y como se muestra en la figura 13, se muestran los valores de Bruce.


[![El usuario visita actualmente es la configuración de His se muestra](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Figura 13**: el de usuario actualmente visitar es se muestra la configuración His ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image39.png))


> [!NOTE]
> Ir hacia delante y manualmente agregar registros en la `UserProfiles` tabla para cada usuario de pertenencia. En el paso 6, veremos cómo personalizar el control CreateUserWizard para agregar automáticamente una nueva fila a la `UserProfiles` cuando se crea una nueva cuenta de usuario de la tabla.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Paso 3: Permitir al usuario editar su ciudad principal, la página principal y la firma

En este momento puede ver el usuario que ha iniciado en su ciudad natal, la página principal y la configuración de firma, pero aún no pueden modificar. Vamos a actualizar el control DetailsView para que se pueden editar los datos.

Lo primero que debemos hacer es agregar un `UpdateCommand` para SqlDataSource, especificando el `UPDATE` instrucción que se ejecutará y sus correspondientes parámetros. Seleccione el SqlDataSource y, desde la ventana Propiedades, haga clic en el botón de puntos suspensivos situado junto a la propiedad UpdateQuery para que aparezca el cuadro de diálogo Editor de parámetros y comandos. Escriba el siguiente `UPDATE` instrucción en el cuadro de texto:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

A continuación, haga clic en el botón "Actualizar parámetros", lo cual creará un parámetro en el control SqlDataSource `UpdateParameters` recopilación para cada uno de los parámetros en el `UPDATE` instrucción. Deje el origen para todos los parámetros establecidos en ninguno y haga clic en el botón Aceptar para completar el cuadro de diálogo.


[![Especifique el SqlDataSource UpdateCommand y UpdateParameters](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Figura 14**: especifique el SqlDataSource `UpdateCommand` y `UpdateParameters` ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image42.png))


Debido a las adiciones hemos realizado para el control SqlDataSource, DetailsView control puede ahora admite la edición. De las etiquetas inteligentes de DetailsView, active la casilla "Habilitar edición". Esto agrega un CommandField para el control `Fields` colección con su `ShowEditButton` propiedad establecida en True. Esto hace que un botón Editar cuando DetailsView se muestra en modo de solo lectura y actualización y los botones Cancelar cuando se muestre en el modo de edición. En lugar de requerir al usuario que haga clic en Editar, sin embargo, podemos tenemos la presentación de DetailsView en un estado "siempre editable" estableciendo el control DetailsView [ `DefaultMode` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) a `Edit`.

Con estos cambios, el marcado declarativo del control DetailsView debe ser similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Tenga en cuenta la adición de la CommandField y `DefaultMode` propiedad.

Continúe y probar esta página a través de un explorador. Cuando se visita con un usuario que tiene un registro correspondiente en `UserProfiles`, la configuración del usuario se muestra en una interfaz editable.


[![DetailsView representa una interfaz Editable](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Figura 15**: DetailsView representa una interfaz Editable ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image45.png))


Intente cambiar los valores y haga clic en el botón de actualización. Parece como si no ocurre nada. Hay una devolución de datos y los valores se guardan en la base de datos, pero no aparece ningún indicador visual que se produjeron al guardar.

Para solucionarlo, vuelva a Visual Studio y agregar un control de etiqueta por encima de DetailsView. Establecer su `ID` a `SettingsUpdatedMessage`, sus `Text` propiedad "se ha actualizado la configuración de" y su `Visible` y `EnableViewState` propiedades para `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Es necesario mostrar el `SettingsUpdatedMessage` etiquetar cada vez que se actualiza el DetailsView. Para ello, cree un controlador de eventos para el DetailsView `ItemUpdated` eventos y agregue el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Vuelva a la `AdditionalUserInfo.aspx` página a través de un explorador y actualizar los datos. Esta vez, se muestra un mensaje de estado útil.


[![Un mensaje breve es aparece cuando los valores se actualizan](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Figura 16**: un breve mensaje se muestra cuando la configuración se actualiza ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image48.png))


> [!NOTE]
> El control DetailsView de edición del interfaz deja mucho que se desee. Usa cuadros de texto de tamaño estándar, pero el campo de firma probablemente debe ser un cuadro de texto de varias líneas. Un control RegularExpressionValidator debe usarse para asegurarse de que la dirección URL de página principal, si no escribe, comienza con "http://" o "https://". Además, desde el DetailsView control tiene su `DefaultMode` propiedad establecida en `Edit`, el botón Cancelar no hace nada. Debería cualquiera quitarse o, al hacer clic, redirigirá al usuario a otra página (como `~/Default.aspx`). Estas mejoras deja como un ejercicio para el lector.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Agregar un vínculo a la`AdditionalUserInfo.aspx`página en la página maestra

Actualmente, el sitio Web no proporciona los vínculos a la `AdditionalUserInfo.aspx` página. La única forma de llegar a él consiste en especificar dirección URL de la página directamente en la barra de direcciones del explorador. Vamos a agregar un vínculo a esta página en la `Site.master` página maestra.

Recuerde que la página maestra contiene un control LoginView Web en su `LoginContent` ContentPlaceHolder que muestra un marcado diferente para los visitantes autenticados y anónimos. Actualizar el control LoginView `LoggedInTemplate` para incluir un vínculo a la `AdditionalUserInfo.aspx` página. Después de realizar estos cambios el LoginView marcado declarativo del control debe ser similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Tenga en cuenta la adición de la `lnkUpdateSettings` control de hipervínculo para el `LoggedInTemplate`. Con este vínculo en su lugar, los usuarios autenticados pueden saltar rápidamente a la página para ver y modificar su configuración particular de ciudad, la firma y la página principal.

## <a name="step-4-adding-new-guestbook-comments"></a>Paso 4: Agregar comentarios nuevos de libro de visitas

La `Guestbook.aspx` página es donde los usuarios autenticados pueden ver el libro de visitas y deje un comentario. Puede empezar con la creación de la interfaz para agregar comentarios de libro de visitas de nuevo.

Abra la `Guestbook.aspx` página en Visual Studio y crear una interfaz de usuario que se compone de dos controles de cuadro de texto, uno para el sujeto del nuevo comentario y otro para el cuerpo. Configurar el primer control de cuadro de texto `ID` propiedad `Subject` y su `Columns` propiedad a 40; establezca segundo `ID` a `Body`, sus `TextMode` a `MultiLine`y su `Width` y `Rows` propiedades para "95%" y 8, respectivamente. Para completar la interfaz de usuario, agregue un control de botón Web denominado `PostCommentButton` y establecer su `Text` propiedad en "Comment Your" Post".

Puesto que cada comentario del libro de visitas requiere un asunto y al cuerpo, agregue un control RequiredFieldValidator para cada uno de los cuadros de texto. Establecer el `ValidationGroup` propiedad de estos controles para "EnterComment"; del mismo modo, establezca la `PostCommentButton` del control `ValidationGroup` propiedad a "EnterComment". Para obtener más información acerca de ASP. Controles de validación de NET, consulte [validación del formulario en ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [Diseccionando los controles de validación en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)y el [Tutorial de controles de servidor de validación](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) en [W3Schools](http://www.w3schools.com/).

Después de diseñar la interfaz de usuario marcado declarativo de la página debe tener un aspecto similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Con la interfaz de usuario completa, la siguiente tarea consiste en Insertar un nuevo registro en el `GuestbookComments` tabla cuando el `PostCommentButton` se hace clic en. Esto puede realizarse de varias maneras: podemos escribir código de ADO.NET en el botón `Click` controlador de eventos; se puede agregar un control SqlDataSource a la página, configure su `InsertCommand`y, a continuación, llame a su `Insert` método desde el `Click` eventos controlador; o podríamos crear un nivel intermedio que era responsable de insertar nuevos comentarios de libro de visitas y esta funcionalidad invocar desde el `Click` controlador de eventos. Puesto que analizamos utilizando un SqlDataSource en el paso 3, vamos a usar código de ADO.NET aquí.

> [!NOTE]
> Las clases ADO.NET permiten el acceso mediante programación a datos desde una base de datos de Microsoft SQL Server se encuentran en el `System.Data.SqlClient` espacio de nombres. Puede que necesite importar este espacio de nombres en la clase de código subyacente de la página (es decir, `using System.Data.SqlClient;`).


Crear un controlador de eventos para el `PostCommentButton`de `Click` eventos y agregue el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

El `Click` controlador de eventos se inicia mediante la comprobación de que los datos proporcionados por el usuario están válidos. Si no es así, el controlador de eventos se cierra antes de insertar un registro. Suponiendo que los datos proporcionados son válidos, el usuario ha iniciado sesión actualmente `UserId` valor se recuperan y almacenan en la `currentUserId` variable local. Este valor es necesario porque se debemos proporcionar un `UserId` valor al insertar un registro en `GuestbookComments`.

A continuación, la cadena de conexión para el `SecurityTutorials` base de datos se recupera de `Web.config` y `INSERT` se especifica la instrucción SQL. Un `SqlConnection` objeto, a continuación, se crea y se abre. Después, un `SqlCommand` se construye el objeto y los valores de los parámetros que se usan en el `INSERT` consulta se han asignado. El `INSERT` , a continuación, se ejecuta la instrucción y cierra la conexión. Al final del controlador de eventos, el `Subject` y `Body` cuadros de texto `Text` se borran propiedades para que no se conservan los valores del usuario a través de la devolución de datos.

Continúe y pruebe esta página en un explorador. Puesto que esta página en el `Membership` la carpeta no es accesible para los visitantes anónimos. Por lo tanto, debe iniciar sesión (si aún no lo tiene). Especifique un valor para el `Subject` y `Body` cuadros de texto y haga clic en el `PostCommentButton` botón. Esto hará que un nuevo registro va a agregar a `GuestbookComments`. En la devolución de datos, el asunto y cuerpo proporcionada se borran de los cuadros de texto.

Tras hacer clic en el `PostCommentButton` no existe el botón no aparece ningún indicador visual que se agregó el comentario en el libro de visitas. Necesitamos actualizar esta página para mostrar los comentarios del libro de visitas existentes, lo hará en el paso 5. Una vez que lo, el comentario just-agregado aparecerá en la lista de los comentarios, proporcionar comentarios visuales adecuados. Por ahora, confirme que se guardó el comentario del libro de visitas examinando el contenido de la `GuestbookComments` tabla.

Figura 17 muestra el contenido de la `GuestbookComments` tabla después de que se han dejado dos comentarios.


[![Puede ver los comentarios del libro de visitas en la tabla GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Figura 17**: puede ver los comentarios del libro de visitas en el `GuestbookComments` tabla ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image51.png))


> [!NOTE]
> Si un usuario intenta insertar un comentario de libro de visitas que contiene potencialmente peligroso marcado: por ejemplo, HTML, ASP.NET producirá una `HttpRequestValidationException`. Para obtener más información sobre esta excepción, ¿por qué se produce, y cómo permitir a los usuarios enviar valores potencialmente peligrosos, consulte el [solicitar notas del producto validación](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Paso 5: Mostrar los comentarios existentes de libro de visitas

Además de dejar comentarios, un usuario que visita el `Guestbook.aspx` página también podrá ver los comentarios existentes del libro de visitas. Para ello, agregue un control ListView denominado `CommentList` a la parte inferior de la página.

> [!NOTE]
> ListView (control) es nuevo en ASP.NET versión 3.5. Está diseñado para mostrar una lista de elementos en un diseño muy flexible y personalizable, pero aún siguen ofreciendo integrados editar, insertar, eliminar, paginación y ordenación funcionalidades como GridView. Si usa ASP.NET 2.0, debe utilizar el control DataList o repetidor en su lugar. Para obtener más información sobre el uso de la vista de lista, vea [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog, [asp: ListView Control](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)y el artículo [mostrar datos con el ListView Control](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Abra la etiqueta inteligente de la vista de lista y, en la lista desplegable Elegir origen de datos, enlazar el control a un nuevo origen de datos. Como vimos en el paso 2, se iniciará al Asistente para configuración de orígenes de datos. Seleccione el icono de la base de datos, asigne el nombre resultante SqlDataSource `CommentsDataSource`y haga clic en Aceptar. A continuación, seleccione la `SecurityTutorialsConnectionString` conexión cadena en la lista desplegable y haga clic en siguiente.

En este momento en el paso 2 se especificó los datos para consultar por selección la `UserProfiles` de la tabla en la lista desplegable y seleccione las columnas que se va a devolver (hacen referencia a la figura 9). Esta vez, sin embargo, desea elaborar una instrucción SQL que extrae los no solo los registros de `GuestbookComments`, pero también del comentarista ciudad natal, página principal, la firma y nombre de usuario. Por lo tanto, seleccione el botón de opción "Especificar una instrucción SQL personalizada o un procedimiento almacenado" y haga clic en siguiente.

Se abrirá la pantalla "Definir instrucciones o procedimientos almacenados personalizados". Haga clic en el botón Generador de consultas para crear gráficamente la consulta. El generador de consultas, empiece por pide para especificar las tablas que desea consultar en. Seleccione el `GuestbookComments`, `UserProfiles`, y `aspnet_Users` tablas y haga clic en Aceptar. Esto agregará las tres tablas a la superficie de diseño. Dado que hay restricciones de clave externa entre la `GuestbookComments`, `UserProfiles`, y `aspnet_Users` tablas, el generador de consultas automáticamente `JOIN` s estas tablas.

Todo lo que queda es para especificar las columnas que se va a devolver. Desde el `GuestbookComments` tabla seleccione la `Subject`, `Body`, y `CommentDate` columnas; devuelven el `HomeTown`, `HomepageUrl`, y `Signature` columnas de la `UserProfiles` tabla; y devolver `UserName` desde `aspnet_Users`. Además, agregue "`ORDER BY CommentDate DESC`" al final de la `SELECT` consultas de modo que los mensajes más recientes se devuelvan en primer lugar. Después de realizar estas selecciones, la interfaz del generador de consultas debe ser similar a la pantalla en la figura 18.


[![La consulta construidos combina el GuestbookComments, UserProfiles y aspnet_Users tablas](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Figura 18**: la consulta construye `JOIN` s el `GuestbookComments`, `UserProfiles`, y `aspnet_Users` tablas ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image54.png))


Haga clic en Aceptar para cerrar la ventana del generador de consultas y volver a la pantalla "Definir instrucciones o procedimientos almacenados personalizados". Haga clic en siguiente para avanzar a la pantalla de "Consulta de prueba", donde puede ver los resultados de la consulta haciendo clic en el botón de consulta de prueba. Cuando esté listo, haga clic en Finalizar para completar al Asistente para configurar orígenes de datos.

Cuando se completa el asistente Configurar origen de datos en el paso 2, el control DetailsView asociado `Fields` recopilación se actualizó para incluir un BoundField para cada columna devuelta por la `SelectCommand`. Sin embargo, el control ListView, permanece intacto; es necesario definir su diseño. Diseño de la vista de lista se puede construir manualmente a través de su marcado declarativo o desde la opción "Configurar ListView" en su etiqueta inteligente. Normalmente prefieren definir manualmente el marcado pero use el método que resulte más natural.

Ha terminado con los siguientes `LayoutTemplate`, `ItemTemplate`, y `ItemSeparatorTemplate` para mi control ListView de formularios:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

El `LayoutTemplate` define el marcado emitidos por el control, mientras el `ItemTemplate` procesa cada elemento devuelto por la SqlDataSource. El `ItemTemplate`del marcado resultante se coloca en el `LayoutTemplate`del `itemPlaceholder` control. Además el `itemPlaceholder`, el `LayoutTemplate` incluye un control DataPager, lo que limita la vista de lista para mostrar solo 10 comentarios de libro de visitas por página (valor predeterminado) y representa una interfaz de paginación.

Mi `ItemTemplate` muestra el asunto del comentario de cada libro de visitas en un `<h4>` elemento con el cuerpo situado por debajo del sujeto. Tenga en cuenta que la sintaxis utilizada para mostrar el cuerpo de la toma los datos devueltos por la `Eval("Body")` instrucción de enlace de datos, lo convierte en una cadena y los saltos de línea reemplaza con la `<br />` elemento. Esta conversión es necesario para mostrar los saltos de línea especificados al enviar el comentario, ya que omite el espacio en blanco HTML. Firma del usuario se muestra debajo el cuerpo en cursiva, seguido por ciudad natal del usuario, un vínculo a su página de inicio, la fecha y hora en que se realiza el comentario y el nombre de usuario de la persona que deja el comentario.

Tómese un momento para ver la página a través de un explorador. Debería ver los comentarios que agregó en el libro de visitas en el paso 5 aparecerá aquí.


[![GuesBook.aspx ahora muestra los comentarios del libro de visitas](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Figura 19**: `Guestbook.aspx` ahora muestra los comentarios del libro de visitas ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image57.png))


Intente agregar un nuevo comentario para el libro de visitas. Al hacer clic en el `PostCommentButton` responde botón de la página y el comentario se agrega a la base de datos, pero el control ListView no se actualiza para mostrar el nuevo comentario. Esto puede corregirse, ya sea:

- Actualizando el `PostCommentButton` del botón `Click` controlador de eventos para el control ListView, invoca ese TI `DataBind()` método después de insertar el nuevo comentario en la base de datos, o
- Al establecer el control ListView `EnableViewState` propiedad `false`. Este enfoque funciona porque al deshabilitar el estado de vista del control, debe volver a enlazar a los datos subyacentes en cada postback.

El sitio Web del tutorial descargable de este tutorial muestra ambas técnicas. El control ListView `EnableViewState` propiedad a `false` y el código necesario para enlazar mediante programación los datos a la vista de lista está presente en el `Click` controlador de eventos, pero está comentada.

> [!NOTE]
> Actualmente la `AdditionalUserInfo.aspx` página permite al usuario ver y editar su configuración particular de ciudad, la firma y la página principal. Sería estupendo actualizar `AdditionalUserInfo.aspx` para mostrar el inicio de sesión en los comentarios del libro de visitas del usuario. Es decir, además de examinar y modificar sus datos, un usuario puede visitar el `AdditionalUserInfo.aspx` página para ver el libro de visitas comentarios que se realiza en el pasado. Dejar este como un ejercicio para el lector interesado.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Paso 6: Personalizar el Control CreateUserWizard para incluir una interfaz para la ciudad de inicio, la página principal y la firma

El `SELECT` consulta usada por la `Guestbook.aspx` página usa un `INNER JOIN` para combinar los registros relacionados entre el `GuestbookComments`, `UserProfiles`, y `aspnet_Users` tablas. Si un usuario que no tiene ningún registro en `UserProfiles` hace un libro de visitas de comentario, el comentario no se mostrarán en la vista de lista porque la `INNER JOIN` sólo devuelve `GuestbookComments` registros cuando hay registros coincidentes en `UserProfiles` y `aspnet_Users`. Y como hemos visto en el paso 3, si un usuario no tiene un registro `UserProfiles` no se puede ver o editar su configuración en la `AdditionalUserInfo.aspx` página.

Sobra decir debido a nuestro diseño decisiones es importante que cada cuenta de usuario en el sistema de pertenencia tiene la correspondiente registran en el `UserProfiles` tabla. Lo que queremos es para que un registro correspondiente para agregarse a `UserProfiles` cada vez que se crea una nueva cuenta de usuario de pertenencia a través de CreateUserWizard.

Como se describe en el [ *crear cuentas de usuario* ](creating-user-accounts-cs.md) tutorial, después de la nueva cuenta de usuario de pertenencia se crea el control CreateUserWizard genera su [ `CreatedUser` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Podemos crear un controlador de eventos para este evento, obtener el identificador de usuario para el usuario recién creado y, a continuación, insertar un registro en el `UserProfiles` tabla con valores predeterminados para la `HomeTown`, `HomepageUrl`, y `Signature` columnas. Es más, es posible solicitar al usuario para estos valores mediante la personalización de la interfaz del control CreateUserWizard para incluir cuadros de texto adicionales.

Veamos primero cómo agregar una nueva fila a la `UserProfiles` tabla el `CreatedUser` controlador de eventos con valores predeterminados. A continuación, veremos cómo personalizar la interfaz de usuario del control CreateUserWizard para incluir campos de formulario adicionales para recopilar ciudad natal del nuevo usuario, página de inicio y la firma.

### <a name="adding-a-default-row-touserprofiles"></a>Agregar una fila predeterminada para`UserProfiles`

En el [ *crear cuentas de usuario* ](creating-user-accounts-cs.md) tutorial hemos agregado un control CreateUserWizard a la `CreatingUserAccounts.aspx` página en el `Membership` carpeta. Para disponer de CreateUserWizard control agregar un registro a `UserProfiles` tabla tras la creación de cuentas de usuario, deberá actualizar la funcionalidad del control CreateUserWizard. En lugar de realizar estos cambios a la `CreatingUserAccounts.aspx` página, vamos a agregar en su lugar un nuevo control CreateUserWizard a `EnhancedCreateUserWizard.aspx` página y realice las modificaciones para este tutorial no existe.

Abra la `EnhancedCreateUserWizard.aspx` página en Visual Studio y arrastre un control CreateUserWizard del cuadro de herramientas hasta la página. Establecer el control CreateUserWizard `ID` propiedad `NewUserWizard`. Como se explicó en la <a id="_msoanchor_5"> </a> [ *crear cuentas de usuario* ](creating-user-accounts-cs.md) tutorial, interfaz de usuario predeterminada del control CreateUserWizard pide al visitante la información necesaria. Una vez que se ha proporcionado esta información, el control crea internamente una nueva cuenta de usuario en el marco de trabajo de pertenencia, todo ello sin nos tener que escribir una sola línea de código.

El control CreateUserWizard eleva un número de eventos durante su flujo de trabajo. Después de un visitante proporciona información sobre la solicitud y envía el formulario, el control CreateUserWizard inicialmente se activa su [ `CreatingUser` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Si hay un problema durante el proceso de creación, el [ `CreateUserError` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) se activa; sin embargo, si el usuario se creó correctamente, la [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) se genera. En el <a id="_msoanchor_6"> </a> [ *crear cuentas de usuario* ](creating-user-accounts-cs.md) tutorial hemos creado un controlador de eventos para el `CreatingUser` eventos para asegurarse de que el nombre de usuario proporcionado no contenía ninguna inicial o espacios finales, y que el nombre de usuario no ha aparecido en cualquier parte en la contraseña.

Para agregar una fila en la `UserProfiles` tabla para el usuario recién creado, es necesario crear un controlador de eventos para el `CreatedUser` eventos. En el momento en el `CreatedUser` se desencadena el evento, la cuenta de usuario ya se ha creado en el marco de trabajo de pertenencia, lo que nos permite recuperar el valor de identificador de usuario de la cuenta.

Crear un controlador de eventos para el `NewUserWizard`de `CreatedUser` eventos y agregue el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Seres de código anterior al recuperar el identificador de usuario de la cuenta de usuario recién agregados. Esto se logra mediante la `Membership.GetUser(username)` método para devolver información sobre un usuario determinado y, a continuación, usar el `ProviderUserKey` propiedad que recupere su identificador de usuario. El nombre de usuario especificado por el usuario en el control CreateUserWizard está disponible a través de su [ `UserName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

A continuación, se recupera la cadena de conexión de `Web.config` y `INSERT` se especifica la instrucción. Se crean instancias de los objetos necesarios de ADO.NET y ejecuta el comando. El código asigna un [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) de instancia para la `@HomeTown`, `@HomepageUrl`, y `@Signature` parámetros, que tiene el efecto de inserción de base de datos `NULL` los valores para el `HomeTown`, `HomepageUrl`, y `Signature` campos.

Visite la `EnhancedCreateUserWizard.aspx` página a través de un explorador y crear una nueva cuenta de usuario. Una vez hecho esto, vuelva a Visual Studio y examine el contenido de la `aspnet_Users` y `UserProfiles` tablas (al igual que hicimos en la figura 12). Debería ver la nueva cuenta de usuario en `aspnet_Users` y su correspondiente `UserProfiles` fila (con `NULL` los valores de `HomeTown`, `HomepageUrl`, y `Signature`).


[![Se han agregado una nueva cuenta de usuario y el registro UserProfiles](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Figura 20**: una cuenta de usuario nueva y `UserProfiles` registro se han agregado ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image60.png))


Después de que el visitante ha proporcionado la información de su cuenta nueva y hace clic en el botón "Crear usuario", la cuenta de usuario se crea y agrega una fila a la `UserProfiles` tabla. CreateUserWizard, a continuación, muestra su `CompleteWizardStep`, que muestra un mensaje de confirmación y un botón Continuar. Al hacer clic en el botón Continuar provoca una devolución de datos, pero no se realiza ninguna acción, dejando el usuario atascados en la `EnhancedCreateUserWizard.aspx` página.

Se puede especificar una dirección URL para enviar al usuario cuando se hace clic en el botón Continuar a través del control CreateUserWizard [ `ContinueDestinationPageUrl` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Establecer el `ContinueDestinationPageUrl` propiedad en "~ / Membership/AdditionalUserInfo.aspx". Esto lleva al usuario nuevo a `AdditionalUserInfo.aspx`, donde podrán ver y actualizar su configuración.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personalización de interfaz de CreateUserWizard a preguntar por el nuevo usuario Pueblo de inicio, página principal y la firma

Interfaz predeterminada del control CreateUserWizard es suficiente para escenarios de creación de cuenta simple donde necesita recopilarán solo información de cuenta de usuario básica como nombre de usuario, contraseña y correo electrónico. Pero ¿qué ocurre si deseamos solicitar el visitante que escribir su ciudad natal, la página de inicio y la firma al crear su cuenta de usuario? Es posible personalizar la interfaz del control CreateUserWizard para recopilar información adicional durante el registro, y esta información puede usarse en el `CreatedUser` controlador de eventos para insertar registros adicionales en la base de datos subyacente.

El control CreateUserWizard extiende el control de Asistente de ASP.NET, que es un control que permite a un programador de la página Definir una serie de ordenada `WizardSteps`. El control de asistente representa el paso activo y proporciona una interfaz de navegación que permite el visitante para moverse a través de estos pasos. El control Wizard es ideal para dividir una tarea larga en varios pasos cortos. Para obtener más información sobre el control del asistente, consulte [crear una interfaz de usuario de paso a paso con el Control de Asistente de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Marcado de predeterminado del control CreateUserWizard define dos `WizardSteps`: `CreateUserWizardStep` y `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

La primera `WizardStep`, `CreateUserWizardStep`, representa la interfaz que solicita el nombre de usuario, contraseña, correo electrónico y así sucesivamente. Después de que el visitante proporciona esta información y haga clic en "Create User", se muestra el `CompleteWizardStep`, que muestra el mensaje de confirmación y un botón Continuar.

Para personalizar la interfaz del control CreateUserWizard para incluir campos de formulario adicionales, se puede:

- <strong>Crear uno o varios nuevos</strong><strong>`WizardStep`</strong><strong>s para contener los elementos de la interfaz de usuario adicionales</strong>. Para agregar un nuevo `WizardStep` a CreateUserWizard, haga clic en el "Agregar o quitar `WizardSteps`" vínculo desde su etiqueta inteligente para iniciar la `WizardStep` Editor de la colección. Desde ahí puede agregar, quitar o reordenar los pasos del asistente. Éste es el enfoque que se usará para este tutorial.

- <strong>Convertir el</strong><strong>`CreateUserWizardStep`</strong><strong>en un modo editable</strong><strong>`WizardStep`</strong><strong>.</strong> Esto reemplaza el `CreateUserWizardStep` con un equivalente `WizardStep` cuyo marcado define una interfaz de usuario que coincida con el `CreateUserWizardStep`' s. Convirtiendo el `CreateUserWizardStep` en un `WizardStep` podemos cambiar la posición de los controles o agregar elementos de la interfaz de usuario adicionales a este paso. Para convertir el `CreateUserWizardStep` o `CompleteWizardStep` en un modo editable `WizardStep`, haga clic en el "personalizar crear usuario paso a paso" o "Personalizar paso completo" vincular de etiquetas inteligentes del control.

- **Usar una combinación de las dos opciones anteriores.**

Algo importante a tener en cuenta es que el control CreateUserWizard ejecuta su proceso de creación de la cuenta de usuario cuando se hace clic en el botón "Create User" desde su `CreateUserWizardStep`. No importa si no hay más `WizardStep` s después de la `CreateUserWizardStep` o no.

Al agregar un personalizado `WizardStep` al control CreateUserWizard para recopilar una entrada de usuario adicionales, personalizado `WizardStep` puede colocarse antes o después de la `CreateUserWizardStep`. Si se trata de antes de la `CreateUserWizardStep` , a continuación, se recopilan los datos proporcionados por el usuario adicionales de personalizado `WizardStep` está disponible para el `CreatedUser` controlador de eventos. Sin embargo, si personalizado `WizardStep` viene después de `CreateUserWizardStep` , a continuación, en el momento personalizado `WizardStep` se muestra la nueva cuenta de usuario ya ha creado y el `CreatedUser` ya ha activado el evento.

Figura 21 muestra el flujo de trabajo cuando el agregado `WizardStep` precede a la `CreateUserWizardStep`. Puesto que se han recopilado la información de usuario adicional cuando la `CreatedUser` desencadena el evento, lo único que debemos hacer es actualizar la `CreatedUser` controlador de eventos para recuperar estas entradas y úselos para el `INSERT` valores de parámetro de la instrucción (en lugar de `DBNull.Value`).


[![El flujo de trabajo de CreateUserWizard cuando un WizardStep adicional precede el CreateUserWizardStep](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Figura 21**: The CreateUserWizard flujo de trabajo cuando una adicionales `WizardStep` Precedes el `CreateUserWizardStep` ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image63.png))


Si la opción de instalación `WizardStep` se coloca *después* la `CreateUserWizardStep`, sin embargo, el proceso de cuenta de usuario de creación se produce antes de que el usuario haya tenido una oportunidad para escribir su ciudad natal, la página principal o la firma. En tal caso, esta información adicional debe insertarse en la base de datos una vez creada la cuenta de usuario, como se muestra en la figura 22.


[![El flujo de trabajo de CreateUserWizard cuando llega un WizardStep adicional tras el CreateUserWizardStep](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Figura 22**: The CreateUserWizard flujo de trabajo cuando una adicionales `WizardStep` viene después la `CreateUserWizardStep` ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image66.png))


Espera a que el flujo de trabajo se muestra en la figura 22 para insertar un registro en el `UserProfiles` tabla hasta que finalice el paso 2. Si el visitante cierre su explorador después del paso 1, sin embargo, se habrá alcanzado un estado donde se creó una cuenta de usuario, pero no hay ningún registro se agregó a `UserProfiles`. Una solución es tener un registro con `NULL` o los valores predeterminados de los valores insertados en `UserProfiles` en el `CreatedUser` controlador de eventos (que se desencadena después del paso 1) y actualice este registrar después de que se complete el paso 2. Esto garantiza que un `UserProfiles` se agregará el registro para la cuenta de usuario incluso si el usuario sale de la mitad del proceso de registro a través de.

Para este tutorial vamos a crear un nuevo `WizardStep` que tiene lugar tras el `CreateUserWizardStep` pero antes del `CompleteWizardStep`. Vamos a obtenga primero la WizardStep en colocar y configurado y, a continuación, examinará el código.

En la etiqueta inteligente del control CreateUserWizard, seleccione el "Agregar o quitar `WizardStep` s", que abrirá el `WizardStep` cuadro de diálogo Editor de la colección. Agregue un nuevo `WizardStep`, y establece su `ID` a `UserSettings`, sus `Title` a "La configuración" y su `StepType` a `Step`. Colóquelo para que va después de la `CreateUserWizardStep` ("suscribirse para obtener una cuenta nueva") y antes de la `CompleteWizardStep` ("completar"), tal como se muestra en la figura 23.


[![Agregar un nuevo WizardStep al Control CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Figura 23**: agregar un nuevo `WizardStep` al CreateUserWizard Control ([haga clic aquí para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image69.png))


Haga clic en Aceptar para cerrar el `WizardStep` cuadro de diálogo Editor de la colección. El nuevo `WizardStep` se pone de manifiesto marcado declarativo del control CreateUserWizard actualizado:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Tenga en cuenta las nuevas `<asp:WizardStep>` elemento. Tenemos que agregar la interfaz de usuario para recopilar ciudad natal del nuevo usuario, página de inicio y firma aquí. Puede escribir este contenido en la sintaxis declarativa o a través del diseñador. Para utilizar el diseñador, seleccione el paso de "Configuración de la" en la lista desplegable de la etiqueta inteligente para ver el paso en el diseñador.

> [!NOTE]
> Al seleccionar un paso a través de la lista de desplegable de la etiqueta inteligente actualiza el control CreateUserWizard [ `ActiveStepIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), que especifica el índice del paso inicial. Por lo tanto, si usa esta lista desplegable para editar el paso de "Configuración de la" en el diseñador, asegúrese de volver a darle "Inicio de sesión a una nueva cuenta" para que este paso se muestra cuando los usuarios visiten primero la `EnhancedCreateUserWizard.aspx` página.


Crear una interfaz de usuario en el paso de "Configuración de la" que contiene tres controles de cuadro de texto denominados `HomeTown`, `HomepageUrl`, y `Signature`. Después de crear esta interfaz, el marcado declarativo del control CreateUserWizard debe ser similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Continúe y visite esta página a través de un explorador y crear una nueva cuenta de usuario, especificar valores para la ciudad natal, la página principal y la firma. Después de completar la `CreateUserWizardStep` se crea la cuenta de usuario en el marco de trabajo de pertenencia y el `CreatedUser` ejecuciones de controlador de eventos, que agrega una nueva fila a `UserProfiles`, pero con una base de datos `NULL` valor `HomeTown`, `HomepageUrl`, y `Signature`. Nunca se usan los valores especificados para la ciudad natal, la página principal y la firma. El resultado neto es una nueva cuenta de usuario con un `UserProfiles` grabar cuya `HomeTown`, `HomepageUrl`, y `Signature` campos todavía tienen que especificarse.

Es necesario ejecutar el código después del paso de "Configuración de la" que toma los valores de ciudad, honepage y firma principales especificados por el usuario y actualiza la correspondiente `UserProfiles` registro. Cada vez que el usuario se mueve entre los pasos de un asistente de control, el asistente [ `ActiveStepChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) se activa. Podemos crear un controlador de eventos para este evento y la actualización del `UserProfiles` tabla cuando se haya completado el paso de "Configuración de la".

Agregar un controlador de eventos para el CreateUserWizard `ActiveStepChanged` eventos y agregue el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

El código anterior se inicia mediante la determinación de si hemos llegado el paso "Completar". Puesto que el paso "Completar" se produce inmediatamente después del paso de "Configuración de la", a continuación, cuando se alcanza el visitante "Completa" paso a paso que quiere decir que acaba de finalizar el paso de "Configuración de la".

En tal caso, es necesario hacer referencia mediante programación a los controles de cuadro de texto dentro de la `UserSettings WizardStep`. Esto se logra utilizando primero la `FindControl` método mediante programación que hacen referencia a la `UserSettings WizardStep`y, a continuación, vuelva a hacer referencia a los cuadros de texto desde el `WizardStep`. Una vez que se ha hecho referencia a los cuadros de texto, estamos listos para ejecutar el `UPDATE` instrucción. El `UPDATE` instrucción tiene el mismo número de parámetros como el `INSERT` instrucción en el `CreatedUser` controlador de eventos, pero aquí usamos los valores de ciudad, la página principal y la firma particulares suministrados por el usuario.

Con este controlador de eventos en su lugar, visite la `EnhancedCreateUserWizard.aspx` página a través de un explorador y crear una nueva cuenta de usuario especificar valores para la ciudad natal, la página principal y la firma. Después de crear la nueva cuenta debe redirigir a la `AdditionalUserInfo.aspx` página, donde introducidas doméstica ciudad, la firma y la página principal se muestra la información.

> [!NOTE]
> Nuestro sitio Web actualmente tiene dos páginas desde el que un visitante puede crear una nueva cuenta: `CreatingUserAccounts.aspx` y `EnhancedCreateUserWizard.aspx`. Mapa del sitio del sitio Web y la página de inicio de sesión apuntan a la `CreatingUserAccounts.aspx` página, pero la `CreatingUserAccounts.aspx` página no solicita al usuario su información de ciudad, la página principal y la firma particular y no se agregará una fila correspondiente a `UserProfiles`. Por lo tanto, actualizar la `CreatingUserAccounts.aspx` página por lo que ofrece esta funcionalidad o actualizar la página del mapa del sitio y de inicio de sesión para hacer referencia a `EnhancedCreateUserWizard.aspx` en lugar de `CreatingUserAccounts.aspx`. Si elige esta última opción, asegúrese de actualizar la `Membership` la carpeta `Web.config` archivo con el fin de permitir el acceso a los usuarios anónimos el `EnhancedCreateUserWizard.aspx` página.


## <a name="summary"></a>Resumen

En este tutorial, observamos técnicas para el modelado de datos que está relacionada con las cuentas de usuario en el marco de pertenencia. En concreto, analizamos modelado entidades que compartan una relación uno a varios con cuentas de usuario, así como datos que comparte una relación uno a uno. Además, hemos visto cómo se relacionan información podría mostrar, insertar y actualizada, con algunos ejemplos de uso del control SqlDataSource y otros con el código de ADO.NET.

Este tutorial completa nuestro aspecto en cuentas de usuario. Empezando por el siguiente tutorial se mostrará en color nuestra atención a los roles. En los próximos varios tutoriales, veremos el marco de trabajo de Roles, vea cómo crear nuevos roles, cómo asignar roles a los usuarios, para determinar qué roles de un usuario pertenece a y cómo debe aplicar la autorización basada en roles.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Obtener acceso y actualizar datos en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Control de asistente 2.0 de ASP.NET](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Crear una interfaz de usuario de paso a paso con el Control de asistente 2.0 de ASP.NET](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Crear parámetros de Control de origen de datos personalizado](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personalizar el Control CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Tutoriales del Control DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Mostrar datos con el Control ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Si examinamos los controles de validación en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Edición de inserción y eliminación de datos](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Validación del formulario en ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Recopilar información de registro de usuario personalizada](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Perfiles de ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [El Control de asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Inicio rápido de perfiles de usuario](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales se revisó por varios revisores útiles. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](user-based-authorization-cs.md)
> [Siguiente](creating-the-membership-schema-in-sql-server-vb.md)
