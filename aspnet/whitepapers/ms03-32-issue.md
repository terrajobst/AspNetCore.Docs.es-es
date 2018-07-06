---
uid: whitepapers/ms03-32-issue
title: Corrección para el Error de "Aplicación de servidor no disponible" después de aplicar la actualización de seguridad para Internet Explorer | Microsoft Docs
author: rick-anderson
description: Este documento describe la revisión que corrige un problema con la actualización de seguridad MS03-32 para Internet Explorer que afecta a las aplicaciones de ASP.NET 1.0 que se ejecutan en Wi...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 1a289379229335a9841dec48e577c19173419891
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836728"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Corrección para el Error de "Aplicación de servidor no disponible" después de aplicar la actualización de seguridad para Internet Explorer
====================
> Este documento describe la revisión que corrige un problema con la actualización de seguridad MS03-32 para Internet Explorer que afecta a las aplicaciones de ASP.NET 1.0 que se ejecutan en Windows XP Professional.
> 
> Se aplica a ASP.NET 1.0 y Windows XP Professional.


Microsoft identificado un problema con la actualización de seguridad MS03-32 de revisión de seguridad de Internet Explorer y ASP.NET 1.0, que se ejecutan en Windows XP. Esta revisión puede instalarse manualmente o mediante la obtención de actualizaciones críticas recientes desde el sitio Windows Update.

El síntoma de este problema es que después de instalar la revisión en un equipo con Windows XP, todas las solicitudes a las aplicaciones ASP.NET que se ejecutan en el servidor web local de IIS 5.1 como resultado un mensaje de error que dice "Aplicación de servidor no disponible". No se ven afectadas las solicitudes a servidores web remotos.

Este problema solo afecta a las instalaciones que se ejecutan ASP.NET 1.0 en Windows XP. No afecta a las máquinas que ejecutan Windows 2000 o Windows Server 2003. No afecta las máquinas que ejecutan Windows XP con ASP.NET 1.1 instalado.

Tenga en cuenta que este problema **no** un error de seguridad con ASP.NET. Lo **no** abrirá o permitir que los ataques contra un servidor o una aplicación ASP.NET. En su lugar, es puramente un error funcional causado por la revisión de seguridad.

Estamos trabajando duro en una solución definitiva para resolver este problema. Mientras tanto, puede ejecutar el siguiente archivo por lotes para solucionar el problema. El archivo por lotes hace lo siguiente:

1. Detiene los servicios de estado IIS y ASP.NET
2. Elimina y vuelve a crear la cuenta ASPNET con una contraseña conocida temporal
3. Usa el Windows `runas` comando para iniciar un archivo ejecutable que se crea un perfil de usuario ASPNET
4. Vuelve a registrar ASP.NET. Esto crea una nueva contraseña aleatoria para la cuenta y aplica la configuración predeterminada de control de acceso ASP.NET para él
5. Reinicia el servicio IIS

El archivo por lotes contiene una contraseña temporal codificado de forma rígida de "<strong>1pass@word</strong>" que le pedirá que escriba para la ejecución de comandos cuando se ejecuta el archivo por lotes. Después de que se complete el comando "runas", se vuelve a crear la contraseña de la cuenta ASPNET con un valor aleatorio seguro. Tenga en cuenta que el archivo por lotes puede producir un error si la contraseña codificada no cumple los requisitos de complejidad de contraseña en su entorno. Si es así, puede cambiarlo a otro valor que sea adecuado para su entorno.

*> [!IMPORTANT]* Si ha agregado la configuración de control de acceso personalizados o los permisos de cuenta de base de datos para la cuenta ASPNET, necesitará volver a crear después de este archivo por lotes. Esto es porque cuando se vuelve a crear la cuenta, obtendrá un nuevo identificador de seguridad (SID).

*> [!IMPORTANT]* Si está ejecutando el proceso de trabajo ASP.NET con una cuenta distinta de la cuenta ASPNET personalizada, no debe ejecutar este archivo por lotes. En su lugar, debe iniciar sesión interactivamente o utilizar el comando runas con esa cuenta a la que se creará un perfil de usuario para esa cuenta.

El archivo por lotes se incluye en el archivo autoextraíble siguiente. Para usarla:

1. Debe ejecutar como una cuenta con privilegios de administrador
2. [Descargue y abra el archivo ejecutable autoextraíble](ms03-32-issue/_static/fixup1.exe)
3. Extraiga el contenido en c:\
4. Seleccione Ejecutar... en el menú Inicio y escriba `cmd.exe`
5. En las ventanas de comandos abierto, escriba `c:\fixup.cmd`.
6. Cuando se le solicite, escriba <strong>1pass@word</strong> como contraseña.
7. Si tiene permisos de cuenta de base de datos para la cuenta ASPNET o de configuración de control de acceso anteriormente personalizado, deberá volver a aplicar esta configuración ahora.

Mis disculpas muchas las molestias que esto ha causado. Publicaremos información adicional cuando se encuentre disponible.

La tabla siguiente detalla las plataformas y las versiones afectadas por este problema.

| .NET Framework | Plataforma | Afectados |
| --- | --- | --- |
| Versión 1.0 | Windows 2000 Professional | No |
| Versión 1.0 | Windows 2000 Server | No |
| Versión 1.0 | Windows XP Professional | Sí |
| Versión 1.0 | Windows Server 2003 | No |
| Versión 1.0 | Windows XP Home con Cassini | No |
| Versión 1.1 | Windows 2000 Professional | No |
| Versión 1.1 | Windows 2000 Server | No |
| Versión 1.1 | Windows XP Professional | No |
| Versión 1.1 | Windows Server 2003 | No |
| Versión 1.1 | Windows XP Home con Cassini | No |

Gracias,   
 El equipo de ASP.NET
