---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Usar un CAPTCHA para evitar que usen su Razor de ASP.NET Web Bots) sitio | Microsoft Docs
author: microsoft
description: En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realizar tareas en una ASP.NET Web Pages (Razor) se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: cfdc8561f8ee4b97cc1ef2111113846a1c579d5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389077"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Sitio de usar un CAPTCHA para evitar que los Bots usen su Razor de ASP.NET Web)
====================
por [Microsoft](https://github.com/microsoft)

> En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realizar tareas en un sitio Web de ASP.NET Web Pages (Razor).
> 
> **Lo que aprenderá:** 
> 
> - Cómo agregar una prueba CAPTCHA para su sitio.
> 
> Estas son las características ASP.NET incorporadas en el artículo:
> 
> - El `ReCaptcha` auxiliar.
> 
> > [!NOTE]
> > La información de este artículo se aplica a las páginas Web de ASP.NET 1.0 y Web Pages 2.


## <a name="about-captchas"></a>Acerca de CAPTCHAs

Cada vez que permite a los usuarios registrarse en su sitio, o incluso simplemente escriba un nombre y la dirección URL (como para un comentario en el blog), es posible que obtenga una avalancha de nombres falsos. Esto se dejan a menudo por programas automatizados (bots) que intentan salir de las direcciones URL en cada sitio Web que puede encontrar. (Un motivo común es registrar las direcciones URL de productos para su venta).

Puede ayudar a asegurarse de que un usuario es una persona real y no un programa del equipo mediante el uso de un *CAPTCHA* para validar a los usuarios cuando registre o en caso contrario, escriba su nombre y el sitio. CAPTCHA significa pruebas completamente automatizadas Turing pública indicar a los equipos y los seres humanos separados. Un CAPTCHA es un *desafío / respuesta* prueba en el que se pregunta al usuario hacer algo que sea fácil de una persona pero difícil para un programa automatizado hacer. El tipo más común de CAPTCHA es uno donde ver algunas letras distorsionada y se le pide que escribirlas. (La distorsión se supone que resulte difícil para los bots descifrar las letras).

## <a name="adding-a-recaptcha-test"></a>Agregar una prueba de ReCaptcha

En las páginas ASP.NET, puede usar el `ReCaptcha` auxiliar para representar una prueba CAPTCHA que se basa en el servicio de ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). El `ReCaptcha` auxiliar muestra una imagen de dos palabras distorsionadas que los usuarios deben especificar correctamente antes de que se valide la página. El servicio ReCaptcha.Net se valida la respuesta del usuario.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrar su sitio Web en ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Cuando haya completado el registro, obtendrá una clave pública y una clave privada.
2. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
3. Si aún no tiene un  *\_AppStart.cshtml* , en la carpeta raíz de un sitio Web, cree un archivo denominado  *\_AppStart.cshtml*.
4. Agregue el siguiente `Recaptcha` configuración de aplicación auxiliar en el  *\_AppStart.cshtml* archivo: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Establecer el `PublicKey` y `PrivateKey` propiedades mediante sus propias claves públicas y privadas.
6. Guardar el  *\_AppStart.cshtml* de archivo y ciérrelo.
7. En la carpeta raíz de un sitio Web, cree la nueva página denominada *Recaptcha.cshtml*.
8. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Ejecute el *Recaptcha.cshtml* página en un explorador. Si el `PrivateKey` valor es válido, la página muestra el control de ReCaptcha y un botón. Si no se hubiera establecido las claves de forma global en  *\_AppStart.html*, la página mostrará un error. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Escriba las palabras de la prueba. Si se pasa la prueba de ReCaptcha, verá un mensaje a tal efecto. En caso contrario, verá un mensaje de error y se vuelve a mostrar el control de ReCaptcha.

> [!NOTE]
> Si el equipo está en un dominio que utiliza un servidor proxy, deberá configurar el `defaultproxy` elemento de la *Web.config* archivo. El ejemplo siguiente se muestra un *Web.config* archivo con el `defaultproxy` elemento configurado para habilitar el servicio de ReCaptcha para que funcione.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


- [Personalizar el comportamiento de todo el sitio para sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sitio de ReCaptcha](https://www.google.com/recaptcha)
