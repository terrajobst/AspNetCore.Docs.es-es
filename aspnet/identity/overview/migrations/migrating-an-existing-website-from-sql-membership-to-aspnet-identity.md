---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrar un sitio Web existente de la pertenencia SQL a ASP.NET Identity | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra los pasos para migrar una aplicación web existente con el usuario y los datos del rol creados mediante la suscripción de SQL a la nueva identidad de ASP.NET s...
ms.author: aspnetcontent
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4acb8c82c9b05de9d587466170f8fac4ef9b6dde
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812256"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrar un sitio Web existente de la pertenencia SQL a ASP.NET Identity
====================
por [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> En este tutorial se muestra los pasos para migrar una aplicación web existente con datos de usuarios y roles creados mediante la pertenencia de SQL para el nuevo sistema ASP.NET Identity. Este enfoque implica cambiar el esquema de base de datos existente a la que sea necesario por la identidad de ASP.NET y el enlace en las clases antiguos o más a él. Después de adoptar este enfoque, una vez que se migra la base de datos, se controlarán las futuras actualizaciones de identidad sin esfuerzo.


Para este tutorial, echaremos una plantilla de aplicación web (formularios Web Forms) creada con Visual Studio 2010 para crear datos de usuario y el rol. Después, usaremos las secuencias de comandos SQL para migrar la base de datos existente a las tablas necesarias por el sistema de identidad. A continuación, instalar los paquetes de NuGet necesarios y agregar nuevas páginas de administración de cuenta que usan el sistema de identidad para la administración de pertenencia. Como prueba de la migración, los usuarios creados mediante la suscripción de SQL deben ser capaz de iniciar sesión y usuarios nuevos deben ser capaz de registrar. Puede encontrar un ejemplo completo [aquí](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Vea también [migrar de la pertenencia ASP.NET a ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Introducción

### <a name="creating-an-application-with-sql-membership"></a>Crear una aplicación con la pertenencia de SQL

1. Debemos comenzar con una aplicación existente que utiliza la pertenencia de SQL y tiene datos de usuario y el rol. Con el propósito de este artículo, vamos a crear una aplicación web en Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Con la herramienta de configuración de ASP.NET, cree 2 usuarios: **oldAdminUser** y **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Crear un rol denominado administrador y agregue 'oldAdminUser' como un usuario de ese rol.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Cree una sección de administración del sitio con un Default.aspx. Establezca la etiqueta a la autorización en el archivo web.config para habilitar el acceso únicamente a los usuarios en roles de administrador. Puede encontrar más información aquí [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Ver la base de datos en el Explorador de servidores para comprender las tablas creadas por el sistema de pertenencia SQL. Los datos de inicio de sesión de usuario se almacenan en aspnet\_usuarios y aspnet\_tablas de pertenencia, mientras que los datos de rol se almacenan en aspnet\_tabla Roles. Información sobre qué usuarios están en qué roles se almacenan en aspnet\_UsersInRoles tabla. Para la administración de pertenencia básica es suficiente trasladar la información de las tablas anteriores para el sistema ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrar a Visual Studio 2013

1. Instale Visual Studio Express 2013 para Web o Visual Studio 2013 junto con el [actualizaciones más recientes](https://www.microsoft.com/download/details.aspx?id=44921).
2. En la versión instalada de Visual Studio, abra el proyecto anterior. Si no está instalado SQL Server Express en el equipo, se muestra un símbolo del sistema al abrir el proyecto, ya que la cadena de conexión utiliza SQL Express. Puede elegir instalar SQL Express o como evitar cambiar la cadena de conexión a LocalDb. En este artículo se cambiarán a LocalDb.
3. Abra web.config y cambie la cadena de conexión de. SQLExpess a v11.0 (LocalDb). Quitar ' User Instance = true' de la cadena de conexión.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Abra el Explorador de servidores y compruebe que se pueden observar el esquema de tabla y los datos.
5. El sistema ASP.NET Identity funciona con la versión 4.5 o posterior de framework. Redestinar la aplicación a la versión 4.5 o superior.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Compile el proyecto para comprobar que no hay ningún error.

### <a name="installing-the-nuget-packages"></a>Instalar los paquetes de Nuget

1. En el Explorador de soluciones, haga clic en el proyecto &gt; **administrar paquetes de NuGet**. En el cuadro de búsqueda, escriba "Asp.net Identity". Seleccione el paquete en la lista de resultados y haga clic en instalar. Acepte el contrato de licencia, haga clic en el botón "Acepto". Tenga en cuenta que este paquete instalará los paquetes de dependencia: Entity Framework y la identidad de Microsoft ASP.NET Core. De forma similar, instale los siguientes paquetes (omita los últimos 4 paquetes OWIN si no desea habilitar el registro de OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrar la base de datos para el nuevo sistema de identidad

El siguiente paso es migrar la base de datos existente a un esquema requerido por el sistema ASP.NET Identity. Para lograr esto se ejecute una instancia de SQL de secuencias de comandos que tiene un conjunto de comandos para crear nuevas tablas y migrar la información de usuario existentes a las nuevas tablas. Puede encontrar el archivo de script [aquí](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Este archivo de script es específico para este ejemplo. Si creó el esquema para las tablas mediante la suscripción de SQL personalizada o modifica la necesidad de secuencias de comandos cambiarse según corresponda.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Cómo generar el script SQL para la migración de esquema

Para que las clases de ASP.NET Identity a trabajar inmediatamente con los datos de los usuarios existentes, es necesario migrar el esquema de base de datos a la que sea necesario mediante la identidad de ASP.NET. Esto se logra mediante la adición de nuevas tablas y copiar la información existente en esas tablas. De forma predeterminada, ASP.NET Identity utiliza Entity Framework para asignar las clases del modelo de identidad en la base de datos para almacenar o recuperar información. Estas clases de modelo implementan las interfaces de identidad de core definir objetos de rol y usuario. Las tablas y las columnas de la base de datos se basan en estas clases de modelo. Las clases de modelo de Entity Framework v2.1.0 de identidad y sus propiedades son tal como se define a continuación

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id. | cadena | Id. | RoleId | ProviderKey | Id. |
| Nombre de usuario | cadena | nombre | Identificador de usuario | Identificador de usuario | Tipo de notificación |
| PasswordHash | cadena |  |  | LoginProvider | ClaimValue |
| SecurityStamp | cadena |  |  |  | Usuario\_Id. |
| Correo electrónico | cadena |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | cadena |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| Valor de AccessFailedCount | int |  |  |  |  |

Es necesario tener tablas para cada uno de estos modelos con las columnas correspondientes a las propiedades. La asignación entre las clases y las tablas se define en el `OnModelCreating` método de la `IdentityDBContext`. Esto se conoce como el método de API fluido de configuración y se puede encontrar más información [aquí](https://msdn.microsoft.com/data/jj591617.aspx). La configuración de las clases es tal como se menciona más abajo

| **Clase** | **Table** | **Clave principal** | **Clave externa** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id. |  |
| IdentityRole | AspnetRoles | Id. |  |
| IdentityUserRole | AspnetUserRole | Identificador de usuario + RoleId | Usuario\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | Identificador de usuario + ProviderKey + LoginProvider | UserId -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id. | Usuario\_Id -&gt;AspnetUsers |

Con esta información podemos crear instrucciones SQL para crear nuevas tablas. Nos podemos escribir cada instrucción individualmente o generar el script completo mediante los comandos de EntityFramework PowerShell que, a continuación, se puede modificar según sea necesario. Para ello, en Abrir frente a la **Package Manager Console** desde el **vista** o **herramientas** menú

- Ejecute el comando "Enable-Migrations" para habilitar las migraciones de Entity Framework.
- Ejecute el comando "Add-migration initial", que crea el código de configuración inicial para crear la base de datos en C# / VB.
- El último paso es ejecutar "Update-Database: secuencia de comandos" comando que genera el script SQL basado en las clases del modelo.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Este script de generación de base de datos puede utilizarse como un inicio donde llevaremos a cabo cambios adicionales para agregar nuevas columnas y copia de datos. La ventaja de esto es que generamos el `_MigrationHistory` tabla utilizada por Entity Framework para modificar el esquema de base de datos cuando el modelo de clases de cambio para versiones futuras de versiones de identidad. 

La información de usuario de pertenencia SQL tenía otras propiedades además de las de la clase de modelo de usuario de identidad es decir, el correo electrónico, los intentos de contraseña, última fecha de inicio de sesión, última fecha de bloqueo etcetera. Esta información es útil y nos gustaría que se puede transferir al sistema de identidad. Esto puede hacerse agregando propiedades adicionales al modelo de usuario y asignarlas a las columnas de tabla en la base de datos. Esto se logra mediante la adición de una clase que cree subclases el `IdentityUser` modelo. Podemos agregar las propiedades para esta clase personalizada y edite el script SQL para agregar las columnas correspondientes al crear la tabla. El código para esta clase se describe más detalladamente en el artículo. El script SQL para crear el `AspnetUsers` después de agregar nuevas propiedades sería la tabla

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

A continuación debe copiar la información existente de la base de datos de pertenencia SQL a las tablas recién agregadas para la identidad. Esto puede hacerse a través de SQL mediante la copia de datos directamente desde una tabla a otra. Para agregar datos en las filas de tabla, usamos el `INSERT INTO [Table]` construir. Para copiar de otra tabla, podemos usar el `INSERT INTO` instrucción junto con el `SELECT` instrucción. Para obtener toda la información de usuario que se necesita para consultar el *aspnet\_usuarios* y *aspnet\_pertenencia* tablas y copiar los datos a la *AspNetUsers*tabla. Usamos el `INSERT INTO` y `SELECT` junto con `JOIN` y `LEFT OUTER JOIN` instrucciones. Para obtener más información sobre cómo consultar y copiar datos entre las tablas, consulte [esto](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) vínculo. Además las tablas AspnetUserLogins y AspnetUserClaims están vacías para comenzar ya que no hay ninguna información de pertenencia SQL que se asigna a esta de forma predeterminada. Es la única información que se copian los usuarios y roles. Para el proyecto creado en los pasos anteriores, sería la consulta SQL para copiar información en la tabla de usuarios

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

En la instrucción SQL anterior, información acerca de cada usuario desde el *aspnet\_usuarios* y *aspnet\_pertenencia* se copian las tablas en las columnas de la  *AspnetUsers* tabla. La única modificación hecha aquí es cuando se copie la contraseña. Puesto que utiliza el algoritmo de cifrado para las contraseñas en la pertenencia SQL 'PasswordSalt' y 'PasswordFormat', copiaremos demasiado junto con la contraseña con hash para que puede utilizarse para descifrar la contraseña de identidad. Esto se explica más adelante en el artículo al enlazar un hasher de contraseñas personalizadas. 

Este archivo de script es específico para este ejemplo. Para las aplicaciones que tienen tablas adicionales, los desarrolladores pueden seguir un enfoque similar para agregar propiedades adicionales en la clase de modelo de usuario y asignarlas a las columnas en la tabla AspnetUsers. Para ejecutar el script,

1. Abra el Explorador de servidores. Expanda la conexión 'ApplicationServices' para mostrar las tablas. Haga clic con el botón derecho en el nodo de tablas y seleccione la opción "Nueva consulta"

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. En la ventana de consulta, copie y pegue el script SQL completo desde el archivo Migrations.sql. Ejecute el archivo de script presionando el botón de flecha 'Ejecutar'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Actualice la ventana del explorador de servidores. Se crean cinco tablas nuevas en la base de datos.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    A continuación es cómo la información de las tablas de pertenencia SQL se asignan al nuevo sistema de identidad.

    ASPNET\_Roles--&gt; AspNetRoles

    ASP\_netUsers y asp\_netMembership--&gt; AspNetUsers

    ASPNET\_UserInRoles--&gt; AspNetUserRoles

    Como se explicó en la sección anterior, las tablas AspNetUserClaims y AspNetUserLogins están vacías. El campo 'Discriminador' en la tabla AspNetUser debe coincidir con el nombre de clase de modelo que se define como el siguiente paso. También la columna PasswordHash tiene el formato ' contraseña cifrada | valor "salt" de contraseña | formato de la contraseña '. Esto le permite usar SQL pertenencia crypto una lógica especial para que se pueden reutilizar las contraseñas antiguas. En el que se explica más adelante en el artículo.

### <a name="creating-models-and-membership-pages"></a>Creación de modelos y las páginas de pertenencia

Como se mencionó anteriormente, la característica de identidad utiliza Entity Framework para comunicarse con la base de datos para almacenar información de la cuenta de forma predeterminada. Para trabajar con datos existentes en la tabla, necesitamos crear clases de modelo que se asignan a las tablas y enlazar en el sistema de identidad. Como parte del contrato de identidad, las clases del modelo deben implementar las interfaces definidas en el archivo dll de Identity.Core o pueden extender la implementación existente de estas interfaces disponibles en Microsoft.AspNet.Identity.EntityFramework.

En nuestro ejemplo, las tablas AspNetRoles, AspNetUserClaims, AspNetLogins y AspNetUserRole tengan columnas que son similares a la implementación existente del sistema de identidad. Por lo tanto, nos podemos volver a usar las clases existentes para asignar a estas tablas. La tabla AspNetUser tiene algunas columnas adicionales que se usan para almacenar información adicional de las tablas de pertenencia SQL. Esto se puede asignar mediante la creación de una clase de modelo que amplíe la implementación existente de 'IdentityUser' y agregue las propiedades adicionales.

1. Carpeta de modelos crea en el proyecto y agregue una clase de usuario. El nombre de la clase debe coincidir con los datos agregados en la columna 'Discriminador' de la tabla 'AspnetUsers'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    La clase de usuario debe extender la clase IdentityUser se encuentra en la *Microsoft.AspNet.Identity.EntityFramework* dll. Declarar las propiedades de clase que se asignan a las columnas AspNetUser. Las propiedades ID, nombre de usuario, PasswordHash y SecurityStamp se definen en el IdentityUser y se omiten. A continuación es el código para la clase de usuario que tiene todas las propiedades

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Se requiere una clase DbContext de Entity Framework con el fin de conservar los datos en los modelos a las tablas y recuperar datos de tablas para rellenar los modelos. *Microsoft.AspNet.Identity.EntityFramework* dll define la clase de IdentityDbContext que interactúa con las tablas de identidad para recuperar y almacenar información. IdentityDbContext&lt;tuser&gt; toma una clase 'TUser', que puede ser cualquier clase que extiende la clase IdentityUser.

    Cree una nueva clase ApplicationDBContext que extiende IdentityDbContext bajo la carpeta 'Modelos', pasando la clase 'User' creada en el paso 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Administración de usuarios en el nuevo sistema de identidad se realiza mediante UserManager&lt;tuser&gt; clase definida en el *Microsoft.AspNet.Identity.EntityFramework* dll. Es necesario crear una clase personalizada que extiende UserManager, pasando la clase 'User' creada en el paso 1.

    En la carpeta Models, cree una nueva clase UserManager que extiende UserManager&lt;usuario&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Las contraseñas de los usuarios de la aplicación se cifran y almacenan en la base de datos. El algoritmo de cifrado utilizado en la pertenencia SQL es diferente de la existente en el nuevo sistema de identidad. Para volver a usar las contraseñas antiguas, necesitamos selectivamente descifrar las contraseñas cuando los usuarios antiguos inicien sesión con el algoritmo de pertenencias SQL al usar el algoritmo criptográfico en identidad para los nuevos usuarios.

    La clase UserManager tiene una propiedad 'PasswordHasher', que almacena una instancia de una clase que implementa la interfaz 'IPasswordHasher'. Esto se usa para cifrar y descifrar las contraseñas durante las transacciones de autenticación de usuario. En la clase UserManager definida en el paso 3, cree una nueva clase SQLPasswordHasher y copie el siguiente código.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Resuelva los errores de compilación mediante la importación de los espacios de nombres System.Text y System.Security.Cryptography.

    El método EncodePassword cifra la contraseña de acuerdo con la implementación de criptografía de pertenencia SQL predeterminada. Esto procede de la dll de System.Web. Si la aplicación antigua usa una implementación personalizada, a continuación, deben reflejarse aquí. Necesitamos definir otros dos métodos *HashPassword* y *VerifyHashedPassword* que usan el *EncodePassword* al hash de una contraseña determinada o para comprobar un texto sin formato (método) contraseña con una existente en la base de datos.

    El sistema de pertenencia SQL utiliza PasswordHash y PasswordSalt, PasswordFormat para la contraseña especificada por los usuarios cuando registre o cambiar su contraseña. Durante la migración, los tres campos se almacenan en la columna PasswordHash en la tabla AspNetUser separada por la ' |' caracteres. Cuando un usuario inicia sesión y la contraseña tiene estos campos, usamos a la criptografía de pertenencia SQL para comprobar la contraseña; en caso contrario, se usa a criptografía de predeterminada del sistema de identidad para comprobar la contraseña. De este modo los usuarios antiguos no tendría que cambiar sus contraseñas cuando se haya migrado a la aplicación.
5. Declare el constructor de la clase UserManager y pasarlo como el SQLPasswordHasher a la propiedad en el constructor.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Crear nueva cuenta de las páginas de administración

El siguiente paso en la migración es agregar páginas de administración de cuenta que permiten al usuario registre e inicie sesión. Las páginas de la cuenta antigua de pertenencia SQL usan los controles que no funcionan con el nuevo sistema de identidad. Para agregar el nuevo usuario seguir el tutorial de este vínculo a las páginas de administración [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) a partir del paso 'Agregar formularios Web Forms para registrar usuarios para la aplicación' puesto que ya hemos creado el proyecto y agregar el paquete NuGet paquetes.

Necesitamos realizar algunos cambios para el ejemplo funcione con el proyecto que tenemos aquí.

- El código Register.aspx.cs y Login.aspx.cs detrás de uso de las clases del `UserManager` desde los paquetes de identidad para crear un usuario. Este ejemplo usa UserManager agregado en la carpeta Models, siga los pasos mencionados anteriormente.
- Utilice la clase de usuario creada en lugar de la IdentityUser en Register.aspx.cs y Login.aspx.cs código detrás de las clases. Esto enlaza en nuestra clase de usuario personalizada en el sistema de identidad.
- Se puede omitir la parte para crear la base de datos.
- El desarrollador debe establecer el atributo ApplicationId para el nuevo usuario para que coincida con el identificador de aplicación actual. Esto puede hacerse consultando el ApplicationId para esta aplicación antes de que se crea un objeto de usuario en la clase Register.aspx.cs y establecerla antes de crear el usuario. 

    Ejemplo:

    Define un método en la página Register.aspx.cs para consultar aspnet\_aplicaciones de la tabla y obtiene el identificador de aplicación según el nombre de la aplicación

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Ahora obtener o establecer esto en el objeto de usuario

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Usar el antiguo nombre de usuario y la contraseña para iniciar sesión un usuario existente. Utilice la página de registro para crear un nuevo usuario. Compruebe también que los usuarios están en roles, según lo previsto.

Migración al sistema de identidad ayuda al usuario agregar autenticación abierta (OAuth) a la aplicación. Consulte el ejemplo [aquí](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) que con OAuth habilitado.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial hemos mostrado cómo migrar los usuarios de pertenencia SQL a ASP.NET Identity, pero no trasladar datos de perfil. En el siguiente tutorial, buscaremos en trasladar datos de perfil de pertenencia SQL para el nuevo sistema de identidad.

Puede dejar comentarios en la parte inferior de este artículo.

*Gracias a Tom Dykstra y Rick Anderson por revisar el artículo.*
