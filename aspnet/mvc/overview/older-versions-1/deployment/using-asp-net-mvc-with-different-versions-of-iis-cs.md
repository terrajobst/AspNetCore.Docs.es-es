---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Usar ASP.NET MVC con distintas versiones de IIS (C#) | Microsoft Docs
author: microsoft
description: En este tutorial, aprenderá a usar ASP.NET MVC y el enrutamiento de direcciones URL, con diferentes versiones de Internet Information Services. Aprenda estrategias diferentes...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: aa7d00c0f54212d495f48929ed2a453942a1ed7d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829801"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Usar ASP.NET MVC con distintas versiones de IIS (C#)
====================
por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá a usar ASP.NET MVC y el enrutamiento de direcciones URL, con diferentes versiones de Internet Information Services. Aprenda estrategias distintas para usar ASP.NET MVC con IIS 7.0 (modo clásico), IIS 6.0 y versiones anteriores de IIS.


El marco de MVC de ASP.NET depende de enrutamiento de ASP.NET para enrutar las solicitudes del explorador a acciones del controlador. Para sacar provecho de enrutamiento de ASP.NET, es posible que deba realizar pasos de configuración adicionales en el servidor web. Todo depende de la versión de Internet Information Services (IIS) y el modo de la aplicación de procesamiento de solicitudes.

Este es un resumen de las distintas versiones de IIS:

- IIS 7.0 (modo integrado) - ninguna configuración especial necesaria para usar el enrutamiento de ASP.NET.
- IIS 7.0 (modo clásico): debe realizar una configuración especial para usar el enrutamiento de ASP.NET.
- IIS 6.0 o a continuación: tiene que realizar cualquier configuración especial para usar el enrutamiento de ASP.NET.

La versión más reciente de IIS es la versión 7.5 (en Win7). IIS 7 de IIS se incluye con Windows Server 2008 y VISTA SP1 y superior. También puede instalar IIS 7.0 en cualquier versión del sistema operativo Vista excepto Home Basic (vea [ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7.0 admite dos modos de procesamiento de solicitudes. Puede usar el modo integrado o en modo clásico. No es necesario realizar ningún paso de configuración especial cuando se usa IIS 7.0 en modo integrado. Sin embargo, es necesario realizar configuración adicional al utilizar IIS 7.0 en modo clásico.

Microsoft Windows Server 2003 incluye IIS 6.0. No puede actualizar IIS 6.0 a IIS 7.0, cuando se usa el sistema operativo Windows Server 2003. Debe realizar pasos de configuración adicionales cuando se usa IIS 6.0.

Microsoft Windows XP Professional incluye IIS 5.1. Debe realizar pasos de configuración adicionales cuando se usa IIS 5.1.

Por último, Microsoft Windows 2000 y Microsoft Windows 2000 Professional incluye IIS 5.0. Debe realizar pasos de configuración adicionales cuando se usa IIS 5.0.

## <a name="integrated-versus-classic-mode"></a>Integrado en comparación con el modo clásico

IIS 7.0 puede procesar las solicitudes mediante dos modos de procesamiento de solicitudes diferente: integrado y clásico. El modo integrado de proporciona un mejor rendimiento y más características. El modo clásico se incluye por razones de compatibilidad con versiones anteriores de IIS.

El modo de procesamiento de solicitud viene determinada por el grupo de aplicaciones. Puede determinar de qué modo de procesamiento está usando una aplicación web en particular al determinar el grupo de aplicaciones asociado a la aplicación. Siga estos pasos:

1. Inicie el Administrador de Internet Information Services
2. En la ventana conexiones, seleccione una aplicación
3. En la ventana de acciones, haga clic en el **configuración básica** vínculo para abrir el cuadro de diálogo Editar aplicación cuadro (consulte la figura 1)
4. Tome nota del grupo de aplicaciones seleccionado.

De forma predeterminada, IIS está configurado para admitir dos grupos de aplicaciones: **DefaultAppPool** y **Classic .NET AppPool**. Si se selecciona DefaultAppPool, la aplicación se está ejecutando en modo de procesamiento de solicitud integrada. Si se selecciona Classic .NET AppPool, la aplicación se ejecuta en modo de procesamiento de solicitudes clásico.

[![El cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Figura 1**: detectar el modo de procesamiento de solicitud ([haga clic aquí para ver imagen en tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Tenga en cuenta que puede modificar el modo de procesamiento de solicitudes en el cuadro de diálogo Editar aplicación. Haga clic en el botón de selección y cambie el grupo de aplicaciones asociado a la aplicación. Tenga en cuenta que hay problemas de compatibilidad al cambiar una aplicación ASP.NET del modelo clásico al modo integrado. Para obtener más información, vea los artículos siguientes:

- Actualización de ASP.NET 1.1 a IIS 7.0 en Windows Vista y Windows Server 2008: [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integración de ASP.NET con IIS 7.0: [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Si una aplicación de ASP.NET utiliza DefaultAppPool, no es necesario realizar ningún paso adicional para obtener el enrutamiento de ASP.NET (y, por tanto, ASP.NET MVC) para que funcione. Sin embargo, si la aplicación de ASP.NET está configurada para usar Classic .NET AppPool, a continuación, siga leyendo, tiene más trabajo que hacer.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Usar ASP.NET MVC con versiones anteriores de IIS

Si necesita usar ASP.NET MVC con una versión anterior de IIS a IIS 7.0 o deba utilizar IIS 7.0 en modo clásico, tiene dos opciones. En primer lugar, puede modificar la tabla de rutas para usar las extensiones de archivo. Por ejemplo, en lugar de solicitar una dirección URL como /Store/Details, podría solicitar una dirección URL como /Store.aspx/Details.

La segunda opción es crear algo llamado un *asignación de script comodín*. Una asignación de script comodín permite asignar todas las solicitudes en el marco de ASP.NET.

Si no tiene acceso al servidor web (por ejemplo, su ASP.NET MVC aplicación hospedada por un proveedor de servicios de Internet), a continuación, deberá usar la primera opción. Si no desea modificar la apariencia de las direcciones URL y tener acceso al servidor web, a continuación, puede usar la segunda opción.

Exploramos cada opción en detalle en las secciones siguientes.

## <a name="adding-extensions-to-the-route-table"></a>Agregar extensiones a la tabla de rutas

La manera más fácil de obtener el enrutamiento de ASP.NET para que funcione con versiones anteriores de IIS es modificar la tabla de rutas en el archivo Global.asax. El valor predeterminado y el archivo Global.asax sin modificar en el listado 1 configura una ruta con el nombre de la ruta predeterminada.

**Listado 1 - Global.asax (sin cambios)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

La ruta predeterminada configurada en el listado 1 permite a las direcciones URL de ruta que tienen un aspecto similar al siguiente:

/ Home/Index

/ Product/detalles/3

O producto

Lamentablemente, las versiones anteriores de IIS no pasan estas solicitudes para el marco de ASP.NET. Por lo tanto, estas solicitudes no se enrutan a un controlador. Por ejemplo, si se realiza una solicitud de explorador para el índice de/Home de dirección URL, a continuación, obtendrá la página de error en la figura 2.

[![El cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Figura 2**: recibir un error 404 no encontrado ([haga clic aquí para ver imagen en tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Versiones anteriores de IIS solo asignan algunas solicitudes para el marco de ASP.NET. La solicitud debe ser para una dirección URL con la extensión de archivo de la derecha. Por ejemplo, una solicitud para /SomePage.aspx se asigna al marco de ASP.NET. Sin embargo, una solicitud para /SomePage.htm no lo hace.

Por lo tanto, para configurar el enrutamiento de ASP.NET para que funcione, se debe modificar la ruta predeterminada para que incluya una extensión de archivo que se asigna al marco de ASP.NET.

Esto se realiza mediante un script denominado `registermvc.wsf`. Se incluye con la versión 1 de ASP.NET MVC en `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, pero a partir del 2 de ASP.NET este script se ha movido a ASP.NET Futures, disponible en [ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978).

Ejecuta este script registra una nueva extensión .mvc con IIS. Después de registrar la extensión .mvc, puede modificar las rutas en el archivo Global.asax para que las rutas de usar la extensión .mvc.

El archivo Global.asax modificado en el listado 2 funciona con versiones anteriores de IIS.

**Listado 2 - Global.asax (modificado con las extensiones)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Importante**: no olvide crear una aplicación de MVC de ASP.NET nuevo después de modificar el archivo Global.asax.

Hay dos cambios importantes en el archivo Global.asax en el listado 2. Ahora hay dos rutas definidas en Global.asax. Patrón de URL de la ruta predeterminada, la primera ruta, ahora es similar a:

{controller}.mvc/{action}/{id}

La adición de la extensión .mvc cambia el tipo de archivos que intercepta el módulo de enrutamiento de ASP.NET. Con este cambio, la aplicación de ASP.NET MVC ahora enruta las solicitudes similar al siguiente:

/Home.Mvc/index/

/Product.Mvc/Details/3

/Product.Mvc/

La segunda ruta, la ruta raíz, es nueva. Este patrón de URL de la ruta raíz es una cadena vacía. Esta ruta es necesaria para la coincidencia de las solicitudes realizadas en la raíz de la aplicación. Por ejemplo, la ruta raíz coincidirá con una solicitud que tiene este aspecto:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Después de realizar estas modificaciones en la tabla de rutas, deberá asegurarse de que todos los vínculos en la aplicación son compatibles con estos nuevos patrones de dirección URL. En otras palabras, asegúrese de que todos los vínculos de incluyen la extensión .mvc. Si usa el método auxiliar Html.ActionLink() para generar los vínculos, a continuación, no es necesario realizar ningún cambio.

En lugar de la secuencia de comandos registermvc.wcf, puede agregar una nueva extensión para IIS que se asigna a mano para el marco de ASP.NET. Al agregar una nueva extensión, asegúrese de que la casilla de verificación con la etiqueta **Compruebe que el archivo existe** no está activada.

## <a name="hosted-server"></a>Servidor hospedado

No siempre tendrá acceso al servidor web. Por ejemplo, si va a hospedar la aplicación de ASP.NET MVC mediante un proveedor de hospedaje de Internet, no tendrá necesariamente acceso a IIS.

En ese caso, debe usar una de las extensiones de archivo existentes que están asignadas para el marco de ASP.NET. El .aspx, .axd y .ashx extensiones son ejemplos de extensiones de archivo asignadas a ASP.NET.

Por ejemplo, el archivo Global.asax modificado en el listado 3 utiliza la extensión .aspx en lugar de la extensión .mvc.

**Listado 3 - Global.asax (modificado con las extensiones de aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

El archivo Global.asax en el listado 3 es exactamente el mismo que el archivo Global.asax anterior con la salvedad de que usa la extensión .aspx en lugar de la extensión .mvc. No tienes que realizar ninguna configuración en el servidor web remoto para usar la extensión de aspx.

## <a name="creating-a-wildcard-script-map"></a>Creación de una asignación de Script comodín

Si no desea modificar las direcciones URL de la aplicación de ASP.NET MVC y tener acceso al servidor web, tiene una opción adicional. Puede crear una asignación de script comodín que se asigna a todas las solicitudes al servidor web para el marco de ASP.NET. De este modo, puede usar el valor predeterminado de tabla de rutas de ASP.NET MVC con IIS 7.0 (en el modo clásico) o IIS 6.0.

Tenga en cuenta que esta opción hace que IIS interceptar cada solicitud realizada en el servidor web. Esto incluye las solicitudes de imágenes, páginas ASP clásicas y páginas HTML. Por lo tanto, lo que permite un carácter comodín asignación de script a ASP.NET tienen implicaciones de rendimiento.

Le mostramos cómo habilitar una asignación de script comodín para IIS 7.0:

1. Seleccione la aplicación en la ventana Conexiones
2. Asegúrese de que el **características** seleccionar vista
3. Haga doble clic en el **asignaciones de controlador** botón
4. Haga clic en el **Agregar asignación de Script comodín** vincular (consulte la figura 3)
5. Escriba la ruta de acceso a aspnet\_archivo isapi.dll (puede copiar esta ruta de acceso de la asignación de script PageHandlerFactory)
6. Escriba el nombre de MVC
7. Haga clic en el **Aceptar** botón

[![El cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Figura 3**: creación de una asignación de script comodín con IIS 7.0 ([haga clic aquí para ver imagen en tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Siga estos pasos para crear una asignación de script comodín con IIS 6.0:

1. Haga clic en un sitio Web y seleccione Propiedades
2. Seleccione el **Home Directory** ficha
3. Haga clic en el **configuración** botón
4. Seleccione el **asignaciones** ficha
5. Haga clic en el **insertar** (consulte la figura 4)
6. Pegue la ruta de acceso aspnet\_isapi.dll en el campo ejecutable (puede copiar esta ruta de acceso de la asignación de script para archivos .aspx)
7. Desactive la casilla **Compruebe que el archivo existe**
8. Haga clic en el **Aceptar** botón

[![El cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Figura 4**: creación de una asignación de script comodín con IIS 6.0 ([haga clic aquí para ver imagen en tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Después de habilitar asignaciones de secuencia de comandos de comodines, deberá modificar la tabla de rutas en el archivo Global.asax para que incluya una ruta raíz. En caso contrario, aparecerá la página de error en la figura 5 cuando se realiza una solicitud para la página raíz de la aplicación. Puede usar el archivo Global.asax modificado en el listado 4.

[![El cuadro de diálogo nuevo proyecto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Figura 5**: error de la ruta raíz que faltan ([haga clic aquí para ver imagen en tamaño completo](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Listado 4 - Global.asax (modificado con la ruta raíz)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Después de habilitar una asignación de script comodín para IIS 7.0 o IIS 6.0, puede realizar las solicitudes que funcionan con la tabla de rutas predeterminadas que tienen un aspecto similar al siguiente:

/

/ Home/Index

/ Product/detalles/3

O producto

## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo puede usar ASP.NET MVC cuando se usa una versión anterior de IIS (o IIS 7.0 en modo clásico). Se describen dos métodos de obtención de enrutamiento de ASP.NET para que funcione con versiones anteriores de IIS: modificar la tabla de enrutamiento predeterminada o crear una asignación de script comodín.

La primera opción, deberá modificar las direcciones URL usadas en la aplicación de ASP.NET MVC. Una ventaja muy significativa de esta primera opción es que no necesita acceso a un servidor web para modificar la tabla de rutas. Esto significa que puede usar esta opción incluso al hospedar su aplicación ASP.NET MVC con un Internet empresa de hospedaje.

La segunda opción es crear una asignación de script comodín. La ventaja de esta segunda opción es que no es necesario modificar las direcciones URL. La desventaja de esta segunda opción es que puede afectar el rendimiento de su aplicación ASP.NET MVC.

> [!div class="step-by-step"]
> [Siguiente](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
