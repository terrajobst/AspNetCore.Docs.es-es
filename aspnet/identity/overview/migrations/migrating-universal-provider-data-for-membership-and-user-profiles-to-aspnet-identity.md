---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrar datos de proveedor Universal para la suscripción y perfiles de usuario para la identidad de ASP.NET (C#) | Documentos de Microsoft
author: rustd
description: Este tutorial describen los pasos necesarios migrar datos de las funciones y del usuario y los datos de perfil de usuario creados con proveedores universales de una aplicación existente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: f65f93b20543d06ea70a9009b6921e297477c99e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871576"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrar datos de proveedor Universal para la suscripción y perfiles de usuario para la identidad de ASP.NET (C#)
====================
por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial describen los pasos necesarios migrar datos de las funciones y del usuario y los datos de perfil de usuario creados con proveedores universales de una aplicación existente para el modelo de identidad de ASP.NET. El enfoque mencionados aquí puede usarse migrar datos de perfil de usuario en una aplicación con la suscripción SQL también.


Con la versión de Visual Studio 2013, el equipo de ASP.NET introdujo un nuevo sistema de ASP.NET Identity y puede leer más acerca de esa versión [aquí](../../index.md). En el artículo que se va a migrar las aplicaciones web de [pertenencia de SQL en el nuevo sistema de identidad](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), este artículo explica los pasos para migrar aplicaciones existentes que siguen el modelo de proveedores para la administración de usuarios y roles en el nuevo modelo de identidad. El objetivo de este tutorial será principalmente sobre cómo migrar los datos de perfil de usuario para enlazarlo sin problemas en el nuevo sistema. Migrar la información de usuario y el rol es similar para la suscripción SQL. El enfoque de seguir para migrar datos de perfil puede usarse en una aplicación con la suscripción SQL también.

Por ejemplo, empezaremos con una aplicación web creada con Visual Studio 2012 que usa el modelo de proveedores. Se podrá, a continuación, agregue código para la administración de perfiles, registrar un usuario, agregar datos de perfil para los usuarios, migrar el esquema de base de datos y, a continuación, cambie la aplicación para usar el sistema de identidades de usuario y administración de funciones. Como una prueba de migración, los usuarios creados mediante proveedores universales deben ser podrán iniciar sesión y deben ser capaz de registrar los nuevos usuarios.

> [!NOTE]
> Puede encontrar el ejemplo completo en [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Resumen de migración de datos de perfil

Antes de comenzar con las migraciones, echemos un vistazo a la experiencia de almacenamiento de datos de perfil en el modelo de proveedores. Datos de perfil de aplicación a los usuarios se pueden almacenar en varias formas, los más comunes entre ellos está utilizando el perfil integrado proveedores enviados junto con los proveedores universales. Incluyen los pasos

1. Agregar una clase que tiene propiedades que se usan para almacenar los datos de perfil.
2. Agregue una clase que extiende 'ProfileBase' e implementa métodos para obtener los datos de perfil descritos anteriormente para el usuario.
3. Habilitar el uso de proveedores de perfiles de forma predeterminada en el *web.config* de archivos y definir la clase declarada en el paso 2 de # que se utilizará al obtener acceso a la información de perfil.

La información de perfil se almacena como datos binarios en la tabla 'Perfiles' de la base de datos y xml serializado.

Después de migrar la aplicación para utilizar el nuevo sistema de identidad de ASP.NET, la información de perfil se deserializa y se almacenan como propiedades en la clase de usuario. Cada propiedad, a continuación, se asignan a columnas de la tabla de usuario. Aquí la ventaja es que las propiedades pueden trabajar directamente con la clase de usuario además de no tener que serializar o deserializar la información de datos cada vez cuando se obtiene acceso.

## <a name="getting-started"></a>Introducción

1. Cree una nueva aplicación de formularios Web Forms de ASP.NET 4.5 en Visual Studio 2012. El ejemplo actual usa la plantilla de formularios Web Forms, pero podría utilizar aplicación MVC también.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Crear una nueva carpeta 'Modelos' para almacenar información de perfil  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Por ejemplo, almacenar háganoslo la fecha de nacimiento, ciudad, alto y peso del usuario en el perfil. El alto y el peso se almacenan como una clase personalizada denominada 'PersonalStats'. Para almacenar y recuperar el perfil, se necesita una clase que extiende 'ProfileBase'. Vamos a crear una nueva clase 'AppProfile' para obtener y almacenar información de perfil.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Habilitar el perfil en el *web.config* archivo. Escriba el nombre de clase que se utilizará para almacenar o recuperar información de usuario creada en el paso 3 de #.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Agregar una página de formularios web en la carpeta 'Account' para obtener los datos del perfil del usuario y los almacene. Haga clic con el botón secundario en el proyecto y seleccione 'Agregar nuevo elemento'. Agregue una nueva página de formularios Web Forms con página maestra 'AddProfileData.aspx'. Copie lo siguiente en la sección 'MainContent':

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Agregue el código siguiente en el código subyacente:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Agregue el espacio de nombres bajo qué AppProfile se define la clase para quitar los errores de compilación.
6. Ejecutar la aplicación y crear un nuevo usuario con el nombre de usuario '**olduser'.** Navegue a la página 'AddProfileData' y agregue información de perfil para el usuario.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Puede comprobar que los datos se almacenan como xml serializado en la tabla 'Perfiles' mediante la ventana del explorador de servidores. En Visual Studio, desde el menú "Ver", elija 'Explorador de servidores'. Debería haber una conexión de datos para la base de datos definido en el *web.config* archivo. Al hacer clic en la conexión de datos muestra las categorías de sub diferentes. Expanda 'Tablas' para mostrar las distintas tablas en la base de datos, a continuación, haga clic con el botón secundario en "Perfiles" y elegir "Mostrar datos de tabla' para ver los datos de perfil que se almacenan en la tabla de perfiles.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrar el esquema de base de datos

Para que la base de datos existente que funcione con el sistema de identidad, es necesario actualizar el esquema en la base de datos de identidad para admitir los campos que se agregan a la base de datos original. Esto puede hacerse mediante secuencias de comandos SQL para crear nuevas tablas y copiar la información existente. En la ventana 'Explorador de servidores de ', expanda el 'DefaultConnection' para mostrar las tablas. Haga clic en tablas y seleccione 'Nueva consulta'

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Pegue el script SQL de [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) y ejecútelo. Si se actualiza 'DefaultConnection', podemos ver que se agregan las nuevas tablas. Puede comprobar los datos dentro de las tablas para ver que la información se ha migrado.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrar la aplicación para utilizar la identidad de ASP.NET

1. Instalar los paquetes de Nuget necesarios para la identidad de ASP.NET:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Puede encontrar más información acerca de cómo administrar paquetes de Nuget [aquí](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Para trabajar con los datos existentes en la tabla, es necesario crear clases de modelo que se asignan a las tablas y enlazar en el sistema de identidades. Como parte del contrato de identidad, las clases del modelo deben implementar las interfaces definidas en el archivo dll Identity.Core o amplían la implementación existente de estas interfaces disponibles en Microsoft.AspNet.Identity.EntityFramework. Vamos a usar las clases existentes para el rol, los inicios de sesión de usuario y notificaciones de usuario. Es necesario utilizar un usuario personalizado para nuestro ejemplo. Haga clic con el botón secundario en el proyecto y crear nueva carpeta 'IdentityModels'. Agregue una nueva clase de 'User' tal y como se muestra a continuación:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Tenga en cuenta que 'ProfileInfo' ahora es una propiedad en la clase de usuario. Por lo tanto, podemos usar la clase de usuario para trabajar directamente con datos de perfil.

Copie los archivos en el **IdentityModels** y **IdentityAccount** carpetas desde el origen de la descarga ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Estos tienen las demás clases del modelo y las nuevas páginas necesarias para el usuario y administración de roles mediante las API de identidad de ASP.NET. El enfoque usado es similar a la pertenencia de SQL y se puede encontrar una explicación detallada [aquí](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

## <a name="copying-profile-data-to-the-new-tables"></a>Copiar datos de perfil en las nuevas tablas

Como se mencionó anteriormente, es necesario deserializar los datos xml en las tablas de perfiles y los almacene en las columnas de la tabla AspNetUsers. Las nuevas columnas creadas en la tabla de usuarios en el paso anterior, por lo que todo lo que queda es rellenar las columnas con los datos necesarios. Para ello, usamos una aplicación de consola que se ejecuta una vez para rellenar las columnas recién creadas en la tabla de usuarios.

1. Cree una nueva aplicación de consola en la solución existente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Instale la versión más reciente del paquete de Entity Framework.
3. Agregar la aplicación web creado anteriormente como una referencia a la aplicación de consola. Para hacer esto derecho haga clic en proyecto, 'Agregar referencias a', a continuación, la solución, a continuación, haga clic en el proyecto y haga clic en Aceptar.
4. Copie el siguiente código en la clase Program.cs. Esta lógica lee los datos de perfil para cada usuario, lo serializa como objeto 'ProfileInfo' y lo almacena en la base de datos.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Algunos de los modelos usados se definen en la carpeta 'IdentityModels' del proyecto de aplicación web, por lo que debe incluir los espacios de nombres correspondiente.
5. El código anterior funciona en el archivo de base de datos en la aplicación\_crear carpeta de datos del proyecto de aplicación web en los pasos anteriores. Para hacer referencia a, actualizar la cadena de conexión en el archivo app.config de la aplicación de consola con la cadena de conexión en el archivo web.config de la aplicación web. También proporcionan la ruta de acceso física completa en la propiedad 'AttachDbFilename'.
6. Abra un símbolo del sistema y navegue hasta la carpeta bin de la aplicación de consola anterior. Ejecute el archivo ejecutable y revise los resultados del registro tal como se muestra en la siguiente imagen.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Abra la tabla 'AspNetUsers' en el Explorador de servidores y comprobar los datos en las nuevas columnas que contengan las propiedades. Se deben actualizar con los valores de propiedad correspondiente.

## <a name="verify-functionality"></a>Comprobar la funcionalidad

Use las páginas de pertenencia recién agregado que se implementan mediante ASP.NET Identity que inicie sesión un usuario de la base de datos anterior. El usuario debe ser capaz de iniciar sesión con las mismas credenciales. Pruebe las otras funcionalidades como agregar OAuth, crear un nuevo usuario, cambio de contraseñas, la adición de roles, agregar usuarios a roles, etcetera.

Los datos de perfil para el usuario anterior y los nuevos usuarios se deben recuperar y almacenan en la tabla de usuarios. Ya no se debe hacer referencia a la tabla anterior.

## <a name="conclusion"></a>Conclusión

El artículo describe el proceso de migración de aplicaciones web que usan el modelo de proveedor de pertenencia a ASP.NET Identity. Además, el artículo describe migrar datos de perfil para que los usuarios se puede enlazar en el sistema de identidades. Por favor, deje sus comentarios a continuación para resolver dudas y problemas que surgen al migrar la aplicación.

*Gracias a Rick Anderson y Robert McMurray para revisar el artículo.*
