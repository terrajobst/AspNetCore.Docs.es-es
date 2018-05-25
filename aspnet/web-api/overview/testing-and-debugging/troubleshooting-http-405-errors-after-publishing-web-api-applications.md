---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Solución de problemas de HTTP 405 errores después de la publicación de Web API 2 aplicaciones | Documentos de Microsoft
author: rmcmurray
description: Este tutorial describe cómo solucionar errores de HTTP 405 después de publicar una aplicación de API Web en un servidor web de producción.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: b87ae7420e1295030e90c30e97b1e331413ce263
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Solución de problemas de HTTP 405 errores después de la publicación de Web API 2 aplicaciones
====================
por [Robert McMurray](https://github.com/rmcmurray)

> Este tutorial describe cómo solucionar errores de HTTP 405 después de publicar una aplicación de API Web en un servidor web de producción.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (versión 7 o posterior)
> - [Web API](../../index.md) (versión 1 o 2)


Las aplicaciones de API Web normalmente utilizan varios verbos HTTP comunes: GET, POST, PUT, DELETE y a veces la revisión. Dicho esto, los desarrolladores quizás se enfrente a situaciones donde se implementan los verbos por otro módulo IIS en su servidor de producción, lo que conduce a una situación donde un controlador de API Web que funciona correctamente en Visual Studio o en un servidor de desarrollo devolverá un HTTP 405 error cuando se implementa en un servidor de producción. Afortunadamente se fácilmente resuelve este problema, pero la resolución garantiza una explicación de por qué se está produciendo el problema.

## <a name="what-causes-http-405-errors"></a>¿Qué ocasiona HTTP 405 errores

El primer paso para obtener información sobre cómo solucionar problemas de errores de HTTP 405 es comprender lo que significa realmente un error 405 HTTP. El control principal de documentos para HTTP es [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), que define el código de estado HTTP 405 como ***método no permitido***y se describe más el código de estado como una situación donde &quot;el método se especifica en la línea de la solicitud no está permitida para el recurso identificado por el URI de solicitud.&quot; En otras palabras, no se permite el verbo HTTP para la dirección URL específica que se ha solicitado un cliente HTTP.

Como una breve revisión, estos son algunos de los métodos HTTP usados más tal como se define en RFC 2616, RFC 4918 y 5789 RFC:

| Método HTTP | Descripción |
| --- | --- |
| **OBTENER** | Este método se utiliza para recuperar datos de un URI y, probablemente el método HTTP más utilizadas. |
| **HEAD** | Este método es muy similar al método GET, salvo que realmente no recupera los datos desde el URI de solicitud: simplemente recupera el estado de HTTP. |
| **EXPONER** | Este método se utiliza normalmente para enviar datos nuevos para el identificador URI; POST a menudo se usa para enviar datos del formulario. |
| **PUT** | Este método se utiliza normalmente para enviar datos sin procesar para el identificador URI; PUT a menudo se usa para enviar los datos JSON o XML para las aplicaciones Web API. |
| **ELIMINAR** | Este método se utiliza para quitar datos de un URI. |
| **OPCIONES** | Este método suele usarse para recuperar la lista de métodos HTTP admitidos para un URI. |
| **COPIAR MOVER** | Estos dos métodos se usan con WebDAV y su finalidad es autoexplicativo. |
| **MKCOL** | Este método se usa con WebDAV, y se utiliza para crear una colección (por ejemplo, un directorio) en el URI especificado. |
| **PROPFIND PROPPATCH** | Estos dos métodos se usan con WebDAV, y se usan para consultar o establecer las propiedades de un URI. |
| **BLOQUEAR DESBLOQUEAR** | Estos dos métodos se usan con WebDAV, y se utilizan para bloquear o desbloquear el recurso identificado por el URI de solicitud cuando se crean. |
| **REVISIÓN** | Este método se usa para modificar un recurso HTTP existente. |

Cuando uno de estos métodos HTTP se configura para su uso en el servidor, el servidor responderá con el código de estado HTTP y otros datos que son adecuados para la solicitud. (Por ejemplo, un método GET puede recibir un HTTP 200 ***Aceptar*** respuesta y un método PUT podrían recibir un HTTP 201 ***creado*** respuesta.)

Si el método HTTP no está configurado para su uso en el servidor, el servidor responderá con un HTTP 501 ***no implementa*** error.

Sin embargo, cuando un método HTTP está configurado para su uso en el servidor, pero se ha deshabilitado para un URI determinado, el servidor responderá con un HTTP 405 ***método no permitido*** error.

## <a name="example-http-405-error"></a>Ejemplo HTTP 405 Error

El siguiente ejemplo solicitud y respuesta HTTP muestran una situación donde un cliente HTTP está intentando establecer el valor a una aplicación de API Web en un servidor web y el servidor devuelve un error HTTP que indica que el método PUT no está permitido:


Solicitud de HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


Respuesta de HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


En este ejemplo, el cliente HTTP envía una solicitud JSON válida a la dirección URL para una aplicación de API Web en un servidor web, pero el servidor devolvió un mensaje de error HTTP 405, lo que indica que no se permite el método PUT para la dirección URL. En cambio, si el URI de solicitud no coincidía con una ruta para la aplicación de API Web, el servidor devolverá error HTTP 404 ***no se encuentra*** error.

## <a name="resolving-http-405-errors"></a>Resolver HTTP 405 errores

Hay varios motivos por qué no puede permitirse un verbo HTTP específico, pero hay un escenario principal que es la causa principal de este error en IIS: varios controladores se definen para el mismo verbo/método y uno de los controladores está bloqueando el controlador esperado desde procesar la solicitud. Por medio de explicación, IIS procesa controladores desde la primera a la última basados en las entradas de controlador de orden en el archivo applicationHost.config y web.config, donde la primera combinación de coincidencia de ruta de acceso, verbo, recursos, etc., se usará para atender la solicitud.

El ejemplo siguiente es un extracto de un archivo applicationHost.config para un servidor IIS que se devuelve un error 405 HTTP cuando se usa el método PUT para enviar datos a una aplicación de API Web. En este extracto, se definen varios controladores HTTP y cada controlador tiene un conjunto diferente de métodos HTTP para el que se configura: la última entrada de la lista es el controlador de contenido estático, que es el controlador predeterminado que se utiliza después de que los demás controladores han tenido un chanc e para examinar la solicitud:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

En el ejemplo anterior, el controlador de WebDAV y el controlador de direcciones URL sin extensión para ASP.NET (que se utiliza para la API Web) estén claramente definidas para listas independientes de métodos HTTP. Tenga en cuenta que el controlador de DLL de ISAPI se configura para todos los métodos HTTP, aunque esta configuración no necesariamente se producirá un error. Sin embargo, opciones de configuración como este se deben considerar al solucionar problemas de errores de HTTP 405.

En el ejemplo anterior, el controlador de DLL de ISAPI no era el problema; de hecho, el problema no se definió en el archivo applicationHost.config para el servidor IIS, el problema se debe a una entrada que se realizó en el archivo web.config cuando se creó la aplicación de API Web en Visual Studio. El siguiente extracto del archivo web.config de la aplicación muestra la ubicación del problema:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

En este extracto, se vuelve a definir el controlador de direcciones URL sin extensión ASP.NET para incluir métodos HTTP adicionales que se utilizarán con la aplicación de API Web. Sin embargo, puesto que se define un conjunto de métodos HTTP similar para el controlador de WebDAV, se produce un conflicto. En este caso, el controlador de WebDAV es definido y cargado por IIS, incluso aunque WebDAV está deshabilitada para el sitio Web que incluye la aplicación de API Web. Durante el procesamiento de una solicitud PUT de HTTP, IIS llama al módulo de WebDAV, puesto que se define para el verbo PUT. Cuando se llama al módulo de WebDAV, comprueba su configuración y ve que está deshabilitado, por lo que devolverá un HTTP 405 ***método no permitido*** error para cualquier solicitud que se parece a una solicitud de WebDAV. Para resolver este problema, debe quitar WebDAV en la lista de módulos HTTP para el sitio Web donde se define la aplicación de API Web. En el ejemplo siguiente se muestra el aspecto que podría apariencia:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

A menudo, este escenario se encuentra después de que se publica una aplicación desde un entorno de desarrollo en un entorno de producción, y esto se produce porque la lista de controladores y módulos es diferente entre los entornos de desarrollo y producción. Por ejemplo, si está utilizando Visual Studio 2012 o 2013 para desarrollar una aplicación de API Web, IIS Express 8 es el servidor web predeterminado para las pruebas. Este servidor web de desarrollo es una versión reducida de la funcionalidad completa de IIS que se incluye en un producto de servidor, y este servidor de desarrollo de web contiene algunos cambios que se han agregado para escenarios de desarrollo. Por ejemplo, el módulo de WebDAV a menudo se instala en un servidor web de producción que se ejecuta la versión completa de IIS, aunque puede que no sea en la realidad. La versión de desarrollo de IIS (IIS Express), instala el módulo de WebDAV, pero las entradas para el módulo de WebDAV están intencionadamente comentadas, por lo que nunca se carga el módulo de WebDAV en IIS Express, a menos que se modifique específicamente la configuración de IIS Express configuración para agregar funcionalidad de WebDAV a la instalación de IIS Express. Como resultado, la aplicación web funcionen correctamente en el equipo de desarrollo, pero puede encontrar errores de HTTP 405 al publicar su aplicación de API Web en el servidor web de producción.

## <a name="summary"></a>Resumen

HTTP 405 errores se producen cuando un método HTTP no está permitido por un servidor web para una dirección URL solicitada. Esta condición se ve a menudo cuando se haya definido un controlador determinado para un verbo específico, y ese controlador está reemplazando el controlador que se espera para procesar la solicitud.

Si se produce una situación donde recibirá un mensaje de error HTTP 501, lo que significa que la funcionalidad específica que no se ha implementado en el servidor, a menudo esto significa que no hay ningún controlador definido en la configuración de IIS que coincide con la solicitud HTTP, que probablemente indica que algo no se instaló correctamente en el sistema, o algo ha modificado la configuración de IIS para que no hay que ningún controlador definido ese método de compatibilidad con HTTP específico. Para resolver el problema, se debe volver a instalar cualquier aplicación que está intentando utilizar un método HTTP para el que no tiene ningún módulo correspondiente o definiciones de controlador.
