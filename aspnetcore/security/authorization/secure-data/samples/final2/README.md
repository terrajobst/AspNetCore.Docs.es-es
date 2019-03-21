---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210111"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Procedimiento para generar y ejecutar el ejemplo de datos de usuario segura

* Establezca la contraseña con la herramienta Secret Manager:

  `dotnet user-secrets set SeedUserPW <pw>`

* Actualización de la base de datos:

  `dotnet ef database update`

* Habilitar HTTPS en el proyecto
