---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementar un proveedor de almacenamiento de identidad de ASP.NET personalizado MySQL | Microsoft Docs
author: raquelsa
description: ASP.NET Identity es un sistema extensible que le permite crear su propio proveedor de almacenamiento y conectar a su aplicación sin tener que volver a trabajar el receptor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: ''
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 44c21b749377c5df445a20deee3f1b689c7c84c0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381132"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementar un proveedor de almacenamiento de identidad de ASP.NET personalizado de MySQL
====================
por [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity es un sistema extensible que le permite crear su propio proveedor de almacenamiento y conectar a su aplicación sin tener que volver a trabajar la aplicación. Este tema describe cómo crear un proveedor de almacenamiento de MySQL para ASP.NET Identity. Para obtener información general de la creación de proveedores de almacenamiento personalizados, consulte [información general de almacenamiento proveedores personalizados para ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Para completar este tutorial, debe tener Visual Studio 2013 con Update 2.
> 
> En este tutorial hará lo siguiente:
> 
> - Muestra cómo crear una instancia de base de datos MySQL en Azure.
> - Muestra cómo usar una herramienta de cliente de MySQL (MySQL Workbench) para crear tablas y administrar la base de datos remota en Azure.
> - Muestra cómo reemplazar el valor predeterminado de implementación de almacenamiento de información de identidad de ASP.NET con nuestra implementación personalizada en un proyecto de aplicación MVC.
> 
> En este tutorial se escribió originalmente por Raquel Soares De Almeida y Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Se actualizó el proyecto de ejemplo para identidad 2.0 por Suhas Joshi. El tema se ha actualizado por Tom FitzMacken identidad 2.0.


## <a name="download-completed-project"></a>Proyecto de descarga completada

Al final de este tutorial, tendrá un proyecto de aplicación de MVC con ASP.NET Identity, trabajar con una base de datos MySQL hospedada en Azure.

Puede descargar el proveedor de almacenamiento de MySQL completado en [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Los pasos que se llevará a cabo

En este tutorial aprenderá a:

1. Creación de una base de datos MySQL en Azure
2. Crear tablas de la identidad de ASP.NET en MySQL
3. Crear una aplicación MVC y configúrelo para utilizar el proveedor de MySQL
4. Ejecutar la aplicación

En este tema no cubre la arquitectura de ASP.NET Identity y las decisiones que debe tomar al implementar un proveedor de almacenamiento del cliente. Para obtener esa información, consulte [información general de Custom proveedores de almacenamiento para ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Revise las clases de proveedor de almacenamiento de MySQL

Antes de pasar a los pasos para crear el proveedor de almacenamiento de MySQL, echemos un vistazo a las clases que constituyen el proveedor de almacenamiento. Necesitará las clases que administran las operaciones de base de datos y las clases que se llaman desde la aplicación para administrar usuarios y roles.

### <a name="storage-classes"></a>Clases de almacenamiento

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -contiene propiedades para el usuario.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -contiene operaciones para agregar, actualizar o recuperar los usuarios.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -contiene las propiedades de roles.
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -contiene operaciones para agregar, eliminar, actualizar y recuperar roles.

### <a name="data-access-layer-classes"></a>Clases de capa de acceso a datos

En este ejemplo, las clases de nivel de acceso de datos contienen instrucciones SQL para trabajar con las tablas; Sin embargo, en el código puede utilizar la asignación relacional de objetos (ORM) como Entity Framework o NHibernate. En concreto, la aplicación puede experimentar un rendimiento deficiente sin un ORM que incluye la carga diferida y el almacenamiento en caché de objeto. ¿Para obtener más información, consulte [Identity de ASP.NET 2.0 sin Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -contiene la conexión de base de datos de MySQL y métodos para realizar operaciones de base de datos. Se instancian con una instancia de esta clase UserStore y RoleStore.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -contiene operaciones de base de datos para la tabla que almacena los roles.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -contiene operaciones de base de datos para la tabla que almacena las notificaciones de usuario.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -contiene operaciones de base de datos para la tabla que almacena la información de inicio de sesión de usuario.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -contiene operaciones de base de datos para la tabla que almacena los usuarios que están asignados a cada rol.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -contiene operaciones de base de datos para la tabla que almacena los usuarios.

## <a name="create-a-mysql-database-instance-on-azure"></a>Creación de una instancia de base de datos MySQL en Azure

1. Inicie sesión en [Azure Portal](https://manage.windowsazure.com/).
2. Haga clic en **+ nuevo** en la parte inferior de la página y, a continuación, seleccione **almacén**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. En el **elegir y complemento** asistente, seleccione **base de datos de MySQL de ClearDB** y haga clic en la flecha siguiente en la parte inferior derecha del cuadro de diálogo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Mantenga el valor predeterminado **gratis** planear y cambiar el **nombre** a **IdentityMySQLDatabase**. Seleccione la región más cercana y, a continuación, haga clic en la flecha siguiente.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Haga clic en la marca de verificación para completar la creación de la base de datos.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Una vez creada la base de datos, puede administrarlo desde el **complementos** ficha en el portal de administración.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Puede obtener la información de conexión de base de datos haciendo clic en **información de conexión** en la parte inferior de la página.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copie la cadena de conexión haciendo clic en el botón Copiar y guardarla para usarla más adelante en la aplicación de MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Crear tablas de la identidad de ASP.NET en una base de datos MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Instalar la herramienta MySQL Workbench para conectarse y administrar la base de datos MySQL

1. Instalar el **MySQL Workbench** herramienta desde el [página de descargas de MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Inicie la aplicación y agregar haga clic en el **MySQLConnections +** para agregar una nueva conexión. Use los datos de cadena de conexión que copió de la base de datos MySQL de Azure que creó anteriormente en este tutorial.
3. Después de establecer la conexión, abra una nueva **consulta** pestaña; pegar los comandos de [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) en la consulta y ejecutarla con el fin de crear las tablas de base de datos.
4. Ahora tiene todas las tablas necesarias ASP.NET Identity creadas en una base de datos MySQL hospedada en Azure, como se muestra a continuación.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Crear un proyecto de aplicación MVC de plantilla y configúrelo para utilizar el proveedor de MySQL

Si es necesario, instale [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Descargar el proyecto ASP.NET.Identity.MySQL desde CodePlex

1. Vaya a la dirección URL del repositorio en [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Descargar el código fuente.
3. Extraiga el archivo .zip en una carpeta local.
4. Abra la solución AspNet.Identity.MySQL y genérelo.

### <a name="create-a-new-mvc-application-project-from-template"></a>Crear un nuevo proyecto de aplicación MVC de plantilla

1. Haga clic en el **AspNet.Identity.MySQL** solución y **agregar**, **nuevo proyecto**
2. En el **Agregar nuevo proyecto** diálogo seleccione **Visual C#** a la izquierda y, a continuación, **Web** y, a continuación, seleccione **aplicación Web ASP.NET**. Nombre del proyecto **IdentityMySQLDemo**; y, a continuación, haga clic en Aceptar.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la plantilla MVC con las opciones predeterminadas (que incluye **cuentas de usuario individuales** como método de autenticación) y haga clic en **Aceptar** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. En el Explorador de soluciones, haga clic en el proyecto IdentityMySQLDemo y seleccione **administrar paquetes de NuGet**. En el cuadro de diálogo de cuadro de texto de búsqueda, escriba **Identity.EntityFramework**. Seleccione este paquete en la lista de resultados y haga clic en **desinstalar**. Se le pedirá para desinstalar el paquete EntityFramework de dependencia. Haga clic en sí como se dejará de este paquete en esta aplicación.
5. A la derecha, haga clic en el proyecto IdentityMySQLDemo, seleccione **agregar**, **referencia, soluciones, proyectos;** seleccione el proyecto AspNet.Identity.MySQL y haga clic en **Aceptar**.
6. En el proyecto IdentityMySQLDemo, reemplace todas las referencias a  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   with  
     `using AspNet.Identity.MySQL;`
7. En IdentityModels.cs, establezca **ApplicationDbContext** derivar **MySqlDatabase** e incluir un constructor que toman un único parámetro con el nombre de la conexión.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Abra el archivo IdentityConfig.cs. En el **ApplicationUserManager.Create** método, replace, crear una instancia de UserManager con el código siguiente:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Abra el archivo web.config y reemplace la cadena DefaultConnection con esta entrada reemplazando los valores resaltados con la cadena de conexión de la base de datos MySQL que creó en los pasos anteriores:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Ejecute la aplicación y conéctese a la base de datos MySQL

1. Haga clic en el **IdentityMySQLDemo** del proyecto y seleccione **establecer como proyecto de inicio**
2. Presione **Ctrl + F5** para compilar y ejecutar la aplicación.
3. Haga clic en **registrar** ficha en la parte superior de la página.
4. Escriba un nuevo nombre de usuario y contraseña y, a continuación, haga clic en **registrar**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. El nuevo usuario ahora está registrado y ha iniciado sesión.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Vuelva a la herramienta MySQL Workbench e inspeccione el **IdentityMySQLDatabase** contenido de la tabla. Examinar la tabla de usuarios para las entradas que registre los nuevos usuarios.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo habilitar otros métodos de autenticación en esta aplicación, consulte [crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Para obtener información sobre cómo integrar su base de datos con OAuth y cómo configurar los roles para limitar el acceso a los usuarios a la aplicación, consulte [implementar una aplicación ASP.NET MVC 5 segura con pertenencia, OAuth y SQL Database en Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
