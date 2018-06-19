---
title: Páginas de ayuda de ASP.NET Core Web API con Swagger/Open API
author: rsuter
description: En este tutorial se proporciona una guía sobre cómo incorporar Swagger para generar documentación y páginas de ayuda para una aplicación de API web.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: e44b491fd5265e12646efa42f12eb0662e287f04
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077341"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a>Páginas de ayuda de ASP.NET Core Web API con Swagger/Open API

Por [Christoph Nienaber](https://twitter.com/zuckerthoben) y [Rico Suter](http://rsuter.com)

Cuando un desarrollador usa una API Web, puede que le resulte complicado comprender sus diversos métodos. [Swagger](https://swagger.io/), también conocido como Open API, resuelve el problema de generar páginas útiles de ayuda y documentación relativas a las API Web. Así, reporta ventajas como una documentación interactiva, la generación de SDK de cliente y la detectabilidad de API.

En este artículo, nos centraremos en las implementaciones de Swagger .NET [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) y [NSwag](https://github.com/RSuter/NSwag):

* **Swashbuckle.AspNetCore** es un proyecto de código abierto para generar documentos de Swagger para las API web de ASP.NET Core.

* **NSwag** es otro proyecto de código abierto que sirve para integrar la [interfaz de usuario de Swagger](https://swagger.io/swagger-ui/) o [ReDoc](https://github.com/Rebilly/ReDoc) en las API Web de ASP.NET Core. Ofrece métodos para generar código de cliente de C# y TypeScript para la API.

## <a name="what-is-swagger--open-api"></a>¿Qué es Swagger/Open API?

Swagger es una especificación independiente del lenguaje que sirve para describir API de [REST](https://en.wikipedia.org/wiki/Representational_state_transfer). El proyecto de Swagger se donó a la [iniciativa OpenAPI](https://www.openapis.org/), donde se ahora conoce como Open API. Ambos nombres se usan indistintamente, aunque se prefiere Open API. Gracias a esta API, tanto los equipos como los personas podrán conocer las funciones de un servicio sin necesidad de obtener acceso directo a la implementación (código fuente, acceso a la red, documentación). Uno de los objetivos consiste en reducir al mínimo la cantidad de trabajo necesario para conectar servicios que no están asociados. Otro es reducir la cantidad de tiempo necesario para documentar un servicio con precisión.

## <a name="swagger-specification-swaggerjson"></a>Especificación de Swagger (swagger.json)

El elemento principal del flujo de Swagger es la especificación de Swagger, que es de forma predeterminada un documento llamado *swagger.json*. Lo genera la cadena de herramientas de Swagger (o las implementaciones de terceros de dicha cadena) en función de su servicio. Describe las funciones de su API y cómo tener acceso a ella con HTTP. Esta especificación también controla la interfaz de usuario de Swagger, y la cadena de herramientas la usa para permitir la detección y generación de código de cliente. Este es un ejemplo de especificación de Swagger (se ha reducido por motivos de brevedad):

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

## <a name="swagger-ui"></a>Interfaz de usuario de Swagger

La [interfaz de usuario de Swagger](https://swagger.io/swagger-ui/) es una interfaz de usuario basada en Internet que proporciona información sobre el servicio por medio de la especificación de Swagger generada. Swashbuckle y NSwag incluyen una versión insertada de la interfaz de usuario de Swagger, de modo que se puede hospedar en una aplicación ASP.NET Core realizando una llamada de registro de middleware. La interfaz de usuario web tiene este aspecto:

![Interfaz de usuario de Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Todos los métodos de acción públicos aplicados a los controladores se pueden probar desde la interfaz de usuario. Haga clic en un nombre de método para expandir la sección. Agregue todos los parámetros necesarios y haga clic en **Try it out!** (¡Pruébelo!).

![Ejemplo de prueba de acción GET de Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> La versión de interfaz de usuario de Swagger usada para las capturas de pantalla es la versión 2. Para obtener un ejemplo de la versión 3, vea el [ejemplo de Petstore](http://petstore.swagger.io/).

## <a name="next-steps"></a>Pasos siguientes

* [Get started with Swashbuckle](xref:tutorials/get-started-with-swashbuckle) (Introducción a Swashbuckle)
* [Get started with NSwag](xref:tutorials/get-started-with-nswag) (Introducción a NSwag)
