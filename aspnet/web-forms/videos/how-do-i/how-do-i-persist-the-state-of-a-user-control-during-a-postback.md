---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: "[¿Cómo puedo]: el estado de un Control de usuario se conservan durante un Postback | Documentos de Microsoft"
author: rick-anderson
description: "En este vídeo Chris Pels muestra cómo guardar el estado de uno o más objetos en un control de usuario. En primer lugar, se crea un control de usuario que representa el abilit..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="7787d-104">[¿Cómo puedo]: conservar el estado de un Control de usuario durante una devolución de datos</span><span class="sxs-lookup"><span data-stu-id="7787d-104">[How Do I]: Persist the State of a User Control During a Postback</span></span>
====================
<span data-ttu-id="7787d-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="7787d-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="7787d-106">En este vídeo Chris Pels muestra cómo guardar el estado de uno o más objetos en un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="7787d-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="7787d-107">En primer lugar, se crea un control de usuario que representa la capacidad de un usuario especificar los criterios de filtro para buscar.</span><span class="sxs-lookup"><span data-stu-id="7787d-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="7787d-108">Además, se crea una clase de filtro complementario para almacenar la información del filtro.</span><span class="sxs-lookup"><span data-stu-id="7787d-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="7787d-109">Se agregan varios elementos de la interfaz de usuario para el control de filtro junto con algunos métodos y propiedades para almacenar la información del filtro actual en la instancia de la clase de filtro.</span><span class="sxs-lookup"><span data-stu-id="7787d-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="7787d-110">A continuación, la persistencia del control de usuario se implementa mediante el método de RegisterRequiresControlState y métodos asociados de guardar ni restaurar.</span><span class="sxs-lookup"><span data-stu-id="7787d-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="7787d-111">Estos métodos almacenan la instancia de la clase de filtro y sus datos durante los postbacks de página.</span><span class="sxs-lookup"><span data-stu-id="7787d-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="7787d-112">Por último, hay una explicación de cómo almacenar varios objetos en la implementación de estado de control.</span><span class="sxs-lookup"><span data-stu-id="7787d-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="7787d-113">&#9654; Vea el vídeo (23 minutos)</span><span class="sxs-lookup"><span data-stu-id="7787d-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
