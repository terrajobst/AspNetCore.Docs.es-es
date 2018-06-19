---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: solución de problemas (12 de 12) | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2a8342f026498a7cf3ff4a3c158ed177c15b7111
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890390"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: solución de problemas (12 de 12)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web. También puede usar Visual Studio 2010 si instala la actualización de publicación de Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar sitios Web de Windows Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Esta página describe algunos problemas comunes que pueden surgir al implementar una aplicación web ASP.NET mediante Visual Studio. Para cada uno de ellos se proporcionan una o varias posibles causas y soluciones correspondientes.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Error del servidor en '/' aplicación - configuración actual de errores personalizados evitar que los detalles del Error no puedan ser vistos de forma remota

### <a name="scenario"></a>Escenario

Después de implementar un sitio a un host remoto, obtendrá un mensaje de error que hace referencia a la configuración de customErrors en el archivo Web.config pero no indica cuál fue la causa real del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

De forma predeterminada, ASP.NET muestra información detallada del error cuando la aplicación web se ejecuta en el equipo local. Por lo general no desea mostrar información detallada del error cuando la aplicación web esté disponible públicamente a través de Internet, ya que los piratas informáticos puede utilizar esta información para buscar puntos vulnerables en la aplicación. Sin embargo, cuando se implemente un sitio o se actualizan a un sitio, en ocasiones, algo saldrá mal y necesita obtener el mensaje de error real.

Para habilitar la aplicación mostrar mensajes de error detallados cuando se ejecuta en el host remoto, edite el archivo Web.config para establecer `customErrors` modo desactivado, volver a implementar la aplicación y ejecute de nuevo la aplicación:

1. Si el archivo Web.config de la aplicación tiene un `customErrors` elemento en el `system.web` elemento, cambiar la `mode` atributo en "off". En caso contrario, agregue un `customErrors` elemento en el `system.web` elemento con la `mode` atributo establecido en "off", tal como se muestra en el ejemplo siguiente:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Implementar la aplicación.
3. Ejecute la aplicación y repita lo hizo anteriormente que produjo el error que se produzca. Ahora puede ver lo que es el mensaje de error real.
4. Cuando haya resuelto el error, restaure el original `customErrors` configurar e implementar la aplicación.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Se denegó el acceso en una página Web que usa SQL Server Compact

### <a name="scenario"></a>Escenario

Cuando se implementa un sitio que usa SQL Server Compact y ejecutar una página en el sitio implementado que tiene acceso a la base de datos, verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

La cuenta de servicio de red en el servidor debe ser capaz de leer archivos binarios nativos SQL Compact de servicio que se encuentran en el *bin\amd64* o *bin\x86* carpeta, pero no tener permisos de lectura para esas carpetas. Conjunto de permisos de lectura para el servicio de red en el *bin* carpeta, asegurándose de ampliar los permisos a las subcarpetas.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>No se puede leer el archivo de configuración debido a permisos insuficientes

### <a name="scenario"></a>Escenario

Al hacer clic en Visual Studio botón Publicar para implementar una aplicación en IIS en el equipo local, se produce un error de publicación y la **salida** ventana muestra un mensaje de error similar al siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Para usar un solo clic publicar en IIS en el equipo local, debe estar ejecutando Visual Studio con permisos de administrador. Cierre Visual Studio y reiniciarlo con permisos de administrador.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>No se pudo conectar al equipo de destino... Mediante el proceso especificado

### <a name="scenario"></a>Escenario

Al hacer clic en Visual Studio botón Publicar para implementar una aplicación, se produce un error de publicación y la **salida** ventana muestra un mensaje de error similar al siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Un servidor proxy es interrumpir la comunicación con el servidor de destino. Desde el Panel de Control de Windows o en Internet Explorer, seleccione **opciones de Internet** y seleccione la **conexiones** ficha. En el **propiedades de Internet** cuadro de diálogo, haga clic en **configuración de LAN**. En el **configuración de red de área Local (LAN)** cuadro de diálogo, desactive la **detectar automáticamente la configuración** casilla de verificación. A continuación, haga clic en el botón Publicar nuevo.

Si el problema continúa, póngase en contacto con el administrador del sistema para determinar qué se puede hacer con la configuración de proxy o firewall. El problema ocurre porque Web Deploy usa un puerto no estándar para la implementación de servicio de administración Web (8172); para otras conexiones, Web Deploy usa el puerto 80. Cuando se implementan en un proveedor de hospedaje de terceros, normalmente se usan el servicio de administración de Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>No existe el grupo de aplicaciones 4.0 de .NET de forma predeterminada

### <a name="scenario"></a>Escenario

Al implementar una aplicación que requiere .NET Framework 4, vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

ASP.NET 4 no está instalado en IIS. Si el servidor que está implementando es el equipo de desarrollo y tiene instalado Visual Studio 2010, ASP.NET 4 está instalado en el equipo pero no puede instalarse en IIS. En el servidor que va a implementar en, abra un símbolo del sistema con privilegios elevados e instale ASP.NET 4 en IIS mediante la ejecución de los siguientes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

También tendrá que establecer manualmente la versión de .NET Framework del grupo de aplicaciones predeterminado. Para obtener más información, consulte el [implementación en IIS como un entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formato de la cadena de inicialización no cumple la especificación que comienza en el índice 0.

### <a name="scenario"></a>Escenario

Después de implementar una aplicación con un solo clic publicar, cuando ejecuta una página que tiene acceso a la base de datos obtendrá el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Abra la *Web.config* archivo en el sitio implementado y compruebe si los valores de cadena de conexión comienzan por `$(ReplacableToken_`, como en el ejemplo siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Si las cadenas de conexión el aspecto mostrado en este ejemplo, edite el archivo de proyecto y agregue la siguiente propiedad a la `PropertyGroup` elemento que es para todas las configuraciones de compilación:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

A continuación, volver a implementar la aplicación.

## <a name="http-500-internal-server-error"></a>Error de servidor interno 500 de HTTP

### <a name="scenario"></a>Escenario

Cuando se ejecuta el sitio implementado, vea el siguiente mensaje de error sin información específica que indica la causa del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Hay muchas causas de 500 errores, pero una posible causa si está siguiendo estos tutoriales es colocar un elemento XML en el lugar incorrecto en uno de los archivos de transformación XML. Por ejemplo, obtendría este error si coloca la transformación que inserta un `<location>` elemento bajo `<system.web>` en lugar de directamente en `<configuration>`. En ese caso, la solución es corregir el archivo de transformación XML y volver a implementar.

## <a name="http-50021-internal-server-error"></a>Error interno del servidor 500.21 de HTTP

### <a name="scenario"></a>Escenario

Cuando se ejecuta el sitio implementado, vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

El sitio se han implementado destinos ASP.NET 4, pero ASP.NET 4 no está registrado en IIS en el servidor. En el servidor abra un símbolo del sistema con privilegios elevados y ejecute los siguientes comandos para registrar ASP.NET 4:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

También tendrá que establecer manualmente la versión de .NET Framework del grupo de aplicaciones predeterminado. Para obtener más información, consulte el [implementación en IIS como un entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Error de inicio de sesión abriendo SQL Server Express base de datos de aplicación\_datos

### <a name="scenario"></a>Escenario

Informado el *Web.config* cadena de conexión para que señale a una base de datos de SQL Server Express como archivo un *.mdf* un archivo en su *aplicación\_datos* carpeta y también el primero tiempo que se ejecuta la aplicación que verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

El nombre de la *.mdf* archivo no puede coincidir con el nombre de cualquier base de datos SQL Server Express que alguna vez ha existido en el equipo, incluso si elimina el *.mdf* archivos de la base de datos ya existente. Cambiar el nombre de la *.mdf* archivo por un nombre que nunca se ha utilizado como un nombre de base de datos y cambiar la *Web.config* archivo que se usará el nuevo nombre. Como alternativa, puede usar [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) eliminar existente previamente SQL Server Express bases de datos.

## <a name="model-compatibility-cannot-be-checked"></a>Compatibilidad de modelo no se puede comprobar

### <a name="scenario"></a>Escenario

Se actualizó la *Web.config* cadena de conexión para que señale a una nueva base de datos SQL Server Express de archivo y la primera vez que ejecute la aplicación verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Si el nombre de la base de datos se colocan en el archivo Web.config se ha utilizado alguna vez antes de que en el equipo, puede que ya exista una base de datos con algunas tablas en ella. Seleccione un nuevo nombre que no se ha utilizado en el equipo antes de y cambiar la *Web.config* archivo de punto para usar este nuevo nombre de base de datos. Como alternativa, puede usar [utilidad Express de SQL Server](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) o [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para eliminar la base de datos existente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Error SQL cuando trate de una secuencia de comandos crear usuarios o Roles

### <a name="scenario"></a>Escenario

Usa la implementación de la base de datos configurada en el **Empaquetar/publicar SQL** ficha, secuencias de comandos SQL que se ejecutan durante la implementación incluyen comandos Create User o Create Role y produce un error de ejecución de secuencia de comandos cuando se ejecutan los comandos. Puede ver más mensajes, como las siguientes:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Si este error se produce cuando se ha configurado la implementación de la base de datos en el **Publicar Web** asistente en lugar del **Empaquetar/publicar SQL** ficha, cree un subproceso en el [configuración y Implementación](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) foro y la solución se agregará a esta página para solucionar problemas.

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

La cuenta de usuario que usa para realizar la implementación no tiene permiso para crear usuarios o roles. Por ejemplo, podría asignar la empresa de hospedaje la `db_datareader`, `db_datawriter`, y `db_ddladmin` roles a la cuenta de usuario que configura automáticamente. Estos son suficientes para crear la mayoría de los objetos de base de datos, pero no para crear usuarios o roles. Una manera de evitar el error es mediante la exclusión de usuarios y roles de implementación de base de datos. Puede hacerlo mediante la edición de la `PreSource` (elemento) para la base de datos de la genera automáticamente la secuencia de comandos para que incluya los siguientes atributos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Para obtener información sobre cómo editar la `PreSource` el elemento en el archivo de proyecto, vea [Cómo: modificar la configuración de implementación en el archivo de proyecto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Si los usuarios o roles en la base de datos de desarrollo deben estar en la base de datos de destino, póngase en contacto con su proveedor de hospedaje.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Error de tiempo de espera SQL Server al ejecutar Scripts personalizados durante la implementación

### <a name="scenario"></a>Escenario

Ha especificado los scripts SQL personalizados para que se ejecute durante la implementación y, cuando se ejecuta ellos con Web Deploy, el tiempo de espera.

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Ejecutar varias secuencias de comandos que tienen modos de transacción diferentes, puede producir errores de tiempo de espera. De forma predeterminada, ejecutan scripts generados automáticamente en una transacción, pero scripts personalizados no tienen que serlo. Si selecciona el **extraer datos y/o esquema de una base de datos** opción el **Empaquetar/publicar SQL** ficha, y si agrega un script SQL personalizado, debe cambiar la configuración de la transacción en algunos scripts para que todos los scripts usan la misma configuración de transacción. Para obtener más información, consulte [Cómo: implementar una base de datos con un proyecto de aplicación Web](https://msdn.microsoft.com/library/dd465343.aspx).

Si ha configurado la configuración de transacciones para que todos son iguales, pero sigue apareciendo este error, una posible solución alternativa es ejecutar las secuencias de comandos por separado. En el **secuencias de comandos de base de datos** cuadrícula en el **Empaquetar/publicar** pestaña SQL, desactive el **Include** casilla de verificación para el script que generó el error de tiempo de espera, a continuación, publique el proyecto. A continuación, vaya a la **secuencias de comandos de base de datos** cuadrícula, seleccione ese script **Include** casilla de verificación y desactive el **Include** casillas de verificación de los demás scripts. A continuación, vuelva a publicar el proyecto. Este tiempo al publicar, se ejecuta solo el script personalizado seleccionado.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Datos de la secuencia del manifiesto del sitio no están disponibles

### <a name="scenario"></a>Escenario

Si está instalando un paquete mediante la *deploy.cmd* de archivos con la `t` opción (prueba), verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

El mensaje de error significa que el comando no puede generar un informe de pruebas. Sin embargo, puede ejecutar el comando si usas el `y` opción (instalación real). El mensaje sólo indica que hay un problema al ejecutar el comando en modo de prueba.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Esta aplicación requiere ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Escenario

Cuando intenta implementar, verá el mensaje de error siguiente:

 Error: Los datos de la secuencia de ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' no está disponible. El grupo de aplicaciones que está intentando usar tiene la propiedad 'managedRuntimeVersion' establecida en 'v2.0'. Esta aplicación requiere "v4.0". 

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

ASP.NET 4 no está instalado en IIS. Si el servidor que está implementando es el equipo de desarrollo y tiene instalado Visual Studio 2010, ASP.NET 4 está instalado en el equipo pero no puede instalarse en IIS. En el servidor que va a implementar en, abra un símbolo del sistema con privilegios elevados e instale ASP.NET 4 en IIS mediante la ejecución de los siguientes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>No se puede convertir Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Escenario

Cuando se implementa un paquete, verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Está intentando implementar desde el Administrador de IIS mediante la UI 1.1 implementar Web a un servidor que tiene instalado Web Deploy 2.0. Si está utilizando la herramienta de administración remota de IIS para implementar mediante la importación de un paquete, compruebe el **nuevas características disponibles** cuadro de diálogo al establecer la conexión. (Este cuadro de diálogo podría solo se muestra una vez cuando se establece la conexión. Para borrar la conexión y empezar de nuevo, cierre el Administrador de IIS y lo abra de nuevo mediante la especificación de `inetmgr /reset` en el símbolo del sistema.) Si una de las características en la lista es **Implementar interfaz de usuario Web**y tiene un número de versión inferior a 8, el servidor que se va a implementar en podría tener las versiones 1.1 y 2.0 de Web Deploy instalado. Para implementar desde un cliente que tenga instalado 2.0, el servidor debe tener sólo Web Deploy 2.0 instalado. Tendrá que ponerse en contacto con su proveedor de hospedaje para resolver este problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>No se puede cargar los componentes nativos de SQL Server Compact

### <a name="scenario"></a>Escenario

Cuando se ejecuta el sitio implementado, vea el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

No tiene el sitio implementado *amd64* y *x86* subcarpetas con los ensamblados nativos en ellas en la aplicación *bin* carpeta. En un equipo con SQL Server Compact instalado, los ensamblados nativos se encuentran en *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Es la mejor manera de obtener los archivos correctos en las carpetas correctas en un proyecto de Visual Studio instalar el paquete de NuGet SqlServerCompact. Instalación del paquete agrega un script posterior a la compilación para copiar los ensamblados nativos en *amd64* y *x86*. Sin embargo, en orden para que se implementará, deberás manualmente incluirlos en el proyecto. Para obtener más información, consulte el [implementar SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Error "Ruta de acceso no es válida" después de implementar una aplicación de Entity Framework Code First

### <a name="scenario"></a>Escenario

Implementar una aplicación que utiliza Entity Framework Code First Migrations y un DBMS, como SQL Server Compact que almacena la base de datos en un archivo en la aplicación\_carpeta de datos. Tener migraciones de Code First configurado para crear la base de datos tras la primera implementación. Al ejecutar la aplicación obtendrá un mensaje de error similar al ejemplo siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Código primero intenta crear la base de datos pero la aplicación\_carpeta de datos no existe. O bien no hay ningún archivo el *aplicación\_datos* carpeta cuando implementa o seleccionó **Excluir aplicación\_datos** en el **Empaquetar/Publicar Web** pestaña de la **propiedades del proyecto** ventana. El proceso de implementación no cree una carpeta en el servidor si no hay ningún archivo en la carpeta que se va a copiar en el servidor. Si ya ha configurado en el sitio de la base de datos, el proceso de implementación eliminará los archivos y la *aplicación\_datos* carpeta si seleccionó **quitar archivos adicionales en destino** en el perfil de publicación. Para solucionar el problema, coloque un archivo de marcador de posición, como un archivo .txt en el *aplicación\_datos* carpeta, asegúrese de que no tiene **Excluir aplicación\_datos** seleccionada y volver a implementar. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"No se puede usar objetos COM que se ha separado de su RCW subyacente".

### <a name="scenario"></a>Escenario

Ha estado correctamente con un solo clic publicar para implementar la aplicación y, a continuación, se empiece a obtener este error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

Cerrar y reiniciar Visual Studio normalmente es todo lo que necesita para resolver este error.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Implementación produce un error porque usuario utilizan credenciales para setACL de publicación no tiene autoridad

### <a name="scenario"></a>Escenario

Publicación se produce un error que indica no tiene autoridad para establecer los permisos de carpeta (la cuenta de usuario que está utilizando no tiene autoridad setACL).

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

De forma predeterminada, Visual Studio establece permisos de lectura en la carpeta raíz del sitio y permisos de escritura en la aplicación\_carpeta de datos. Si sabe que los permisos predeterminados de las carpetas del sitio son correctos y no deben establecerse, deshabilitar este comportamiento mediante la adición de **&lt;IncludeSetACLProviderOn destino&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** hasta el archivo de perfil de publicación (que afecta a un solo perfil) o al archivo wpp.targets (que afecta a todos los perfiles). Para obtener información sobre cómo editar estos archivos, consulte [Cómo: modificar la configuración de implementación en los archivos de perfil (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Errores de acceso denegado cuando la aplicación intenta escribir en una carpeta de la aplicación

### <a name="scenario"></a>Escenario

Los errores de aplicación cuando intenta crear o editar un archivo en una de las carpetas de aplicación, porque no tiene autoridad de escritura para esa carpeta.

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

De forma predeterminada, Visual Studio establece permisos de lectura en la carpeta raíz del sitio y permisos de escritura en la aplicación\_carpeta de datos. Si la aplicación necesita acceso de escritura a una subcarpeta, puede establecer permisos para esa carpeta, como se muestra en el [permisos de la carpeta de configuración](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) y [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutoriales. Si la aplicación necesita acceso de escritura a la carpeta raíz del sitio, tiene que evitar desde la configuración de acceso de solo lectura en la carpeta raíz mediante la adición de **&lt;IncludeSetACLProviderOn destino&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** hasta el archivo de perfil de publicación (que afecta a un solo perfil) o al archivo wpp.targets (que afecta a todos los perfiles). Para obtener información sobre cómo editar estos archivos, consulte [Cómo: modificar la configuración de implementación en los archivos de perfil (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Error de configuración: atributo targetFramework hace referencia a una versión posterior a la versión instalada de .NET Framework

### <a name="scenario"></a>Escenario

Se publicó correctamente un proyecto web cuyo destino es 4.5 de ASP.NET, pero al ejecutar la aplicación (con la `customErrors` modo establecido en "off" en el archivo Web.config) obtendrá el error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

El cuadro de Error de origen de la página de error resalta la línea siguiente del archivo Web.config como la causa del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Causa y solución posibles

El servidor no es compatible con ASP.NET 4.5. Póngase en contacto con el proveedor de hospedaje para determinar cuándo y si puede agregarse compatibilidad para ASP.NET 4.5. Si no es posible actualizar el servidor, se debe implementar un proyecto web destinado a ASP.NET 4 o versiones anteriores en su lugar. Si implementa un web proyecto ASP.NET 4 o versiones anteriores en el mismo destino, seleccione la **quitar archivos adicionales en destino** casilla de verificación en la **configuración** pestaña de la **Publicar Web**asistente. Si no selecciona **quitar archivos adicionales en destino**, se seguirá pudiendo obtener la página de Error de configuración.

El proyecto **propiedades** windows incluye una lista desplegable de plataforma de destino, pero no se puede resolver este problema, simplemente cambie de **.NET Framework 4.5** a **de.NETFramework4**. Si cambia la plataforma de destino a una versión anterior de framework, el proyecto seguirá teniendo referencias a ensamblados de la versión de framework posterior y no se ejecutará. Tendrá que cambiar esas referencias manualmente o crear un nuevo proyecto que tenga como destino .NET Framework 4 o versiones anterior. Para obtener más información, consulte [.NET Framework de destino para los sitios Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
