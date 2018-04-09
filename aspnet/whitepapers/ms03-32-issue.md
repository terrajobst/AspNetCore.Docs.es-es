---
uid: whitepapers/ms03-32-issue
title: Corrección de Error de "Aplicación de servidor no disponible" después de aplicar la actualización de seguridad de Internet Explorer | Documentos de Microsoft
author: rick-anderson
description: Este documento describe la revisión que corrige un problema con la actualización de seguridad MS03-32 para Internet Explorer que afecta a las aplicaciones de ASP.NET 1.0 que se ejecutan en Wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: dd1a564cd347364abc3ca5ac0a9ffda448bcede8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Corregir error de "Aplicación de servidor no disponible" después de aplicar la actualización de seguridad de Internet Explorer
====================
> Este documento describe la revisión que corrige un problema con la actualización de seguridad MS03-32 para Internet Explorer que afecta a las aplicaciones de ASP.NET 1.0 que se ejecutan en Windows XP Professional.
> 
> Se aplica a Windows XP Professional y ASP.NET 1.0.


Microsoft identificado un problema con la actualización de seguridad MS03-32 para la revisión de seguridad de Internet Explorer y 1.0 de ASP.NET que se ejecuta en Windows XP. Esta revisión puede instalarse manualmente o mediante la obtención de actualizaciones críticas recientes desde el sitio de Windows Update.

El síntoma de este problema es que después de instalar la revisión en un equipo con Windows XP, todas las solicitudes a aplicaciones de ASP.NET que se ejecutan en el servidor web local de IIS 5.1 como resultado un mensaje de error que dice "Aplicación de servidor no disponible". Las solicitudes a servidores web remotos no se ven afectadas.

Este problema solo afecta a las instalaciones que ejecutan ASP.NET 1.0 en Windows XP. No afecta a equipos que ejecutan Windows 2000 o Windows Server 2003. No afecta los equipos que ejecutan Windows XP con instalado ASP.NET 1.1.

Tenga en cuenta que este problema **no es** un error de seguridad con ASP.NET. Se **no** se abrirán o permitir que los ataques malintencionados en una aplicación ASP.NET o un servidor. En su lugar, es simplemente un error funcional causado por la revisión de seguridad.

Estamos trabajando duro en una solución definitiva para este problema. Mientras tanto, puede ejecutar el archivo por lotes siguiente para solucionar el problema. El archivo por lotes hace lo siguiente:

1. Detiene los servicios de estado IIS y ASP.NET
2. Elimina y vuelve a crear la cuenta ASPNET con una contraseña conocida temporal
3. Utiliza las ventanas `runas` comando para iniciar un archivo ejecutable que se crea un perfil de usuario ASPNET
4. Vuelve a registrar ASP.NET. Esto crea una nueva contraseña aleatoria para la cuenta y aplica la configuración predeterminada de control de acceso ASP.NET para él
5. Reinicia el servicio IIS

El archivo por lotes contiene una contraseña temporal codificado de forma rígida de "<strong>1pass@word</strong>" que le solicita que especifique para la ejecución de comandos cuando se ejecuta el archivo por lotes. Una vez completado el comando de ejecución, se vuelve a crear la contraseña de la cuenta ASPNET con un valor aleatorio seguro. Tenga en cuenta que el archivo por lotes puede producir un error si la contraseña codificados de forma rígida no cumple los requisitos de complejidad de contraseña en su entorno. Si es así, se puede cambiar a otro valor que sea adecuado para su entorno.

*> [!IMPORTANT]* Si se han agregado valores de configuración de control de acceso personalizado o permisos de cuenta de base de datos para la cuenta ASPNET, deben volver a crearse una vez completada de este archivo por lotes. Esto es porque cuando se vuelve a crear la cuenta, obtendrá un nuevo identificador de seguridad (SID).

*> [!IMPORTANT]* Si está ejecutando el proceso de trabajo ASP.NET con una cuenta personalizada que no sea la cuenta ASPNET, no debe ejecutar este archivo por lotes. En su lugar, debe iniciar sesión forma interactiva o utilice el comando runas con esa cuenta a la que se creará un perfil de usuario para esa cuenta.

El archivo por lotes se incluye en el archivo autoextraíble siguiente. Uso:

1. Debe ejecutar como una cuenta con privilegios de administrador
2. [Descargue y abra el archivo ejecutable autoextraíble](ms03-32-issue/_static/fixup1.exe)
3. Extraiga el contenido en c:\
4. Seleccione Ejecutar... en el menú Inicio y escriba `cmd.exe`
5. En las ventanas de comando Abrir, escriba `c:\fixup.cmd`.
6. Cuando se le solicite, escriba <strong>1pass@word</strong> como la contraseña.
7. Si tiene valores de configuración de control de acceso personalizado anteriormente o permisos de cuenta de base de datos para la cuenta ASPNET, debe volver a aplicar esta configuración ahora.

Disculpas por las molestias que esto ha provocado que muchos como medida. Publicaremos información adicional cuando se encuentre disponible.

La tabla siguiente detalla las plataformas y versiones afectadas por este problema.

| .NET Framework | Plataforma | Afectados |
| --- | --- | --- |
| Versión 1.0 | Windows 2000 Professional | No |
| Versión 1.0 | Windows 2000 Server | No |
| Versión 1.0 | Windows XP Professional | Sí |
| Versión 1.0 | Windows Server 2003 | No |
| Versión 1.0 | Windows XP Home Edition con Cassini | No |
| Versión 1.1 | Windows 2000 Professional | No |
| Versión 1.1 | Windows 2000 Server | No |
| Versión 1.1 | Windows XP Professional | No |
| Versión 1.1 | Windows Server 2003 | No |
| Versión 1.1 | Windows XP Home Edition con Cassini | No |

Gracias,   
 El equipo de ASP.NET
