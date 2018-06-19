---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET deniega el acceso a directorios IIS | Documentos de Microsoft
author: rick-anderson
description: Estas notas del producto describen lo que debe hacer si una solicitud para la aplicación ASP.NET devuelve el error "acceso denegado al directorio DirectoryName. No se pudo s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "30070776"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="ba149-104">Denegado el acceso a directorios IIS de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ba149-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="ba149-105">Estas notas del producto describen lo que debe hacer si una solicitud para la aplicación ASP.NET devuelve el error "acceso denegado a *DirectoryName* directory.</span><span class="sxs-lookup"><span data-stu-id="ba149-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="ba149-106">No se pudo iniciar la supervisión de cambios en el directorio."</span><span class="sxs-lookup"><span data-stu-id="ba149-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="ba149-107">Se aplica a ASP.NET 1.0 y 1.1 de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ba149-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="ba149-108">RTM de ASP.NET V1 ahora se ejecuta con un menor con privilegios de cuenta de windows - registrado como la cuenta "ASPNET" en un equipo local.</span><span class="sxs-lookup"><span data-stu-id="ba149-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="ba149-109">En algunos sistemas bloqueados, esta cuenta puede no predeterminada tener lectura acceso de seguridad para los directorios de contenido de un sitio Web, el directorio raíz de la aplicación o el directorio raíz del sitio web.</span><span class="sxs-lookup"><span data-stu-id="ba149-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="ba149-110">En este caso le notificará el siguiente error cuando se solicitan páginas desde una aplicación web determinada:</span><span class="sxs-lookup"><span data-stu-id="ba149-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="ba149-111">Para solucionar este problema debe cambiar los permisos de seguridad en los directorios adecuados.</span><span class="sxs-lookup"><span data-stu-id="ba149-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="ba149-112">En concreto, ASP.NET requiere leer, ejecutar y lista de acceso de la cuenta ASPNET para la raíz del sitio web (por ejemplo: c:\inetpub\wwwroot o en cualquier directorio de sitio alternativo que se ha configurado en IIS), el directorio de contenido y el directorio raíz de la aplicación con el fin de supervisar los cambios del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="ba149-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="ba149-113">La raíz de la aplicación se corresponde con la ruta de acceso de la carpeta asociada con el directorio virtual de la aplicación en la herramienta de administración de IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="ba149-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="ba149-114">Por ejemplo, considere la siguiente jerarquía de la aplicación en la carpeta wwwroot.</span><span class="sxs-lookup"><span data-stu-id="ba149-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="ba149-115">En este ejemplo, la cuenta ASPNET necesita los permisos de lectura definidos anteriormente para el contenido en el myapp y el directorio wwwroot.</span><span class="sxs-lookup"><span data-stu-id="ba149-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="ba149-116">Una única ACL heredada en la carpeta raíz también opcionalmente sirve para ambos directorios si está anidados.</span><span class="sxs-lookup"><span data-stu-id="ba149-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="ba149-117">Para agregar permisos para un directorio, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ba149-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="ba149-118">Mediante el Explorador de Windows, navegue hasta el directorio</span><span class="sxs-lookup"><span data-stu-id="ba149-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="ba149-119">Haga clic con el botón secundario en la carpeta del directorio y elija "Propiedades"</span><span class="sxs-lookup"><span data-stu-id="ba149-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="ba149-120">Vaya a la pestaña "Seguridad" en el cuadro de diálogo de propiedades</span><span class="sxs-lookup"><span data-stu-id="ba149-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="ba149-121">Haga clic en el botón "Agregar" y escriba el nombre del equipo seguido del nombre de la cuenta ASPNET.</span><span class="sxs-lookup"><span data-stu-id="ba149-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="ba149-122">Por ejemplo, en un equipo denominado "webdev", ¿escriba webdev\ASPNET y haga clic en "Aceptar".</span><span class="sxs-lookup"><span data-stu-id="ba149-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="ba149-123">Asegúrese de que la cuenta ASPNET tiene la "lectura &amp; Execute", "Mostrar el contenido de la carpeta" y "Lectura" casillas de verificación activadas.</span><span class="sxs-lookup"><span data-stu-id="ba149-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="ba149-124">Haga clic en Aceptar para cerrar el cuadro de diálogo y guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="ba149-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="ba149-125">Si lo desea, estos cambios se pueden automatizar mediante secuencias de comandos o la herramienta "cacls.exe" que se incluye con Windows.</span><span class="sxs-lookup"><span data-stu-id="ba149-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="ba149-126">Para obtener más información sobre la cuenta ASPNET, consulte el [documento de preguntas más frecuentes sobre](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="ba149-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="ba149-127">Si una determinada aplicación web se basa en tener escritura o modificar permisos para un archivo o carpeta determinado, esto se puede conceder el mismo procedimiento y comprobando las casillas de verificación de "Escritura" o "Modificar".</span><span class="sxs-lookup"><span data-stu-id="ba149-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="ba149-128">En equipos que permiten a todos los usuarios o el grupo a los usuarios acceso de lectura a estos directorios (que es la configuración predeterminada), no se encuentre ningún problema y los pasos anteriores no será necesarios.</span><span class="sxs-lookup"><span data-stu-id="ba149-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
