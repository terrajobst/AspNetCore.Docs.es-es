---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Descripción de la autenticación de ASP.NET AJAX y los servicios de aplicación de perfiles | Microsoft Docs
author: scottcate
description: El servicio de autenticación permite a los usuarios proporcionar las credenciales para recibir una cookie de autenticación y es el servicio de puerta de enlace para permitir que el usuario personalizada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 323fec56f18281b5b5a3d312a2e4c4c7133e3f03
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393066"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Descripción de la autenticación de ASP.NET AJAX y los servicios de aplicación de perfiles
====================
por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> El servicio de autenticación permite a los usuarios proporcionar las credenciales para recibir una cookie de autenticación, y es el servicio de puerta de enlace para permitir que los perfiles de usuario personalizado proporcionado por ASP.NET. Uso del servicio de autenticación de AJAX de ASP.NET es compatible con la autenticación de formularios de ASP.NET estándar, por lo que las aplicaciones que utilicen la autenticación de formularios (por ejemplo con el inicio de sesión de control) no se separarán mediante la actualización al servicio de autenticación de AJAX.


## <a name="introduction"></a>Introducción

Como parte de .NET Framework 3.5, Microsoft ofrece una mejora considerable del entorno; no solo está disponible un nuevo entorno de desarrollo, pero las nuevas características de Language-Integrated Query (LINQ) y otras mejoras del lenguaje estarán disponibles próximamente. Además, algunas características conocidas de otros conjuntos de herramientas, en particular las extensiones de AJAX de ASP.NET, que se incluyen como miembros selectos de la biblioteca de clases Base de .NET Framework. Estas extensiones permiten muchas nuevas características de cliente enriquecidas, como la representación parcial de páginas sin necesidad de una actualización de página completa, la capacidad de acceder a servicios Web a través de script de cliente (incluida la API de generación de perfiles de ASP.NET) y una amplia API del lado cliente diseñado para crear el reflejo de muchos de los esquemas de control que se muestra en el conjunto de controles de servidor ASP.NET.

Este artículo examina la implementación y el uso de la generación de perfiles de ASP.NET y servicios de autenticación de formularios tal como se exponen mediante las extensiones de AJAX de Microsoft ASP.NET AJAX ExtensionsThe facilitan la autenticación de formularios increíblemente admitir, tal y como (así como el servicio de generación de perfiles) se expone a través de un script de proxy de servicio Web. Las extensiones de AJAX también admite la autenticación personalizada a través de la clase AuthenticationServiceManager.

Estas notas del producto se basan en la versión Beta 2 de Visual Studio 2008 y .NET Framework 3.5. Estas notas del producto también se da por supuesto que trabajará con Visual Studio 2008 Beta 2, no Visual Web Developer Express y proporcionará tutoriales de acuerdo con la interfaz de usuario de Visual Studio. Algunos ejemplos de código pueden utilizar las plantillas de proyecto no está disponibles en Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Autenticación y perfiles*

Los perfiles de ASP.NET de Microsoft y servicios de autenticación proporcionados por el sistema de autenticación de formularios de ASP.NET y son los componentes estándar de ASP.NET. Las extensiones de AJAX de ASP.NET proporcionan acceso de script a estos servicios a través de servidores proxy de script a través de un modelo en el espacio de nombres Sys.Services de la biblioteca de cliente AJAX bastante sencillo.

El servicio de autenticación permite a los usuarios proporcionar las credenciales para recibir una cookie de autenticación, y es el servicio de puerta de enlace para permitir que los perfiles de usuario personalizado proporcionado por ASP.NET. Uso del servicio de autenticación de AJAX de ASP.NET es compatible con la autenticación de formularios de ASP.NET estándar, por lo que las aplicaciones que utilicen la autenticación de formularios (por ejemplo con el inicio de sesión de control) no se separarán mediante la actualización al servicio de autenticación de AJAX.

El servicio de perfiles permite la integración automática y el almacenamiento de datos de usuario según la pertenencia a proporcionados por el servicio de autenticación. Los datos almacenados se especifican mediante el archivo web.config, y los diversos proveedores de servicios de generación de perfiles controlan la administración de datos. Al igual que con el servicio de autenticación, el servicio de perfil de AJAX es compatible con el servicio de perfil ASP.NET estándar, por lo que las páginas que actualmente incorpora características del servicio de perfil de ASP.NET no deberían verse interrumpidas por incluida la compatibilidad con AJAX.

Incorporar la autenticación de ASP.NET y los propios servicios de generación de perfiles a una aplicación está fuera del ámbito de este artículo. Para obtener más información sobre el tema, vea MSDN Library hacen referencia a artículo administrar usuarios mediante pertenencia en [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET también incluye una utilidad para configurar automáticamente la pertenencia a un servidor SQL, que es el proveedor de servicios de autenticación predeterminado para la pertenencia de ASP.NET. Para obtener más información, consulte el artículo de la herramienta de registro de SQL Server de ASP.NET (Aspnet\_regsql.exe) en [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Mediante el servicio de autenticación de ASP.NET AJAX*

El servicio de autenticación de AJAX de ASP.NET debe estar habilitado en el archivo web.config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

El servicio de autenticación requiere habilitar la autenticación de formularios de ASP.NET y las cookies estén habilitadas en el explorador del cliente (una secuencia de comandos no puede habilitar una sesión sin cookies ya que las sesiones sin cookies requieren parámetros de URL).

Una vez que el servicio de autenticación de AJAX está habilitado y configurado, el script de cliente puede aprovechar inmediatamente el objeto Sys.Services.AuthenticationService. Principalmente, el script de cliente deseará aprovechar las ventajas de la `login` método y `isLoggedIn` propiedad. Varias propiedades existen para proporcionar valores predeterminados para el método de inicio de sesión, que puede aceptar un gran número de parámetros.

*Miembros de Sys.Services.AuthenticationService*

*método de inicio de sesión:*

El método login() inicia una solicitud para autenticar las credenciales del usuario. Este método es asincrónico y no bloquea la ejecución.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| userName | Requerido. El nombre de usuario para autenticar. |
| contraseña | Opcional (valor predeterminado es null). Contraseña del usuario. |
| isPersistent | Opcional (valor predeterminado es false). Si la cookie de autenticación del usuario debe conservar entre sesiones. Si es false, el usuario se cerrará la sesión cuando se cierra el explorador o la sesión expira. |
| URL de redireccionamiento | Opcional (valor predeterminado es null). La dirección URL para redirigir el explorador tras una autenticación correcta. Si este parámetro es null o una cadena vacía, se produce ninguna redirección. |
| customInfo | Opcional (valor predeterminado es null). Este parámetro se utiliza actualmente y está reservado para uso futuro. |
| loginCompletedCallback | Opcional (valor predeterminado es null). La función que se va a llamar cuando se haya completado correctamente el inicio de sesión. Si se especifica, este parámetro reemplaza la propiedad defaultLoginCompleted. |
| failedCallback | Opcional (valor predeterminado es null). La función que se va a llamar cuando el inicio de sesión ha fallado. Si se especifica, este parámetro reemplaza la propiedad defaultFailedCallback. |
| userContext | Opcional (valor predeterminado es null). Datos de contexto de usuario personalizado que se deben pasar a las funciones de devolución de llamada. |

*Valor devuelto:*

Esta función no incluye un valor devuelto. Sin embargo, un número de comportamientos se incluyen la finalización de una llamada a esta función:

- La página actual, o bien se actualizará o se puede cambiar si la `redirectUrl` parámetro era null ni una cadena vacía.
- Sin embargo, si el parámetro era null o una cadena vacía, el `loginCompletedCallback` parámetro, o `defaultLoginCompletedCallback` se llama a la propiedad.
- Si se produce un error en la llamada al servicio web, el `failedCallback` parámetro de `defaultFailedCallback` se llama a la propiedad.

*método de cierre de sesión:*

El método logout() quita la cookie de credenciales y se cierra la sesión del usuario actual de la aplicación web.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| URL de redireccionamiento | Opcional (valor predeterminado es null). La dirección URL para redirigir el explorador tras una autenticación correcta. Si este parámetro es null o una cadena vacía, se produce ninguna redirección. |
| logoutCompletedCallback | Opcional (valor predeterminado es null). La función que se va a llamar cuando se haya completado correctamente el cierre de sesión. Si se especifica, este parámetro reemplaza la propiedad defaultLogoutCompleted. |
| failedCallback | Opcional (valor predeterminado es null). La función que se va a llamar cuando el inicio de sesión ha fallado. Si se especifica, este parámetro reemplaza la propiedad defaultFailedCallback. |
| userContext | Opcional (valor predeterminado es null). Datos de contexto de usuario personalizado que se deben pasar a las funciones de devolución de llamada. |

*Valor devuelto:*

Esta función no incluye un valor devuelto. Sin embargo, un número de comportamientos se incluyen la finalización de una llamada a esta función:

- La página actual, o bien se actualizará o se puede cambiar si la `redirectUrl` parámetro era null ni una cadena vacía.
- Sin embargo, si el parámetro era null o una cadena vacía, el `logoutCompletedCallback` parámetro, o `defaultLogoutCompletedCallback` se llama a la propiedad.
- Si se produce un error en la llamada al servicio web, el `failedCallback` parámetro de `defaultFailedCallback` se llama a la propiedad.

*Propiedad defaultFailedCallback (get, set):*

Esta propiedad especifica una función que se debe llamar si se produce un error al comunicarse con el servicio web. Recibirá un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| error | Especifica la información de error. |
| userContext | Especifica la información de contexto de usuario cuando se llamó la función de inicio de sesión o cierre de sesión. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad defaultLoginCompletedCallback (get, set):*

Esta propiedad especifica una función que se debe llamar cuando se ha completado la llamada al servicio de inicio de sesión web. Recibirá un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| validCredentials | Especifica si el usuario proporciona credenciales válidas. `true` Si el usuario ha iniciado correctamente en caso contrario `false`. |
| userContext | Especifica la información de contexto de usuario cuando se llamó la función de inicio de sesión. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad defaultLogoutCompletedCallback () (get, set):*

Esta propiedad especifica una función que se debe llamar cuando se ha completado la llamada de servicio web de cierre de sesión. Recibirá un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| resultado | Este parámetro siempre será `null`; está reservado para uso futuro. |
| userContext | Especifica la información de contexto de usuario cuando se llamó la función de inicio de sesión. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad isLoggedIn (get):*

Esta propiedad obtiene el estado actual de la autenticación del usuario; se establece el objeto ScriptManager durante una solicitud de página.

Esta propiedad devuelve `true` si el usuario ha iniciado; de lo contrario, devuelve `false`.

*propiedad ruta de acceso (get, set):*

Esta propiedad mediante programación determina la ubicación del servicio web de autenticación. Se puede usar para reemplazar el proveedor de autenticación predeterminado, así como una establecer mediante declaración en la propiedad de ruta de acceso del nodo secundario del control ScriptManager AuthenticationService (para obtener más información, consulte el usar un proveedor de servicios de autenticación personalizada temas siguientes).

Tenga en cuenta que no cambia la ubicación del servicio de autenticación predeterminado. Sin embargo, ASP.NET AJAX permite especificar la ubicación de un servicio web que proporciona la misma interfaz de clase que el proxy de servicio de autenticación de ASP.NET AJAX.

Tenga en cuenta también que esta propiedad no debe establecerse en un valor que se dirige la solicitud de script fuera del sitio actual. Dado que la aplicación actual no recibirá las credenciales de autenticación, sería inútil; Además, el AJAX tecnología subyacente no debe registrar las solicitudes entre sitios y puede generar una excepción de seguridad en el explorador del cliente.

Esta propiedad es un `String` objeto que representa la ruta de acceso al servicio web de autenticación.

*propiedad de tiempo de espera (get, set):*

Esta propiedad determina la longitud de tiempo de espera para el servicio de autenticación antes de asumir que la solicitud de inicio de sesión ha producido un error. Si se agota el tiempo de espera mientras se espera que se complete una llamada, se llamará a la devolución de llamada de error de solicitud y no se completará la llamada.

Esta propiedad es un `Number` objeto que representa el número de milisegundos de espera para obtener los resultados del servicio de autenticación.

*Ejemplo de código: Iniciar sesión en el servicio de autenticación*

El marcado siguiente es un ejemplo de página ASP.NET con una llamada sencilla secuencia de comandos a los métodos de inicio de sesión y cierre de sesión de la clase AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Acceso a la generación de perfiles de datos a través de AJAX de ASP.NET

El servicio de generación de perfiles de ASP.NET también se expone a través de las extensiones de AJAX de ASP.NET. Puesto que el servicio de generación de perfiles de ASP.NET proporciona una API enriquecida y detallada por el que se va a almacenar y recuperar datos de usuario, esto puede ser una herramienta de productividad excelente.

El servicio de perfil debe estar habilitado en el archivo web.config; no es de forma predeterminada. Para ello, asegúrese de que el `profileService` ha habilitado el elemento secundario = web.config especificado en true y que haya especificado las propiedades que se pueden leer o escritas del siguiente modo:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

También se debe configurar el servicio de perfil. Aunque la configuración del servicio de generación de perfiles está fuera del ámbito de este artículo, merece la pena tener en cuenta que los grupos tal como se define en la configuración de perfil será accesibles como subpropiedades del nombre del grupo. Por ejemplo, con la siguiente sección de perfil especificada:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Script de cliente debería poder tener acceso a nombre, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip y BackgroundColor como propiedades del campo de propiedades de la clase ProfileService.

Una vez configurado el servicio de generación de perfiles de AJAX, estará inmediatamente disponible en páginas. Sin embargo, tendrá que cargar una vez antes de su uso.

*Miembros de Sys.Services.ProfileService*

*campo de propiedades:*

El campo de propiedades expone todos los datos de perfil configurado como propiedades secundarias que pueden hacer referencia a la convención de nombre de operador de punto. Las propiedades que son elementos secundarios de los grupos de propiedades se denominan GroupName.PropertyName. En la configuración de perfil de ejemplo presentada anteriormente, para obtener el estado del usuario, podría utilizar el siguiente identificador:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*método de carga:*

Carga una lista seleccionada o todas las propiedades del servidor.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (valor predeterminado es null). Las propiedades que se va a cargar desde el servidor. |
| loadCompletedCallback | Opcional (valor predeterminado es null). La función que se va a llamar cuando haya finalizado la carga. |
| failedCallback | Opcional (valor predeterminado es null). La función que se invoca si se produce un error. |
| userContext | Opcional (valor predeterminado es null). Información de contexto que se pasará a la función de devolución de llamada. |

La función de carga no tiene un valor devuelto. Si la llamada se ha realizado correctamente, llamará a cualquiera la `loadCompletedCallback` parámetro o `defaultLoadCompletedCallback` propiedad. Si el error en la llamada o el tiempo de espera agotado, ya sea el `failedCallback` parámetro o `defaultFailedCallback` se llamará la propiedad.

Si el `propertyNames` parámetro no se proporciona, se recuperan todas las propiedades de lectura configurada desde el servidor.

*el método Save:*

El método save() guarda la lista de propiedades especificado (o todas las propiedades) en el perfil de usuario ASP.NET.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (valor predeterminado es null). Las propiedades se guarden en el servidor. |
| saveCompletedCallback | Opcional (valor predeterminado es null). Se ha completado la función que se llama cuando se guarda. |
| failedCallback | Opcional (valor predeterminado es null). La función que se invoca si se produce un error. |
| userContext | Opcional (valor predeterminado es null). Información de contexto que se pasará a la función de devolución de llamada. |

Guardar función no tiene un valor devuelto. Si la llamada se completa correctamente, llamará a cualquiera la `saveCompletedCallback` parámetro o `defaultSaveCompletedCallback` propiedad. Si el error en la llamada o el tiempo de espera agotado, ya sea el `failedCallback` o `defaultFailedCallback` se llamará la propiedad.

Si el `propertyNames` parámetro es null, todas las propiedades de perfil se enviarán al servidor y el servidor decidirá qué propiedades se pueden guardar y cuáles no.

*Propiedad defaultFailedCallback (get, set):*

Esta propiedad especifica una función que se debe llamar si se produce un error al comunicarse con el servicio web. Recibirá un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| Error | Especifica la información de error. |
| userContext | Especifica la información de contexto de usuario proporcionada cuando la carga o se llamó a guardar la función. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad defaultSaveCompleted (get, set):*

Esta propiedad especifica una función que se debe llamar a notificar la finalización del proceso de guardar los datos de perfil del usuario. Recibirá un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| numPropsSaved | Especifica el número de propiedades que se guardaron. |
| userContext | Especifica la información de contexto de usuario proporcionada cuando la carga o se llamó a guardar la función. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad defaultLoadCompleted (get, set):*

Esta propiedad especifica una función que se debe llamar cuando se complete la carga de datos de perfil del usuario. Recibirá un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| numPropsLoaded | Especifica el número de propiedades cargadas. |
| userContext | Especifica la información de contexto de usuario proporcionada cuando la carga o se llamó a guardar la función. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad ruta de acceso (get, set):*

Esta propiedad mediante programación determina la ubicación del servicio web de perfil. Se puede usar para reemplazar el proveedor de servicios de perfil predeterminado, así como uno establecer mediante declaración en la propiedad de ruta de acceso del nodo secundario del control ScriptManager ProfileService.

Tenga en cuenta que no cambia la ubicación del servicio de perfil predeterminado. Sin embargo, ASP.NET AJAX permite especificar la ubicación de un servicio web que proporciona la misma interfaz de clase que el proxy de servicio de autenticación de ASP.NET AJAX.

Tenga en cuenta también que esta propiedad no debe establecerse en un valor que se dirige la solicitud de script fuera del sitio actual. El AJAX tecnología subyacente no debe registrar las solicitudes entre sitios y puede generar una excepción de seguridad en el explorador del cliente.

Esta propiedad es un `String` objeto que representa la ruta de acceso al servicio web de perfil.

*propiedad de tiempo de espera (get, set):*

Esta propiedad determina el período de tiempo de espera para el servicio de perfil antes de asumir que la carga o Guardar solicitud ha producido un error. Si se agota el tiempo de espera mientras se espera que se complete una llamada, se llamará a la devolución de llamada de error de solicitud y no se completará la llamada.

Esta propiedad es un `Number` objeto que representa el número de milisegundos de espera para obtener los resultados desde el servicio de perfil.

*Ejemplo de código: cargar datos de perfil de carga de página*

El código siguiente comprueba si un usuario está autenticado y si es así, cargará color de fondo preferido del usuario como la página.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Uso de un proveedor de servicios de autenticación personalizada*

Las extensiones de AJAX de ASP.NET permiten crear un proveedor de servicios de autenticación de script personalizado mediante la exposición de su funcionalidad a través de un servicio web personalizado. Para poder usarlo, el servicio web debe exponer dos métodos, `Login` y `Logout`; y estos métodos deben especificarse con las mismas firmas de método que el servicio web de autenticación de ASP.NET AJAX de forma predeterminada.

Una vez haya creado el servicio web personalizado, deberá especificar la ruta de acceso a ella, ya sea mediante declaración en la página, mediante programación en código o mediante el script de cliente.

*Para establecer la ruta de acceso de forma declarativa:*

Para establecer la ruta de acceso de forma declarativa, incluya al elemento secundario AuthenticationService del objeto de ScriptManager en la página ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Para establecer la ruta de acceso en el código:*

Para establecer la ruta de acceso mediante programación, especifique la ruta de acceso a través de la instancia de su administrador de scripts:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Para establecer la ruta de acceso en la secuencia de comandos:*

Para establecer la ruta de acceso mediante programación en la secuencia de comandos, use el `path` propiedad de la clase AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Servicio Web de ejemplo para una autenticación personalizada*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Resumen

Servicios ASP.NET - específicamente los servicios de generación de perfiles, la pertenencia y autenticación - fácilmente se exponen a JavaScript en el explorador del cliente. Esto permite a los desarrolladores integrar su código del lado cliente con el mecanismo de autenticación sin problemas, sin tener que depender de los controles como UpdatePanel para realizar el trabajo pesado. Se pueden proteger los datos de perfil del cliente, mediante el uso de valores de configuración web; No hay datos disponibles de forma predeterminada, y los desarrolladores deben participar en las propiedades de perfil.

Además, mediante la creación de implementaciones de servicios web simplificada con las firmas de método equivalente, los desarrolladores pueden crear proveedores de script personalizado para estos servicios intrínsecos de ASP.NET. Compatibilidad con estas técnicas simplifica el desarrollo de aplicaciones cliente enriquecidas, al tiempo que proporciona a los desarrolladores con una amplia gama de flexibilidad para satisfacer necesidades específicas.

## <a name="bio"></a>*Biografía*

Scott Cate ha estado trabajando con tecnologías Web de Microsoft desde 1997 y es el presidente de myKB.com ([www.myKB.com](http://www.myKB.com)) donde se especializa en la escritura de ASP.NET en función de las aplicaciones que se centra en soluciones de Software de Base de conocimiento. Se puede establecer contacto con Scott por correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Siguiente](understanding-asp-net-ajax-localization.md)
