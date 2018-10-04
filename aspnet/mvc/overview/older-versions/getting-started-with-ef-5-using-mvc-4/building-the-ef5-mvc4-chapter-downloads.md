---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Compilar el capítulo descargas para EF 5 MVC 4 tutoriales | Microsoft Docs
author: Rick-Anderson
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: fa018ea12929efa742a323d96938d372b634fdd7
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577075"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="c74e1-103">Compilar el capítulo descargas para EF 5 MVC 4 tutoriales</span><span class="sxs-lookup"><span data-stu-id="c74e1-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="c74e1-104">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="c74e1-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="c74e1-105">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="c74e1-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="c74e1-106">La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c74e1-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="c74e1-107">Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="c74e1-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="c74e1-108">Creación de las descargas de capítulos</span><span class="sxs-lookup"><span data-stu-id="c74e1-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="c74e1-109">Descargue y descomprima el archivo zip de ejemplo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="c74e1-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="c74e1-110">En el paquete de descarga sin comprimir, encontrará los archivos zip adicionales, uno para la finalización de cada capítulo.</span><span class="sxs-lookup"><span data-stu-id="c74e1-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="c74e1-111">Haga clic con el botón derecho en el archivo zip deseada, haga clic en **propiedades**y haga clic en el **desbloquear** botón.</span><span class="sxs-lookup"><span data-stu-id="c74e1-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="c74e1-112">Descomprima el archivo.</span><span class="sxs-lookup"><span data-stu-id="c74e1-112">Unzip the file.</span></span>
4. <span data-ttu-id="c74e1-113">Haga doble clic en el *CUx.sln* archivo para iniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c74e1-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="c74e1-114">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca**, a continuación, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c74e1-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="c74e1-115">En la consola de administrador de paquetes (PMC), haga clic en **restaurar**.</span><span class="sxs-lookup"><span data-stu-id="c74e1-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="c74e1-116">Salga de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c74e1-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="c74e1-117">Reinicie Visual Studio, abra el archivo de solución que se ha cerrado en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="c74e1-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="c74e1-118">En la consola de administrador de paquetes (PMC), escriba el `Update-Database` comando:</span><span class="sxs-lookup"><span data-stu-id="c74e1-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="c74e1-119">Si recibe el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="c74e1-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="c74e1-120">*El término 'Update-Database' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable. Compruebe la ortografía del nombre, o si incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta e inténtelo de nuevo.*</span><span class="sxs-lookup"><span data-stu-id="c74e1-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="c74e1-121">Salga y reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c74e1-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="c74e1-122">Se ejecutará cada migración, a continuación, se ejecutará el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="c74e1-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="c74e1-123">Ahora puede ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c74e1-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="c74e1-124">Anterior</span><span class="sxs-lookup"><span data-stu-id="c74e1-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
