---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Sitio mediante un CAPTCHA para evitar Bots del uso de la Web de ASP.NET Razor) | Documentos de Microsoft
author: microsoft
description: Este artículo explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realizar tareas en un ASP.NET Web Pages (Razor) se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529924"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Sitio mediante un CAPTCHA para evitar Bots del uso de la Web de ASP.NET Razor)
====================
por [Microsoft](https://github.com/microsoft)

> En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realizar tareas en un sitio Web de ASP.NET Web Pages (Razor).
> 
> **Lo que aprenderá:** 
> 
> - Cómo agregar una prueba CAPTCHA a su sitio.
> 
> Estas son las características ASP.NET presentadas en el artículo:
> 
> - El `ReCaptcha` auxiliar.
> 
> > [!NOTE]
> > La información de este artículo se aplica a páginas Web de ASP.NET 1.0 y 2 páginas Web.


## <a name="about-captchas"></a>Acerca de CAPTCHAs

Cada vez que permite a los usuarios registrar en su sitio, o incluso simplemente escriba un nombre y una dirección URL (como un comentario de blog), es posible que obtenga una avalancha de nombres falsas. Se dejan con frecuencia por programas automatizados (bots) que intente excluir las direcciones URL en todos los sitios Web que puede encontrar. (Una motivación común es registrar las direcciones URL de productos para la venta).

Puede ayudar a asegurarse de que un usuario es una persona real y no un programa del equipo mediante el uso de un *CAPTCHA* para validar a los usuarios cuando registre o en caso contrario, escriba su nombre y el sitio. CAPTCHA significa prueba completamente automatizada pública Turing indicar a los equipos y los seres humanos separadas. Es un CAPTCHA un *desafío / respuesta* prueba en el que se pregunta al usuario hacer algo que es fácil de una persona hacer pero difícil para un programa automatizado realizar. El tipo más común de CAPTCHA es uno donde vea algunas letras distorsionada y le pide que escribirlas. (La distorsión se supone que resulte difícil de robots de descifrar las letras).

## <a name="adding-a-recaptcha-test"></a>Agregar una prueba de ReCaptcha

En las páginas ASP.NET, puede usar el `ReCaptcha` auxiliar para representar una prueba CAPTCHA que se basa en el servicio de ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). El `ReCaptcha` auxiliar muestra una imagen de dos o más palabras distorsionados que los usuarios deben especificar correctamente antes de que se valide la página. El servicio ReCaptcha.Net se valida la respuesta del usuario.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrar su sitio Web en ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Cuando haya completado el registro, obtendrá una clave pública y una clave privada.
2. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
3. Si aún no tiene un  *\_AppStart.cshtml* , en la carpeta raíz de un sitio Web, cree un archivo denominado  *\_AppStart.cshtml*.
4. Agregue el siguiente `Recaptcha` configuración de la aplicación auxiliar en el  *\_AppStart.cshtml* archivo: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Establecer el `PublicKey` y `PrivateKey` propiedades mediante sus propias claves públicas y privadas.
6. Guardar el  *\_AppStart.cshtml* archivo y ciérrelo.
7. En la carpeta raíz de un sitio Web, cree la nueva página denominada *Recaptcha.cshtml*.
8. Reemplace el contenido existente con lo siguiente: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Ejecute el *Recaptcha.cshtml* página en un explorador. Si el `PrivateKey` valor es válido, la página muestra el control de ReCaptcha y un botón. Si no se hubiera establecido las claves de forma global en  *\_AppStart.html*, la página mostrará un error. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Escriba las palabras de la prueba. Si se pasa la prueba de ReCaptcha, verá un mensaje a tal efecto. En caso contrario, verá un mensaje de error y se vuelve a mostrar el control de ReCaptcha.

> [!NOTE]
> Si el equipo está en un dominio que utiliza un servidor proxy, tendrá que configurar el `defaultproxy` elemento de la *Web.config* archivo. El ejemplo siguiente muestra un *Web.config* de archivos con la `defaultproxy` elemento configurado para habilitar el servicio de ReCaptcha para que funcione.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


- [Personalizar el comportamiento de todo el sitio para los sitios de páginas Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sitio de ReCaptcha](https://www.google.com/recaptcha)
