---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210111"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="6fd06-101">Procedimiento para generar y ejecutar el ejemplo de datos de usuario segura</span><span class="sxs-lookup"><span data-stu-id="6fd06-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="6fd06-102">Establezca la contraseña con la herramienta Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="6fd06-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="6fd06-103">Actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="6fd06-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="6fd06-104">Habilitar HTTPS en el proyecto</span><span class="sxs-lookup"><span data-stu-id="6fd06-104">Enable HTTPS in the project</span></span>
