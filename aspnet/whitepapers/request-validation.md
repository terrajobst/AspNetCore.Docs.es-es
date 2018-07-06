---
uid: whitepapers/request-validation
title: Validación de solicitudes - prevenir ataques de scripts | Microsoft Docs
author: rick-anderson
description: Este artículo describe la característica de validación de solicitudes de ASP.NET que, de forma predeterminada, la aplicación es evitar el procesamiento enviar contenido de HTML sin codificar...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0dfbfcae70792c57d530fc5e6fb73f8f96ec6e02
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809750"
---
<a name="request-validation---preventing-script-attacks"></a>Validación de solicitudes - prevenir ataques de scripts
====================
> Este documento describe la característica de validación de solicitudes de ASP.NET que, de forma predeterminada, la aplicación es evitar el procesamiento sin codificar contenido HTML que se envían al servidor. Cuando la aplicación se ha diseñado para procesar datos HTML de forma segura, se puede deshabilitar esta característica de validación de solicitud.
> 
> Se aplica a ASP.NET 1.1 y ASP.NET 2.0.


Validación de solicitudes, una característica de ASP.NET desde la versión 1.1, impide que el servidor acepte contenido con HTML sin codificar. Esta característica está diseñada para ayudar a evitar algunos ataques de inyección de script mediante el cual el código de script de cliente o HTML se puede enviarse a un servidor, almacenar y, a continuación, se presentan a otros usuarios. Siguen establecidos estrictamente, se recomienda que valide todos los datos recibidos y codificación HTML cuando corresponda.

Por ejemplo, cree una página Web que solicita la dirección de correo electrónico del usuario y luego almacena ese correo electrónico en una base de datos. Si el usuario escribe &lt;SCRIPT&gt;("hello from script") de la alerta&lt;/SCRIPT&gt; en lugar de una dirección de correo electrónico válida, cuando esos datos se presentan, esta secuencia de comandos se puede ejecutar si el contenido no se codificó correctamente. La característica de validación de solicitudes de ASP.NET evita que esto suceda.

## <a name="why-this-feature-is-useful"></a>¿Por qué esta característica es útil

Muchos sitios no son conscientes de que están dispuestos a ataques de inyección de script sencillo. Si el propósito de estos ataques es a estropear el sitio presentando HTML o potencialmente ejecutar script de cliente para redirigir al usuario al sitio de los piratas informáticos, ataques de inyección de script son un problema que los desarrolladores Web deben lidiar con.

Ataques de inyección de script son una preocupación para todos los desarrolladores web, independientemente de si usan otras tecnologías de desarrollo web, ASP o ASP.NET.

La característica de validación de solicitudes ASP.NET proactivamente impide estos ataques, ya que no permite contenido HTML sin codificar ser procesados por el servidor a menos que el desarrollador decide permitir que el contenido.

## <a name="what-to-expect-error-page"></a>Qué esperar: página de Error

La captura de pantalla siguiente muestra algunos ejemplos de código ASP.NET:

![](request-validation/_static/image1.png)

Resultados de este código se ejecuta en una página sencilla que le permite escribir algún texto en el cuadro de texto, haga clic en el botón y mostrar el texto en el control de etiqueta:

![](request-validation/_static/image2.png)

Sin embargo, se han JavaScript, como `<script>alert("hello!")</script>` que se especifique y enviado se obtendría una excepción:

![](request-validation/_static/image3.png)

El mensaje de error indica que un 'potencialmente peligrosos Request.Form se ha detectado el valor' y ofrecen más detalles en la descripción de exactamente lo que ha ocurrido y cómo cambiar el comportamiento. Por ejemplo:

Validación de solicitudes ha detectado un valor de entrada potencialmente peligrosos del cliente, y ha anulado el procesamiento de la solicitud. Este valor puede indicar un intento de poner en peligro la seguridad de la aplicación, como un ataque XSS. Puede deshabilitar la validación de solicitudes estableciendo `validateRequest=false` en la directiva de página o en la sección de configuración. Sin embargo, se recomienda encarecidamente que la aplicación compruebe explícitamente todas las entradas en este caso.

## <a name="disabling-request-validation-on-a-page"></a>Deshabilitar la validación de solicitud en una página

Para deshabilitar la validación de solicitud en una página que se debe establecer el `validateRequest` atributo de la directiva de página a `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Cuando se deshabilita la validación de solicitudes, se puede enviar contenido a una página; es responsabilidad del programador de la página para asegurarse de que el contenido se codifican correctamente o se procesan.

## <a name="disabling-request-validation-for-your-application"></a>Deshabilitar la validación de solicitud para la aplicación

Para deshabilitar la validación de solicitudes para su aplicación, debe modificar o crear un archivo Web.config para la aplicación y establezca el atributo validateRequest de la `<pages />` sección a `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Si desea deshabilitar la validación de solicitudes para todas las aplicaciones en el servidor, puede realizar esta modificación en el archivo Machine.config.

> [!CAUTION]
> Cuando se deshabilita la validación de solicitudes, se puede enviar contenido a la aplicación; es responsabilidad del desarrollador de la aplicación para asegurarse de que el contenido se codifican correctamente o se procesan.

El código siguiente se modifica para desactivar la validación de solicitud:

![](request-validation/_static/image4.png)

Ahora, si el siguiente código de JavaScript se ha escrito en el cuadro de texto `<script>alert("hello!")</script>` el resultado sería:

![](request-validation/_static/image5.png)

Para evitar que esto suceda, con validación de solicitudes que se ha desactivado, se necesitamos, en HTML, codifique el contenido.

## <a name="how-to-html-encode-content"></a>Cómo a HTML codificar contenido

Si ha deshabilitado la validación de solicitudes, es recomendable contenido codificar en HTML que se almacenará para su uso futuro. Codificación HTML reemplazará automáticamente cualquier '&lt;'o'&gt;' (junto con otros símbolos) con su HTML correspondiente representación codificada. Por ejemplo, '&lt;'se sustituye por'&amp;lt;' y '&gt;'se sustituye por'&amp;gt;'. Los exploradores usan estos códigos especiales para mostrar el '&lt;'o'&gt;' en el explorador.

El contenido puede ser fácilmente codificada en HTML en el servidor con el `Server.HtmlEncode(string)` API. Contenido también puede ser fácilmente descodifica para HTML, es decir, revertir a HTML estándar mediante el `Server.HtmlDecode(string)` método.

![](request-validation/_static/image6.png)

Lo que resulta en:

![](request-validation/_static/image7.png)
