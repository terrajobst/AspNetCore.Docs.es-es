---
title: 'Pasos siguientes: DevOps con ASP.NET Core y Azure'
author: CamSoper
description: Adicionales de las rutas de aprendizaje de DevOps con ASP.NET Core y Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892012"
---
# <a name="next-steps"></a>Pasos siguientes

En esta guía, ha creado una canalización de DevOps para una aplicación de ejemplo de ASP.NET Core. ¡Enhorabuena! Esperamos que haya disfrutado de aprendizaje publicar aplicaciones web ASP.NET Core en Azure App Service y automatizar la integración continua de los cambios.

Más allá de hospedaje web y DevOps, Azure tiene una amplia gama de servicios de plataforma como-servicio (PaaS) útiles para los desarrolladores de ASP.NET Core. En esta sección se ofrece una breve descripción de algunos de los servicios más usados.

## <a name="storage-and-databases"></a>Almacenamiento y bases de datos

[Caché en Redis](/azure/redis-cache/) está disponible como un servicio de almacenamiento en caché de datos de alto rendimiento y baja latencia. Se puede usar para el almacenamiento en caché de resultados de la página, reducir las solicitudes de base de datos y proporcionar el estado de sesión de ASP.NET Core en varias instancias de una aplicación.

[Almacenamiento de Azure](/azure/storage/) es escalable a gran escala en la nube el almacenamiento de Azure. Los desarrolladores pueden aprovechar las ventajas de [Queue Storage](/azure/storage/queues/storage-queues-introduction) para poner en cola confiable de mensajes, y [Table Storage](/azure/storage/tables/table-storage-overview) es un almacén de pares clave-valor NoSQL diseñado para agiliza el desarrollo con conjuntos de datos semiestructurados masivos.

[La base de datos de SQL Azure](/azure/sql-database/) proporciona la funcionalidad de la base de datos relacional como servicio mediante el motor de Microsoft SQL Server.

[COSMOS DB](/azure/cosmos-db/) servicio de base de datos NoSQL de varios modelo distribuido globalmente. Varias API están disponibles, incluidas las API de SQL (anteriormente denominado DocumentDB), Cassandra y MongoDB.

## <a name="identity"></a>identidad

[Azure Active Directory](/azure/active-directory/) y [Azure Active Directory B2C](/azure/active-directory-b2c/) son ambos servicios de identidad. Azure Active Directory está diseñado para escenarios empresariales y permite la colaboración de Azure AD B2B (negocio a negocio), mientras que Azure Active Directory B2C es el previsto de los escenarios de cliente de empresa, incluidos los inicio de sesión de red social.

## <a name="mobile"></a>Móvil

[Notification Hubs](/azure/notification-hubs/) es un motor de notificaciones de inserción multiplataforma y escalable para enviar rápidamente millones de mensajes a aplicaciones que se ejecutan en distintos tipos de dispositivos.

## <a name="web-infrastructure"></a>Infraestructura Web

[Azure Container Service](/azure/aks/) administra el entorno hospedado de Kubernetes, lo que rápida y fácil de implementar y administrar aplicaciones en contenedores sin tener conocimientos de orquestación de contenedores.

[Azure Search](/azure/search/) se usa para crear una solución de búsqueda empresarial sobre contenido privado y heterogéneo.

[Service Fabric](/azure/service-fabric/) es una plataforma de sistemas distribuidos que facilita la tarea empaquetar, implementar y administrar escalable y confiable microservicios y contenedores.
