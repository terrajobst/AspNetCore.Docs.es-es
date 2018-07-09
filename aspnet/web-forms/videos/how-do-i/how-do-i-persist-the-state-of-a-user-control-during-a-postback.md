---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo conservar el estado de uno o más objetos en un control de usuario. En primer lugar, se crea un control de usuario que representa el abilit...
ms.author: aspnetcontent
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 7862d83d3df3ca5407b7d8fd465cf42da8e7228a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805183"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[¿Cómo]: mantener el estado de un Control de usuario durante una devolución de datos
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="09f25-104">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="09f25-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="09f25-105">En este vídeo Chris Pels muestra cómo conservar el estado de uno o más objetos en un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="09f25-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="09f25-106">En primer lugar, se crea un control de usuario que representa la capacidad de un usuario especificar los criterios de filtro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="09f25-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="09f25-107">Además, se crea un clase de filtro complementario para almacenar la información del filtro.</span><span class="sxs-lookup"><span data-stu-id="09f25-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="09f25-108">Se agregan varios elementos de interfaz de usuario para el control de filtro junto con algunos métodos y propiedades para almacenar la información del filtro actual en la instancia de la clase de filtro.</span><span class="sxs-lookup"><span data-stu-id="09f25-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="09f25-109">A continuación, la persistencia del control de usuario se implementa mediante el método RegisterRequiresControlState y métodos guardar ni restaurar asociados.</span><span class="sxs-lookup"><span data-stu-id="09f25-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="09f25-110">Estos métodos almacenan la instancia de la clase de filtro y sus datos durante las devoluciones de página.</span><span class="sxs-lookup"><span data-stu-id="09f25-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="09f25-111">Por último, hay una explicación de cómo almacenar varios objetos en la implementación de estado de control.</span><span class="sxs-lookup"><span data-stu-id="09f25-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="09f25-112">&#9654;Vea el vídeo (23 minutos)</span><span class="sxs-lookup"><span data-stu-id="09f25-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
