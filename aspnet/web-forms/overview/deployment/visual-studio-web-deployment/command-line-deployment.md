---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: "Implementación de Web ASP.NET con Visual Studio: implementación de línea de comandos | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8446b3fc05e3ef4a5a30c753c989252fd7f1a56f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Implementación de Web ASP.NET con Visual Studio: implementación de línea de comandos
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo invocar la web de Visual Studio publicar canalización desde la línea de comandos. Esto es útil para escenarios donde desea [automatizar el proceso de implementación](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) en lugar de hacerlo manualmente en Visual Studio, normalmente mediante un [sistema de control de versiones de código de origen](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Realiza un cambio para implementar

Actualmente, la página acerca muestra el código de plantilla.

![Acerca de la página con el código de plantilla](command-line-deployment/_static/image1.png)

Va a reemplazar con código que muestra un resumen de la inscripción de estudiantes.

Abra la *About.aspx* página, elimine todas las revisiones dentro de la `MainContent` `Content` elemento e inserte el siguiente marcado en su lugar:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Ejecute el proyecto y seleccione el **sobre** página.

![Acerca de la página](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Implementar en prueba mediante la línea de comandos

No puede implementar otro cambio de base de datos, por lo que deshabilitar dbDacFx base de datos implementación para la base de datos de aspnet ContosoUniversity. Abra el **publicación Web** asistente y en cada uno de los tres perfiles, desactive de publicación la **Actualizar base de datos** casilla de verificación en la **configuración** ficha.

En la página de inicio de Windows 8, busque **símbolo del sistema para desarrolladores de VS2012**.

Haga clic en el icono de **símbolo del sistema para desarrolladores de VS2012** y haga clic en **ejecutar como administrador**.

Escriba el siguiente comando en el símbolo del sistema, reemplace la ruta de acceso al archivo de solución con la ruta de acceso al archivo de solución:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild genera la solución e implementa en el entorno de prueba.

![Salida de la línea de comandos](command-line-deployment/_static/image3.png)

Abra un explorador y vaya a `http://localhost/ContosoUniversity`, a continuación, haga clic en el **sobre** página para comprobar que la implementación se realizó correctamente.

Si no ha creado ningún alumno en pruebas, verá una página vacía en el **estadísticas de cuerpo de estudiante** encabezado. Vaya a la **estudiantes** página, haga clic en **agregar alumno**y agregue algunos alumnos y, a continuación, volver a la **sobre** página para ver las estadísticas de estudiante.

![Acerca de la página en el entorno de prueba](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opciones de línea de comandos de clave

El comando que escribió pasa la ruta de acceso del archivo de solución y dos propiedades de MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Implementación de la solución en comparación con la implementación de proyectos individuales

Especificar el archivo de solución hace que todos los proyectos de la solución que se crea. Si tiene varios proyectos web de la solución, se aplica el comportamiento de MSBuild siguiente:

- Las propiedades que especifique en la línea de comandos se pasan a todos los proyectos. Por lo tanto, cada proyecto web debe tener un perfil de publicación con el nombre que especifique. Si especifica `/p:PublishProfile=Test`, cada proyecto web debe tener un perfil de publicación con el nombre *prueba*.
- Puede publicar correctamente un proyecto cuando otro incluso no se compila. Para obtener más información, vea el subproceso de stackoverflow [se produce un error de MSBuild con dos paquetes](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Si especifica un proyecto individual en lugar de una solución, tiene que agregar un parámetro que especifica la versión de Visual Studio. Si usa Visual Studio 2012 la línea de comandos sería similar al ejemplo siguiente:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

El número de versión para Visual Studio 2010 es 10.0. Para obtener más información, consulte [compatibilidad del proyecto de Visual Studio y VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) en el blog de Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Especificar el perfil de publicación

Puede especificar el perfil de publicación por nombre o por la ruta de acceso completa a la *.pubxml* de archivos, como se muestra en el ejemplo siguiente:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Métodos admitidos para la publicación en línea de comandos de publicación de Web

Tres métodos de publicación se admiten para la publicación de la línea de comandos:

- `MSDeploy`-Publicar mediante Web Deploy.
- `Package`-Publicar mediante la creación de un paquete de Web Deploy. Tendrá que instalar el paquete por separado desde el comando de MSBuild que lo crea.
- `FileSystem`-Publicar copiando archivos a una carpeta especificada.

### <a name="specifying-the-build-configuration-and-platform"></a>Especificar la configuración de compilación y plataforma

La configuración de compilación y la plataforma deben establecerse en Visual Studio o en la línea de comandos. Los perfiles de publicación incluyen propiedades que se denominan `LastUsedBuildConfiguration` y `LastUsedPlatform`, pero no se puede establecer estas propiedades para determinar cómo se compila el proyecto. Para obtener más información, consulte [MSBuild: cómo establecer la propiedad de configuración](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) en el blog de Sayed Hashimi.

## <a name="deploy-to-staging"></a>Implementar en almacenamiento provisional

Para implementar en Azure, debe agregar la contraseña a la línea de comandos. Si guarda la contraseña en el perfil de publicación en Visual Studio, que se guardó en un formato cifrado en la su *. pubxml.user* archivo. Ese archivo no tiene acceso a MSBuild cuando realice una implementación de línea de comandos, por lo que debe pasar la contraseña en un parámetro de línea de comandos.

1. Copie la contraseña que necesita en el *.publishsettings* archivo que descargó anteriormente para el sitio web de ensayo. La contraseña es el valor de la `userPWD` atributo para la Web Deploy `publishProfile` elemento.

    ![Contraseña de Web Deploy](command-line-deployment/_static/image5.png)
2. En la página de inicio de Windows 8, busque **símbolo del sistema para desarrolladores de VS2012**y haga clic en el icono para abrir el símbolo del sistema. (No es necesario que abrirlo como administrador en este momento porque no se implemente en IIS en el equipo local.)
3. Escriba el siguiente comando en el símbolo del sistema, reemplace la ruta de acceso al archivo de solución con la ruta de acceso al archivo de solución y la contraseña con la contraseña:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Observe que esta línea de comandos incluye un parámetro adicional: `/p:AllowUntrustedCertificate=true`. Como este tutorial se está escribiendo, el `AllowUntrustedCertificate` se debe establecer la propiedad cuando se publica en Azure desde la línea de comandos. Cuando se publique la solución para este error, no necesitará ese parámetro.
4. Abra un explorador y vaya a la dirección URL de su sitio de ensayo y, a continuación, haga clic en el **sobre** página para comprobar que la implementación se realizó correctamente.

    Como se vio anteriormente para el entorno de prueba, es posible que deba crear algunos alumnos para ver las estadísticas de la **sobre** página.

## <a name="deploy-to-production"></a>Implementar en producción

El proceso para la implementación en producción es similar al proceso de ensayo.

1. Copie la contraseña que necesita en el *.publishsettings* archivo que descargó anteriormente para el sitio web de producción.
2. Abra **símbolo del sistema para desarrolladores de VS2012**.
3. Escriba el siguiente comando en el símbolo del sistema, reemplace la ruta de acceso al archivo de solución con la ruta de acceso al archivo de solución y la contraseña con la contraseña:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Para un sitio de producción real, si ha habido también un cambio de base de datos, que normalmente copiaría la *aplicación\_offline.htm* de archivos en el sitio antes de la implementación y eliminar después de la implementación correcta.
4. Abra un explorador y vaya a la dirección URL de su sitio de ensayo y, a continuación, haga clic en el **sobre** página para comprobar que la implementación se realizó correctamente.

## <a name="summary"></a>Resumen

Ahora ha implementado una actualización de la aplicación mediante la línea de comandos.

![Acerca de la página en el entorno de prueba](command-line-deployment/_static/image6.png)

En el siguiente tutorial, verá un ejemplo de cómo extender la web canalización de publicación. En el ejemplo se le mostrará cómo implementar archivos que no están incluidos en el proyecto.

>[!div class="step-by-step"]
[Anterior](deploying-a-database-update.md)
[Siguiente](deploying-extra-files.md)
