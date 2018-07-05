---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Cómo: Agregar páginas de dispositivos móviles a los formularios Web de ASP.NET / MVC Application | Microsoft Docs'
author: rick-anderson
description: Esta página se describe varias maneras de servir páginas optimizadas para dispositivos móviles de los formularios Web Forms de ASP.NET / aplicación de MVC y sugiere la arquitectura y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: ''
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 075329087cb5e07d85bba0c546538e7cc55ac463
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366762"
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
> Esta página se describe varias maneras de servir páginas optimizadas para dispositivos móviles de los formularios Web Forms de ASP.NET / aplicación de MVC y sugiere la arquitectura y diseño cuestiones a considerar cuando el destino es una amplia gama de dispositivos. Este documento también explica por qué los controles ASP.NET Mobile desde ASP.NET 2.0 a 3.5 ahora están obsoletos y se tratan algunas alternativas modernas.


## <a name="contents"></a>Contenido

- Información general
- Opciones de arquitectura
- Detección de explorador y del dispositivo
- Cómo las aplicaciones de formularios Web Forms ASP.NET pueden presentar páginas específicas de dispositivos móviles
- Cómo las aplicaciones de ASP.NET MVC pueden presentar páginas específicas de dispositivos móviles
- Recursos adicionales

Para obtener ejemplos de código descargable que muestra las técnicas de este artículo técnico para ASP.NET Web Forms y MVC, consulte [Mobile Apps & sitios con ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Información general

Los dispositivos móviles – smartphones, tabletas y teléfonos de la característica: siguen creciendo en popularidad como un medio para tener acceso a la Web. Para muchos desarrolladores web y las empresas orientados a web, esto significa que resulta cada vez más importante proporcionar una gran experiencia de exploración para los visitantes utilizan esos dispositivos.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Cómo versiones preliminares de los exploradores móviles compatibles con ASP.NET

Las versiones de ASP.NET 2.0 a 3.5 incluye *controles ASP.NET Mobile*: un conjunto de controles de servidor para dispositivos móviles en el *System.Web.Mobile.dll* ensamblado y el  *En System.Web.UI.MobileControls* espacio de nombres. El ensamblado todavía está incluido en ASP.NET 4, pero está en desuso. Para migrar a los enfoques más modernos, tales como los descritos en este artículo, se aconseja a los desarrolladores.

El motivo por qué controles móviles de ASP.NET se han marcado como obsoleto es que su diseño se centra en los teléfonos móviles que son comunes en torno a 2005 y anterior. Los controles están diseñados principalmente para representar el marcado de cHTML o de WML (en lugar de HTML normal) para los exploradores WAP de aquella era. Pero WAP, WML y cHTML ya no son relevantes para la mayoría de los proyectos, porque HTML se ha convertido en el lenguaje de marcado ubicuo para exploradores de escritorio y móviles iguales.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Los desafíos de hoy en día se admiten aquellos dispositivos móviles

Aunque los exploradores móviles ahora casi totalmente compatible con HTML, todavía sufrirán muchos desafíos al que tiene como finalidad crear excelentes experiencias de exploración móviles:

- ***Tamaño de pantalla*** : los dispositivos móviles varían considerablemente en el formulario y sus pantallas suelen ser mucho menor que los monitores de escritorio. Por lo tanto, es posible que deba crear diseños de página completamente diferente para ellos.
- ***Métodos de entrada*** : algunos dispositivos tienen teclados, algunos tienen fonocaptores, otros usuarios usan un toque. Deberá considerar navegación varios mecanismos y los datos de los métodos de entrada.
- ***Cumplimiento de estándares*** – muchos exploradores móviles no son compatibles con los estándares más recientes de HTML, CSS o JavaScript.
- ***Ancho de banda*** : rendimiento de la red de telefonía móvil datos varía ampliamente y algunos usuarios finales se encuentran en las tarifas que cobran por en megabytes.

No hay ninguna solución única; la aplicación tendrá que buscar y comportarse de manera diferente según el dispositivo tener acceso a él. Dependiendo de qué nivel de compatibilidad con dispositivos móvil que desee, esto puede ser un desafío mayor para los desarrolladores web que alguna vez fue el escritorio "wars explorador".

Desarrolladores que abordan la compatibilidad del explorador móvil por primera vez a menudo inicialmente crean que solo es importante admitir los smartphones más recientes y más sofisticados (p. ej., Windows Phone 7, iPhone o Android), quizás porque los desarrolladores a menudo personalmente poseen como dispositivos. Sin embargo, todavía son extremadamente populares teléfonos más baratos y sus propietarios deben usar para navegar por Internet, especialmente en los países donde los teléfonos móviles son más fáciles de obtener que una conexión de banda ancha. Debe decidir qué gama de dispositivos para admitir teniendo en cuenta sus clientes es probable que su negocio. Si va a crear un folleto en línea para un spa de mantenimiento de lujo, podría tomar la decisión de negocio solo como destino smartphones avanzados, mientras que si va a crear un sistema de reservas de vale para un cine, probablemente necesitará tener en cuenta que los visitantes con menos eficaces características teléfonos.

## <a name="architectural-options"></a>Opciones de arquitectura

Antes de entrar en los detalles técnicos específicos de ASP.NET Web Forms o MVC, tenga en cuenta que los desarrolladores web en general tienen tres opciones posibles principales para admitir exploradores móviles:

1. ***No hacer nada:*** simplemente puede crear una aplicación web orientada a servicios de escritorio estándar y se basan en los exploradores móviles se representan de manera aceptable. 

    - **Ventaja**: es la opción más económica de implementar y mantener: un trabajo adicional no
    - **Desventaja**: ofrece la experiencia del usuario final peor: 

        - Los smartphones más recientes puede representar el HTML, así como un explorador de escritorio, pero todavía se verán obligados a los usuarios para aplicar zoom y desplazamiento horizontal y verticalmente para consumir el contenido en una pantalla pequeña. Esto está lejos de ser óptimo.
        - Los dispositivos más antiguos y los teléfonos de característica pueden producir un error al representar el marcado de forma satisfactoria.
        - Incluso en los últimos dispositivos Tablet PC (cuyas pantallas pueden ser tan grandes como las de portátiles), se aplican las reglas de interacción diferentes. Entrada basados en funcionalidad táctil funciona mejor con los botones más grandes y vincula diseminación aún más separado y no hay ninguna manera de mantener el mouse de un cursor del mouse sobre un menú emergente.
2. ***Resolver el problema en el cliente* –** con cuidado de uso de CSS y [mejora progresiva](http://en.wikipedia.org/wiki/Progressive_enhancement) puede crear marcado, estilos y scripts que se adapten a cualquier explorador que está ejecutando. Por ejemplo, con [las consultas de medios de CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), podría crear un diseño de varias columna que se convierte en un diseño de columna única en los dispositivos cuyas pantallas son más estrechas que el umbral seleccionado. 

    - **Ventaja**: optimiza la representación del dispositivo específico en uso, incluso para futuros dispositivos desconocidos función cualquier pantalla y las características de entrada tienen
    - **Ventaja**: fácilmente le permite compartir la lógica del lado servidor en todos los tipos de dispositivo: duplicación mínima de código ni esfuerzo alguno
    - **Desventaja**: los dispositivos móviles son muy diferentes del que es posible que realmente quiere las páginas móviles sea completamente distinto de las páginas de escritorio, que muestra información diferentes dispositivos de escritorio. Estas variaciones pueden ser ineficaces o poco probables de conseguir con eficacia a través de CSS por sí solo, especialmente teniendo en cuenta cómo incoherentemente dispositivos antiguos interpretan las reglas de CSS. Esto es especialmente cierto para los atributos CSS 3.
    - **Desventaja**: no proporciona compatibilidad con lógica de servidor diferente y flujos de trabajo para distintos dispositivos. Por ejemplo, no es posible, implemente una compra carro de la desprotección flujo de trabajo simplificado para usuarios móviles mediante CSS por sí solo.
    - **Desventaja**: uso de ancho de banda ineficaz. El servidor puede tener que transmitir el marcado y los estilos que se aplican a todos los dispositivos posibles, aunque el dispositivo de destino sólo utilizará un subconjunto de esa información.
3. ***Solucionar el problema en el servidor* –** si el servidor sabe cuál es el dispositivo tiene acceso a ella, o en menos de las características de ese dispositivo, como su tamaño de la pantalla y el método de entrada, y ya sea un dispositivo móvil: se puede ejecutar una lógica diferente y marcado HTML diferentes de salida. 

    - **Ventaja:** flexibilidad máxima. No hay ningún límite a cuánto puede variar la lógica del lado servidor para dispositivos móviles u optimizar el marcado para el diseño deseado, específicos del dispositivo.
    - **Ventaja:** uso eficaz del ancho de banda. Solo necesita transmitir el marcado y la información de estilo que se va a usar el dispositivo de destino.
    - **La desventaja:** a veces fuerza la repetición de código o esfuerzo (p. ej., que puede crear copias similares pero ligeramente distintos de las páginas de formularios Web Forms o las vistas de MVC). Donde sea posible que separará lógica común en una capa subyacente o servicio, pero aún así, algunas partes del código de interfaz de usuario o de marcado que tenga que se duplican y, a continuación, se mantienen en paralelo.
    - **La desventaja:** detección de dispositivos no es trivial. Se requiere una lista o una base de datos de tipos de dispositivos conocidos y sus características (que no sea siempre actualizadas perfectamente) y no está garantizado que coincidan exactamente con todas las solicitudes entrantes. Este documento describe algunas opciones y sus riesgos más adelante.

Para obtener los mejores resultados, la mayoría de los desarrolladores encontrar que necesitan para combinar las opciones de (2) y (3). Diferencias menores de estilo se implementan mejor en el cliente mediante CSS o JavaScript incluso, mientras que las diferencias más importantes en los datos, el flujo de trabajo o el marcado se implementan la forma más eficaz en el código del lado servidor.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Este documento se centra en técnicas de servidor

Puesto que ASP.NET Web Forms y MVC son ambas tecnologías principalmente en el servidor, en este artículo se centrará en las técnicas de servidor que le permiten generar marcado diferente y la lógica para los exploradores móviles. Por supuesto, también se puede combinar con cualquier técnica de lado cliente (p. ej., CSS 3 las consultas de medios, mejora progresiva de JavaScript), pero es más una cuestión de diseño web de desarrollo de ASP.NET.

## <a name="browser-and-device-detection"></a>Detección de explorador y del dispositivo

El requisito previo de claves para todas las técnicas de lado servidor para admitir dispositivos móviles es saber qué dispositivo usa el visitante. De hecho, aún mejor que saber el número de fabricante y modelo del dispositivo es saber el *características* del dispositivo. Pueden incluir características:

- ¿Es un dispositivo móvil?
- Método de entrada (mouse/teclado, táctil, teclado, un joystick,...)
- Tamaño de pantalla (físicamente y en píxeles)
- Formatos admitidos de medios y datos
- Etcetera.

Es mejor tomar decisiones basadas en las características que el número de modelo, ya que, a continuación, estará equipado mejor para controlar futuros dispositivos.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Uso de ASP. Compatibilidad con detección de explorador integrado de la red

Los desarrolladores de ASP.NET Web Forms y MVC inmediatamente pueden detectar características importantes de un explorador visita inspeccionando las propiedades de la *Request.Browser* objeto. Por ejemplo, vea

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...y muchos otros

En segundo plano, la plataforma ASP.NET coincide con la entrada *User-Agent* encabezado HTTP (UA) con expresiones regulares en un conjunto de archivos XML de definición de explorador. De forma predeterminada, la plataforma incluye las definiciones para varios dispositivos móviles comunes, y puede agregar archivos de definición de explorador personalizados para que otros usuarios que quieran reconocer. Para obtener más información, vea la página MSDN [controles de servidor Web de ASP.NET y las capacidades del explorador](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Uso de la base de datos del dispositivo WURFL a través de 51Degrees.mobi Foundation

Mientras que ASP. Compatibilidad con detección de explorador integrado de la red será suficiente para muchas aplicaciones, hay dos casos principales es posible que no sea suficiente:

- ***Desea reconocer los últimos dispositivos***(sin necesidad de crear manualmente los archivos de definición de explorador para ellos). Tenga en cuenta que los archivos de definición de explorador del 4 de .NET no son lo suficientemente recientes como para reconocer los iPad de Apple, teléfonos Android, exploradores de Opera Mobile o Windows Phone 7.
- ***Necesita información más detallada sobre las capacidades de dispositivo***. Es posible que deba saber sobre el método de entrada de un dispositivo (por ejemplo, el teclado de vs táctil) o qué audio formatea el explorador admite. Esta información no está disponible en los archivos de definición de explorador estándar.

El [ *Wireless Universal Resource File* proyecto (WURFL)](http://wurfl.sourceforge.net/) mantiene mucho más detallada y actualizada información acerca de los dispositivos móviles en uso hoy en día.

La buena noticia para los desarrolladores de .NET es que ASP. Característica de detección de explorador de la red es extensible, por lo que es posible mejorarlo para solucionar estos problemas. Por ejemplo, puede agregar el código abierto [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) biblioteca al proyecto. Es un IHttpModule de ASP.NET o un explorador capacidades de proveedor (se puede utilizar en aplicaciones de formularios Web Forms y MVC), que lee los datos WURFL y lo enlaza a ASP directamente. Mecanismo de detección de explorador integrado de la red. Una vez haya instalado el módulo, *Request.Browser* repentinamente contendrá información mucho más precisa y detallada: lo reconocerá muchos de los dispositivos que se ha mencionado anteriormente y enumerar sus capacidades (incluido correctamente funcionalidades adicionales como método de entrada). Consulte la documentación del proyecto para obtener más detalles.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Cómo las aplicaciones de formularios Web Forms pueden presentar páginas específicas de dispositivos móviles

De forma predeterminada, presentamos cómo se muestra una nueva aplicación de formularios Web Forms en dispositivos móviles comunes:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Claramente, ni el diseño es apta para móviles muy: esta página se diseñó para un monitor grande, orientado a horizontal, no para una pantalla pequeña orientación vertical. Así que ¿qué puede hacer al respecto?

Como se explicó anteriormente en este artículo, hay muchas maneras de personalizar las páginas para dispositivos móviles. Algunas técnicas están basadas en servidor, otros se ejecutan en el cliente.

### <a name="creating-a-mobile-specific-master-page"></a>Creación de una página maestra específica móvil

Según sus requisitos, puede usar los mismos formularios Web para todos los visitantes, pero tiene dos páginas maestra independientes: uno para los visitantes del escritorio, otro para los visitantes en dispositivos móviles. Esto ofrece la flexibilidad de cambiar el nivel superior marcado HTML para adaptarlo a dispositivos móviles, sin que sea necesario duplicar cualquier lógica de página o la hoja de estilos CSS.

Esto es fácil de hacer. Por ejemplo, puede agregar un controlador de PreInit como el siguiente a un formulario Web Forms:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Ahora, cree una página maestra denominada Mobile.Master en la carpeta de nivel superior de la aplicación y se usará cuando se detecta un dispositivo móvil. La página maestra móvil puede hacer referencia a una hoja de estilos CSS específicas de dispositivos móviles si es necesario. Los visitantes escritorio seguirán viendo la página maestra predeterminada, no lo móvil.

### <a name="creating-independent-mobile-specific-web-forms"></a>Creación de independiente de formularios Web Forms específicas de dispositivos móviles

Para obtener la máxima flexibilidad, puede ir mucho más allá de simplemente tener páginas maestras independientes para distintos tipos de dispositivos. Puede implementar dos *totalmente independientes conjuntos de páginas de formularios Web Forms* : uno establecido para los exploradores de escritorio, otro conjunto para dispositivos móviles. Esto funciona mejor si desea presentar información muy diferente o flujos de trabajo a los visitantes en dispositivos móviles. El resto de esta sección describe este enfoque en detalle.

Suponiendo que ya tiene una aplicación de formularios Web Forms diseñada para los exploradores de escritorio, la manera más fácil para continuar es crear una subcarpeta denominada "Mobile" dentro del proyecto y compilar las páginas móviles no existe. Puede crear todo un sitio secundario, con sus propias páginas maestras, las hojas de estilo y páginas, utilizando las mismas técnicas que usaría para cualquier otra aplicación de formularios Web Forms. No es necesario generar un equivalente para móviles *cada* página en el sitio de escritorio; puede elegir qué subconjunto de funcionalidad que tiene sentido para los visitantes en dispositivos móviles.

Las páginas móviles pueden compartir recursos estáticos comunes (como imágenes, JavaScript o CSS archivos) con sus páginas normales si lo desea. Puesto que la carpeta "Mobile" le *no* marcarse como una aplicación independiente cuando se hospeda en IIS (es simplemente una subcarpeta simple de páginas de formularios Web Forms), también compartirán las mismas configuración, los datos de sesión y otras infraestructuras como su páginas del escritorio.

> [!NOTE]
> Puesto que este enfoque implica normalmente algunas duplicaciones de código (las páginas móviles están probable que comparten algunas similitudes con páginas de escritorio), es importante el factor de cualquier negocio lógica o datos acceso código común en una capa subyacente compartida o un servicio. En caso contrario, deberá doble el esfuerzo de crear y mantener la aplicación.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Redirigir visitantes en dispositivos móviles a las páginas móviles

A menudo resulta conveniente redirigir los visitantes en dispositivos móviles a las páginas móviles sólo en el *primera* solicitar en su sesión de exploración (y no en todas las solicitudes en su sesión), porque:

- A continuación, puede permitir fácilmente visitantes en dispositivos móviles tener acceso a las páginas de escritorio si lo desean, simplemente poner un vínculo en la página maestra que se va a "Versión de escritorio". El visitante no pueden redirigir a una página móvil, porque ya no es la primera solicitud en su sesión.
- Evita el riesgo de interferir con las solicitudes de los recursos dinámicos compartidos entre las partes de escritorio y móviles de su sitio (por ejemplo, si tiene un formulario Web comunes que se muestran las partes móviles y de escritorio de su sitio en un IFRAME, así como ciertos controladores de Ajax)

Para ello, puede colocar la lógica de redirección en una **sesión\_iniciar** método. Por ejemplo, agregue el método siguiente al archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurar la autenticación de formularios para respetar las páginas móviles

Tenga en cuenta que la autenticación de formularios hace determinadas suposiciones sobre dónde puede redirigir a los visitantes durante y después del proceso de autenticación:

- Cuando un usuario necesita autenticarse, autenticación de formularios redirigirá a la página de inicio de sesión de escritorio, independientemente de si un usuario móvil o de escritorio (ya que solo tiene un concepto de *una* dirección URL de inicio de sesión). Suponiendo que desea aplicar el estilo de la página de inicio de sesión móvil diferente, deberá mejorar su página de inicio de sesión de escritorio para que lo redirige a los usuarios móviles a una página de inicio de sesión móvil independiente. Por ejemplo, agregue el código siguiente a su **desktop** código de página de inicio de sesión en segundo plano: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Después de que un usuario inicia sesión correctamente, la autenticación de formularios de forma predeterminada redirigirá a la página principal del escritorio (ya que solo tiene un concepto de *uno* página predeterminada). Necesario mejorar su página de inicio de sesión móvil para que lo redirige a la página principal de mobile después un registro correcto. Por ejemplo, agregue el código siguiente a su **móvil** código de página de inicio de sesión en segundo plano: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Este código supone que la página tiene un control de servidor de inicio de sesión llamado LoginUser, como se muestra en la plantilla de proyecto predeterminada.

### <a name="working-with-output-caching"></a>Trabajar con la caché de resultados

Si usa la caché de resultados, tenga en cuenta que de forma predeterminada, es posible que un usuario de escritorio visitar una URL de determinadas (lo que su salida se almacene en caché), seguido por un usuario móvil que, a continuación, recibe el resultado de desktop almacenados en caché. Esta advertencia se aplica si solo está variar la página maestra por tipo de dispositivo, o implementar totalmente separada formularios Web Forms por tipo de dispositivo.

Para evitar el problema, puede indicar a ASP.NET para modificar la entrada de caché según si el visitante usa un dispositivo móvil. Agregue un parámetro VaryByCustom a la declaración de directiva OutputCache de la página de la manera siguiente:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

A continuación, defina *isMobileDevice* como una caché personalizada parámetro agregando el siguiente método reemplace al archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Esto garantizará que visitantes en dispositivos móviles a la página no reciben la salida guardado previamente en la memoria caché un visitante de escritorio.

### <a name="a-working-example"></a>Un ejemplo práctico

Para ver estas técnicas en acción, descargue [ejemplos de código de este artículo técnico](https://docs.microsoft.com/aspnet/mobile/overview). La aplicación de ejemplo de formularios Web Forms redirige automáticamente a los usuarios móviles a un conjunto de páginas específicas de dispositivos móviles en una subcarpeta denominada Mobile. El marcado y los estilos de dichas páginas mejor está optimizado para los exploradores móviles, como puede ver en las capturas de pantalla siguiente:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Para obtener más sugerencias sobre cómo optimizar su marcado y CSS para los exploradores móviles, consulte la sección "Estilos páginas móviles para los exploradores móviles" más adelante en este documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Cómo las aplicaciones de ASP.NET MVC pueden presentar páginas específicas de dispositivos móviles

Puesto que el patrón Model-View-Controller separa la lógica de aplicación (en los controladores) desde la lógica de presentación (en las vistas), puede elegir desde cualquiera de los métodos siguientes para controlar la compatibilidad con dispositivos móvil en el código del lado servidor:

1. ***Usar los mismos controladores y vistas para los exploradores de escritorio y móviles, pero representan las vistas con distintos diseños de Razor según el tipo de dispositivo *.** Esta opción funciona mejor si están mostrando datos idénticos en todos los dispositivos, pero simplemente desea proporcionar distintas hojas de estilo CSS o cambiar algunos elementos de HTML de nivel superior para dispositivos móviles.
2. ***Usar los mismos controladores para los exploradores de escritorio y móviles, pero presentar vistas distintas según el tipo de dispositivo***. Esta opción funciona mejor si está mostrando aproximadamente los mismos datos y proporcionar los mismos flujos de trabajo para los usuarios finales, pero desea presentar HTML muy diferente para satisfacer el dispositivo que se usa.
3. ***Crear áreas bien diferenciadas para los exploradores de escritorio y móviles, implementar vistas y controladores independientes para cada *.** Esta opción funciona mejor si mostrar pantallas muy diferentes, que contiene información diferente e iniciales al usuario a través de diferentes flujos de trabajo optimizado para su tipo de dispositivo. Significa que es posible que algunos repetición de código, pero puede minimizar esto mediante la exclusión lógica común en una capa o de servicio subyacente.

Si desea aprovechar el **primera** opción y variar solo el diseño de Razor por tipo de dispositivo, es muy fácil. Solo tiene que modificar su \_ViewStart.cshtml archivo como sigue:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Ahora puede crear un diseño específico de dispositivos móviles denominado \_LayoutMobile.cshtml con una estructura de la página y CSS reglas optimizados para dispositivos móviles.

Si desea aprovechar el **segundo** opción render vistas totalmente diferente según el tipo de dispositivo del visitante, consulte [entrada de blog de Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

El resto de este artículo se centra en la **tercer** opción – Crear controladores independientes *y* vistas para dispositivos móviles: para que pueda controlar exactamente qué subconjunto de funcionalidad se ofrece para los visitantes en dispositivos móviles.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configurar un área dentro de la aplicación de MVC de ASP.NET Mobile

Puede agregar un área denominada "Mobile" a una aplicación ASP.NET MVC existente de la manera normal: haga doble clic en el nombre del proyecto en el Explorador de soluciones, a continuación, elija Agregar à área. A continuación, puede agregar controladores y vistas como lo haría para cualquier otra área dentro de una aplicación ASP.NET MVC. Por ejemplo, agregar a su área móvil un nuevo controlador denominado HomeController para que actúe como una página de inicio para los visitantes en dispositivos móviles.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Lo que garantiza la dirección URL de /Mobile llega a la página principal de móvil

Si desea que la dirección URL /Mobile para llegar a la acción Index en HomeController dentro de su área móvil, deberá realizar dos pequeños cambios en la configuración de enrutamiento. En primer lugar, actualice la clase MobileAreaRegistration para que el HomeController es el controlador predeterminado en el área móvil, tal como se muestra en el código siguiente:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Esto significa que la página principal de móvil ahora se ubicarán en /Mobile en lugar de/Mobile/Home, porque "Hogar" es ahora el implícitamente nombre de controlador de forma predeterminada para las páginas móviles.

A continuación, tenga en cuenta que al agregar un segundo HomeController a su aplicación (es decir, la móviles, además de escritorio uno existente), habrá divide la página principal de escritorio normal. Se producirá un error con el error "*se encontraron varios tipos que coinciden con el controlador denominado 'Inicio'*". Para resolver este problema, actualice la configuración de enrutamiento de nivel superior (de Global.asax.cs) para especificar que el HomeController escritorio debería tener prioridad cuando hay ambigüedad:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Ahora el error irá ubicación y la dirección URL http://<em>suSitio</em>/ llegará a la página principal de escritorio y http://<em>suSitio</em>/mobile/ llegará a la página principal de dispositivos móvil.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Redirigir visitantes en dispositivos móviles a su área móvil

Hay muchas diferentes puntos de extensibilidad de ASP.NET MVC, por lo que hay muchas maneras posibles para inyectar lógica de redirección. Es una opción clara crear un atributo de filtro, [RedirectMobileDevicesToMobileArea], que realiza un redireccionamiento si se cumplen las condiciones siguientes:

1. Es la primera solicitud en la sesión del usuario (es decir, Session.IsNewSession es igual a true)
2. La solicitud procede de un explorador móvil (es decir, Request.Browser.IsMobileDevice es igual a true)
3. El usuario ya no solicita un recurso en el área móvil (es decir, el *ruta* parte de la dirección URL no empieza por /Mobile)

El ejemplo descargable incluido en este artículo incluye una implementación de esta lógica. Se implementa como un filtro de autorización, derivado de AuthorizeAttribute, lo que significa que pueda funcionar correctamente, incluso si está utilizando el almacenamiento en caché de salida (en caso contrario, si un visitante escritorio primera accesos a una URL determinada, el resultado de desktop podrían almacenarse en caché y, después, atiende a posteriores visitantes en dispositivos móviles).

Ya que es un filtro, puede elegir uno de ellos para aplicarla a determinados controladores y acciones, por ejemplo,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… o puede aplicarla a todos los controladores y acciones como un MVC 3 *filtros globales* en el archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

El ejemplo descargable también muestra cómo puede crear subclases de este atributo que redireccionan a ubicaciones específicas dentro de su área móvil. Esto significa, por ejemplo, que puede:

- Registrar un filtro global tal como se muestra arriba que envía los visitantes en dispositivos móviles a la página principal móvil de forma predeterminada.
- También aplicar un filtro de [RedirectMobileDevicesToMobileProductPage] especial a una acción de "ver el producto" que toma los visitantes en dispositivos móviles a la versión móvil de cualquier página del producto que lo haya solicitado.
- También se aplican otras subclases del filtro especiales para realizar determinadas acciones, redirigiendo a la página móvil equivalente visitantes en dispositivos móviles

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurar la autenticación de formularios para respetar las páginas móviles

Si usa la autenticación de formularios, debe tener en cuenta que cuando un usuario necesita iniciar sesión, redirige automáticamente al usuario a una sola "iniciar sesión" dirección URL específica, cuyo valor predeterminado es **/cuenta/inicio de sesión**. Esto significa que los usuarios móviles que se le redirija a la acción escritorio "iniciar sesión".

Para evitar este problema, agregue lógica a la acción "iniciar sesión" del escritorio para que los usuarios móviles redirige de nuevo a una acción "iniciar sesión" móviles. Si usa la plantilla de aplicación de ASP.NET MVC predeterminada, actualizar acción de inicio de sesión de AccountController como sigue:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… y, a continuación, implementar una acción de "iniciar sesión" específicas de dispositivos móviles adecuados en un controlador llamado AccountController en su área móvil.

### <a name="working-with-output-caching"></a>Trabajar con la caché de resultados

Si usa el filtro [OutputCache], debe forzar la entrada de caché que varían según el tipo de dispositivo. Por ejemplo, escriba:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

A continuación, agregue el método siguiente al archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Esto garantizará que visitantes en dispositivos móviles a la página no reciben la salida guardado previamente en la memoria caché un visitante de escritorio.

### <a name="a-working-example"></a>Un ejemplo práctico

Para ver estas técnicas en acción, descargue [código de este artículo técnico asociado ejemplos](https://docs.microsoft.com/aspnet/mobile/overview). El ejemplo incluye una aplicación de ASP.NET MVC 3 (Release Candidate) ha mejorado para admitir dispositivos móviles mediante los métodos descritos anteriormente.

## <a name="further-guidance-and-suggestions"></a>Más orientación y sugerencias

En la siguiente explicación se aplica tanto a los formularios Web Forms y los desarrolladores MVC que usan las técnicas descritas en este documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Mejorar su lógica de redirección mediante 51Degrees.mobi Foundation

La lógica de redirección que se muestra en este documento puede ser suficiente para la aplicación, pero no funcionará si tiene que deshabilitar las sesiones, o con los exploradores móviles que rechazan las cookies (estos no tienen sesiones), ya que no sabrá si una solicitud determinada es la primera de ellas desde ese visitante.

Ya ha aprendido cómo la base de código abierto 51Degrees.mobi puede mejorar la precisión de ASP. Detección de explorador de la red. También tiene una capacidad integrada para redirigir los visitantes en dispositivos móviles a ubicaciones específicas configurados en el archivo Web.config. Es capaz de funcionar sin dependiendo de las sesiones ASP.NET (y, por tanto, las cookies) al almacenar un registro temporal de los valores hash de los encabezados HTTP y las direcciones IP de los visitantes, de modo que lo sepa si cada solicitud es la primera de ellas desde un visitante determinado.

El siguiente elemento agregado a la sección fiftyOne del archivo web.config redirigirá la primera solicitud desde un dispositivo móvil detectado a la página ~ / Mobile/Default.aspx. Las solicitudes de páginas bajo la carpeta móvil le *no* redirigirá, independientemente del tipo de dispositivo. Si el dispositivo móvil ha estado inactivo durante un período de 20 minutos o más el dispositivo se olvidarán y las solicitudes posteriores se tratará como nuevos para los fines de redirección.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Para obtener más información, consulte [51degrees.mobi documentación Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Le *puede* característica de redirección de la Fundación de 51Degrees.mobi de uso en aplicaciones de ASP.NET MVC, pero tendrá que definir la configuración de redirección en cuanto a direcciones URL sin formato, no en términos de los parámetros de enrutamiento o colocar filtros de MVC en acciones. Esto es porque (en el momento de redactar) 51Degrees.mobi Foundation no reconoce los filtros o enrutamiento.


### <a name="disabling-transcoders-and-proxy-servers"></a>Deshabilitar servidores Proxy y los transcodificadores

Operadores de red móvil tienen dos objetivos amplia en su enfoque a internet móvil:

1. Proporcionar como contenido mucho pertinente como sea posible
2. Maximizar el número de clientes que pueden compartir el ancho de banda de red limitado de radio

Puesto que la mayoría de las páginas web se diseñaron para pantallas de tamaño de escritorio grandes y conexiones de banda ancha de línea rápido se ha corregido, usar muchos operadores *transcodificadores* o *servidores proxy* que alterar dinámicamente el contenido web. Puede modificar el formato HTML o CSS para que se ajuste a pantallas más pequeñas (especialmente para la "característica teléfonos" que no tienen la capacidad de procesamiento para controlar los diseños complejos), y pueden volver a comprimir las imágenes (lo que reduce considerablemente su calidad) para mejorar las velocidades de entrega de la página.

Pero si ha realizado el esfuerzo para producir una versión optimizada para móviles de su sitio, probablemente no desea que el operador de red que interfieran con él cualquiera aún más. Puede agregar la siguiente línea a la página\_eventos de carga en cualquier formulario Web Forms de ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

O bien, para un controlador ASP.NET MVC, puede agregar la siguiente invalidación de método para que se aplique a todas las acciones en el controlador:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

El mensaje HTTP resultante informa a los transcodificadores conformes de W3C y los servidores de proxy no alterar el contenido. Por supuesto, no hay ninguna garantía de que los operadores de redes móviles respetará este mensaje.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Aplicar estilos a las páginas móviles para los exploradores móviles

Está fuera del ámbito de este documento describir en detalle qué tipos de trabajo de marcado HTML correctamente o qué técnicas de diseño web maximizar la facilidad de uso en determinados dispositivos. Se copia para buscar un diseño lo suficientemente sencillo, optimizado para una pantalla tamaño mobile, sin usar no confiable HTML o CSS trucos de posicionamiento. Sin embargo, es una técnica importante que merece la pena mencionar, el *etiqueta meta de ventanilla*.

Lo ciertos exploradores móviles, en un esfuerzo para mostrar las páginas web diseñadas para los monitores de escritorio, representan la página en un lienzo virtual, también denominado "ventanilla" (por ejemplo, la ventanilla virtual es 980 píxeles de ancho en iPhone y 850 píxeles de ancho en Opera Mobile de forma predeterminada) y, a continuación, Reduzca el resultado para ajustarla a píxeles físicos de la pantalla. El usuario puede, a continuación, usar el zoom y panorámica dicha ventanilla. Esto tiene la ventaja que permite que el explorador muestre la página en su diseño previsto, pero también tiene la desventaja que obliga a Zoom y panorámica, que no es apto para el usuario. Si va a diseñar para dispositivos móviles, es mejor diseño para una pantalla estrecha para que no sea necesario zoom o desplazamiento horizontal.

Una manera de indicar al explorador móvil ancho debe ser la ventanilla es a través de la no estándar *ventanilla* etiqueta meta. Por ejemplo, si agrega lo siguiente a la sección de encabezado de la página,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… a continuación, compatibilidad con exploradores de smartphone se diseñe la página en un lienzo virtual a nivel de 480 píxeles. Esto significa que, si los elementos HTML definen el ancho en términos de porcentaje, se interpretará los porcentajes en relación con este ancho de 480 píxeles, no el ancho del área de visualización predeterminado. Como resultado, el usuario es menos probable que tenga que aplicar el zoom y desplazar horizontalmente: considerablemente mejora la experiencia de exploración móvil.

Si desea que el ancho de la ventanilla para que coincida con píxeles físicos del dispositivo, se puede especificar lo siguiente:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Para que funcione correctamente, debe no fuerza explícitamente elementos que se va a superar ese ancho (por ejemplo, mediante un *ancho* atributo o propiedad CSS), en caso contrario, se forzará el explorador para usar un área de visualización mayor independientemente. Vea también: [más detalles acerca de la etiqueta de ventanilla no estándar](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Los smartphones más modernos admiten *orientación dual*: se mantendrían en modo vertical u horizontal. Por lo tanto, es importante no realizar suposiciones sobre el ancho de pantalla en píxeles. No incluso se supone que se ha corregido el ancho de pantalla, dado que el usuario puede orientar volver a su dispositivo mientras están en la página.

Dispositivos Windows Mobile y Blackberry antiguos también pueden aceptar las siguientes etiquetas meta en el encabezado de página para informarles de contenido se ha optimizado para dispositivos móviles y, por tanto, no deben transformarse.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Recursos adicionales

Para obtener una lista de los emuladores de dispositivos móviles y los simuladores puede usar para probar la aplicación web ASP.NET mobile, consulte la página [simular dispositivos móviles conocidos para realizar pruebas](../mobile/device-simulators.md).

## <a name="credits"></a>Créditos

- Autor principal: Steven Sanderson
- Los revisores / adicionales escritores de contenido: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
