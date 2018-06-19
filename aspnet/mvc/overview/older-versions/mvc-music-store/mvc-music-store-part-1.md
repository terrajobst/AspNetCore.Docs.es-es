---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Información general y archivo -> Nuevo proyecto | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 1 cubre información general y archivo -> Nuevo proyecto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868976"
---
<a name="part-1-overview-and-file-new-project"></a>Parte 1: Información general y archivo -> Nuevo proyecto
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 1 cubre información general y archivo -&gt;nuevo proyecto.


## <a name="overview"></a>Información general

La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Web Developer para el desarrollo web. Se comenzará lentamente, por lo que la experiencia de desarrollo de nivel web principiantes es correcto.

La aplicación que desarrollaremos es una tienda de música simple. Hay tres partes principales para la aplicación: la compra, la desprotección y administración.

![](mvc-music-store-part-1/_static/image1.jpg)

Los visitantes pueden examinar álbumes por género:

![](mvc-music-store-part-1/_static/image2.jpg)

Pueden ver un sólo álbum y agregarlo a su carro:

![](mvc-music-store-part-1/_static/image3.jpg)

Puede revisar su carro de la eliminación de elementos que ya no desean:

![](mvc-music-store-part-1/_static/image4.jpg)

Continuar con la desprotección les pedirá que inicie sesión o regístrese para una cuenta de usuario.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Después de crear una cuenta, puede completar el pedido rellenando la información de pago y envío. Para no complicar las cosas, estamos ejecutando una promoción increíble: todo es gratis Cuando especifique el código de promoción "Gratuito".

![](mvc-music-store-part-1/_static/image5.jpg)

Después de ordenar, verán una pantalla de confirmación simple:

![](mvc-music-store-part-1/_static/image6.jpg)

Además de las páginas de faceing de cliente, se deberá crear una sección de administrador que muestra una lista de álbumes desde la que los administradores pueden crear, editar y eliminar álbumes:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Archivo -&gt; nuevo proyecto

### <a name="installing-the-software"></a>Instalar el software

Este tutorial se iniciará mediante la creación de un nuevo proyecto de ASP.NET MVC 3 con el libre Visual Web Developer 2010 Express (que es gratuito) y, a continuación, vamos a agregar características para crear una aplicación operativa completa incrementalmente. Al hacerlo, hablaremos sobre acceso a la base de datos, los escenarios de envío de formulario, validación de datos, mediante páginas maestras para el diseño de página coherente, con AJAX para actualizaciones de la página y validación, el inicio de sesión de usuario y mucho más.

Puede seguir a lo largo de paso a paso, o bien puede descargar la aplicación completa de [tienda de música de MVC](https://github.com/evilDave/MVC-Music-Store).

Puede usar Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (es decir, una versión gratuita de Visual Studio 2010) para compilar la aplicación. Usaremos SQL Server Compact (también disponible) para hospedar la base de datos. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.


- [Requisitos previos de visual Studio Web Developer Express SP1]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0] - incluida la compatibilidad en tiempo de ejecución y herramientas


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Crear un nuevo proyecto de ASP.NET MVC 3

Comenzaremos seleccionando "Nuevo proyecto" en el menú archivo en Visual Web Developer. Se abrirá el cuadro de diálogo nuevo proyecto.

![](mvc-music-store-part-1/_static/image5.png)

Seleccione Visual C# -&gt; plantillas Web grupo a la izquierda, a continuación, elija la plantilla de "Aplicación Web de ASP.NET MVC 3" en la columna central. Denomine el proyecto MvcMusicStore y presione el botón Aceptar.

![](mvc-music-store-part-1/_static/image8.jpg)

Se mostrará un cuadro de diálogo secundario que nos permite realizar algunas MVC una configuración específica para el proyecto. Seleccione lo siguiente:

Plantilla de proyecto: seleccione vacío

Ver motor: seleccione Razor

Usar marcado semántico de HTML5: activada

Compruebe que la configuración forman tal y como se muestra a continuación, presione el botón Aceptar.

![](mvc-music-store-part-1/_static/image9.jpg)

Esto creará el proyecto. ¡Eche un vistazo a las carpetas que se han agregado a la aplicación en el Explorador de soluciones en el lado derecho.

![](mvc-music-store-part-1/_static/image10.jpg)

La plantilla vacía MVC 3 no está completamente vacía, agrega una estructura de carpetas básico:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC hace uso de algunas convenciones de nomenclatura básicas para los nombres de carpeta:

| **Folder** | **Purpose** |
| --- | --- |
| **/ Controladores** | Controladores de responden a los datos proporcionados por el explorador, decidir qué hacer con él y devolver la respuesta al usuario. |
| **/ Vistas** | Vistas mantenga nuestras plantillas de interfaz de usuario |
| **/Models** | Modelos de mantengan y manipulan datos |
| **/Content** | Esta carpeta contiene nuestras imágenes, CSS y cualquier otro contenido estático |
| **/ Secuencias de comandos** | Esta carpeta contiene los archivos de JavaScript |

Estas carpetas se incluyen incluso en una aplicación MVC de ASP.NET vacía porque el marco de MVC de ASP.NET de forma predeterminada, usa un enfoque de "Convención sobre configuración" y hace algunas suposiciones predeterminado basados en convenciones de nomenclatura de la carpeta. Por ejemplo, controladores de buscarán vistas en la carpeta Views de forma predeterminada sin tener que especificarlo explícitamente en el código. Quede con las convenciones predeterminadas reduce la cantidad de código que necesita para escribir, y también puede hacer sea más fácil para otros programadores a comprender el proyecto. Explicaremos estas convenciones más mientras compilamos nuestra aplicación.

> [!div class="step-by-step"]
> [Siguiente](mvc-music-store-part-2.md)
