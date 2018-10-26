---
title: DevOps con ASP.NET Core y Azure | Pasos siguientes
author: CamSoper
description: Una guía que proporciona guías de un extremo a otro sobre cómo crear una canalización de DevOps para una aplicación ASP.NET Core hospedada en Azure.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: b82e7251b507f8d141930673d50722cfaa576db5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089886"
---
# <a name="next-steps"></a><span data-ttu-id="750d7-103">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="750d7-103">Next steps</span></span>

<span data-ttu-id="750d7-104">En esta guía, ha creado una canalización de DevOps para una aplicación de ejemplo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="750d7-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="750d7-105">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="750d7-105">Congratulations!</span></span> <span data-ttu-id="750d7-106">Esperamos que haya disfrutado de aprendizaje publicar aplicaciones web ASP.NET Core en Azure App Service y automatizar la integración continua de los cambios.</span><span class="sxs-lookup"><span data-stu-id="750d7-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="750d7-107">Más allá de hospedaje web y DevOps, Azure tiene una amplia gama de servicios de plataforma como-servicio (PaaS) útiles para los desarrolladores de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="750d7-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="750d7-108">En esta sección se ofrece una breve descripción de algunos de los servicios más usados.</span><span class="sxs-lookup"><span data-stu-id="750d7-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="750d7-109">Almacenamiento y bases de datos</span><span class="sxs-lookup"><span data-stu-id="750d7-109">Storage and databases</span></span>

<span data-ttu-id="750d7-110">[Caché en Redis](/azure/redis-cache/) está disponible como un servicio de almacenamiento en caché de datos de alto rendimiento y baja latencia.</span><span class="sxs-lookup"><span data-stu-id="750d7-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="750d7-111">Se puede usar para el almacenamiento en caché de resultados de la página, reducir las solicitudes de base de datos y proporcionar el estado de sesión de ASP.NET Core en varias instancias de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="750d7-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="750d7-112">[Almacenamiento de Azure](/azure/storage/) es escalable a gran escala en la nube el almacenamiento de Azure.</span><span class="sxs-lookup"><span data-stu-id="750d7-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="750d7-113">Los desarrolladores pueden aprovechar las ventajas de [Queue Storage](/azure/storage/queues/storage-queues-introduction) para poner en cola confiable de mensajes, y [Table Storage](/azure/storage/tables/table-storage-overview) es un almacén de pares clave-valor NoSQL diseñado para agiliza el desarrollo con conjuntos de datos semiestructurados masivos.</span><span class="sxs-lookup"><span data-stu-id="750d7-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="750d7-114">[La base de datos de SQL Azure](/azure/sql-database/) proporciona la funcionalidad de la base de datos relacional como servicio mediante el motor de Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="750d7-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="750d7-115">[COSMOS DB](/azure/cosmos-db/) servicio de base de datos NoSQL de varios modelo distribuido globalmente.</span><span class="sxs-lookup"><span data-stu-id="750d7-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="750d7-116">Varias API están disponibles, incluidas las API de SQL (anteriormente denominado DocumentDB), Cassandra y MongoDB.</span><span class="sxs-lookup"><span data-stu-id="750d7-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="750d7-117">identidad</span><span class="sxs-lookup"><span data-stu-id="750d7-117">Identity</span></span>

<span data-ttu-id="750d7-118">[Azure Active Directory](/azure/active-directory/) y [Azure Active Directory B2C](/azure/active-directory-b2c/) son ambos servicios de identidad.</span><span class="sxs-lookup"><span data-stu-id="750d7-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="750d7-119">Azure Active Directory está diseñado para escenarios empresariales y permite la colaboración de Azure AD B2B (negocio a negocio), mientras que Azure Active Directory B2C es el previsto de los escenarios de cliente de empresa, incluidos los inicio de sesión de red social.</span><span class="sxs-lookup"><span data-stu-id="750d7-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="750d7-120">Móvil</span><span class="sxs-lookup"><span data-stu-id="750d7-120">Mobile</span></span>

<span data-ttu-id="750d7-121">[Notification Hubs](/azure/notification-hubs/) es un motor de notificaciones de inserción multiplataforma y escalable para enviar rápidamente millones de mensajes a aplicaciones que se ejecutan en distintos tipos de dispositivos.</span><span class="sxs-lookup"><span data-stu-id="750d7-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="750d7-122">Infraestructura Web</span><span class="sxs-lookup"><span data-stu-id="750d7-122">Web infrastructure</span></span>

<span data-ttu-id="750d7-123">[Azure Container Service](/azure/aks/) administra el entorno hospedado de Kubernetes, lo que rápida y fácil de implementar y administrar aplicaciones en contenedores sin tener conocimientos de orquestación de contenedores.</span><span class="sxs-lookup"><span data-stu-id="750d7-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="750d7-124">[Azure Search](/azure/search/) se usa para crear una solución de búsqueda empresarial sobre contenido privado y heterogéneo.</span><span class="sxs-lookup"><span data-stu-id="750d7-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="750d7-125">[Service Fabric](/azure/service-fabric/) es una plataforma de sistemas distribuidos que facilita la tarea empaquetar, implementar y administrar escalable y confiable microservicios y contenedores.</span><span class="sxs-lookup"><span data-stu-id="750d7-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
