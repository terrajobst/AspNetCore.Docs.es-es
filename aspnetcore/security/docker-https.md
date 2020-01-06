---
title: Hospedaje de imágenes de ASP.NET Core con Docker a través de HTTPS
author: rick-anderson
description: Aprenda a hospedar imágenes de ASP.NET Core con Docker a través de HTTPS
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
no-loc:
- Let's Encrypt
uid: security/docker-https
ms.openlocfilehash: 47027033c0b7130f2d38d22c02a54945b2cc31b3
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358918"
---
# <a name="hosting-aspnet-core-images-with-docker-over-https"></a><span data-ttu-id="17c89-103">Hospedaje de imágenes de ASP.NET Core con Docker a través de HTTPS</span><span class="sxs-lookup"><span data-stu-id="17c89-103">Hosting ASP.NET Core images with Docker over HTTPS</span></span>

<span data-ttu-id="17c89-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17c89-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="17c89-105">[De forma predeterminada](/aspnet/core/security/enforcing-ssl)ASP.net Core usa https.</span><span class="sxs-lookup"><span data-stu-id="17c89-105">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="17c89-106">[Https](https://en.wikipedia.org/wiki/HTTPS) se basa en los [certificados](https://en.wikipedia.org/wiki/Public_key_certificate) de confianza, identidad y cifrado.</span><span class="sxs-lookup"><span data-stu-id="17c89-106">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="17c89-107">En este documento se explica cómo ejecutar imágenes de contenedor creadas previamente con HTTPS.</span><span class="sxs-lookup"><span data-stu-id="17c89-107">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="17c89-108">Consulte [desarrollo de aplicaciones ASP.net Core con Docker a través de https](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md) para escenarios de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="17c89-108">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="17c89-109">Este ejemplo requiere [docker 17,06](https://docs.docker.com/release-notes/docker-ce) o posterior del [cliente de Docker](https://www.docker.com/products/docker).</span><span class="sxs-lookup"><span data-stu-id="17c89-109">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17c89-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="17c89-110">Prerequisites</span></span>

<span data-ttu-id="17c89-111">El [SDK de .net Core 2,2](https://www.microsoft.com/net/download) o posterior es necesario para algunas de las instrucciones de este documento.</span><span class="sxs-lookup"><span data-stu-id="17c89-111">The [.NET Core 2.2 SDK](https://www.microsoft.com/net/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="17c89-112">Certificados</span><span class="sxs-lookup"><span data-stu-id="17c89-112">Certificates</span></span>

<span data-ttu-id="17c89-113">Se requiere un certificado de una [entidad de certificación](https://wikipedia.org/wiki/Certificate_authority) para el [hospedaje de producción](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) en un dominio.</span><span class="sxs-lookup"><span data-stu-id="17c89-113">A certificate from a [certificate authority](https://wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span> <span data-ttu-id="17c89-114">[Let's Encrypt](https://letsencrypt.org/) es una entidad de certificación que ofrece certificados gratuitos.</span><span class="sxs-lookup"><span data-stu-id="17c89-114">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="17c89-115">En este documento se usan [certificados de desarrollo autofirmados](https://en.wikipedia.org/wiki/Self-signed_certificate) para hospedar imágenes pregeneradas en `localhost`.</span><span class="sxs-lookup"><span data-stu-id="17c89-115">This document uses [self-signed development certificates](https://en.wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="17c89-116">Las instrucciones son similares a usar certificados de producción.</span><span class="sxs-lookup"><span data-stu-id="17c89-116">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="17c89-117">Para los certificados de producción:</span><span class="sxs-lookup"><span data-stu-id="17c89-117">For production certs:</span></span>

* <span data-ttu-id="17c89-118">La herramienta `dotnet dev-certs` no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="17c89-118">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="17c89-119">No es necesario almacenar los certificados en la ubicación usada en las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="17c89-119">Certificates do not need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="17c89-120">Cualquier ubicación debería funcionar, aunque no se recomienda almacenar certificados en el directorio del sitio.</span><span class="sxs-lookup"><span data-stu-id="17c89-120">Any location should work, although storing certs within your site directory is not recommended.</span></span>

<span data-ttu-id="17c89-121">El volumen de instrucciones monta los certificados en contenedores.</span><span class="sxs-lookup"><span data-stu-id="17c89-121">The instructions volume mount certificates into containers.</span></span> <span data-ttu-id="17c89-122">Puede agregar certificados a las imágenes de contenedor con un comando `COPY` en un *Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="17c89-122">You can add certificates into container images with a `COPY` command in a *Dockerfile*.</span></span> <span data-ttu-id="17c89-123">No se recomienda copiar certificados en una imagen por los siguientes motivos:</span><span class="sxs-lookup"><span data-stu-id="17c89-123">Copying certificates into an image isn't recommended for the following reasons:</span></span>

* <span data-ttu-id="17c89-124">Dificulta el uso de la misma imagen para realizar pruebas con certificados de desarrollador.</span><span class="sxs-lookup"><span data-stu-id="17c89-124">It makes difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="17c89-125">Dificulta el uso de la misma imagen para hospedar con certificados de producción.</span><span class="sxs-lookup"><span data-stu-id="17c89-125">It makes difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="17c89-126">Existe un riesgo significativo de divulgación de certificados.</span><span class="sxs-lookup"><span data-stu-id="17c89-126">There is significant risk of certificate disclosure.</span></span>

## <a name="running-pre-built-container-images-with-https"></a><span data-ttu-id="17c89-127">Ejecución de imágenes de contenedor pregeneradas con HTTPS</span><span class="sxs-lookup"><span data-stu-id="17c89-127">Running pre-built container images with HTTPS</span></span>

<span data-ttu-id="17c89-128">Utilice las siguientes instrucciones para la configuración del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="17c89-128">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="17c89-129">Windows con contenedores de Linux</span><span class="sxs-lookup"><span data-stu-id="17c89-129">Windows using Linux containers</span></span>

<span data-ttu-id="17c89-130">Generar certificado y configurar equipo local:</span><span class="sxs-lookup"><span data-stu-id="17c89-130">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="17c89-131">En los comandos anteriores, reemplace `{ password here }` por una contraseña.</span><span class="sxs-lookup"><span data-stu-id="17c89-131">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="17c89-132">Ejecute la imagen de contenedor con ASP.NET Core configurado para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="17c89-132">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="17c89-133">La contraseña debe coincidir con la contraseña usada para el certificado.</span><span class="sxs-lookup"><span data-stu-id="17c89-133">The password must match the password used for the certificate.</span></span>

### <a name="macos-or-linux"></a><span data-ttu-id="17c89-134">macOS o Linux</span><span class="sxs-lookup"><span data-stu-id="17c89-134">macOS or Linux</span></span>

<span data-ttu-id="17c89-135">Generar certificado y configurar equipo local:</span><span class="sxs-lookup"><span data-stu-id="17c89-135">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="17c89-136">`dotnet dev-certs https --trust` solo se admite en macOS y Windows.</span><span class="sxs-lookup"><span data-stu-id="17c89-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="17c89-137">Debe confiar en los certificados en Linux de la manera que sea compatible con el distribución.</span><span class="sxs-lookup"><span data-stu-id="17c89-137">You need to trust certs on Linux in the way that is supported by your distro.</span></span> <span data-ttu-id="17c89-138">Es probable que necesite confiar en el certificado en el explorador.</span><span class="sxs-lookup"><span data-stu-id="17c89-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="17c89-139">En los comandos anteriores, reemplace `{ password here }` por una contraseña.</span><span class="sxs-lookup"><span data-stu-id="17c89-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="17c89-140">Ejecute la imagen de contenedor con ASP.NET Core configurado para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="17c89-140">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v ${HOME}/.aspnet/https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="17c89-141">La contraseña debe coincidir con la contraseña usada para el certificado.</span><span class="sxs-lookup"><span data-stu-id="17c89-141">The password must match the password used for the certificate.</span></span>

### <a name="windows-using-windows-containers"></a><span data-ttu-id="17c89-142">Windows con contenedores de Windows</span><span class="sxs-lookup"><span data-stu-id="17c89-142">Windows using Windows containers</span></span>

<span data-ttu-id="17c89-143">Generar certificado y configurar equipo local:</span><span class="sxs-lookup"><span data-stu-id="17c89-143">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="17c89-144">En los comandos anteriores, reemplace `{ password here }` por una contraseña.</span><span class="sxs-lookup"><span data-stu-id="17c89-144">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="17c89-145">Ejecute la imagen de contenedor con ASP.NET Core configurado para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="17c89-145">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=\https\aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:C:\https\ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="17c89-146">La contraseña debe coincidir con la contraseña usada para el certificado.</span><span class="sxs-lookup"><span data-stu-id="17c89-146">The password must match the password used for the certificate.</span></span>
