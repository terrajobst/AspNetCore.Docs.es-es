---
uid: whitepapers/request-validation
title: "Solicitar validación - prevención de ataques de Script | Documentos de Microsoft"
author: rick-anderson
description: "Este documento describe la característica de validación de solicitud de ASP.NET donde, de forma predeterminada, la aplicación no puede procesar enviar contenido de HTML sin codificar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a>La validación - prevención de ataques de secuencia de comandos de solicitudes
====================
> Este documento describe la característica de validación de solicitud de ASP.NET donde, de forma predeterminada, la aplicación no puede procesar el contenido HTML sin codificar enviado al servidor. Esta característica de validación de solicitud puede estar deshabilitada cuando la aplicación se ha diseñado para procesar datos HTML de forma segura.
> 
> Se aplica a ASP.NET 1.1 y ASP.NET 2.0.


Validación de solicitudes, una característica de ASP.NET desde la versión 1.1, impide que el servidor acepte contenido HTML sin codificar que lo contiene. Esta característica está diseñada para ayudar a evitar algunos ataques de inyección de script mediante el cual código de script de cliente o HTML puede ser sin saberlo enviada a un servidor, almacena y, a continuación, se presentan a otros usuarios. Se recomienda encarecidamente seguir que validar todos los datos recibidos y codificación HTML que cuando corresponda.

Por ejemplo, cree una página Web que solicita la dirección de correo electrónico de un usuario y, a continuación, almacena ese correo electrónico en una base de datos. Si el usuario escribe &lt;SCRIPT&gt;alerta ("Hola desde un script")&lt;/SCRIPT&gt; en lugar de una dirección de correo electrónico válida, cuando esos datos se presentación, esta secuencia de comandos se puede ejecutar si el contenido no se codificó correctamente. La característica de validación de solicitud de ASP.NET evita que esto suceda.

## <a name="why-this-feature-is-useful"></a>¿Por qué esta característica es útil

Muchos sitios no son conscientes de que estén abiertos a ataques de inyección de script simple. Si el propósito de este tipo de ataques es a estropear el sitio mostrando HTML o potencialmente ejecutar script de cliente para redirigir al usuario al sitio de un intruso, ataques de inyección de script son un problema que los desarrolladores Web deben lidiar con.

Ataques de inyección de script son un problema de todos los desarrolladores web, independientemente de que están utilizando ASP.NET, ASP u otras tecnologías de desarrollo de web.

La característica de validación de solicitud ASP.NET proactivamente impide estos ataques al no permitir el contenido sobre HTML sin codificar ser procesados por el servidor a menos que el desarrollador decide permitir que el contenido.

## <a name="what-to-expect-error-page"></a>Qué esperar: página de Error

La captura de pantalla siguiente muestra ejemplos de código de ASP.NET:

![](request-validation/_static/image1.png)

Resultados de este código se ejecuta en una página sencilla que le permite escribir texto en el cuadro de texto, haga clic en el botón y muestra el texto en el control de etiqueta:

![](request-validation/_static/image2.png)

Sin embargo, eran JavaScript, como `<script>alert("hello!")</script>` que se especifique y envían obtendríamos una excepción:

![](request-validation/_static/image3.png)

El mensaje de error indica que un 'potencialmente peligroso Request.Form detectó el valor' y se proporcionan más detalles en la descripción en cuanto a exactamente lo que ha sucedido y cómo cambiar el comportamiento. Por ejemplo:

Validación de solicitudes ha detectado un valor de entrada de cliente potencialmente peligrosos y ha anulado el procesamiento de la solicitud. Este valor puede indicar un intento de poner en peligro la seguridad de la aplicación, como un ataque XSS. Puede deshabilitar la validación de solicitudes estableciendo `validateRequest=false` en la directiva de página o en la sección de configuración. Sin embargo, se recomienda encarecidamente que su aplicación compruebe explícitamente todas las entradas en este caso.

## <a name="disabling-request-validation-on-a-page"></a>Cómo deshabilitar la validación de solicitud en una página

Para deshabilitar la validación de solicitud en una página que se debe establecer el `validateRequest` atributo de la directiva de página a `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Cuando se deshabilita la validación de solicitudes, el contenido se puede enviar a una página; es responsabilidad del programador de la página para asegurarse de que el contenido se codifican o se procesan correctamente.

## <a name="disabling-request-validation-for-your-application"></a>Cómo deshabilitar la validación de solicitud para la aplicación

Para deshabilitar la validación de solicitudes en la aplicación, debe modificar o crear un archivo Web.config para la aplicación y establezca el atributo validateRequest de la `<pages />` sección a `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Si desea deshabilitar la validación de solicitudes para todas las aplicaciones en el servidor, puede realizar esta modificación en el archivo Machine.config.

> [!CAUTION]
> Cuando se deshabilita la validación de solicitudes, el contenido se puede enviar a la aplicación; es responsabilidad del desarrollador de la aplicación para asegurarse de que el contenido se codifica correctamente o se procese.

El código siguiente se modifica para desactivar la validación de solicitud:

![](request-validation/_static/image4.png)

Ahora, si se ha escrito el código JavaScript siguiente en el cuadro de texto `<script>alert("hello!")</script>` el resultado sería:

![](request-validation/_static/image5.png)

Para evitar que esto ocurra, con validación de solicitudes está desactivada, se necesitamos, en HTML, codificar el contenido.

## <a name="how-to-html-encode-content"></a>Cómo a HTML codificar contenido

Si deshabilita la validación de solicitudes, es recomendable contenido codificar en HTML que se almacenará para un uso futuro. Codificación HTML reemplazará automáticamente cualquier '&lt;'o'&gt;' (junto con otros símbolos) con su correspondiente HTML representación con codificación. Por ejemplo, '&lt;'se reemplaza por'&amp;lt;' y '&gt;'se reemplaza por'&amp;gt;'. Los exploradores utilizan estos códigos especiales para mostrar el '&lt;'o'&gt;' en el explorador.

Contenido puede ser fácilmente codificada en HTML en el servidor mediante el `Server.HtmlEncode(string)` API. Contenido también puede ser fácilmente descodificados para HTML, es decir, vuelto a usar el estándar HTML la `Server.HtmlDecode(string)` método.

![](request-validation/_static/image6.png)

Lo que:

![](request-validation/_static/image7.png)
