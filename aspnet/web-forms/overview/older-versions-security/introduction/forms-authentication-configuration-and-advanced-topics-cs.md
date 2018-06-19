---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Los formularios de configuración de autenticación y temas avanzados (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial se examinará las distintas configuraciones de autenticación de formularios y vea cómo modificarlas a través del elemento de formularios. Esto supondrá un detallada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: d6578737478fb86f64be261925becc3adec33247
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891785"
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Configuración de autenticación de formularios y temas avanzados (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> En este tutorial se examinará las distintas configuraciones de autenticación de formularios y vea cómo modificarlas a través del elemento de formularios. Esto supondrá una visión detallada de personalizar el valor de tiempo de espera del vale de autenticación de formularios, mediante una página de inicio de sesión con una dirección URL personalizada (por ejemplo, SignIn.aspx en lugar de Login.aspx) y vales de autenticación de formularios sin cookies.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](an-overview-of-forms-authentication-cs.md) analizamos los pasos necesarios para implementar la autenticación mediante formularios en una aplicación de ASP.NET, al especificar valores de configuración en el archivo Web.config para crear un registro en la página para mostrar diferentes contenido de los usuarios autenticados y anónimos. Recuerde que hemos configurado el sitio Web para utilizar autenticación de formularios estableciendo el atributo de modo de la &lt;autenticación&gt; elemento a los formularios. El &lt;autenticación&gt; elemento puede incluir opcionalmente un &lt;formularios&gt; elemento secundario, a través del cual se puede especificar una gran variedad de opciones de autenticación de formularios.

En este tutorial se examina las distintas configuraciones de autenticación de formularios y vea cómo modificarlas a través de la &lt;formularios&gt; elemento. Esto supondrá una visión detallada de personalizar el valor de tiempo de espera del vale de autenticación de formularios, mediante una página de inicio de sesión con una dirección URL personalizada (por ejemplo, SignIn.aspx en lugar de Login.aspx) y vales de autenticación de formularios sin cookies. También examinar más detenidamente la composición del vale de autenticación de formularios y vea las precauciones que ASP.NET necesario para garantizar la seguridad de inspección y a la alteración de los datos del vale. Por último, veremos cómo almacenar datos de usuario adicionales en el vale de autenticación de formularios y cómo modele estos datos a través de un objeto de entidad de seguridad personalizado.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Paso 1: Examinar la &lt;formularios&gt; valores de configuración

El sistema de autenticación de formularios de ASP.NET ofrece una serie de opciones de configuración que se pueden personalizar en una base de aplicación por aplicación. Esto incluye opciones como: vale de la duración de la autenticación de formularios; ¿Qué tipo de protección se aplica al vale; con la autenticación sin cookies de qué condiciones se utilizan vales; la ruta de acceso a la página de inicio de sesión; y otra información. Para modificar los valores predeterminados, agregue un [ &lt;formularios&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) como un elemento secundario de la [ &lt;autenticación&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx), especificar esas propiedades los valores que desee personalizar como atributos XML de este modo:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Tabla 1 se resumen las propiedades que se pueden personalizar mediante el &lt;formularios&gt; elemento. Puesto que el archivo Web.config es un archivo XML, los nombres de atributo en la columna izquierda distinguen mayúsculas de minúsculas.


| <strong>Attribute</strong> |                                                                                                                                                                                                                                     <strong>Descripción</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         sin cookies         |                                                                                                                Este atributo especifica bajo qué condiciones se almacena el vale de autenticación en una cookie en comparación con la que se incrusta en la dirección URL. Valores permitidos son: UseCookies; UseUri; Detección automática; y UseDeviceProfile (valor predeterminado). Paso 2 examina esta configuración con más detalle.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indica la dirección URL que se redirigen a los usuarios después de iniciar sesión en la página de inicio de sesión si no hay ningún valor de URL de redireccionamiento especificado en la cadena de consulta. El valor predeterminado es default.aspx.                                                                                                                                                         |
|           dominio           | Cuando se utiliza vales de autenticación basado en cookies, esta configuración especifica el valor de dominio de la cookie. El valor predeterminado es una cadena vacía, que hace que el explorador que utilice el dominio desde el que se emitió (por ejemplo, www.yourdomain.com). En este caso, la cookie le <strong>no</strong> enviará al realizar solicitudes a subdominios, como admin.yourdomain.com. Si desea que la cookie que se pasan a todos los subdominios que necesite personalizar el atributo de dominio si se establece como "sudominio.com". |
|  enableCrossAppRedirects   |                                                                                                                                                                   Un valor booleano que indica si los usuarios autenticados se memorizan al redirigir a direcciones URL en otras aplicaciones web en el mismo servidor. El valor predeterminado es false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      La dirección URL de la página de inicio de sesión. El valor predeterminado es login.aspx.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Cuando se usa vales de autenticación basado en cookies, el nombre de la cookie. El valor predeterminado es. ASPXAUTH.                                                                                                                                                                                                   |
|            ruta de acceso            |                                                                             Cuando se utiliza vales de autenticación basado en cookies, esta configuración especifica el atributo de ruta de acceso de la cookie. El atributo de ruta de acceso permite que un programador limitar el ámbito de una cookie a una jerarquía de directorio concreto. El valor predeterminado es /, que informa el explorador para que envíe la cookie de vale de autenticación a cualquier solicitud que se producen en el dominio.                                                                              |
|         protección         |                                                                                                                                            Indica qué técnicas se utilizan para proteger el vale de autenticación de formularios. Los valores permitidos son: todos (predeterminado); Cifrado; None; y la validación. Estas opciones se describen en detalle en el paso 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Un valor booleano que indica si se requiere una conexión SSL para transmitir la cookie de autenticación. El valor predeterminado es false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Un valor booleano que indica que si el tiempo de espera de la cookie de autenticación se restablece cada vez que el usuario visita el sitio durante una sola sesión. El valor predeterminado es true. La directiva de tiempo de espera de vale de autenticación se describe con más detalle en la especificación sección del valor de tiempo de espera del vale.                                                                                                 |
|          tiempo de espera           |                                                                                                                               Especifica el tiempo, en minutos, tras el cual expira la cookie de vale de autenticación. El valor predeterminado es 30. La directiva de tiempo de espera de vale de autenticación se describe con más detalle en la especificación sección del valor de tiempo de espera del vale.                                                                                                                               |

**Tabla 1**: un resumen de la &lt;formularios&gt; atributos del elemento

En ASP.NET 2.0 y versiones posteriores, el valor predeterminado los valores de autenticación de formularios están codificados en la clase FormsAuthenticationConfiguration en .NET Framework. Las modificaciones se deben aplicar en una base de aplicación por aplicación en el archivo Web.config. Esto difiere de ASP.NET 1.x, donde los valores de autenticación de formularios predeterminados se almacenan en el archivo machine.config (y, por tanto, se podrán modificar a través de edición machine.config). En el tema de ASP.NET 1.x, merece la pena mencionar que un número de la configuración del sistema de autenticación de formularios tiene valores predeterminados diferentes en ASP.NET 2.0 y versiones posteriores que en ASP.NET 1.x. Si la aplicación va a migrar desde un entorno de ASP.NET 1.x, es importante tener en cuenta estas diferencias. Consulte [el &lt;formularios&gt; documentación técnica de elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) para obtener una lista de las diferencias.

> [!NOTE]
> Varios de los ajustes de la autenticación de formularios, como el tiempo de espera, el dominio y la ruta de acceso, especifican los detalles de la cookie de vale de autenticación de formularios resultante. Para obtener más información sobre las cookies, cómo funcionan y sus diversas propiedades, lea [este tutorial Cookies](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Especificar valor de tiempo de espera del vale

El vale de autenticación de formularios es un token que representa una identidad. Con vales de autenticación basado en cookies, este token se mantiene en la forma de una cookie y enviado al servidor web en cada solicitud. Posesión del token, en esencia, declara, soy *nombre de usuario*, ya han iniciado sesión en y se usa para que la identidad de un usuario se puede guardar todas las visitas de página.

El vale de autenticación de formularios no solo incluye la identidad del usuario, sino que también contiene información para ayudar a garantizar la integridad y la seguridad del token. Después de todo, no queremos un usuario para poder crear un token falsificado, o para modificar un token legítimas de alguna manera underhanded malintencionado.

Un bit de este tipo de información incluida en el vale es un *caducidad*, que es la fecha y hora el vale no es válido. Cada vez que FormsAuthenticationModule inspecciona un vale de autenticación, garantiza que caducidad del vale todavía no ha pasado. Si lo tiene, se omite el vale y se identifica al usuario como anónimo. Esta medida de seguridad ayuda a protegerse contra ataques de reproducción. Sin una fecha de expiración, si un pirata informático era capaz de obtener sus manos en el vale de autenticación válido de un usuario - quizás por obtener acceso físico a su equipo y la raíz a través de sus cookies, podría enviar una solicitud al servidor con este vale de autenticación robada y obtener acceso. Mientras que la expiración no impide que este escenario, no limita la ventana durante el cual se completará correctamente este tipo de ataque.

> [!NOTE]
> Paso 3 detalles adicionales técnicas utilizadas por el sistema de autenticación de formularios para proteger el vale de autenticación.


Al crear el vale de autenticación, el sistema de autenticación de formularios determina su expiración consultando la configuración de tiempo de espera. Como se indicó en la tabla 1, el tiempo de espera de establecer los valores predeterminados y 30 minutos, lo que significa que cuando se crea el vale de autenticación de formularios su expiración se establece en una fecha y hora 30 minutos en el futuro.

La expiración define un tiempo absoluto en el futuro cuando expira el vale de autenticación de formularios. Pero normalmente es conveniente implementar una expiración variable, uno que se restablece cada vez que el usuario vuelve a visitar el sitio a los desarrolladores. Este comportamiento está determinado por la configuración de slidingExpiration. Si establece en true (valor predeterminado), cada vez que FormsAuthenticationModule autentica un usuario, actualiza caducidad del vale. Si se establece en false, la expiración no se actualiza en cada solicitud, por lo que el vale caduque exactamente el tiempo de espera número de minutos transcurridos tras cuando el vale fue el primero creado.

> [!NOTE]
> La expiración que se almacenan en el vale de autenticación es una fecha absoluta y el valor de tiempo, como el 2 de agosto de 2008 11:34 AM. Además, la fecha y hora son relativo a la hora de local del servidor web. Esta decisión de diseño puede tener algunos efectos secundarios interesantes alrededor de horario de verano (DST), que es cuando los relojes de los Estados Unidos se adelanta una hora (suponiendo que el servidor web se hospeda en una configuración regional que se observa el horario de verano). Tenga en cuenta lo que sucedería para un sitio Web ASP.NET con una expiración de 30 minutos sobre la hora en que comienza el horario de verano (que es a las 2:00 A.M.). Imagine que un visitante inicia sesión el sitio del 11 de marzo de 2008 en 1:55 a. M. Esto generaría un vale de autenticación de formularios que expira en 11 de marzo de 2008 a las 2:25 A.M. (30 minutos en el futuro). Sin embargo, una vez RTM 2:00 A.M., el reloj salta hasta las 3:00 A.M. a causa de horario de verano. Cuando el usuario carga una nueva página seis minutos después de iniciar sesión (a las 3:01 A.M.), FormsAuthenticationModule indica que el vale ha caducado y redirige al usuario a la página de inicio de sesión. Para obtener una explicación más exhaustiva sobre este y otros problemas de tiempo de espera de vale de autenticación, así como soluciones alternativas, adquiera un ejemplar de Stefan Schackow *Professional ASP.NET 2.0 seguridad, la pertenencia y la administración de roles* (ISBN: 978-0-7645-9698-8).


Figura 1 muestra el flujo de trabajo cuando slidingExpiration se establece en false y tiempo de espera se establece en 30. Tenga en cuenta que el vale de autenticación que se generan en el inicio de sesión contiene la fecha de expiración, y este valor no se actualiza en las solicitudes subsiguientes. Si FormsAuthenticationModule descubre que ha caducado el vale, lo descarta y trata la solicitud como anónimo.


[![Una representación gráfica de slidingExpiration de expiración cuando del vale autenticación de formularios es false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Figura 01**: una representación gráfica de slidingExpiration de expiración cuando del vale autenticación de formularios es false ([haga clic aquí para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


La figura 2 muestra el flujo de trabajo cuando slidingExpiration se establece en true y el tiempo de espera se establece en 30. Cuando se recibe una solicitud autenticada (con un vale no hayan caducado) se actualiza su expiración en tiempo de espera número de minutos en el futuro.


[![Una representación gráfica del vale de autenticación de formularios cuando slidingExpiration es true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Figura 02**: una representación gráfica del vale de autenticación de formularios cuando slidingExpiration es true ([haga clic aquí para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Cuando se utiliza vales de autenticación basado en cookies (valor predeterminado), esta explicación se convierte en un poco más confusa porque las cookies también pueden tener sus propios caducados especificados. Expiración de una cookie (o ausencia de las mismas) indica al explorador cuando se debe destruir la cookie. Si la cookie no tiene una fecha de expiración, se destruye cuando se cierra el explorador. Si hay una fecha de expiración, sin embargo, la cookie permanece almacenada en el equipo del usuario hasta la fecha y transcurra el tiempo especificado en la expiración. Cuando se destruye una cookie con el explorador, ya no se envía al servidor web. Por lo tanto, la destrucción de una cookie es análoga al usuario que inicia sesión en el sitio.

> [!NOTE]
> Por supuesto, un usuario puede quitar proactivamente las cookies almacenadas en su equipo. En Internet Explorer 7, que se vaya a herramientas, opciones y haga clic en el botón Eliminar en la sección de historial de exploración. Desde allí, haga clic en el botón de eliminar las cookies.


El sistema de autenticación de formularios crea las cookies de sesión o expiración dependiendo del valor pasado a la *persistCookie* parámetro. Recuerde que los métodos de GetAuthCookie, SetAuthCookie y RedirectFromLoginPage de la clase FormsAuthentication toman dos parámetros de entrada: *nombre de usuario* y *persistCookie*. La página de inicio de sesión que se creó en el tutorial anterior incluye una casilla Recordar mi cuenta, que determina si se crea una cookie persistente. Las cookies persistentes están basados en la expiración; las cookies no persistentes son basados en sesión.

Los conceptos de tiempo de espera y slidingExpiration ya mencionados aplican a la misma para ambos cookies basadas en sesión y expiración. Hay solo una diferencia menor en ejecución: al usar cookies de expiración con slidingTimeout establecido en true, expiración de la cookie solo se actualiza cuando más que ha transcurrido la mitad del tiempo especificado.

Vamos a actualizar directivas de tiempo de espera de vale de autenticación de nuestro sitio Web para vales de tiempo de espera después de una hora (60 minutos), con una expiración variable. Para aplicar este cambio, actualice el archivo Web.config, agregar un &lt;formularios&gt; elemento a la &lt;autenticación&gt; elemento con el siguiente marcado:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Mediante una dirección URL de página de inicio de sesión que no sea Login.aspx

Puesto que FormsAuthenticationModule redirige automáticamente los usuarios no autorizados a la página de inicio de sesión, debe conocer la dirección URL de la página de inicio de sesión. Esta dirección URL especificada en el atributo loginUrl en el &lt;formularios&gt; elemento y el valor predeterminado es login.aspx. Si va a trasladar a través de un sitio Web existente, puede haber una página de inicio de sesión con una dirección URL diferente, uno que ya se ha marcado e indexado por motores de búsqueda. En lugar de cambiar el nombre de la página de inicio de sesión existente a login.aspx e importantes vínculos y los marcadores de los usuarios, en su lugar, puede modificar el atributo loginUrl para que apunte a la página de inicio de sesión.

Por ejemplo, si su página de inicio de sesión se denomina SignIn.aspx y se encuentra en el directorio de usuarios, podría apuntar el ajuste de configuración loginUrl ~/Users/SignIn.aspx así:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Puesto que la aplicación actual ya tiene una página de inicio de sesión denominada Login.aspx, no es necesario especificar un valor personalizado en el &lt;formularios&gt; elemento.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Paso 2: Uso de vales de autenticación de formularios sin cookies

De forma predeterminada el sistema de autenticación de formularios determina si se debe almacenar sus vales de autenticación en la colección de cookies o incrustarlos en la dirección URL basada en el agente de usuario visita el sitio. Todos los exploradores de escritorio estándar, como las cookies de soporte técnico de Internet Explorer, Firefox, Opera y Safari, pero no todos los dispositivos móviles.

La directiva de cookie utilizada por el sistema de autenticación de formularios depende del valor sin cookies en el &lt;formularios&gt; elemento, que se puede asignar uno de cuatro valores:

- UseCookies - especifica que siempre se utilizará los vales de autenticación basado en cookies.
- UseUri - indica que nunca se utilizará los vales de autenticación basado en cookies.
- No se usan la detección automática: si el perfil de dispositivo no es compatible con las cookies, vales de autenticación basado en cookies; Si el perfil del dispositivo las admite, se utiliza un mecanismo de sondeo para determinar si las cookies están habilitadas.
- UseDeviceProfile - el valor predeterminado; utiliza vales de autenticación basado en cookies solo si el perfil del dispositivo las admite. No se utiliza ningún mecanismo de sondeo.

La configuración de detección automática y UseDeviceProfile se basan en un *perfil de dispositivo* en determinar si se debe usar los vales de autenticación basado en cookies o sin cookies. ASP.NET mantiene una base de datos de varios dispositivos y sus capacidades, como si son compatibles con las cookies, ¿qué versión de JavaScript que admiten y así sucesivamente. Cada vez que un dispositivo solicita una página web desde un servidor web que se envía a lo largo de un *usuario-agente* encabezado HTTP que identifica el tipo de dispositivo. ASP.NET hace coincidir automáticamente la cadena user-agent proporcionado con el correspondiente perfil especificado en su base de datos.

> [!NOTE]
> Esta base de datos de las capacidades del dispositivo se almacena en un número de archivos XML que se ajustan a la [esquema de archivo de definición de explorador](https://msdn.microsoft.com/library/ms228122.aspx). Los archivos del perfil de dispositivo de forma predeterminada se encuentran en % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. También puede agregar archivos personalizados a la aplicación de la aplicación\_carpeta de exploradores. Para obtener más información, consulte [How To: detectar tipos de explorador en ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Dado que el valor predeterminado es UseDeviceProfile, vales de autenticación de formularios sin cookies se usará cuando se visita el sitio por un dispositivo cuyo perfil notifica que no admiten cookies.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>El vale de autenticación en la dirección URL de codificación

Las cookies son un medio natural para incluir información desde el explorador en cada solicitud para un determinado sitio Web, que es la razón por la configuración de autenticación de formularios de forma predeterminada usa cookies si el dispositivo las visitando admite. Si no se admiten las cookies, se debe emplear un medio alternativo para pasar el vale de autenticación del cliente al servidor. Es una solución alternativa habitual utilizada en entornos sin cookies codificar los datos de la cookie en la dirección URL.

Es la mejor manera de ver cómo dicha información puede estar incrustado en la dirección URL forzar que el sitio para usar los vales de autenticación sin cookies. Esto se consigue estableciendo la opción de configuración sin cookies para UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Una vez que ha realizado este cambio, visite el sitio a través de un explorador. Al visitar como usuario anónimo, las direcciones URL será exactamente como lo hacían antes. Por ejemplo, al visitar la página Default.aspx barra de direcciones de mi navegador muestra la dirección URL siguiente:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Sin embargo, al iniciar sesión, el vale de autenticación de formularios se incrusta en la dirección URL. Por ejemplo, después de visitar la página de inicio de sesión e iniciar sesión como Sam, estoy volverá a la página Default.aspx, pero esta vez de la dirección URL es:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

El vale de autenticación de formularios se han incrustado en la dirección URL. La cadena (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) representa la información del vale de autenticación con codificación hexadecimal y son los mismos datos que normalmente se almacenan en una cookie.

En orden para los vales de autenticación sin cookies para que funcione, el sistema debe codificar todas las direcciones URL en la página para incluir los datos de vale de autenticación, en caso contrario, se perderá el vale de autenticación cuando el usuario hace clic en un vínculo. Por suerte, esta lógica incrustación se realiza automáticamente. Para demostrar esta funcionalidad, abra la página Default.aspx y agregue un control de hipervínculo, establecer sus propiedades Text y NavigateUrl en Probar vínculo y SomePage.aspx, respectivamente. No importa que realmente no hay una página de nuestro proyecto denominado SomePage.aspx.

Guarde los cambios en Default.aspx y, a continuación, se visita a través de un explorador. Inicie sesión en el sitio para que el vale de autenticación de formularios se incrusta en la dirección URL. A continuación, en Default.aspx, haga clic en el vínculo de vínculo de prueba. ¿Qué ocurre? Si no existe ninguna página denominada SomePage.aspx, a continuación, se ha producido un error 404, pero no es lo que es importante aquí. En su lugar, se centran en la barra de direcciones en el explorador. Tenga en cuenta que incluye el vale de autenticación de formularios en la dirección URL.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

La dirección URL SomePage.aspx en el vínculo se convierte automáticamente en una dirección URL que incluye el vale de autenticación - no teníamos escribir ni una sola línea de código. El vale de autenticación de formularios se incrustarán automáticamente en la dirección URL de los hipervínculos que no comienzan por http:// o /. No importa si aparece el hipervínculo en una llamada a Response.Redirect, en un control de hipervínculo o en un elemento delimitador HTML (es decir, &lt;un href = "..."&gt;... &lt;/a&gt;). Siempre y cuando la dirección URL no es algo parecido a http://www.someserver.com/SomePage.aspx o /SomePage.aspx, los formularios se incrustará el vale de autenticación para que podamos.

> [!NOTE]
> Vales de autenticación de formularios sin cookies adhieren a las mismas directivas de tiempo de espera como vales de autenticación basado en cookies. Sin embargo, los vales de autenticación sin cookies son más susceptibles de ataques de reproducción, puesto que el vale de autenticación se incrusta directamente en la dirección URL. Imagine un usuario que visita un sitio Web, inicia sesión en y, a continuación, pega la dirección URL en un correo electrónico a un compañero de trabajo. Si el compañero hace clic en ese vínculo antes de alcanza la expiración, registrará que el usuario que envió el correo electrónico.


## <a name="step-3-securing-the-authentication-ticket"></a>Paso 3: Proteger el vale de autenticación

El vale de autenticación de formularios se transmite a través de la conexión en una cookie o incrustado directamente dentro de la dirección URL. Además de información de identidad, el vale de autenticación también puede incluir datos de usuario (como se verá en el paso 4). Por lo tanto, es importante que se cifran los datos del vale de los curiosos y (incluso más importante) que el sistema de autenticación de formularios puede garantizar que el vale no ha sido manipulado.

Para garantizar la privacidad de los datos del vale, el sistema de autenticación de formularios puede cifrar los datos del vale. Error al cifrar los datos del vale envía información potencialmente confidencial a través de la conexión en texto sin formato.

Para garantizar la autenticidad de un vale, el sistema de autenticación de formularios debe *validar* el vale. Validación consiste en asegurarse de que no se ha modificado un elemento determinado de datos y se lleva a cabo a través de un  *[código de autenticación (MAC) del mensaje](http://en.wikipedia.org/wiki/Message_authentication_code)*. En pocas palabras, el MAC es una pequeña cantidad de información que identifica los datos que es necesario validar (en este caso, el vale). Si se modifican los datos representados por el equipo MAC, a continuación, al equipo MAC y los datos no coincidirá con. Además, es computacionalmente difícil para un usuario malintencionado para modificar los datos tanto generar su propio MAC para que se correspondan con los datos modificados.

Al crear (o modificar) un vale, el sistema de autenticación de formularios crea un equipo MAC y lo adjunta a los datos del vale. Cuando llega una solicitud posterior, el sistema de autenticación de formularios compara los datos de MAC y vale para validar la autenticidad de los datos del vale. La figura 3 ilustra gráficamente este flujo de trabajo.


[![Se garantiza la autenticidad del vale a través de un equipo MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Figura 03**: autenticidad del vale se garantiza a través de un equipo MAC ([haga clic aquí para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


¿Qué medidas de seguridad se aplican al vale de autenticación depende de la configuración de protección en el &lt;formularios&gt; elemento. La configuración de protección puede asignarse a uno de los tres valores siguientes:

- All: el vale está cifrado y firmado digitalmente (el valor predeterminado).
- Cifrado: cifrado solo se aplica, no se genera ningún MAC.
- Ninguno: el vale se cifran ni firmado digitalmente.
- Se genera la validación: un equipo MAC, pero los datos de vale se envían a través de la conexión en texto sin formato.

Microsoft recomienda utilizar la configuración de todos.

### <a name="setting-the-validation-and-decryption-keys"></a>Establecimiento de la validación y las claves de descifrado

El cifrado y hash algoritmos utilizados por el sistema de autenticación de formularios para cifrar y validar el vale de autenticación son personalizables a través de la [ &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx) en el archivo Web.config. Tabla 2 contornos el &lt;machineKey&gt; atributos del elemento y sus valores posibles.

| **Attribute** | **Descripción** |
| --- | --- |
| descifrado | Indica el algoritmo utilizado para el cifrado. Este atributo puede tener uno de los siguientes cuatro valores: - Auto - el valor predeterminado; Determina el algoritmo que se basa en la longitud del atributo decryptionKey. -AES - utiliza el [estándar de cifrado avanzado (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmo. -DES - utiliza el [estándar de cifrado de datos (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) este algoritmo se consideran menos seguro computacional y no debe usarse. -3DES - utiliza el [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmo, que aplica el algoritmo DES triple. |
| decryptionKey | La clave secreta usada por el algoritmo de cifrado. Este valor debe ser una cadena hexadecimal de la longitud apropiada (según el valor de descifrado), generar automáticamente o cualquiera de estos valores le anexado, IsolateApps. Agregar IsolateApps indica a ASP.NET que se usará un valor único para cada aplicación. El valor predeterminado es AutoGenerate, IsolateApps. |
| validación | Indica el algoritmo utilizado para la validación. Este atributo puede tener uno de los siguientes cuatro valores: - AES - utiliza el algoritmo estándar de cifrado avanzado (AES). -MD5 - utiliza el [síntesis del mensaje 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritmo. -SHA1 - utiliza el [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmo (el valor predeterminado). -3DES - utiliza el algoritmo Triple DES. |
| validationKey | La clave secreta usada por el algoritmo de validación. Este valor debe ser una cadena hexadecimal de la longitud apropiada (según el valor de validación), generar automáticamente o cualquiera de estos valores le anexado, IsolateApps. Agregar IsolateApps indica a ASP.NET que se usará un valor único para cada aplicación. El valor predeterminado es AutoGenerate, IsolateApps. |

**Tabla 2**: el &lt;machineKey&gt; atributos del elemento

Una discusión detallada de estas opciones de cifrado y validación y los profesionales de TI y los contras de los diversos algoritmos, queda fuera del ámbito de este tutorial. Para buscar una profundidad en estos problemas, incluidas instrucciones sobre qué algoritmos de cifrado y validación que se utilizan, las longitudes de clave que se utiliza y la mejor manera de generar estas claves, consulte *Professional ASP.NET 2.0 seguridad, la pertenencia y la administración de roles* .

De forma predeterminada, las claves utilizadas para el cifrado y la validación se generan automáticamente para cada aplicación, y dichas claves están almacenadas en la autoridad de seguridad Local (LSA). En resumen, la configuración predeterminada garantiza que las claves únicas de un servidor web server de web y aplicación por aplicación. Por lo tanto, este comportamiento predeterminado no funcionará para los dos escenarios siguientes:

- **Granja de servidores Web** : en un [granja de servidores web](http://en.wikipedia.org/wiki/Web_farm) escenario, una sola aplicación web se hospeda en varios servidores web para los fines de redundancia y escalabilidad. Cada solicitud entrante se envía a un servidor en la granja de servidores, lo que significa que a través de la duración de una sesión de usuario, servidores diferentes pueden utilizarse para controlar sus varias solicitudes. Por lo tanto, cada servidor debe utilizar las mismas claves de cifrado y validación para que cree el vale de autenticación de formularios, cifrado y validado en un servidor puede descifrar y validar en un servidor diferente en la granja de servidores.
- **Cross vale de uso compartido de aplicaciones** -un único servidor web puede hospedar varias aplicaciones de ASP.NET. Si necesita para que estas aplicaciones diferentes compartir un vale de autenticación de formularios único, es imperativo que coinciden con sus claves de cifrado y validación de.

Cuando se trabaja en una granja de servidores web, establecer o uso compartido de vales de autenticación en todas las aplicaciones en el mismo servidor, debe configurar el &lt;machineKey&gt; elemento en las aplicaciones afectadas para que sus decryptionKey y valores de validationKey coinciden.

Aunque ninguno de los escenarios anteriores se aplica a nuestra aplicación de ejemplo, todavía podemos especificar valores de validationKey y decryptionKey explícita y definir los algoritmos que se va a usar. Agregar un &lt;machineKey&gt; si se establece en el archivo Web.config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Para obtener más información visite [How To: configurar MachineKey en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Los valores decryptionKey y validationKey se toman de [Steve Gibson](http://www.grc.com/stevegibson.htm)del [perfecta contraseñas web página](https://www.grc.com/passwords.htm), que genera 64 caracteres hexadecimales aleatorios en cada visita de página. Para reducir la probabilidad de que estas claves que realiza la forma en las aplicaciones de producción, se recomienda para reemplazar las claves anteriores con generado aleatoriamente que desde la página de contraseñas perfecta.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Paso 4: Almacenar datos de usuario adicionales en el vale

Muchas aplicaciones web mostrar información sobre o basar la presentación de la página en el usuario ha iniciado sesión actualmente. Por ejemplo, una página web podría mostrar el nombre del usuario y la fecha de último que inició sesión en la esquina superior de cada página. El vale de autenticación de formularios almacena el nombre de usuario del usuario ha iniciado sesión actualmente, pero cuando se necesita ninguna otra información, la página debe ir a la tienda de usuario - normalmente una base de datos - para buscar la información que no se almacenan en el vale de autenticación.

Con un poco de código podemos almacenar información de usuario adicional en el vale de autenticación de formularios. Estos datos se puede expresar mediante la [clase FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)del [propiedad UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Se trata de un lugar útil para colocar pequeñas cantidades de información sobre el usuario que normalmente se necesita. El valor especificado en la propiedad se incluye como parte de la cookie de vale de autenticación y, al igual que los demás campos vale, se cifra y valida de UserData se basa en la configuración del sistema de autenticación de formularios. De forma predeterminada, UserData es una cadena vacía.

Con el fin de almacenar datos de usuario en el vale de autenticación, es necesario escribir un poco de código en la página de inicio de sesión que toma la información específica del usuario y lo almacena en el vale. Puesto que UserData es una propiedad de tipo cadena, los datos almacenados en ella deben serializarse correctamente como una cadena. Por ejemplo, imagine que el almacén de usuario incluida como fecha de nacimiento de cada usuario y el nombre de su empresa, y deseamos almacenar valores de estas dos propiedades en el vale de autenticación. Estos valores se pueden serializar en una cadena mediante la concatenación de fecha del usuario de cadena del nacimiento con una barra vertical (|), seguido del nombre de la empresa. Para un usuario nacido en el 15 de agosto de 1974 que funciona para Northwind Traders, se podría asignar la propiedad UserData la cadena: 1974-08-15 | Northwind Traders.

Cada vez que se necesita tener acceso a los datos almacenados en el vale, podemos hacer esto al arrastrar FormsAuthenticationTicket de la solicitud actual y deserializar la propiedad UserData. En el caso de la fecha de nacimiento y empleador ejemplo de nombre, se debería dividir la cadena de UserData en dos subcadenas según el delimitador (|).


[![Información adicional del usuario puede almacenarse en el vale de autenticación](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Figura 04**: adicionales usuario pueda almacenarse información en el vale de autenticación ([haga clic aquí para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Escritura de información en UserData

Por desgracia, no es tan sencillo como se podría esperar que agregar información específica del usuario a un vale de autenticación de formularios. La propiedad UserData de la clase FormsAuthenticationTicket es de solo lectura y solo se puede especificar a través del constructor de clase FormsAuthenticationTicket. Al especificar la propiedad UserData en el constructor, también es necesario proporcionar el vale 's otros valores: el nombre de usuario, la fecha de emisión, expiración y así sucesivamente. Cuando se crea la página de inicio de sesión en el tutorial anterior, esto se controla para que podamos por la clase FormsAuthentication. Al agregar UserData a FormsAuthenticationTicket, tendrá que escribir código para replicar gran parte de la funcionalidad ya proporcionada por la clase FormsAuthentication.

Vamos a examinar el código necesario para trabajar con datos del usuario mediante la actualización de la página Login.aspx para registrar información adicional sobre el usuario para el vale de autenticación. Supongamos que el almacén de usuario contiene información acerca de la empresa que trabaja el usuario y su título, y que desea capturar esta información en el vale de autenticación. Actualice el controlador de eventos de la página Login.aspx LoginButton haga clic en para que el código tiene un aspecto similar al siguiente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Vamos a recorrer este código de línea a la vez. El método inicia definiendo cuatro matrices de cadenas: los usuarios, contraseñas, companyName y titleAtCompany. Estos objetos Array contienen los nombres de usuario, contraseñas, nombres de las compañías y títulos de las cuentas de usuario en el sistema, de que existen tres: Scott, Jisun y Sam. En una aplicación real, estos valores se pueden consultar desde el almacén del usuario, no codificados de forma rígida en el código fuente de la página.

En el tutorial anterior, si las credenciales proporcionadas son válidas se denomina simplemente FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked), que realiza los pasos siguientes:

1. Crea los formularios de vale de autenticación
2. El vale se escribió en el almacén adecuado. Para los vales de autenticación basada en cookies, se usa la colección de cookies del explorador; para los vales de autenticación sin cookies, los datos del vale se serializan en la dirección URL
3. Redirige al usuario a la página adecuada

Estos pasos se replican en el código anterior. En primer lugar, la cadena que finalmente se almacenará en la propiedad UserData está formada por la combinación del nombre de la empresa y el título, delimita los dos valores con un carácter de barra vertical (|).

cadena userDataString = string. Concat (companyName [i], "|", titleAtCompany[i]);

A continuación, el FormsAuthentication.GetAuthCookie se invoca el método, que crea el vale de autenticación, cifra y valida según las opciones de configuración y lo coloca en un objeto HttpCookie.

HttpCookie authCookie = FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked);

Para trabajar con el FormAuthenticationTicket incrustado dentro de la cookie, es necesario llamar a la clase FormAuthentication [descifrar método](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), pasando el valor de cookie.

Vale FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value);

A continuación, creamos un *nueva* FormsAuthenticationTicket instancia según los valores del FormsAuthenticationTicket existente. Sin embargo, este vale nuevo incluye la información específica del usuario (userDataString).

FormsAuthenticationTicket newTicket = FormsAuthenticationTicket nueva (vale. Versión de vale. Nombre, vale. IssueDate, vale. Expiración de vale. IsPersistent, userDataString);

, A continuación, cifrar (y validar) la nueva instancia de FormsAuthenticationTicket mediante una llamada a la [cifrar método](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)y pone de nuevo estos datos cifrados (y validados) en authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Por último, authCookie se agrega a la colección Response.Cookies y se llama al método GetRedirectUrl para determinar la página correspondiente para enviar al usuario.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Todo este código es necesario porque la propiedad UserData es de solo lectura y la clase FormsAuthentication no proporciona ningún método para especificar la información de UserData en sus métodos GetAuthCookie, SetAuthCookie o RedirectFromLoginPage.

> [!NOTE]
> El código que se examina solo almacena información específica del usuario en un vale de autenticación basada en cookies. Las clases responsables de serializar el vale de autenticación de formularios a la dirección URL son internas para .NET Framework. Artículo largo corto, no se puede almacenar datos de usuario en un vale de autenticación de formularios sin cookies.


### <a name="accessing-the-userdata-information"></a>Acceso a la información de UserData

En este momento nombre de la compañía y el título de cada usuario se almacena en la propiedad UserData del vale de autenticación de formularios cuando inicie sesión. Esta información puede tener acceso desde el vale de autenticación en cualquier página sin necesidad de realizar un viaje al almacén del usuario. Para ilustrar cómo se puede recuperar esta información de la propiedad UserData, vamos a actualizar Default.aspx para que su mensaje de bienvenida incluye no solo el nombre del usuario, pero también la compañía que trabajan para y su título.

Actualmente, Default.aspx contiene un AuthenticatedMessagePanel Panel con un control de etiqueta denominado WelcomeBackMessage. Este Panel solo se muestra a los usuarios autenticados. Actualice el código en la página del Default.aspx\_cargar el controlador de eventos para que tenga un aspecto similar al siguiente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Si Request.IsAuthenticated es true, entonces la propiedad de texto del WelcomeBackMessage se establece primero en aquí está otra vez, *nombre de usuario*. A continuación, la propiedad User.Identity se convierte en un objeto FormsIdentity para que podamos acceder a FormsAuthenticationTicket subyacente. Una vez que tenemos FormsAuthenticationTicket, se deserializa la propiedad UserData en el nombre de la compañía y el título. Esto se consigue mediante la división de la cadena en el carácter de barra vertical. El nombre de la compañía y el título, a continuación, se muestran en la etiqueta WelcomeBackMessage.

Figura 5 se muestra una captura de pantalla de esta presentación en acción. Iniciar sesión como Scott muestra un mensaje de back-Bienvenido que incluye el título y la empresa de Scott.


[![Se muestran del actualmente usuario que inició sesión empresa y título](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Figura 05**: se muestran del actualmente usuario que inició sesión empresa y título ([haga clic aquí para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Propiedad de UserData del vale de autenticación actúa como una memoria caché para el almacén del usuario. Al igual que cualquier caché debe actualizarse cuando se modifican los datos subyacentes. Por ejemplo, si hay una página web desde el que los usuarios pueden actualizar su perfil, los campos que se almacena en caché en la propiedad UserData deben actualizarse para reflejar los cambios realizados por el usuario.


## <a name="step-5-using-a-custom-principal"></a>Paso 5: Uso de una entidad de seguridad personalizada

En cada solicitud entrante FormsAuthenticationModule intenta autenticar al usuario. Si hay un vale de autenticación no expiradas, FormsAuthenticationModule asigna la propiedad HttpContext.User a un nuevo objeto GenericPrincipal. Este objeto GenericPrincipal tiene una identidad de tipo FormsIdentity, que incluye una referencia para el vale de autenticación de formularios. La clase GenericPrincipal contiene la reconstrucción funcionalidad mínima necesaria para una clase que implementa IPrincipal: solo tiene una propiedad de identidad y a un método IsInRole.

El objeto principal tiene dos responsabilidades: para indicar cuáles son las funciones que pertenece el usuario y para proporcionar información de identidad. Esto se logra a través de IsInRole la interfaz de IPrincipal (*roleName*) método y propiedad de identidad, respectivamente. La clase GenericPrincipal permite una matriz de cadenas de nombres de función para especificarse a través de su constructor; su IsInRole (*roleName*) método simplemente comprueba si el valor en *roleName* existe dentro de la matriz de cadena. Cuando FormsAuthenticationModule crea el objeto GenericPrincipal, pasa de una matriz de cadena vacía al constructor de la clase GenericPrincipal. Por lo tanto, cualquier llamada a IsInRole siempre devolverá false.

La clase GenericPrincipal satisface las necesidades para escenarios de autenticación basada en formularios mayoría donde no se utilizan los roles. Para aquellas situaciones donde el tratamiento del rol predeterminado no es suficiente o si necesita asociar un objeto personalizado de la identidad del usuario, puede crear un objeto IPrincipal personalizado durante el flujo de trabajo de autenticación y asignarlo a la propiedad HttpContext.User.

> [!NOTE]
> Como se verá en el futuro tutoriales, cuando ASP. Está habilitada framework de Roles de .NET crea un objeto de entidad de seguridad personalizado del tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) y sobrescribe el objeto GenericPrincipal creado para la autenticación de formularios. Esto consigue con el fin de personalizar el método IsInRole de la entidad de seguridad para comunicarse con la API de las funciones del marco de trabajo.


Puesto que nos hemos no le preocupa nosotros mismos roles aún, sería la única razón que tenemos para crear una entidad de seguridad personalizada en ese momento asociar un objeto personalizado de la identidad a la entidad de seguridad. En el paso 4 analizamos almacenar información de usuario adicional en la propiedad UserData del vale de autenticación, en particular, nombre de la compañía del usuario y su título. Sin embargo, la información de UserData solo es accesible a través del vale de autenticación y, a continuación, solo como una cadena serializada, lo que significa que cada vez que desea ver la información de usuario almacenada en el vale es necesario analizar la propiedad UserData.

Podemos mejorar la experiencia del desarrollador mediante la creación de una clase que implementa IIdentity e incluye propiedades CompanyName y título. De este modo, un desarrollador puede tener acceso a nombre de la compañía del usuario ha iniciado sesión actualmente y título directamente a través de las propiedades CompanyName y título sin que sea necesario saber cómo analizar la propiedad UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Crear las clases Principal e identidad personalizada

Para este tutorial, vamos a crear los objetos principal e identity personalizados en la aplicación\_carpeta de código. Empiece por agregar una aplicación\_carpeta al proyecto de código: haga doble clic en el nombre del proyecto en el Explorador de soluciones, seleccione la opción de Agregar carpeta ASP.NET y elija aplicación\_código. La aplicación\_carpeta de código es una carpeta especial de ASP.NET que contiene archivos de clase específico en el sitio Web.

> [!NOTE]
> La aplicación\_solo se debe utilizar la carpeta de código al administrar el proyecto a través del modelo de proyecto de sitio Web. Si usas el [el modelo de proyecto de aplicación Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), cree una carpeta estándar y agregue las clases a la. Por ejemplo, podría agregar una nueva carpeta denominada clases y coloque su código no existe.


A continuación, agregue dos nuevos archivos de clase a la aplicación\_carpeta de código, un CustomIdentity.cs con nombre y otro denominado archivo CustomPrincipal.cs.


[![Agregar las clases de CustomPrincipal y CustomIdentity al proyecto](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Figura 06**: agregar las clases de CustomPrincipal y CustomIdentity a un proyecto ([haga clic aquí para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


La clase CustomIdentity es responsable de implementar la interfaz IIdentity, que define las propiedades Name, IsAuthenticated y AuthenticationType. Además de las propiedades necesarias, nos interesa exponer los subyacente vale de autenticación de formularios así como propiedades de nombre de la compañía y el título del usuario. Escriba el código siguiente en la clase CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Tenga en cuenta que la clase incluye una variable de miembro FormsAuthenticationTicket (\_vale) y que se debe proporcionar esta información de los vales a través del constructor. Estos datos de vale se utilizan para devolver el nombre de la identidad; la propiedad UserData se analiza para devolver los valores de las propiedades CompanyName y título.

A continuación, cree la clase CustomPrincipal. Puesto que nos no interesan con los roles en ese momento, el constructor de la clase CustomPrincipal acepta solo un objeto de CustomIdentity; su método IsInRole siempre devuelve false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Asignar un objeto CustomPrincipal al contexto de seguridad de la solicitud entrante

Ahora tenemos una clase que extiende la especificación de la identidad predeterminada para incluir propiedades CompanyName y el título, así como una clase de entidad de seguridad personalizada que usa la identidad personalizada. Se está listos para ir a la canalización ASP.NET y asigna el objeto principal personalizado al contexto de seguridad de la solicitud entrante.

La canalización ASP.NET toma una solicitud entrante y lo procesa mediante una serie de pasos. En cada paso, se genera un evento concreto, lo que permite a los desarrolladores aprovechar la canalización ASP.NET y modificar la solicitud en ciertos puntos del ciclo de vida. FormsAuthenticationModule, por ejemplo, se espera para que ASP.NET generar el [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), momento en que inspecciona la solicitud entrante de un vale de autenticación. Si se encuentra un vale de autenticación, un objeto GenericPrincipal se crea y se asigna a la propiedad HttpContext.User.

Después del evento AuthenticateRequest, la canalización ASP.NET genera el [PostAuthenticateRequest eventos](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que es donde podemos reemplazar el objeto GenericPrincipal creado por FormsAuthenticationModule con una instancia de nuestro Objeto CustomPrincipal. Figura 7 muestra este flujo de trabajo.


[![El objeto GenericPrincipal se sustituye por un CustomPrincipal en el evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Figura 07**: The GenericPrincipal se sustituye por un CustomPrincipal en el evento PostAuthenticationRequest ([haga clic aquí para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Para ejecutar código en respuesta a un evento de la canalización ASP.NET, se podemos crear el controlador de eventos apropiado en Global.asax o para crear nuestra propia módulo HTTP. Para este tutorial vamos a crear el controlador de eventos en Global.asax. Empiece por agregar Global.asax a su sitio Web. Haga doble clic en el nombre del proyecto en el Explorador de soluciones y agregue un elemento de tipo de clase de aplicación Global denominado Global.asax.


[![Agregar un archivo Global.asax al sitio Web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Figura 08**: agregar un archivo Global.asax a su sitio Web ([haga clic aquí para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


La plantilla predeterminada de Global.asax incluye controladores de eventos para un número de los eventos de la canalización ASP.NET, incluido el inicio, fin y [evento de Error](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), entre otros. No dude en quitar estos controladores de eventos, no es imprescindible para esta aplicación. El evento que nos interesa es PostAuthenticateRequest. Actualice el archivo Global.asax para que su marcado tiene un aspecto similar al siguiente:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

La aplicación\_OnPostAuthenticateRequest método ejecuta cada vez que el tiempo de ejecución ASP.NET genera el evento PostAuthenticateRequest, lo que sucede una vez en cada solicitud de página entrante. El controlador de eventos se inicia y comprueba si el usuario se autentica y se autenticó mediante la autenticación de formularios. Si es así, se crea un nuevo objeto de CustomIdentity y se pasa el vale de autenticación de la solicitud actual en su constructor. A continuación, se crea un objeto CustomPrincipal y se pasa el objeto de CustomIdentity recién creada en su constructor. Por último, contexto de seguridad de la solicitud actual se asigna al objeto CustomPrincipal recién creado.

Tenga en cuenta que el último paso: asociar el objeto CustomPrincipal con el contexto de seguridad de la solicitud - asigna la entidad de seguridad a las dos propiedades: HttpContext.User y Thread.CurrentPrincipal. Estas dos asignaciones son necesarias debido al modo en que se administran los contextos de seguridad en ASP.NET. .NET Framework asocia un contexto de seguridad de cada subproceso en ejecución; Esta información está disponible como un objeto IPrincipal a través de la [objeto de subproceso](https://msdn.microsoft.com/library/system.threading.thread.aspx)del [propiedad CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). ¿Qué es un confuso es que ASP.NET tiene su propia información de contexto de seguridad (HttpContext.User).

En determinados escenarios, se examina la propiedad Thread.CurrentPrincipal al determinar el contexto de seguridad; en otros escenarios, se usa HttpContext.User. Por ejemplo, existen características de seguridad en .NET que permiten a los desarrolladores mediante declaración indicar lo que los usuarios o roles pueden crear una instancia de una clase o invocar métodos específicos (vea [agregar reglas de autorización para el negocio y el uso de capas de datos PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Interiormente, estas técnicas declarativas determinan el contexto de seguridad a través de la propiedad Thread.CurrentPrincipal.

En otros escenarios, se utiliza la propiedad HttpContext.User. Por ejemplo, en el tutorial anterior, hemos usado esta propiedad para mostrar el nombre de usuario del usuario con sesión iniciada actualmente. Claramente, a continuación, es imprescindible que la información de contexto de seguridad en las propiedades Thread.CurrentPrincipal y HttpContext.User coincidan.

El tiempo de ejecución ASP.NET sincroniza automáticamente estos valores de propiedad para que podamos. Sin embargo, la sincronización se produce después del evento AuthenticateRequest, pero *antes de* el evento PostAuthenticateRequest. Por lo tanto, al agregar una entidad de seguridad personalizada en el evento PostAuthenticateRequest que necesitamos estar seguro de que los asigne manualmente Thread.CurrentPrincipal o bien Thread.CurrentPrincipal y HttpContext.User estarán sincronizados. Vea [Context.User vs. Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) para obtener una explicación más detallada sobre este problema.

### <a name="accessing-the-companyname-and-title-properties"></a>Obtener acceso a las propiedades de título y CompanyName

Cada vez que una solicitud llega y se envía al motor de ASP.NET, la aplicación\_se activará el controlador de eventos OnPostAuthenticateRequest en Global.asax. Si la solicitud se ha autenticado correctamente por FormsAuthenticationModule, el controlador de eventos creará un nuevo objeto CustomPrincipal con un objeto CustomIdentity basado en el vale de autenticación de formularios. Con esta lógica en su lugar, el acceso a la información sobre el nombre de la empresa el usuario ha iniciado sesión actualmente y el título es increíblemente sencillo.

Volver a la página\_controlador de eventos de carga en Default.aspx, donde en el paso 4 se escribió el código para recuperar el vale de autenticación de formularios y analizar la propiedad UserData para mostrar el nombre de la compañía y el título del usuario. Con los objetos CustomPrincipal y CustomIdentity en uso ahora, no hay ninguna necesidad para analizar los valores de propiedad de UserData del vale. En su lugar, sólo tiene que obtener una referencia al objeto CustomIdentity y usar sus propiedades CompanyName y título:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Resumen

En este tutorial se examina cómo personalizar la configuración del sistema de autenticación de formularios a través de Web.config. Hemos observado cómo se controla la caducidad del vale de autenticación y cómo se utilizan las medidas de seguridad de cifrado y validación para proteger el vale de inspección y modificación. Por último, analizamos utilizando la propiedad UserData del vale de autenticación para almacenar información de usuario adicionales en el propio vale y cómo usar objetos principal e identity personalizados para exponer esta información de un modo más sencillo de para los desarrolladores.

Este tutorial concluye el examen de autenticación de formularios en ASP.NET. El tutorial siguiente inicia el viaje en el marco de trabajo de pertenencia.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Si examinamos la autenticación de formularios](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Explica: Autenticación de formularios en ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Cómo: Proteger la autenticación de formularios en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 seguridad, la pertenencia y la administración de roles](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Proteger los controles de inicio de sesión](https://msdn.microsoft.com/library/ms178346.aspx)
- [El &lt;autenticación&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [El &lt;formularios&gt; (elemento) para &lt;autenticación&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [El &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Comprensión del vale de autenticación de formularios y la Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Aprendizaje mediante vídeo sobre los temas incluidos en este Tutorial

- [Cómo cambiar las propiedades de autenticación de formularios](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Cómo el programa de instalación y utilice autenticación sin cookies en una aplicación ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Reubicación del inicio de sesión de los formularios ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configuración de claves personalizadas de inicio de sesión de los formularios](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Agregar datos personalizados al método de autenticación](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usar objetos de entidad de seguridad personalizados](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Alicja Maziarz. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](an-overview-of-forms-authentication-cs.md)
> [Siguiente](security-basics-and-asp-net-support-vb.md)
