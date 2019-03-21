---
ms.openlocfilehash: c82571d3cfa57ccd6e7c83f654f119bdd8991486
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210460"
---
```console
npm run release
```

Este comando da como resultado la entrega de los activos del lado cliente cuando se ejecuta la aplicación. Los recursos se colocan en la carpeta *wwwroot*.

Webpack ha completado las tareas siguientes:

* Purgar el contenido del directorio *wwwroot*.
* Convertir TypeScript en JavaScript, proceso conocido como *transpilación*.
* Alterar el código JavaScript generado para reducir el tamaño del archivo, proceso conocido como *minificación*.
* Copiar los archivos JavaScript, CSS y HTML procesados desde *src* en el directorio *wwwroot*.
* Insertar los elementos siguientes en el archivo *wwwroot/index.html*:
  * Etiqueta `<link>`, que hace referencia al archivo *wwwroot/main.\<hash\>.css*. Esta etiqueta se coloca inmediatamente antes de la etiqueta `</head>` de cierre.
  * Etiqueta `<script>`, que hace referencia al archivo *wwwroot/main.\<hash\>.js* minificado. Esta etiqueta se coloca inmediatamente antes de la etiqueta `</body>` de cierre.
