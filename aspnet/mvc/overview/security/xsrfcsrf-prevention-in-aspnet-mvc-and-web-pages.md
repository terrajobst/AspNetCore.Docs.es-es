---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: "Prevención de XSRF/CSRF en MVC de ASP.NET y páginas Web | Documentos de Microsoft"
author: Rick-Anderson
description: "Falsificación de solicitud entre sitios (también conocido como XSRF o CSRF) es un ataque contra las aplicaciones hospedadas en web mediante el cual un sitio web malintencionado puede influir en el interactivo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 4ff4ed20d0768a48f8afb2deeb7cdb6b4c60b5bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevención de XSRF/CSRF en MVC de ASP.NET y páginas Web
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Solicitud entre sitios falsificación (también conocido como XSRF o CSRF) es un ataque contra las aplicaciones hospedadas en web mediante el cual un sitio web malintencionado puede influir en la interacción entre un explorador del cliente y un sitio web de confianza para ese explorador. Estos ataques se realizan porque los exploradores web enviará los tokens de autenticación automáticamente con cada solicitud a un sitio web. El ejemplo canónico es una cookie de autenticación, como ASP. Vale de autenticación de formularios de NET. Sin embargo, los sitios web que use cualquier mecanismo de autenticación persistente (por ejemplo, la autenticación de Windows, Basic etc.) puede tener como destino estos ataques.
> 
> Un ataque XSRF es distinto de un ataque de suplantación de identidad. Ataques de suplantación de identidad requieren interacción por parte de la víctima. En un ataque de suplantación de identidad, un sitio web malintencionado imiten el sitio web de destino y la víctima se engañado para proporcionar información confidencial con el atacante. En un ataque XSRF, no suele haber ninguna interacción necesarios de la víctima. En su lugar, el atacante confía en el explorador que se envíen automáticamente todas las cookies relevantes para el sitio web de destino.
> 
> Para obtener más información, consulte el [Abrir proyecto de seguridad de aplicación Web](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomía de un ataque

Para recorrer en iteración un ataque XSRF, considere la posibilidad de un usuario que desea realizar algunas operaciones de banca en línea. Este usuario visita por primera vez WoodgroveBank.com y registros, momento en que el encabezado de respuesta contendrá la cookie de autenticación:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Dado que la cookie de autenticación es una cookie de sesión, se se borrará automáticamente el explorador cuando termina el proceso de explorador. Sin embargo, hasta ese momento, el explorador incluirá automáticamente la cookie con cada solicitud para WoodgroveBank.com. Ahora, el usuario desea transferir los 1000 dólares a otra cuenta, por lo que rellena un formulario en el sitio de banca, y el explorador realiza dicha solicitud al servidor:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Dado que esta operación tiene un efecto secundario (inicia una transacción monetaria), el sitio de un banco elegida requerir una solicitud HTTP POST con el fin de iniciar la operación. El servidor lee el token de autenticación de la solicitud, busca el número de cuenta del usuario actual, comprueba que existe fondos suficientes y, a continuación, inicia la transacción en la cuenta de destino.

Ella banca en línea completa, el usuario se desplaza fuera del sitio de un banco o las visitas otras ubicaciones en la web. Uno de esos sitios – fabrikam.com: incluye el siguiente marcado en una página incrustado en un &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Lo que le hace que el explorador realizar esta solicitud:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

El atacante está aprovechando el hecho de que el usuario puede seguir teniendo un token de autenticación válido para el sitio web de destino, y está usando un pequeño fragmento de código de Javascript para hacer que el explorador realizar una solicitud HTTP POST en el sitio de destino automáticamente. Si el token de autenticación sigue siendo válido, el sitio de banca iniciará a una transferencia de 250 USD en la cuenta de elección del atacante.

### <a name="ineffective-mitigations"></a>Mitigaciones ineficaces

Es interesante tener en cuenta que en el escenario anterior, el hecho de que WoodgroveBank.com que se accedió a través de SSL y tenía una cookie de autenticación solo SSL era insuficiente para frustrar los ataques. El atacante es capaz de especificar el [esquema URI](http://en.wikipedia.org/wiki/URI_scheme) (https) en ella &lt;formulario&gt; elemento y el explorador continuará enviando cookies vigentes para el sitio de destino siempre y cuando las cookies son coherentes con el URI esquema de destino pretendido.

Uno podría argumentar que el usuario simplemente no visite sitios de confianza, como visitar solo sitios de confianza se ayuda a estén protegidos en línea. Hay algo de verdad para esto, pero lamentablemente estos consejos no siempre resulta práctico. Quizás el usuario "confía" del sitio de noticias locales ConsolidatedMessenger. ConsolidatedMessenger.com y llega hasta la visita ese sitio en su lugar, pero este sitio tiene una vulnerabilidad XSS que permite a un atacante insertar el mismo fragmento de código que se estaba ejecutando en fabrikam.com.

Puede comprobar que las solicitudes entrantes tengan una [encabezado Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) que hacen referencia a su dominio. Este comando detiene las solicitudes enviadas sin darse cuenta de un dominio de otro fabricante. Sin embargo, algunas personas deshabilitar falta el encabezado de su explorador por motivos de privacidad y los atacantes a veces pueden suplantar ese encabezado si el sujeto tiene cierto insegura software instalado. Comprobar la [encabezado Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) no se considera una opción segura para evitar ataques de XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Mitigaciones de pila en tiempo de ejecución XSRF Web

El tiempo de ejecución de pila de ASP.NET Web utiliza una variante de la [patrón del token Sincronizador](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) para defenderse frente a ataques XSRF. La forma general del patrón del token Sincronizador es que dos tokens de anti-XSRF se envían al servidor con cada HTTP POST (además del token de autenticación): un símbolo (token) como una cookie y el otro como un valor de formulario. Los valores de símbolo (token) generados por el tiempo de ejecución ASP.NET no son deterministas o de predicción por un atacante. Cuando se envían los tokens, el servidor permitirá la solicitud continuar sólo si ambos tokens superan una comprobación de comparación.

La comprobación de solicitud XSRF *el token de sesión* se almacena como una cookie HTTP y contiene actualmente la siguiente información en su carga:

- Un token de seguridad, que consta de un identificador de 128 bits aleatorio.   
 La siguiente imagen muestra el token de sesión de comprobación de XSRF solicitud muestra con las herramientas de desarrollo F12 de Internet Explorer: (tenga en cuenta esto es la implementación actual y está sujeta, incluso probable, que cambie.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

El *símbolo (token) de campo* se almacena como un `<input type="hidden" />` y contiene la siguiente información en su carga:

- Nombre de usuario del usuario que ha iniciado la sesión (si se autenticó).
- Cualquier dato adicional proporcionada por un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Las cargas de los tokens de anti-XSRF se cifrarán y firmarán, por lo que no se puede ver el nombre de usuario al usar herramientas para examinar los tokens. Cuando la aplicación web tiene como destino 4.0 de ASP.NET, servicios criptográficos proporcionados por el [MachineKey.Encode](https://msdn.microsoft.com/en-us/library/system.web.security.machinekey.encode.aspx) rutina. Cuando la aplicación web tiene como destino ASP.NET 4.5 o superior de servicios criptográficos proporcionados por el [MachineKey.Protect](https://msdn.microsoft.com/en-us/library/system.web.security.machinekey.protect(v=vs.110)) rutina, lo que ofrece un mejor rendimiento, extensibilidad y seguridad. Consulte que el blog siguiente se registra para obtener más detalles:

- [Mejoras criptográficas en ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Mejoras criptográficas en ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Mejoras criptográficas en ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generar los tokens

Para generar los tokens de anti-XSRF, llame a la [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/en-us/library/dd470175.aspx) método desde una vista de MVC o @AntiForgery.GetHtml() desde una página de Razor. El tiempo de ejecución, a continuación, realizará los siguientes pasos:

1. Si la solicitud HTTP actual ya contiene un token de sesión anti-XSRF (la cookie anti-XSRF \_ \_RequestVerificationToken), se extrae el token de seguridad de ella. Si la solicitud HTTP no contiene un token de sesión anti-XSRF o si se produce un error en la extracción del token de seguridad, se generará un nuevo token anti-XSRF aleatorio.
2. Se genera un token de campo anti-XSRF con el token de seguridad de paso (1) anterior y la identidad del usuario ha iniciado la sesión actual. (Para obtener más información acerca de cómo determinar la identidad del usuario, consulte la  **[escenarios con compatibilidad especial](#_Scenarios_with_special)**  sección más adelante.) Además, si un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/jj158328(v=vs.111).aspx) está configurado, el tiempo de ejecución llamará a su [GetAdditionalData](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) método e incluir la cadena devuelta en el token de campo. (Consulte la  **[configuración y extensibilidad](#_Configuration_and_extensibility)**  sección para obtener más información.)
3. Si se generó un nuevo token anti-XSRF en el paso (1), un token de nueva sesión que lo contiene, se creará y se agregará a la colección de cookies HTTP saliente. El token de campo del paso (2) se encapsulará en una `<input type="hidden" />` elemento y este marcado HTML será el valor devuelto de `Html.AntiForgeryToken()` o `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Validando los tokens

Para validar los tokens de anti-XSRF entrante, el programador incluye un [ValidateAntiForgeryToken](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) atributo en su acción de MVC o controlador o llamadas `@AntiForgery.Validate()` desde su página de Razor. El tiempo de ejecución llevará a cabo los pasos siguientes:

1. El token de sesión entrante y el token de campo se leen y extrae el token anti-XSRF de cada uno. Los tokens de anti-XSRF deben ser idénticos por paso (2) en la rutina de generación.
2. Si se autentica el usuario actual, su nombre de usuario se compara con el nombre de usuario almacenado en el token de campo. Deben coincidir con los nombres de usuario.
3. Si un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) está configurado, el runtime llama su *ValidateAdditionalData* método. El método debe devolver el valor booleano *true*.

Si la validación es correcta, se permite la solicitud para continuar. Si se produce un error en la validación, el marco de trabajo producirá un *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condiciones de error

Empezando por el Runtime de pila Web de ASP.NET v2, cualquier *HttpAntiForgeryException* que se produce durante la validación contendrá información detallada acerca del fallo. Las condiciones de error actualmente definidos son:

- El token de sesión o el token de formulario no está presente en la solicitud.
- El token de sesión o el token de formulario es ilegible. La causa más probable de esto es una granja de servidores que ejecutan versiones no coincidentes de la ejecución de pila de Web de ASP.NET o una granja de servidores donde el &lt;machineKey&gt; elemento en el archivo Web.config difiere entre equipos. Puede utilizar una herramienta como Fiddler para forzar esta excepción mediante la manipulación con cualquier token anti-XSRF.
- El token de sesión y el token de campo se cambiaron.
- El token de sesión y el token de campo contengan los tokens de seguridad no coinciden.
- El nombre de usuario incrustado en el símbolo (token) de campo no coincide con el nombre de usuario ha iniciado la sesión del usuario actual.
- El  *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)*  el método devolvió *false*.

Las funciones de anti-XSRF también pueden realizar una comprobación adicional durante la validación o la generación de tokens y errores durante estas comprobaciones pueden producir excepciones que se producen. Consulte la [WIF / ACS / basada en notificaciones autenticación](#_WIF_ACS) y  **[configuración y extensibilidad](#_Configuration_and_extensibility)**  secciones para obtener más información.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Escenarios con compatibilidad especial

### <a name="anonymous-authentication"></a>Autenticación anónima

El sistema anti-XSRF contiene compatibilidad especial para los usuarios anónimos, donde "anónimo" se define como un usuario donde la *IIdentity.IsAuthenticated* propiedad devuelve *false*. Los escenarios incluyen proporcionando protección de XSRF a la página de inicio de sesión (antes de que el usuario se autentica) y los esquemas de autenticación personalizados donde la aplicación utiliza un mecanismo distinto de *IIdentity* para identificar a los usuarios.

Para admitir estos escenarios, recuerde que los tokens de campo y de sesión están unidos por un token de seguridad, que es un identificador opaco generada de forma aleatoria de 128 bits. Este token de seguridad se utiliza para realizar un seguimiento de sesión de un usuario individual como navega el sitio, por lo que sirve para el propósito de un identificador anónimo eficazmente. Se utiliza una cadena vacía en lugar del nombre de usuario para las rutinas de validación y generación de que se ha descrito anteriormente.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF y ACS / basada en notificaciones autenticación

Normalmente, el *IIdentity* clases integradas en .NET Framework tienen la propiedad que *IIdentity.Name* es suficiente para identificar de forma única un usuario concreto dentro de una aplicación determinada. Por ejemplo, *FormsIdentity.Name* devuelve el nombre de usuario almacenado en la base de datos de pertenencia (que es único para todas las aplicaciones según esa base de datos), *WindowsIdentity.Name* devuelve el identidad de dominio completo del usuario y así sucesivamente. Estos sistemas proporcionan no solo la autenticación; también *identificar* a los usuarios a una aplicación.

Autenticación basada en notificaciones, por otro lado, no requiere necesariamente identificar un usuario determinado. En su lugar, el *ClaimsPrincipal* y *ClaimsIdentity* tipos están asociados a un conjunto de *notificación* instancias, que las notificaciones individuales pueden ser "es más de 18 años de edad" o " es un administrador"para cualquier otra cosa. Puesto que el usuario no se identificó necesariamente, el tiempo de ejecución no se puede utilizar el *ClaimsIdentity.Name* propiedad como un identificador único para este usuario concreto. El equipo ha visto ejemplos reales donde *ClaimsIdentity.Name* devuelve *null*, devuelve un nombre descriptivo (pantalla) o en caso contrario, devuelve una cadena que no es adecuada para su uso como un identificador único para el usuario.

Muchas de las implementaciones que utilizan autenticación basada en notificaciones están usando [Azure Access Control Service](https://msdn.microsoft.com/en-us/library/windowsazure/gg429786.aspx) (ACS) en particular. ACS permite al desarrollador configurar individuales *proveedores de identidades* (por ejemplo, AD FS, el proveedor de Microsoft Account, proveedores de OpenID como Yahoo!, etc.), y devuelven los proveedores de identidades *nombre identificadores*. Estos identificadores de nombre pueden contener información de identificación personal (PII) como una dirección de correo electrónico o podrían ser anónima como un identificador Personal privado (PPID). En cualquier caso, la tupla (proveedor de identidad, identificador de nombre) suficientemente actúa como un token de seguimiento adecuado para un usuario determinado mientras que explora el sitio, por lo que el tiempo de ejecución de ASP.NET Web pila, puede utilizar la tupla en lugar del nombre de usuario al generar y validar los tokens de campo anti-XSRF. El URI para el proveedor de identidades y el identificador de nombre determinado son:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(consulte esta [página del documento de ACS](https://msdn.microsoft.com/en-us/library/windowsazure/gg185971.aspx) para obtener más información.)

Al generar o validar un token, el tiempo de ejecución de ASP.NET Web pila en tiempo de ejecución intentará enlace a los tipos:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35`(Para el SDK de WIF.)
- `System.Security.Claims.ClaimsIdentity`(Para .NET 4.5).

Si existen estos tipos y si el usuario actual *IIIIdentity* implementa o subclases de uno de estos tipos, usará la utilidad anti-XSRF (proveedor de identidad, identificador de nombre) tupla en lugar del nombre de usuario al generar y Validando los tokens. Si no hay ninguna tupla este tipo, se producirá un error en la solicitud con un error que describe al programador cómo configurar el sistema anti-XSRF para conocer el mecanismo de autenticación basada en notificaciones concreto en uso. Consulte la  **[configuración y extensibilidad](#_Configuration_and_extensibility)**  sección para obtener más información.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID autenticación

Por último, la instalación de anti-XSRF ofrece una compatibilidad especial para las aplicaciones que utilizan la autenticación de OAuth u OpenID. Esta compatibilidad está basado en la heurística: si actual *IIdentity.Name* comienza con http:// o https://, a continuación, las comparaciones de nombre de usuario se realizará mediante un comparador Ordinal en lugar de con el comparador de OrdinalIgnoreCase predeterminado.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configuración y extensibilidad

En ocasiones, los desarrolladores pueden desea un mayor control sobre la generación de anti-XSRF y comportamientos de validación. Por ejemplo, quizás no es deseable comportamiento predeterminado de las aplicaciones MVC y páginas Web auxiliares de agregar automáticamente las cookies HTTP para la respuesta y el desarrollador que desee conservar los tokens en otro lugar. Existen dos API que le ayudará a esto:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

El *GetTokens* método toma como entrada un token de sesión existente del comprobación de solicitud XSRF (que puede ser null) y se genera como salida un nuevo token de sesión de comprobación de solicitud XSRF y el token de campo. Los tokens son cadenas opacas simplemente con ninguna decoración; el *formToken* valor para la instancia no se encapsulará en una &lt;entrada&gt; etiqueta. El *newCookieToken* valor puede ser null; en tal caso, el *oldCookieToken* valor sigue siendo válido y no debe establecerse ninguna nueva cookie de respuesta. El autor de llamada de *GetTokens* es responsable de conservar las cookies de respuesta necesarios o generar todas las marcas necesarias; la *GetTokens* propio método no afectará a la respuesta como un efecto secundario. El *validar* método toma la sesión entrante y campo tokens y ejecuta la lógica de validación mencionado anteriormente sobre ellos.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

El desarrollador puede configurar el sistema anti-XSRF de aplicación\_iniciar. La configuración es mediante programación. Las propiedades de los métodos estático *AntiForgeryConfig* tipo se describen a continuación. Uso de notificaciones de mayoría de los usuarios desearán establecer la propiedad UniqueClaimTypeIdentifier.

| **Property** | **Descripción** |
| --- | --- |
| **AdditionalDataProvider** | Un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) que proporciona datos adicionales durante la generación de tokens y consume datos adicionales durante la validación del token. El valor predeterminado es *null*. Para obtener más información, consulte el [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) sección. |
| **CookieName** | Una cadena que proporciona el nombre de la cookie HTTP que se utiliza para almacenar el token de sesión anti-XSRF. Si no se establece este valor, un nombre se generará automáticamente basándose en la ruta de acceso virtual de la aplicación implementada. El valor predeterminado es *null*. |
| **RequireSsl** | Un valor booleano que indica si los tokens de anti-XSRF son necesarios para enviarse a través de un canal protegido con SSL. Si este valor es *true*, las cookies automáticamente generado tendrá la marca "segura" activados y las API de anti-XSRF producirá un error si se llama desde dentro de una solicitud que no se envía a través de SSL. El valor predeterminado es *false*. |
| **SuppressIdentityHeuristicChecks** | Un valor booleano que indica si el sistema anti-XSRF debe desactivar su compatibilidad para identidades basadas en notificaciones. Si este valor es *true*, el sistema asumirá que *IIdentity.Name* es adecuado para su uso como un identificador único por el usuario y no intentará volver a casos especiales *IClaimsIdentity*o *ClClaimsIdentity* tal y como se describe en el [WIF y ACS / basada en notificaciones autenticación](#_WIF_ACS) sección. El valor predeterminado es `false`. |
| **UniqueClaimTypeIdentifier** | Una cadena que indica el tipo de notificación de que es adecuada para su uso como un identificador único por el usuario. Si este valor es el conjunto y la actual *IIdentity* basada en notificaciones, el sistema intentará extraer una notificación del tipo especificado por *UniqueClaimTypeIdentifier*, y se utilizará el valor correspondiente en lugar del nombre de usuario del usuario al generar el token de campo. Si no se encuentra el tipo de notificación, el sistema se producirá un error de la solicitud. El valor predeterminado es *null*, lo que indica que el sistema debe utilizar (proveedor de identidad, identificador de nombre) tupla descrito anteriormente en lugar del nombre de usuario del usuario. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

El  *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/en-us/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)*  tipo permite a los desarrolladores extender el comportamiento del sistema anti-XSRF por los datos adicionales de ida y vuelta de cada token. El *GetAdditionalData* método se llama cada vez que se genera un token de campo y el valor devuelto está incrustado en el token generado. Un implementador podría devolver una marca de tiempo, un nonce o cualquier otro valor que desea desde este método.

De forma similar, el *ValidateAdditionalData* método se llama cada vez que se valida un token de campo y la cadena de "datos adicionales" que estaba incrustada en el token se pasa al método. La rutina de validación podría implementar un tiempo de espera (activando la hora actual en el momento en que se almacenó cuando se creó el token), un nonce comprobación rutina o cualquier otro había deseado lógica.

## <a name="design-decisions-and-security-considerations"></a>Las decisiones de diseño y las consideraciones de seguridad

El token de seguridad que vincula los tokens de sesión y el campo técnicamente solo es necesario al intentar proteger a los usuarios anónimos / sin autenticar frente a ataques XSRF. Cuando el usuario se autentica, el token de autenticación propio (presumiblemente enviada en forma de una cookie) podría usarse como una mitad de sincronizador de un par de tokens. Sin embargo, hay escenarios válidos para la protección de páginas de inicio de sesión de aciertos por los usuarios no autenticados y la lógica de anti-XSRF se realizó más sencilla siempre generar y validar el token de seguridad, incluso para los usuarios autenticados. También proporcionan algunos protección adicional en caso de que un atacante, como establecer o adivinar que el token de sesión podría ser otro obstáculo para superar el atacante pone en peligro alguna vez un token de campo.

Los desarrolladores deben tener cuidado cuando varias aplicaciones se hospedan en un único dominio. Por ejemplo, aunque *example1.cloudapp.net* y *example2.cloudapp.net* son diferentes de los hosts, hay una relación de confianza implícita entre todos los hosts bajo la  *\*. cloudapp.net* dominio. Esta relación de confianza implícita [permite a los hosts no sea de confianza influir en sus respectivas cookies](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (las directivas de mismo origen que rigen las solicitudes AJAX no necesariamente se aplican a las cookies HTTP). El tiempo de ejecución de ASP.NET Web pila proporciona algunos mitigación en que el nombre de usuario se incrusta en el símbolo (token) de campo, por lo que incluso si un subdominio malintencionado puede sobrescribir un token de sesión no podrá generar un token de campo válido para el usuario. Sin embargo, cuando se hospeda en un entorno de este tipo las rutinas de anti-XSRF integrados todavía no ofrece protección frente secuestro de sesión o inicio de sesión XSRF.

Las rutinas de anti-XSRF actualmente no defenderse contra [clickjacking](https://www.owasp.org/index.php/Clickjacking). Las aplicaciones que se va a defenderse contra clickjacking pueden hacer fácilmente mediante el envío de X-Frame-Options: encabezado SAMEORIGIN con cada respuesta. Este encabezado es compatible con todos los exploradores modernos. Para obtener más información, consulte el [blog de IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [blog SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), y [OWASP](https://www.owasp.org/index.php/Clickjacking). El tiempo de ejecución de ASP.NET Web pila posible en algunos Asegúrese de futuras versiones MVC y aplicaciones auxiliares de páginas Web anti-XSRF establecen automáticamente este encabezado para que las aplicaciones estén protegidas automáticamente frente a este ataque.

Los desarrolladores Web deben continuar para asegurarse de que su sitio no es vulnerable a los ataques XSS. Los ataques XSS son muy eficaces y un ataque con éxito, también interrumpiría las defensas de tiempo de ejecución de ASP.NET Web pila frente a ataques XSRF.

## <a name="acknowledgment"></a>Confirmación

[@LeviBroderick](https://twitter.com/LeviBroderick), gran parte del código de seguridad ASP.NET que escribió la mayor parte de esta información.
