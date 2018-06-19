---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Precompilar el sitio Web (VB) | Documentos de Microsoft
author: rick-anderson
description: 'Visual Studio ofrece a los programadores de ASP.NET dos tipos de proyectos: proyectos de aplicación Web (WAP) y proyectos de sitios Web (WSPs). Una de la diferencias clave betwe...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 7296808480fa48b4afd0b308cd27707378519747
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889961"
---
<a name="precompiling-your-website-vb"></a>Precompilar el sitio Web (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio ofrece a los programadores de ASP.NET dos tipos de proyectos: proyectos de aplicación Web (WAP) y proyectos de sitios Web (WSPs). Una de las diferencias principales entre los dos tipos de proyecto es que WAP debe tener el código compilado explícitamente antes de la implementación, mientras que el código en un WSP se puede compilar automáticamente en el servidor web. Sin embargo, es posible precompilar un WSP antes de la implementación. Este tutorial explora las ventajas de la precompilación y muestra cómo precompilar un sitio Web desde dentro de Visual Studio y desde la línea de comandos.


## <a name="introduction"></a>Introducción

Visual Studio ofrece dos tipos de proyecto diferente de los programadores de ASP.NET: los proyectos de aplicación Web (WAP) y proyectos de sitio Web (WSP). Una de las diferencias principales entre estos tipos de proyecto es que requiere WAP *compilación explícita* mientras que los usan WSPs *compilación automática*, de forma predeterminada. Con puntos de acceso inalámbrico, se compila código de la aplicación web en un único ensamblado, que se crea en el sitio de Web `Bin` carpeta. Implementación implica copiar el contenido de marcado (el `.aspx.ascx`, y `.master` archivos) en el proyecto, junto con el ensamblado en el `Bin` carpeta; el código subyacente no es necesario implementar archivos de clase por sí mismos. Por otro lado, implemente WSPs copiando las páginas de marcado y sus clases de código subyacente correspondiente en el entorno de producción. Las clases de código subyacente se compilan a petición en el servidor web.

> [!NOTE]
> Hacen referencia a la sección "Explícita compilación Versus compilación automática" en la [ *determinar qué archivos deben implementarse* tutorial](determining-what-files-need-to-be-deployed-vb.md) para obtener más información sobre las diferencias entre el proyecto modelos, compilación automática y explícita y cómo afecta el modelo de compilación a la implementación.


La opción de compilación automática es fácil de usar. No hay ningún paso de compilación explícito y sólo los archivos que se han modificado necesidad para implementarse, mientras que la compilación explícita necesita implementar las páginas modificadas de marcado y el ensamblado compilado just. Sin embargo, la implementación automática tiene dos posibles inconvenientes:

- Dado que las páginas se deben compilar automáticamente cuando visitaban en primer lugar, puede existir un retraso breve pero apreciable cuando se solicita una página ASP.NET por primera vez después de la implementación.
- Compilación automática requiere que tanto el marcado y código fuente código declarativo estén presentes en el servidor web. Esto puede ser una opción poco atractivo si tiene pensado en vender la aplicación web para los clientes que se instalará en sus servidores web.

Si puede ser de dos por encima de las deficiencias son separadores de acuerdo, puede cambia al modelo WAP o *precompilar* el WSP antes de la implementación. Este tutorial examina las opciones de precompilación más adecuadas para un sitio Web hospedado y le guía a través del proceso de precompilación e implementación de un sitio Web precompilado.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Información general sobre la compilación y generación de código de ASP.NET

Antes de adentrarnos en las opciones de precompilación disponibles, en primer lugar Hablemos sobre la generación de código y la compilación que tiene lugar cuando se solicita una página ASP.NET por primera vez desde que se ha creado o actualizó por última vez. Como sabe, las páginas ASP.NET se componen de dos partes: marcado declarativo en la `.aspx` archivo y una parte de código fuente, normalmente se encuentra en un archivo de clase de código subyacente independiente (`.aspx.vb`). Los pasos realizados por el tiempo de ejecución cuando se solicita una página ASP.NET depende del modelo de compilación de la aplicación.

Con WAP, código fuente de las páginas debe compilarse explícitamente en un único ensamblado antes de implementarse. Durante la implementación, este ensamblado y las distintas páginas de marcado se copian en el entorno de producción. Cuando llega una solicitud al servidor web para una página ASP.NET, el tiempo de ejecución crea una instancia de la clase de código subyacente de la página e invoca su `ProcessRequest` método, que inicia el ciclo de vida de la página y, en última instancia, se genera el contenido de la página, que se devuelve a el solicitante. El tiempo de ejecución puede trabajar con la clase de código subyacente de la página ASP.NET porque la clase de código subyacente ya se ha compilado en un ensamblado antes de la implementación.

Con WSPs y compilación automática, no hay ningún paso de compilación explícita antes de la implementación. En su lugar, la implementación implica copiar declarativo y el contenido de código de origen en el entorno de producción. Cuando llega una solicitud al servidor web para una página ASP.NET por primera vez desde que se ha creado o actualizó por última vez la página, el tiempo de ejecución debe compilar primero la clase de código subyacente en un ensamblado. Este ensamblado compilado se guarda en la carpeta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, aunque se puede personalizar la ubicación de esta carpeta a través de la `<pages>` elemento `Web.config`. Dado que el ensamblado está guardado en el disco, no es necesario volver a compilar en solicitudes posteriores a la misma página.

> [!NOTE]
> Como cabría esperar, hay un breve lapso de tiempo cuando se solicita una página por primera vez (o por primera vez, ya que se ha cambiado) en un sitio que usa una compilación automática que lleve un momento para el servidor para compilar el código de la página y guarde el ensamblado resultante a disco.


En resumen, con la compilación explícita debe compilar el código de origen del sitio Web antes de la implementación, guardar el tiempo de ejecución de tener que realizar ese paso. Compilación automática con el tiempo de ejecución controla la compilación de código fuente de las páginas, pero con un costo de inicialización ligero para la primera vez que visita la página desde que se creó o actualizó por última vez.

Pero ¿qué ocurre con la parte de las páginas ASP.NET declarativa (el `.aspx` archivo)? Esto resulta evidente que hay una relación entre el `.aspx` archivos y el código de sus clases de código subyacente, como los controles Web definidos en el marcado declarativo son accesibles en el código. También es obvio que el contenido de la `.aspx` archivos influye en gran medida en la marca presentada generada por la página. ¿Cómo funciona el tiempo de ejecución con el texto, HTML, y la sintaxis del control Web definidos en el `.aspx` archivo para generar la página solicitada del contenido representado?

No deseo obtener sidetracked demasiado en los detalles de implementación de bajo nivel, que varían entre los puntos de acceso inalámbrico y WSPs, pero en pocas palabras el tiempo de ejecución genera automáticamente un archivo de clase que contiene los distintos controles Web como métodos y miembros protegidos. Este archivo generado se implementa como un *clase parcial* a la clase de código subyacente correspondiente. ([Clases parciales](http://www.dotnet-guide.com/partialclasses.html) permitir que el contenido de una sola clase pueden distribuir en varios archivos.) Por lo tanto, se define la clase de código subyacente en dos lugares: en el `.aspx.vb` archivo que crean y en esta clase autogenerada creados por el tiempo de ejecución. Esta clase autogenerada se almacena en la `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` carpeta.

Lo importante quitarle aquí es que para una página ASP.NET representar el tiempo de ejecución tanto su declarativa y partes del código fuente deben compilarse en un ensamblado. Con puntos de acceso inalámbrico, el código fuente se compila explícitamente en un ensamblado antes de la implementación, pero el marcado declarativo aun así se convierten en código y compilar en tiempo de ejecución en el servidor web. Con WSPs con una compilación automática, el código fuente y el marcado declarativo deben compilarse en el servidor web.

Es posible utilizar la compilación explícita con el modelo WSP. Puede compilar explícitamente la parte de código fuente, al igual que con el modelo WAP. Además, también puede compilar el marcado declarativo.

## <a name="precompilation-options"></a>Opciones de precompilación

.NET Framework se suministra con un [herramienta de compilación de ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) que le permite compilar el código fuente (e incluso el contenido) de una aplicación ASP.NET compilada mediante el modelo WSP. Esta herramienta se comercializó junto con la versión 2.0 de .NET Framework y se encuentra en la `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` carpeta; se puede utilizar desde la línea de comandos o iniciarse desde Visual Studio a través de la opción de publicar el sitio Web del menú de la compilación.

La herramienta de compilación proporciona dos formatos generales de la compilación: precompilación y precompilación para la implementación en el contexto. Con la precompilación en contexto se ejecuta el `aspnet_compiler.exe` herramienta desde la línea de comandos y especifique la ruta de acceso al directorio virtual o la ruta de acceso física de un sitio Web que se encuentra en el equipo. La herramienta de compilación, a continuación, compila cada página ASP.NET en el proyecto, almacenar la versión compilada en el `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` carpeta igual que si las páginas tenían cada uno de ellos ha visitado por primera vez desde un explorador. Precompilación en contexto puede acelerar la primera solicitud realizada a las páginas ASP.NET recién implementadas en el sitio porque reduce el tiempo de ejecución de la necesidad de realizar este paso. Sin embargo, no es útil para la mayoría de los sitios Web hospedados precompilación en contexto porque requiere que se pueda ejecutar programas desde el servidor web de línea de comandos. En entornos de hospedaje compartidos no se permite este nivel de acceso.

> [!NOTE]
> Para obtener más información sobre la precompilación en contexto, visite [How To: Precompile ASP.NET Web Sites](https://msdn.microsoft.com/library/ms227972.aspx) y [precompilación en ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


En lugar de compilar las páginas en el sitio Web a la `Temporary ASP.NET Files` precompilación para la implementación de carpeta, compila las páginas en un directorio de su elección y en un formato que se puede implementar en el entorno de producción.

Hay dos tipos de precompilación para la implementación que se exploran en este tutorial: precompilación con una interfaz de usuario actualizable y precompilación con una interfaz de usuario no actualizable. Precompilación con una interfaz de usuario actualizable deja el marcado declarativo en la `.aspx`, `.ascx`, y `.master` archivos, lo que permite a un desarrollador ver y, si lo desea, modifique el marcado declarativo en el servidor de producción. Genera la precompilación con una interfaz de usuario no actualizable `.aspx` páginas que sean de tipo void de cualquier tipo de contenido y quita `.ascx` y `.master` archivos, con lo que se oculte el marcado declarativo y prohibición de un desarrollador pueda cambiar desde el entorno de producción.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Precompilar para la implementación con una interfaz de usuario actualizable

Es la mejor manera de comprender la precompilación para la implementación ver un ejemplo en acción. Vamos a precompilar el WSP de revisiones de libro para la implementación mediante una interfaz de usuario actualizable. La herramienta de compilación de ASP.NET se puede invocar desde el menú de compilación de Visual Studio o desde la línea de comandos. Esta sección examina mediante la herramienta desde dentro de Visual Studio; la sección "precompilar from the Command Line" examina ejecutando la herramienta de compilación desde la línea de comandos.

Abra el WSP de revisión de libro en Visual Studio, vaya al menú de compilación y seleccione la opción de menú Publicar sitio Web. Esto inicia el cuadro de diálogo Publicar sitio Web (vea **figura 1**), donde puede especificar la ubicación de destino, si la interfaz de usuario del sitio precompilado es actualizable y otras opciones de herramienta del compilador. La ubicación de destino puede ser un servidor web remoto o el servidor FTP, pero por ahora, elija una carpeta en la unidad de disco duro del equipo. Puesto que deseamos precompilar el sitio con una interfaz de usuario actualizable, deje la casilla "Permitir que este sitio precompilado se actualizable" activada y haga clic en Aceptar.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Figura 1**: la herramienta de compilación de ASP.NET precompila el sitio Web a la ubicación de destino especificado  
 ([Haga clic aquí para ver la imagen a tamaño completo](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> La opción de publicar el sitio Web en el menú Generar no está disponible en Visual Web Developer. Si utiliza Visual Web Developer debe usar la versión de línea de comandos de la herramienta de compilación de ASP.NET, que se explica en la sección "precompilar from the Command Line".


Después de precompilar el sitio Web, navegue hasta la ubicación de destino especificada en el cuadro de diálogo Publicar sitio Web. Tómese un momento para comparar el contenido de esta carpeta con el contenido del sitio Web. **Figura 2** muestra la carpeta del sitio Web de reseñas de libros. Observe que contiene ambos `.aspx` y `.aspx.cs` archivos. Además, tenga en cuenta que la `Bin` directorio incluye un único archivo, `Elmah.dll`, que se agregó en el [tutorial anterior](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Figura 2**: el directorio del proyecto contiene `.aspx` y `.aspx.cs` archivos; el `Bin` carpeta incluye solo `Elmah.dll`  
 ([Haga clic aquí para ver la imagen a tamaño completo](precompiling-your-website-vb/_static/image6.png))

**Figura 3** muestra la carpeta de ubicación de destino cuyo contenido se crearon mediante la herramienta de compilación de ASP.NET. Esta carpeta no contiene ningún archivo de código subyacente. Además, esta carpeta `Bin` directorio incluye varios ensamblados y dos `.compiled` archivos además la `Elmah.dll` ensamblado.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Figura 3**: la carpeta de ubicación de destino incluye los archivos para la implementación  
 ([Haga clic aquí para ver la imagen a tamaño completo](precompiling-your-website-vb/_static/image9.png))

A diferencia de la compilación explícita en puntos de acceso inalámbrico, la precompilación para el proceso de implementación no crea un ensamblado para todo el sitio. En su lugar, procesa por lotes juntos varias páginas en cada ensamblado. También se compilan los `Global.asax` archivo (si existe) en su propio ensamblado, así como todas las clases en el `App_Code` carpeta. Los archivos que contienen el marcado declarativo para ASP.NET web páginas, controles de usuario y páginas maestras (`.aspx`, `.ascx`, y `.master` archivos, respectivamente) se copian como-está en el directorio de la ubicación de destino. Del mismo modo, el `Web.config` archivo se copió directamente, junto con los archivos estáticos, como imágenes, clases CSS y archivos PDF. Para obtener una descripción más formal de cómo la herramienta de compilación controla varios tipos de archivo, consulte [de control de archivos durante la precompilación ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Puede indicar a la herramienta de compilación para crear un ensamblado por cada página ASP.NET, Control de usuario o página maestra activando la casilla de verificación "Utilizado fijo de nombres y ensamblados de página única" en el cuadro de diálogo Publicar sitio Web. Tener cada página ASP.NET que se compila en su propio ensamblado permite un control más minucioso sobre la implementación. Por ejemplo, si actualiza una sola página web ASP.NET y necesarios para implementar ese cambio, necesita implementar solo esa página `.aspx` ensamblado asociado al entorno de producción y archivo. Consulte [How To: generar nombres fijos con la herramienta de compilación de ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) para obtener más información.


El directorio de la ubicación de destino también contiene un archivo que no formaba parte del proyecto web precompilado, de a saber `PrecompiledApp.config`. Este archivo indica el tiempo de ejecución ASP.NET que la aplicación se precompiló y si se precompiló con una interfaz de usuario actualizable o mediodía actualizables.

Por último, tómese un momento para abrir uno de los `.aspx` archivos en la ubicación de destino con Visual Studio o el editor de texto preferido. Al precompilar para la implementación con una interfaz de usuario actualizable, las páginas ASP.NET en el directorio de la ubicación de destino contienen el marcado mismo exactamente como los archivos correspondientes en el sitio Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Precompilar para la implementación con una interfaz de usuario no actualizable

También se puede utilizar la herramienta de compilación ASP.NET para precompilar un sitio para la implementación con una interfaz de usuario no actualizable. Precompilar el sitio con una interfaz de usuario no actualizable funciona de manera similar precompilar con una interfaz de usuario actualizable, la principal diferencia radica en que las páginas ASP.NET, controles de usuario y las páginas maestras en el directorio de destino se quitan de su marcado. Para precompilar un sitio Web para la implementación con una interfaz de usuario no es actualizable, elija la opción de publicar el sitio Web en el menú Generar, pero desactive la opción "Permitir que este sitio precompilado se actualizable" (vea **figura 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Figura 4**: desactive la "Permitir que este sitio precompilado se actualizable" opción para precompilar con unos no actualizable interfaz de usuario  
 ([Haga clic aquí para ver la imagen a tamaño completo](precompiling-your-website-vb/_static/image12.png))

**Figura 5** muestra la carpeta de ubicación de destino después de precompilar con una interfaz de usuario no actualizable.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Figura 5**: la carpeta de ubicación de destino para la implementación con una interfaz de usuario no actualizable  
 ([Haga clic aquí para ver la imagen a tamaño completo](precompiling-your-website-vb/_static/image15.png))

Comparar **figura 3** a **figura 5**. Aunque las dos carpetas pueden tener la misma apariencia, tenga en cuenta que la carpeta de la interfaz de usuario no actualizable carece de la página maestra, `Site.master`. Y mientras **figura 5** incluye las distintas páginas ASP.NET, si ve el contenido de estos archivos, verá que ha ha su marcado declarativo que se han quitado y reemplazado por el texto de marcador de posición: "Este es un archivo de marcador generado por la herramienta de precompilación y no deben eliminarse! "

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Figura 5**: se ha quitado el marcado declarativo de las páginas ASP.NET

El `Bin` carpetas de **las figuras 3** y **5** diferir considerablemente más. Además de los ensamblados, el `Bin` carpeta **figura 5** incluye un `.compiled` archivo para cada página ASP.NET, el Control de usuario y la página maestra.

Precompilar un sitio con una interfaz de usuario no actualizable es útil en situaciones donde no desea que el contenido de las páginas ASP.NET que va a modificar la persona o empresa que instala o administra el sitio Web en el entorno de producción. Si compila una aplicación web ASP.NET que vende a los clientes lo instalen en sus propios servidores web, puede que desee asegurarse de que no modifican la apariencia y funcionamiento de su sitio editando directamente el `.aspx` páginas que enviarlos. Al precompilar el sitio Web con una interfaz de usuario no es actualizable, distribuir el marcador de posición `.aspx` páginas como parte de la instalación, lo que evita que los clientes de examinar o modificar su contenido.

### <a name="precompiling-from-the-command-line"></a>Precompilar desde la línea de comandos

En segundo plano, cuadro de diálogo Publicar sitio Web de Visual Studio, invoca la herramienta de compilación de ASP.NET (`aspnet_compiler.exe`) para precompilar el sitio Web. Como alternativa, puede invocar esta herramienta desde la línea de comandos. De hecho, si utiliza Visual Web Developer, a continuación, debe ejecutar la herramienta de compilador desde la línea de comandos, como menú de compilación de Visual Web Developer no incluir la opción de publicar el sitio Web.

Para usar la herramienta de compilador desde la línea de comandos, inicie quitando a la línea de comandos y desplácese hasta el directorio de framework, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. A continuación, escriba la siguiente instrucción en la línea de comandos:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

El comando anterior inicia la herramienta de compilador ASP.NET (`aspnet_compiler.exe`) y, a través de la `-p` modificador, indica a precompilar el sitio Web con raíz en *físico\_ruta de acceso\_a\_aplicación*; Este valor será similar a `C:\MySites\BookReviews`y debe estar delimitado por comillas.

La `-v` conmutador especifica el directorio virtual del sitio. Si el sitio está registrado como el sitio Web predeterminado en la metabase de IIS, puede omitir el `-p` cambia y simplemente especificar el directorio virtual de la aplicación. Si usas el `-p` cambia, el procedimiento de valor la `-v` conmutador indica la raíz del sitio Web y se utiliza para resolver las referencias a raíz de la aplicación. Por ejemplo, si especifica un valor de `-v /MySite` , a continuación, se hace referencia en la aplicación para `~/path/file` se resolverá como `~/MySite/path/file`. Dado que el sitio de reseñas de libros se encuentra en el directorio raíz en mi empresa de hospedaje web he usado el conmutador `-v /`.

El `-f` conmutador, si está presente, indica a la herramienta de compilación para sobrescribir el *destino\_ubicación\_carpeta* directorio si ya existe. Si se omite la `-f` conmutador y la carpeta de ubicación de destino ya existe, la herramienta de compilación se cerrará con el error: "error ASPRUNTIME: el directorio de destino no está vacío. Por favor, elimínelo manualmente o elija un destino diferente."

La `-u` conmutador, si está presente, lo notifica a la herramienta para crear una interfaz de usuario actualizable. Omita este modificador para precompilar el sitio con una interfaz de usuario no actualizable.

Por último, el *destino\_ubicación\_carpeta* es la ruta de acceso física al directorio de la ubicación de destino; este valor será similar a `C:\MySites\Output\BookReviews`y debe estar delimitado por comillas.

## <a name="deploying-the-precompiled-website"></a>Implementar el sitio Web precompilado

En este punto hemos visto cómo utilizar la herramienta de compilación de ASP.NET para precompilar un sitio Web mediante las opciones de interfaz de usuario actualizables y no son actualizables. Sin embargo, nuestros ejemplos hasta ahora han precompilado el sitio Web en una carpeta local y no en el entorno de producción. Las buenas noticias son que implementar el sitio Web precompilado es muy sencilla y se puede realizar a través de Visual Studio o algún otro mecanismo de copia archivos, como desde un cliente FTP independiente.

El cuadro de diálogo Publicar sitio Web (primero se muestra en **figura 1**) tiene una opción de ubicación de destino, lo que indica que se copian los archivos del sitio Web precompilado en. Esta ubicación puede ser un servidor web remoto o el servidor FTP. Especificar un servidor remoto en este cuadro de texto se precompila e implementa el sitio Web en el servidor especificado en un solo paso. Como alternativa, puede precompilar el sitio Web en una carpeta local y, a continuación, copiar manualmente el contenido de esa carpeta en el entorno de producción a través de FTP u otro enfoque.

Con el sitio Web precompilado se implementan automáticamente a través del cuadro de diálogo Publicar sitio Web de Visual Studio es útil para los sitios simples que no existen diferencias de configuración entre los entornos de desarrollo y producción. Sin embargo, como se indicó en el [ *comunes diferencias de configuración entre desarrollo y producción* tutorial](common-configuration-differences-between-development-and-production-vb.md) no es raro que tales diferencias que existen. Por ejemplo, la aplicación web de reseñas de libros utiliza otra base de datos en el entorno de producción que en el entorno de desarrollo. Cuando Visual Studio publica el sitio Web en un servidor remoto a ciegas copia la información del archivo de configuración en el entorno de desarrollo.

Para los sitios con diferencias de configuración entre los entornos de desarrollo y producción puede ser preferible precompilar el sitio en un directorio local, copie los archivos de configuración específicos de la producción y, a continuación, copie el contenido de la salida precompilado producción.

Para repasar acerca de cómo copiar archivos desde el entorno de desarrollo al entorno de producción, consulte la [ *implementar su sitio Web mediante un cliente FTP* ](deploying-your-site-using-an-ftp-client-vb.md) y [  *Implementación de su sitio Web en Visual Studio* ](determining-what-files-need-to-be-deployed-vb.md) tutoriales.

## <a name="summary"></a>Resumen

ASP.NET admite dos modos de compilación: automática y explícita. Como se describe en los tutoriales anteriores, los proyectos de aplicación Web (WAP) Utilice la compilación explícita mientras que los proyectos de sitios Web (WSPs) usan una compilación automática, de forma predeterminada. Sin embargo, es posible compilar explícitamente un WSP antes de la implementación mediante la herramienta de compilación de ASP.NET.

Este tutorial se centra en de precompilación la herramienta de compilación para compatibilidad con la implementación. Al precompilar para la implementación, la herramienta de compilación crea una carpeta de ubicación de destino, compila código fuente de la aplicación web especificado y copia estas compila los ensamblados y los archivos de contenido en la carpeta de ubicación de destino. La herramienta de compilación puede configurarse para crear una interfaz de usuario actualizable o no actualizable. Al precompilar con una opción de interfaz de usuario no es actualizable, se quita el marcado declarativo en los archivos de contenido. En pocas palabras, precompilación permite implementar una aplicación basada en el proyecto de sitio Web sin incluir los archivos de código fuente y con el marcado declarativo quitado, si lo desea.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Precompilación de sitio Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Código subyacente y la compilación en ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Precompilación en ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opciones de sitio precompilado en ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-elmah-vb.md)
> [Siguiente](users-and-roles-on-the-production-website-vb.md)
