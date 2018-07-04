---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Solución de problemas HTTP 405 errores después de la publicación Web API 2 aplicaciones | Microsoft Docs
author: rmcmurray
description: Este tutorial describe cómo solucionar errores de HTTP 405 después de publicar una aplicación de API Web a un servidor web de producción.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 8027644e9430d49962e61db21b9e21426eabd136
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366124"
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Solución de problemas HTTP 405 errores después de la publicación Web API 2 aplicaciones
====================
por [Robert McMurray](https://github.com/rmcmurray)

> Este tutorial describe cómo solucionar errores de HTTP 405 después de publicar una aplicación de API Web a un servidor web de producción.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (versión 7 o posterior)
> - [API Web](../../index.md) (versión 1 o 2)


Las aplicaciones de API Web normalmente utilizan varios verbos HTTP comunes: GET, POST, PUT, DELETE y a veces, aplicar revisiones. Dicho esto, los desarrolladores pueden surgir situaciones donde se implementan los verbos por otro módulo IIS en su servidor de producción, lo que conduce a una situación donde se devolverá un controlador de API Web que funciona correctamente en Visual Studio o en un servidor de desarrollo de una HTTP 405 error cuando se implementa en un servidor de producción. Afortunadamente se resuelve fácilmente este problema, pero la resolución lo garantiza. para obtener una explicación de por qué se está produciendo el problema.

## <a name="what-causes-http-405-errors"></a>¿Qué hace que HTTP 405 errores

El primer paso para aprender a problemas de errores de HTTP 405 es comprender qué significa realmente un error HTTP 405. El control principal de documentos para HTTP es [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), que define el código de estado HTTP 405 como ***método no permitido***y permita describir este código de estado como una situación donde &quot;el método se especifica en la línea de solicitud no está permitida para el recurso identificado por el URI de solicitud.&quot; En otras palabras, no se permite el verbo HTTP para la dirección URL específica que se ha solicitado un cliente HTTP.

Repaso breve, estos son algunos de los métodos HTTP usados más tal como se define en RFC 2616, RFC 4918 y RFC 5789:

| Método HTTP | Descripción |
| --- | --- |
| **OBTENER** | Este método se utiliza para recuperar datos de un URI y probablemente el método HTTP más usados. |
| **HEAD** | Este método es muy similar al método GET, excepto que no recupera realmente los datos desde el URI de solicitud: simplemente recupera el estado de HTTP. |
| **EXPONER** | Este método se utiliza normalmente para enviar datos nuevos al URI; POST a menudo se usa para enviar datos de formulario. |
| **PUT** | Este método se utiliza normalmente para enviar datos sin procesar al URI; PUT a menudo se usa para enviar datos JSON o XML a las aplicaciones de API Web. |
| **ELIMINAR** | Este método se usa para quitar los datos de un URI. |
| **OPCIONES** | Este método se utiliza normalmente para recuperar la lista de métodos HTTP admitidos para un URI. |
| **COPIAR MOVER** | Estos dos métodos se usan con WebDAV, y su finalidad no necesita explicación. |
| **MKCOL** | Este método se usa con WebDAV, y se utiliza para crear una colección (por ejemplo, un directorio) en el URI especificado. |
| **PROPFIND PROPPATCH** | Estos dos métodos se usan con WebDAV, y se usan para consultar o establecer las propiedades de un URI. |
| **DESBLOQUEO DEL BLOQUEO** | Estos dos métodos se usan con WebDAV, y se usan para bloquear o desbloquear el recurso identificado por el URI de solicitud cuando se crean. |
| **REVISIÓN** | Este método se utiliza para modificar un recurso existente de HTTP. |

Cuando uno de estos métodos HTTP se configura para su uso en el servidor, el servidor responderá con el código de estado HTTP y otros datos que es adecuados para la solicitud. (Por ejemplo, un método GET puede recibir un HTTP 200 ***Aceptar*** respuesta y un método PUT es posible que reciba un HTTP 201 ***creado*** respuesta.)

Si el método HTTP no está configurado para su uso en el servidor, el servidor responderá con un 501 HTTP ***no implementa*** error.

Sin embargo, cuando un método HTTP está configurado para su uso en el servidor, pero se ha deshabilitado para un URI determinado, el servidor responderá con un HTTP 405 ***método no permitido*** error.

## <a name="example-http-405-error"></a>Ejemplo HTTP 405 Error

El siguiente ejemplo solicitud y respuesta HTTP muestran una situación donde un cliente HTTP está intentando colocar el valor a una aplicación de API Web en un servidor web y el servidor devuelve un error HTTP que indica que el método PUT no está permitido:


Solicitud HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


Respuesta HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


En este ejemplo, el cliente HTTP envía una solicitud JSON válida a la dirección URL para una aplicación de API Web en un servidor web, pero el servidor devolvió un mensaje de error HTTP 405 que indica que el método PUT no se permite para la dirección URL. En cambio, si el URI de solicitud no coincidió con una ruta para la aplicación de API Web, el servidor devolverá error HTTP 404 ***no se encuentra*** error.

## <a name="resolving-http-405-errors"></a>Resolver HTTP 405 errores

Hay varias razones por qué no se puede permitir un verbo HTTP específico, pero no hay un escenario principal que es la causa principal de este error en IIS: varios controladores se definen para el mismo verbo/método y uno de los controladores está bloqueando el controlador esperado procesar la solicitud. Por medio de explicación, IIS procesa los controladores de primera a la última en función de las entradas de controlador del orden en el archivo applicationHost.config y web.config, donde la combinación primera coincidencia de ruta de acceso, verbo, recursos, etc., se usará para atender la solicitud.

El ejemplo siguiente es un extracto de un archivo applicationHost.config para un servidor IIS que se devuelve un error 405 HTTP cuando se usa el método PUT para enviar datos a una aplicación de API Web. En este extracto, se definen varios controladores HTTP y cada controlador tiene un conjunto diferente de métodos HTTP para el que está configurado: la última entrada en la lista es el controlador de contenido estático, que es el controlador predeterminado que se usa después de que los otros controladores han tenido un chanc e para examinar la solicitud:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

En el ejemplo anterior, el controlador de WebDAV y el controlador de la dirección URL sin extensión de ASP.NET (que se utiliza para API Web) se definen claramente para listas independientes de los métodos HTTP. Tenga en cuenta que el controlador de DLL de ISAPI se configura para todos los métodos HTTP, aunque esta configuración no necesariamente se producirá un error. Sin embargo, opciones de configuración como este se deben considerar al solucionar problemas de errores de HTTP 405.

En el ejemplo anterior, el controlador de DLL de ISAPI no era el problema; de hecho, el problema no se definió en el archivo applicationHost.config para el servidor IIS: el problema fue causado por una entrada que se realizó en el archivo web.config cuando se creó la aplicación de API Web en Visual Studio. El siguiente extracto del archivo web.config de la aplicación muestra la ubicación del problema:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

En este extracto, el controlador de la dirección URL sin extensión de ASP.NET se ha redefinido para incluir métodos HTTP adicionales que se usará con la aplicación de API Web. Sin embargo, puesto que se define un conjunto de métodos HTTP similar para el controlador de WebDAV, se produce un conflicto. En este caso, el controlador de WebDAV definido y cargado por IIS, incluso si WebDAV está deshabilitado para el sitio Web que incluye la aplicación de API Web. Durante el procesamiento de una solicitud HTTP PUT, IIS llama al módulo de WebDAV, puesto que está definido para el verbo PUT. Cuando se llama al módulo de WebDAV, comprueba la configuración y verá que está deshabilitado, por lo que devolverá un HTTP 405 ***método no permitido*** error para cualquier solicitud que se parece a una solicitud de WebDAV. Para resolver este problema, debe quitar WebDAV en la lista de módulos HTTP para el sitio Web donde se define la aplicación Web API. El ejemplo siguiente muestra lo que es posible que el aspecto:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

A menudo, este escenario se encuentra después de una aplicación se publica en un entorno de desarrollo en un entorno de producción, y esto se produce porque la lista de controladores y módulos es diferente entre los entornos de desarrollo y producción. Por ejemplo, si utiliza Visual Studio 2012 o 2013 para desarrollar una aplicación de API Web, IIS Express 8 es el servidor web predeterminado para las pruebas. Este servidor de desarrollo web es una versión reducida de la funcionalidad completa de IIS que se incluye en un producto de servidor, y este servidor web de desarrollo contiene algunos cambios que se agregaron para escenarios de desarrollo. Por ejemplo, el módulo de WebDAV a menudo se instala en un servidor web de producción que se está ejecutando la versión completa de IIS, aunque no es posible en el uso real. La versión de desarrollo de IIS (IIS Express), instala el módulo de WebDAV, pero las entradas para el módulo de WebDAV intencionadamente están marcadas con comentarios, por lo que nunca se carga el módulo de WebDAV en IIS Express, a menos que se modifique específicamente la configuración de IIS Express configuración para agregar funcionalidad de WebDAV para la instalación de IIS Express. Como resultado, la aplicación web puede funcionar correctamente en el equipo de desarrollo, pero que pueden producirse errores de HTTP 405 al publicar su aplicación de API Web en el servidor web de producción.

## <a name="summary"></a>Resumen

HTTP 405 errores se producen cuando un método HTTP no se permite un servidor web para una dirección URL solicitada. Esta condición suele aparecer cuando se ha definido un controlador determinado para un verbo específico, y ese controlador está reemplazando el controlador que necesita para procesar la solicitud.

Si se produce una situación donde recibirá un mensaje de error HTTP 501, lo que significa que no se ha implementado la funcionalidad específica en el servidor, esto suele significar que no hay ningún controlador definido en la configuración de IIS que coincide con la solicitud HTTP, que probablemente indica que algo no se instaló correctamente en el sistema, o algo modificó la configuración de IIS para que haya que ningún controlador definido ese método HTTP específico de soporte técnico. Para resolver este problema, deberá volver a instalar cualquier aplicación que está intentando usar un método HTTP para el que no tiene el módulo correspondiente o definiciones del controlador.
