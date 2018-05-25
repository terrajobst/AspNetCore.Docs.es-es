---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Mejoras en Visual Studio 2005 | Documentos de Microsoft
author: microsoft
description: Visual Studio 2005 proporciona a los desarrolladores de aplicaciones Web con una larga lista de mejoras para proyectos Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2018
---
<a name="improvements-in-visual-studio-2005"></a>Mejoras en Visual Studio 2005
====================
por [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 proporciona a los desarrolladores de aplicaciones Web con una larga lista de mejoras para proyectos Web.


Visual Studio 2005 proporciona a los desarrolladores de aplicaciones Web con una larga lista de mejoras para proyectos Web. Tan eficaz como Visual Studio .NET 2002 y 2003, hay muchas quejas de la manera en que se administran los proyectos Web. Visual Studio 2005, se agrega un número significativo de nuevas características para tratar estas reclamaciones. Para aquellos que prefieren la manera en que Visual Studio .NET 2003 controlan la compilación de aplicaciones Web, consulte [proyectos de aplicación Web](https://go.microsoft.com/fwlink/?LinkId=57870).

En este módulo, también se tratan mejoras en la creación del proyecto Web, administración y desarrollo. En un módulo posterior, también se tratan mejoras en la creación de proyectos Web y su implementación.

## <a name="frontpage-server-extensions"></a>Extensiones de servidor de FrontPage

Visual Studio .NET 2002 y 2003 requiere extensiones de servidor de FrontPage en el cuadro con el fin de crear o compilar proyectos Web. Los desarrolladores tenía una opción entre dos modos de acceso diferente (modo de acceso de extensiones de servidor de FrontPage o archivo), ambos utilizan las extensiones de servidor de FrontPage para realizar tareas como la configuración de la raíz de la aplicación en IIS, etcetera.

Visual Studio 2005 quita la dependencia de las extensiones de servidor de FrontPage para proyectos locales. Visual Studio 2005 ahora tiene acceso a la metabase de IIS directamente en lugar de usar las extensiones de servidor de FrontPage. Visual Studio 2005 también agrega compatibilidad para FTP que permite el acceso remoto de proyecto sin necesidad de extensiones de servidor de FrontPage.

Para aquellos desarrolladores que desean usar las extensiones de servidor de FrontPage en sus proyectos, la opción sigue estando disponible. Sin embargo, basándose en los comentarios seguro de la Comunidad de desarrolladores ASP.NET, no es un requisito.

> [!NOTE]
> Extensiones de servidor de FrontPage siguen siendo necesarias para crear el proyecto remoto, apertura, etcetera.


## <a name="aspnet-development-server"></a>servidor de desarrollo de ASP.NET

Visual Studio 2005 se suministra con un nuevo servidor Web llamado servidor de desarrollo de ASP.NET. (Este servidor Web se conocía anteriormente como Cassini).

Hay varias ventajas del servidor de desarrollo de ASP.NET.

- Ahora es posible que no sean administradores desarrollar y depurar en un servidor Web.
- El servidor de desarrollo de ASP.NET asigna dinámicamente directorios virtuales a cualquier ubicación en el sistema de archivos que permite que las ubicaciones de proyecto flexible.
- A los usuarios que ya se están utilizando IIS en Windows XP Professional ahora podrá crear nuevas aplicaciones Web que no tendrá ningún efecto en la estructura de archivo o carpeta de su sitio Web predeterminado en IIS.

Es necesaria ninguna configuración especial para aprovechar el servidor de desarrollo de ASP.NET. Cuando se depura un proyecto Web que se hospeda en el sistema de archivos o se desplazó, Visual Studio 2005 se iniciará automáticamente una instancia del servidor de desarrollo de ASP.NET en un puerto aleatorio para atender la solicitud.

En el servidor de desarrollo de ASP.NET más adelante en este módulo se explicará más información.

## <a name="improved-file-management"></a>Administración de archivos mejorado

En Visual Studio 2002 y 2003, un archivo de proyecto (.vbproj para VB.NET) y .csproj para C# almacena información en todos los archivos de la aplicación Web. La visualización del explorador de soluciones se basa en la información del archivo en el archivo de proyecto. Por este motivo, el Explorador de soluciones a menudo mostraría una información inexacta en casos donde se usaron editores externos. Visual Studio 2002 y 2003 podría sobrescribir a menudo los cambios de archivo o no mostrar la versión más reciente de los archivos.

Visual Studio 2005 no inmediatamente con el archivo de proyecto. En su lugar, lee la información de archivos y carpetas directamente desde el disco, lo que da lugar a una pantalla precisa de los archivos del proyecto. Dado que la carpeta de referencias en Visual Studio 2002 y 2003 no representa una carpeta real en la aplicación Web, Visual Studio 2005 también quita la carpeta referencias del explorador de soluciones. Para obtener acceso a las referencias para el proyecto en Visual Studio 2005, debe usar las páginas de propiedades para el proyecto.

## <a name="creating-web-projects"></a>Crear proyectos Web

Los desarrolladores Web cuentan con nuevas opciones disponibles para la creación del proyecto en Visual Studio 2005. Sitios Web ahora pueden crearse en cualquier lugar en el sistema de archivos y, a continuación, se pueden depurar o examinar utilizando el nuevo servidor de desarrollo de ASP.NET. Los desarrolladores también pueden crear nuevos sitios Web mediante FTP.

Haga clic aquí para ver un tutorial en vídeo de creación de proyectos Web en Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Proyectos de sistema de archivos

Como vimos en el tutorial en vídeo, puede crear sitios Web en el sistema de archivos en el equipo local o en una ubicación remota a través de un recurso compartido de archivos. Sitios Web que se crean en el sistema de archivos se examinan y depuran con el servidor de desarrollo de ASP.NET.

> [!NOTE]
> El servidor de desarrollo de ASP.NET puede provocar confusión para los clientes. Si se crea un proyecto Web en el sistema de archivos en la estructura de directorios de IISs (es decir, c:/inetpub/wwwroot), el sitio Web todavía puede examinarse a través del servidor de desarrollo de ASP.NET cuando se inicia desde dentro de Visual Studio 2005. Por lo tanto, cualquier configuración de IIS (es decir, los métodos de autenticación) no es aplicable.


El proyecto web predeterminado, también se quita mucho de la sobrecarga por solo incluye una página Default.aspx, archivo default.cs y una carpeta de aplicación/_Data. El archivo web.config y carpetas especiales (es decir, aplicación/_code) se agregan según sean necesarios. El proyecto web sólo incluye los archivos y carpetas que necesita.

### <a name="http-projects"></a>Proyectos HTTP

Proyectos HTTP pueden ser proyectos creados en un sitio Web de IIS local o en un sitio Web remoto. La ubicación de proyecto predeterminada es `http://localhost`. Si hace clic en el botón Examinar, hay dos opciones de HTTP: IIS Local y el sitio remoto. La diferencia principal en estas dos opciones es el método en el que se muestra la información del sitio web en el cuadro de diálogo Elegir ubicación y en cómo se copian los archivos al servidor Web.

La opción de IIS Local lee la información del sitio de la metabase en el equipo local y se copian los archivos mediante el sistema de archivos. La opción de sitio remoto usa las extensiones de servidor de FrontPage y la información del sitio y los archivos se copian mediante HTTP y llamadas RPC de las extensiones de servidor de FrontPage.

> [!NOTE]
> El archivo de vs###/_tmp.htm y get/_aspx/_ver.aspx ya no se utilizan para determinar la información de versión.


La opción de HTTP de forma predeterminada es IIS Local. Esta opción lee la Metabase de IIS para determinar qué sitios están disponibles y la ubicación en la que se va a crear contenido. Puede seleccionar otra carpeta o directorio virtual, selecciónelo en la vista de árbol. Se puede también crear un nuevo directorio virtual, marcar las carpetas como aplicaciones, así como eliminar los directorios virtuales existentes de este cuadro de diálogo.


![El cuadro de diálogo de ubicación elegir](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: el cuadro de diálogo de ubicación elegir


A diferencia de en versiones anteriores de Visual Studio, si activa la **Use capa de Sockets seguros** casilla de verificación y el certificado SSL no coincide con la dirección URL que se va a examinar, se le mostrará un cuadro de diálogo Alerta de seguridad que se pregunta si quiere ¿desea hacer. Con Visual Studio .NET 2003, si el certificado no es una coincidencia, crear el proyecto produciría un error.


![Certificado SSL de sobre alertas de seguridad](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: alerta de seguridad sobre el certificado SSL


### <a name="note-on-host-headers"></a>Tenga en cuenta en encabezados de Host

Si está creando una aplicación Web en un sitio enlazado a una dirección IP específica, debe asegurarse de que se ha configurado un encabezado de host. De lo contrario, Visual Studio creará el sitio en `http://localhost`, pero la dirección IP no resolverá correctamente cuando el sitio está explorando o depurando desde el IDE.

Si selecciona la opción de sitio remoto, el cuadro de diálogo cambia para permitirle especificar la dirección URL de destino para el nuevo sitio Web. Esta dirección URL debe estar en un servidor que tiene habilitadas las extensiones de servidor de FrontPage. Si desea trabajar con el servidor Web local con las extensiones de servidor de FrontPage, puede utilizar la opción de sitio remoto y especificar una dirección URL local.


![Crear un sitio Web en un servidor remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: crear un sitio Web en un servidor remoto


Al crear una aplicación en un sitio remoto a través de SSL, si no coincide con el certificado SSL, el cuadro de diálogo de confirmación es ligeramente diferente que el cuadro de diálogo aparece cuando se usa la opción de IIS Local.


![La alerta de seguridad de sitio remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: la alerta de seguridad de sitio remoto


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 presenta la opción para crear sitios Web mediante FTP. Cuando se usa esta opción, el IDE crea los archivos de forma local en la carpeta temporal de los usuarios y, a continuación, utiliza FTP para mover los archivos en la ubicación FTP.

> [!NOTE]
> La ubicación de la carpeta temporal es c:/Documents and Settings /&lt;usuario&gt;/Local Temp/configuración/VWDWebCache/&lt;Server&gt;/_&lt;nombre de la aplicación&gt;


Al usar la opción de FTP, aparecerá un cuadro de diálogo Elegir ubicación. Escriba la información de conexión necesaria de FTP en este cuadro de diálogo tal y como se muestra a continuación.


![El cuadro de diálogo de ubicación FTP de elegir](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: el cuadro de diálogo de ubicación FTP de elegir


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratorio: El programa de instalación FTP del sitio y cree un proyecto

Los siguientes pasos configuran el sitio FTP para que un usuario tiene una ubicación que solo se puede cargar en a través de FTP.

### <a name="install-the-ftp-service"></a>Instalar el servicio FTP

1. Abra Agregar o quitar programas, seleccione Agregar o quitar componentes de Windows
2. Seleccione servicios de Internet Information Server (servidor de aplicaciones en Windows 2003) y haga clic en **detalles**.
3. Comprobar **servicio de protocolo de transferencia de archivos (FTP)** y haga clic en **Aceptar**.
4. Haga clic en **siguiente** para instalar el servicio FTP.

### <a name="create-a-new-folder-for-content"></a>Cree una nueva carpeta para el contenido

1. En el Explorador de Windows, cree una nueva carpeta denominada **User1** dentro de c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurar las carpetas y permisos en carpetas.

1. En Herramientas administrativas, abra el complemento Servicios de Internet Information Server. Ahora tendrá una carpeta de sitios FTP en el nodo de nombre de equipo.
2. Expanda **sitios FTP**.
3. Haga clic en el **sitio FTP predeterminado**, seleccione **New**, a continuación, **directorio Virtual**, a continuación, haga clic en **siguiente**.
4. Escriba **User1** para el nombre del directorio virtual y haga clic en **siguiente**.
5. Escriba **c:/inetpub/wwwroot/User1** para la ruta de acceso y haga clic en **siguiente**.
6. Haga clic en **siguiente** y, a continuación, **finalizar** para completar el asistente.
7. Haga clic en el **User1** directorio virtual en el sitio FTP predeterminado y seleccione **propiedades**.
8. Compruebe el **escribir** casilla de verificación y haga clic en **Aceptar** para cerrar el cuadro de diálogo.
9. Haga clic en **sitio FTP predeterminado** y seleccione **propiedades**.
10. En el **cuentas de seguridad** ficha, desactive la opción **permitir conexiones anónimas**.
11. Haga clic en **Sí** en el cuadro de diálogo que le pregunta si desea continuar.
12. Haga clic en **Aceptar** para cerrar el cuadro de diálogo.
13. Expanda el **sitio Web predeterminado** en el **sitios Web** nodo.
14. Haga clic en el **User1** directorio y seleccione **propiedades**
15. En el **configuración de la aplicación** sección, haga clic en **crear** para marcar la carpeta como una aplicación.
16. Haga clic en **Aceptar** para cerrar el cuadro de diálogo.
17. Cierre el complemento Servicios de Internet Information Server.

### <a name="create-web-project"></a>Crear un proyecto web

1. Abra Visual Studio 2005.
2. Desde el **archivo** menú, seleccione **nuevo sitio Web**.
3. En el **ubicación** lista desplegable, seleccione **FTP**.
4. Haga clic en **examinar**.
5. Escriba **localhost** en el **Server** cuadro de texto.
6. Escriba **User1** en el cuadro de texto del directorio.
7. Haga clic en **abiertos**. La ubicación FTP se escribirá en el cuadro de diálogo nuevo sitio Web.
8. Haga clic en **Aceptar**.
9. Desactive la opción **anónimo iniciar sesión** en el cuadro de diálogo Iniciar sesión de FTP, escriba sus credenciales y haga clic en **Aceptar**.
10. ¿Cuál es la dirección URL para el proyecto? (La dirección URL para el proyecto se mostrará en el Explorador de soluciones).
11. Desde el **generar** menú, seleccione **Generar sitio Web** o **generar solución**.
12. Haga doble clic en Default.aspx en el Explorador de soluciones y seleccione **ver en el explorador**.
13. En el cuadro de diálogo sitio Web requiere la dirección URL, escriba `http://localhost/user1` para la dirección URL y haga clic en **Aceptar**.

> [!NOTE]
> Si recibe un error que indica una imposibilidad de cargar el tipo /_Default, asegúrese de que está ejecutando ASP.NET 2.0 en el sitio Web y no una versión anterior. Puede hacerlo desde la ficha ASP.NET en Internet Information Services.


## <a name="opening-web-projects"></a>Abrir proyectos Web

Abrir proyectos Web es similar a la creación de proyectos. En las siguientes secciones informan sobre las áreas para vigilar out para mientras trabaja en el IDE. También explica cómo trabajar con proyectos Web mediante HTTP y FTP.

Para abrir un proyecto Web, seleccione Abrir sitio Web en el menú archivo. Se le pedirá el mismo cuadro de diálogo Elegir ubicación descrito anteriormente y tiene las mismas cuatro opciones disponibles: sitio remoto, IIS Local, FTP y sistema de archivos.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Sistema de archivos

Como se indica anteriormente en este módulo, Visual Studio ya no usa un archivo de proyecto. Por tanto, si desea abrir un sitio Web del sistema de archivos, realmente tendrá la posibilidad de elegir cualquier carpeta que desee, incluso si no se creó la carpeta que elija como un proyecto Web inicialmente en Visual Studio. Por ejemplo, puede elegir abrir la carpeta Mis documentos como un sitio Web y Visual Studio, abrir y mostrar los archivos tal y como se muestra a continuación.


![Mis documentos abiertos como un sitio Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *Mis documentos* abre como un sitio Web


Dado que Visual Studio solo crea otros archivos y carpetas cuando sea necesario, no hay archivos o carpetas adicionales se agregan a la ubicación que se abre. Un efecto secundario de esta arquitectura es que impide que los sitios Web de anidamiento en el sistema de archivos. Por ejemplo, considere la siguiente estructura de directorios.

Proyecto Web en C:/MiSitioWeb

Otro proyecto web en C:/MiSitioWeb/anidadas

Cuando se abre el sitio Web en c:/MiSitioWeb, la carpeta Nested aparecerá como una subcarpeta de la aplicación.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Al abrir los sitios Web a través de HTTP, la configuración se lee desde la metabase de IIS (IIS Local) o mediante extensiones de servidor de FrontPage (sitio remoto). Si no hay aplicaciones web anidados, estos se muestran también con un icono que se identifica como una aplicación. Si está familiarizado con trabajar con aplicaciones web de FrontPage, el comportamiento en Visual Studio 2005 es similar.

Aunque Visual Studio mostrará un icono para las aplicaciones que se anidan debajo de la aplicación que está actualmente abierta en el IDE, le no permitirá expandirlas para ver su contenido. Sin embargo, puede, haga doble clic en ellos para abrirlos. Al hacerlo, se le mostrará un cuadro de diálogo que le pide que abra la aplicación web (y reemplazar la solución actualmente abierta) o agregar la aplicación Web a la solución actual.


![Haga doble clic en un icono de la aplicación anidada se presenta con este cuadro de diálogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: haga doble clic en un icono de la aplicación anidada se presenta con este cuadro de diálogo


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Sitio FTP

Cuando se abre un sitio a través de FTP, los archivos se todos copian localmente a la carpeta temporal. La ruta de acceso completa para la ubicación de almacenamiento local se muestra en el panel de propiedades para el proyecto y se crea con el formato siguiente.

C:/Documents and Settings /&lt;usuario&gt;/Local Temp/configuración/VWDWebCache/&lt;Server&gt;/_&lt;nombre de la aplicación&gt;

Cuando se utiliza FTP, Visual Studio será necesario especificar la dirección URL base para el proyecto para que pueda examinar tal y como se muestra a continuación. Si no especifica una dirección URL base, Visual Studio le pedirá la primera vez que intente examinar una página en el sitio Web.


![Especificar una dirección URL Base para los sitios FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: especificar una dirección URL Base para los sitios FTP


## <a name="improvements-in-compilation"></a>Mejoras en la compilación

Trabajar con aplicaciones Web en Visual Studio 2005 es considerablemente más rápida que en versiones anteriores. Esto es debido en ninguna parte pequeña a los cambios en la arquitectura de compilación.

En Visual Studio 2002 y 2003, las aplicaciones Web se compilan en un ensamblado principal que reside en la carpeta/bin. En Visual Studio 2005, se agregó una carpeta de aplicación/_Code. Clases y otro código de interfaz de usuario no se agregan a la carpeta de aplicación/_Code. Cuando Visual Studio compila el proyecto, todos los archivos en la carpeta de aplicación/_Code se compilan en un único archivo App/_Code.dll. El resultado de este cambio es que las compilaciones posteriores son mucho más rápidas que en versiones anteriores.

> [!NOTE]
> También se puede utilizar la utilidad de línea de comandos de MSBuild para compilar aplicaciones de ASP.NET Web. Dicha herramienta se explicará en el módulo 9.


Otra mejora de la compilación es la opción de página Generar nuevo en el menú Generar. Esta característica permite que un desarrollador volver a generar solo la página actual (junto con, del curso y dependencias) para que los cambios pueden compilarse más rápidamente. Dado que C# no ofrece la compilación en segundo plano para fines de actualización de IntelliSense, etc., beneficiará enormemente de esta característica porque eso permitirá para que IntelliSense para actualizar rápidamente basta con volver a generar una sola página.

Las propiedades de compilación de un proyecto le permiten configurar el tipo de compilación que se produce antes de que se ejecuta la página de inicio. Los programadores pueden optar por crear sólo la página actual para que Visual Studio puede comenzar a depurar las aplicaciones más rápidamente después de realizar cambios de código.


![La acción de inicio de la página de compilación](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: la acción de inicio de la página de compilación


Otra mejora excelente para Visual Studio y la arquitectura ASP.NET está en el área de editar y continuar. En Visual Studio 2005, los programadores pueden empezar a depurar un proyecto y realizar cambios de código en el proyecto sin desconectar al depurador. De hecho, literalmente puede comenzar a depurar un proyecto, agregue una nueva clase, agregue código para esa clase, agregue código a la página que crea una nueva instancia de esa clase y ejecutar un método de la clase, todo ello sin separar al depurador. Ejecutar el nuevo código es literalmente tan fácil como actualizar el explorador.

Haga clic aquí para ver un tutorial en vídeo de la edición y continúe con la característica en Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


La sólida de editar y continuar la funcionalidad de ASP.NET 2.0 y Visual Studio 2005 es debido a un cambio de arquitectura para las aplicaciones ASP.NET. En ASP.NET 1.x, las aplicaciones creadas en Visual Studio 2002/2003 se compila en un ensamblado primario que se almacena en la carpeta/bin. Todas las clases, páginas, etc. para la aplicación se compila en que un archivo DLL. A continuación, en tiempo de ejecución, ASP.NET podría compilar todos los controles, el marcado y el código ASP.NET en las páginas y copie los archivos DLL en la carpeta temporal de ASP.NET.

En Visual Studio 2005 con ASP.NET 2.0, los modelos de dos compilación describen anteriormente (uno para Visual Studio) y otro para ASP.NET en tiempo de ejecución se han combinado en un modelo común de compilación. Esto significa que todos los problemas de compilación se detectan durante la fase de desarrollo en lugar de en tiempo de ejecución. También se permite para el diseñador y compatibilidad con IntelliSense para características como controles de usuario y las páginas maestras.

Haga clic aquí para ver un tutorial en vídeo de compatibilidad con el diseñador para controles de usuario.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Cuando se quita un control de usuario de una página, el @Register directiva permanece en el marcado y debe quitarse manualmente para evitar errores del analizador si se elimina el control de usuario desde el sitio Web.


Otra mejora en el modelo de compilación de Visual Studio es la característica de publicación de sitios Web. Dado que la característica de publicación precompila el sitio Web, los desarrolladores pueden disfrutar el rendimiento agregado de no tener que compilar algo a petición. También precompila todo el código fuente en la carpeta de aplicación/_Code en un archivo DLL para que no hay código fuente debe implementarse.


![El cuadro de diálogo Publicar sitio Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: el cuadro de diálogo Publicar sitio Web


> [!NOTE]
> También se puede utilizar la utilidad aspnet/_compile.exe para precompilar una aplicación Web ASP.NET. Dicha herramienta se explicará en el módulo 9.


Al publicar un sitio Web, los archivos precompilados se almacena en la carpeta archivos temporales de ASP.NET tal y como se muestra a continuación. Archivos con un *.compiled* extensión de archivo son archivos XML que definen dependencias de determinados archivos DLL. Los controles de formulario Web Forms o un usuario se compilan en DLL aleatorias que comienzan por *aplicación /_Web /_*.

Si deja el *permitir que este sitio precompilado se actualizables* activa la casilla de verificación, marcado dentro de los controles de formularios Web Forms y el usuario no será precompilada en un archivo DLL que lo que le permite realizar cambios después de la implementación. Si prefiere bloquear el marcado para que no se permiten cambios en el contenido distribuido, desactive esta casilla.

El *uso corregido únicos y nomenclatura de ensamblados de página* casilla de verificación le permite deshabilitar la compilación por lotes para que cada página se compila en un ensamblado con nombre fijo. Deja desactivada esta casilla permite aprovechar las ventajas de la compilación por lotes.

El *habilitar nombres seguros en los ensamblados precompilan* casilla permite nombres seguros los ensamblados precompilados.

> [!NOTE]
> En ASP.NET 1.x, ensamblados con nombre seguro debían instalarse en la caché de ensamblados Global (GAC). En ASP.NET 2.0, no es necesario instalar los ensamblados con nombre seguro en la GAC.


![Un archivos precompiladas de aplicaciones de ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: un archivos precompiladas de aplicaciones de ASP.NET


> [!NOTE]
> En la aplicación anterior, no había ningún archivo web.config. Si se hubiese producido, habría ha llamado *PrecompiledApp.config* después de la publicación Web sitio proceso.


## <a name="improvements-in-deployment"></a>Mejoras en la implementación

Como con Visual Studio 2002 y 2003, Visual Studio 2005 ofrece una característica de proyecto de la copia. Sin embargo, la característica ha sido beefed en Visual Studio 2005 y ahora se denomina Copiar sitio Web.

El cuadro de diálogo Copiar sitio Web se divide en un marco de la izquierda y un marco de la derecha. El marco de la izquierda se denomina el sitio Web de origen y el marco de la derecha se denomina el sitio Web remoto. Único lo que puede confundir a algunos desarrolladores es que el sitio que se muestra en el marco derecho no es necesariamente un sitio remoto. Podría deberse a un sitio en el sistema de archivos local o en la instancia local de IIS. Además, el sitio que se muestra en el marco de la izquierda no es necesariamente el sitio Web de origen porque el cuadro de diálogo le permite publicar desde el sitio Web remoto *a* el sitio Web de origen.

Si va a copiar un proyecto en un sitio Web remoto, ese sitio debe tener instalado las extensiones de servidor de FrontPage. Si no es así, debe conectarse mediante FTP. Por otro lado, si va a copiar un proyecto a la instancia local de IIS, las extensiones de servidor de FrontPage no son necesarias.

> [!NOTE]
> Si intenta crear un nuevo sitio Web en la instancia local de IIS y se instalan las extensiones de servidor de FrontPage 2002, obtendrá un mensaje de error que le indica que la creación de sitios Web no se admite en un servidor de SharePoint. En ese caso, tiene la opción de instalar las extensiones de servidor de FrontPage 2000 o quitar las extensiones de servidor de FrontPage.


Haga clic aquí para ver un tutorial de vídeo de la característica de Copiar sitio Web.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Mejoras en la depuración

Hay cuatro mejoras claves en la depuración en Visual Studio 2005.

- Es posible de fábrica depura localmente como no administradores.
- El atributo de depuración para el elemento Compilation ahora es false de forma predeterminada.
- El programa de instalación y configuración de la depuración remota es más fácil que nunca.
- Ahora puede depurar un sitio Web abierto a través de una ubicación FTP.

## <a name="debugging-as-a-non-administrator"></a>Depuración como no administradores

La adición del servidor de desarrollo de ASP.NET permite depurar fácilmente aplicaciones de ASP.NET desde el cuadro que no son administradores. Cuando se depura una aplicación de ASP.NET que se ejecuta en el sistema de archivos local, Visual Studio inicia el servidor de desarrollo de ASP.NET en el contexto del usuario que ha iniciado sesión. Ese usuario, a continuación, puede depurar la aplicación sin ninguna configuración adicional.

## <a name="debug-is-false-by-default"></a>Depuración es False de forma predeterminada

En ASP.NET 1.x, la *depurar* de atributo en el *compilación* elemento del archivo web.config se ha establecido en *true* de forma predeterminada. Siempre se recomienda que los programadores establecer este atributo en *false* antes de implementar una aplicación en producción, sino también porque la mayoría de los desarrolladores no acaban de entender las consecuencias de dejar el atributo de depuración establecido en es true, simplemente deja como-es.

El problema más grave con el atributo de depuración establecido en true es que deshabilita el modelo de compilación por lotes ASP.NETs. Por lo tanto, cada página se compila en un archivo DLL independiente. Si un servidor Web aplicación consta de miles de páginas (no una capacidad de por ningún medio), que significa que se creará varias DLL pequeñas miles por esa aplicación. Aunque estos archivos DLL son un tamaño reducidas, que no se carguen en cualquier ubicación determinada en la memoria. Por lo tanto, se causa de la fragmentación de memoria del sistema y puede contribuir a OutOfMemoryException apariciones.

En ASP.NET 2.0, el atributo de depuración se establece en false de forma predeterminada. Como ya ha visto, cuando un desarrollador depura una aplicación ASP.NET en Visual Studio 2005, le pedirá que agregue un archivo web.config con la depuración habilitada. Si lo hace, incurre en los mismos inconvenientes que estaban presentes en ASP.NET 1.x, pero ahora el desarrollador claramente se advierte que se debe restablecer el atributo en false antes de mover la aplicación en producción.

## <a name="remote-debugging-setup-and-configuration"></a>Instalación y configuración de depuración remota

Depuración remota en Visual Studio 2002/2003, dependiera Machine Debug Manager (mdm.exe) y el proceso de vs7jit.exe. Debido a eso, solución de problemas de depuración remotas era a menudo un cuadro negro para los clientes y, a menudo no era mucho mejor de PSS.

Visual Studio 2005, se elimina la dependencia en los procesos de mdm.exe y vs7jit.exe. En su lugar, ahora usa el servicio de Monitor de depuración remota (msvsmon.exe).

El requisito para la depuración remota en Visual Studio 2005 es muy sencillo. Debe ejecutar msvsmon.exe en el servidor remoto antes de depurar. Puede instalar al Monitor de depuración remota desde el CD de Visual Studio o simplemente ejecute msvsmon.exe desde un recurso compartido sin instalar nada en absoluto en el servidor Web.

Cuando se ejecuta msvsmon.exe, es probable que se quejan de puertos que se va a bloquear para la depuración remota. Afortunadamente, fácilmente puede desbloquear los puertos de la derecha en el cuadro de diálogo de advertencia tal y como se muestra a continuación.


![Notificación de que Firewall de Windows está bloqueando la depuración remota](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: notificación de que Firewall de Windows está bloqueando la depuración remota


Una vez que se ha desbloqueado los puertos necesarios para la depuración, verá al Monitor de depuración remota tal y como se muestra a continuación. Desde esta interfaz, puede supervisar las conexiones y cambiar fácilmente los permisos de depuración.


![El Monitor de depuración remota](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: el Monitor de depuración remota


También es posible depurar una aplicación Web abierta a través de FTP de forma remota. Los pasos son los mismos que los cubierto anteriormente. Sin embargo, debe especificar una dirección URL base para examinar el proyecto FTP, tal y como se describió anteriormente en este módulo.

## <a name="lab-2"></a>Práctica 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Depuración remota con Visual Studio 2005

Esta práctica le guiará a través de la depuración remota con Visual Studio 2005.

Haga clic aquí para ver un tutorial en vídeo de este laboratorio.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Abra vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Esta práctica requiere disponer de dos equipos, una ejecución Visual Studio 2005 y el otro está ejecutando IIS 5 o superior.

1. Abra Visual Studio 2005 y cree un nuevo sitio Web en el servidor remoto.

> [!NOTE]
> Puede crear el sitio Web en una instancia remota de IIS o a través de FTP.


1. Desde el servidor Web remoto, busque msvsmon.exe en el equipo de desarrollo mediante una ruta UNC y ejecutarlo.  
 La ubicación predeterminada de msvsmon.exe es //server/c$/Program archivos/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Si se le pide que desbloquear los puertos para la depuración remota, hágalo.
3. En el equipo de desarrollo, abra el código subyacente para Default.aspx y establecer un punto de interrupción en el método de página/_Load.
4. Iniciar la depuración desde el equipo de desarrollo.

Debe alcanzar el punto de interrupción según lo previsto.

## <a name="aspnet-development-server"></a>servidor de desarrollo de ASP.NET

Como hemos ya mencionado, Visual Studio 2005 se suministra con un servidor Web como el servidor de desarrollo de ASP.NET. (El servidor de desarrollo de ASP.NET se conoce a veces como Cassini.) Este servidor Web es una forma cómoda para examinar y depurar aplicaciones Web que se ejecutan en el sistema de archivos.

El servidor de desarrollo de ASP.NET es un servidor Web restringido. No permite las conexiones remotas, no permite las solicitudes de cualquier usuario distinto del usuario que inició el servidor Web. No tiene la capacidad de ofrecer servicio a las páginas ASP. Se sirven solo los recursos ASP.NET y los recursos HTML (incluidas imágenes, archivos CSS, etc.).

Se puede iniciar el servidor de desarrollo de ASP.NET a través de la línea de comandos ejecutando el archivo WebDev.WebServer.exe ubicado en c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. El siguiente cuadro de diálogo muestra los parámetros que están disponibles.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**


> [!NOTE]
> El servidor de desarrollo de ASP.NET no se admite cuando se inicia de forma explícita mediante la línea de comandos.
