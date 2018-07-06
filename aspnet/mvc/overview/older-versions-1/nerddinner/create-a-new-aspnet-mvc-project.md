---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Cree un nuevo proyecto de ASP.NET MVC | Microsoft Docs
author: microsoft
description: Paso 1 muestra cómo colocar la estructura básica de la aplicación NerdDinner en su lugar.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 88ef850503725cc57c92a11952729b4bd2205a69
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835099"
---
<a name="create-a-new-aspnet-mvc-project"></a>Cree un nuevo proyecto de ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 1 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 1 muestra cómo colocar la estructura básica de la aplicación NerdDinner en su lugar.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-1-file-gtnew-project"></a>Paso 1 de NerdDinner: Archivo -&gt;nuevo proyecto

Comenzaremos también nuestra aplicación NerdDinner seleccionando el **archivo -&gt;nuevo proyecto** elemento de menú dentro de Visual Studio 2008 o el gratuita Visual Web Developer 2008 Express.

Se abrirá el cuadro de diálogo "Nuevo proyecto". Para crear una nueva aplicación MVC de ASP.NET, se deberá seleccionar el nodo "Web" en el lado izquierdo del cuadro de diálogo y, a continuación, elija la plantilla de proyecto "Aplicación Web de ASP.NET MVC" de la derecha:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Asegúrese de que ha descargado e instalado ASP.NET MVC: en caso contrario no se mostrará en el cuadro de diálogo nuevo proyecto. Puede usar la versión V2 de la [instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) si aún no ha instalado todavía (ASP.NET MVC está disponible dentro de la "plataforma Web -&gt;marcos de trabajo y los tiempos de ejecución" sección).*

Llamaremos al nuevo proyecto, vamos a crear "NerdDinner" y, a continuación, haga clic en el botón "Aceptar" para crearlo.

Cuando hacemos clic en "Aceptar" se abrirá un cuadro de diálogo adicional que nos pedirá que, opcionalmente, cree un proyecto de prueba unitaria para la nueva aplicación en Visual Studio. Este proyecto de prueba unitaria nos permite crear pruebas automatizadas que comprueben la funcionalidad y el comportamiento de nuestra aplicación (algo que trataremos cómo la tarea más adelante en este tutorial).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

La lista desplegable de "Marco de pruebas" en el cuadro de diálogo anterior se rellena con todos los disponibles ASP.NET MVC proyecto plantillas de pruebas unitarias instaladas en el equipo. Las versiones se pueden descargar para NUnit, MBUnit y XUnit. También se admite el marco de pruebas unitarias de Visual Studio integrado.

*Nota: El marco de pruebas unitarias de Visual Studio solo está disponible con Visual Studio 2008 Professional y versiones posteriores. Si usa VS 2008 Standard Edition o Visual Web Developer 2008 Express deberá descargar e instalar las extensiones de NUnit, MBUnit o XUnit para ASP.NET MVC para que se mostrará este cuadro de diálogo. No se mostrará el cuadro de diálogo si no existen los marcos de pruebas instalados.*

Se usará el nombre de "NerdDinner.Tests" predeterminado para el proyecto de prueba que se crea y use la opción de framework "Visual Studio de prueba unitaria". Cuando hacemos clic en el botón "Aceptar" Visual Studio creará una solución para nosotros con dos proyectos en ella: uno para nuestra aplicación web y otro para nuestras pruebas unitarias:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examinar la estructura de directorios de NerdDinner

Cuando se crea una nueva aplicación MVC de ASP.NET con Visual Studio, agrega automáticamente un número de archivos y directorios al proyecto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Proyectos de ASP.NET MVC predeterminada tienen seis directorios de nivel superior:

| **Directorio** | **Propósito** |
| --- | --- |
| **Y controladores** | En la que colocar las clases de controlador que controlan las solicitudes URL |
| **/ Modelos** | En la que colocar las clases que representan y manipulan datos |
| **O vistas** | En la que colocar los archivos de plantilla de la interfaz de usuario que son responsables de la salida de representación |
| **/ Scripts** | En la que colocar los archivos de biblioteca de JavaScript y los scripts (.js) |
| **/ Content** | Dónde colocar CSS y archivos de imagen y otro contenido que no sean-dinámicos no admiten de JavaScript |
| **/ Aplicación\_datos** | Almacenar archivos de datos que desea lectura/escritura. |

ASP.NET MVC no requiere esta estructura. De hecho, los desarrolladores que trabajan en aplicaciones de gran tamaño se normalmente realizan particiones de la aplicación de seguridad en varios proyectos para que sea más fácil de administrar (por ejemplo: clases de modelo de datos entran a menudo en un proyecto de biblioteca de clases independiente de la aplicación web). Sin embargo, la estructura del proyecto de forma predeterminada, proporcionar una convención de directorio predeterminada agradable que podemos usar para mantener limpio nuestros problemas de la aplicación.

Cuando se expande el directorio /Controllers, encontraremos que Visual Studio agrega dos clases de controlador: HomeController y AccountController, de forma predeterminada al proyecto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Cuando se expande el directorio aquí se, buscaremos tres subdirectorios: / Home, / Account y /Shared –, así como plantilla de varios archivos dentro de ellos también se agregaron al proyecto de forma predeterminada:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Cuando se expande el/Content y directorios / scripts, se encontrará un archivo Site.css que se usa para definir el estilo de todo el código HTML en el sitio, así como las bibliotecas de JavaScript que pueden habilitar ASP.NET AJAX y jQuery se admiten dentro de la aplicación:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Cuando se expande el proyecto NerdDinner.Tests nos encontrará dos clases que contienen pruebas unitarias para nuestras clases de controlador:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Estos archivos de forma predeterminada, Visual Studio agregados nos proporcionan una estructura básica para una aplicación de trabajo - junto con la página de inicio, acerca de la página, las páginas de inicio de sesión y cierre de sesión/registro de cuenta y una página de error no controlado (todo conectada y funcionan de fábrica).

### <a name="running-the-nerddinner-application"></a>Ejecutar la aplicación NerdDinner

Podemos ejecutar el proyecto al seleccionar el **Debug -&gt;Iniciar depuración** o **Debug -&gt;iniciar sin depurar** elementos de menú:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Esto inicia la ASP.NET Web-servidor integrado que se incluye con Visual Studio y ejecutar la aplicación:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

A continuación aparece la página principal de nuestro nuevo proyecto (dirección URL: "/") cuando se ejecuta:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Al hacer clic en la pestaña "About" muestra una acerca de la página (dirección URL: "/ Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Al hacer clic en el vínculo "Iniciar sesión" en la parte superior derecha nos lleva a una página de inicio de sesión (dirección URL: "/ cuenta/inicio de sesión")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Si no tenemos que podemos hacer clic en el vínculo registrar una cuenta de inicio de sesión (dirección URL: "/ cuenta/Register") para crear uno:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

El código para implementar la principal anterior, sobre y el cierre de sesión / register funcionalidad se ha agregado de forma predeterminada, cuando creamos nuestro nuevo proyecto. Vamos a usar como punto de partida de nuestra aplicación.

### <a name="testing-the-nerddinner-application"></a>Probar la aplicación NerdDinner

Si usamos la Professional Edition o una versión posterior de Visual Studio 2008, podemos usar la unidad integrada, las pruebas de compatibilidad del IDE de Visual Studio para el proyecto de prueba:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Elegir una de las opciones anteriores se abra el panel "Los resultados de pruebas" en el IDE y nos proporcione con estado correcto o incorrecto en las pruebas 27 unitarias incluido en nuestro nuevo proyecto que cubren la funcionalidad integrada:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Más adelante en este tutorial crearemos un poco más acerca de las pruebas automatizadas y agregaremos pruebas unitarias adicionales que cubren la funcionalidad de la aplicación que se implemente.

### <a name="next-step"></a>Paso siguiente

Ahora tenemos una estructura de una aplicación básica en su lugar. Ahora vamos a [crear una base de datos para almacenar los datos de aplicación](create-a-database.md).

> [!div class="step-by-step"]
> [Anterior](introducing-the-nerddinner-tutorial.md)
> [Siguiente](create-a-database.md)
