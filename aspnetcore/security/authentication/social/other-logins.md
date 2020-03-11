---
title: Proveedores de autenticación OAuth externos
author: rick-anderson
description: Detectar proveedores de autenticación OAuth externos que funcionan con aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/otherlogins
ms.openlocfilehash: 2bc9a11d0a46e54b4206f846d187b8c1cc954f89
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654461"
---
# <a name="external-oauth-authentication-providers"></a><span data-ttu-id="1a2eb-103">Proveedores de autenticación OAuth externos</span><span class="sxs-lookup"><span data-stu-id="1a2eb-103">External OAuth authentication providers</span></span>

<span data-ttu-id="1a2eb-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd)y [Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="1a2eb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="1a2eb-105">La lista siguiente incluye proveedores de autenticación OAuth externos comunes que funcionan con aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a2eb-105">The following list includes common external OAuth authentication providers that work with ASP.NET Core apps.</span></span> <span data-ttu-id="1a2eb-106">Los paquetes NuGet de terceros, como los mantenidos por [ASPNET-entrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), se pueden usar para complementar los proveedores de autenticación implementados por el equipo de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="1a2eb-106">Third-party NuGet packages, such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), can be used to complement the authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="1a2eb-107">[LinkedIn](https://www.linkedin.com/developer/apps) ([instrucciones](https://developer.linkedin.com/docs/oauth2))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-107">[LinkedIn](https://www.linkedin.com/developer/apps) ([Instructions](https://developer.linkedin.com/docs/oauth2))</span></span>

* <span data-ttu-id="1a2eb-108">[Instagram](https://www.instagram.com/developer/register/) ([instrucciones](https://www.instagram.com/developer/authentication/))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-108">[Instagram](https://www.instagram.com/developer/register/) ([Instructions](https://www.instagram.com/developer/authentication/))</span></span>

* <span data-ttu-id="1a2eb-109">[Reddit](https://www.reddit.com/login?dest=https%3A%2F%2F www.reddit.com%2Fprefs%2Fapps) ([instrucciones](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-109">[Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([Instructions](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))</span></span>

* <span data-ttu-id="1a2eb-110">[GitHub](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([instrucciones](https://developer.github.com/v3/oauth/))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-110">[Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([Instructions](https://developer.github.com/v3/oauth/))</span></span>

* <span data-ttu-id="1a2eb-111">[Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([instrucciones](https://developer.yahoo.com/bbauth/user.html))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-111">[Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([Instructions](https://developer.yahoo.com/bbauth/user.html))</span></span>

* <span data-ttu-id="1a2eb-112">[Tumblr](https://www.tumblr.com/oauth/apps) ([instrucciones](https://www.tumblr.com/docs/api/v2#auth))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-112">[Tumblr](https://www.tumblr.com/oauth/apps) ([Instructions](https://www.tumblr.com/docs/api/v2#auth))</span></span>

* <span data-ttu-id="1a2eb-113">[Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([instrucciones](https://developers.pinterest.com/docs/api/overview/?))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-113">[Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([Instructions](https://developers.pinterest.com/docs/api/overview/?))</span></span>

* <span data-ttu-id="1a2eb-114">[Pocket](https://getpocket.com/developer/apps/new) ([instrucciones](https://getpocket.com/developer/docs/authentication))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-114">[Pocket](https://getpocket.com/developer/apps/new) ([Instructions](https://getpocket.com/developer/docs/authentication))</span></span>

* <span data-ttu-id="1a2eb-115">[Flickr](https://www.flickr.com/services/apps/create) ([instrucciones](https://www.flickr.com/services/api/auth.oauth.html))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-115">[Flickr](https://www.flickr.com/services/apps/create) ([Instructions](https://www.flickr.com/services/api/auth.oauth.html))</span></span>

* <span data-ttu-id="1a2eb-116">[Dribble](https://dribbble.com/signup) ([instrucciones](https://developer.dribbble.com/v1/oauth/))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-116">[Dribble](https://dribbble.com/signup) ([Instructions](https://developer.dribbble.com/v1/oauth/))</span></span>

* <span data-ttu-id="1a2eb-117">[Vimeo](https://vimeo.com/join) ([instrucciones](https://developer.vimeo.com/api/authentication))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-117">[Vimeo](https://vimeo.com/join) ([Instructions](https://developer.vimeo.com/api/authentication))</span></span>

* <span data-ttu-id="1a2eb-118">[Soundcloud](https://soundcloud.com/you/apps/new) ([instrucciones](https://developers.soundcloud.com/blog/we-love-oauth-2))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-118">[SoundCloud](https://soundcloud.com/you/apps/new) ([Instructions](https://developers.soundcloud.com/blog/we-love-oauth-2))</span></span>

* <span data-ttu-id="1a2eb-119">[VK](https://vk.com/apps?act=manage) ([instrucciones](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))</span><span class="sxs-lookup"><span data-stu-id="1a2eb-119">[VK](https://vk.com/apps?act=manage) ([Instructions](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))</span></span>

[!INCLUDE[Multiple authentication providers](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
