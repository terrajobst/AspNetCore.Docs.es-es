---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Cree un nuevo proyecto de MVC de ASP.NET | Documentos de Microsoft
author: microsoft
description: "Paso 1 muestra cómo colocar la estructura de aplicación NerdDinner básica en su lugar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 4d30a6803b1478014a2afb814ac317df27394446
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-a-new-aspnet-mvc-project"></a>Cree un nuevo proyecto de MVC de ASP.NET
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 1 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 1 muestra cómo colocar la estructura de aplicación NerdDinner básica en su lugar.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-1-file-gtnew-project"></a>Paso 1 de NerdDinner: Archivo -&gt;nuevo proyecto

Comenzaremos nuestra aplicación NerdDinner seleccionando el **archivo -&gt;nuevo proyecto** elemento de menú dentro de Visual Studio 2008 o el libre Visual Web Developer 2008 Express.

Se abrirá el cuadro de diálogo "Nuevo proyecto". Para crear una nueva aplicación MVC de ASP.NET, se podrá seleccionar el nodo de "Web" en el lado izquierdo del cuadro de diálogo y, a continuación, elija la plantilla de proyecto de "Aplicación Web de ASP.NET MVC" de la derecha:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Asegúrese de que haya descargado e instalado ASP.NET MVC - en caso contrario no aparece en el cuadro de diálogo nuevo proyecto. Puede usar V2 de la [instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) si no ha instalado todavía (ASP.NET MVC está disponible dentro de la "plataforma Web -&gt;marcos y tiempos de ejecución" sección).*

Se le asigne al nuevo proyecto que vamos a crear "NerdDinner" y, a continuación, haga clic en el botón "Aceptar" para crearla.

Cuando se hace clic en "Aceptar" Visual Studio abrirá un cuadro de diálogo adicional que se le solicita, opcionalmente, crear un proyecto de prueba unitaria para la nueva aplicación. Este proyecto de prueba unitaria nos permite crear pruebas automatizadas que comprueban la funcionalidad y el comportamiento de la aplicación (algo hablaremos sobre cómo tareas más adelante en este tutorial).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

La lista desplegable de "Marco de pruebas" en el cuadro de diálogo anterior se rellena con todos los disponibles ASP.NET MVC proyecto plantillas de pruebas unitarias instaladas en el equipo. Las versiones se pueden descargar para NUnit, MBUnit y XUnit. También se admite el marco de pruebas unitarias de Visual Studio integrado.

*Nota: El marco de pruebas unitarias de Visual Studio solo está disponible con Visual Studio 2008 Professional y versiones posteriores. Si usas VS 2008 Standard Edition o Visual Web Developer 2008 Express debe descargar e instalar las extensiones de NUnit, MBUnit o XUnit para ASP.NET MVC en orden para que se mostrará este cuadro de diálogo. El cuadro de diálogo no se mostrará si no hay ningún marco de pruebas instalado.*

Se utilizará el nombre de "NerdDinner.Tests" predeterminado para el proyecto de prueba que se crea y utilice la opción del marco "Visual Studio prueba unitaria". Cuando se hace clic en el botón "Aceptar" Visual Studio creará una solución para que podamos con dos proyectos en ella: uno para nuestra aplicación web y otro para nuestras pruebas unitarias:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Examinar la estructura de directorios NerdDinner

Cuando se crea una nueva aplicación MVC de ASP.NET con Visual Studio, agrega automáticamente un número de archivos y directorios en el proyecto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Proyectos de ASP.NET MVC predeterminada tienen seis directorios de nivel superior:

| **Directorio** | **Propósito** |
| --- | --- |
| **/ Controladores** | Dónde colocar las clases de controlador que controlan las solicitudes de dirección URL |
| **/ Modelos** | En la que colocar las clases que representan y manipulan datos |
| **/ Vistas** | Dónde colocar los archivos de plantilla de interfaz de usuario que son responsables de una salida de representación |
| **/ Secuencias de comandos** | Dónde colocar scripts (.js) y archivos de la biblioteca de JavaScript |
| **/ Content** | Dónde se colocan el CSS y archivos de imagen y otro tipo de contenido no-dinámicos no admiten de JavaScript |
| **/ Aplicación\_datos** | Donde almacenar los archivos de datos que desee a lectura/escritura. |

ASP.NET MVC no requiere esta estructura. De hecho, los desarrolladores que trabajan en aplicaciones de gran tamaño será normalmente particionar la aplicación de seguridad en varios proyectos para que sea más fácil de administrar (por ejemplo: clases del modelo de datos entran a menudo en un proyecto de biblioteca de clases independiente de la aplicación web). La estructura del proyecto de forma predeterminada, obstante, ofrece una convención de directorio predeterminado "nice" que podemos usar para mantener lo que nos ocupa aplicación limpio.

Cuando se expande el directorio /Controllers buscaremos que Visual Studio agrega dos clases de controlador: HomeController y AccountController: de forma predeterminada al proyecto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Cuando se expande el directorio /Views, buscaremos tres subdirectorios: / Home, AccountType y /Shared –, así como plantilla de varios archivos dentro de ellas también se agregaron al proyecto de forma predeterminada:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Cuando se expandan los directorios / scripts y/Content, buscaremos un archivo Site.css que se usa para definir el estilo de todo el código HTML en el sitio, así como las bibliotecas de JavaScript que pueden habilitar ASP.NET AJAX y jQuery admiten dentro de la aplicación:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Cuando se expande el proyecto NerdDinner.Tests buscaremos dos clases que contienen pruebas unitarias para las clases de controlador:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Estos archivos de forma predeterminada, Visual Studio agregados nos proporcionan una estructura básica para una aplicación operativa - junto con la página de inicio, acerca de la página, las páginas de inicio de sesión o cierre de sesión/registro de cuenta y una página de error no controlado (todos los vertical con cable y funciona de fábrica).

### <a name="running-the-nerddinner-application"></a>Ejecutar la aplicación NerdDinner

Podemos ejecutar el proyecto eligiendo en la **Debug -&gt;Iniciar depuración** o **Debug -&gt;iniciar sin depurar** elementos de menú:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Esto se inicie el ASP.NET Web-servidor integrado que se incluye con Visual Studio y ejecute nuestra aplicación:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

A continuación se muestra la página principal de nuestro nuevo proyecto (dirección URL: "/") cuando se ejecuta:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Haga clic en la ficha "About" Mostrar una acerca de la página (dirección URL: "/ principal/sobre"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Haga clic en el vínculo "Iniciar sesión" en la parte superior derecha nos lleva a una página de inicio de sesión (dirección URL: "/ Account/inicio de sesión")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Si no tenemos una cuenta de inicio de sesión, podemos hacer clic en el vínculo de registro (dirección URL: "/ Account/Register") para crear una:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

El código para implementar el inicio anterior, sobre y el cierre de sesión / register funcionalidad se agregó de forma predeterminada cuando se crea el nuevo proyecto. Vamos a usar como punto de partida de nuestra aplicación.

### <a name="testing-the-nerddinner-application"></a>Probar la aplicación NerdDinner

Si usamos el Professional Edition o una versión posterior de Visual Studio 2008, podemos usar la comprobación de compatibilidad del IDE de Visual Studio de unidades integradas para el proyecto de prueba:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Elegir una de las opciones anteriores se abra el panel de "resultados de prueba" en el IDE y proporcionarnos éxito/error de estado de las pruebas de 27 unidad incluidas en nuestro nuevo proyecto que cubren la funcionalidad integrada:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Más adelante en este tutorial se podrá comunicar más información acerca de las pruebas automatizadas y agregar pruebas unitarias adicionales que cubren la funcionalidad de la aplicación que se implementa.

### <a name="next-step"></a>Paso siguiente

Ahora tenemos una estructura de una aplicación básica en su lugar. Ahora vamos a [crear una base de datos para almacenar los datos de aplicación](create-a-database.md).

>[!div class="step-by-step"]
[Anterior](introducing-the-nerddinner-tutorial.md)
[Siguiente](create-a-database.md)
