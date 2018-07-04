---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Información general y archivo -> Nuevo proyecto | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 1 abarca información general y archivo -> Nuevo proyecto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: c03b62db2227c167c68ca5cf8174e4322658d39d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377894"
---
<a name="part-1-overview-and-file-new-project"></a>Parte 1: Información general y archivo -> Nuevo proyecto
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 1 abarca información general y archivo -&gt;nuevo proyecto.


## <a name="overview"></a>Información general

El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Web Developer para el desarrollo web. Se comenzará lentamente, por lo que la experiencia de desarrollo de web de nivel principiante es correcto.

La aplicación que se va a compilar es una tienda de música simple. Hay tres partes principales a la aplicación: la compra, la desprotección y la administración.

![](mvc-music-store-part-1/_static/image1.jpg)

Los visitantes pueden examinar álbumes por género:

![](mvc-music-store-part-1/_static/image2.jpg)

Pueden ver un álbum y agregarlo a su carro de compra:

![](mvc-music-store-part-1/_static/image3.jpg)

Puede revisar su carro de compra, quitar todos los elementos que ya no desean:

![](mvc-music-store-part-1/_static/image4.jpg)

Continuar con la desprotección le pedirá que inicie sesión o regístrese para una cuenta de usuario.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Después de crear una cuenta, puede completar el pedido al rellenar la información de pago y envío. Para simplificar las cosas, estamos ejecutando una promoción increíble: todo está disponible si se especifica el código de promoción "Gratis".

![](mvc-music-store-part-1/_static/image5.jpg)

Después de la ordenación, verán una pantalla de confirmación simple:

![](mvc-music-store-part-1/_static/image6.jpg)

Además de las páginas del cliente faceing, también generar una sección de administrador que se muestra una lista de álbumes desde el que los administradores pueden crear, editar y eliminar álbumes:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Archivo -&gt; nuevo proyecto

### <a name="installing-the-software"></a>Instalación del software

En este tutorial comenzará creando un nuevo proyecto de ASP.NET MVC 3 con la gratuita Visual Web Developer 2010 Express (que es gratuito) y, a continuación, vamos a agregar gradualmente funciones para crear una aplicación operativa completa. En el camino, hablaremos sobre acceso a la base de datos, escenarios de registro de formato, validación de datos, mediante páginas maestras para el diseño de página coherente, con AJAX para las actualizaciones de página y validación, el inicio de sesión de usuario y mucho más.

Puede seguir paso a paso, o bien puede descargar la aplicación completa de [MVC-música-Store](https://github.com/evilDave/MVC-Music-Store).

Puede usar Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (una versión gratuita de Visual Studio 2010) para compilar la aplicación. Vamos a usar SQL Server Compact (también disponible) para hospedar la base de datos. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.


- [Requisitos previos de visual Studio Web Developer Express SP1]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0] - incluida la compatibilidad en tiempo de ejecución y herramientas


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Crear un nuevo proyecto de ASP.NET MVC 3

Comenzaremos seleccionando "Nuevo proyecto" en el menú archivo en Visual Web Developer. Se abrirá el cuadro de diálogo nuevo proyecto.

![](mvc-music-store-part-1/_static/image5.png)

Seleccionaremos Visual C# -&gt; Web plantillas de grupo a la izquierda y, después, elija la plantilla "Aplicación Web de ASP.NET MVC 3" en la columna central. Nombre del proyecto MvcMusicStore y presione el botón Aceptar.

![](mvc-music-store-part-1/_static/image8.jpg)

Se mostrará un cuadro de diálogo secundario que nos permite realizar algunas configuraciones específicas de MVC para nuestro proyecto. Seleccione lo siguiente:

Plantilla de proyecto: seleccione vacío

Ver motor: selección de Razor

Usar marcado semántico HTML5: activada

Compruebe que la configuración tal como se muestra a continuación y presione el botón Aceptar.

![](mvc-music-store-part-1/_static/image9.jpg)

Esto creará el proyecto. Echemos un vistazo a las carpetas que se han agregado a la aplicación en el Explorador de soluciones en el lado derecho.

![](mvc-music-store-part-1/_static/image10.jpg)

La plantilla vacía MVC 3 no está completamente vacía, agrega una estructura de carpetas básica:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC hace uso de algunas convenciones de nomenclatura básicas para los nombres de carpeta:

| **Carpeta** | **Propósito** |
| --- | --- |
| **Y controladores** | Los controladores de responden a la entrada desde el explorador, decidir qué hacer con él y devolver la respuesta al usuario. |
| **O vistas** | Las vistas contienen nuestras plantillas de interfaz de usuario |
| **/ Modelos** | Modelos contienen y manipulan datos |
| **/ Content** | Esta carpeta contiene nuestras imágenes, CSS y cualquier otro contenido estático |
| **/ Scripts** | Esta carpeta contiene los archivos de JavaScript |

Estas carpetas se incluyen incluso en una aplicación MVC de ASP.NET vacía porque el marco de ASP.NET MVC predeterminada usa un enfoque de "Convención sobre configuración" y hace algunas suposiciones predeterminadas según las convenciones de nomenclatura de carpeta. Por ejemplo, los controladores de buscarán las vistas en la carpeta Views predeterminada sin tener que especificarlo explícitamente en el código. Ceñirse a las convenciones predeterminadas reduce la cantidad de código que necesita para escribir, y puede también facilitan para otros desarrolladores a comprender el proyecto. Explicaremos estas convenciones más a medida que construimos nuestra aplicación.

> [!div class="step-by-step"]
> [Siguiente](mvc-music-store-part-2.md)
