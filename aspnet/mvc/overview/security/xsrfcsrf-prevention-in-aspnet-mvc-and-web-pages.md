---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevención de XSRF/CSRF en ASP.NET MVC y Web Pages | Microsoft Docs
author: Rick-Anderson
description: Falsificación de solicitud entre sitios (también conocida como XSRF o CSRF) es un ataque contra aplicaciones hospedadas en web mediante el cual un sitio web malintencionado puede influir en el interactivo...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 5db661cccc58d1101f95091b069ab5cbfe78a378
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577943"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevención de XSRF/CSRF en ASP.NET MVC y Web Pages
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Solicitud entre sitios que falsificación (también conocida como XSRF o CSRF) es un ataque contra aplicaciones hospedadas en web mediante el cual un sitio web malintencionado puede influir en la interacción entre un explorador cliente y un sitio web de confianza de ese explorador. Estos ataques se posible porque los exploradores web enviarán tokens de autenticación automáticamente con cada solicitud a un sitio web. El ejemplo canónico es una cookie de autenticación, por ejemplo, ASP. Vale de autenticación de formularios de la red. Sin embargo, los sitios web que use cualquier mecanismo de autenticación persistente (por ejemplo, la autenticación de Windows, Basic etc.) puede tener como destino estos ataques.
> 
> Un ataque XSRF es distinto de un ataque de suplantación de identidad. Los ataques de suplantación de identidad requieren interacción de la víctima. En un ataque de suplantación de identidad, un sitio web malintencionado imitará el sitio web de destino y la víctima se engañado para que proporcione información confidencial al atacante. En un ataque XSRF, no suele haber ninguna interacción necesaria de la víctima. En su lugar, el atacante confía en el explorador enviando automáticamente todas las cookies relevantes al sitio web de destino.
> 
> Para obtener más información, consulte el [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomía de un ataque

Para recorrer en iteración un ataque XSRF, considere la posibilidad de un usuario que desea realizar algunas transacciones bancarias en línea. En primer lugar, este usuario visita WoodgroveBank.com y registros, momento en que el encabezado de respuesta contendrá la cookie de autenticación:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Dado que la cookie de autenticación es una cookie de sesión, lo se borrará automáticamente el explorador cuando se cierra el proceso del explorador. Sin embargo, hasta ese momento, el explorador incluirá automáticamente la cookie con cada solicitud a WoodgroveBank.com. Ahora, el usuario desea transferir 1000 USD a otra cuenta, por lo que rellena un formulario en el sitio de banca, y el explorador realiza esta solicitud al servidor:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Dado que esta operación tiene un efecto secundario (inicia una transacción monetaria), ha decidido que el sitio de banca requieren una solicitud HTTP POST con el fin de iniciar la operación. El servidor lee el token de autenticación en la solicitud, busca el número de cuenta del usuario actual, comprueba que existe suficientes fondos y, a continuación, inicia la transacción en la cuenta de destino.

Banca en línea completa, el usuario navega fuera del sitio de la banca y propia visita otras ubicaciones en la web. Uno de esos sitios – fabrikam.com: incluye el siguiente marcado en una página insertada dentro de un &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Lo que hace que el explorador realizar esta solicitud:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

El atacante está aprovechando el hecho de que el usuario puede seguir teniendo un token de autenticación válido para el sitio web de destino y que está utilizando un pequeño fragmento de Javascript para hacer que el explorador realizar una solicitud HTTP POST en el sitio de destino automáticamente. Si el token de autenticación sigue siendo válido, el sitio de banca iniciará a una transferencia de 250 dólares en la cuenta de elección del atacante.

### <a name="ineffective-mitigations"></a>Mitigaciones ineficaces

Es interesante tener en cuenta que en el escenario anterior, el hecho de que WoodgroveBank.com se tenía acceso a través de SSL y tenía una cookie de autenticación solo SSL era insuficiente para frustrar los ataques. El atacante es capaz de especificar el [esquema URI](http://en.wikipedia.org/wiki/URI_scheme) (https) en su &lt;formulario&gt; elemento y el explorador seguirá enviando las cookies que no hayan expirado al sitio de destino siempre y cuando las cookies son coherentes con el identificador URI esquema del destino previsto.

Uno podría argumentar que el usuario simplemente no visite sitios de confianza, como visitar solo sitios de confianza se ayuda a permanecer seguro en línea. Hay algo de verdad a esto, pero Desgraciadamente este Consejo no siempre resulta práctico. Quizás el usuario "confía" el sitio de noticias locales ConsolidatedMessenger. ConsolidatedMessenger.com y se va a visitar ese sitio en su lugar, pero ese sitio tiene una vulnerabilidad XSS que permite que un atacante insertar el mismo fragmento de código que se estaba ejecutando en fabrikam.com.

Puede comprobar que las solicitudes entrantes tengan una [encabezado Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) que hacen referencia a su dominio. Esto detendrá las solicitudes enviadas al cambiar de un dominio de otros fabricantes. Sin embargo, algunas personas deshabilitar encabezado Referer de su explorador por motivos de privacidad y los atacantes pueden suplantar a veces ese encabezado si el sujeto tiene cierta inseguro software instalado. Comprobando la [encabezado Referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) no se considera un enfoque seguro para la prevención de ataques XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Mitigaciones de pila en tiempo de ejecución XSRF Web

El tiempo de ejecución de ASP.NET Web Stack utiliza una variante de la [patrón del token Sincronizador](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) para defenderse contra ataques XSRF. La forma general del patrón de token Sincronizador es que dos tokens de anti-XSRF se envían al servidor con cada solicitud HTTP POST (además del token de autenticación): un token como una cookie y el otro como un valor de formulario. Los valores del token generados por el tiempo de ejecución ASP.NET no son deterministas o predecible por un atacante. Cuando se envían los tokens, el servidor permitirá la solicitud continuar sólo si ambos tokens pasan una comprobación de comparación.

La comprobación de solicitud XSRF *token de sesión* se almacena como una cookie HTTP y actualmente contiene la siguiente información en su carga:

- Un token de seguridad, que consta de un identificador de 128 bits aleatorio.   
 La siguiente imagen muestra el token de sesión de verificación de XSRF solicitud muestra con las herramientas de desarrollo F12 de Internet Explorer: (tenga en cuenta esto es la implementación actual y está sujeto, incluso probable, que cambie.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

El *token campo* se almacena como un `<input type="hidden" />` y contiene la siguiente información en su carga:

- Nombre de usuario del usuario que ha iniciado la sesión (si está autenticado).
- Datos adicionales proporcionados por un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Las cargas de los tokens de anti-XSRF están cifradas y firmadas, por lo que no se puede ver el nombre de usuario al utilizar herramientas para examinar los tokens. Cuando la aplicación web está destinado a ASP.NET 4.0, servicios criptográficos proporcionados por el [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) rutina. Cuando la aplicación web está destinado a ASP.NET 4.5 o superior servicios criptográficos proporcionados por el [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) rutina, que ofrece un mejor rendimiento, extensibilidad y seguridad. Consulte que el siguientes entradas de blog para obtener más detalles:

- [Las mejoras criptográficas en ASP.NET 4.5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Las mejoras criptográficas en ASP.NET 4.5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Las mejoras criptográficas en ASP.NET 4.5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generar los tokens

Para generar los tokens de anti-XSRF, llame a la [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) método desde una vista de MVC o @AntiForgery.GetHtml() desde una página de Razor. El tiempo de ejecución, a continuación, realizará los pasos siguientes:

1. Si la solicitud HTTP actual ya contiene un token de sesión de anti-XSRF (la cookie de anti-XSRF \_ \_RequestVerificationToken), se extrae el token de seguridad de ella. Si la solicitud HTTP no contiene un token de sesión de anti-XSRF o si se produce un error en la extracción del token de seguridad, se generará un nuevo token anti-XSRF aleatorio.
2. Se genera un token de campo anti-XSRF mediante el token de seguridad del paso (1) anterior y la identidad del usuario ha iniciado la sesión actual. (Para obtener más información acerca de cómo determinar la identidad del usuario, consulte el **[escenarios con compatibilidad especial](#_Scenarios_with_special)** sección más adelante.) Además, si un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) está configurado, el tiempo de ejecución llamará a su [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) método e incluir la cadena devuelta en el token de campo. (Consulte la **[configuración y extensibilidad](#_Configuration_and_extensibility)** sección para obtener más información.)
3. Si se ha generado un nuevo token anti-XSRF en el paso (1), un nuevo token de sesión que lo contiene, se creará y se agregará a la colección de cookies HTTP saliente. El token de campo del paso (2) se ajustará en un `<input type="hidden" />` elemento y este marcado HTML será el valor devuelto de `Html.AntiForgeryToken()` o `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Validando los tokens

Para validar los tokens de anti-XSRF entrante, el desarrollador incluye un [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) atributo en su acción de MVC o controlador llamadas `@AntiForgery.Validate()` desde su página de Razor. El tiempo de ejecución llevará a cabo los pasos siguientes:

1. Se leen el token de sesión entrante y el token de campo y extrae el token anti-XSRF de cada uno. Los tokens de anti-XSRF deben ser idénticos en cada paso (2) en la rutina de generación.
2. Si el usuario actual está autenticado, el nombre de usuario se compara con el nombre de usuario almacenado en el token de campo. Deben coincidir con los nombres de usuario.
3. Si un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) está configurado, el runtime llama a su *ValidateAdditionalData* método. El método debe devolver el valor booleano *true*.

Si la validación es correcta, se permite la solicitud para continuar. Si se produce un error de validación, el marco de trabajo producirá un *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condiciones de error

A partir de la ASP.NET Web Stack Runtime v2, cualquier *HttpAntiForgeryException* que se produce durante la validación contendrá información detallada sobre qué salió mal. Las condiciones de error definidos actualmente son:

- El token de sesión o el token de formulario no está presente en la solicitud.
- El token de sesión o el token de formulario es ilegible. La causa más probable de esta es una granja de servidores que ejecutan versiones no coincidentes de la ASP.NET Web Stack en tiempo de ejecución o una granja de servidores donde el &lt;machineKey&gt; elemento en el archivo Web.config es diferente entre máquinas. Puede usar una herramienta como Fiddler para forzar esta excepción mediante la alteración con cualquier token anti-XSRF.
- Se intercambiaron el token de sesión y el token de campo.
- El token de sesión y el token de campo contienen los tokens de seguridad que no coinciden.
- El nombre de usuario incrustado en el símbolo (token) de campo no coincide con el nombre de usuario ha iniciado la sesión del usuario actual.
- El *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* devuelto del método *false*.

Las instalaciones de anti-XSRF también pueden realizar comprobaciones adicionales durante la generación de tokens y errores durante estas comprobaciones pueden producir excepciones que se producen. Vea el [WIF y ACS / basada en notificaciones autenticación](#_WIF_ACS) y **[configuración y extensibilidad](#_Configuration_and_extensibility)** secciones para obtener más información.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Escenarios con compatibilidad especial

### <a name="anonymous-authentication"></a>Autenticación anónima

El sistema de anti-XSRF contiene compatibilidad especial para los usuarios anónimos, donde "anónimo" se define como un usuario donde la *IIdentity.IsAuthenticated* propiedad devuelve *false*. Los escenarios incluyen proporcionando protección de XSRF a la página de inicio de sesión (antes de que el usuario se autentica) y los esquemas de autenticación personalizados donde la aplicación utiliza un mecanismo distinto *IIdentity* para identificar a los usuarios.

Para admitir estos escenarios, recuerde que los tokens de sesión y el campo están unidos por un token de seguridad, que es un identificador opaco generada de forma aleatoria de 128 bits. Este token de seguridad se usa para realizar un seguimiento de sesión de un usuario individual como navega el sitio, por lo que efectivamente tiene la finalidad de un identificador anónimo. Se utiliza una cadena vacía en lugar del nombre de usuario para las rutinas de validación y generación que se ha descrito anteriormente.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF y ACS basado en notificaciones de autenticación

Normalmente, el *IIdentity* clases integradas en .NET Framework tienen la propiedad que *IIdentity.Name* es suficiente para identificar de forma exclusiva un usuario determinado dentro de una aplicación determinada. Por ejemplo, *FormsIdentity.Name* devuelve el nombre de usuario almacenado en la base de datos de pertenencia (que es único para todas las aplicaciones según esa base de datos), *WindowsIdentity.Name* devuelve el identidad de dominio completo del usuario y así sucesivamente. Estos sistemas proporcionan no solo la autenticación; también *identificar* usuarios en una aplicación.

Autenticación basada en notificaciones, por otro lado, no requieren necesariamente la identificación de un usuario determinado. En su lugar, el *ClaimsPrincipal* y *ClaimsIdentity* tipos están asociados a un conjunto de *notificación* instancias, donde las notificaciones individuales pueden ser "es más de 18 años de edad" o " es un administrador"para cualquier otra cosa. Puesto que el usuario no se identificó necesariamente, el tiempo de ejecución no se puede usar el *ClaimsIdentity.Name* propiedad como un identificador único para este usuario concreto. El equipo ha detectado ejemplos reales donde *ClaimsIdentity.Name* devuelve *null*, devuelve un nombre descriptivo (presentación), o en caso contrario, devuelve una cadena que no es adecuada para su uso como un identificador único para el usuario.

Muchas de las implementaciones que utilizan autenticación basada en notificaciones usa [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) en particular. ACS permite al desarrollador configure individuales *proveedores de identidades* (por ejemplo, ADFS, el proveedor de Microsoft Account, proveedores de OpenID como Yahoo!, etc.), y devuelven los proveedores de identidades *nombre identificadores*. Estos identificadores de nombre pueden contener información de identificación personal (PII) como una dirección de correo electrónico, o podría ser similar a un identificador Personal privado (PPID) son anónimos. No obstante, la tupla (proveedor de identidad, identificador de nombre) lo suficientemente actúa como un token de seguimiento adecuado para un usuario determinado mientras ella es examinar el sitio, por lo que el tiempo de ejecución de ASP.NET Web Stack puede usar la tupla en lugar del nombre de usuario cuando se genera y validación de tokens de campo de anti-XSRF. El URI para el proveedor de identidades y el identificador de nombre determinado son:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(consulte este [página de documentación de ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) para obtener más información.)

Al generar o validar un token, el tiempo de ejecución de ASP.NET Web Stack en tiempo de ejecución intentará enlace a los tipos:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (Para el SDK de WIF).
- `System.Security.Claims.ClaimsIdentity` (Para .NET 4.5).

Si existen estos tipos y si el usuario actual *IIIIdentity* implementa o subclases uno de estos tipos, usará la utilidad anti-XSRF (proveedor de identidad, identificador de nombre) la tupla en lugar del nombre de usuario cuando se genera y Validando los tokens. Si ninguna tupla este tipo está presente, se producirá un error en la solicitud con un error que describe al desarrollador de cómo configurar el sistema de anti-XSRF para entender el mecanismo de autenticación basada en notificaciones determinado en uso. Consulte la **[configuración y extensibilidad](#_Configuration_and_extensibility)** sección para obtener más información.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID autenticación

Por último, la utilidad anti-XSRF tiene compatibilidad especial para las aplicaciones que usan la autenticación de OAuth u OpenID. Esta compatibilidad está basada en heurística: si actual *IIdentity.Name* comienza con http:// o https://, a continuación, las comparaciones de nombre de usuario se realizará utilizando un comparador Ordinal en lugar de con el comparador de OrdinalIgnoreCase predeterminado.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configuración y extensibilidad

En ocasiones, los programadores pueden desear un mayor control sobre los comportamientos de validación y generación de anti-XSRF. Por ejemplo, quizás el comportamiento predeterminado de las aplicaciones MVC y Web Pages auxiliares de agregar automáticamente las cookies HTTP para la respuesta es no deseado y el desarrollador que desee conservar los tokens en otro lugar. Existen dos API para ayudar con esto:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

El *GetTokens* método toma como entrada un token de sesión existente del comprobación de solicitud XSRF (que puede ser null) y genera como salida un nuevo token de sesión de comprobación de solicitud XSRF y el token de campo. Los tokens son cadenas opacas simplemente con ninguna decoración; el *formToken* valor por ejemplo no se ajustará en un &lt;entrada&gt; etiqueta. El *newCookieToken* valor puede ser null; si esto ocurre, el *oldCookieToken* valor sigue siendo válido y no debe establecerse ninguna nueva cookie de respuesta. El llamador de *GetTokens* es responsable de conservar las cookies de respuesta necesario o generar cualquier marcado necesario; el *GetTokens* propio método no afectará a la respuesta como un efecto secundario. El *validar* método toma la sesión entrante y los tokens de campo y ejecuta la lógica de validación mencionado anteriormente sobre ellos.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

El desarrollador puede configurar el sistema de anti-XSRF de la aplicación\_iniciar. La configuración es mediante programación. Las propiedades de estático *AntiForgeryConfig* tipo se describen a continuación. Uso de notificaciones de mayoría de los usuarios querrán establecer la propiedad UniqueClaimTypeIdentifier.

| **Property** | **Descripción** |
| --- | --- |
| **AdditionalDataProvider** | Un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) que proporciona datos adicionales durante la generación de tokens y consume datos adicionales durante la validación del token. El valor predeterminado es *null*. Para obtener más información, consulte el [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) sección. |
| **CookieName** | Una cadena que proporciona el nombre de la cookie HTTP que se utiliza para almacenar el token de sesión anti-XSRF. Si no se establece este valor, un nombre se generará automáticamente basándose en la ruta de acceso virtual de la aplicación implementada. El valor predeterminado es *null*. |
| **RequireSsl** | Un valor booleano que determina si los tokens de anti-XSRF son necesarios para enviarse a través de un canal seguro para SSL. Si este valor es *true*, las cookies que se genera automáticamente tendrán el marcador "secure" establecido y las API de anti-XSRF producirá un error si se llama desde dentro de una solicitud que no se envía a través de SSL. El valor predeterminado es *false*. |
| **SuppressIdentityHeuristicChecks** | Un valor booleano que determina si el sistema de anti-XSRF debe desactivar la compatibilidad con identidades basadas en notificaciones. Si este valor es *true*, el sistema asumirá que *IIdentity.Name* es adecuado para su uso como un identificador único por el usuario y no volverá a intentar distinguieran mayúsculas y minúsculas *IClaimsIdentity*o *ClClaimsIdentity* como se describe en el [WIF y ACS / basada en notificaciones autenticación](#_WIF_ACS) sección. El valor predeterminado es `false`. |
| **UniqueClaimTypeIdentifier** | Una cadena que indica el tipo de notificación de que es adecuada para su uso como un identificador único por el usuario. Si este valor es el conjunto y la actual *IIdentity* basada en notificaciones, el sistema intentará extraer una notificación del tipo especificado por *UniqueClaimTypeIdentifier*, y se usará el valor correspondiente en lugar del nombre de usuario del usuario al generar el token de campo. Si no se encuentra el tipo de notificación, el sistema se producirá un error de la solicitud. El valor predeterminado es *null*, lo que indica que el sistema debe utilizar (proveedor de identidad, identificador de nombre) como se describió anteriormente en lugar del nombre de usuario de tupla. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

El *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* tipo permite a los desarrolladores extender el comportamiento del sistema anti-XSRF por los datos adicionales de ida y vuelta de cada token. El *GetAdditionalData* método se llama cada vez que se genera un token de campo y el valor devuelto está incrustado en el token generado. Un implementador podría devolver una marca de tiempo, un valor nonce o cualquier otro valor que desea desde este método.

De forma similar, el *ValidateAdditionalData* método se llama cada vez que se valida un token de campo y la cadena de "datos adicionales" en el que estaba incrustada dentro del token se pasa al método. La rutina de validación podría implementar un tiempo de espera (activando la hora actual en el momento en que se almacenó cuando se creó el token), un valor nonce comprobando la rutina, o cualquier otro deseadas lógica.

## <a name="design-decisions-and-security-considerations"></a>Las decisiones de diseño y consideraciones de seguridad

Técnicamente el token de seguridad que vincula los tokens de sesión y el campo solo es necesario cuando se intenta proteger a los usuarios no autenticados y anónimos frente a ataques XSRF. Cuando el usuario está autenticado, el token de autenticación propio (supuestamente se envían en forma de una cookie) se pueden utilizar como una mitad del Sincronizador de un par de tokens. Sin embargo, hay escenarios válidos para la protección de las páginas de inicio de sesión afectadas por los usuarios no autenticados y la lógica de anti-XSRF era mucho más sencilla mediante siempre generar y validar el token de seguridad, incluso para los usuarios autenticados. Además, proporcionar una protección adicional en caso de que un token de campo se deja de ser confidencial por un atacante, como configurar o adivinar que el token de sesión sería otro obstáculo para el atacante puede superar.

Los desarrolladores deben tener cuidado cuando varias aplicaciones se hospedan en un único dominio. Por ejemplo, aunque *example1.cloudapp.net* y *example2.cloudapp.net* son diferentes de los hosts, hay una relación de confianza implícita entre todos los hosts bajo la  *\*. cloudapp.net* dominio. Esta relación de confianza implícita [permite a los hosts no sea de confianza influir en las cookies de los demás](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (el mismo origen directivas que rigen las solicitudes AJAX no necesariamente se aplican a las cookies HTTP). El tiempo de ejecución de ASP.NET Web Stack proporciona algunos mitigación en que se incrusta el nombre de usuario en el símbolo (token) de campo, por lo que incluso si un subdominio malintencionado puede sobrescribir un token de sesión será no se puede generar un token de campo válido para el usuario. Sin embargo, cuando se hospeda en un entorno las rutinas integradas anti-XSRF todavía no defienden de secuestro de sesión o inicio de sesión XSRF.

Las rutinas de anti-XSRF actualmente no defenderse contra [secuestro de clic](https://www.owasp.org/index.php/Clickjacking). Las aplicaciones que desean defenderse contra el secuestro de clic pueden hacerlo fácilmente mediante el envío de X-Frame-Options: encabezado SAMEORIGIN con cada respuesta. Este encabezado es compatible con todos los exploradores modernos. Para obtener más información, consulte el [blog de IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [blog SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), y [OWASP](https://www.owasp.org/index.php/Clickjacking). Es posible que el tiempo de ejecución de ASP.NET Web Stack en algunos Asegúrese de futuras versiones MVC y las aplicaciones auxiliares de páginas Web anti-XSRF establecen automáticamente este encabezado, por lo que las aplicaciones estén protegidas automáticamente frente a este ataque.

Los desarrolladores Web deben continuar para asegurarse de que su sitio no es vulnerable a ataques XSS. Los ataques XSS son muy eficaces y un aprovechamiento correcto también interrumpiría las defensas en tiempo de ejecución de ASP.NET Web Stack frente a ataques XSRF.

## <a name="acknowledgment"></a>Confirmación

[@LeviBroderick](https://twitter.com/LeviBroderick), gran parte del código de seguridad ASP.NET que escribió la mayor parte de esta información.
