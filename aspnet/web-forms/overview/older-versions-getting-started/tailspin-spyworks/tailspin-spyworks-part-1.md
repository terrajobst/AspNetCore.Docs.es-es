---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: Archivo -> Nuevo proyecto | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 1 abarca información general y archivo/nuevo proyecto.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: d2e4b36c9029e86eea9b09974839e96e9aa39ced
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836027"
---
<a name="part-1-file--new-project"></a>Parte 1: Archivo -> Nuevo proyecto
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 1 abarca información general y archivo/nuevo proyecto.


## <a id="_Toc260221666"></a>  Información general

Este tutorial es una introducción a ASP.NET WebForms. Se comenzará lentamente, por lo que la experiencia de desarrollo de web de nivel principiante es correcto.

La aplicación que se va a compilar es una tienda en línea sencilla.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Los visitantes pueden examinar los productos por categoría:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Pueden ver un producto único y agregarlo a su carro de compra:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Puede revisar su carro de compra, quitar todos los elementos que ya no desean:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Continuar con la desprotección le pedirá que

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Después de la ordenación, verán una pantalla de confirmación simple:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Comenzaremos por crear un nuevo proyecto de formularios Web Forms de ASP.NET en Visual Studio 2010 y vamos a agregar gradualmente funciones para crear una aplicación operativa completa. Por el camino, hablaremos sobre acceso a la base de datos, vistas de lista y la cuadrícula, las páginas de actualización de datos, validación de datos, mediante páginas maestras para diseño de página coherente, AJAX, validación, pertenencia a usuario y mucho más.

Puede seguir paso a paso, o bien puede descargar la aplicación completa de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Puede usar Visual Studio 2010 o el gratuita Visual Web Developer 2010 desde [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Para compilar la aplicación, puede usar SQL Server o el libre SQL Server Express para hospedar la base de datos.

## <a id="_Toc260221667"></a>  Archivo / nuevo proyecto

Comenzaremos seleccionando el nuevo proyecto en el menú archivo en Visual Studio. Se abrirá el cuadro de diálogo nuevo proyecto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Seleccionaremos Visual C# / Web plantillas de grupo a la izquierda y, a continuación, elija la plantilla "Aplicación Web de ASP.NET" en la columna central. Nombre del proyecto TailspinSpyworks y presione el botón Aceptar.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Esto creará el proyecto. Echemos un vistazo a las carpetas que se incluyen en nuestra aplicación en el Explorador de soluciones en el lado derecho.

![](tailspin-spyworks-part-1/_static/image10.jpg)

La solución vacía no está completamente vacía, agrega una estructura de carpetas básica:

![](tailspin-spyworks-part-1/_static/image1.png)

Tenga en cuenta las convenciones que se implementa mediante la plantilla de proyecto predeterminada ASP.NET 4.

- La carpeta "Account", implementa una interfaz de usuario básica para ASP. Subsistema de pertenencia de la red.
- La carpeta "Scripts" sirve como repositorio para los archivos de JavaScript del lado cliente y los archivos de .js de jQuery core se ponen a disposición de forma predeterminada.
- La carpeta "Estilos" se usa para organizar nuestros gráficos del sitio web (hojas de estilos CSS)

Cuando se presiona F5 para ejecutar la aplicación y presentar la página default.aspx se ve lo siguiente.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Será nuestro primer mejora de aplicaciones reemplazar el archivo Style.css desde la plantilla predeterminada de formularios Web Forms con las clases CSS y archivos de imagen asociado que se representarán el asthetics visual que queramos para nuestra aplicación Tailspin Spyworks.

Una vez hecho esto, nuestra página default.aspx representa similar al siguiente.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Tenga en cuenta los vínculos de imagen en la parte superior derecha de la página y los elementos de menú que se han agregado a la página maestra. Solo los vínculos "Iniciar sesión" y "Cuenta" apuntan a las páginas que existen (generadas por la plantilla predeterminada) y el resto de las páginas que se implementará a medida que construimos nuestra aplicación.

También vamos a cambiar la ubicación de la página maestra en el directorio de estilos. Aunque esto es sólo una preferencia puede hacer las cosas un poco más fácil si decidimos que nuestra aplicación "skinable" en el futuro.

Una vez hecho esto, deberá cambiar la página maestra referencias en todos los archivos .aspx generan por el valor predeterminado las páginas de formularios Web Forms de ASP.NET.

> [!div class="step-by-step"]
> [Siguiente](tailspin-spyworks-part-2.md)
