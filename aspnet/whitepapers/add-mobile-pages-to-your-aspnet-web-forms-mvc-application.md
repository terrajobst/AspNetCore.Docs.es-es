---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: "Cómo: Agregar páginas de dispositivos móviles a los formularios de ASP.NET Web / aplicación MVC | Documentos de Microsoft"
author: rick-anderson
description: "Este procedimiento se describe varias maneras de servir páginas optimizadas para dispositivos móviles desde los formularios Web Forms de ASP.NET / aplicación de MVC y sugiere arquitectónicos y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: aac359b26c508784793a67260dc2e65c30db687a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Cómo: Agregar páginas de dispositivos móviles a los formularios de ASP.NET Web / aplicación de MVC
====================
> **Se aplica a**
> 
> - Versión de formularios Web Forms ASP.NET 4.0
> - ASP.NET MVC versión 3.0
> 
> **Resumen**
> 
> Este procedimiento se describe varias maneras de servir páginas optimizadas para dispositivos móviles desde los formularios Web Forms de ASP.NET / aplicación de MVC y se sugieren arquitectura y diseño cuestiones a considerar cuando se destinan a una amplia gama de dispositivos. Este documento también explica por qué los controles de ASP.NET Mobile de ASP.NET 2.0 a la versión 3.5 están obsoletos y se tratan algunas alternativas modernas.


## <a name="contents"></a>Contenido

- Información general
- Opciones de arquitectura
- Detección de explorador y del dispositivo
- Cómo las aplicaciones de formularios Web Forms de ASP.NET pueden presentar páginas específicas de mobile
- Cómo las aplicaciones de ASP.NET MVC pueden presentar páginas específicas de mobile
- Recursos adicionales

Para obtener ejemplos de código descargables que muestra las técnicas de este documento para ASP.NET Web Forms y MVC, vea [sitios con ASP.NET y aplicaciones móviles](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Información general

Dispositivos móviles – smartphones, característica teléfonos y tabletas – continuarán aumentando en popularidad a fin de obtener acceso a la Web. Para muchos desarrolladores de web y las empresas orientados a web, esto significa que es cada vez más importante proporcionar una buena experiencia de exploración para los visitantes que utilizan los dispositivos.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Cómo anteriores versiones de exploradores móviles compatibles con ASP.NET

Versiones ASP.NET 2.0 a 3.5 incluye *controles de ASP.NET Mobile*: un conjunto de controles de servidor para dispositivos móviles en el *System.Web.Mobile.dll* ensamblado y el  *En System.Web.UI.MobileControls* espacio de nombres. El ensamblado aún se incluye en ASP.NET 4, pero está en desuso. Se recomienda a los desarrolladores para migrar a los enfoques más modernos, tales como los descritos en este documento.

El motivo por qué controles de ASP.NET Mobile se han marcado como obsoleto es que su diseño está orientada a servicios alrededor de los teléfonos móviles que son comunes en 2005 y versiones anteriores. Los controles están diseñados principalmente para representar el marcado WML o cHTML (en lugar de HTML regular) para los exploradores WAP de ese era. Pero WAP, WML y cHTML ya no son relevantes para la mayoría de los proyectos, porque HTML se ha convertido en el lenguaje de marcado generalizado para los exploradores de escritorio y móviles similares.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Los desafíos de admitir dispositivos móviles de hoy en día

Aunque los exploradores móviles ahora casi universalmente compatible con HTML, todavía se enfrentará muchos desafíos cuando el objetivo sea para crear excelentes experiencias de exploración móviles:

- ***Tamaño de la pantalla*** : los dispositivos móviles varían considerablemente en el formulario y sus pantallas suelen ser mucho menor que los monitores de escritorio. Por lo tanto, necesitará crear diseños de página completamente distinta para ellos.
- ***Métodos de entrada*** : algunos dispositivos tienen teclados, algunas tienen fonocaptores, otros usuarios usan un toque. Debe tener en cuenta varios casos de navegación de datos y mecanismos de métodos de entrada.
- ***Cumplimiento de estándares*** – muchos exploradores móviles no son compatibles con los estándares más recientes de HTML, CSS o JavaScript.
- ***Ancho de banda*** : rendimiento de la red de datos móviles varía significativamente y algunos usuarios finales son en tarifas que cobran según en megabytes.

No hay ninguna solución única; la aplicación tendrá que buscar y comportarse de forma diferente según el dispositivo de tener acceso a ella. Dependiendo de qué nivel de compatibilidad con dispositivos móvil desea, puede ser un desafío mucho mayor para los desarrolladores web que alguna vez era el escritorio "guerras explorador".

Los desarrolladores llegando al soporte técnico de explorador móvil por primera vez a menudo inicialmente pensar solo es importante admitir los smartphones más sofisticados y más recientes (p. ej., Windows Phone 7, iPhone o Android), quizás porque los programadores a menudo personal poseen como dispositivos. Sin embargo, teléfonos más baratos siguen siendo muy populares y sus propietarios usarlos para navegar por la red, especialmente en los países donde teléfonos móviles son más fáciles obtener acceso a una conexión de banda ancha. Deberá decidir qué variedad de dispositivos para admitir teniendo en cuenta sus clientes es probable que su negocio. Si va a compilar un folleto en línea para un spa de mantenimiento de lujo, podría tomar una decisión de negocio solo como destino avanzadas smartphones, mientras que si está creando un sistema de vales de reserva para un cine, probablemente necesitará tener en cuenta para los visitantes con menos eficaz característica teléfonos.

## <a name="architectural-options"></a>Opciones de arquitectura

Antes de entrar en los detalles técnicos específicos de formularios Web Forms de ASP.NET o MVC, tenga en cuenta que los desarrolladores web por lo general tienen tres opciones posibles principales para admitir exploradores móviles:

1. ***No hacer nada:*** simplemente puede crear una aplicación web estándar, orientada a servicios de escritorio y se basan en exploradores móviles para representar un rendimiento aceptable. 

    - **Ventaja**: es la opción más barata para implementar y mantener: extra ya no funcionan
    - **Desventaja**: proporciona la experiencia del usuario final peor: 

        - Los smartphones más recientes puede provocar que el código HTML tan válido como un explorador de escritorio, pero todavía se verán obligados a los usuarios para aplicar zoom y desplazarse horizontalmente y verticalmente para consumir el contenido Web en una pantalla pequeña. Esto está muy alejado del óptimo.
        - Los dispositivos más antiguos y los teléfonos de función pueden producir un error al representar el marcado de forma satisfactoria.
        - Incluso en los dispositivos de tableta más recientes (cuyas pantallas pueden ser tan grandes como las pantallas de portátil), se aplican reglas diferentes interacción. Entrada basada en táctil funciona mejor con botones más grandes y vincula spread más alejado, y no hay ninguna manera de mantener un cursor del mouse sobre un menú emergente.
2. ***Resolver el problema en el cliente* :** con cuidado de uso de CSS y [mejora progresiva](http://en.wikipedia.org/wiki/Progressive_enhancement) puede crear marcado, estilos y scripts que se adapten a cualquier explorador está ejecutando. Por ejemplo, con [consultas de medios de CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), puede crear un diseño de varias columna que se convertirá en un diseño de columna única en los dispositivos cuyas pantallas son más estrechas que un umbral seleccionado. 

    - **Ventaja**: optimiza la representación para el dispositivo específico en uso, incluso para futuros dispositivos desconocidos según cualquier pantalla y los datos de entrada características tienen
    - **Ventaja**: fácilmente le permite compartir la lógica de servidor a través de todos los tipos de dispositivo mínima duplicación de código o el esfuerzo
    - **Desventaja**: los dispositivos móviles son tan diferentes de dispositivos de escritorio que realmente puede desea que las páginas móviles sea completamente diferente de las páginas de escritorio, que muestra una información diferente. Estas variaciones pueden ser ineficaces o imposibles de conseguir Fortalezca de forma a través de CSS por sí sola, especialmente teniendo en cuenta cómo forma incoherente los dispositivos más antiguos interpretan las reglas de CSS. Esto es especialmente cierto de atributos CSS 3.
    - **Desventaja**: no proporciona compatibilidad con distintos lógica de servidor y los flujos de trabajo para diferentes dispositivos. Por ejemplo, no es posible, implementar una simplificada compra carro desprotección flujo de trabajo de usuarios móviles por medio de CSS por sí sola.
    - **Desventaja**: uso de ancho de banda ineficaz. Es posible que el servidor deba transmitir el marcado y los estilos que se aplican a todos los dispositivos posibles, aunque el dispositivo de destino sólo utilizará un subconjunto de dicha información.
3. ***Solucionar el problema en el servidor* :** si el servidor sabe cuál es el dispositivo tiene acceso a ella, o al menos las características de dichos dispositivos, como el tamaño de la pantalla y el método de entrada, y si es un dispositivo móvil: se puede ejecutar una lógica diferente y marcado HTML diferente de salida. 

    - **Ventaja:** flexibilidad máxima. No hay ningún límite en cuanto al cuánto puede variar la lógica del lado del servidor para dispositivos móviles u optimizar el marcado para el diseño deseado, específicos del dispositivo.
    - **Ventaja:** uso eficaz del ancho de banda. Solo necesita transmitir el marcado y la información de estilo que el dispositivo de destino se va a usar.
    - **Inconvenientes:** en ocasiones fuerza la repetición del trabajo o el código (por ejemplo, lo que crea copias similares pero ligeramente distintos de las páginas de formularios Web Forms o las vistas MVC). Donde sea posible que repercutirán comunes lógica de cierre en una capa subyacente o servicio, pero aún así, algunas partes del código de interfaz de usuario o de marcado puede tener duplicados y, a continuación, se mantiene en paralelo.
    - **Inconvenientes:** detección del dispositivo no es trivial. Se requiere una lista o una base de datos de tipos de dispositivos conocidos y sus características (que no sea siempre perfectamente actualizadas) y garantiza que no coincidan exactamente con cada solicitud entrante. Este documento describe algunas opciones y los riesgos más adelante.

Para obtener los mejores resultados, la mayoría de los desarrolladores encuentran que necesitan para combinar opciones (2) y (3). Pequeñas diferencias de estilos se implementan mejor en el cliente mediante CSS o JavaScript incluso, mientras que las principales diferencias en datos, flujo de trabajo o marcado más eficazmente se implementa en el código de servidor.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Este documento se centra en las técnicas de servidor

Puesto que ASP.NET Web Forms y MVC son ambas tecnologías principalmente en el servidor, estas notas del producto se centrarán en las técnicas de servidor que le permiten generar un marcado diferente y la lógica para los exploradores móviles. Por supuesto, también puede combinar esto con cualquier técnica de cliente (p. ej., CSS 3 consultas de medios, mejora progresiva JavaScript), pero es más una cuestión de diseño web de desarrollo de ASP.NET.

## <a name="browser-and-device-detection"></a>Detección de explorador y del dispositivo

El requisito previo de clave para todas las técnicas de servidor para admitir dispositivos móviles consiste en conocer cuál es el dispositivo está usando el visitante. De hecho, incluso mejor que conocer el número de fabricante y modelo de dichos dispositivos es saber la *características* del dispositivo. Pueden incluir características:

- ¿Es un dispositivo móvil?
- Método de entrada (mouse, teclado, táctil, teclado, joystick,...)
- Tamaño de la pantalla (físicamente y en píxeles)
- Formatos admitidos de medios y los datos
- Etcetera.

Es mejor tomar decisiones basadas en características que el número de modelo, ya que, a continuación, estará mejor preparado para controlar futuros dispositivos.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Uso de ASP. Compatibilidad con la detección de red explorador integrado

Los desarrolladores de formularios Web Forms ASP.NET y MVC inmediatamente pueden detectar características importantes de un explorador visitando mediante la inspección de propiedades de la *Request.Browser* objeto. Por ejemplo, vea

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- .. .y muchos otros

En segundo plano, la plataforma ASP.NET coincide con la entrada *User-Agent* encabezado HTTP (UAC) con expresiones regulares en un conjunto de archivos XML de definición de explorador. De forma predeterminada, la plataforma incluye definiciones para varios dispositivos móviles comunes, y puede agregar archivos de definición de explorador personalizados para que otros usuarios que se va a reconocer. Para obtener más información, vea la página MSDN [controles de servidor Web de ASP.NET y las funciones del explorador](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Uso de la base de datos del dispositivo WURFL a través de 51Degrees.mobi Foundation

Mientras que ASP. Compatibilidad con la detección de explorador integrado del NET será suficiente para muchas aplicaciones, hay dos casos principales que quizás no sea suficiente:

- ***Desea reconocer los dispositivos más recientes***(sin necesidad de crear manualmente los archivos de definición de explorador para ellos). Tenga en cuenta que los archivos de definición de explorador del 4 de .NET no son suficientemente recientes como para reconocer iPad de Apple, teléfonos Android, exploradores Opera Mobile o Windows Phone 7.
- ***Se necesita información más detallada sobre las capacidades del dispositivo***. Puede que necesite saber acerca de métodos de entrada de un dispositivo (por ejemplo, el teclado de vs táctil) o el audio formatea el explorador admite. Esta información no está disponible en los archivos de definición de explorador estándar.

El [ *archivo de recursos Universal Wireless* proyecto (WURFL)](http://wurfl.sourceforge.net/) mantiene mucho más actualizada y detallada información sobre dispositivos móviles en uso actualmente.

Lo bueno para los desarrolladores de .NET es que ASP. Característica de detección de explorador del NET es extensible, por lo que es posible mejorar para solucionar estos problemas. Por ejemplo, puede agregar el código abierto [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) biblioteca al proyecto. Es un IHttpModule de ASP.NET o un explorador capacidades de proveedor (se puede usar en aplicaciones de formularios Web Forms y MVC), que lee los datos WURFL directamente y lo enlaza en ASP. Mecanismo de detección de explorador integrado del NET. Una vez haya instalado el módulo, *Request.Browser* repentinamente contendrá información mucho más precisa y detallada: correctamente reconocerá muchos de los dispositivos que se ha mencionado previamente y se lista sus capacidades (incluidos funcionalidades adicionales como método de entrada). Consulte la documentación del proyecto para obtener más detalles.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Cómo las aplicaciones de formularios Web Forms pueden presentar páginas específicas de mobile

De forma predeterminada, mostramos cómo muestra una nueva aplicación de formularios Web Forms en dispositivos móviles comunes:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Claramente, ni diseño parece muy preparado para dispositivos móviles: esta página se diseñó para un monitor grande, orientados a horizontal, no para una pantalla pequeña orientación vertical. ¿Qué puede hacer sobre él?

Según lo descrito anteriormente en este documento, hay muchas maneras de personalizar las páginas para dispositivos móviles. Algunas técnicas se basan en el servidor, otros ejecutan en el cliente.

### <a name="creating-a-mobile-specific-master-page"></a>Crear una página maestra específica para equipos móviles

Según sus requisitos, es posible que pueda usar los mismos formularios Web para todos los visitantes, pero tener instalados dos páginas maestras: uno para los visitantes del escritorio y otro para los visitantes móviles. Esto le da la flexibilidad de cambiar el nivel superior marcado HTML para adaptarlo a dispositivos móviles, sin necesidad de duplicar cualquier lógica de la página o la hoja de estilos CSS.

Esto es muy fácil de hacer. Por ejemplo, puede agregar un controlador de PreInit como el siguiente a un formulario Web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Ahora, cree una página maestra denominada Mobile.Master en la carpeta de nivel superior de la aplicación y se usará cuando se detecta un dispositivo móvil. La página maestra móvil puede hacer referencia a una hoja de estilos CSS específica para equipos móviles si es necesario. Los visitantes del escritorio seguirán viendo la página maestra predeterminada, no lo móvil.

### <a name="creating-independent-mobile-specific-web-forms"></a>Crear independientes de formularios Web Forms móviles específica

Para obtener la máxima flexibilidad, puede ir mucho más allá del hecho de tener diferentes páginas maestras para distintos tipos de dispositivos. Puede implementar dos *totalmente separar conjuntos de páginas de formularios Web Forms* : uno establecido para los exploradores de escritorio, otro conjunto para dispositivos móviles. Esto funciona mejor si desea presentar información muy diferente o flujos de trabajo a los visitantes móviles. El resto de esta sección describe este enfoque en detalle.

Suponiendo que ya tenga una aplicación de formularios Web Forms diseñada para los exploradores de escritorio, la manera más fácil para continuar es crear una subcarpeta denominada "Móvil" dentro del proyecto y crear páginas móviles no existe. Puede crear todo un sitio secundario, con su propio páginas maestras, las hojas de estilos y páginas, utilizando las mismas técnicas que le gustaría usar para ninguna otra aplicación de formularios Web Forms. No es necesario generar un equivalente para móviles *cada* página en el sitio de escritorio; puede elegir qué subconjunto de funcionalidad que tenga sentido para los visitantes móviles.

Las páginas móviles pueden compartir recursos estáticos comunes (como imágenes, JavaScript o CSS archivos) con las páginas normales si lo desea. Puesto que la carpeta "Móvil" le *no* marcarse como una aplicación independiente cuando se hospeda en IIS (es simplemente una subcarpeta simple de páginas de formularios Web Forms), también compartirán las mismas configuración, los datos de sesión y otras infraestructuras como los páginas de escritorio.

> [!NOTE]
> Puesto que este enfoque normalmente implica algunos duplicación de código (páginas de dispositivos móviles están probable que comparten algunas similitudes con páginas de escritorio), es importante factor cualquier comunes business lógica o datos de acceso a código en una capa subyacente compartida o un servicio. En caso contrario, deberá doble el esfuerzo de la creación y mantenimiento de la aplicación.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Redireccionar a los visitantes móviles a las páginas móviles

A menudo resulta cómodo redirigir los visitantes móviles a las páginas móviles sólo en el *primer* en su sesión de exploración (y no en todas las solicitudes en su sesión), una solicitud porque:

- Fácilmente, a continuación, puede permitir que los visitantes móviles tener acceso a las páginas de escritorio si lo desean, simplemente ha colocado un vínculo en la página maestra que se va a "Versión de escritorio". El visitante no se redirige a una página móvil, porque ya no es la primera solicitud en su sesión.
- Evita el riesgo de interferir con las solicitudes para cualquier recurso dinámico compartido entre las partes de escritorio y móviles de su sitio (por ejemplo, si tiene un formulario Web Forms comunes que partes de escritorio y móviles de su sitio se muestran en un IFRAME, así como ciertos controladores de Ajax)

Para ello, puede colocar la lógica de redirección en una **sesión\_iniciar** método. Por ejemplo, agregue el método siguiente al archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurar la autenticación de formularios para respetar las páginas móviles

Tenga en cuenta que la autenticación de formularios hace ciertas suposiciones sobre dónde puede redirigir a los visitantes durante y después del proceso de autenticación:

- Cuando un usuario necesita autenticarse, autenticación de formularios se redirigirá a la página de inicio de sesión de escritorio, independientemente de si un usuario de escritorio o portátil (porque solo tiene un concepto de *una* dirección URL de inicio de sesión). Suponiendo que desea aplicar el estilo de la página de inicio de sesión móvil diferente, debe mejorar su página de inicio de sesión de escritorio para que los usuarios móviles redirige a una página de inicio de sesión móvil independiente. Por ejemplo, agregue el código siguiente a su **desktop** código de página de inicio de sesión en segundo plano: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Después de que un usuario inicia sesión correctamente, la autenticación de formularios de forma predeterminada redirigirá a la página principal del escritorio (porque solo tiene un concepto de *una* página predeterminada). Necesario mejorar su página de inicio de sesión móvil para que redirige a la página principal de mobile después de un registro correcto. Por ejemplo, agregue el código siguiente a su **móvil** código de página de inicio de sesión en segundo plano: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
 Este código supone que la página tiene un control de servidor de inicio de sesión llamado LoginUser, como se muestra en la plantilla de proyecto de forma predeterminada.

### <a name="working-with-output-caching"></a>Trabajar con la caché de resultados

Si está usando la caché de resultados, tenga en cuenta que de forma predeterminada, es posible que un usuario de escritorio visitar una ciertas URL (lo que provoca la salida se almacene en caché), seguido por un usuario móvil que, a continuación, recibe la salida almacenada en caché de escritorio. Esta advertencia se aplica si solo está varía la página maestra por tipo de dispositivo, o implementar totalmente independiente formularios Web Forms por tipo de dispositivo.

Para evitar el problema, puede indicar a ASP.NET para modificar la entrada de caché según si el visitante usa un dispositivo móvil. Agregue un parámetro VaryByCustom a la declaración de directiva OutputCache de la página de la manera siguiente:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

A continuación, defina *isMobileDevice* como una memoria caché personalizada invalidar parámetro agregando el siguiente método al archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Esto garantizará que los visitantes móviles a la página no reciben el resultado guardado previamente en la memoria caché un visitante de escritorio.

### <a name="a-working-example"></a>Obtener un ejemplo funcional

Para ver estas técnicas en acción, descargue [ejemplos de código del este documento](https://docs.microsoft.com/aspnet/mobile/overview). La aplicación de ejemplo de formularios Web Forms redirige automáticamente los usuarios móviles a un conjunto de páginas móviles específicos en una subcarpeta denominada Mobile. El marcado y un estilo de esas páginas se optimizan mejor para los exploradores móviles, como puede ver en las siguientes capturas de pantalla:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Para obtener más sugerencias sobre cómo optimizar el marcado y CSS para los exploradores móviles, consulte la sección "Aplicación de estilos móviles páginas para los exploradores móviles" más adelante en este documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Cómo las aplicaciones de ASP.NET MVC pueden presentar páginas específicas de mobile

Puesto que el modelo Model-View-Controller separa la lógica de la aplicación (en los controladores) desde la lógica de presentación (en las vistas), puede elegir desde cualquiera de los métodos siguientes para controlar la compatibilidad con dispositivos móvil en el código de servidor:

1. ***Usar los mismos controladores y vistas para los exploradores de escritorio y móviles, pero representar las vistas con distintos diseños de Razor según el tipo de dispositivo *.** Esta opción funciona mejor si muestra datos idénticos en todos los dispositivos, pero simplemente desee proporcionar diferentes hojas de estilos CSS o cambiar algunos elementos HTML de nivel superior para dispositivos móviles.
2. ***Usar los mismos controladores para los exploradores de escritorio y móviles, pero presentar vistas distintas según el tipo de dispositivo***. Esta opción funciona mejor si se está mostrando aproximadamente los mismos datos y proporcionar los mismos flujos de trabajo para los usuarios finales, pero desea presentar HTML muy diferente para satisfacer el dispositivo que se va a usar.
3. ***Crear áreas bien diferenciadas para los exploradores de escritorio y móviles, implementar controladores independientes y vistas para cada *.** Esta opción funciona mejor si está mostrar pantallas muy diferentes, que contiene información diferente y a la izquierda al usuario a través de diferentes flujos de trabajo optimizada para su tipo de dispositivo. Puede significar algunos repetición del código, pero puede minimizarlo factorización lógica común en una capa o un servicio subyacente.

Si desea trasladar el **primer** opción y variar el diseño de Razor sólo por tipo de dispositivo, es muy fácil. Sólo tiene que modificar su \_ViewStart.cshtml de archivos como se indica a continuación:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Ahora puede crear un diseño específico de mobile denominado \_LayoutMobile.cshtml con una estructura de página y CSS reglas optimizada para dispositivos móviles.

Si desea trasladar el **segundo** opción totalmente diferentes vistas de presentación según el tipo de dispositivo del visitante, consulte [entrada de blog de Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

El resto de este documento se centra en la **tercer** opción: crear controladores separados *y* vistas para dispositivos móviles: para poder controlar exactamente qué subconjunto de funcionalidad se ofrece para los visitantes móviles.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configuración de un área móvil dentro de la aplicación de ASP.NET MVC

Puede agregar un área denominada "Móvil" para una aplicación de MVC de ASP.NET existente en la forma normal: haga doble clic en el nombre del proyecto en el Explorador de soluciones y después elija Agregar à área. A continuación, puede agregar controladores y vistas como lo haría con cualquier otra área dentro de una aplicación de ASP.NET MVC. Por ejemplo, agregar a su área móvil un nuevo controlador denominado HomeController para actuar como una página de inicio para los visitantes móviles.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Garantizar la dirección URL /Mobile llega a la página principal de mobile

Si desea que la dirección URL /Mobile para llegar a la acción del índice en HomeController dentro de su área móvil, debe realizar dos pequeños cambios en la configuración de enrutamiento. En primer lugar, actualice la clase MobileAreaRegistration para que HomeController el controlador predeterminado en el área de dispositivos móvil, como se muestra en el código siguiente:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Esto significa la página principal de mobile ahora se ubicarán en /Mobile, en lugar de/Mobile/principal, ya que "Inicio" es ahora la implícitamente predeterminado nombre del controlador de páginas de dispositivos móviles.

A continuación, tenga en cuenta que al agregar un segundo HomeController a su aplicación (es decir, la móviles, además de escritorio uno existente), habrá divide la página principal del escritorio normal. Se producirá un error con el error "*se han encontrado varios tipos que coinciden con el controlador denominado 'Inicio'*". Para resolver este problema, actualice la configuración de enrutamiento de nivel superior (en Global.asax.cs) para especificar que el escritorio HomeController debería tener prioridad cuando hay ambigüedad:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Ahora el error irá ubicación y la dirección URL http://*su sitio*/ llegará a la página principal de escritorio y http://*su sitio*/mobile/ llegará a la página principal de mobile.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Redireccionar a los visitantes móviles a su área móvil

Hay muchos puntos de extensibilidad distintas en ASP.NET MVC, por lo que hay muchas maneras posibles de inyectar lógica de redirección. Una opción ordenada consiste en crear un atributo de filtro, [RedirectMobileDevicesToMobileArea], que realiza un redireccionamiento si se cumplen las condiciones siguientes:

1. Es la primera solicitud en la sesión del usuario (es decir, Session.IsNewSession es igual a true)
2. La solicitud proviene de un explorador móvil (por ejemplo, Request.Browser.IsMobileDevice es igual a true)
3. El usuario ya no solicita un recurso en el área de dispositivos móvil (es decir, el *ruta de acceso* parte de la dirección URL no comienza con /Mobile)

El ejemplo descargable incluido con este documento incluye una implementación de esta lógica. Se implementa como un filtro de autorización, derivado de AuthorizeAttribute, lo que significa que pueda funcionar correctamente incluso si está utilizando el almacenamiento en caché de salida (en caso contrario, si un visitante escritorio primera accede a una determinada dirección URL, la salida de escritorio podría se almacena en caché y, a continuación, se envía al visitantes móviles posteriores).

Ya que es un filtro, puede elegir cualquiera para aplicarlo a determinados controladores y acciones, por ejemplo,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… o puede aplicarla a todos los controladores y acciones como un MVC 3 *filtros globales* en el archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

El ejemplo descargable muestra también cómo puede crear subclases de este atributo que redirección a ubicaciones concretas dentro del área objeto móvil. Esto significa, por ejemplo, que puede:

- Registrar un filtro global tal como se muestra arriba que envía los visitantes móviles a la página principal móvil de forma predeterminada.
- Aplicar un filtro [RedirectMobileDevicesToMobileProductPage] especial a una acción de "producto vista" que toma los visitantes móviles a la versión móvil de cualquier página del producto que haya solicitado.
- También se aplican otras subclases especial del filtro para realizar determinadas acciones, redirigir los visitantes móviles a la página móvil equivalente

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurar la autenticación de formularios para respetar las páginas móviles

Si utiliza la autenticación de formularios, debe tener en cuenta que cuando un usuario necesita iniciar sesión, redirige automáticamente al usuario a una sola "iniciar sesión" dirección URL específica, cuyo valor predeterminado es **/Account/inicio de sesión**. Esto significa que los usuarios móviles se le redirige a la acción de escritorio "iniciar sesión".

Para evitar este problema, agregue lógica a la acción "iniciar sesión" del escritorio para que los usuarios móviles redirige de nuevo a una acción de "iniciar sesión" móviles. Si usa el valor predeterminado de plantilla de aplicación de ASP.NET MVC, actualice acción de inicio de sesión del AccountController como sigue:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… y, a continuación, implementar una acción de "iniciar sesión" específica para equipos móviles adecuado en un controlador denominado AccountController en su área móvil.

### <a name="working-with-output-caching"></a>Trabajar con la caché de resultados

Si usa el filtro [OutputCache], debe forzar la entrada de caché que varían según el tipo de dispositivo. Por ejemplo, escriba:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

A continuación, agregue el método siguiente al archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Esto garantizará que los visitantes móviles a la página no reciben el resultado guardado previamente en la memoria caché un visitante de escritorio.

### <a name="a-working-example"></a>Obtener un ejemplo funcional

Para ver estas técnicas en acción, descargue [código del este documento asociado ejemplos](https://docs.microsoft.com/aspnet/mobile/overview). El ejemplo incluye una aplicación de ASP.NET MVC 3 (Release Candidate) ha mejorado para admitir dispositivos móviles mediante los métodos descritos anteriormente.

## <a name="further-guidance-and-suggestions"></a>Obtener más orientación y sugerencias

La explicación siguiente se aplica tanto a los formularios Web Forms y los desarrolladores MVC que están utilizando las técnicas descritas en este documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Mejorar su lógica de redirección mediante 51Degrees.mobi Foundation

La lógica de redirección que se muestra en este documento puede ser perfectamente suficiente para la aplicación, pero no funcionará si tiene que deshabilitar las sesiones, o con los exploradores móviles que rechazan cookies (estos no pueden tener las sesiones), ya que no sabe si una solicitud determinada es la primera de ellas desde ese visitante.

Ya ha aprendido cómo el 51Degrees.mobi de código abierto Foundation puede mejorar la precisión de ASP. Detección del explorador de NET. También tiene una capacidad integrada para redirigir los visitantes móviles a ubicaciones específicas configurados en el archivo Web.config. Puede funcionar sin dependiendo de las sesiones de ASP.NET (y, por tanto, las cookies) mediante el almacenamiento temporal de registro de los valores hash de los encabezados HTTP y direcciones IP de los visitantes, de modo que sepa si es o no cada solicitud de la primera de ellas desde un vistor determinado.

El siguiente elemento agregado a la sección fiftyOne del archivo web.config redirigirá la primera solicitud desde un dispositivo móvil detectado a la página ~ / Mobile/Default.aspx. Las solicitudes a las páginas bajo la carpeta móvil le *no* redirigido, independientemente del tipo de dispositivo. Si el dispositivo móvil ha estado inactivo durante un período de 20 minutos o más el dispositivo se olvide de y las solicitudes posteriores se tratará como nuevos para los fines de la redirección.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Para obtener más información, consulte [51degrees.mobi documentación Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Se *puede* característica de redirección del uso 51Degrees.mobi Foundation en las aplicaciones de ASP.NET MVC, pero tendrá que definir la configuración de redirección en cuanto a direcciones URL sin formato, no en cuanto a los parámetros de enrutamientos o colocar filtros MVC en acciones. Esto es porque (en el momento de escribir) 51Degrees.mobi Foundation no reconoce los filtros o enrutamiento.


### <a name="disabling-transcoders-and-proxy-servers"></a>Deshabilitar transcodificadores y servidores Proxy

Operadores de red móvil tienen dos objetivos generales en su enfoque a internet móvil:

1. Proporcionar como contenido relevante mucho como sea posible
2. Maximizar el número de clientes que pueden compartir el ancho de banda de red limitado de radio

Puesto que la mayoría de las páginas web diseñados para las pantallas grandes de tamaño de escritorio y conexiones de banda ancha de línea rápida fijo, muchos operadores usar *transcodificadores* o *servidores proxy* que modifican dinámicamente el contenido web. Puede modificar el código HTML o CSS para adaptarlo a pantallas más pequeñas (especialmente para la "función teléfonos" que no tienen la capacidad de procesamiento para controlar diseños complejos), y pueden volver a comprimir las imágenes (lo que reduce considerablemente su calidad) para mejorar la velocidad de entrega de página.

Pero si ha realizado el esfuerzo necesario para generar una versión optimizada de móviles de su sitio, probablemente no desee interferir con cualquier aún más el operador de red. Puede agregar la siguiente línea a la página\_eventos de carga en cualquier formulario Web de ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

O bien, para un controlador de MVC de ASP.NET, puede agregar la invalidación del método siguiente para que se aplique a todas las acciones en ese controlador:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

El mensaje HTTP resultante informa a transcodificadores conforme W3C y servidores proxy de no alterar el contenido. Por supuesto, no hay ninguna garantía de que los operadores de red móvil respetará este mensaje.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Aplicar un estilo a las páginas de dispositivos móviles para los exploradores móviles

Queda fuera del ámbito de este documento se describen con gran detalle qué tipos de trabajos de marcado HTML correctamente o qué técnicas de diseño web maximizar la facilidad de uso de dispositivos específicos. Ha seguridad para buscar un diseño lo suficientemente sencillo, optimizado para una pantalla mobile tamaño, sin usar no confiable HTML o CSS de posición trucos. Sin embargo, es una técnica importante cabe mencionar, la *ventanilla meta etiqueta*.

Algunos exploradores móviles modernos, en un esfuerzo mostrar las páginas web destinado a monitores de escritorio, presentar la página en un lienzo virtual, también denominado "ventanilla" (p. ej., la ventanilla virtual es 980 píxeles de ancho en iPhone y 850 píxeles de ancho en Opera Mobile de forma predeterminada) y, a continuación, reducir el resultado que se ajuste a píxeles físicos de la pantalla. El usuario puede, a continuación, usar el zoom y panorámica dicha ventanilla. Esto tiene la ventaja de que permite que el explorador muestre la página en su diseño previsto, pero también tiene la desventaja que obliga al aplicar zoom y panorámica, que no es apto para el usuario. Si está diseñando para dispositivos móviles, es mejor diseño para una pantalla limitada para que no sea necesaria ninguna zoom o desplazamiento horizontal.

Una manera de indicar al explorador móvil el ancho debe ser la ventanilla es a través de la no estándar *ventanilla* meta etiqueta. Por ejemplo, si agrega lo siguiente a la sección de encabezado de la página,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… a continuación, compatibilidad con exploradores de smartphone se estructurará la página en un lienzo virtual a nivel de 480 píxeles. Esto significa que, si los elementos HTML definen el ancho en términos de porcentaje, se interpretará los porcentajes en relación con este ancho de 480 píxeles, no el ancho de la ventanilla de forma predeterminada. Como resultado, el usuario es menos probable que deba aplicar zoom y desplazar horizontalmente: considerablemente mejora de la experiencia de exploración móvil.

Si desea que el ancho de la ventanilla para que coincida con los píxeles físicos del dispositivo, puede especificar lo siguiente:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Para que funcione correctamente, no debe explícitamente obligar a elementos que se va a superar ese ancho (por ejemplo, mediante un *ancho* atributo o propiedad CSS), en caso contrario, el explorador se verán obligado a utilizar una ventanilla más grande sin tener en cuenta. Vea también: [más detalles sobre la etiqueta de la ventanilla no estándar](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Smartphones más modernos admiten *orientación dual*: se pueden guardar en modo vertical u horizontal. Por lo tanto, es importante no haga suposiciones sobre el ancho de pantalla en píxeles. No incluso se supone que se ha corregido el ancho de pantalla, ya que el usuario puede reorientar su dispositivo mientras están en la página.

Los dispositivos Windows Mobile y Blackberry más antiguos también pueden aceptar las siguientes etiquetas meta en el encabezado de página para informarles de contenido se ha optimizado para dispositivos móviles y, por tanto, no deben transformarse.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Recursos adicionales

Para obtener una lista de los emuladores de dispositivos móviles y los simuladores puede usar para probar la aplicación web ASP.NET mobile, consulte la página [simular dispositivos móviles más conocidos para la prueba](../mobile/device-simulators.md).

## <a name="credits"></a>Créditos

- Autor principal: Steven Sanderson
- Los revisores / adicionales escritores de contenido: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
