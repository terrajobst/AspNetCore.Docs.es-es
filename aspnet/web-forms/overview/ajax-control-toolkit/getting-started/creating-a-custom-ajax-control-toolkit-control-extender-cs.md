---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Crear un personalizado de AJAX Control extensor de Control Toolkit (C#) | Microsoft Docs
author: microsoft
description: Los extensores personalizados le permiten personalizar y ampliar las capacidades de los controles de ASP.NET sin tener que crear nuevas clases.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390958"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="c0c66-103">Crear un extensor de Control del Kit de herramientas de Control personalizado de AJAX (C#)</span><span class="sxs-lookup"><span data-stu-id="c0c66-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="c0c66-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c0c66-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c0c66-105">Los extensores personalizados le permiten personalizar y ampliar las capacidades de los controles de ASP.NET sin tener que crear nuevas clases.</span><span class="sxs-lookup"><span data-stu-id="c0c66-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="c0c66-106">En este tutorial, obtendrá información sobre cómo crear un extensor de control personalizado de AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="c0c66-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="c0c66-107">Crearemos una sencilla pero útil, nuevo extensor que cambia el estado de un botón de deshabilitado a habilitado cuando se escribe texto en un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="c0c66-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="c0c66-108">Después de leer este tutorial, podrá ampliar el Kit de herramientas de AJAX de ASP.NET con sus propios controles extensores.</span><span class="sxs-lookup"><span data-stu-id="c0c66-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="c0c66-109">Puede crear los extensores de control personalizado con Visual Studio o Visual Web Developer (asegúrese de que tiene la versión más reciente de Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="c0c66-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="c0c66-110">Información general del extensor DisabledButton</span><span class="sxs-lookup"><span data-stu-id="c0c66-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="c0c66-111">Nuestro nuevo extensor de control se denomina el extensor DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="c0c66-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="c0c66-112">Este extensor tendrá tres propiedades:</span><span class="sxs-lookup"><span data-stu-id="c0c66-112">This extender will have three properties:</span></span>

- <span data-ttu-id="c0c66-113">TargetControlID - el cuadro de texto que se extiende el control.</span><span class="sxs-lookup"><span data-stu-id="c0c66-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="c0c66-114">TargetButtonIID - el botón que se ha deshabilitado o habilitado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="c0c66-115">DisabledText - el texto que se muestra inicialmente en el botón.</span><span class="sxs-lookup"><span data-stu-id="c0c66-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="c0c66-116">Cuando empiece a escribir, el botón muestra el valor de la propiedad de texto del botón.</span><span class="sxs-lookup"><span data-stu-id="c0c66-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="c0c66-117">Enlazar el extensor DisabledButton a un control TextBox y Button.</span><span class="sxs-lookup"><span data-stu-id="c0c66-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="c0c66-118">Antes de escribir cualquier texto, el botón está deshabilitado y el TextBox y Button este aspecto:</span><span class="sxs-lookup"><span data-stu-id="c0c66-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="c0c66-119">([Haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="c0c66-120">Después de que comience a escribir texto, el botón está habilitado y el TextBox y Button este aspecto:</span><span class="sxs-lookup"><span data-stu-id="c0c66-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="c0c66-121">([Haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="c0c66-122">Para crear el extensor de control, es necesario crear los tres archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c0c66-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="c0c66-123">DisabledButtonExtender.cs: este archivo es la clase de control de servidor que administrará la creación del dispositivo extender y permiten establecer las propiedades en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="c0c66-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="c0c66-124">También define las propiedades que se pueden establecer en el dispositivo extender.</span><span class="sxs-lookup"><span data-stu-id="c0c66-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="c0c66-125">Estas propiedades son accesibles a través del código y en tiempo de diseño y coinciden con las propiedades definidas en el archivo DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="c0c66-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="c0c66-126">DisabledButtonBehavior.js--Este archivo es donde agregará todos de la lógica de script de cliente.</span><span class="sxs-lookup"><span data-stu-id="c0c66-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="c0c66-127">DisabledButtonDesigner.cs - esta clase habilita la funcionalidad de tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="c0c66-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="c0c66-128">Necesita esta clase si desea que el extensor de control para que funcione correctamente con el Diseñador de Visual Studio o Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="c0c66-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="c0c66-129">Por lo que un extensor de control consta de un control de servidor, un comportamiento del lado cliente y una clase de diseñador del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="c0c66-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="c0c66-130">Aprenda a crear los tres de estos archivos en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="c0c66-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="c0c66-131">Crear el proyecto y el sitio Web de extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="c0c66-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="c0c66-132">El primer paso es crear un proyecto de biblioteca de clases y el sitio Web en Visual Studio o Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="c0c66-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="c0c66-133">Nos ll crear el extensor personalizado en el proyecto de biblioteca de clases y probar el extensor personalizado en el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="c0c66-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="c0c66-134">Permiten s iniciar con el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="c0c66-134">Let�s start with the website.</span></span> <span data-ttu-id="c0c66-135">Siga estos pasos para crear el sitio Web:</span><span class="sxs-lookup"><span data-stu-id="c0c66-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="c0c66-136">Seleccione la opción de menú **archivo, nuevo sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="c0c66-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="c0c66-137">Seleccione el **sitio Web de ASP.NET** plantilla.</span><span class="sxs-lookup"><span data-stu-id="c0c66-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="c0c66-138">Nombre del nuevo sitio Web *Website1*.</span><span class="sxs-lookup"><span data-stu-id="c0c66-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="c0c66-139">Haga clic en el **Aceptar** botón.</span><span class="sxs-lookup"><span data-stu-id="c0c66-139">Click the **OK** button.</span></span>

<span data-ttu-id="c0c66-140">A continuación, se debe crear el proyecto de biblioteca de clases que contendrá el código para el extensor de control:</span><span class="sxs-lookup"><span data-stu-id="c0c66-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="c0c66-141">Seleccione la opción de menú **nuevo proyecto de archivos, agregar,**.</span><span class="sxs-lookup"><span data-stu-id="c0c66-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="c0c66-142">Seleccione el **biblioteca de clases** plantilla.</span><span class="sxs-lookup"><span data-stu-id="c0c66-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="c0c66-143">Nombre de la nueva biblioteca de clases con el nombre **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="c0c66-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="c0c66-144">Haga clic en el **Aceptar** botón.</span><span class="sxs-lookup"><span data-stu-id="c0c66-144">Click the **OK** button.</span></span>

<span data-ttu-id="c0c66-145">Después de completar estos pasos, la ventana Explorador de soluciones debería parecerse a la figura 1.</span><span class="sxs-lookup"><span data-stu-id="c0c66-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="c0c66-146">[![Solución de proyecto de biblioteca de clase y de sitio Web](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c0c66-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="c0c66-147">**Figura 01**: solución de proyecto de biblioteca de clase y de sitio Web ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="c0c66-148">A continuación, deberá agregar todas las referencias de ensamblado necesarias para el proyecto de biblioteca de clases:</span><span class="sxs-lookup"><span data-stu-id="c0c66-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="c0c66-149">Haga clic en el proyecto CustomExtenders y seleccione la opción de menú **Agregar referencia**.</span><span class="sxs-lookup"><span data-stu-id="c0c66-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="c0c66-150">Seleccione la pestaña. NET.</span><span class="sxs-lookup"><span data-stu-id="c0c66-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="c0c66-151">Agregue referencias a los siguientes ensamblados:</span><span class="sxs-lookup"><span data-stu-id="c0c66-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="c0c66-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="c0c66-152">System.Web.dll</span></span>
    2. <span data-ttu-id="c0c66-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="c0c66-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="c0c66-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="c0c66-154">System.Design.dll</span></span>
    4. <span data-ttu-id="c0c66-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="c0c66-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="c0c66-156">Seleccione la pestaña Examinar.</span><span class="sxs-lookup"><span data-stu-id="c0c66-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="c0c66-157">Agregue una referencia al ensamblado AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="c0c66-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="c0c66-158">Este ensamblado se encuentra en la carpeta donde descargó el AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="c0c66-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="c0c66-159">Después de completar estos pasos, la carpeta de referencias del proyecto de biblioteca de clases debería parecerse a la figura 2.</span><span class="sxs-lookup"><span data-stu-id="c0c66-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="c0c66-160">[![Carpeta de referencias con las referencias necesarias](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="c0c66-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="c0c66-161">**Figura 02**: carpeta de referencias con las referencias necesarias ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="c0c66-162">Crear el extensor de Control personalizado</span><span class="sxs-lookup"><span data-stu-id="c0c66-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="c0c66-163">Ahora que tenemos nuestra biblioteca de clases, podemos comenzar a crear nuestro control extensor.</span><span class="sxs-lookup"><span data-stu-id="c0c66-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="c0c66-164">Permiten s comenzar con el nivel más básico de una clase de control extensor personalizado (consulte el listado 1).</span><span class="sxs-lookup"><span data-stu-id="c0c66-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="c0c66-165">**Listado 1 - MyCustomExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="c0c66-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="c0c66-166">Hay varias cosas que observará acerca de la clase del extensor de control en el listado 1.</span><span class="sxs-lookup"><span data-stu-id="c0c66-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="c0c66-167">En primer lugar, tenga en cuenta que la clase hereda de la clase ExtenderControlBase base.</span><span class="sxs-lookup"><span data-stu-id="c0c66-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="c0c66-168">Todos los controles extensores de AJAX Control Toolkit se derivan de esta clase base.</span><span class="sxs-lookup"><span data-stu-id="c0c66-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="c0c66-169">Por ejemplo, la clase base incluye la propiedad TargetID que es una propiedad obligatoria de cada extensor de control.</span><span class="sxs-lookup"><span data-stu-id="c0c66-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="c0c66-170">A continuación, observe que la clase incluye los siguientes dos atributos relacionados con el script de cliente:</span><span class="sxs-lookup"><span data-stu-id="c0c66-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="c0c66-171">WebResource - hace que un archivo se incluye como un recurso incrustado en un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="c0c66-172">ClientScriptResource - hace que un recurso de script que deben recuperarse de un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="c0c66-173">El atributo WebResource se utiliza para incrustar el archivo MyControlBehavior.js JavaScript en el ensamblado cuando se compila el extensor personalizado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="c0c66-174">El atributo ClientScriptResource se usa para recuperar el script MyControlBehavior.js desde el ensamblado cuando se usa el extensor personalizado en una página web.</span><span class="sxs-lookup"><span data-stu-id="c0c66-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="c0c66-175">En el orden de los atributos de WebResource y ClientScriptResource funcione, debe compilar el archivo JavaScript como un recurso incrustado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="c0c66-176">Seleccione el archivo en la ventana Explorador de soluciones, abra la hoja de propiedades y asigne el valor *recurso incrustado* a la **acción de compilación** propiedad.</span><span class="sxs-lookup"><span data-stu-id="c0c66-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="c0c66-177">Tenga en cuenta que el extensor de control también incluye un atributo TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="c0c66-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="c0c66-178">Este atributo se utiliza para especificar el tipo de control que se extiende por el extensor de control.</span><span class="sxs-lookup"><span data-stu-id="c0c66-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="c0c66-179">En el caso de la lista 1, se usa el extensor de control para ampliar un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="c0c66-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="c0c66-180">Por último, tenga en cuenta que el extensor incluye una propiedad denominada MyProperty.</span><span class="sxs-lookup"><span data-stu-id="c0c66-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="c0c66-181">La propiedad se marca con el atributo ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="c0c66-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="c0c66-182">Los métodos GetPropertyValue() y SetPropertyValue() se utilizan para pasar el valor de propiedad desde el extensor de control de servidor para el comportamiento del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c0c66-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="c0c66-183">Permiten s continúe e implemente el código para el extensor DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="c0c66-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="c0c66-184">El código para este extensor puede encontrarse en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="c0c66-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="c0c66-185">**Listado 2 - DisabledButtonExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="c0c66-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="c0c66-186">El extensor DisabledButton en el listado 2 tiene dos propiedades denominadas TargetButtonID y DisabledText.</span><span class="sxs-lookup"><span data-stu-id="c0c66-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="c0c66-187">El IDReferenceProperty aplicado a la propiedad TargetButtonID evita que cualquier cosa que no sea el identificador de un control de botón se asignen a esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="c0c66-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="c0c66-188">Los atributos de WebResource y ClientScriptResource asocian un comportamiento de cliente ubicado en un archivo denominado DisabledButtonBehavior.js con este extensor.</span><span class="sxs-lookup"><span data-stu-id="c0c66-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="c0c66-189">Trataremos este archivo de JavaScript en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="c0c66-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="c0c66-190">Crear el comportamiento del extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="c0c66-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="c0c66-191">El componente de cliente de un extensor de control se denomina un comportamiento.</span><span class="sxs-lookup"><span data-stu-id="c0c66-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="c0c66-192">La lógica real para deshabilitar y habilitar el botón está contenida en el comportamiento de DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="c0c66-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="c0c66-193">El código de JavaScript para el comportamiento se incluye en el listado 3.</span><span class="sxs-lookup"><span data-stu-id="c0c66-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="c0c66-194">**Listado 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="c0c66-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="c0c66-195">El archivo de JavaScript en el listado 3 contiene una clase de cliente denominada DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="c0c66-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="c0c66-196">Esta clase, como su gemelo del lado servidor, incluye dos propiedades denominadas TargetButtonID y obtenga DisabledText que puede tener acceso mediante\_TargetButtonID/set\_TargetButtonID y obtenga\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="c0c66-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="c0c66-197">El método initialize() asocia un controlador de evento keyup con el elemento de destino para el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="c0c66-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="c0c66-198">Cada vez que escriba una letra en el cuadro de texto asociado a este comportamiento, se ejecuta el controlador keyup.</span><span class="sxs-lookup"><span data-stu-id="c0c66-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="c0c66-199">El controlador keyup habilita o deshabilita el botón dependiendo de si el cuadro de texto asociado con el comportamiento contiene cualquier texto.</span><span class="sxs-lookup"><span data-stu-id="c0c66-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="c0c66-200">Recuerde que debe compilar el archivo de JavaScript en el listado 3 como recurso incrustado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="c0c66-201">Seleccione el archivo en la ventana Explorador de soluciones, abra la hoja de propiedades y asigne el valor *recurso incrustado* a la **acción de compilación** propiedad (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="c0c66-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="c0c66-202">Esta opción está disponible en Visual Studio y Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="c0c66-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="c0c66-203">[![Adición de un archivo JavaScript como un recurso incrustado](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="c0c66-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="c0c66-204">**Figura 03**: adición de un archivo JavaScript como un recurso incrustado ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="c0c66-205">Creando el Diseñador de extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="c0c66-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="c0c66-206">Hay una clase último que se debe crear para completar nuestro extensor.</span><span class="sxs-lookup"><span data-stu-id="c0c66-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="c0c66-207">Es necesario crear la clase de diseñador en el listado 4.</span><span class="sxs-lookup"><span data-stu-id="c0c66-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="c0c66-208">Esta clase es necesaria para que el extensor se comporten correctamente con el Diseñador de Visual Studio o Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="c0c66-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="c0c66-209">**Listado 4 - DisabledButtonDesigner.cs**</span><span class="sxs-lookup"><span data-stu-id="c0c66-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="c0c66-210">Asociar el diseñador en el listado 4 con el extensor DisabledButton con el atributo de diseñador. Deberá aplicar el atributo de diseñador a la clase DisabledButtonExtender similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c0c66-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="c0c66-211">Usar el extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="c0c66-211">Using the Custom Extender</span></span>

<span data-ttu-id="c0c66-212">Ahora que hemos terminado de crear el extensor de control DisabledButton, es el tiempo para usarlo en nuestro sitio Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c0c66-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="c0c66-213">En primer lugar, necesitamos agregar el extensor personalizado al cuadro de herramientas.</span><span class="sxs-lookup"><span data-stu-id="c0c66-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="c0c66-214">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="c0c66-214">Follow these steps:</span></span>

1. <span data-ttu-id="c0c66-215">Haga doble clic en la página en la ventana Explorador de soluciones, abra una página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c0c66-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="c0c66-216">Haga clic en el cuadro de herramientas y seleccione la opción de menú **elegir elementos**.</span><span class="sxs-lookup"><span data-stu-id="c0c66-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="c0c66-217">En el cuadro de diálogo Elegir elementos del cuadro de herramientas, busque el ensamblado CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="c0c66-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="c0c66-218">Haga clic en el **Aceptar** botón para cerrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="c0c66-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="c0c66-219">Después de completar estos pasos, el extensor de control DisabledButton debe aparecer en el cuadro de herramientas (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="c0c66-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="c0c66-220">[![DisabledButton en el cuadro de herramientas](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="c0c66-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="c0c66-221">**Figura 04**: DisabledButton en el cuadro de herramientas ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="c0c66-222">A continuación, se debe crear una nueva página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c0c66-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="c0c66-223">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="c0c66-223">Follow these steps:</span></span>

1. <span data-ttu-id="c0c66-224">Cree una nueva página ASP.NET denominada ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="c0c66-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="c0c66-225">Arrastre un ScriptManager en la página.</span><span class="sxs-lookup"><span data-stu-id="c0c66-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="c0c66-226">Arrastre un control de cuadro de texto a la página.</span><span class="sxs-lookup"><span data-stu-id="c0c66-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="c0c66-227">Arrastre un control de botón en la página.</span><span class="sxs-lookup"><span data-stu-id="c0c66-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="c0c66-228">En la ventana Propiedades, cambie la propiedad de Id. del botón en el valor <em>btnSave</em> y la propiedad de texto en el valor *guardar\**.</span><span class="sxs-lookup"><span data-stu-id="c0c66-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="c0c66-229">Hemos creado una página con un control ASP.NET TextBox y Button estándar.</span><span class="sxs-lookup"><span data-stu-id="c0c66-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="c0c66-230">A continuación, necesitamos ampliar el control de cuadro de texto con el extensor DisabledButton:</span><span class="sxs-lookup"><span data-stu-id="c0c66-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="c0c66-231">Seleccione el **Agregar extensor** opción para abrir el cuadro de diálogo del Asistente para Extender la tarea (consulte la figura 5).</span><span class="sxs-lookup"><span data-stu-id="c0c66-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="c0c66-232">Tenga en cuenta que el cuadro de diálogo incluye el extensor DisabledButton personalizado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="c0c66-233">Seleccione el extensor DisabledButton y haga clic en el **Aceptar** botón.</span><span class="sxs-lookup"><span data-stu-id="c0c66-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="c0c66-234">[![El cuadro de diálogo del Asistente para Extender](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="c0c66-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="c0c66-235">**Figura 05**: cuadro de diálogo del Asistente de extensor ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="c0c66-236">Por último, podemos establecer las propiedades del extensor DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="c0c66-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="c0c66-237">Puede modificar las propiedades del extensor DisabledButton modificando las propiedades del control de cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="c0c66-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="c0c66-238">Seleccione el cuadro de texto en el diseñador.</span><span class="sxs-lookup"><span data-stu-id="c0c66-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="c0c66-239">En la ventana Propiedades, expanda el nodo de extensores (consulte la figura 6).</span><span class="sxs-lookup"><span data-stu-id="c0c66-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="c0c66-240">Asigne el valor *guardar* a la propiedad DisabledText y el valor *btnSave* a la propiedad TargetButtonID.</span><span class="sxs-lookup"><span data-stu-id="c0c66-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="c0c66-241">[![Establecer las propiedades de extensor](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="c0c66-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="c0c66-242">**Figura 06**: establecer las propiedades de extensor ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="c0c66-243">Al ejecutar la página (presionando F5), el control de botón está deshabilitado inicialmente.</span><span class="sxs-lookup"><span data-stu-id="c0c66-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="c0c66-244">Tan pronto como se comienza a escribir texto en el cuadro de texto, el botón de control está habilitado (consulte la figura 7).</span><span class="sxs-lookup"><span data-stu-id="c0c66-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="c0c66-245">[![El extensor DisabledButton en acción](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="c0c66-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="c0c66-246">**Figura 07**: DisabledButton el extensor en acción ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="c0c66-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="c0c66-247">Resumen</span><span class="sxs-lookup"><span data-stu-id="c0c66-247">Summary</span></span>

<span data-ttu-id="c0c66-248">El objetivo de este tutorial era explicar cómo puede ampliar el AJAX Control Toolkit con controles de extensor personalizado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="c0c66-249">En este tutorial, creamos un extensor de control DisabledButton simple.</span><span class="sxs-lookup"><span data-stu-id="c0c66-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="c0c66-250">Este extensor se implementa mediante la creación de una clase DisabledButtonExtender, un comportamiento DisabledButtonBehavior JavaScript y una clase DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="c0c66-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="c0c66-251">Siga un conjunto similar de pasos cada vez que cree un extensor de control personalizado.</span><span class="sxs-lookup"><span data-stu-id="c0c66-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c0c66-252">[Anterior](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Siguiente](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c0c66-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
