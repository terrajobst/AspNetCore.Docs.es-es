---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migración de datos de proveedor Universal para la suscripción y los perfiles de usuario a ASP.NET Identity (C#) | Microsoft Docs
author: rustd
description: Este tutorial describen los pasos que son necesarios para migrar usuarios y datos de las funciones y los datos de perfil de usuario creados con proveedores universales de una aplicación existente...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 6115b2a6caca05659f1c35ce97954807a6fb01ae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838285"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrar datos de proveedor Universal para la suscripción y los perfiles de usuario a ASP.NET Identity (C#)
====================
por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> En este tutorial se describe los pasos que son necesarios para migrar usuarios y datos de las funciones y los datos de perfil de usuario creados con proveedores universales de una aplicación existente para el modelo de identidad de ASP.NET. El enfoque mencionado aquí en una aplicación con la pertenencia SQL también se puede usar migrar datos de perfil de usuario.


Con el lanzamiento de Visual Studio 2013, el equipo de ASP.NET introdujo un nuevo sistema ASP.NET Identity, y puede leer más acerca de esa versión [aquí](../../index.md). En el artículo para migrar aplicaciones web de [pertenencia de SQL para el nuevo sistema de identidad](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), en este artículo se muestra los pasos para migrar aplicaciones existentes que siguen el modelo de proveedores para la administración de usuarios y roles el nuevo modelo de identidad. El objetivo de este tutorial será principalmente sobre cómo migrar los datos de perfil de usuario para enlazarlo a la perfección en el nuevo sistema. Migración de información de usuario y el rol es similar para la pertenencia SQL. El enfoque siguió para migrar datos de perfil se puede usar en una aplicación con la pertenencia SQL también.

Por ejemplo, empezaremos con una aplicación web creada con Visual Studio 2012 que usa el modelo de proveedores. Se podrá, a continuación, agregue código para la administración de perfiles, registrar un usuario, agregar datos de perfil para los usuarios, migrar el esquema de base de datos y, a continuación, cambie la aplicación para usar el sistema de identidad para la administración de usuarios y roles. Como prueba de la migración, los usuarios creados con proveedores universales deben podrán iniciar sesión y nuevos usuarios deben ser capaz de registrar.

> [!NOTE]
> Puede encontrar el ejemplo completo en [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Resumen de migración de datos de perfil

Antes de comenzar con las migraciones, echemos un vistazo a la experiencia de almacenamiento de datos de perfil en el modelo de proveedores. Datos de perfil de aplicación a los usuarios se pueden almacenar en varias formas, entre ellos los más comunes que se va a usando los proveedores de perfiles integradas que se incluye con los proveedores universales. Incluyen los pasos

1. Agregue una clase que tiene propiedades que se usan para almacenar datos de perfil.
2. Agregue una clase que extiende 'ProfileBase' e implementa métodos para obtener los datos del perfil anterior para el usuario.
3. Habilitar el uso de proveedores de perfiles de forma predeterminada en el *web.config* de archivos y definir la clase declarada en el paso 2 de # que se usará al obtener acceso a la información de perfil.

La información de perfil se almacena como datos binarios en la tabla "Perfiles" en la base de datos y xml serializado.

Después de migrar la aplicación para usar el nuevo sistema ASP.NET Identity, la información de perfil se deserializa y se almacenan como propiedades en la clase de usuario. Cada propiedad, a continuación, se puede asignar a columnas de la tabla de usuario. Aquí la ventaja es que las propiedades pueden trabajar directamente con la clase de usuario además de no tener que serializar/deserializar la información de datos cada vez cuando se obtiene acceso.

## <a name="getting-started"></a>Introducción

1. Cree una nueva aplicación de ASP.NET 4.5 Web Forms en Visual Studio 2012. El ejemplo actual utiliza la plantilla de formularios Web Forms, pero también podría usar aplicación MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Cree una nueva carpeta 'Modelos' para almacenar información de perfil  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Por ejemplo, nos gustaría almacenar la fecha de nacimiento, ciudad, alto y el peso del usuario en perfil. El alto y el peso se almacenan como una clase personalizada denominada 'PersonalStats'. Para almacenar y recuperar el perfil, se necesita una clase que extiende 'ProfileBase'. Vamos a crear una nueva clase 'AppProfile' para obtener y almacenar información de perfil.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Habilitar el perfil en el *web.config* archivo. Escriba el nombre de clase que se usará para almacenar o recuperar información de usuario creada en el paso 3 de #.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Agregar una página de formularios web en la carpeta 'Account' para obtener los datos de perfil del usuario y almacenarlos. Haga clic con el botón derecho en el proyecto y seleccione "Agregar nuevo elemento". Agregue una nueva página de formularios Web Forms con página maestra 'AddProfileData.aspx'. Copie lo siguiente en la sección "MainContent":

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Agregue el código siguiente en el código subyacente:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Agregue el espacio de nombres bajo qué AppProfile se define la clase para quitar los errores de compilación.
6. Ejecute la aplicación y crear un nuevo usuario con el nombre de usuario '**olduser'.** Vaya a la página 'AddProfileData' y agregue la información de perfil del usuario.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Puede comprobar que los datos se almacenan como xml serializado en la tabla "Perfiles" mediante la ventana del explorador de servidores. En Visual Studio, desde el menú "Ver", elija 'Explorador de servidores'. Debe haber una conexión de datos para la base de datos definido en el *web.config* archivo. Al hacer clic en la conexión de datos muestra diferentes subcategorías. Expanda "Tablas" para mostrar las diferentes tablas en la base de datos, a continuación, haga clic con el botón derecho en "Perfiles" y elija "Mostrar datos de tabla' para ver los datos de perfil almacenados en la tabla de perfiles.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrar el esquema de base de datos

Para que la base de datos existente que funcione con el sistema de identidad, es necesario actualizar el esquema en la base de datos de identidad para admitir los campos que se agregan a la base de datos original. Esto puede hacerse mediante secuencias de comandos SQL para crear nuevas tablas y copiar la información existente. En la ventana 'Explorador de servidores de ', expanda el 'DefaultConnection' para mostrar las tablas. A la derecha, haga clic en las tablas y seleccione "Nueva consulta"

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Pegue el script SQL de [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) y ejecútelo. Si se actualiza el 'DefaultConnection', podemos ver que se agregan las nuevas tablas. Puede comprobar los datos dentro de las tablas para ver que la información se ha migrado.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrar la aplicación para usar ASP.NET Identity

1. Instale los paquetes de Nuget necesarios para la identidad de ASP.NET:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Puede encontrar más información acerca de cómo administrar paquetes de Nuget [aquí](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Para trabajar con datos existentes en la tabla, necesitamos crear clases de modelo que se asignan a las tablas y enlazar en el sistema de identidad. Como parte del contrato de identidad, las clases del modelo deben implementar las interfaces definidas en el archivo dll de Identity.Core o pueden extender la implementación existente de estas interfaces disponibles en Microsoft.AspNet.Identity.EntityFramework. Vamos a usar las clases existentes para el rol, los inicios de sesión de usuario y notificaciones de usuario. Debemos usar un usuario personalizado para nuestro ejemplo. Haga clic con el botón derecho en el proyecto y crear nueva carpeta 'IdentityModels'. Agregue una nueva clase de "User", tal como se muestra a continuación:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Tenga en cuenta que la 'ProfileInfo' ahora es una propiedad en la clase de usuario. Por lo tanto, podemos usar la clase de usuario para trabajar directamente con datos de perfil.

Copie los archivos en el **IdentityModels** y **IdentityAccount** carpetas desde el origen de descarga ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Estos tienen las demás clases del modelo y las nuevas páginas necesarias para el usuario y la administración de roles mediante las API de identidad de ASP.NET. El enfoque usado es similar a la pertenencia de SQL y se puede encontrar una explicación detallada [aquí](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Copiar datos de perfil para las nuevas tablas

Como se mencionó anteriormente, es necesario deserializar los datos xml en las tablas de perfiles y almacenarla en las columnas de la tabla AspNetUsers. Las nuevas columnas creadas en la tabla de usuarios en el paso anterior, por lo que lo único que queda es rellenar las columnas con los datos necesarios. Para ello, usamos una aplicación de consola que se ejecuta una vez para rellenar las columnas recién creadas en la tabla de usuarios.

1. Crear una nueva aplicación de consola en la solución existente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Instale la versión más reciente del paquete de Entity Framework.
3. Agregar la aplicación web creada anteriormente como una referencia a la aplicación de consola. Para ello haga clic en el proyecto y luego "Agregar las referencias", a continuación, la solución, a continuación, haga clic en el proyecto y haga clic en Aceptar.
4. Copie el siguiente código en la clase Program.cs. Esta lógica lee los datos de perfil para cada usuario, lo serializa como 'ProfileInfo' objeto y lo almacena en la base de datos.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Algunos de los modelos usados se definen en la carpeta 'IdentityModels' del proyecto de aplicación web, por lo que debe incluir los espacios de nombres correspondiente.
5. El código anterior funciona en el archivo de base de datos en la aplicación\_crear carpeta de datos del proyecto de aplicación web en los pasos anteriores. Para hacer referencia a este, actualice la cadena de conexión en el archivo app.config de la aplicación de consola con la cadena de conexión en el archivo web.config de la aplicación web. También proporcionan la ruta de acceso física completa en la propiedad 'AttachDbFilename'.
6. Abra un símbolo del sistema y vaya a la carpeta bin de la aplicación de consola anterior. Ejecute el archivo ejecutable y revise la salida del registro como se muestra en la siguiente imagen.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Abra la tabla 'AspNetUsers' en el Explorador de servidores y comprobar los datos de las nuevas columnas que contienen las propiedades. Se deben actualizar con los valores de propiedad correspondiente.

## <a name="verify-functionality"></a>Comprobar la funcionalidad

Use las páginas de suscripción recién agregada que se implementan mediante la identidad de ASP.NET para iniciar sesión un usuario de la base de datos. El usuario debe ser capaz de iniciar sesión con las mismas credenciales. Pruebe las otras funcionalidades, como agregar OAuth, crear un nuevo usuario, cambio de contraseñas, agregar funciones, agregar usuarios a roles, etcetera.

Los datos de perfil para el usuario anterior y los nuevos usuarios se deben recuperar y almacenan en la tabla de usuarios. Ya no se debe hacer referencia a la tabla anterior.

## <a name="conclusion"></a>Conclusión

El artículo describe el proceso de migración de aplicaciones web que usan el modelo de proveedor de pertenencia a ASP.NET Identity. Además, el artículo que describe migrar datos de perfil para que los usuarios se enlazan en el sistema de identidad. Deje sus comentarios a continuación para preguntas y problemas que surgen cuando se migra la aplicación.

*Gracias a Rick Anderson y Robert McMurray por revisar el artículo.*
