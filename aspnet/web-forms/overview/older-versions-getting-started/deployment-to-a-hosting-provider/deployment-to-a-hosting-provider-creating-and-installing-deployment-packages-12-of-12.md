---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: solución de problemas (12 de 12) | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc3a412820638b347d2e5781c01481f622dd2033
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376354"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: solución de problemas (12 de 12)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo implementar en sitios Web de Windows Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Esta página describen algunos problemas comunes que pueden surgir al implementar una aplicación web ASP.NET con Visual Studio. Para cada uno de ellos se proporcionan uno o más posibles causas y las soluciones correspondientes.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Error del servidor en '/' Application - configuración actual de errores personalizados impedir que los detalles del Error se ven de forma remota

### <a name="scenario"></a>Escenario

Después de implementar un sitio en un host remoto, obtendrá un mensaje de error que se menciona la configuración customErrors en el archivo Web.config, pero no indica cuál fue la causa real del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

De forma predeterminada, ASP.NET se muestra información detallada del error solo cuando la aplicación web se ejecuta en el equipo local. Por lo general no desea mostrar información detallada del error cuando la aplicación web está disponible públicamente a través de Internet, ya que los piratas informáticos es posible que pueda usar esta información para encontrar vulnerabilidades en la aplicación. Sin embargo, al implementar un sitio o actualizaciones a un sitio, en ocasiones, algo vaya mal y necesita obtener el mensaje de error real.

Para habilitar la aplicación para mostrar mensajes de error detallados cuando se ejecuta en el host remoto, edite el archivo Web.config para establecer `customErrors` modo desactivado, volver a implementar la aplicación y ejecute de nuevo la aplicación:

1. Si el archivo Web.config de la aplicación tiene un `customErrors` elemento en el `system.web` elemento, cambie el `mode` atributo en "off". En caso contrario, agregue un `customErrors` elemento en el `system.web` elemento con el `mode` atributo establecido en "off", tal como se muestra en el ejemplo siguiente:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Implemente la aplicación.
3. Ejecute la aplicación y repita lo que hizo anteriormente que produjo el error que se produzca. Ahora puede ver lo que es el mensaje de error real.
4. Cuando haya resuelto el error, restaure el original `customErrors` configuración y volver a implementar la aplicación.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Se deniega el acceso en una página Web que usa SQL Server Compact

### <a name="scenario"></a>Escenario

Cuando implemente un sitio que usa SQL Server Compact y ejecuta una página en el sitio implementado que tiene acceso a la base de datos, vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

La cuenta de servicio de red en el servidor debe ser capaz de leer el servicio de SQL Compact archivos binarios nativos que están en el *bin\amd64* o *bin\x86* carpeta, pero no tiene permisos de lectura para esas carpetas. Conjunto de permisos de lectura para el servicio de red en el *bin* carpeta, asegurándose de ampliar los permisos para las subcarpetas.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>No se puede leer el archivo de configuración debido a permisos insuficientes

### <a name="scenario"></a>Escenario

Al hacer clic en la publicación de Visual Studio botón para implementar una aplicación en IIS en el equipo local, se produce un error de publicación y la **salida** ventana muestra un mensaje de error similar al siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Para usar un solo clic publicar en IIS en el equipo local, debe ejecutar Visual Studio con permisos de administrador. Cierre Visual Studio y reiniciarlo con permisos de administrador.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>No se pudo conectar al equipo de destino... Mediante el proceso especificado

### <a name="scenario"></a>Escenario

Al hacer clic en la publicación de Visual Studio botón para implementar una aplicación, se produce un error de publicación y la **salida** ventana muestra un mensaje de error similar al siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Un servidor proxy es interrumpir la comunicación con el servidor de destino. En el Panel de Control de Windows o en Internet Explorer, seleccione **opciones de Internet** y seleccione el **conexiones** ficha. En el **propiedades de Internet** cuadro de diálogo, haga clic en **Configuración LAN**. En el **configuración de red de área Local (LAN)** cuadro de diálogo, desactive la **detectar automáticamente la configuración** casilla de verificación. A continuación, haga clic en el botón Publicar nuevo.

Si el problema persiste, póngase en contacto con el administrador del sistema para determinar qué se puede hacer con la configuración del firewall o proxy. El problema se produce porque Web Deploy usa un puerto no estándar para la implementación de servicio de administración Web (8172); para otras conexiones, Web Deploy usa el puerto 80. Cuando se implementa en un proveedor de hospedaje de terceros, normalmente usa el servicio de administración Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>No existe el grupo de aplicaciones 4.0 de .NET de forma predeterminada

### <a name="scenario"></a>Escenario

Al implementar una aplicación que requiere .NET Framework 4, vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

ASP.NET 4 no está instalado en IIS. Si el servidor que está implementando es el equipo de desarrollo y tiene instalado Visual Studio 2010, ASP.NET 4 está instalado en el equipo pero no puede instalarse en IIS. En el servidor que va a implementar en, abra un símbolo del sistema con privilegios elevados e instale ASP.NET 4 en IIS mediante la ejecución de los siguientes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

También es posible que deba establecer manualmente la versión de .NET Framework del grupo de aplicaciones predeterminado. Para obtener más información, consulte el [implementar en IIS como un entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formato de la cadena de inicialización no cumple la especificación empezando por el índice 0.

### <a name="scenario"></a>Escenario

Después de implementar una aplicación con un solo clic publicar, cuando ejecuta una página que tiene acceso a la base de datos reciba el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Abra el *Web.config* archivo en el sitio implementado y compruebe si los valores de cadena de conexión comienzan con `$(ReplacableToken_`, como en el ejemplo siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Si las cadenas de conexión parecen a este ejemplo, edite el archivo de proyecto y agregue la siguiente propiedad a la `PropertyGroup` elemento que es para todas las configuraciones de compilación:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

A continuación, volver a implementar la aplicación.

## <a name="http-500-internal-server-error"></a>Error de servidor interno 500 de HTTP

### <a name="scenario"></a>Escenario

Cuando se ejecuta el sitio implementado, vea el siguiente mensaje de error sin información específica que indica la causa del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Hay muchas causas de 500 errores, pero una posible causa si está siguiendo estos tutoriales es colocar un elemento XML en el lugar incorrecto en uno de los archivos de transformación XML. Por ejemplo, obtendría este error si coloca la transformación que inserta un `<location>` elemento bajo `<system.web>` en lugar de directamente en `<configuration>`. En ese caso es la solución corregir el archivo de transformación de XML y volver a implementar.

## <a name="http-50021-internal-server-error"></a>Error interno del servidor 500.21 de HTTP

### <a name="scenario"></a>Escenario

Cuando se ejecuta el sitio implementado, vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

El sitio ha implementado los destinos de ASP.NET 4, pero ASP.NET 4 no está registrado en IIS en el servidor. En el servidor, abra un símbolo del sistema con privilegios elevados y registrar ASP.NET 4 mediante la ejecución de los siguientes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

También es posible que deba establecer manualmente la versión de .NET Framework del grupo de aplicaciones predeterminado. Para obtener más información, consulte el [implementar en IIS como un entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Error al abrir SQL Server Express Database en la aplicación de inicio de sesión\_datos

### <a name="scenario"></a>Escenario

Le mantendrá al día el *Web.config* archivo de cadena de conexión para que señale a una base de datos de SQL Server Express como un *.mdf* de archivos en su *aplicación\_datos* carpeta y la primera hora de que ejecutar la aplicación que verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

El nombre de la *.mdf* archivo no puede coincidir con el nombre de cualquier base de datos SQL Server Express que alguna vez ha existido en el equipo, incluso si elimina el *.mdf* archivo de la base de datos ya existente. Cambiar el nombre de la *.mdf* archivo a un nombre que nunca ha usado como un nombre de base de datos y cambio el *Web.config* archivo que se usará el nuevo nombre. Como alternativa, puede usar [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) existentes previamente SQL Server Express de eliminar las bases de datos.

## <a name="model-compatibility-cannot-be-checked"></a>Compatibilidad de modelo no se puede comprobar

### <a name="scenario"></a>Escenario

Se actualizó el *Web.config* archivo de cadena de conexión para que señale a una nueva base de datos SQL Server Express, y la primera vez que ejecute la aplicación verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Si el nombre de la base de datos que colocan en el archivo Web.config se ha utilizado nunca antes en el equipo, puede que ya exista una base de datos con algunas tablas en ella. Seleccione un nuevo nombre que no se ha usado en el equipo antes de cambiar el *Web.config* archivo para que señale para usar este nuevo nombre de base de datos. Como alternativa, puede usar [utilidad Express de SQL Server](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) o [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para eliminar la base de datos existente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Error SQL cuando se trata de una secuencia de comandos crear usuarios o Roles

### <a name="scenario"></a>Escenario

Usa la implementación de la base de datos configurada en el **Empaquetar/publicar SQL** ficha, scripts SQL que se ejecutan durante la implementación incluyen comandos Create User o Create Role y produce un error de ejecución de secuencia de comandos cuando se ejecutan esos comandos. Es posible que vea que más detallada de los mensajes, como las siguientes:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Si este error se produce cuando se ha configurado la implementación de la base de datos en el **publicación Web** asistente en lugar de **Empaquetar/publicar SQL** pestaña, cree un subproceso en el [configuración y Implementación](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) foro y la solución se agregarán a esta página de solución de problemas.

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

La cuenta de usuario que se usa para realizar la implementación no tiene permiso para crear usuarios o roles. Por ejemplo, podría asignar la empresa de hospedaje el `db_datareader`, `db_datawriter`, y `db_ddladmin` roles a la cuenta de usuario que configura automáticamente. Estos son suficientes para crear la mayoría de los objetos de base de datos, pero no para la creación de usuarios o roles. Una manera de evitar el error es mediante la exclusión de usuarios y roles de implementación de la base de datos. Para ello, edite el `PreSource` (elemento) para la base de datos de la genera automáticamente la secuencia de comandos para que incluya los siguientes atributos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Para obtener información sobre cómo editar el `PreSource` el elemento en el archivo de proyecto, vea [Cómo: modificar la configuración de implementación en el archivo de proyecto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Si los usuarios o roles en la base de datos de desarrollo deben estar en la base de datos de destino, póngase en contacto con su proveedor de hospedaje.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Error de tiempo de espera SQL Server al ejecutar Scripts personalizados durante la implementación

### <a name="scenario"></a>Escenario

Ha especificado los scripts SQL personalizados para ejecutar durante la implementación y, cuando se ejecuta ellos con Web Deploy, el tiempo de espera.

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Varios scripts que tienen modos de transacción diferentes puede provocar errores en tiempo de espera. De forma predeterminada, ejecutan scripts generados automáticamente en una transacción, pero no lo hacen los scripts personalizados. Si selecciona el **extraer datos y/o esquema de una base de datos** opción el **Empaquetar/publicar SQL** ficha, y si agrega un script SQL personalizado, debe cambiar la configuración de transacciones en algunos scripts para que todos los scripts usan la misma configuración de la transacción. Para obtener más información, consulte [Cómo: implementar una base de datos con un proyecto de aplicación Web](https://msdn.microsoft.com/library/dd465343.aspx).

Si ha configurado la configuración de transacciones para que todos son iguales, pero sigue apareciendo este error, una posible solución alternativa es ejecutar las secuencias de comandos por separado. En el **secuencias de comandos de base de datos** cuadrícula en el **Empaquetar/publicar** pestaña SQL, desactive la **Include** casilla de verificación de la secuencia de comandos que produce el error de tiempo de espera, a continuación, publicar el proyecto. A continuación, vaya a la **secuencias de comandos de base de datos** cuadrícula, seleccione ese script **Include** casilla de verificación y desactive el **Include** casillas de verificación de los demás scripts. A continuación, vuelva a publicar el proyecto. Este tiempo al publicar, solo las ejecuciones de script personalizado seleccionado.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Datos de Stream de manifiesto de sitio no están disponibles

### <a name="scenario"></a>Escenario

Si está instalando un paquete mediante el *deploy.cmd* archivo con el `t` opción (prueba), vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

El mensaje de error significa que el comando no puede generar un informe de pruebas. Sin embargo, puede ejecutar el comando si usas el `y` opción (instalación real). El mensaje sólo indica que hay un problema al ejecutar el comando en modo de prueba.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Esta aplicación requiere ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Escenario

Cuando se intenta implementar, consulte el siguiente mensaje de error:

 Error: Los datos de secuencia de ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' no está disponible. El grupo de aplicaciones que está intentando usar tiene la propiedad 'managedRuntimeVersion' establecida en 'v2.0'. Esta aplicación requiere "v4.0". 

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

ASP.NET 4 no está instalado en IIS. Si el servidor que está implementando es el equipo de desarrollo y tiene instalado Visual Studio 2010, ASP.NET 4 está instalado en el equipo pero no puede instalarse en IIS. En el servidor que va a implementar en, abra un símbolo del sistema con privilegios elevados e instale ASP.NET 4 en IIS mediante la ejecución de los siguientes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>No se puede convertir Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Escenario

Al implementar un paquete, vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Está intentando implementar desde el Administrador de IIS mediante la UI Web de implementación de 1.1 a un servidor que tiene instalado Web Deploy 2.0. Si usa la herramienta de administración remota de IIS para implementar mediante la importación de un paquete, compruebe el **nuevas características disponibles** cuadro de diálogo al establecer la conexión. (Este cuadro de diálogo es posible que solo se muestra una vez cuando se establece primero la conexión. Para borrar la conexión y empezar de nuevo, cierre el Administrador de IIS y lo abra de nuevo mediante la especificación de `inetmgr /reset` en el símbolo del sistema.) Si una de las características en la lista es **Implementar interfaz de usuario Web**y tiene un número de versión inferior a 8, el servidor que está implementando podría tener las versiones 1.1 y 2.0 de Web Deploy instalado. Para implementar desde un cliente que ha instalado 2.0, el servidor debe tener solo Web Deploy 2.0 instalado. Tendrá que ponerse en contacto con su proveedor de hospedaje para resolver este problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>No se puede cargar los componentes nativos de SQL Server Compact

### <a name="scenario"></a>Escenario

Cuando se ejecuta el sitio implementado, vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

No tiene el sitio implementado *amd64* y *x86* subcarpetas con los ensamblados nativos en ellos en la aplicación *bin* carpeta. En un equipo que tiene SQL Server Compact instalado, se encuentran los ensamblados nativos en *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Es la mejor manera de obtener los archivos correctos en las carpetas correctas en un proyecto de Visual Studio instalar el paquete NuGet SqlServerCompact. Instalación del paquete agrega un script posterior a la compilación para copiar los ensamblados nativos en *amd64* y *x86*. Sin embargo, en orden de estos para implementarse, tiene que incluirlos manualmente en el proyecto. Para obtener más información, consulte el [implementar SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Error de "Ruta de acceso no es válido" después de implementar una aplicación de Entity Framework Code First

### <a name="scenario"></a>Escenario

Implementar una aplicación que usa migraciones de Entity Framework Code First y un DBMS como SQL Server Compact que almacena su base de datos en un archivo en la aplicación\_carpeta de datos. Tener migraciones de Code First configurada para crear la base de datos tras la primera implementación. Al ejecutar la aplicación obtendrá un mensaje de error similar al ejemplo siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Código primero intenta crear la base de datos, pero la aplicación\_no existe la carpeta de datos. O bien no hay ningún archivo el *aplicación\_datos* carpeta cuando se ha implementado o seleccionó **Excluir aplicación\_datos** en el **Empaquetar/Publicar Web** pestaña de la **las propiedades del proyecto** ventana. El proceso de implementación no creará una carpeta en el servidor si no hay ningún archivo en la carpeta que se copiarán en el servidor. Si ya ha configurado en el sitio de la base de datos, el proceso de implementación eliminará los archivos y la *aplicación\_datos* propia si seleccionó carpeta **quitar archivos adicionales en destino** en el perfil de publicación. Para solucionar el problema, coloque un archivo de marcador de posición como un archivo .txt en el *aplicación\_datos* carpeta, asegúrese de que no es necesario **Excluir aplicación\_datos** seleccionado y volver a implementar. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"El objeto COM que se ha separado de su RCW subyacente no puede utilizarse."

### <a name="scenario"></a>Escenario

Ha sido correctamente con un solo clic publicar para implementar la aplicación y, a continuación, empezará a recibir este error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Cerrar y reiniciar Visual Studio normalmente es todo lo que es necesario para resolver este error.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Implementación produce un error debido a usuario usan credenciales para la publicación no tiene setACL autoridad

### <a name="scenario"></a>Escenario

Publicación se produce un error que indica que no tiene autoridad para establecer permisos de carpetas (que usa la cuenta de usuario no tiene autoridad setACL).

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

De forma predeterminada, Visual Studio los conjuntos de permisos de lectura en la carpeta raíz del sitio y permisos de escritura en la aplicación\_carpeta de datos. Si sabe que los permisos predeterminados en las carpetas del sitio son correctos y no deben establecerse, deshabilitar este comportamiento agregando **&lt;IncludeSetACLProviderOn destino&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** para el archivo de perfil de publicación (que afecta a un solo perfil) o para los archivos de destino.WPP (que afecta a todos los perfiles). Para obtener información acerca de cómo editar estos archivos, consulte [Cómo: modificar la configuración de implementación en los archivos de perfil (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Errores de acceso denegado cuando la aplicación intenta escribir en una carpeta de aplicación

### <a name="scenario"></a>Escenario

Los errores de aplicación cuando intenta crear o editar un archivo en una de las carpetas de aplicación, porque no tiene autoridad de escritura para esa carpeta.

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

De forma predeterminada, Visual Studio los conjuntos de permisos de lectura en la carpeta raíz del sitio y permisos de escritura en la aplicación\_carpeta de datos. Si la aplicación necesita acceso de escritura en una subcarpeta, puede establecer permisos para esa carpeta, como se muestra en el [establecer permisos de carpeta](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) y [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutoriales. Si la aplicación necesita acceso de escritura a la carpeta raíz del sitio, tendrá que evitar que la configuración de acceso de solo lectura en la carpeta raíz agregando **&lt;IncludeSetACLProviderOn destino&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** para el archivo de perfil de publicación (que afecta a un solo perfil) o para los archivos de destino.WPP (que afecta a todos los perfiles). Para obtener información acerca de cómo editar estos archivos, consulte [Cómo: modificar la configuración de implementación en los archivos de perfil (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Error de configuración: atributo targetFramework hace referencia a una versión posterior a la versión instalada de .NET Framework

### <a name="scenario"></a>Escenario

Se publicó correctamente un proyecto web que tenga como destino ASP.NET 4.5, pero al ejecutar la aplicación (con el `customErrors` modo establecido en "off" en el archivo Web.config) muestra el siguiente error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

El cuadro de Error de origen de la página de error resalta la línea siguiente del archivo Web.config como la causa del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

El servidor no admite ASP.NET 4.5. Póngase en contacto con el proveedor de hospedaje para determinar cuándo y si puede agregarse compatibilidad para ASP.NET 4.5. Si actualiza el servidor no es una opción, tendrá que implementar un proyecto web que tenga como destino ASP.NET 4 o versiones anteriores en su lugar. Si implementa un 4 de ASP.NET o un proyecto de web anterior al mismo destino, seleccione el **quitar archivos adicionales en destino** casilla de verificación en la **configuración** pestaña de la **publicación Web**asistente. Si no selecciona **quitar archivos adicionales en destino**, se seguirá pudiendo obtener la página de Error de configuración.

El proyecto **propiedades** windows incluye una lista desplegable de plataforma de destino, pero no se puede resolver este problema cambiando simplemente desde **.NET Framework 4.5** a **de.NETFramework4**. Si cambia la plataforma de destino a una versión anterior de framework, el proyecto seguirá teniendo referencias a ensamblados de la última versión marco de trabajo y no se ejecutará. Tendrá que cambiar esas referencias manualmente o crear un nuevo proyecto que tenga como destino .NET Framework 4 o versiones anterior. Para obtener más información, consulte [compatibilidad de .NET Framework para sitios Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
