---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'Identidad de ASP.NET: Uso del almacenamiento de MySQL con un proveedor de MySQL de Entity Framework (C#) | Documentos de Microsoft'
author: maumar
description: Este tutorial muestra cómo reemplazar el mecanismo de almacenamiento de datos predeterminado para ASP.NET Identity con Entity Framework (proveedor de cliente SQL) con un proporcione MySQL...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873396"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>Identidad de ASP.NET: Uso del almacenamiento de MySQL con un proveedor de Entity Framework MySQL (C#)
====================
por [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Este tutorial muestra cómo reemplazar el mecanismo de almacenamiento de datos predeterminado para [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) con Entity Framework (proveedor de cliente SQL) con un proveedor de MySQL.


En este tutorial se explicará en los temas siguientes:

- Crear una base de datos MySQL en Azure
- Crear una aplicación de MVC con plantilla de Visual Studio 2013 MVC
- Configurar Entity Framework para trabajar con un proveedor de base de datos de MySQL
- Ejecutar la aplicación para comprobar los resultados

Al final de este tutorial, tendrá una aplicación de MVC con la identidad de ASP.NET almacenar que está usando una base de datos de MySQL que se hospeda en Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Crear una instancia de base de datos MySQL en Azure

1. Inicie sesión en el [Portal de Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Haga clic en **NEW** en la parte inferior de la página y, a continuación, seleccione **almacén**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. En el **elegir y un complemento** asistente, seleccione **base de datos de MySQL de ClearDB**y, a continuación, haga clic en el **siguiente** flecha en la parte inferior del marco de:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Mantenga el valor predeterminado **libre** previsto, cambie la **nombre** a **IdentityMySQLDatabase**, seleccione la región más cercana a la y, a continuación, haga clic en el **siguiente** flecha en la parte inferior del marco de:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Haga clic en el **compra** marca de verificación para completar la creación de la base de datos.  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Una vez creada la base de datos, pueda administrarlo desde la **complementos** ficha en el portal de administración. Para recuperar la información de conexión para la base de datos, haga clic en **información de conexión** en la parte inferior de la página:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copie la cadena de conexión, haga clic en el botón Copiar mediante el **CONNECTIONSTRING** campo y guárdelo; usará esta información más adelante en este tutorial para la aplicación de MVC:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Crear un proyecto de aplicación de MVC

Para completar los pasos descritos en esta sección del tutorial, primero deberá instalar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Una vez que se haya instalado Visual Studio, siga estos pasos para crear un nuevo proyecto de aplicación de MVC:

1. Abra Visual Studio 2103.
2. Haga clic en **nuevo proyecto** desde el **iniciar** página, también puede hacer clic en el **archivo** menú y, a continuación, **nuevo proyecto**:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Cuando el **nuevo proyecto** aparece el cuadro de diálogo, expanda **Visual C#** en la lista de plantillas, a continuación, haga clic en **Web**y seleccione **aplicación Web de ASP.NET**. Denomine el proyecto **IdentityMySQLDemo** y, a continuación, haga clic en **Aceptar**:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **MVC** plantilla de soporteCon las opciones predeterminadas; Esto le permitirá configurar **cuentas de usuario individuales** como método de autenticación. Haga clic en **Aceptar**:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurar Entity Framework para trabajar con una base de datos de MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Actualizar el ensamblado de Entity Framework para el proyecto

La aplicación de MVC que se creó a partir de la plantilla de Visual Studio 2013 contiene una referencia a la [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) del paquete, pero tiene ha importantes actualizaciones a dicho ensamblado desde su lanzamiento que contienen mejoras de rendimiento. Para utilizar estas actualizaciones más recientes en la aplicación, siga estos pasos.

1. Abra el proyecto MVC en Visual Studio 2013.
2. Haga clic en **herramientas**, a continuación, haga clic en **Administrador de paquetes de biblioteca**y, a continuación, haga clic en **Package Manager Console**:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. El **Package Manager Console** aparecerá en la sección inferior de Visual Studio. Tipo de &quot; **EntityFramework de paquete de actualización** &quot; y presione ENTRAR:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Instalar al proveedor de MySQL para Entity Framework

En orden para Entity Framework para conectarse a la base de datos MySQL, debe instalar a un proveedor de MySQL. Para ello, abra el **Package Manager Console** y tipo &quot; **Pre - Install-Package MySql.Data.Entity**&quot;, y, a continuación, presione ENTRAR.

> [!NOTE]
> Se trata de una versión preliminar del ensamblado y, por lo tanto puede contener errores. No se debe usar una versión preliminar del proveedor en producción.


[Haga clic en la siguiente imagen para expandirlo.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Realizar cambios de configuración de proyecto en el archivo Web.config de la aplicación

En esta sección que configurará el Entity Framework para utilizar el proveedor de MySQL que acaba de instalar, registrar el generador de proveedores de MySQL y agregar la cadena de conexión de Azure.

> [!NOTE]
> Los ejemplos siguientes contienen una versión de ensamblado específico para MySql.Data.dll. Si cambia la versión del ensamblado, debe modificar los valores de configuración adecuado con la versión correcta.


1. Abra el archivo Web.config para el proyecto en Visual Studio 2013.
2. Busque las siguientes opciones de configuración, que define el proveedor de base de datos predeterminado y el generador para Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Reemplace los valores de configuración con los siguientes valores, que se va a configurar el Entity Framework para usar el proveedor MySQL: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Busque la &lt;connectionStrings&gt; sección y reemplazarlo con el código siguiente, que define la cadena de conexión para la base de datos de MySQL que se hospeda en Azure (tenga en cuenta que el valor de providerName también se ha cambiado desde la original):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Agregar contexto MigrationHistory personalizado

Entity Framework Code First usa un **MigrationHistory** tabla para realizar un seguimiento de cambios en el modelo y para asegurar la coherencia entre el esquema de base de datos y el esquema conceptual. Sin embargo, esta tabla no funciona para MySQL de forma predeterminada porque la clave principal es demasiado grande. Para solucionar este problema, debe reducir el tamaño de clave para esa tabla. Para ello, siga estos pasos:

1. La información de esquema para esta tabla se capturan en un **HistoryContext**, que puede modificarse como cualquier otro **DbContext**. Para ello, agregue un nuevo archivo de clase denominado **MySqlHistoryContext.cs** al proyecto y reemplazar su contenido con el código siguiente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. A continuación debe configurar Entity Framework para utilizar modificados **HistoryContext**, en lugar del valor predeterminado. Esto puede hacerse mediante el aprovechamiento de características de la configuración basada en código. Para ello, agregue el nuevo archivo de clase denominado **MySqlConfiguration.cs** al proyecto y reemplace su contenido con:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Creación de un inicializador de Entity Framework personalizado para ApplicationDbContext

El proveedor de MySQL que se destaquen en este tutorial no admite actualmente las migraciones de Entity Framework, por lo que deberá usar a inicializadores de modelo para poder conectarse a la base de datos. Dado que este tutorial usa una instancia de MySQL en Azure, debe crear a un inicializador personalizado de Entity Framework.

> [!NOTE]
> Este paso no es necesario si se conecta a una instancia de SQL Server en Azure o si está utilizando una base de datos que se hospeda de forma local.


Para crear a un inicializador de Entity Framework personalizado para MySQL, siga estos pasos:

1. Agregar un nuevo archivo de clase denominado **MySqlInitializer.cs** al proyecto y reemplazar es contenido por el código siguiente: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Abra la **IdentityModels.cs** archivo para el proyecto, que se encuentra en la **modelos** directorio y reemplace su contenido con el siguiente: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Ejecutar la aplicación y comprobar la base de datos

Una vez haya completado los pasos descritos en las secciones anteriores, debe probar la base de datos. Para ello, siga estos pasos:

1. Presione **Ctrl + F5** para compilar y ejecutar la aplicación web.
2. Haga clic en el **registrar** ficha en la parte superior de la página:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Escriba un nuevo nombre de usuario y una contraseña y, a continuación, haga clic en **registrar**:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. En este momento en que se crean las tablas de ASP.NET Identity en la base de datos MySQL, y el usuario registrado y ha iniciado sesión en la aplicación:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalación de la herramienta de MySQL Workbench para comprobar los datos

1. Instalar el **MySQL Workbench** herramienta desde la [página de descargas de MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. En el Asistente para la instalación: **selección de características** ficha, seleccione **MySQL Workbench** en **aplicaciones** sección.
3. Inicie la aplicación y agregar una nueva conexión con los datos de cadena de conexión de la base de datos de MySQL de Azure que creó en el exponerla a riesgos de este tutorial.
4. Después de establecer la conexión, inspeccionar la **ASP.NET Identity** tablas creadas en el **IdentityMySQLDatabase.**
5. Verá que todas las identidades de ASP.NET requiere que se crean tablas tal como se muestra en la imagen siguiente:  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Inspeccionar el **aspnetusers** para la instancia de la tabla para comprobar las entradas como registrar nuevos usuarios.  
  
   [Haga clic en la siguiente imagen para expandirlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
