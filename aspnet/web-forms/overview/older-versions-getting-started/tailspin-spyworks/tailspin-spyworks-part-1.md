---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: Archivo -> Nuevo proyecto | Documentos de Microsoft'
author: JoeStagner
description: "Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 1 cubre información general y archivo/nuevo proyecto."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a>Parte 1: Archivo -> Nuevo proyecto
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 1 cubre información general y archivo/nuevo proyecto.


## <a id="_Toc260221666"></a>Información general

Este tutorial es una introducción a los formularios Web Forms de ASP.NET. Se comenzará lentamente, por lo que la experiencia de desarrollo de nivel web principiantes es correcto.

La aplicación que desarrollaremos es una tienda en línea simple.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Los visitantes pueden examinar los productos por categoría:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Pueden ver un único producto y agregarlo a su carro:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Puede revisar su carro de la eliminación de elementos que ya no desean:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Continuar con la desprotección les pedirá que

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Después de ordenar, verán una pantalla de confirmación simple:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Comenzaremos por crear un nuevo proyecto de formularios Web Forms de ASP.NET en Visual Studio 2010 e incrementalmente vamos a agregar características para crear una aplicación operativa completa. Al hacerlo, hablaremos sobre acceso de base de datos, vistas de lista y la cuadrícula, páginas de actualización de datos, validación de datos, mediante páginas maestras para diseño de página coherente, AJAX, validación, pertenencia a usuarios y mucho más.

Puede seguir a lo largo de paso a paso, o bien puede descargar la aplicación completa de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Puede usar Visual Studio 2010 o el libre Visual Web Developer 2010 desde [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Para compilar la aplicación, puede usar SQL Server o la libre SQL Server Express al host de la base de datos.

## <a id="_Toc260221667"></a>Archivo / nuevo proyecto

Comenzaremos seleccionando el nuevo proyecto en el menú archivo en Visual Studio. Se abrirá el cuadro de diálogo nuevo proyecto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Seleccione Visual C# / plantillas Web grupo a la izquierda y, a continuación, elija la plantilla de "Aplicación Web de ASP.NET" en la columna central. Denomine el proyecto TailspinSpyworks y presione el botón Aceptar.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Esto creará el proyecto. ¡Eche un vistazo a las carpetas que se incluyen en nuestra aplicación en el Explorador de soluciones en el lado derecho.

![](tailspin-spyworks-part-1/_static/image10.jpg)

La solución vacía no está completamente vacía: agrega una estructura de carpetas básico:

![](tailspin-spyworks-part-1/_static/image1.png)

Tenga en cuenta las convenciones de implementada por la plantilla de proyecto predeterminada de ASP.NET 4.

- La carpeta "Cuenta", implementa una interfaz de usuario básica para ASP. Subsistema de pertenencia de NET.
- La carpeta "Scripts" actúa como repositorio para los archivos de JavaScript del lado de cliente y los archivos .js de jQuery de núcleo se ponen a disposición de forma predeterminada.
- La carpeta "Styles" se usa para organizar nuestros elementos visuales del sitio web (hojas de estilos CSS)

Cuando se presiona F5 para ejecutar la aplicación y presentar la página default.aspx vemos lo siguiente.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Nuestro primer mejora de aplicaciones estará para reemplazar el archivo Style.css de la plantilla de formularios Web Forms predeterminada con los archivos de imagen asociada que se representarán el asthetics visual que queramos para nuestra aplicación Tailspin Spyworks y clases CSS.

Una vez hecho esto, nuestra página default.aspx representa similar al siguiente.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Tenga en cuenta los vínculos de imagen en la parte superior derecha de la página y los elementos de menú que se han agregado a la página maestra. Solo los vínculos "inicio de sesión" y "Cuenta" apuntan a las páginas que existen (generadas por la plantilla predeterminada) y el resto de las páginas que se va a implementar mientras compilamos nuestra aplicación.

También vamos a cambiar la ubicación de la página maestra en el directorio de estilos. Aunque esto es sólo una preferencia puede hacer cosas un poco más fácil si se decide que nuestra aplicación "skinable" en el futuro.

Una vez hecho esto que será necesario cambiar la página maestra referencias en todos los archivos .aspx generan por el valor predeterminado páginas de formularios Web Forms de ASP.NET.

>[!div class="step-by-step"]
[Siguiente](tailspin-spyworks-part-2.md)
