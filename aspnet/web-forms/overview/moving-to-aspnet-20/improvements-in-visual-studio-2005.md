---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Mejoras en Visual Studio 2005 | Microsoft Docs
author: microsoft
description: Visual Studio 2005 proporciona a los desarrolladores de aplicaciones Web con una larga lista de mejoras para proyectos Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 35ebb6e94c3da105b802c843e36f193f81a45782
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397644"
---
<a name="improvements-in-visual-studio-2005"></a>Mejoras en Visual Studio 2005
====================
por [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 proporciona a los desarrolladores de aplicaciones Web con una larga lista de mejoras para proyectos Web.


Visual Studio 2005 proporciona a los desarrolladores de aplicaciones Web con una larga lista de mejoras para proyectos Web. Tan eficaces como Visual Studio .NET 2002 y 2003, había muchas quejas de la manera en que se controlaban los proyectos Web. Visual Studio 2005, se agrega un número significativo de las nuevas características para resolver estas quejas. Para aquellos que prefieren la forma en que Visual Studio .NET 2003 controlan la compilación de aplicaciones Web, vea [proyectos de aplicación Web](https://go.microsoft.com/fwlink/?LinkId=57870).

En este módulo, acá abarcaremos mejoras en el desarrollo, administración y creación de proyectos Web. En un módulo posterior, acá abarcaremos mejoras en la creación de proyectos Web e implementarlos.

## <a name="frontpage-server-extensions"></a>Extensiones de servidor de FrontPage

Visual Studio .NET 2002 y 2003 requiere extensiones de servidor en el cuadro con el fin de crear o crear proyectos Web. Los desarrolladores tenía una elección entre los dos modos de acceso diferentes (modo de acceso de archivo o extensiones de servidor), ambos utilizan FrontPage Server Extensions para realizar tareas como la configuración de la raíz de la aplicación en IIS, etcetera.

Visual Studio 2005 elimina la dependencia en FrontPage Server Extensions para proyectos locales. Visual Studio 2005 ahora tiene acceso a la metabase de IIS directamente en lugar de usar las extensiones de servidor de FrontPage. Visual Studio 2005 también agrega compatibilidad con FTP que permite el acceso remoto del proyecto sin necesidad de extensiones de servidor.

Para aquellos desarrolladores que desean usar FrontPage Server Extensions en sus proyectos, la opción está disponible todavía. Sin embargo, en función de información eficaz de la Comunidad de desarrolladores ASP.NET, no es un requisito.

> [!NOTE]
> Extensiones de servidor siguen siendo necesarias para la creación del proyecto remoto, apertura, etcetera.


## <a name="aspnet-development-server"></a>servidor de desarrollo de ASP.NET

Visual Studio 2005 se distribuye con un nuevo servidor Web llamado servidor de desarrollo de ASP.NET. (Este servidor Web se conocía anteriormente como Cassini).

Hay varias ventajas del servidor de desarrollo de ASP.NET.

- Ahora es posible que no sean administradores desarrollar y depurar en un servidor Web.
- El servidor de desarrollo de ASP.NET asigna dinámicamente los directorios virtuales a cualquier ubicación en el sistema de archivos que se permite para las ubicaciones de proyectos flexible.
- Los usuarios en Windows XP Professional ya está usando IIS ahora podrán crear nuevas aplicaciones Web que no afectan a la estructura de archivo o carpeta de su sitio Web predeterminado en IIS.

Es necesaria ninguna configuración especial para aprovechar el servidor de desarrollo de ASP.NET. Cuando se depura un proyecto Web que se hospeda en el sistema de archivos o se puede examinar, Visual Studio 2005 se iniciará automáticamente una instancia del servidor de desarrollo de ASP.NET en un puerto aleatorio para atender la solicitud.

En el servidor de desarrollo de ASP.NET más adelante en este módulo se tratarán más información.

## <a name="improved-file-management"></a>Administración de archivos mejoradas

En Visual Studio 2002 y 2003, un archivo de proyecto (.vbproj para VB.NET) y .csproj para C# almacena información en todos los archivos de la aplicación Web. La presentación del explorador de soluciones se basa en la información del archivo en el archivo de proyecto. Por este motivo, el Explorador de soluciones a menudo mostraría una información inexacta en casos donde se usaron los editores externos. Visual Studio 2002 y 2003 podría sobrescribir a menudo cambios en el archivo o no mostrar la versión más reciente de los archivos.

Visual Studio 2005 no inmediatamente con el archivo de proyecto. En su lugar, lee la información de archivos y carpetas directamente desde el disco, lo que resulta en una presentación precisa de los archivos en el proyecto. Dado que la carpeta de referencias en Visual Studio 2002 y 2003 no representa una carpeta real en la aplicación Web, Visual Studio 2005 también quita la carpeta referencias del explorador de soluciones. Para obtener acceso a las referencias del proyecto en Visual Studio 2005, debe usar las páginas de propiedades del proyecto.

## <a name="creating-web-projects"></a>Creación de proyectos Web

Los desarrolladores Web tienen muchas opciones nuevas disponibles para la creación del proyecto en Visual Studio 2005. Sitios Web ahora se pueden crear en cualquier lugar en el sistema de archivos y, a continuación, se pueden depurar o puede examinar usando el nuevo servidor de desarrollo de ASP.NET. Los desarrolladores también pueden crear nuevos sitios Web mediante FTP.

Haga clic aquí para ver un tutorial en vídeo de la creación de proyectos Web en Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Proyectos de sistema de archivos

Como se vio en el tutorial en vídeo, puede elegir crear sitios Web en el sistema de archivos en el equipo local o en una ubicación remota a través de un recurso compartido de archivos. Sitios Web que se crean en el sistema de archivos se puede examinar y depurar mediante el servidor de desarrollo de ASP.NET.

> [!NOTE]
> El servidor de desarrollo de ASP.NET puede provocar confusión para los clientes. Si se crea un proyecto Web en el sistema de archivos en la estructura de directorios IISs (es decir, c:/inetpub/wwwroot), el sitio Web siguen explorarán a través del servidor de desarrollo de ASP.NET cuando se inicia desde dentro de Visual Studio 2005. Por lo tanto, cualquier configuración de IIS (es decir, los métodos de autenticación) no es aplicable.


El proyecto web predeterminado también se quita mucho de la sobrecarga por sólo incluye una página Default.aspx, archivo default.cs y una carpeta de aplicación/_Data. El archivo web.config y carpetas especiales (es decir, aplicación/_fragmentos) se agregan según sean necesarios. El proyecto web sólo incluye los archivos y carpetas que necesita.

### <a name="http-projects"></a>Proyectos HTTP

Los proyectos HTTP pueden ser proyectos que se crean en un sitio Web de IIS local o en un sitio Web remoto. La ubicación de proyecto predeterminada es `http://localhost`. Si hace clic en el botón Examinar, hay dos opciones de HTTP: IIS Local y el sitio remoto. La principal diferencia en estas dos opciones es el método en el que se muestra la información del sitio web en el cuadro de diálogo Elegir la ubicación y en cómo se copian los archivos al servidor Web.

La opción de IIS Local lee la información del sitio de la metabase en el equipo local y se copian los archivos mediante el sistema de archivos. La opción de sitio remoto usa las extensiones de servidor y la información del sitio y se copian los archivos mediante HTTP y las llamadas RPC de extensiones de servidor de FrontPage.

> [!NOTE]
> El archivo vs###/_tmp.htm y get/_aspx/_ver.aspx ya no se usan para determinar la información de versión.


La opción de HTTP predeterminada es IIS Local. Esta opción lee la Metabase de IIS para determinar qué sitios están disponibles y la ubicación en la que se va a crear contenido. Puede seleccionar una carpeta diferente o un directorio virtual, selecciónela en la vista de árbol. Se puede también crear un nuevo directorio virtual, marcar las carpetas como aplicaciones, así como eliminar directorios virtuales existentes de este cuadro de diálogo.


![El cuadro de diálogo ubicación elegir](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: el cuadro de diálogo ubicación elegir


A diferencia de en versiones anteriores de Visual Studio, si comprueba el **Use Secure Sockets Layer** checkbox y el certificado SSL no coincide con la dirección URL que se va a examinar, aparecerá un cuadro de diálogo de alerta de seguridad que le preguntará si lo haría al igual que continuar. Con Visual Studio .NET 2003, si el certificado no era una coincidencia, crear el proyecto produciría un error.


![Certificado SSL de sobre alertas de seguridad](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: alerta de seguridad sobre el certificado SSL


### <a name="note-on-host-headers"></a>Nota sobre los encabezados de Host

Si va a crear una aplicación Web en un sitio enlazado a una dirección IP específica, deberá asegurarse de que se ha configurado un encabezado de host. En caso contrario, Visual Studio creará el sitio en `http://localhost`, pero la dirección IP no resolverá correctamente cuando el sitio se puede examinar o depurando desde el IDE.

Si selecciona la opción de sitio remoto, el cuadro de diálogo cambia para que pueda escribir la dirección URL de destino para el nuevo sitio Web. Esta dirección URL debe estar en un servidor que tenga habilitado las extensiones de servidor de FrontPage. Si desea trabajar con el servidor Web local mediante las extensiones de servidor de FrontPage, puede usar la opción de sitio remoto y especificar una dirección URL local.


![Crear un sitio Web en un servidor remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: crear un sitio Web en un servidor remoto


Al crear una aplicación en un sitio remoto a través de SSL, si no coincide con el certificado SSL, el cuadro de diálogo de confirmación es ligeramente diferente que el cuadro de diálogo aparece cuando se usa la opción de IIS Local.


![La alerta de seguridad de sitio remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: la alerta de seguridad de sitio remoto


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 presenta la opción para crear sitios Web a través de FTP. Cuando se usa esta opción, el IDE crea los archivos localmente en la carpeta temporal de los usuarios y, a continuación, utiliza FTP para mover los archivos a la ubicación FTP.

> [!NOTE]
> La ubicación de la carpeta temp es c:/Documents and Settings /&lt;usuario&gt;/Local Temp/configuración/VWDWebCache/&lt;Server&gt;/_&lt;nombre de la aplicación&gt;


Cuando se usa la opción de FTP, aparecerá un cuadro de diálogo Elegir ubicación. Escriba la información de conexión de FTP necesaria en este cuadro de diálogo como se muestra a continuación.


![El cuadro de diálogo ubicación FTP elegir](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: el cuadro de diálogo ubicación FTP elegir


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratorio: Configuración FTP del sitio y cree un proyecto

Los pasos siguientes configuran el sitio FTP para que un usuario tiene una ubicación que solo se puede cargar en a través de FTP.

### <a name="install-the-ftp-service"></a>Instalar el servicio FTP

1. Abra Agregar o quitar programas, seleccione Agregar o quitar componentes de Windows
2. Seleccione servicios de Internet Information Server (servidor de aplicaciones en Windows 2003) y haga clic en **detalles**.
3. Comprobar **servicio de protocolo de transferencia de archivos (FTP)** y haga clic en **Aceptar**.
4. Haga clic en **siguiente** para instalar el servicio FTP.

### <a name="create-a-new-folder-for-content"></a>Cree una nueva carpeta para el contenido

1. En el Explorador de Windows, cree una nueva carpeta denominada **User1** dentro de c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurar los permisos y las carpetas en las carpetas.

1. Abra el complemento de Internet Information Services desde herramientas administrativas. Ahora tendrá una carpeta de sitios FTP en el nodo de nombre de equipo.
2. Expanda **sitios FTP**.
3. Haga clic en el **sitio FTP predeterminado**, seleccione **New**, a continuación, **directorio Virtual**, a continuación, haga clic en **siguiente**.
4. Escriba **User1** para el nombre del directorio virtual y haga clic en **siguiente**.
5. Escriba **c:/inetpub/wwwroot/User1** la ruta de acceso y haga clic en **siguiente**.
6. Haga clic en **siguiente** y, a continuación, **finalizar** para completar el asistente.
7. Haga clic en el **User1** directorio virtual en el sitio FTP predeterminado y seleccione **propiedades**.
8. Compruebe el **escribir** casilla y haga clic en **Aceptar** para cerrar el cuadro de diálogo.
9. Haga clic en **sitio FTP predeterminado** y seleccione **propiedades**.
10. En el **cuentas de seguridad** pestaña, desactive la opción **permitir conexiones anónimas**.
11. Haga clic en **Sí** en el cuadro de diálogo que le pregunta si desea continuar.
12. Haga clic en **Aceptar** para cerrar el cuadro de diálogo.
13. Expanda el **sitio Web predeterminado** bajo el **sitios Web** nodo.
14. Haga clic en el **User1** directorio y seleccione **propiedades**
15. En el **configuración de la aplicación** sección, haga clic en **crear** para marcar la carpeta que la aplicación.
16. Haga clic en **Aceptar** para cerrar el cuadro de diálogo.
17. Cierre el complemento de Internet Information Services.

### <a name="create-web-project"></a>Crear el proyecto web

1. Abra Visual Studio 2005.
2. Desde el **archivo** menú, seleccione **nuevo sitio Web**.
3. En el **ubicación** lista desplegable, seleccione **FTP**.
4. Haga clic en **examinar**.
5. Escriba **localhost** en el **Server** cuadro de texto.
6. Escriba **User1** en el cuadro de texto del directorio.
7. Haga clic en **abierto**. La ubicación FTP se escribirá en el cuadro de diálogo nuevo sitio Web.
8. Haga clic en **Aceptar**.
9. Desactive la opción **sesión anónimo** en el cuadro de diálogo Iniciar sesión de FTP, escriba sus credenciales y haga clic en **Aceptar**.
10. ¿Qué es la dirección URL del proyecto? (La dirección URL para el proyecto se mostrará en el Explorador de soluciones).
11. Desde el **compilar** menú, seleccione **Generar sitio Web** o **compilar solución**.
12. Haga doble clic en Default.aspx en el Explorador de soluciones y seleccione **ver en el explorador**.
13. En el cuadro de diálogo sitio Web requiere la dirección URL, escriba `http://localhost/user1` para la dirección URL y haga clic en **Aceptar**.

> [!NOTE]
> Si se produce un error que indica una imposibilidad de cargar el tipo /_Default, asegúrese de que se está ejecutando ASP.NET 2.0 en su sitio Web y no una versión anterior. Puede hacerlo desde la ficha ASP.NET en Internet Information Services.


## <a name="opening-web-projects"></a>Abrir proyectos Web

Abrir proyectos Web es similar a la creación de proyectos. Las secciones siguientes destacar áreas vigilar out para mientras trabaja en el IDE. También se describe cómo trabajar con proyectos Web mediante HTTP y FTP.

Para abrir un proyecto Web, seleccione Abrir sitio Web en el menú archivo. Se le pedirá el mismo cuadro de diálogo Elegir ubicación cubierto anteriormente y tiene las mismas cuatro opciones disponibles: sitio remoto, Local de IIS, FTP y sistema de archivos.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Sistema de archivos

Como se indicó anteriormente en este módulo, Visual Studio ya no utiliza un archivo de proyecto. Por lo tanto, si decide abrir un sitio Web del sistema de archivos, en realidad tiene la posibilidad de elegir cualquier carpeta que desee, incluso si no se creó la carpeta que elija como un proyecto Web inicialmente en Visual Studio. Por ejemplo, puede elegir abrir la carpeta Mis documentos como un sitio Web y Visual Studio alegremente abrirlo y mostrar los archivos tal como se muestra a continuación.


![Mis documentos abiertos como un sitio Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *Mis documentos* abre como un sitio Web


Dado que Visual Studio solo crea otros archivos y carpetas cuando sea necesario, no hay archivos o carpetas adicionales se agregan a la ubicación que abra. Un efecto secundario de esta arquitectura es que impide que los sitios Web de anidamiento en el sistema de archivos. Por ejemplo, considere la siguiente estructura de directorios.

Proyecto Web en C:/MiSitioWeb

Otro proyecto web en C:/MiSitioWeb/anidados

Al abrir el sitio Web en c:/MyWebSite, la carpeta Nested aparecerá como una subcarpeta de la aplicación.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Al abrir los sitios Web a través de HTTP, la configuración se lee desde la metabase de IIS (IIS Local) o mediante FrontPage Server Extensions (sitio remoto). Si no hay aplicaciones web anidados, estos se muestran también con un icono que los identifica como una aplicación. Si está familiarizado con trabajar con aplicaciones web de FrontPage, el comportamiento en Visual Studio 2005 es similar.

Aunque Visual Studio mostrará un icono para las aplicaciones que se anidan debajo de la aplicación que está actualmente abierta en el IDE, no le permitirá expandirlas para ver su contenido. Sin embargo, puede, haga doble clic en ellos para abrirlos. Al hacerlo, se le presentará un cuadro de diálogo que le solicitará que abrir la aplicación web (y reemplazar la solución actualmente abierta) o agregar la aplicación Web a la solución actual.


![Haga doble clic en un icono de aplicación anidada presenta este cuadro de diálogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: haga doble clic en un icono de aplicación anidada presenta este cuadro de diálogo


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Sitio FTP

Al abrir un sitio a través de FTP, los archivos se todos copian localmente en la carpeta temporal. La ruta de acceso completa para la ubicación de almacenamiento local se muestra en el panel de propiedades para el proyecto y se crea con el formato siguiente.

C:/Documents and Settings /&lt;usuario&gt;/Local Temp/configuración/VWDWebCache/&lt;Server&gt;/_&lt;nombre de la aplicación&gt;

Cuando se utiliza FTP, Visual Studio deberá especificar la dirección URL base para el proyecto, por lo que puede examinar como se muestra a continuación. Si no especifica una dirección URL base, Visual Studio le pedirá que para la primera vez que intente examinar una página en el sitio Web.


![Especificar una dirección URL Base para los sitios FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: especificar una dirección URL Base para los sitios FTP


## <a name="improvements-in-compilation"></a>Mejoras en la compilación

Trabajar con aplicaciones Web en Visual Studio 2005 es considerablemente más rápido que en versiones anteriores. Esto es debido en ninguna parte pequeña a los cambios en la arquitectura de la compilación.

En Visual Studio 2002 y 2003, las aplicaciones Web se compilan en un ensamblado principal que reside en la carpeta/bin. En Visual Studio 2005, se agregó una carpeta de aplicación/_fragmentos. Las clases y otro código que no son de interfaz de usuario se agregan a la carpeta de aplicación/_fragmentos. Cuando Visual Studio compila el proyecto, todos los archivos en la carpeta de aplicación/_fragmentos se compilan en un único archivo App/_Code.dll. El resultado de este cambio es que las compilaciones posteriores son mucho más rápidas que en versiones anteriores.

> [!NOTE]
> También se puede usar la utilidad de línea de comandos de MSBuild para compilar aplicaciones de ASP.NET Web. Esa herramienta se explicará en el módulo 9.


Otra mejora de la compilación es la nueva opción de compilación de página en el menú Generar. Esta característica permite que un desarrollador recompilar sólo la página actual (junto con, del curso y las dependencias) para que se pueden compilar los cambios más rápidamente. Dado que C# no ofrece la compilación en segundo plano para fines de actualización de IntelliSense, etc., beneficiará enormemente de esta característica porque eso permitirá para que IntelliSense se actualizan rápidamente simplemente volver a generar una sola página.

Las propiedades de un proyecto de compilación permiten configurar el tipo de compilación que se produce antes de la página de inicio. Los desarrolladores pueden elegir solo compilar la página actual para que Visual Studio puede iniciar la depuración de aplicaciones más rápidamente después de realizar cambios de código.


![La acción de inicio de la página de compilación](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: la acción de inicio de la página de compilación


Otra gran mejora a Visual Studio y la arquitectura de ASP.NET se encuentra en el área de editar y continuar. En Visual Studio 2005, los desarrolladores pueden empezar a depurar un proyecto y realizar cambios de código en el proyecto sin desconectar al depurador. De hecho, literalmente se puede iniciar la depuración de un proyecto, agregue una nueva clase, agregue código para esa clase, agregue código a la página que se crea una nueva instancia de esa clase y ejecutar un método de la clase, todo ello sin separar al depurador. Ejecutar el nuevo código es literalmente tan sencillo como actualizar el explorador.

Haga clic aquí para ver un tutorial en vídeo de la edición y continuar con la característica en Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


La sólida de editar y continuar la funcionalidad en ASP.NET 2.0 y Visual Studio 2005 es debido a un cambio de arquitectura para aplicaciones ASP.NET. En ASP.NET 1.x, las aplicaciones creadas en Visual Studio 2002/2003 se compilan en un ensamblado principal que se almacenó en la carpeta/bin. Todas las clases, las páginas, etc. para la aplicación se han compilado en que un archivo DLL. A continuación, en tiempo de ejecución, ASP.NET podría compilar todos los controles, marcado y código ASP.NET dentro de las páginas y copiar esos archivos DLL en la carpeta temporal de ASP.NET.

Se han combinado en Visual Studio 2005 mediante ASP.NET 2.0, los modelos de dos de compilación de esquema anteriormente (uno para Visual Studio) y otro para ASP.NET en tiempo de ejecución en un modelo común de compilación. Esto significa que todos los problemas de compilación ahora se detectan durante la fase de desarrollo en lugar de en tiempo de ejecución. También se permite para la compatibilidad con IntelliSense para funciones como controles de usuario y las páginas maestras y de diseñador.

Haga clic aquí para ver un tutorial en vídeo de compatibilidad con el diseñador para controles de usuario.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Cuando se quita un control de usuario de una página, el @Register directiva permanece en el marcado y debe quitarse manualmente con el fin de evitar errores de análisis si se elimina el control de usuario desde el sitio Web.


Otra mejora en el modelo de compilación de Visual Studio es la característica de publicación de sitios Web. Dado que la característica de publicación precompila el sitio Web, los desarrolladores pueden disfrutar el rendimiento adicional de no tener que compilar algo a petición. También precompila todo el código fuente en la carpeta de aplicación/_fragmentos en un archivo DLL para que no hay código fuente debe implementarse.


![El cuadro de diálogo Publicar sitio Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: cuadro de diálogo Publicar sitio Web


> [!NOTE]
> También se puede usar la utilidad aspnet/_compile.exe para precompilar una aplicación Web ASP.NET. Esa herramienta se explicará en el módulo 9.


Al publicar un sitio Web, los archivos precompilados se almacena en la carpeta archivos temporales de ASP.NET como se muestra a continuación. Los archivos con un *.compiled* extensión de archivo son archivos XML que definen las dependencias para las DLL determinadas. Los controles de formularios Web Forms o un usuario se compilan en DLL aleatorias que comienzan por *App /_Web /_*.

Si deja el *permitir que este sitio precompilado se actualice* está activada la casilla de verificación, marcado dentro de los controles de formularios Web Forms y el usuario no será precompilada en un archivo DLL que lo que permite realizar cambios después de la implementación. Si prefiere bloquean el marcado para que no se permiten cambios en el contenido distribuido, desactive esta casilla.

El *uso se ha corregido la nomenclatura y solo ensamblados de página* casilla de verificación le permite deshabilitar la compilación por lotes para que cada página se compila en un ensamblado con nombre se ha corregido. Deja desactivada esta casilla permite aprovechar las ventajas de la compilación por lotes.

El *Habilitar nombre seguro en los ensamblados precompilados* casilla permite un nombre seguro los ensamblados precompilados.

> [!NOTE]
> En ASP.NET 1.x, los ensamblados con nombre seguro debían instalarse en la caché de ensamblados Global (GAC). En ASP.NET 2.0, no es necesario para instalar a ensamblados con nombre seguro en la GAC.


![Un archivos compilados previamente de aplicaciones de ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: un archivos compilados previamente de aplicaciones de ASP.NET


> [!NOTE]
> En la aplicación anterior, no había ningún archivo web.config. Si se hubiese producido, habría llamó *PrecompiledApp.config* proceso del sitio después de la publicación Web.


## <a name="improvements-in-deployment"></a>Mejoras en la implementación

Como con Visual Studio 2002 y 2003, Visual Studio 2005 ofrece una característica de copiar proyecto. Sin embargo, la característica se ha reforzado en Visual Studio 2005 y ahora se denomina Copiar sitio Web.

El cuadro de diálogo Copiar sitio Web se divide en un marco de la izquierda y un marco de la derecha. El marco izquierdo se denomina el sitio Web de origen y el marco de la derecha se denomina el sitio Web remoto. Una cosa que puede confundir a algunos desarrolladores es que el sitio que se muestra en el marco derecho no es necesariamente un sitio remoto. Podría ser un sitio en el sistema de archivos local o en la instancia local de IIS. Además, el sitio que se muestra en el marco izquierdo no es necesariamente el sitio Web de origen como el cuadro de diálogo le permite publicar desde el sitio Web remoto *a* el sitio Web de origen.

Si va a copiar un proyecto a un sitio Web remoto, ese sitio debe tener instalado las extensiones de servidor de FrontPage. Si no es así, deberá conectarse mediante FTP. Por otro lado, si va a copiar un proyecto a la instancia local de IIS, extensiones de servidor no son necesarias.

> [!NOTE]
> Si intenta crear un nuevo sitio Web en la instancia local de IIS y se instalan las extensiones de servidor de FrontPage 2002, obtendrá un mensaje de error que le indica que la creación de sitios Web no se admite en un servidor de SharePoint. En ese caso, tiene la opción de instalar las extensiones de servidor de FrontPage 2000 o quitar las extensiones de servidor de FrontPage.


Haga clic aquí para ver un vídeo tutorial de la característica Copiar sitio Web.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Mejoras en la depuración

Hay cuatro mejoras claves en la depuración en Visual Studio 2005.

- Depura localmente como un administrador no es posible de inmediato.
- El atributo de depuración para el elemento Compilation ahora es false de forma predeterminada.
- Instalación y configuración de la depuración remota es más fácil que nunca.
- Ahora puede depurar un sitio Web abierto a través de una ubicación FTP.

## <a name="debugging-as-a-non-administrator"></a>Depuración como no administrador

La adición del servidor de desarrollo de ASP.NET permite depurar fácilmente aplicaciones de ASP.NET desde el comienzo que no sean administradores. Cuando se depura una aplicación ASP.NET que se ejecutan en el sistema de archivos local, Visual Studio inicia el servidor de desarrollo de ASP.NET en el contexto del usuario que ha iniciado sesión. Ese usuario, a continuación, puede depurar la aplicación sin ninguna configuración adicional.

## <a name="debug-is-false-by-default"></a>Depuración es False de forma predeterminada

En ASP.NET 1.x, el *depurar* atributo en el *compilación* se estableció el elemento del archivo web.config *true* de forma predeterminada. Se siempre se recomienda que los desarrolladores establecer este atributo en *false* antes de implementar una aplicación en producción, sino porque la mayoría de los desarrolladores no comprende completamente las consecuencias de dejar el atributo de depuración establecido en es true, simplemente deja como-es.

El problema más grave con tener el atributo de depuración establecido en true es que deshabilita el modelo de compilación por lotes ASP.NETs. Por lo tanto, cada página se compila en un archivo DLL independiente. Si un servidor Web aplicación consta de miles de páginas (no oído hablar de modo alguno), que significa que se crearán varios archivos DLL pequeño miles por esa aplicación. Aunque estos archivos DLL son pequeños, no se cargan en cualquier ubicación concreta en la memoria. Por lo tanto, se provocar la fragmentación de memoria del sistema y puede contribuir a las apariciones de OutOfMemoryException.

En ASP.NET 2.0, el atributo de depuración se establece en false de forma predeterminada. Como ya ha visto, cuando depura una aplicación ASP.NET en Visual Studio 2005, un desarrollador, se le pide que agregue un archivo web.config con la depuración habilitada. Si lo hace, incurre en el mismos inconvenientes que estaban presentes en ASP.NET 1.x, pero ahora el desarrollador claramente se advierte que se debe restablecer el atributo en false antes de pasar a la aplicación en producción.

## <a name="remote-debugging-setup-and-configuration"></a>Instalación y configuración de depuración remota

En Visual Studio 2002/2003, la depuración remota se basaba en el Administrador de depuración del equipo (mdm.exe) y el proceso vs7jit.exe. Por este motivo, solución de problemas de depuración remotas a menudo era una caja negra para los clientes y a menudo no era mucho mejor para PSS.

Visual Studio 2005 elimina la dependencia en los procesos de mdm.exe y vs7jit.exe. En su lugar, ahora usa el servicio de Monitor de depuración remota (msvsmon.exe).

El requisito para la depuración remota en Visual Studio 2005 es bastante sencillo. Deberá ejecutar msvsmon.exe en el servidor remoto antes de depurar. Puede instalar al Monitor de depuración remota desde el CD de Visual Studio o simplemente ejecute msvsmon.exe desde un recurso compartido sin instalar nada en absoluto en el servidor Web.

Cuando se ejecuta msvsmon.exe, es probable que se quejan de puertos que se va a bloquear para la depuración remota. Afortunadamente, puede desbloquear fácilmente los puertos directamente desde el cuadro de diálogo de advertencia tal y como se muestra a continuación.


![Notificación de que el Firewall de Windows está bloqueando la depuración remota](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: notificación de que el Firewall de Windows está bloqueando la depuración remota


Una vez que se han desbloqueado los puertos necesarios para la depuración, verá al Monitor de depuración remota tal como se muestra a continuación. Desde esta interfaz, puede supervisar las conexiones y cambiar fácilmente de los permisos de depuración.


![El Monitor de depuración remota](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: el Monitor de depuración remota


También es posible depurar una aplicación Web que se abrió a través de FTP de forma remota. Los pasos son los mismos que los cubierto anteriormente. Sin embargo, deberá especificar una dirección URL base para examinar el proyecto FTP, tal como se describe anteriormente en este módulo.

## <a name="lab-2"></a>Práctica 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Depuración remota con Visual Studio 2005

Esta práctica le guiará a través de la depuración remota con Visual Studio 2005.

Haga clic aquí para ver un tutorial en vídeo de este laboratorio.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Esta práctica requiere que tenga dos equipos, una ejecución Visual Studio 2005 y la otra ejecuta IIS 5 o superior.

1. Abra Visual Studio 2005 y cree un nuevo sitio Web en el servidor remoto.

> [!NOTE]
> Puede crear el sitio Web en una instancia remota de IIS o a través de FTP.


1. Desde el servidor Web remoto, busque msvsmon.exe en el equipo de desarrollo con una ruta de acceso UNC y ejecútelo.  
 La ubicación predeterminada de msvsmon.exe es //server/c$/Program archivos/Microsoft Visual Studio 8/Common7/IDE/remoto depurador/x86.
2. Si se le solicite desbloquear puertos para la depuración remota, hágalo.
3. Desde el equipo de desarrollo, abra el código subyacente para Default.aspx y establezca un punto de interrupción en el método de página/_cargar.
4. Iniciar la depuración desde el equipo de desarrollo.

Debe alcanzar el punto de interrupción según lo previsto.

## <a name="aspnet-development-server"></a>servidor de desarrollo de ASP.NET

Como se comentó que hayamos, Visual Studio 2005 se distribuye con un servidor Web que llama el servidor de desarrollo de ASP.NET. (El servidor de desarrollo de ASP.NET se conoce a veces como Cassini.) Este servidor Web es un medio cómodo para examinar y depurar aplicaciones Web que se ejecutan en el sistema de archivos.

El servidor de desarrollo de ASP.NET es un servidor Web restringido. No permite conexiones remotas, no permite las solicitudes de cualquier usuario que no sea el usuario que inició el servidor Web. No tiene la capacidad de servir páginas ASP. Se sirven solo los recursos ASP.NET y los recursos HTML (incluidas las imágenes, archivos CSS, etc.).

Se puede iniciar el servidor de desarrollo de ASP.NET a través de la línea de comandos ejecutando el archivo WebDev.WebServer.exe ubicado en c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. El siguiente cuadro de diálogo muestra los parámetros que están disponibles.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**


> [!NOTE]
> El servidor de desarrollo de ASP.NET no se admite cuando se inicia explícitamente a través de la línea de comandos.
