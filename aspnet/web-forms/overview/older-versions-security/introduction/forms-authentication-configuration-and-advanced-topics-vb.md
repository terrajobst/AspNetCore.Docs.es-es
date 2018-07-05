---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: Configuración de la autenticación formularios y temas avanzados (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial examinaremos las distintas configuraciones de autenticación de formularios y vea cómo modificarlos mediante el elemento de formularios. Esto supondrá una detallada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: 493cb81271ea1c0439f7b499c5b48e659d3589b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390866"
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>Configuración de autenticación de formularios y temas avanzados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip) o [descargar PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> En este tutorial examinaremos las distintas configuraciones de autenticación de formularios y vea cómo modificarlos mediante el elemento de formularios. Esto supondrá una visión detallada de personalizar el valor de tiempo de espera del vale de autenticación de formularios, con una página de inicio de sesión con una dirección URL personalizada (por ejemplo, SignIn.aspx en lugar de Login.aspx) y vales de autenticación de formularios sin cookies.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](an-overview-of-forms-authentication-vb.md) analizamos los pasos necesarios para implementar la autenticación de formularios en una aplicación ASP.NET, especificar la configuración de Web.config a la creación de un registro en la página para mostrar diferentes contenido para usuarios autenticados y anónimos. Recuerde que hemos configurado el sitio Web para usar autenticación de formularios estableciendo el atributo de modo de la &lt;autenticación&gt; elemento a los formularios. El &lt;autenticación&gt; elemento puede incluir opcionalmente un &lt;formularios&gt; elemento secundario, a través del cual se puede especificar una gran variedad de opciones de autenticación de formularios.

En este tutorial examinaremos las distintas configuraciones de autenticación de formularios y vea cómo modificarlos a través de la &lt;formularios&gt; elemento. Esto supondrá una visión detallada de personalizar el valor de tiempo de espera del vale de autenticación de formularios, con una página de inicio de sesión con una dirección URL personalizada (por ejemplo, SignIn.aspx en lugar de Login.aspx) y vales de autenticación de formularios sin cookies. También examinaremos más detenidamente la composición del vale de autenticación de formularios y vea las precauciones que ASP.NET toma para asegurarse de que los datos del vale están seguros contra inspección y manipulación. Por último, veremos cómo almacenar datos de usuario adicional en el vale de autenticación de formularios y cómo modelar estos datos a través de un objeto principal personalizado.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Paso 1: Examinar el &lt;formularios&gt; opciones de configuración

El sistema de autenticación de formularios de ASP.NET ofrece una serie de opciones de configuración que se pueden personalizar en la aplicación por aplicación. Esto incluye opciones como: la duración de la autenticación de formularios de vale; ¿Qué tipo de protección se aplica al vale; en la autenticación sin cookies qué condiciones se usan vales; la ruta de acceso a la página de inicio de sesión; y otra información. Para modificar los valores predeterminados, agregue un [ &lt;formularios&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) como elemento secundario de la [ &lt;autenticación&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx), especificando la propiedad de esos los valores que desee personalizar como atributos XML la siguiente manera:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

La tabla 1 resume las propiedades que se pueden personalizar mediante la &lt;formularios&gt; elemento. Puesto que el archivo Web.config es un archivo XML, los nombres de atributo en la columna izquierda distinguen mayúsculas de minúsculas.


| <strong>Attribute</strong> |                                                                                                                                                                                                                                     <strong>Descripción</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         sin cookies         |                                                                                                                Este atributo especifica bajo qué condiciones el vale de autenticación se almacena en una cookie en comparación con la que se incrusta en la dirección URL. Valores permitidos son: UseCookies; UseUri; Detección automática; y UseDeviceProfile (predeterminado). Paso 2 examina esta configuración con más detalle.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indica la dirección URL que se redirigen a los usuarios después de iniciar sesión en la página de inicio de sesión si no hay ningún valor de URL de redireccionamiento especificado en la cadena de consulta. El valor predeterminado es default.aspx.                                                                                                                                                         |
|           dominio           | Cuando se utiliza vales de autenticación basada en cookies, esta configuración especifica el valor de dominio de la cookie s. El valor predeterminado es una cadena vacía, lo que hace que el explorador usar el dominio desde el que se emitió (por ejemplo, www.yourdomain.com). En este caso, la cookie le <strong>no</strong> enviarse al realizar solicitudes a subdominios, como admin.yourdomain.com. Si desea que la cookie que se pasará a todos los subdominios que necesita para personalizar el atributo de dominio si se establece en sudominio.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Un valor booleano que indica si los usuarios autenticados se memorizan al redirigir a direcciones URL de otras aplicaciones web en el mismo servidor. El valor predeterminado es false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      La dirección URL de la página de inicio de sesión. El valor predeterminado es login.aspx.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Cuando se usan vales de autenticación basada en cookies, el nombre de la cookie. El valor predeterminado es. ASPXAUTH.                                                                                                                                                                                                   |
|            ruta de acceso            |                                                                             Cuando se utiliza vales de autenticación basada en cookies, esta configuración especifica el atributo de ruta de acceso de la cookie s. El atributo de ruta de acceso permite que un desarrollador limitar el ámbito de una cookie para una jerarquía de directorios concreta. El valor predeterminado es /, que indica al explorador para enviar la cookie de vale de autenticación a cualquier solicitud realizada al dominio.                                                                              |
|         protección         |                                                                                                                                            Indica las técnicas que se usan para proteger el vale de autenticación de formularios. Los valores permitidos son: todo (predeterminado); Cifrado; Ninguno; y la validación. Estas opciones se describen en detalle en el paso 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Un valor booleano que indica si se requiere una conexión SSL para transmitir la cookie de autenticación. El valor predeterminado es false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Un valor booleano que indica que si el tiempo de espera de s de la cookie de autenticación se restablece cada vez que el usuario visita el sitio durante una sola sesión. El valor predeterminado es true. La directiva de tiempo de espera de vale de autenticación se describe con más detalle en la especificación el s sección con el valor de tiempo de espera de vale.                                                                                                 |
|          tiempo de espera           |                                                                                                                               Especifica el tiempo, en minutos, tras el cual expira la cookie del vale de autenticación. El valor predeterminado es 30. La directiva de tiempo de espera de vale de autenticación se describe con más detalle en la especificación el s sección con el valor de tiempo de espera de vale.                                                                                                                               |

**Tabla 1**: un resumen de la &lt;formularios&gt; atributos del elemento

En ASP.NET 2.0 y versiones posteriores, el valor predeterminado los valores de autenticación de formularios están codificados en la clase FormsAuthenticationConfiguration en .NET Framework. Cada aplicación por aplicación en el archivo Web.config se deben aplicar las modificaciones. Esto difiere de ASP.NET 1.x, donde los valores de autenticación de formularios predeterminados se almacenaron en el archivo machine.config (y, por tanto, podrían modificarse mediante la edición de machine.config). En el tema de ASP.NET 1.x, merece la pena mencionar que un número de la configuración del sistema de autenticación de formularios tiene valores predeterminados diferentes en ASP.NET 2.0 y más allá de lo que en ASP.NET 1.x. Si la aplicación va a migrar desde un entorno de ASP.NET 1.x, es importante tener en cuenta estas diferencias. Consulte [el &lt;formularios&gt; documentación técnica de elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) para obtener una lista de las diferencias.

> [!NOTE]
> Varias opciones de autenticación de formularios, como el tiempo de espera, el dominio y la ruta de acceso, especifican los detalles de la cookie de vale de autenticación de formularios resultante. Para obtener más información sobre las cookies, cómo funcionan y sus diversas propiedades, lea [este tutorial Cookies](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Especifica el valor de tiempo de espera del vale

El vale de autenticación de formularios es un token que representa una identidad. Con los vales de autenticación basada en cookies, este token se mantiene en forma de una cookie y se envían al servidor web en cada solicitud. Posesión del token, en esencia, se declara, pues soy *username*, ya ha iniciado sesión y se usa para que la identidad de un usuario se puede guardar todas las visitas de la página.

El vale de autenticación de formularios no sólo incluye la identidad del usuario, sino que también contiene información para ayudar a garantizar la integridad y seguridad del token. Después de todo, no queremos un usuario para poder crear un token falsificado, o para modificar un token que de alguna manera underhanded fraudulento.

Un bit de este tipo de información incluida en el vale es un *expiración*, que es la fecha y hora ya no es válido el vale. Cada vez que FormsAuthenticationModule inspecciona un vale de autenticación, se asegura de que expire del vale todavía no ha pasado. Si es así, no tiene en cuenta el vale e identifica el usuario es anónimo. Esta medida de seguridad ayuda a protegerse contra los ataques de reproducción. Sin una fecha de expiración, si un hacker era capaz de obtener sus manos en el vale de autenticación válido de un usuario - quizás por obtener acceso físico a su equipo y la raíz a través de sus cookies, podría enviar una solicitud al servidor con este vale de autenticación robada y Obtenga la entrada. Aunque la expiración no impide que este escenario, limita la ventana durante el cual puede tener éxito este tipo de ataque.

> [!NOTE]
> Paso 3 detalles adicionales técnicas usadas por el sistema de autenticación de formularios para proteger el vale de autenticación.


Al crear el vale de autenticación, el sistema de autenticación de formularios determina su expiración consultando la configuración de tiempo de espera. Como se indicó en la tabla 1, el tiempo de espera de establecer los valores predeterminados en 30 minutos, lo que significa que cuando se crea el vale de autenticación de formularios su expiración se establece en una fecha y hora, 30 minutos en el futuro.

La expiración define un tiempo absoluto en el futuro cuando expira el vale de autenticación de formularios. Pero normalmente los desarrolladores desean implementar una expiración deslizante, que se restablece cada vez que el usuario vuelve a visitar el sitio. Este comportamiento viene determinada por la configuración de slidingExpiration. Si establece en true (valor predeterminado), cada vez FormsAuthenticationModule autentica un usuario, actualiza expiración del vale. Si se establece en false, la expiración no se actualiza en cada solicitud, lo que provoca el vale para expirar exactamente el tiempo de espera número de minutos transcurridos tras cuando el vale fue el primero creado.

> [!NOTE]
> La expiración que se almacenan en el vale de autenticación es una fecha absoluta y el valor de tiempo, como el 2 de agosto de 2008 11:34 AM. Además, la fecha y hora son con respecto a la hora de local del servidor web. Esta decisión de diseño puede tener algunos efectos interesantes alrededor del horario de verano (DST), que es cuando los relojes de los Estados Unidos se adelanta una hora (suponiendo que el servidor web se hospeda en una configuración regional donde se observa el horario de verano). Tenga en cuenta lo que sucedería para un sitio Web ASP.NET con una expiración de 30 minutos cerca del momento en que comienza el horario de verano (que es a las 2:00 A.M.). Imagine que un visitante inicia sesión en el sitio del 11 de marzo de 2008 a las 1:55 AM. Esto generaría un vale de autenticación de formularios que expira a las 11 de marzo de 2008 a 2:25 a. M. (30 minutos en el futuro). Sin embargo, una vez RTM 2:00 A.M., el reloj salta a 3:00 A.M. a causa de horario de verano. Cuando el usuario cargue una nueva página de seis minutos después de iniciar sesión (a las 3:01 AM), FormsAuthenticationModule observa que el vale ha caducado y redirige al usuario a la página de inicio de sesión. Para obtener una explicación más completa sobre esto y otras rarezas de tiempo de espera de vale de autenticación, así como soluciones alternativas, adquiera un ejemplar de Stefan Schackow *Professional ASP.NET 2.0 seguridad pertenencia y administración de roles* (ISBN: 978-0-7645-9698-8).


Figura 1 ilustra el flujo de trabajo cuando slidingExpiration se establece en false y el tiempo de espera se establece en 30. Tenga en cuenta que el vale de autenticación que se generan en el inicio de sesión contiene la fecha de expiración, y este valor no se actualiza en las solicitudes subsiguientes. Si FormsAuthenticationModule detecta que el vale ha caducado, se descarta y trata la solicitud como anónimo.


[![Una representación gráfica de slidingExpiration de expiración cuando del vale autenticación de formularios es false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**Figura 01**: una representación gráfica de slidingExpiration de expiración cuando del vale autenticación de formularios es false ([haga clic aquí para ver imagen en tamaño completo](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


La figura 2 muestra el flujo de trabajo cuando slidingExpiration se establece en true y el tiempo de espera se establece en 30. Cuando se recibe una solicitud autenticada (con un vale no hayan caducado) se actualiza su expiración en tiempo de espera número de minutos en el futuro.


[![Una representación gráfica del vale de autenticación de formularios cuando slidingExpiration es true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**Figura 02**: una representación gráfica del vale de autenticación de formularios cuando es true slidingExpiration ([haga clic aquí para ver imagen en tamaño completo](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


Cuando se utiliza vales de autenticación basada en cookies (el valor predeterminado), esta explicación se convierte en un poco más confusa porque las cookies también pueden tener sus propios expirados especificados. Expiración de una cookie (o falta de preparación) indica al explorador cuando se debe destruir la cookie. Si la cookie no tiene una fecha de expiración, se destruye cuando se cierra el explorador. Si una fecha de expiración está presente, sin embargo, la cookie permanece almacenada en el equipo del usuario hasta la fecha y ha pasado el tiempo especificado en la expiración. Cuando se destruye una cookie del explorador, ya no se envía al servidor web. Por lo tanto, la destrucción de una cookie es análoga al usuario cerrar la sesión en el sitio.

> [!NOTE]
> Por supuesto, un usuario puede quitar proactivamente las cookies almacenadas en su equipo. En Internet Explorer 7, podría ir a herramientas, opciones y haga clic en el botón Eliminar en la sección de historial de exploración. Desde allí, haga clic en el botón de eliminar las cookies.


El sistema de autenticación de formularios crea cookies de sesión o expiración dependiendo del valor pasado a la *persistCookie* parámetro. Recuerde que los métodos de la clase FormsAuthentication GetAuthCookie SetAuthCookie y RedirectFromLoginPage toman dos parámetros de entrada: *username* y *persistCookie*. La página de inicio de sesión que se creó en el tutorial anterior incluye una casilla Recordar mi cuenta, que determina si se creó una cookie persistente. Las cookies persistentes son basado en expiración; las cookies no persistentes son basados en sesión.

Los conceptos de tiempo de espera y slidingExpiration que se discutió aplican el mismo para ambos cookies basadas en sesión y expiración. Hay solo una pequeña diferencia entre en ejecución: al usar cookies de expiración con slidingTimeout establecido en true, expiración de la cookie solo se actualiza cuando más que ha transcurrido la mitad del tiempo especificado.

Vamos a actualizar directivas de tiempo de espera de vale de autenticación de nuestro sitio Web, por lo vales de tiempo de espera después de una hora (60 minutos), con una expiración variable. Para efectuar este cambio, actualice el archivo Web.config, agregar un &lt;formularios&gt; elemento a la &lt;autenticación&gt; elemento con el siguiente marcado:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Uso de una dirección URL de página de inicio de sesión que no sean Login.aspx

Puesto que FormsAuthenticationModule redirige automáticamente los usuarios no autorizados a la página de inicio de sesión, debe saber la dirección URL de la página de inicio de sesión. Esta dirección URL especificada por el atributo loginUrl en el &lt;formularios&gt; elemento y el valor predeterminado es login.aspx. Si va a trasladar a través de un sitio Web existente, es posible que ya tiene una página de inicio de sesión con una dirección URL diferente, uno que ya se ha colocado un marcador e indizado por los motores de búsqueda. En lugar de cambiar el nombre de la página de inicio de sesión existente a login.aspx e interrupción de vínculos y los marcadores de los usuarios, en su lugar, puede modificar el atributo loginUrl para que apunte a la página de inicio de sesión.

Por ejemplo, si la página de inicio de sesión se denominaba SignIn.aspx y se encuentra en el directorio de usuarios, podría señalar el ajuste de configuración loginUrl ~/Users/SignIn.aspx así:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

Dado que nuestra aplicación actual ya tiene una página de inicio de sesión llamada Login.aspx, no es necesario especificar un valor personalizado en el &lt;formularios&gt; elemento.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Paso 2: Uso de vales de autenticación de formularios sin cookies

De forma predeterminada el sistema de autenticación de formularios determina si se almacenan sus vales de autenticación en la colección de cookies o incrustarlos en la dirección URL basada en el agente de usuario visita el sitio. Todos los exploradores de escritorio convencionales como Internet Explorer, Firefox, Opera y Safari, las cookies de soporte técnico, pero no todos los dispositivos móviles.

La directiva de cookie utilizada por el sistema de autenticación de formularios depende de la configuración sin cookies en el &lt;formularios&gt; elemento, que se puede asignar uno de cuatro valores:

- UseCookies: Especifica que siempre se usará los vales de autenticación basada en cookies.
- UseUri - indica que nunca se usará los vales de autenticación basada en cookies.
- No se utiliza la detección automática: si el perfil de dispositivo no es compatible con las cookies, vales de autenticación basada en cookies; Si el perfil de dispositivo admite cookies, se utiliza un mecanismo de sondeo para determinar si las cookies están habilitadas.
- UseDeviceProfile - el valor predeterminado; vales de autenticación basada en cookies se utiliza sólo si el perfil de dispositivo admite cookies. No se usa ningún mecanismo de sondeo.

La configuración de detección automática y UseDeviceProfile se basan en un *perfil de dispositivo* en determinar si se debe usar los vales de autenticación sin cookies o basada en cookies. ASP.NET mantiene una base de datos de varios dispositivos y sus capacidades, por ejemplo, si son compatibles con las cookies, ¿qué versión de JavaScript que admiten y así sucesivamente. Cada vez que un dispositivo solicita una página web desde un servidor web envía a lo largo de un *usuario-agente* encabezado HTTP que identifica el tipo de dispositivo. ASP.NET hace coincidir automáticamente la cadena user-agent proporcionado con el correspondiente perfil especificado en su base de datos.

> [!NOTE]
> Esta base de datos de las capacidades del dispositivo se almacena en un número de archivos XML que cumplen el [esquema de archivo de definición de explorador](https://msdn.microsoft.com/library/ms228122.aspx). Los archivos de perfil de dispositivo de forma predeterminada se encuentran en % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. También puede agregar archivos personalizados a la aplicación de la aplicación\_carpeta exploradores. Para obtener más información, consulte [How To: detectar tipos de explorador en ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Dado que el valor predeterminado es UseDeviceProfile, vales de autenticación de formularios sin cookies se usará cuando se visita el sitio de un dispositivo cuyo perfil notifica que no es compatible con las cookies.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>El vale de autenticación en la dirección URL de codificación

Las cookies son un medio natural para incluir información desde el explorador en cada solicitud a un determinado sitio Web, que es la razón por la configuración de autenticación de formularios de forma predeterminada usa cookies si el dispositivo visitando es compatible con ellos. Si no se admiten cookies, es indispensable utilizar medios alternativos para pasar el vale de autenticación del cliente al servidor. Es una solución común usada en entornos sin cookies codificar los datos de la cookie en la dirección URL.

Es la mejor forma de ver cómo se puede incrustar esa información en la dirección URL forzar que el sitio utilice vales de autenticación sin cookies. Esto puede realizarse estableciendo el valor de configuración sin cookies en UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

Una vez que haya realizado este cambio, visite el sitio mediante un explorador. Cuando se visita como usuario anónimo, las direcciones URL será exactamente como lo hacían antes. Por ejemplo, cuando se visita la página Default.aspx barra de direcciones de mi explorador muestra la dirección URL siguiente:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Sin embargo, al iniciar sesión en, el vale de autenticación de formularios se incrusta en la dirección URL. Por ejemplo, después de visitar la página de inicio de sesión e iniciar sesión como Sam, estoy volverá a la página Default.aspx, pero esta vez de la dirección URL es:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

El vale de autenticación de formularios se ha incrustado dentro de la dirección URL. La cadena (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) representa la información del vale de autenticación con codificación hexadecimal, y son los mismos datos que normalmente se almacenan en una cookie.

Para los vales de autenticación sin cookies para que funcione, el sistema debe codificar todas las direcciones URL en la página para incluir los datos del vale de autenticación, en caso contrario, se perderá el vale de autenticación cuando el usuario hace clic en un vínculo. Afortunadamente, esta lógica de inserción se realiza automáticamente. Para demostrar esta funcionalidad, abra la página Default.aspx y agregue un control de hipervínculo, establecer sus propiedades NavigateUrl y de texto para probar vínculo y SomePage.aspx, respectivamente. No importa que realmente no hay una página en el proyecto denominada SomePage.aspx.

Guardar los cambios a Default.aspx y, a continuación, se visita a través de un explorador. Inicie sesión en el sitio para que el vale de autenticación de formularios se incrusta en la dirección URL. A continuación, en Default.aspx, haga clic en el vínculo Probar vínculo. ¿Qué ocurre? Si no existe ninguna página denominada SomePage.aspx, a continuación, se ha producido un error 404, pero no es lo que es importante aquí. En su lugar, se centran en la barra de direcciones del explorador. Tenga en cuenta que incluye el vale de autenticación de formularios en la dirección URL.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

La dirección URL SomePage.aspx en el vínculo se convierte automáticamente en una dirección URL que incluye el vale de autenticación: no hemos tenido que escribir ni una pizca de código! El vale de autenticación de formulario se incrustan automáticamente en la dirección URL de los hipervínculos que no empiecen por http:// o /. No importa si el hipervínculo que aparece en una llamada para Response.Redirect, en un control de hipervínculo o en un elemento delimitador HTML (es decir, &lt;un href = "..."&gt;... &lt;/a&gt;). Siempre que la dirección URL no es algo parecido a http://www.someserver.com/SomePage.aspx o /SomePage.aspx, los formularios se incrustará el vale de autenticación para nosotros.

> [!NOTE]
> Vales de autenticación de formularios sin cookies cumplen las mismas directivas de tiempo de espera que los vales de autenticación basada en cookies. Sin embargo, los vales de autenticación sin cookies son más propensas a ataques de reproducción, puesto que el vale de autenticación está incrustado directamente en la dirección URL. Imagine un usuario que visita un sitio Web, inicia sesión y, a continuación, se pega la dirección URL en un correo electrónico a un compañero de trabajo. Si su compañero hace clic en ese vínculo antes de alcanza la expiración, iniciará sesión en que el usuario que envió el correo electrónico.


## <a name="step-3-securing-the-authentication-ticket"></a>Paso 3: Proteger el vale de autenticación

El vale de autenticación de formularios se transmite a través de la conexión en una cookie o incrustados directamente en la dirección URL. Además de información de identidad, el vale de autenticación también puede incluir datos de usuario (como veremos en el paso 4). Por lo tanto, es importante que se cifren los datos del vale de ojos espía y (incluso más importante) que el sistema de autenticación de formularios puede garantizar que no se ha alterado el vale.

Para garantizar la privacidad de los datos del vale, el sistema de autenticación de formularios puede cifrar los datos del vale. Error al cifrar los datos del vale envía información potencialmente confidencial a través de la red en texto sin formato.

Para garantizar la autenticidad de un vale, el sistema de autenticación de formularios debe *validar* el vale. La validación es el acto de garantizar que no se ha modificado un elemento de datos determinado y se logra a través de un  *[de mensajes (MAC) del código de autenticación](http://en.wikipedia.org/wiki/Message_authentication_code)*. En pocas palabras, el equipo MAC es un pequeño fragmento de información que identifica los datos que necesita ser validada (en este caso, el vale). Si se modifican los datos representados por el equipo MAC, a continuación, el equipo MAC y los datos no coincidirá con. Además, es difícil computacionalmente para un pirata informático tanto modificar los datos y generar su propio equipo MAC para que se correspondan con los datos modificados.

Al crear (o modificar) una incidencia, el sistema de autenticación de formularios crea un equipo MAC y lo adjunta a los datos del vale. Cuando llega una solicitud posterior, el sistema de autenticación de formularios compara los datos de MAC y el vale para validar la autenticidad de los datos del vale. Figura 3 ilustra gráficamente este flujo de trabajo.


[![Se garantiza la autenticidad del vale a través de un equipo MAC](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**Figura 03**: se garantiza la autenticidad de la incidencia a través de un equipo MAC ([haga clic aquí para ver imagen en tamaño completo](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


¿Qué medidas de seguridad se aplican al vale de autenticación depende de la configuración de protección en el &lt;formularios&gt; elemento. La configuración de protección puede asignarse a uno de los tres valores siguientes:

- Todo: el vale está cifrado y firmado digitalmente (el valor predeterminado).
- Cifrado: cifrado solo se aplica: no se genera ningún equipo MAC.
- : El vale se cifran ni firmado digitalmente.
- Se genera la validación: un equipo MAC, pero los datos del vale se envían a través de la conexión en texto sin formato.

Microsoft recomienda utilizar la configuración de todos.

### <a name="setting-the-validation-and-decryption-keys"></a>Establecimiento de la validación y las claves de descifrado

El cifrado y hash algoritmos usados por el sistema de autenticación de formularios para cifrar y validar el vale de autenticación son personalizables a través de la [ &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx) en el archivo Web.config. Tabla 2 contornos el &lt;machineKey&gt; atributos del elemento y sus valores posibles.

| **Attribute** | **Descripción** |
| --- | --- |
| descifrado | Indica el algoritmo utilizado para el cifrado. Este atributo puede tener uno de los siguientes cuatro valores: - Auto - el valor predeterminado; Determina el algoritmo que se basa en la longitud del atributo decryptionKey. -AES - utiliza el [estándar de cifrado avanzado (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmo. -DES - utiliza el [estándar de cifrado de datos (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) este algoritmo se considera computacionalmente débil y no debe usarse. -3DES - utiliza el [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmo, que aplica el algoritmo DES triple. |
| decryptionKey | La clave secreta usada por el algoritmo de cifrado. Este valor debe ser una cadena hexadecimal de la longitud apropiada (según el valor de descifrado), generar automáticamente o cualquiera de los valores anexada, IsolateApps. Agregar IsolateApps indica a ASP.NET que utilice un valor único para cada aplicación. El valor predeterminado es AutoGenerate, IsolateApps. |
| validación | Indica el algoritmo utilizado para la validación. Este atributo puede tener uno de los siguientes cuatro valores: - AES - utiliza el algoritmo estándar de cifrado avanzado (AES). -MD5 - utiliza el [síntesis del mensaje 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritmo. -SHA1 - utiliza el [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmo (el valor predeterminado). -3DES - utiliza el algoritmo Triple DES. |
| validationKey | La clave secreta usada por el algoritmo de validación. Este valor debe ser una cadena hexadecimal de la longitud apropiada (según el valor en la validación), generar automáticamente o cualquiera de los valores anexada, IsolateApps. Agregar IsolateApps indica a ASP.NET que utilice un valor único para cada aplicación. El valor predeterminado es AutoGenerate, IsolateApps. |

**Tabla 2**: el &lt;machineKey&gt; atributos del elemento

Una discusión detallada de estas opciones de cifrado y validación y las ventajas y desventajas de los diversos algoritmos, queda fuera del ámbito de este tutorial. Para buscar información detallada en estos problemas, incluidas instrucciones sobre qué algoritmos de cifrado y validación que se utilizan, qué longitudes de clave para usar y la mejor manera de generar estas claves, consulte *Professional ASP.NET 2.0 seguridad pertenencia y administración de roles* .

De forma predeterminada, las claves utilizadas para el cifrado y validación se generan automáticamente para cada aplicación, y dichas claves están almacenadas en la autoridad de seguridad Local (LSA). En resumen, la configuración predeterminada garantiza las claves únicas en un servidor web de web-server y la aplicación por aplicación. Por lo tanto, este comportamiento predeterminado no funcionará para los dos escenarios siguientes:

- **Granja de servidores Web** : en un [granja de servidores web](http://en.wikipedia.org/wiki/Web_farm) escenario, una sola aplicación web se hospeda en varios servidores web para los fines de redundancia y escalabilidad. Cada solicitud entrante se envía a un servidor en la granja de servidores, lo que significa que durante la vigencia de una sesión de usuario, los servidores diferentes se pueden utilizar para controlar sus varias solicitudes. Por lo tanto, cada servidor debe usar las mismas claves de cifrado y validación para que cree el vale de autenticación de formularios, cifrado y validado en un servidor puede descifrar y validar en un servidor diferente en la granja de servidores.
- **Cruzar el vale de uso compartido de aplicaciones** -un único servidor web puede hospedar varias aplicaciones de ASP.NET. Si necesita para que estas aplicaciones diferentes compartir un vale de autenticación de formularios único, es imperativo que coinciden con sus claves de cifrado y validación de.

Cuando se trabaja en una granja de servidores web, configuración o vales de autenticación de uso compartido entre aplicaciones en el mismo servidor, deberá configurar el &lt;machineKey&gt; elemento en las aplicaciones afectadas para que sus decryptionKey y los valores de validationKey coinciden.

Aunque ninguno de los escenarios anteriores se aplica a nuestra aplicación de ejemplo, aún podemos especificar los valores de validationKey y decryptionKey explícita y definir los algoritmos que se usará. Agregar un &lt;machineKey&gt; si se establece en el archivo Web.config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

Para obtener más información consulte [How To: Configure MachineKey en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Los valores decryptionKey y validationKey se realizaron desde [Steve Gibson](http://www.grc.com/stevegibson.htm)del [perfecto contraseñas web página](https://www.grc.com/passwords.htm), lo que genera 64 caracteres hexadecimales aleatorios cada vez que visito página. Para reducir la probabilidad de que estas claves incorporarlas a sus aplicaciones de producción, se recomienda reemplazar las claves anteriores junto con generado aleatoriamente que desde la página de contraseñas perfecto.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Paso 4: Almacenar datos de usuario adicionales en el vale

Muchas aplicaciones web mostrar información sobre o mostrar la página de base en el usuario ha iniciado sesión actualmente. Por ejemplo, una página web puede mostrar el nombre de usuario y la fecha por última vez que inició sesión en la esquina superior de cada página. El vale de autenticación de formularios almacena el nombre de usuario del usuario ha iniciado sesión actualmente, pero cuando se necesita cualquier otra información, la página debe ir a la tienda de usuario - normalmente una base de datos - para buscar la información no almacenada en el vale de autenticación.

Con un poco de código podemos almacenar información adicional del usuario en el vale de autenticación de formularios. Estos datos se pueden expresar mediante la [clase FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)del [propiedad UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Se trata de un lugar útil para colocar pequeñas cantidades de información sobre el usuario que normalmente se necesita. El valor especificado en la propiedad se incluye como parte de la cookie de vale de autenticación y, al igual que los demás campos vale, se cifran y validado de UserData según la configuración del sistema de autenticación de formularios. De forma predeterminada, UserData es una cadena vacía.

Para almacenar datos de usuario en el vale de autenticación, se debe escribir un fragmento de código en la página de inicio de sesión que toma la información específica del usuario y lo almacena en el vale. Puesto que UserData es una propiedad de tipo cadena, se deben serializar correctamente los datos almacenados en ella como una cadena. Por ejemplo, imagine que nuestro almacén de usuario incluye la fecha de nacimiento de cada usuario y el nombre de su empleador y queríamos almacenar valores de estas dos propiedades en el vale de autenticación. Nos podríamos serializar estos valores en una cadena mediante la concatenación de fecha del usuario de cadena del nacimiento con una barra vertical (|), seguido del nombre de la empresa. Para un usuario nacido en el 15 de agosto de 1974 que funciona para Northwind Traders, se podría asignar la propiedad UserData la cadena: 1974-08-15 | Northwind Traders.

Cada vez que se necesita tener acceso a los datos almacenados en el vale, podemos hacerlo; para hacerlo FormsAuthenticationTicket de la solicitud actual y deserializar la propiedad UserData. En el caso de la fecha de nacimiento y el empresario ejemplo de nombre, se podría dividir la cadena de UserData en dos subcadenas según el delimitador (|).


[![Información adicional del usuario se puede almacenar en el vale de autenticación](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**Figura 04**: adicionales usuario información puede almacenarse en el vale de autenticación ([haga clic aquí para ver imagen en tamaño completo](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Escribir información en UserData

Lamentablemente, no es tan sencillo como uno podría esperar agregar información específica del usuario a un vale de autenticación de formularios. La propiedad UserData de la clase FormsAuthenticationTicket es de solo lectura y solo se puede especificar a través del constructor de clase FormsAuthenticationTicket. Al especificar la propiedad UserData en el constructor, también se debe proporcionar el vale 's otros valores: el nombre de usuario, la fecha de emisión, la expiración y así sucesivamente. Cuando se creó la página de inicio de sesión en el tutorial anterior, esto se todos controló por nosotros la clase FormsAuthentication. Al agregar UserData a FormsAuthenticationTicket, necesitamos escribir código para replicar muchas de las funcionalidades ya proporciona la clase FormsAuthentication.

Vamos a examinar el código necesario para trabajar con datos del usuario mediante la actualización de la página Login.aspx para registrar información adicional sobre el usuario para el vale de autenticación. Supongamos que nuestro almacén de usuario contiene información acerca de la empresa trabaja el usuario y su título, y que queremos capturar esta información en el vale de autenticación. Actualice el controlador de eventos LoginButton haga clic en la página Login.aspx por lo que el código tenga un aspecto similar al siguiente:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

Vamos a recorrer este código de línea a la vez. El método comienza definiendo cuatro matrices de cadenas: los usuarios, contraseñas, companyName y titleAtCompany. Estas matrices contienen los nombres de usuario, contraseñas, nombres de compañía y títulos de las cuentas de usuario en el sistema, de que existen tres: Scott, Jisun y Sam. En una aplicación real, se podrían consultar estos valores desde el almacén de usuario, no codificados de forma rígida en el código fuente de la página.

En el tutorial anterior, si las credenciales proporcionadas son válidas se denomina simplemente FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked), que realiza los pasos siguientes:

1. Crea los formularios de vale de autenticación
2. El vale se escribió en el almacén adecuado. Para los vales de autenticación basada en cookies, se usa la colección de cookies del explorador; para los vales de autenticación sin cookies, se serializan los datos de entradas en la dirección URL
3. Redirige al usuario a la página adecuada

Estos pasos se replican en el código anterior. En primer lugar, la cadena que se almacenará en la propiedad UserData finalmente se forma combinando el nombre de la empresa y el título, la delimitación de los dos valores con un carácter de barra vertical (|).

Dim userDataString As String = String.Concat(companyName(i), "|", titleAtCompany(i))

A continuación, el FormsAuthentication.GetAuthCookie se invoca al método, que crea el vale de autenticación, cifra y lo valida según los valores de configuración y lo coloca en un objeto HttpCookie.

Dim authCookie como HttpCookie = FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked)

Para trabajar con el FormAuthenticationTicket incrustada dentro de la cookie, es necesario llamar a la clase FormAuthentication [descifrar método](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), pasando el valor de cookie.

Dim vale FormsAuthenticationTicket como = FormsAuthentication.Decrypt(authCookie.Value)

A continuación, creamos un *nuevo* FormsAuthenticationTicket instancia según los valores del objeto FormsAuthenticationTicket existente. Sin embargo, este nuevo vale incluye la información específica del usuario (userDataString).

Dim newTicket como FormsAuthenticationTicket = FormsAuthenticationTicket(ticket. nuevo Versión, vale. Nombre, vale. IssueDate, vale. Caducidad, vale. IsPersistent, userDataString)

Nosotros, a continuación, cifrar (y validar) la nueva instancia de objeto FormsAuthenticationTicket mediante una llamada a la [cifrar método](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)y se vuelven a poner estos datos cifrados (y validados) en authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket)

Por último, authCookie se agrega a la colección Response.Cookies y se llama al método de GetRedirectUrl para determinar la página adecuada para enviar al usuario.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

Todo este código es necesario porque la propiedad UserData es de solo lectura y la clase FormsAuthentication no proporciona ningún método para especificar la información de UserData en sus métodos GetAuthCookie, SetAuthCookie o RedirectFromLoginPage.

> [!NOTE]
> El código que simplemente examinamos almacena información específica del usuario en un vale de autenticación basada en cookies. El responsable de serializar el vale de autenticación de formularios a la dirección URL de las clases son internas a .NET Framework. Larga historia breve, no se puede almacenar datos de usuario en un vale de autenticación de formularios sin cookies.


### <a name="accessing-the-userdata-information"></a>Acceso a la información de UserData

En este momento el nombre de la empresa y el título de cada usuario se almacena en UserData propiedad del vale autenticación de formularios cuando inicie sesión. Esta información puede obtenerse desde el vale de autenticación en cualquier página sin necesidad de un viaje en el almacén de usuario. Para ilustrar cómo se puede recuperar esta información de la propiedad UserData, vamos a actualizar Default.aspx para que su mensaje de bienvenida incluye no sólo el nombre del usuario, pero también la compañía que trabajan para y su título.

Actualmente, Default.aspx contiene un AuthenticatedMessagePanel Panel con un control de etiqueta denominado WelcomeBackMessage. Este Panel solo se muestra a los usuarios autenticados. Actualice el código en la página del Default.aspx\_cargar el controlador de eventos para que tenga un aspecto similar al siguiente:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Si Request.IsAuthenticated es True, entonces la propiedad de texto del WelcomeBackMessage se establece primero en Bienvenido de nuevo, *username*. A continuación, la propiedad User.Identity se convierte en un objeto FormsIdentity para que podamos acceder a FormsAuthenticationTicket subyacente. Una vez que tenemos FormsAuthenticationTicket, se deserializa la propiedad UserData en el nombre de la empresa y el título. Esto se consigue mediante la división de la cadena en el carácter de barra vertical. El nombre de la empresa y el título se muestran a continuación, en la etiqueta WelcomeBackMessage.

Figura 5 muestra una captura de pantalla de esta presentación en acción. Inicie sesión como Scott muestra un mensaje de back-Bienvenido que incluye la empresa y el título de Scott.


[![Empresa actualmente ha iniciado en del usuario y el título se muestran](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**Figura 05**: empresa y el título de la actualmente registrado del usuario que se muestran ([haga clic aquí para ver imagen en tamaño completo](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> Propiedad de UserData del vale de autenticación actúa como una memoria caché para el almacén del usuario. Al igual que cualquier caché, debe actualizarse cuando se modifican los datos subyacentes. Por ejemplo, si hay una página web desde el que los usuarios pueden actualizar su perfil, los campos almacenados en caché en la propiedad UserData deben actualizarse para reflejar los cambios realizados por el usuario.


## <a name="step-5-using-a-custom-principal"></a>Paso 5: Uso de un objeto Principal personalizado

En cada solicitud entrante FormsAuthenticationModule intenta autenticar al usuario. Si está presente un vale de autenticación no expiradas, FormsAuthenticationModule asigna la propiedad HttpContext.User a un nuevo objeto GenericPrincipal. Este objeto GenericPrincipal tiene una identidad de tipo FormsIdentity, que incluye una referencia al vale de autenticación de formularios. La clase GenericPrincipal contiene la funcionalidad mínima operativo necesaria para una clase que implementa IPrincipal: solo tiene una propiedad de identidad y un método IsInRole.

El objeto principal tiene dos responsabilidades: para indicar qué roles que pertenece el usuario y para proporcionar información de identidad. Esto se logra a través de IsInRole la interfaz IPrincipal (*roleName*) método y propiedad de identidad, respectivamente. La clase GenericPrincipal permite una matriz de cadenas de nombres de función para especificarse a través de su constructor; su IsInRole (*roleName*) método simplemente comprueba si ha pasado en *roleName* existe dentro de la matriz de cadenas. Cuando FormsAuthenticationModule crea el objeto GenericPrincipal, pasa una matriz de cadena vacía al constructor de GenericPrincipal. Por lo tanto, cualquier llamada a IsInRole siempre devolverá False.

La clase GenericPrincipal satisface las necesidades para escenarios de autenticación basada en formularios mayoría donde no se usan los roles. En aquellas situaciones donde el tratamiento del rol predeterminado no es suficiente o cuando necesite asociar un objeto IIdentity personalizado con el usuario, puede crear un objeto IPrincipal personalizado durante el flujo de trabajo de autenticación y asignarla a la propiedad HttpContext.User.

> [!NOTE]
> Como veremos en el futuro tutoriales, cuando ASP. Está habilitada framework de Roles de .NET crea un objeto principal personalizado del tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) y sobrescribe el objeto GenericPrincipal creado para la autenticación de formularios. Lo hace para personalizar el método IsInRole de la entidad de seguridad para interactuar con la API de .NET framework de Roles.


Puesto que nos hemos no le preocupa nosotros mismos roles aún, la única razón que para crear a una entidad personalizada en ese momento, tendríamos sería asociar un objeto IIdentity personalizado a la entidad de seguridad. En el paso 4 analizamos almacenar información adicional del usuario en la propiedad UserData del vale de autenticación, en particular, su título y el nombre de la empresa del usuario. Sin embargo, la información de UserData solo es accesible mediante el vale de autenticación y, a continuación, solo como una cadena serializada, lo que significa que, cuando queremos ver la información de usuario almacenada en el vale se necesita analizar la propiedad UserData.

Podemos mejorar la experiencia del desarrollador mediante la creación de una clase que implementa IIdentity e incluye las propiedades de título y CompanyName. De este modo, un desarrollador puede tener acceso a nombre de la empresa del usuario ha iniciado sesión actualmente, y título directamente a través de las propiedades de título y CompanyName sin que sea necesario saber cómo analizar la propiedad UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Creación de la identidad personalizada y las clases de entidad de seguridad

Para este tutorial, vamos a crear los objetos principal e identity personalizados en la aplicación\_carpeta de código. Empiece por agregar una aplicación\_carpeta al proyecto de código: haga doble clic en el nombre del proyecto en el Explorador de soluciones, seleccione la opción de Agregar carpeta ASP.NET y elija aplicación\_código. La aplicación\_carpeta de código es una carpeta especial de ASP.NET que contiene archivos específico para el sitio Web de clase.

> [!NOTE]
> La aplicación\_carpeta de código solo debe usarse al administrar el proyecto mediante el modelo de proyecto de sitio Web. Si usas el [modelo de proyecto de aplicación Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), cree una carpeta estándar y agregue las clases a la. Por ejemplo, podría agregar una carpeta nueva denominada clases y colocar el código allí.


A continuación, agregue dos nuevos archivos de clase a la aplicación\_carpeta de código, una CustomIdentity.vb con nombre y otra denominan CustomPrincipal.vb.


[![Agregue las clases de CustomPrincipal y CustomIdentity al proyecto](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**Figura 06**: agregar las clases de CustomPrincipal y CustomIdentity a su proyecto ([haga clic aquí para ver imagen en tamaño completo](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


La clase CustomIdentity es responsable de implementar la interfaz de IIdentity, que define las propiedades de nombre, IsAuthenticated y AuthenticationType. Además de las propiedades necesarias, estamos interesados en exponer el subyacente vale de autenticación de formularios, así como propiedades de nombre de la empresa y el título del usuario. Escriba el código siguiente en la clase CustomIdentity.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

Tenga en cuenta que la clase incluye una variable de miembro FormsAuthenticationTicket (\_vale) y que se debe proporcionar esta información del vale a través del constructor. Estos datos de vale se utilizan para devolver el nombre de identidad; su propiedad UserData se analiza para devolver los valores de las propiedades de título y CompanyName.

A continuación, cree la clase CustomPrincipal. Puesto que no se preocupan los roles en ese momento, el constructor de la clase CustomPrincipal acepta solo un objeto CustomIdentity; su método IsInRole siempre devuelve False.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Asignación de un objeto CustomPrincipal al contexto de seguridad de la solicitud entrante

Ahora tenemos una clase que extiende la especificación de IIdentity predeterminado para incluir propiedades de título y CompanyName, así como una clase entidad personalizada que usa la identidad personalizada. Se está preparados para ir a la canalización de ASP.NET y se asigna el objeto principal personalizado al contexto de seguridad de la solicitud entrante.

La canalización de ASP.NET toma una solicitud entrante y lo procesa mediante una serie de pasos. En cada paso, se genera un evento determinado, lo que permite a los desarrolladores aprovechar la canalización de ASP.NET y modifique la solicitud en ciertos puntos de su ciclo de vida. FormsAuthenticationModule, por ejemplo, se espera para que ASP.NET genere la [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), momento en que examina las solicitudes entrantes de un vale de autenticación. Si se encuentra un vale de autenticación, se crea un objeto GenericPrincipal y se asigna a la propiedad HttpContext.User.

Después del evento AuthenticateRequest, la canalización de ASP.NET genera el [PostAuthenticateRequest eventos](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que es donde podemos reemplazar el objeto GenericPrincipal creado por FormsAuthenticationModule con una instancia de nuestra Objeto CustomPrincipal. Figura 7 muestra este flujo de trabajo.


[![El objeto GenericPrincipal se sustituye por un CustomPrincipal en el evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**Figura 07**: The GenericPrincipal se sustituye por un CustomPrincipal en el evento PostAuthenticationRequest ([haga clic aquí para ver imagen en tamaño completo](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


Para ejecutar código en respuesta a un evento de la canalización ASP.NET, nos podemos crear el controlador de eventos apropiado en Global.asax o crear nuestro propio módulo HTTP. En este tutorial vamos a crear el controlador de eventos en Global.asax. Empiece agregando Global.asax a su sitio Web. Haga doble clic en el nombre del proyecto en el Explorador de soluciones y agregue un elemento de tipo de clase de aplicación Global denominada Global.asax.


[![Agregue un archivo Global.asax para su sitio Web](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**Figura 08**: agregar un archivo Global.asax para su sitio Web ([haga clic aquí para ver imagen en tamaño completo](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


La plantilla predeterminada de Global.asax incluye controladores de eventos para un número de los eventos de canalización ASP.NET, incluido el inicio, fin y [evento de Error](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), entre otros. No dude en quitar estos controladores de eventos, ya no se necesita para esta aplicación. El evento que nos interesa es PostAuthenticateRequest. Actualice el archivo Global.asax para que su marcado tiene un aspecto similar al siguiente:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

La aplicación\_OnPostAuthenticateRequest método ejecuta cada vez que el tiempo de ejecución ASP.NET genera el evento PostAuthenticateRequest, lo que sucede una vez en cada solicitud entrante de la página. El controlador de eventos se inicia y comprueba si el usuario se autentica y se autenticó mediante la autenticación de formularios. Si es así, se crea un nuevo objeto de CustomIdentity y se pasa el vale de autenticación de la solicitud actual en su constructor. A continuación, un objeto CustomPrincipal se crea y se pasa el objeto CustomIdentity acaba de crear en su constructor. Por último, se asigna el contexto de seguridad de la solicitud actual para el objeto CustomPrincipal recién creado.

Tenga en cuenta que el último paso: asociar el objeto CustomPrincipal con contexto de seguridad de la solicitud - asigna la entidad a dos propiedades: HttpContext.User y Thread.CurrentPrincipal. Estas dos asignaciones son necesarias debido a la manera en que se administran los contextos de seguridad en ASP.NET. .NET Framework se asocia un contexto de seguridad a cada subproceso en ejecución; Esta información está disponible como un objeto IPrincipal a través de la [objeto Thread](https://msdn.microsoft.com/library/system.threading.thread.aspx)del [propiedad CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). ¿Qué es un tipo de confusión es que ASP.NET tiene su propia información de contexto de seguridad (HttpContext.User).

En determinados escenarios, se examina la propiedad Thread.CurrentPrincipal al determinar el contexto de seguridad; en otros escenarios, se usa HttpContext.User. Por ejemplo, existen características de seguridad de .NET que permiten a los desarrolladores mediante declaración indicar qué usuarios o roles pueden crear una instancia de una clase o invocar métodos específicos (consulte [agregar reglas de autorización de negocio y capas de datos con PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Interiormente, estas técnicas declarativas determinan el contexto de seguridad a través de la propiedad Thread.CurrentPrincipal.

En otros escenarios, se usa la propiedad HttpContext.User. Por ejemplo, en el tutorial anterior se utiliza esta propiedad para mostrar el nombre de usuario del usuario ha iniciado sesión actualmente. Claramente, a continuación, es imprescindible que la información de contexto de seguridad en las propiedades de Thread.CurrentPrincipal y HttpContext.User coincide.

El tiempo de ejecución ASP.NET sincroniza automáticamente estos valores de propiedad para nosotros. Sin embargo, la sincronización se produce después del evento AuthenticateRequest, pero *antes* el evento PostAuthenticateRequest. Por lo tanto, al agregar una entidad de seguridad personalizada en el evento PostAuthenticateRequest debemos estar seguro de que asignar manualmente Thread.CurrentPrincipal o bien Thread.CurrentPrincipal y HttpContext.User estarán sincronizados. Consulte [Context.User vs. Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) para obtener una explicación más detallada sobre este problema.

### <a name="accessing-the-companyname-and-title-properties"></a>Acceso a CompanyName y propiedades del título

Cada vez que se recibe una solicitud y se envía al motor de ASP.NET, la aplicación\_se activará el controlador de eventos OnPostAuthenticateRequest en Global.asax. Si la solicitud se ha autenticado correctamente por FormsAuthenticationModule, el controlador de eventos creará un nuevo objeto CustomPrincipal con un objeto de CustomIdentity basado en el vale de autenticación de formularios. Con esta lógica en su lugar, tener acceso a información sobre el nombre de la empresa del usuario ha iniciado sesión actualmente y el título es increíblemente sencillo.

Volver a la página\_controlador de eventos de carga en Default.aspx, donde en el paso 4 que escribimos código para recuperar el vale de autenticación de formulario y analizar la propiedad UserData con el fin de mostrar el nombre de la empresa y el título del usuario. Con los objetos CustomPrincipal y CustomIdentity en uso ahora, no hay ninguna necesidad para analizar los valores de propiedad de UserData del vale. En su lugar, sólo tiene que obtener una referencia al objeto CustomIdentity y utilizar sus propiedades CompanyName y el título:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>Resumen

En este tutorial hemos visto cómo personalizar la configuración del sistema de autenticación de formularios a través de Web.config. Hemos visto cómo se controla la caducidad del vale de autenticación y cómo se usan las medidas de seguridad de cifrado y validación para proteger el vale de inspección y modificación. Por último, le explicamos cómo utilizar la propiedad UserData del vale de autenticación para almacenar información adicional del usuario en el propio vale y cómo usar objetos principal e identity personalizados para exponer esta información de un modo más sencillo para los desarrolladores.

En este tutorial concluye nuestro examen de autenticación de formularios en ASP.NET. El siguiente tutorial inicia nuestro viaje en el marco de pertenencia.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Si examinamos la autenticación de formularios](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Se explica: Autenticación de formularios en ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Cómo: Proteger la autenticación de formularios en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 seguridad, la pertenencia y administración de roles](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Protección de los controles de inicio de sesión](https://msdn.microsoft.com/library/ms178346.aspx)
- [El &lt;autenticación&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [El &lt;formularios&gt; (elemento) para &lt;autenticación&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [El &lt;machineKey&gt; elemento](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Descripción del vale de autenticación de formularios y cookies](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>En los temas contenidos en este Tutorial en vídeo

- [Cómo cambiar las propiedades de autenticación de formularios](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Cómo se instale y Use la autenticación sin cookies en una aplicación ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Reubicación del inicio de sesión de los formularios ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configuración de claves personalizadas de inicio de sesión de los formularios](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Agregar datos personalizados al método de autenticación](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usar objetos de entidad de seguridad personalizados](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Alicja Maziarz. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](an-overview-of-forms-authentication-vb.md)
