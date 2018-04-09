---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Pertenencia y la autorización | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 7 cubre la pertenencia y la autorización.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="62d8a-104">Parte 7: Pertenencia y la autorización</span><span class="sxs-lookup"><span data-stu-id="62d8a-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="62d8a-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="62d8a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="62d8a-106">La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="62d8a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="62d8a-107">La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="62d8a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="62d8a-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="62d8a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="62d8a-109">Parte 7 cubre la pertenencia y la autorización.</span><span class="sxs-lookup"><span data-stu-id="62d8a-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="62d8a-110">Nuestro controlador Store Manager está actualmente accesible para cualquiera que visite nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="62d8a-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="62d8a-111">Vamos a cambiar esta opción para restringir los permisos a los administradores del sitio.</span><span class="sxs-lookup"><span data-stu-id="62d8a-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="62d8a-112">Agregar el AccountController y vistas</span><span class="sxs-lookup"><span data-stu-id="62d8a-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="62d8a-113">Una diferencia entre la plantilla de aplicación Web de ASP.NET MVC 3 completa y la plantilla de aplicación de Web vacía de ASP.NET MVC 3 es que la plantilla vacía no incluye un controlador de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="62d8a-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="62d8a-114">Vamos a agregar un controlador de la cuenta mediante la copia de unos pocos archivos desde una aplicación de MVC de ASP.NET nuevo creada a partir de la plantilla de aplicación Web de ASP.NET MVC 3 completa.</span><span class="sxs-lookup"><span data-stu-id="62d8a-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="62d8a-115">Crear una nueva aplicación de ASP.NET MVC mediante la plantilla de aplicación Web de ASP.NET MVC 3 completa y copie los archivos siguientes en los mismos directorios en el proyecto:</span><span class="sxs-lookup"><span data-stu-id="62d8a-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="62d8a-116">Copie AccountController.cs en el directorio de controladores</span><span class="sxs-lookup"><span data-stu-id="62d8a-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="62d8a-117">Copie AccountModels en el directorio de modelos</span><span class="sxs-lookup"><span data-stu-id="62d8a-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="62d8a-118">Cree un directorio de cuenta en el directorio de vistas y copie todas las cuatro vistas de</span><span class="sxs-lookup"><span data-stu-id="62d8a-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="62d8a-119">Cambiar el espacio de nombres para las clases de controlador y el modelo de modo que comienzan por MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="62d8a-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="62d8a-120">La clase AccountController debería utilizar el espacio de nombres MvcMusicStore.Controllers y la clase AccountModels debe utilizar el espacio de nombres MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="62d8a-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="62d8a-121">*Nota: Estos archivos también están disponibles en la descarga de MvcMusicStore Assets.zip desde el que se copian los archivos de diseño de sitio al principio del tutorial. Los archivos de suscripción se encuentran en el directorio de código.*</span><span class="sxs-lookup"><span data-stu-id="62d8a-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="62d8a-122">La solución actualizada debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="62d8a-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="62d8a-123">Agregar un usuario administrativo con el sitio de configuración de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="62d8a-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="62d8a-124">Antes de que se necesita autorización en nuestro sitio Web, necesitamos crear un usuario con acceso.</span><span class="sxs-lookup"><span data-stu-id="62d8a-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="62d8a-125">La manera más fácil de crear un usuario es usar el sitio Web de configuración de ASP.NET integrado.</span><span class="sxs-lookup"><span data-stu-id="62d8a-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="62d8a-126">Inicie el sitio Web de configuración de ASP.NET, haga clic en siguiente el icono en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="62d8a-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="62d8a-127">Se abre un sitio Web de configuración.</span><span class="sxs-lookup"><span data-stu-id="62d8a-127">This launches a configuration website.</span></span> <span data-ttu-id="62d8a-128">Haga clic en la ficha seguridad en la pantalla principal, haga clic en el vínculo "Habilitar los roles" en el centro de la pantalla.</span><span class="sxs-lookup"><span data-stu-id="62d8a-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="62d8a-129">Haga clic en el vínculo "crear o administrar funciones".</span><span class="sxs-lookup"><span data-stu-id="62d8a-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="62d8a-130">Escriba "Administrador" como el nombre de rol y presione el botón Agregar rol.</span><span class="sxs-lookup"><span data-stu-id="62d8a-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="62d8a-131">Haga clic en el botón Atrás y, a continuación, haga clic en el vínculo de usuario de crear en el lado izquierdo.</span><span class="sxs-lookup"><span data-stu-id="62d8a-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="62d8a-132">Rellene los campos de información de usuario de la izquierda con la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="62d8a-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="62d8a-133">**Campo**</span><span class="sxs-lookup"><span data-stu-id="62d8a-133">**Field**</span></span> | <span data-ttu-id="62d8a-134">**Valor**</span><span class="sxs-lookup"><span data-stu-id="62d8a-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="62d8a-135">**Nombre de usuario**</span><span class="sxs-lookup"><span data-stu-id="62d8a-135">**User Name**</span></span> | <span data-ttu-id="62d8a-136">Administrador</span><span class="sxs-lookup"><span data-stu-id="62d8a-136">Administrator</span></span> |
| <span data-ttu-id="62d8a-137">**Contraseña**</span><span class="sxs-lookup"><span data-stu-id="62d8a-137">**Password**</span></span> | <span data-ttu-id="62d8a-138">¡password123!</span><span class="sxs-lookup"><span data-stu-id="62d8a-138">password123!</span></span> |
| <span data-ttu-id="62d8a-139">**Confirmar contraseña**</span><span class="sxs-lookup"><span data-stu-id="62d8a-139">**Confirm Password**</span></span> | <span data-ttu-id="62d8a-140">¡password123!</span><span class="sxs-lookup"><span data-stu-id="62d8a-140">password123!</span></span> |
| <span data-ttu-id="62d8a-141">**Correo electrónico**</span><span class="sxs-lookup"><span data-stu-id="62d8a-141">**E-mail**</span></span> | <span data-ttu-id="62d8a-142">(funcionará cualquier dirección de correo electrónico)</span><span class="sxs-lookup"><span data-stu-id="62d8a-142">(any email address will work)</span></span> |
| <span data-ttu-id="62d8a-143">**Pregunta de seguridad**</span><span class="sxs-lookup"><span data-stu-id="62d8a-143">**Security Question**</span></span> | <span data-ttu-id="62d8a-144">(nombre que quiera)</span><span class="sxs-lookup"><span data-stu-id="62d8a-144">(whatever you like)</span></span> |
| <span data-ttu-id="62d8a-145">**Respuesta de seguridad**</span><span class="sxs-lookup"><span data-stu-id="62d8a-145">**Security Answer**</span></span> | <span data-ttu-id="62d8a-146">(nombre que quiera)</span><span class="sxs-lookup"><span data-stu-id="62d8a-146">(whatever you like)</span></span> |

<span data-ttu-id="62d8a-147">*Nota: Por supuesto puede utilizar cualquier contraseña que desea. La configuración de seguridad de la contraseña de forma predeterminada, requiere una contraseña que tiene 7 caracteres y contiene un carácter no alfanumérico.*</span><span class="sxs-lookup"><span data-stu-id="62d8a-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="62d8a-148">Seleccione el rol de administrador para este usuario y haga clic en el botón Crear usuario.</span><span class="sxs-lookup"><span data-stu-id="62d8a-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="62d8a-149">En este punto, debería ver un mensaje que indica que el usuario se creó correctamente.</span><span class="sxs-lookup"><span data-stu-id="62d8a-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="62d8a-150">Ahora puede cerrar la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="62d8a-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="62d8a-151">Autorización basada en roles</span><span class="sxs-lookup"><span data-stu-id="62d8a-151">Role-based Authorization</span></span>

<span data-ttu-id="62d8a-152">Ahora se puede restringir el acceso a la StoreManagerController con el atributo [Authorize], especifica que el usuario debe ser en el rol de administrador para tener acceso a cualquier acción de controlador de la clase.</span><span class="sxs-lookup"><span data-stu-id="62d8a-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="62d8a-153">*Nota: Se puede colocar el atributo [Authorize] en los métodos de acción específica, así como en el nivel de clase de controlador.*</span><span class="sxs-lookup"><span data-stu-id="62d8a-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="62d8a-154">Vaya ahora a /StoreManager aparecerá un cuadro de diálogo de inicio de sesión:</span><span class="sxs-lookup"><span data-stu-id="62d8a-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="62d8a-155">Después de iniciar sesión con la nueva cuenta de administrador, podemos acceder a la pantalla Editar álbum como antes.</span><span class="sxs-lookup"><span data-stu-id="62d8a-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="62d8a-156">[Anterior](mvc-music-store-part-6.md)
> [Siguiente](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="62d8a-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
