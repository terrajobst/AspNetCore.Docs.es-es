---
title: Adquisición de bibliotecas del lado cliente en ASP.NET Core con LibMan
author: scottaddie
description: Obtenga información sobre cómo instalar los recursos de bibliotecas del lado cliente en un proyecto de ASP.NET Core mediante Library Manager (LibMan).
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312364"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a>Adquisición de bibliotecas del lado cliente en ASP.NET Core con LibMan

Por [Scott Addie](https://twitter.com/Scott_Addie)

Library Manager (LibMan) es una herramienta ligera para la adquisición de bibliotecas del lado cliente. Asimismo, LibMan permite descargar bibliotecas y marcos populares de sistemas de archivos o [redes de entrega de contenido (CDN)](https://wikipedia.org/wiki/Content_delivery_network). Entre las CDN admitidas se incluyen [CDNJS](https://cdnjs.com/) y [unpkg](https://unpkg.com/#/). Los archivos de biblioteca seleccionados se capturan y se colocan en la ubicación adecuada dentro del proyecto de ASP.NET Core.

## <a name="libman-use-cases"></a>Casos de uso de LibMan

LibMan ofrece las ventajas siguientes:

* Solo se descargan los archivos de biblioteca necesarios.
* No es necesario usar otras herramientas como [Node.js](https://nodejs.org), [npm](https://www.npmjs.com) o [WebPack](https://webpack.js.org) para adquirir un subconjunto de archivos de una biblioteca.
* Los archivos se pueden colocar en una ubicación específica sin tener que usar tareas de compilación ni copiar archivos manualmente.

Para obtener más información sobre las ventajas del LibMan, vea [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s) (Desarrollo web front-end moderno en Visual Studio 2017: segmento LibMan).

LibMan no es un sistema de administración de paquetes. Si ya usa un administrador de paquetes, por ejemplo, npm o [yarn](https://yarnpkg.com), le recomendamos que sigua haciéndolo. LibMan no se ha desarrollado para reemplazar dichas herramientas.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [Repositorio de LibMan en GitHub](https://github.com/aspnet/LibraryManager)
