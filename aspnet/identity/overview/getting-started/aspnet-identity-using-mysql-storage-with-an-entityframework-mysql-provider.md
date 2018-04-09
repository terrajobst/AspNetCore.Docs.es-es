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
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="502d2-103">Identidad de ASP.NET: Uso del almacenamiento de MySQL con un proveedor de Entity Framework MySQL (C#)</span><span class="sxs-lookup"><span data-stu-id="502d2-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="502d2-104">por [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="502d2-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="502d2-105">Este tutorial muestra cómo reemplazar el mecanismo de almacenamiento de datos predeterminado para [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) con Entity Framework (proveedor de cliente SQL) con un proveedor de MySQL.</span><span class="sxs-lookup"><span data-stu-id="502d2-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="502d2-106">En este tutorial se explicará en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="502d2-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="502d2-107">Crear una base de datos MySQL en Azure</span><span class="sxs-lookup"><span data-stu-id="502d2-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="502d2-108">Crear una aplicación de MVC con plantilla de Visual Studio 2013 MVC</span><span class="sxs-lookup"><span data-stu-id="502d2-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="502d2-109">Configurar Entity Framework para trabajar con un proveedor de base de datos de MySQL</span><span class="sxs-lookup"><span data-stu-id="502d2-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="502d2-110">Ejecutar la aplicación para comprobar los resultados</span><span class="sxs-lookup"><span data-stu-id="502d2-110">Running the application to verify the results</span></span>

<span data-ttu-id="502d2-111">Al final de este tutorial, tendrá una aplicación de MVC con la identidad de ASP.NET almacenar que está usando una base de datos de MySQL que se hospeda en Azure.</span><span class="sxs-lookup"><span data-stu-id="502d2-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="502d2-112">Crear una instancia de base de datos MySQL en Azure</span><span class="sxs-lookup"><span data-stu-id="502d2-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="502d2-113">Inicie sesión en el [Portal de Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="502d2-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="502d2-114">Haga clic en **NEW** en la parte inferior de la página y, a continuación, seleccione **almacén**:</span><span class="sxs-lookup"><span data-stu-id="502d2-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="502d2-115">En el **elegir y un complemento** asistente, seleccione **base de datos de MySQL de ClearDB**y, a continuación, haga clic en el **siguiente** flecha en la parte inferior del marco de:</span><span class="sxs-lookup"><span data-stu-id="502d2-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="502d2-116">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-116">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-117">]</span><span class="sxs-lookup"><span data-stu-id="502d2-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="502d2-118">Mantenga el valor predeterminado **libre** previsto, cambie la **nombre** a **IdentityMySQLDatabase**, seleccione la región más cercana a la y, a continuación, haga clic en el **siguiente** flecha en la parte inferior del marco de:</span><span class="sxs-lookup"><span data-stu-id="502d2-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="502d2-119">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-119">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-120">]</span><span class="sxs-lookup"><span data-stu-id="502d2-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="502d2-121">Haga clic en el **compra** marca de verificación para completar la creación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="502d2-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="502d2-122">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-122">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-123">]</span><span class="sxs-lookup"><span data-stu-id="502d2-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="502d2-124">Una vez creada la base de datos, pueda administrarlo desde la **complementos** ficha en el portal de administración.</span><span class="sxs-lookup"><span data-stu-id="502d2-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="502d2-125">Para recuperar la información de conexión para la base de datos, haga clic en **información de conexión** en la parte inferior de la página:</span><span class="sxs-lookup"><span data-stu-id="502d2-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="502d2-126">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-126">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-127">]</span><span class="sxs-lookup"><span data-stu-id="502d2-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="502d2-128">Copie la cadena de conexión, haga clic en el botón Copiar mediante el **CONNECTIONSTRING** campo y guárdelo; usará esta información más adelante en este tutorial para la aplicación de MVC:</span><span class="sxs-lookup"><span data-stu-id="502d2-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="502d2-129">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-129">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-130">]</span><span class="sxs-lookup"><span data-stu-id="502d2-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="502d2-131">Crear un proyecto de aplicación de MVC</span><span class="sxs-lookup"><span data-stu-id="502d2-131">Creating an MVC application project</span></span>

<span data-ttu-id="502d2-132">Para completar los pasos descritos en esta sección del tutorial, primero deberá instalar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="502d2-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="502d2-133">Una vez que se haya instalado Visual Studio, siga estos pasos para crear un nuevo proyecto de aplicación de MVC:</span><span class="sxs-lookup"><span data-stu-id="502d2-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="502d2-134">Abra Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="502d2-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="502d2-135">Haga clic en **nuevo proyecto** desde el **iniciar** página, también puede hacer clic en el **archivo** menú y, a continuación, **nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="502d2-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="502d2-136">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-136">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-137">]</span><span class="sxs-lookup"><span data-stu-id="502d2-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="502d2-138">Cuando el **nuevo proyecto** aparece el cuadro de diálogo, expanda **Visual C#** en la lista de plantillas, a continuación, haga clic en **Web**y seleccione **aplicación Web de ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="502d2-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="502d2-139">Denomine el proyecto **IdentityMySQLDemo** y, a continuación, haga clic en **Aceptar**:</span><span class="sxs-lookup"><span data-stu-id="502d2-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="502d2-140">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-140">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-141">]</span><span class="sxs-lookup"><span data-stu-id="502d2-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="502d2-142">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **MVC** plantilla de soporteCon las opciones predeterminadas; Esto le permitirá configurar **cuentas de usuario individuales** como método de autenticación.</span><span class="sxs-lookup"><span data-stu-id="502d2-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="502d2-143">Haga clic en **Aceptar**:</span><span class="sxs-lookup"><span data-stu-id="502d2-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="502d2-144">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-144">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-145">]</span><span class="sxs-lookup"><span data-stu-id="502d2-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="502d2-146">Configurar Entity Framework para trabajar con una base de datos de MySQL</span><span class="sxs-lookup"><span data-stu-id="502d2-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="502d2-147">Actualizar el ensamblado de Entity Framework para el proyecto</span><span class="sxs-lookup"><span data-stu-id="502d2-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="502d2-148">La aplicación de MVC que se creó a partir de la plantilla de Visual Studio 2013 contiene una referencia a la [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) del paquete, pero tiene ha importantes actualizaciones a dicho ensamblado desde su lanzamiento que contienen mejoras de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="502d2-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="502d2-149">Para utilizar estas actualizaciones más recientes en la aplicación, siga estos pasos.</span><span class="sxs-lookup"><span data-stu-id="502d2-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="502d2-150">Abra el proyecto MVC en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="502d2-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="502d2-151">Haga clic en **herramientas**, a continuación, haga clic en **Administrador de paquetes de biblioteca**y, a continuación, haga clic en **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="502d2-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="502d2-152">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-152">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-153">]</span><span class="sxs-lookup"><span data-stu-id="502d2-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="502d2-154">El **Package Manager Console** aparecerá en la sección inferior de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="502d2-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="502d2-155">Tipo de &quot; **EntityFramework de paquete de actualización** &quot; y presione ENTRAR:</span><span class="sxs-lookup"><span data-stu-id="502d2-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="502d2-156">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-156">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-157">]</span><span class="sxs-lookup"><span data-stu-id="502d2-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="502d2-158">Instalar al proveedor de MySQL para Entity Framework</span><span class="sxs-lookup"><span data-stu-id="502d2-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="502d2-159">En orden para Entity Framework para conectarse a la base de datos MySQL, debe instalar a un proveedor de MySQL.</span><span class="sxs-lookup"><span data-stu-id="502d2-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="502d2-160">Para ello, abra el **Package Manager Console** y tipo &quot; **Pre - Install-Package MySql.Data.Entity**&quot;, y, a continuación, presione ENTRAR.</span><span class="sxs-lookup"><span data-stu-id="502d2-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="502d2-161">Se trata de una versión preliminar del ensamblado y, por lo tanto puede contener errores.</span><span class="sxs-lookup"><span data-stu-id="502d2-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="502d2-162">No se debe usar una versión preliminar del proveedor en producción.</span><span class="sxs-lookup"><span data-stu-id="502d2-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="502d2-163">[Haga clic en la siguiente imagen para expandirlo.]</span><span class="sxs-lookup"><span data-stu-id="502d2-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="502d2-164">Realizar cambios de configuración de proyecto en el archivo Web.config de la aplicación</span><span class="sxs-lookup"><span data-stu-id="502d2-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="502d2-165">En esta sección que configurará el Entity Framework para utilizar el proveedor de MySQL que acaba de instalar, registrar el generador de proveedores de MySQL y agregar la cadena de conexión de Azure.</span><span class="sxs-lookup"><span data-stu-id="502d2-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="502d2-166">Los ejemplos siguientes contienen una versión de ensamblado específico para MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="502d2-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="502d2-167">Si cambia la versión del ensamblado, debe modificar los valores de configuración adecuado con la versión correcta.</span><span class="sxs-lookup"><span data-stu-id="502d2-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="502d2-168">Abra el archivo Web.config para el proyecto en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="502d2-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="502d2-169">Busque las siguientes opciones de configuración, que define el proveedor de base de datos predeterminado y el generador para Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="502d2-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="502d2-170">Reemplace los valores de configuración con los siguientes valores, que se va a configurar el Entity Framework para usar el proveedor MySQL:</span><span class="sxs-lookup"><span data-stu-id="502d2-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="502d2-171">Busque la &lt;connectionStrings&gt; sección y reemplazarlo con el código siguiente, que define la cadena de conexión para la base de datos de MySQL que se hospeda en Azure (tenga en cuenta que el valor de providerName también se ha cambiado desde la original):</span><span class="sxs-lookup"><span data-stu-id="502d2-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="502d2-172">Agregar contexto MigrationHistory personalizado</span><span class="sxs-lookup"><span data-stu-id="502d2-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="502d2-173">Entity Framework Code First usa un **MigrationHistory** tabla para realizar un seguimiento de cambios en el modelo y para asegurar la coherencia entre el esquema de base de datos y el esquema conceptual.</span><span class="sxs-lookup"><span data-stu-id="502d2-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="502d2-174">Sin embargo, esta tabla no funciona para MySQL de forma predeterminada porque la clave principal es demasiado grande.</span><span class="sxs-lookup"><span data-stu-id="502d2-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="502d2-175">Para solucionar este problema, debe reducir el tamaño de clave para esa tabla.</span><span class="sxs-lookup"><span data-stu-id="502d2-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="502d2-176">Para ello, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="502d2-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="502d2-177">La información de esquema para esta tabla se capturan en un **HistoryContext**, que puede modificarse como cualquier otro **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="502d2-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="502d2-178">Para ello, agregue un nuevo archivo de clase denominado **MySqlHistoryContext.cs** al proyecto y reemplazar su contenido con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="502d2-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="502d2-179">A continuación debe configurar Entity Framework para utilizar modificados **HistoryContext**, en lugar del valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="502d2-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="502d2-180">Esto puede hacerse mediante el aprovechamiento de características de la configuración basada en código.</span><span class="sxs-lookup"><span data-stu-id="502d2-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="502d2-181">Para ello, agregue el nuevo archivo de clase denominado **MySqlConfiguration.cs** al proyecto y reemplace su contenido con:</span><span class="sxs-lookup"><span data-stu-id="502d2-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="502d2-182">Creación de un inicializador de Entity Framework personalizado para ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="502d2-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="502d2-183">El proveedor de MySQL que se destaquen en este tutorial no admite actualmente las migraciones de Entity Framework, por lo que deberá usar a inicializadores de modelo para poder conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="502d2-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="502d2-184">Dado que este tutorial usa una instancia de MySQL en Azure, debe crear a un inicializador personalizado de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="502d2-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="502d2-185">Este paso no es necesario si se conecta a una instancia de SQL Server en Azure o si está utilizando una base de datos que se hospeda de forma local.</span><span class="sxs-lookup"><span data-stu-id="502d2-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="502d2-186">Para crear a un inicializador de Entity Framework personalizado para MySQL, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="502d2-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="502d2-187">Agregar un nuevo archivo de clase denominado **MySqlInitializer.cs** al proyecto y reemplazar es contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="502d2-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="502d2-188">Abra la **IdentityModels.cs** archivo para el proyecto, que se encuentra en la **modelos** directorio y reemplace su contenido con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="502d2-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="502d2-189">Ejecutar la aplicación y comprobar la base de datos</span><span class="sxs-lookup"><span data-stu-id="502d2-189">Running the application and verifying the database</span></span>

<span data-ttu-id="502d2-190">Una vez haya completado los pasos descritos en las secciones anteriores, debe probar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="502d2-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="502d2-191">Para ello, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="502d2-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="502d2-192">Presione **Ctrl + F5** para compilar y ejecutar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="502d2-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="502d2-193">Haga clic en el **registrar** ficha en la parte superior de la página:</span><span class="sxs-lookup"><span data-stu-id="502d2-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="502d2-194">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-194">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-195">]</span><span class="sxs-lookup"><span data-stu-id="502d2-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="502d2-196">Escriba un nuevo nombre de usuario y una contraseña y, a continuación, haga clic en **registrar**:</span><span class="sxs-lookup"><span data-stu-id="502d2-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="502d2-197">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-197">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-198">]</span><span class="sxs-lookup"><span data-stu-id="502d2-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="502d2-199">En este momento en que se crean las tablas de ASP.NET Identity en la base de datos MySQL, y el usuario registrado y ha iniciado sesión en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="502d2-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="502d2-200">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-200">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-201">]</span><span class="sxs-lookup"><span data-stu-id="502d2-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="502d2-202">Instalación de la herramienta de MySQL Workbench para comprobar los datos</span><span class="sxs-lookup"><span data-stu-id="502d2-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="502d2-203">Instalar el **MySQL Workbench** herramienta desde la [página de descargas de MySQL](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="502d2-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="502d2-204">En el Asistente para la instalación: **selección de características** ficha, seleccione **MySQL Workbench** en **aplicaciones** sección.</span><span class="sxs-lookup"><span data-stu-id="502d2-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="502d2-205">Inicie la aplicación y agregar una nueva conexión con los datos de cadena de conexión de la base de datos de MySQL de Azure que creó en el exponerla a riesgos de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="502d2-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="502d2-206">Después de establecer la conexión, inspeccionar la **ASP.NET Identity** tablas creadas en el **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="502d2-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="502d2-207">Verá que todas las identidades de ASP.NET requiere que se crean tablas tal como se muestra en la imagen siguiente:</span><span class="sxs-lookup"><span data-stu-id="502d2-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="502d2-208">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-208">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-209">]</span><span class="sxs-lookup"><span data-stu-id="502d2-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="502d2-210">Inspeccionar el **aspnetusers** para la instancia de la tabla para comprobar las entradas como registrar nuevos usuarios.</span><span class="sxs-lookup"><span data-stu-id="502d2-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="502d2-211">[Haga clic en la siguiente imagen para expandirlo.</span><span class="sxs-lookup"><span data-stu-id="502d2-211">[Click the following image to expand it.</span></span> <span data-ttu-id="502d2-212">]</span><span class="sxs-lookup"><span data-stu-id="502d2-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
