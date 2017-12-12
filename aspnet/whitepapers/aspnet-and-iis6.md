---
uid: whitepapers/aspnet-and-iis6
title: Ejecuta ASP.NET 1.1 con IIS 6.0 | Documentos de Microsoft
author: rick-anderson
description: "Mientras que Windows Server 2003 incluye IIS 6.0 y ASP.NET 1.1, estos componentes están deshabilitados de forma predeterminada. Estas notas del producto describen cómo habilitar IIS 6.0 un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a>Ejecuta ASP.NET 1.1 con IIS 6.0
====================
> Mientras que Windows Server 2003 incluye IIS 6.0 y ASP.NET 1.1, estos componentes están deshabilitados de forma predeterminada. Estas notas del producto describen cómo habilitar IIS 6.0 y ASP.NET 1.1 y recomienda diversas opciones de configuración para obtener un rendimiento óptimo de IIS y ASP.NET.
> 
> Se aplica a IIS 6.0 y ASP.NET 1.1.


ASP.NET 1.1 se incluye con Windows Server 2003, que también incluye la versión más reciente de Internet Information Server (IIS) versión 6.0. IIS 6.0 y ASP.NET 1.1 están diseñados para integrarse a la perfección y ASP.NET ahora tiene como valor predeterminado para el nuevo modelo de proceso de trabajo de IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1.1 no está instalado de forma predeterminada

A diferencia de versiones anteriores de sistemas operativos de servidor de Microsoft, Internet Information Server (IIS) no está habilitado de forma predeterminada; ni es ASP.NET 1.1. Hay dos opciones para habilitar IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Habilitar el Asistente para configurar el servidor IIS, opción #1:

Windows Server 2003 se incluye un nuevo 'servidor Asistente para configurar su' para ayudarle a configurar correctamente el servidor en el modo deseado.

Para iniciar el asistente: tenga en cuenta que para ejecutar el asistente debe haber iniciado sesión como administrador: vaya a: inicio | Programas | Herramientas administrativas y seleccione 'configurar el servidor'.

Una vez seleccionado, verá la pantalla de apertura 'Configurar Asistente para el servidor':

![](aspnet-and-iis6/_static/image1.jpg)

Haga clic en ' siguiente &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Haga clic en ' siguiente &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

En esta pantalla, deberá seleccionar ' servidor de aplicaciones (IIS, ASP.NET) como las opciones para configurar.

Haga clic en ' siguiente &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Después de seleccionar esta opción para configurar el servidor como un servidor de aplicaciones, se mostrará esta pantalla preguntar qué capacidades adicionales que deben instalarse. Ninguna opción está activada de forma predeterminada. Para habilitar ASP.NET automáticamente, debe seleccionar ' habilitar ASP. NET'.

Haga clic en ' siguiente &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Esta pantalla muestra cuáles son opciones para instalarse.

Haga clic en ' siguiente &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Verá esta pantalla mientras se están instalando las opciones seleccionadas. Es normal que se ve otro cuadro de diálogo aparecen los cuadros que se están instalando los servicios. Además se le pedirá la ubicación del CD de instalación de Windows Server 2003.

Haga clic en ' siguiente &gt;' cuando haya terminado.

![](aspnet-and-iis6/_static/image7.jpg)

Haga clic en 'Finalizar': Windows Server 2003 ahora está configurado para admitir IIS 6.0 y ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Habilitar IIS, opción #2 - Configuración manual de IIS y ASP.NET

Si no desea usar al 'configurar Asistente para el servidor', opcionalmente, puede instalar IIS 6.0 y ASP.NET 1.1 mediante "Agregar o quitar programas" del Panel de Control.

En primer lugar, abra el Panel de Control:

![](aspnet-and-iis6/_static/image8.jpg)

A continuación, haga clic en 'Agregar o quitar componentes de Windows' que se abrirá al "Asistente de componentes de Windows":

![](aspnet-and-iis6/_static/image9.jpg)

Resaltar y 'Servidor de aplicaciones' y, a continuación, haga clic en 'Detalles'? botón:

![](aspnet-and-iis6/_static/image10.jpg)

Para instalar ASP.NET, compruebe ' ASP. NET'.

Haga clic en 'Aceptar' para volver al Asistente para componentes de Windows. Haga clic en ' siguiente &gt;' desde el Asistente para componentes de Windows para iniciar la instalación:

![](aspnet-and-iis6/_static/image11.jpg)

Es normal que se ve otro cuadro de diálogo aparecen los cuadros que se están instalando los servicios. Además se le pedirá la ubicación del CD de instalación de Windows Server 2003.

Cuando se completa la instalación verá la última pantalla del Asistente para componentes de Windows:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 y ASP.NET 1.1 ahora están configurados y disponibles.

## <a name="recommended-settings"></a>Configuración recomendada

Cuando se ejecuta ASP.NET 1.1 con IIS 6.0, hay varias opciones de configuración que se recomiendan para obtener un rendimiento óptimo de ASP.NET:

- Configurar límites de memoria de proceso de trabajo
- Configuración de reciclaje de procesos de trabajo

### <a name="configuring-worker-process-memory-limits"></a>Configurar límites de memoria de proceso de trabajo

De forma predeterminada IIS 6.0 no establece un límite en la cantidad de memoria que IIS se puede usar. ASP. Característica de la memoria caché de la red se basa en una limitación de memoria para que la caché de forma proactiva pueda quitar elementos no utilizados de la memoria.

Se recomienda que configure la característica de IIS 6.0 de reciclaje de memoria. Para configurar este abra Administrador de Internet Information Services (inicio | Programas | Herramientas administrativas | Internet Information Services). Una vez abierto, expanda la carpeta de 'Grupos de aplicaciones':

Para cada grupo de aplicaciones:

![](aspnet-and-iis6/_static/image13.jpg)

1. Haga doble clic en el grupo de aplicaciones, p. ej. 'DefaultAppPool' y seleccione 'propiedades':

![](aspnet-and-iis6/_static/image14.jpg)

2. A continuación, habilite el reciclaje de memoria, haga clic en cualquiera ' máximo de memoria usada (en megabytes):'. El valor no debe ser mayor que la cantidad de memoria (no virtual) física en el servidor, una buena aproximación es del 60% de la memoria física, es decir, para un servidor con 512MB de memoria física, seleccione 310. También se recomienda que el valor máximo no supera 800MB cuando se usa un espacio de direcciones de 2GB. Si el espacio de direcciones de memoria del servidor es / 3GB, el límite de memoria máxima para el proceso de trabajo puede ser tan alto como 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Haga clic en 'Aplicar' y 'Aceptar' para salir del cuadro de diálogo de propiedades. Repita este paso para todos los grupos de aplicaciones disponibles.

### <a name="configuring-worker-recycling"></a>Configuración de reciclaje de trabajo

De forma predeterminada, IIS 6.0 está configurado para reciclar el proceso de trabajo cada 29 horas. Esto es un poco rigurosa para una aplicación que se ejecuta ASP.NET y se recomienda que el reciclaje de procesos de trabajo automática está deshabilitado.

Para deshabilitar el reciclaje de procesos de trabajo automática, primero abra el Administrador de Internet Information Services (inicio | Programas | Herramientas administrativas | Internet Information Services). Una vez abierto, expanda la carpeta de 'Grupos de aplicaciones':

![](aspnet-and-iis6/_static/image16.jpg)

Para cada grupo de aplicaciones:

1. Haga doble clic en el grupo de aplicaciones, p. ej. 'DefaultAppPool' y seleccione 'propiedades':

![](aspnet-and-iis6/_static/image17.jpg)

2. Desactive la opción ' reciclar el proceso de trabajo (en minutos):':

![](aspnet-and-iis6/_static/image18.jpg)

Haga clic en 'Aplicar' y 'Aceptar' para salir del cuadro de diálogo de propiedades. Repita este paso para todos los grupos de aplicaciones disponibles.

## <a name="granting-write-access-to-the-file-system"></a>Conceder acceso de escritura al sistema de archivos

Si su aplicación necesita acceso de escritura al sistema de archivos y usas NTFS debe modificar un Control de acceso lista (ACL) en el archivo o carpeta para conceder acceso ASP.NET a.

Por ejemplo, para conceder a ASP.NET acceso de escritura a la c:\inetpub\wwwroot primero abra el explorador y navegue al directorio:

![](aspnet-and-iis6/_static/image19.jpg)

A continuación, haga doble clic en el directorio, por ejemplo, 'wwwroot' y seleccione Propiedades. Después de abre el cuadro de diálogo de propiedades, seleccione la ficha 'Seguridad':

![](aspnet-and-iis6/_static/image20.jpg)

El directorio c:\inetpub\wwwroot\ es un directorio especial en el que el grupo de IIS 6.0 especial ' IIS\_WPG' ya se ha concedido lectura &amp; permisos de ejecución, mostrar contenido de carpetas y lectura. Sin embargo, para conceder permiso de escritura, debe hacer clic en la casilla de verificación Permitir para escritura:

![](aspnet-and-iis6/_static/image21.jpg)

Ahora, IIS 6.0 tiene permiso de escritura en esta carpeta. Para conceder permisos de escritura en otras carpetas, siga estos pasos: tenga en cuenta que puede que necesite agregar el IIS\_grupo WPG si aún no existe.

> [!CAUTION]
> Conceder el permiso de escritura en IIS\_WPG permitirá que cualquier aplicación de ASP.NET escribir en este directorio.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Compatibilidad con la autenticación integrada con SQL Server

Permite la autenticación integrada de SQL Server para aprovechar la autenticación de Windows NT para validar las cuentas de inicio de sesión de SQL Server. Esto permite al usuario omitir el proceso de inicio de sesión estándar de SQL Server. Con este enfoque, un usuario de red puede tener acceso a una base de datos de SQL Server sin proporcionar una identificación de inicio de sesión independiente o la contraseña porque SQL Server obtiene la información de contraseña y el usuario desde el proceso de seguridad de red de Windows NT.

Elegir la autenticación integrada para las aplicaciones ASP.NET es una buena elección porque ninguna credencial nunca se almacena en la cadena de conexión para la aplicación. En su lugar, la cadena de conexión utilizada para conectarse a SQL será como se indica a continuación:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Esta cadena de conexión indica a SQL Server para usar las credenciales de Windows de la aplicación intenta tener acceso a SQL Server. En el caso de ASP.NET/IIS 6 sería una cuenta en IIS\_grupo WPG.

Para habilitar la autenticación integrada entre SQL Server y ASP.NET, debe asegurarse primero de que SQL Server está configurado para la autenticación integrada o autenticación de modo mixto: póngase en contacto con su DBA para averiguarlo. Si SQL Server está en uno de estos dos modos, puede usar la autenticación integrada.

Abra el Administrador corporativo SQL Server (iniciar | Programas | Microsoft SQL Server | El Administrador corporativo), seleccione el servidor adecuado y expanda la carpeta seguridad:

![](aspnet-and-iis6/_static/image22.jpg)

Si ' BUILTINT\IIS\_WPG' grupo no aparece, haga doble clic en inicios de sesión y seleccione 'Nuevo inicio de sesión':

![](aspnet-and-iis6/_static/image23.jpg)

En el ' nombre:' cuadro de texto escriba ' [nombre de servidor o dominio] \IIS\_WPG' o haga clic en el botón de puntos suspensivos para abrir el selector de usuario o grupo de Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Seleccione IIS la máquina actual\_grupo WPG y haga clic en 'Agregar' y en Aceptar para cerrar el selector.

A continuación, debe establecer también la base de datos de forma predeterminada y los permisos para tener acceso a la base de datos. Para establecer la base de datos predeterminada elija en la lista desplegable está seleccionada, por ejemplo, Northwind siguiente:

![](aspnet-and-iis6/_static/image25.jpg)

A continuación, haga clic en la ficha acceso de base de datos:

![](aspnet-and-iis6/_static/image26.jpg)

Haga clic en la casilla de verificación Permitir para cada base de datos que se va a permitir el acceso a. También necesitará seleccionar roles de base de datos, la comprobación de base de datos\_propietario garantizará su inicio de sesión tiene todos los permisos necesarios para administrar y usar la base de datos seleccionada.

Haga clic en Aceptar para salir del cuadro de diálogo de propiedades. La aplicación de ASP.NET ya está configurada para admitir la autenticación integrada de SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>No ejecute ASP.NET 1.0 en modo nativo de IIS 6.0

ASP.NET 1.0 en IIS 6.0 solo se admite en modo de compatibilidad de IIS 5.

Para configurar ASP.NET 1.0 para que se ejecute en modo de compatibilidad de IIS 5.0, abra el Administrador de servicios de Internet y haga clic con sitios Web y seleccione Propiedades:

![](aspnet-and-iis6/_static/image27.jpg)

Cambie a la pestaña del servicio y compruebe? Ejecutar el servicio WWW en el modo de aislamiento 5.0 de IIS?:

![](aspnet-and-iis6/_static/image28.jpg)
