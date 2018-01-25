---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: "Descripción de la autenticación de AJAX de ASP.NET y los servicios de aplicación de perfiles | Documentos de Microsoft"
author: scottcate
description: "El servicio de autenticación permite a los usuarios que proporcionen credenciales para recibir una cookie de autenticación y es el servicio de puerta de enlace para permitir que a un usuario personalizado..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 182276f9f91b99beb1ce0fc40dcda1f19376669a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Descripción de la autenticación de AJAX de ASP.NET y los servicios de aplicación de perfiles
====================
por [Scott ndicar](https://github.com/scottcate)

[Descarga de PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> El servicio de autenticación permite a los usuarios que proporcionen credenciales para recibir una cookie de autenticación, y es el servicio de puerta de enlace para permitir que los perfiles de usuario personalizado proporcionado por ASP.NET. Uso del servicio de autenticación de AJAX de ASP.NET es compatible con la autenticación de formularios de ASP.NET estándar, por lo que las aplicaciones que utilicen la autenticación de formularios (por ejemplo, el control es el inicio de sesión) no se anularán mediante la actualización para el servicio de autenticación de AJAX.


## <a name="introduction"></a>Introducción

Como parte de .NET Framework 3.5, Microsoft ofrece una mejora considerable entorno; no solo está disponible un nuevo entorno de desarrollo, pero las nuevas características de Language-Integrated Query (LINQ) y otras mejoras de lenguaje son disponible próximamente. Además, algunas características conocidas de otros conjuntos de herramientas, en concreto las extensiones de AJAX de ASP.NET, que se va a se incluyen como miembros de primera clase de la biblioteca de clases Base de .NET Framework. Estas extensiones permiten muchas características nuevas de cliente completas, incluida la representación parcial de páginas sin necesidad de una actualización de página completa, la capacidad para tener acceso a servicios Web a través de script de cliente (incluida la API de generación de perfiles de ASP.NET) y una amplia API de cliente diseñada para reflejar muchos de los esquemas de control que se muestra en el conjunto de control de servidor ASP.NET.

Estas notas del producto se examinan la implementación y uso de la generación de perfiles de ASP.NET y servicios de autenticación de formularios tal y como se exponen mediante las extensiones de AJAX de Microsoft ASP.NET AJAX ExtensionsThe facilitan la autenticación de formularios increíblemente admitir, tal y como (así como el servicio de generación de perfiles) se expone a través de una secuencia de comandos de proxy de servicio Web. Las extensiones de AJAX también admite la autenticación personalizada a través de la clase AuthenticationServiceManager.

Estas notas del producto se basan en la versión Beta 2 de Visual Studio 2008 y .NET Framework 3.5. Estas notas del producto también se supone que va a trabajar con Visual Studio 2008 Beta 2, no Visual Web Developer Express y proporcionará tutoriales de acuerdo con la interfaz de usuario de Visual Studio. Algunos ejemplos de código pueden utilizar plantillas de proyecto no está disponibles en Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Perfiles y autenticación*

Los perfiles de ASP.NET de Microsoft y los servicios de autenticación proporcionados por el sistema de autenticación de formularios de ASP.NET y son los componentes estándar de ASP.NET. Las extensiones de AJAX de ASP.NET proporcionan acceso de script a estos servicios a través de servidores proxy de script a través de un modelo bastante sencillo en el espacio de nombres de Sys.Services de la biblioteca de cliente de AJAX.

El servicio de autenticación permite a los usuarios que proporcionen credenciales para recibir una cookie de autenticación, y es el servicio de puerta de enlace para permitir que los perfiles de usuario personalizado proporcionado por ASP.NET. Uso del servicio de autenticación de AJAX de ASP.NET es compatible con la autenticación de formularios de ASP.NET estándar, por lo que las aplicaciones que utilicen la autenticación de formularios (por ejemplo, el control es el inicio de sesión) no se anularán mediante la actualización para el servicio de autenticación de AJAX.

El servicio de perfiles permite la integración automática y el almacenamiento de datos de usuario según la pertenencia a proporcionados por el servicio de autenticación. Los datos almacenados se especifican mediante el archivo web.config y los proveedores de servicios de generación de perfiles distintos controlan la administración de datos. Al igual que con el servicio de autenticación, el servicio de perfil de AJAX es compatible con el servicio de perfiles ASP.NET estándar, por lo que no se deben dividir páginas actualmente incorpora características del servicio de perfil de ASP.NET mediante la inclusión de compatibilidad con AJAX.

Incorporar una aplicación en los servicios de generación de perfiles propios y la autenticación de ASP.NET está fuera del ámbito de este artículo. Para obtener más información sobre el tema, vea MSDN Library hacen referencia a artículo administrar usuarios mediante pertenencia en [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET también incluye una utilidad para configurar automáticamente la pertenencia a un SQL Server, que es el proveedor de servicio de autenticación predeterminado para la pertenencia a ASP.NET. Para obtener más información, vea el artículo de la herramienta de registro de servidor de SQL de ASP.NET (Aspnet\_regsql.exe) en [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Mediante el servicio de autenticación de AJAX de ASP.NET*

El servicio de autenticación de AJAX de ASP.NET debe estar habilitado en el archivo web.config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

El servicio de autenticación requiere habilitar la autenticación de formularios de ASP.NET y las cookies estén habilitadas en el explorador del cliente (una secuencia de comandos no puede habilitar una sesión sin cookies ya que las sesiones sin cookies requieren parámetros de dirección URL).

Una vez que el servicio de autenticación de AJAX está habilitado y configurado, el script de cliente puede aprovechar inmediatamente el objeto Sys.Services.AuthenticationService. Principalmente, el script de cliente desee aprovechar las ventajas de la `login` método y `isLoggedIn` propiedad. Varias propiedades existen para proporcionar los valores predeterminados para el método de inicio de sesión, que puede aceptar un gran número de parámetros.

*Miembros de Sys.Services.AuthenticationService*

*método de inicio de sesión:*

El método login() inicia una solicitud para autenticar las credenciales del usuario. Este método es asincrónico y no bloquea la ejecución.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| userName | Requerido. El nombre de usuario para autenticar. |
| contraseña | Opcional (valor predeterminado es null). Contraseña del usuario. |
| isPersistent | Opcional (valor predeterminado es false). Si la cookie de autenticación del usuario debe conservarse en las sesiones. Si es false, el usuario se cerrará la sesión cuando se cierra el explorador o la sesión expira. |
| URL de redireccionamiento | Opcional (valor predeterminado es null). La dirección URL para redirigir el explorador tras una autenticación correcta. Si este parámetro es null o una cadena vacía, se produce ninguna redirección. |
| customInfo | Opcional (valor predeterminado es null). Este parámetro no se utiliza actualmente y está reservado para un uso futuro. |
| loginCompletedCallback | Opcional (valor predeterminado es null). La función que se llamará cuando se haya completado correctamente el inicio de sesión. Si se especifica, este parámetro invalida la propiedad defaultLoginCompleted. |
| failedCallback | Opcional (valor predeterminado es null). Función que se va a llamar cuando se produce un error en el inicio de sesión. Si se especifica, este parámetro invalida la propiedad defaultFailedCallback. |
| userContext | Opcional (valor predeterminado es null). Datos de contexto de usuario personalizada que se deben pasar a las funciones de devolución de llamada. |

*Valor devuelto:*

Esta función no incluye un valor devuelto. Sin embargo, un número de comportamientos se incluyen en la realización de una llamada a esta función:

- La página actual o se actualizarán o cambiarse si el `redirectUrl` parámetro que no es ni null ni una cadena vacía.
- Sin embargo, si el parámetro es null o una cadena vacía, el `loginCompletedCallback` parámetro, o `defaultLoginCompletedCallback` se denomina propiedad.
- Si se produce un error en la llamada al servicio web, el `failedCallback` parámetro de `defaultFailedCallback` se denomina propiedad.

*método de cierre de sesión:*

El método logout() quita la cookie de credenciales y se registra para el usuario actual de la aplicación web.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| URL de redireccionamiento | Opcional (valor predeterminado es null). La dirección URL para redirigir el explorador tras una autenticación correcta. Si este parámetro es null o una cadena vacía, se produce ninguna redirección. |
| logoutCompletedCallback | Opcional (valor predeterminado es null). La función que se llamará cuando se haya completado correctamente el cierre de sesión. Si se especifica, este parámetro invalida la propiedad defaultLogoutCompleted. |
| failedCallback | Opcional (valor predeterminado es null). Función que se va a llamar cuando se produce un error en el inicio de sesión. Si se especifica, este parámetro invalida la propiedad defaultFailedCallback. |
| userContext | Opcional (valor predeterminado es null). Datos de contexto de usuario personalizada que se deben pasar a las funciones de devolución de llamada. |

*Valor devuelto:*

Esta función no incluye un valor devuelto. Sin embargo, un número de comportamientos se incluyen en la realización de una llamada a esta función:

- La página actual o se actualizarán o cambiarse si el `redirectUrl` parámetro que no es ni null ni una cadena vacía.
- Sin embargo, si el parámetro es null o una cadena vacía, el `logoutCompletedCallback` parámetro, o `defaultLogoutCompletedCallback` se denomina propiedad.
- Si se produce un error en la llamada al servicio web, el `failedCallback` parámetro de `defaultFailedCallback` se denomina propiedad.

*Propiedad defaultFailedCallback (get, set):*

Esta propiedad especifica una función que se debe llamar si se produce un error al comunicarse con el servicio web. Debería recibir un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| error | Especifica la información de error. |
| userContext | Especifica la información de contexto de usuario cuando se llamó la función de inicio de sesión o cierre de sesión. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad defaultLoginCompletedCallback (get, set):*

Esta propiedad especifica una función que se debe llamar cuando se completa la llamada de servicio web de inicio de sesión. Debería recibir un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| validCredentials | Especifica si el usuario proporciona credenciales válidas. `true`Si el usuario ha iniciado sesión correctamente en; en caso contrario, `false`. |
| userContext | Especifica la información de contexto de usuario cuando se llamó la función de inicio de sesión. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad defaultLogoutCompletedCallback () (get, set):*

Esta propiedad especifica una función que se debe llamar cuando se completa la llamada de servicio web de cierre de sesión. Debería recibir un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| resultado | Este parámetro siempre será `null`; está reservado para un uso futuro. |
| userContext | Especifica la información de contexto de usuario cuando se llamó la función de inicio de sesión. |
| methodName | El nombre del método que realiza la llamada. |

*isLoggedIn (propiedad) (get):*

Esta propiedad obtiene el estado actual de la autenticación del usuario; se establece el objeto ScriptManager durante una solicitud de página.

Esta propiedad devuelve `true` si el usuario ha iniciado; en caso contrario, devuelve `false`.

*propiedad de ruta de acceso (get, set):*

Esta propiedad mediante programación determina la ubicación del servicio web de autenticación. Se puede utilizar para invalidar el proveedor de autenticación predeterminado, así como uno establecer mediante declaración en la propiedad de ruta de acceso del nodo secundario del control ScriptManager AuthenticationService (para obtener más información, consulte el usar un proveedor de servicios de autenticación personalizada temas siguientes).

Tenga en cuenta que no cambia la ubicación del servicio de autenticación predeterminado. Sin embargo, AJAX de ASP.NET permite especificar la ubicación de un servicio web que proporciona la misma interfaz de clase que el proxy de servicio de autenticación de AJAX de ASP.NET.

Tenga en cuenta también que esta propiedad no debe establecerse en un valor que se dirige la solicitud de script fuera del sitio actual. Dado que la aplicación actual no recibiría las credenciales de autenticación, podría resultar inservible; Además, el AJAX tecnología subyacente no debe publicar las solicitudes entre sitios y puede generar una excepción de seguridad en el explorador del cliente.

Esta propiedad es una `String` objeto que representa la ruta de acceso al servicio web de autenticación.

*propiedad de tiempo de espera (get, set):*

Esta propiedad determina el período de tiempo de espera para el servicio de autenticación antes de asumir que la solicitud de inicio de sesión ha fallado. Si se agota el tiempo de espera mientras se espera que se complete una llamada, se llamará a la devolución de llamada de error de solicitud y no se completará la llamada.

Esta propiedad es una `Number` objeto que representa el número de milisegundos de espera para obtener los resultados del servicio de autenticación.

*Ejemplo de código: Iniciar sesión en el servicio de autenticación*

El siguiente marcado es un ejemplo de página ASP.NET con una llamada de la secuencia de comandos sencilla a los métodos de inicio de sesión y cierre de sesión de la clase AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Obtener acceso a la generación de perfiles de datos a través de AJAX de ASP.NET

El servicio de generación de perfiles de ASP.NET también se expone a través de las extensiones de AJAX de ASP.NET. Puesto que el servicio de generación de perfiles de ASP.NET proporciona una API enriquecida y granular por el que se va a almacenar y recuperar datos de usuario, puede ser una herramienta de productividad excelente.

El servicio de perfil debe estar habilitado en el archivo web.config; no sucede de forma predeterminada. Para ello, asegúrese de que el `profileService` ha habilitado el elemento secundario = web.config especificado en true y que ha especificado las propiedades que se pueden leer o escritas del siguiente modo:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

También se debe configurar el servicio de perfil. Aunque la configuración del servicio de generación de perfiles está fuera del ámbito de este artículo, merece la pena tener en cuenta que los grupos según se define en la configuración de perfiles será accesibles como subpropiedades del nombre del grupo. Por ejemplo, con la siguiente sección de perfil especificada:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Script de cliente podrían tener acceso a nombre, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip y BackgroundColor como propiedades del campo de propiedades de la clase ProfileService.

Una vez configurado el servicio de generación de perfiles de AJAX, estará inmediatamente disponible en páginas. Sin embargo, tendrá que va a cargar una vez antes de su uso.

*Miembros de Sys.Services.ProfileService*

*campo de propiedades:*

El campo de propiedades expone todos los datos de perfil configurado como propiedades secundarias que se pueden hacer referencia mediante la convención de nombre de operador de punto. Propiedades que son elementos secundarios de los grupos de propiedades se denominan GroupName.PropertyName. En la configuración de perfil de ejemplo presentada anteriormente, para obtener el estado del usuario, podría utilizar el siguiente identificador:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*método de carga:*

Carga una lista seleccionada o todas las propiedades del servidor.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (valor predeterminado es null). Las propiedades que se va a cargar desde el servidor. |
| loadCompletedCallback | Opcional (valor predeterminado es null). La función que se llamará cuando haya finalizado la carga. |
| failedCallback | Opcional (valor predeterminado es null). La función se llama a si se produce un error. |
| userContext | Opcional (valor predeterminado es null). Información de contexto que se pasan a la función de devolución de llamada. |

La función de carga no tiene un valor devuelto. Si la llamada se completó correctamente, llamará a uno el `loadCompletedCallback` parámetro o `defaultLoadCompletedCallback` propiedad. Si el error en la llamada o el tiempo de espera expirado, ya sea el `failedCallback` parámetro o `defaultFailedCallback` propiedad se llamará.

Si el `propertyNames` no se proporcionó el parámetro, se recuperan todas las propiedades configuradas de lectura desde el servidor.

*Save (método):*

El método save() guarda la lista de propiedades especificado (o todas las propiedades) en el perfil del usuario ASP.NET.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (valor predeterminado es null). Las propiedades que se van a guardar en el servidor. |
| saveCompletedCallback | Opcional (valor predeterminado es null). Se ha completado la función se debe llamar cuando se guarda. |
| failedCallback | Opcional (valor predeterminado es null). La función se llama a si se produce un error. |
| userContext | Opcional (valor predeterminado es null). Información de contexto que se pasan a la función de devolución de llamada. |

La operación de guardar la función no tiene un valor devuelto. Si la llamada se completa correctamente, llamará a cualquiera la `saveCompletedCallback` parámetro o `defaultSaveCompletedCallback` propiedad. Si el error en la llamada o el tiempo de espera expirado, ya sea el `failedCallback` o `defaultFailedCallback` propiedad se llamará.

Si el `propertyNames` parámetro es null, todas las propiedades de perfil se enviarán al servidor y el servidor que decida qué propiedades se pueden guardar y cuáles no.

*Propiedad defaultFailedCallback (get, set):*

Esta propiedad especifica una función que se debe llamar si se produce un error al comunicarse con el servicio web. Debería recibir un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| Error | Especifica la información de error. |
| userContext | Especifica la información de contexto de usuario proporcionada cuando la carga o guardar la función llamó. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad defaultSaveCompleted (get, set):*

Esta propiedad especifica una función que se debe llamar cuando se complete guardar los datos de perfil del usuario. Debería recibir un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| numPropsSaved | Especifica el número de propiedades que se guardaron. |
| userContext | Especifica la información de contexto de usuario proporcionada cuando la carga o guardar la función llamó. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad defaultLoadCompleted (get, set):*

Esta propiedad especifica una función que se debe llamar cuando se complete la carga de datos de perfil del usuario. Debería recibir un delegado (o una referencia de función).

La referencia de función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| numPropsLoaded | Especifica el número de propiedades cargadas. |
| userContext | Especifica la información de contexto de usuario proporcionada cuando la carga o guardar la función llamó. |
| methodName | El nombre del método que realiza la llamada. |

*propiedad de ruta de acceso (get, set):*

Esta propiedad mediante programación determina la ubicación del servicio web de perfil. Se puede utilizar para invalidar el proveedor de servicios de perfil de manera predeterminada, así como uno establecer mediante declaración en la propiedad de ruta de acceso del nodo secundario del control ScriptManager ProfileService.

Tenga en cuenta que no cambia la ubicación del servicio de perfil predeterminado. Sin embargo, AJAX de ASP.NET permite especificar la ubicación de un servicio web que proporciona la misma interfaz de clase que el proxy de servicio de autenticación de AJAX de ASP.NET.

Tenga en cuenta también que esta propiedad no debe establecerse en un valor que se dirige la solicitud de script fuera del sitio actual. El AJAX tecnología subyacente no debe publicar las solicitudes entre sitios y puede generar una excepción de seguridad en el explorador del cliente.

Esta propiedad es una `String` objeto que representa la ruta de acceso al servicio web de perfil.

*propiedad de tiempo de espera (get, set):*

Esta propiedad determina el período de tiempo de espera para el servicio de perfil antes de asumir que la carga o Guardar solicitud ha fallado. Si se agota el tiempo de espera mientras se espera que se complete una llamada, se llamará a la devolución de llamada de error de solicitud y no se completará la llamada.

Esta propiedad es una `Number` objeto que representa el número de milisegundos de espera para obtener los resultados desde el servicio de perfil.

*Ejemplo de código: cargar datos de perfil al cargar la página*

El código siguiente comprobará si un usuario está autenticado y si es así, cargará color de fondo preferido del usuario como la página.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Mediante un proveedor de servicios de autenticación personalizada*

Las extensiones de AJAX de ASP.NET permiten crear un proveedor de servicios de autenticación de secuencia de comandos personalizada mediante la exposición de su funcionalidad a través de un servicio web personalizado. Para poder usarlo, el servicio web debe exponer dos métodos, `Login` y `Logout`; y estos métodos deben especificarse con las mismas firmas de método que el servicio web de autenticación de AJAX de ASP.NET de forma predeterminada.

Una vez haya creado el servicio web personalizado, debe especificar la ruta de acceso a ella, ya sea mediante declaración en la página mediante programación en código o a través de la secuencia de comandos de cliente.

*Para establecer la ruta de acceso mediante declaración:*

Para configurar la ruta de acceso mediante declaración, incluya al elemento secundario de AuthenticationService del objeto ScriptManager en la página ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Para establecer la ruta de acceso en el código:*

Para establecer la ruta de acceso mediante programación, especifique la ruta de acceso a través de la instancia del Administrador de script:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Para establecer la ruta de acceso en la secuencia de comandos:*

Para establecer la ruta de acceso mediante programación en la secuencia de comandos, use el `path` propiedad de la clase AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Servicio Web de ejemplo para una autenticación personalizada*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Resumen

Servicios ASP.NET - específicamente los servicios de generación de perfiles, la pertenencia y la autenticación - fácilmente se exponen a JavaScript en el explorador del cliente. Esto permite a los desarrolladores integrar su código del lado cliente con el mecanismo de autenticación sin problemas, sin tener que depender de los controles como UpdatePanel para hacer el trabajo pesado. Se pueden proteger los datos de perfil del cliente, mediante el uso de valores de configuración de web; No hay datos disponibles de forma predeterminada, y los desarrolladores deben participar en las propiedades de perfil.

Además, mediante la creación de las implementaciones del servicio web simplificada con firmas de método equivalente, los desarrolladores pueden crear proveedores de secuencia de comandos personalizada para estos servicios intrínsecos de ASP.NET. Compatibilidad con estas técnicas simplifica el desarrollo de aplicaciones cliente enriquecidas, al proporcionar a los desarrolladores con una amplia gama de flexibilidad para satisfacer las necesidades específicas.

## <a name="bio"></a>*Bio*

Scott categoría ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el director general de myKB.com ([www.myKB.com](http://www.myKB.com)) donde está especializado en la escritura de ASP.NET en función de las aplicaciones que se centra en las soluciones de Software de Base de conocimiento. Scott se puede contactar a través de correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Anterior](understanding-asp-net-ajax-updatepanel-triggers.md)
[Siguiente](understanding-asp-net-ajax-localization.md)
