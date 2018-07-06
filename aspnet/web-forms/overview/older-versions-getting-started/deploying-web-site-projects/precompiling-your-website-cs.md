---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Precompilar el sitio Web (C#) | Microsoft Docs
author: rick-anderson
description: 'Visual Studio ofrece dos tipos de proyectos de los desarrolladores de ASP.NET: proyectos de aplicación Web (WAP) y proyectos de sitio Web (WSP). Uno de lo betwe diferencias clave...'
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: 1227c28600444ed1f2710ba1b5e021aae59fe098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840305"
---
<a name="precompiling-your-website-c"></a>Precompilar el sitio Web (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) o [descargar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio ofrece dos tipos de proyectos de los desarrolladores de ASP.NET: proyectos de aplicación Web (WAP) y proyectos de sitio Web (WSP). Una de las principales diferencias entre los dos tipos de proyecto es que WAP deben tener el código compilado de forma explícita antes de la implementación, mientras que se puede compilar automáticamente el código en un archivo WSP en el servidor web. Sin embargo, es posible precompilar un WSP antes de la implementación. Este tutorial explora las ventajas de la precompilación y muestra cómo precompilar un sitio Web desde dentro de Visual Studio y desde la línea de comandos.


## <a name="introduction"></a>Introducción

Visual Studio ofrece dos tipos de proyecto diferente de los desarrolladores de ASP.NET: los proyectos de aplicación Web (WAP) y los proyectos de sitio Web (WSP). Una de las principales diferencias entre estos tipos de proyecto es que requieren un WAP *compilación explícita* mientras que usa wsp *compilación automática*, de forma predeterminada. Con WAP, compile el código de la aplicación web en un único ensamblado, que se crea en el sitio de Web `Bin` carpeta. Implementación implica copiar el contenido de marcado (el `.aspx.ascx`, y `.master` archivos) en el proyecto, junto con el ensamblado en el `Bin` carpeta; el código subyacente no deben implementarse propios archivos de clase. Por otro lado, implementa wsp copiando las páginas de marcado y sus clases de código subyacente correspondiente en el entorno de producción. Las clases de código subyacente se compilan a petición en el servidor web.

> [!NOTE]
> Hacer referencia a la sección "Explícita compilación Versus compilación automática" en el [ *determinar qué archivos se deben implementarse* tutorial](determining-what-files-need-to-be-deployed-cs.md) para obtener más información sobre las diferencias entre el proyecto los modelos, las compilación automática y explícita y cómo afecta el modelo de compilación a la implementación.


Es fácil de usar la opción de compilación automática. No hay ningún paso de compilación explícita y sólo los archivos que se han modificado necesidad para implementarse, mientras que la compilación explícita requiere la implementación de las páginas modificadas de marcado y el ensamblado compilado just. Sin embargo, la implementación automática tiene dos desventajas potenciales:

- Dado que las páginas se deben compilar automáticamente cuando visiten en primer lugar, puede haber un retraso breve pero apreciable cuando se solicita una página ASP.NET por primera vez después de la implementación.
- Compilación automática requiere que tanto el declarativo marcado y código fuente estén presentes en el servidor web. Esto puede ser una opción más atractiva si tiene pensado en vender la aplicación web a los clientes que se instalará en sus servidores web.

Si ya sea de dos por encima de los puntos débiles son los separadores de acuerdo, se puede cambia al modelo WAP o *precompilar* WSP antes de la implementación. En este tutorial se examina las opciones de precompilación más adecuadas para un sitio Web hospedado y le guía a través del proceso de precompilación e implementación de un sitio Web precompilado.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Información general de la compilación y generación de código de ASP.NET

Antes de adentrarnos en las opciones de precompilación disponibles, primero explicaremos la generación de código y compilación que tiene lugar cuando se solicita una página ASP.NET por primera vez desde que se ha creado o actualizado por última vez. Como sabe, las páginas ASP.NET se componen de dos partes: el marcado declarativo en la `.aspx` archivo; y una parte de código fuente, normalmente en un archivo de clase de código subyacente independiente (`.aspx.cs`). Los pasos realizados por el tiempo de ejecución cuando se solicita una página ASP.NET depende del modelo de compilación de la aplicación.

Con WAP, código fuente de las páginas debe compilarse explícitamente en un único ensamblado antes de implementarlo. Durante la implementación, este ensamblado y las diversas páginas de marcado se copian en el entorno de producción. Cuando llega una solicitud al servidor web para una página ASP.NET, el tiempo de ejecución crea una instancia de clase de código subyacente de la página e invoca su `ProcessRequest` método, que se inicia el ciclo de vida de página y, en última instancia, se genera el contenido de la página, que se devuelve al el solicitante. El tiempo de ejecución puede trabajar con la clase de código subyacente de la página ASP.NET porque la clase de código subyacente ya se ha compilado en un ensamblado antes de la implementación.

Con wsp y compilación automática, no hay ningún paso de compilación explícita antes de la implementación. En su lugar, la implementación implica copiar declarativo y el contenido de código de origen al entorno de producción. Cuando llega una solicitud al servidor web para una página ASP.NET por primera vez desde que se ha creado o actualizado por última vez la página, el tiempo de ejecución en primer lugar debe compilar la clase de código subyacente en un ensamblado. Este ensamblado compilado se guarda en la carpeta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, aunque la ubicación de esta carpeta se puede personalizar mediante la `<compilation tempDirectory="" />` elemento de `<system.web>`, normalmente en `Web.config`. Dado que el ensamblado está guardado en el disco, no es necesario volver a compilar en solicitudes posteriores a la misma página.

> [!NOTE]
> Como cabría esperar, hay un ligero retraso cuando se solicita una página por primera vez (o por primera vez desde que se ha cambiado) en un sitio que utiliza la compilación automática como tardará un momento para el servidor al compilar el código de la página y guarde el ensamblado resultante disco.


En resumen, con la compilación explícita debe compilar el código de fuente del sitio Web antes de la implementación, evita que el tiempo de ejecución tenga que realizar ese paso. Compilación automática con el tiempo de ejecución controla la compilación de código fuente de las páginas, pero con un costo de inicialización ligero para la primera visita a la página desde que se creó o actualizó por última vez.

Pero ¿qué sucede con la parte de las páginas ASP.NET declarativa (el `.aspx` archivo)? Es obvio que hay una relación entre el `.aspx` archivos y el código en sus clases de código subyacente, como los controles Web definidos en el marcado declarativo son accesibles en el código. También es obvio que el contenido de la `.aspx` archivos influye en gran medida en el marcado representado generado por la página. ¿Cómo funciona el tiempo de ejecución con el texto, HTML, y la sintaxis del control Web definen en el `.aspx` contenido representado del archivo para generar la página solicitada?

No quiero distraerme demasiado en detalles de implementación de bajo nivel que varían entre WAP y wsp, pero para resumir el tiempo de ejecución genera automáticamente un archivo de clase que contiene los distintos controles Web como métodos y miembros protegidos. Este archivo generado se implementa como un *clase parcial* a la clase de código subyacente correspondiente. ([Clases parciales](http://www.dotnet-guide.com/partialclasses.html) permitir para el contenido de una clase solo se distribuyan entre varios archivos.) Por lo tanto, se define la clase de código subyacente en dos lugares: en el `.aspx.cs` archivo que cree y, en esta clase autogenerada creado por el tiempo de ejecución. Esta clase autogenerada se almacena en el `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` carpeta.

Lo importante evitarle aquí es que para una página ASP.NET ser representado por el tiempo de ejecución tanto su declarativo y partes del código fuente deben compilarse en un ensamblado. Con WAP, el código fuente se compila explícitamente en un ensamblado antes de la implementación, pero todavía se debe convertir en código y compila el tiempo de ejecución en el servidor web el marcado declarativo. Con wsp utilizando las compilación automática, el código fuente y el marcado declarativo deben compilarse en el servidor web.

Es posible utilizar la compilación explícita con el modelo de WSP. Puede compilar explícitamente la parte de código fuente, al igual que con el modelo de WAP. Además, también puede compilar el marcado declarativo.

## <a name="precompilation-options"></a>Opciones de precompilación

.NET Framework se distribuye con un [herramienta de compilación de ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) que le permite compilar el código fuente (e incluso el contenido) de una aplicación ASP.NET compilada mediante el modelo WSP. Esta herramienta se lanzó con la versión 2.0 de .NET Framework y se encuentra en la `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` carpeta; puede ser utilizado desde la línea de comandos o iniciarse desde Visual Studio a través de la opción de publicar el sitio Web del menú de la compilación.

La herramienta de compilación proporciona dos formatos generales de la compilación: precompilación y compilación previa para la implementación en el contexto. Con la precompilación local ejecuta el `aspnet_compiler.exe` de la herramienta desde la línea de comandos y especifique la ruta de acceso al directorio virtual o una ruta de acceso física de un sitio Web que se encuentra en el equipo. La herramienta de compilación, a continuación, compila cada página ASP.NET en el proyecto, almacenar la versión compilada en el `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` carpeta igual que si las páginas de cada uno de ellos habíamos visitadas por primera vez desde un explorador. Precompilación en contexto puede acelerar la primera solicitud realizada a las páginas ASP.NET recién implementadas en el sitio porque palía el tiempo de ejecución de la necesidad de realizar este paso. Sin embargo, no es útil para la mayoría de los sitios Web hospedados precompilación en contexto porque requiere que se pueden ejecutar programas desde el servidor web de línea de comandos. En entornos de hospedaje compartidos no se permite este nivel de acceso.

> [!NOTE]
> Para obtener más información sobre la precompilación en contexto, consulte [How To: Precompilar sitios Web de ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) y [precompilación en ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


En lugar de compilar las páginas en el sitio Web para el `Temporary ASP.NET Files` precompilación para la implementación de carpeta, compila las páginas en un directorio de su elección y en un formato que se puede implementar en el entorno de producción.

Hay dos tipos de precompilación para la implementación que se explorará en este tutorial: compilación previa con una interfaz de usuario actualizable y precompilación con una interfaz de usuario que no es actualizable. Precompilación con una interfaz de usuario actualizable deja el marcado declarativo en la `.aspx`, `.ascx`, y `.master` archivos, lo que permite que un desarrollador ver y, si lo desea, modifique el marcado declarativo en el servidor de producción. Genera la precompilación con una interfaz de usuario que no son actualizables `.aspx` páginas que sean de tipo void de cualquier contenido y quita `.ascx` y `.master` archivos, con lo que se oculte el marcado declarativo y prohibiendo que un desarrollador cambia de la entorno de producción.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Precompilar para la implementación con una interfaz de usuario actualizable

Es la mejor manera de comprender la precompilación para la implementación ver un ejemplo en acción. Vamos a precompilar WSP revisiones de libro para la implementación con una interfaz de usuario actualizable. La herramienta de compilación de ASP.NET se puede invocar desde el menú de compilación de Visual Studio o desde la línea de comandos. Esta sección examinan con la herramienta desde dentro de Visual Studio; la sección "precompilar desde la línea de comandos" examina ejecuta la herramienta de compilador desde la línea de comandos.

Abra el libro revisión WSP en Visual Studio, vaya al menú de compilación y seleccione la opción de menú Publicar sitio Web. Esto inicia el cuadro de diálogo Publicar sitio Web (consulte **figura 1**), donde puede especificar la ubicación de destino, si la interfaz de usuario del sitio precompilado es actualizable y otras opciones de herramienta del compilador. La ubicación de destino puede ser un servidor web remoto o el servidor FTP, pero por ahora, elija una carpeta en la unidad de disco duro del equipo. Puesto que deseamos precompilar el sitio con una interfaz de usuario actualizable, deje activada la casilla "Permitir que este sitio precompilado se actualice" y haga clic en Aceptar.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Figura 1**: la herramienta de compilación de ASP.NET precompila el sitio Web a la ubicación de destino especificado  
 ([Haga clic aquí para ver imagen en tamaño completo](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> La opción de publicar el sitio Web en el menú de la compilación no está disponible en Visual Web Developer. Si está usando Visual Web Developer necesita usar la versión de línea de comandos de la herramienta de compilación de ASP.NET, que se trata en la sección "precompilar desde la línea de comandos".


Después de precompilar el sitio Web, vaya a la ubicación de destino que especificó en el cuadro de diálogo Publicar sitio Web. Dedique un momento para comparar el contenido de esta carpeta con el contenido de su sitio Web. **Figura 2** muestra la carpeta del sitio Web de reseñas de libros. Tenga en cuenta que contiene `.aspx` y `.aspx.cs` archivos. Además, tenga en cuenta que el `Bin` directorio incluye un único archivo, `Elmah.dll`, que se agregó en el [anterior del tutorial](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Figura 2**: el directorio del proyecto contiene `.aspx` y `.aspx.cs` archivos; el `Bin` carpeta simplemente incluye `Elmah.dll`  
 ([Haga clic aquí para ver imagen en tamaño completo](precompiling-your-website-cs/_static/image6.png))

**Figura 3** muestra la carpeta de ubicación de destino cuyo contenido se crearon mediante la herramienta de compilación de ASP.NET. Esta carpeta no contiene los archivos de código subyacente. Además, esta carpeta `Bin` directorio incluye varios ensamblados y dos `.compiled` archivos además el `Elmah.dll` ensamblado.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Figura 3**: la carpeta de ubicación de destino incluye los archivos para la implementación  
 ([Haga clic aquí para ver imagen en tamaño completo](precompiling-your-website-cs/_static/image9.png))

A diferencia de una compilación explícita de WAP, la precompilación para el proceso de implementación no crea un ensamblado para todo el sitio. En su lugar, procesa por lotes juntos varias páginas en cada ensamblado. También se compila el `Global.asax` archivo (si existe) en su propio ensamblado, así como las clases en el `App_Code` carpeta. Los archivos que contienen el marcado declarativo para ASP.NET web páginas, controles de usuario y las páginas maestras (`.aspx`, `.ascx`, y `.master` archivos, respectivamente) se copian como-consiste en el directorio de la ubicación de destino. Del mismo modo, el `Web.config` archivo se copia directamente a través, junto con los archivos estáticos, como imágenes, las clases CSS y archivos PDF. Para obtener una descripción más formal de cómo la herramienta de compilación administra distintos tipos de archivo, consulte [de control de archivos durante la precompilación ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Puede indicar a la herramienta de compilación para crear un ensamblado por cada página ASP.NET, Control de usuario o página maestra activando la casilla "Utilizado se ha corregido nombres y ensamblados de una sola página" en el cuadro de diálogo Publicar sitio Web. Tener cada página ASP.NET que se compilan en su propio ensamblado permite un control más minucioso sobre la implementación. Por ejemplo, si actualiza una única página web ASP.NET y necesarios para implementar ese cambio, si solo necesita implementar esa página `.aspx` archivo y ensamblado asociado al entorno de producción. Consulte [Cómo: generar nombres fijos con la herramienta de compilación de ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) para obtener más información.


El directorio de la ubicación de destino también contiene un archivo que no es parte del proyecto web precompilado, es decir, `PrecompiledApp.config`. Este archivo informa a la ejecución de ASP.NET que se precompiló la aplicación y si se precompiló con una interfaz de usuario actualizable o mediodía actualizable.

Por último, dedique un momento para abrir uno de los `.aspx` archivos en la ubicación de destino mediante Visual Studio o el editor de texto que prefiera. Al precompilar para la implementación con una interfaz de usuario actualizable, las páginas ASP.NET en el directorio de la ubicación de destino contienen el mismo marcado exacto que los archivos correspondientes en el sitio Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Precompilar para la implementación con una interfaz de usuario no actualizable

También se puede usar la herramienta de compilador ASP.NET para precompilar un sitio para la implementación con una interfaz de usuario que no es actualizable. Precompilar el sitio con una interfaz de usuario que no son actualizables funciona muy parecido al precompilar con una interfaz de usuario actualizable, la principal diferencia radica en que las páginas ASP.NET, controles de usuario y las páginas principales en el directorio de destino se quitan de su marcado. Para precompilar un sitio Web para la implementación con una interfaz de usuario que no es actualizable, elija la opción de publicar el sitio Web en el menú compilar, pero desactive la opción "Permitir que este sitio precompilado se actualice" (consulte **figura 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Figura 4**: desactive la "Permitir que este sitio precompilado se actualice" opción para precompilar con unos que no son actualizables interfaz de usuario  
 ([Haga clic aquí para ver imagen en tamaño completo](precompiling-your-website-cs/_static/image12.png))

**Figura 5** muestra la carpeta de ubicación de destino después de precompilar con una interfaz de usuario que no es actualizable.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Figura 5**: la carpeta de ubicación de destino para la implementación con una interfaz de usuario no actualizable  
 ([Haga clic aquí para ver imagen en tamaño completo](precompiling-your-website-cs/_static/image15.png))

Comparar **figura 3** a **figura 5**. Mientras que las dos carpetas sean idénticas, tenga en cuenta que la carpeta de la interfaz de usuario que no son actualizables carece de la página maestra, `Site.master`. Y mientras **figura 5** incluye las diversas páginas ASP.NET, si ve el contenido de estos archivos verá que ha sido su marcado declarativo que se han quitado y reemplazado con el texto de marcador de posición: "Este es un archivo de marcador generado por la herramienta de la precompilación y no deben eliminarse! "

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Figura 5**: se ha quitado el marcado declarativo de las páginas ASP.NET

El `Bin` carpetas en **figuras 3** y **5** difieren considerablemente más. Además de los ensamblados, el `Bin` carpeta **figura 5** incluye un `.compiled` archivo para cada página ASP.NET, el Control de usuario y la página maestra.

Precompilar un sitio con una interfaz de usuario no actualizable es útil en situaciones donde no desea contenido de las páginas de ASP.NET que va a modificar la persona o empresa que instala o administra el sitio Web en el entorno de producción. Si compila una aplicación web ASP.NET que vende a los clientes para instalar en sus propios servidores web, es posible que desee para asegurarse de que no modifican la apariencia y funcionamiento de su sitio editando directamente el `.aspx` páginas que los incluyen. Al precompilar el sitio Web con una interfaz de usuario que no es actualizable, incluyen el marcador de posición `.aspx` páginas como parte de la instalación, lo que impide a los clientes de examinar o modificar su contenido.

### <a name="precompiling-from-the-command-line"></a>Precompilar desde la línea de comandos

En segundo plano, el cuadro de diálogo Publicar sitio Web de Visual Studio invoca la herramienta de compilación de ASP.NET (`aspnet_compiler.exe`) para precompilar el sitio Web. Como alternativa, puede invocar esta herramienta desde la línea de comandos. De hecho, si utiliza Visual Web Developer, a continuación, deberá ejecutar la herramienta de compilador desde la línea de comandos, como menús de Visual Web Developer compilación no incluyen la opción de publicar el sitio Web.

Para usar la herramienta compilador de la línea de comandos, iniciar, dejando en la línea de comandos y navegue hasta el directorio del marco, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. A continuación, escriba la siguiente instrucción en la línea de comandos:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

El comando anterior inicia la herramienta de compilador ASP.NET (`aspnet_compiler.exe`) y, a través de la `-p` conmutador, le indica que precompilar el sitio Web cuya raíz comienza en *físico\_ruta\_a\_aplicación*; Este valor será algo como `C:\MySites\BookReviews`y debe delimitarse con comillas.

El `-v` conmutador especifica el directorio virtual del sitio. Si el sitio está registrado como el sitio Web predeterminado en la metabase de IIS, a continuación, puede omitir el `-p` cambie y simplemente especificar el directorio virtual de la aplicación. Si usas el `-p` cambia, el procedimiento de valor la `-v` conmutador indica la raíz del sitio Web y se usa para resolver las referencias de raíz de la aplicación. Por ejemplo, si especifica un valor de `-v /MySite` , a continuación, hace referencia en la aplicación para `~/path/file` se resolverá como `~/MySite/path/file`. Dado que el sitio de reseñas de libros se encuentra en el directorio raíz en mi empresa de hospedaje web se ha utilizado el conmutador `-v /`.

El `-f` , si está presente, indica a la herramienta de compilación para sobrescribir el *destino\_ubicación\_carpeta* directorio si ya existe. Si se omite la `-f` conmutador y la carpeta de ubicación de destino ya existe, se cerrará la herramienta de compilación con el error: "error ASPRUNTIME: el directorio de destino no está vacío. Elimínelo manualmente o elija un destino diferente."

El `-u` conmutador, si está presente, informa a la herramienta para crear una interfaz de usuario actualizable. Omita este modificador para precompilar el sitio con una interfaz de usuario que no es actualizable.

Por último, el *destino\_ubicación\_carpeta* es la ruta de acceso física al directorio de la ubicación de destino; este valor será algo como `C:\MySites\Output\BookReviews`y debe delimitarse con comillas.

## <a name="deploying-the-precompiled-website"></a>Implementar el sitio Web precompilado

En este punto hemos visto cómo usar la herramienta de compilación de ASP.NET para precompilar un sitio Web con ambos las opciones de la interfaz de usuario actualizables y que no son actualizables. Sin embargo, los ejemplos hasta ahora han precompilado el sitio Web en una carpeta local y no al entorno de producción. La buena noticia es que implementar el sitio Web precompilado es muy sencilla y puede hacerse a través de Visual Studio o algún otro mecanismo de copia archivos, como desde un cliente FTP independiente.

El cuadro de diálogo Publicar sitio Web (mostrada en primer lugar **figura 1**) tiene una opción de ubicación de destino, que indica donde se copian los archivos del sitio Web precompilado. Esta ubicación puede ser un servidor web remoto o el servidor FTP. Especificar un servidor remoto en este cuadro de texto se precompila y el sitio Web implementa en el servidor especificado en un solo paso. Como alternativa, puede precompilar el sitio Web en una carpeta local y, a continuación, copie manualmente el contenido de esa carpeta en el entorno de producción a través de FTP o algún otro enfoque.

Tener el sitio Web precompilado se implementan automáticamente a través del cuadro de diálogo Publicar sitio Web de Visual Studio es útil para sitios sencillos donde no hay ninguna diferencia de la configuración entre entornos de desarrollo y producción. Sin embargo, como se indicó en el [ *Common Configuration diferencias entre desarrollo y producción* tutorial](common-configuration-differences-between-development-and-production-cs.md) no es raro que tales diferencias que existen. Por ejemplo, la aplicación web de reseñas de libros usa una base de datos diferentes en el entorno de producción que en el entorno de desarrollo. Cuando Visual Studio publica el sitio Web en un servidor remoto ciegamente se copia el archivo la información de configuración del entorno de desarrollo.

Para sitios con diferencias de configuración entre entornos de desarrollo y producción puede ser mejor precompilar el sitio en un directorio local, copie los archivos de configuración específicos de la producción y, a continuación, copie el contenido de la salida precompilado en producción.

Para hacer un repaso sobre cómo copiar archivos desde el entorno de desarrollo al entorno de producción, consulte el [ *implementar su sitio Web mediante un cliente FTP* ](deploying-your-site-using-an-ftp-client-cs.md) y [  *Implementación de su sitio Web con Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md) tutoriales.

## <a name="summary"></a>Resumen

ASP.NET admite dos modos de compilación: automático y explícito. Como se describe en los tutoriales anteriores, los proyectos de aplicación Web (WAP) utilizar la compilación explícita mientras que los proyectos de sitio Web (WSP) usan las compilación automática, de forma predeterminada. Sin embargo, es posible compilar explícitamente un archivo WSP antes de la implementación mediante la herramienta de compilación de ASP.NET.

En este tutorial se centra en la compilación previa de la herramienta de compilación para la compatibilidad con la implementación. Al precompilar para la implementación, la herramienta de compilación crea una carpeta de ubicación de destino, compila código fuente de la aplicación web especificado y copia estos compila los ensamblados y los archivos de contenido en la carpeta de ubicación de destino. La herramienta de compilación puede configurarse para crear una interfaz de usuario actualizable o no actualizable. Al precompilar con una opción de interfaz de usuario que no es actualizable, se quita el marcado declarativo en los archivos de contenido. En pocas palabras, precompilación le permite implementar su aplicación basada en el proyecto de sitio Web sin necesidad de incluir los archivos de código fuente y con el marcado declarativo que se quita, si lo desea.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Precompilar el sitio de Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Código subyacente y la compilación en ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Precompilación en ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opciones de sitio precompilado en ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-elmah-cs.md)
> [Siguiente](users-and-roles-on-the-production-website-cs.md)
