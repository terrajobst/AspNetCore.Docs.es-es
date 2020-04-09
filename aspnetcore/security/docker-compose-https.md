---
title: Hosting ASP.NET Core image in container using docker compose with HTTPS
author: ravipal
description: Aprenda a hospedar imágenes ASP.NET Core con Docker Compose a través de HTTPS
monikerRange: '>= aspnetcore-2.1'
ms.author: ravipal
ms.custom: mvc
ms.date: 03/28/2020
no-loc:
- Let's Encrypt
uid: security/docker-compose-https
ms.openlocfilehash: 616ccf906e98534ffda08c0c2b6d0a171f063cc1
ms.sourcegitcommit: d03905aadf5ceac39fff17706481af7f6c130411
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2020
ms.locfileid: "80381813"
---
# <a name="hosting-aspnet-core-images-with-docker-compose-over-https"></a><span data-ttu-id="12a05-103">Hospedar imágenes de ASP.NET Core con Docker Compose a través de HTTPS</span><span class="sxs-lookup"><span data-stu-id="12a05-103">Hosting ASP.NET Core images with Docker Compose over HTTPS</span></span>


<span data-ttu-id="12a05-104">ASP.NET Core usa HTTPS de [forma predeterminada.](/aspnet/core/security/enforcing-ssl)</span><span class="sxs-lookup"><span data-stu-id="12a05-104">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="12a05-105">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) se basa en [certificados](https://en.wikipedia.org/wiki/Public_key_certificate) para la confianza, la identidad y el cifrado.</span><span class="sxs-lookup"><span data-stu-id="12a05-105">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="12a05-106">Este documento explica cómo ejecutar las imágenes de contenedor precompiladas con HTTPS.</span><span class="sxs-lookup"><span data-stu-id="12a05-106">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="12a05-107">Consulte [Desarrollo de aplicaciones ASP.NET principales con Docker a través](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md) de HTTPS para escenarios de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="12a05-107">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="12a05-108">Este ejemplo requiere [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) o posterior del cliente de [Docker.](https://www.docker.com/products/docker)</span><span class="sxs-lookup"><span data-stu-id="12a05-108">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12a05-109">Prerrequisitos</span><span class="sxs-lookup"><span data-stu-id="12a05-109">Prerequisites</span></span>

<span data-ttu-id="12a05-110">El SDK de [.NET Core 2.2](https://dotnet.microsoft.com/download) o posterior es necesario para algunas de las instrucciones de este documento.</span><span class="sxs-lookup"><span data-stu-id="12a05-110">The [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="12a05-111">Certificados</span><span class="sxs-lookup"><span data-stu-id="12a05-111">Certificates</span></span>

<span data-ttu-id="12a05-112">Se requiere un certificado de una entidad de [certificación](https://wikipedia.org/wiki/Certificate_authority) para el hospedaje de [producción](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) para un dominio.</span><span class="sxs-lookup"><span data-stu-id="12a05-112">A certificate from a [certificate authority](https://wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span> <span data-ttu-id="12a05-113">[Let's Encrypt](https://letsencrypt.org/)es una entidad de certificación que ofrece certificados gratuitos.</span><span class="sxs-lookup"><span data-stu-id="12a05-113">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="12a05-114">Este documento utiliza [los Certificados de desarrollo autofirmados](https://wikipedia.org/wiki/Self-signed_certificate) para alojar las imágenes precompiladas sobre `localhost`.</span><span class="sxs-lookup"><span data-stu-id="12a05-114">This document uses [self-signed development certificates](https://wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="12a05-115">Las instrucciones son similares al uso de certificados de producción.</span><span class="sxs-lookup"><span data-stu-id="12a05-115">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="12a05-116">Para certificados de producción:</span><span class="sxs-lookup"><span data-stu-id="12a05-116">For production certificates:</span></span>

* <span data-ttu-id="12a05-117">La `dotnet dev-certs` herramienta no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="12a05-117">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="12a05-118">No es necesario almacenar certificados en la ubicación utilizada en las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="12a05-118">Certificates don't need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="12a05-119">Almacene los certificados en cualquier ubicación fuera del directorio del sitio.</span><span class="sxs-lookup"><span data-stu-id="12a05-119">Store the certificates in any location outside the site directory.</span></span>

<span data-ttu-id="12a05-120">Las instrucciones contenidas en la sección siguiente `volumes` los certificados de montaje de volumen en contenedores mediante la propiedad *docker-compose.yml.*</span><span class="sxs-lookup"><span data-stu-id="12a05-120">The instructions contained in the following section volume mount certificates into containers using the `volumes` property in *docker-compose.yml.*</span></span> <span data-ttu-id="12a05-121">Puede agregar certificados a imágenes de contenedor con un `COPY` comando en un *Dockerfile,* pero no se recomienda.</span><span class="sxs-lookup"><span data-stu-id="12a05-121">You could add certificates into container images with a `COPY` command in a *Dockerfile*, but it's not recommended.</span></span> <span data-ttu-id="12a05-122">No se recomienda copiar certificados en una imagen por las siguientes razones:</span><span class="sxs-lookup"><span data-stu-id="12a05-122">Copying certificates into an image isn't recommended for the following reasons:</span></span>

* <span data-ttu-id="12a05-123">Hace que sea difícil usar la misma imagen para realizar pruebas con certificados de desarrollador.</span><span class="sxs-lookup"><span data-stu-id="12a05-123">It makes it difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="12a05-124">Hace que sea difícil usar la misma imagen para hosting con certificados de producción.</span><span class="sxs-lookup"><span data-stu-id="12a05-124">It makes it difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="12a05-125">Existe un riesgo significativo de divulgación de certificados.</span><span class="sxs-lookup"><span data-stu-id="12a05-125">There is significant risk of certificate disclosure.</span></span>

## <a name="starting-a-container-with-https-support-using-docker-compose"></a><span data-ttu-id="12a05-126">Inicio de un contenedor con compatibilidad con https mediante docker compose</span><span class="sxs-lookup"><span data-stu-id="12a05-126">Starting a container with https support using docker compose</span></span>

<span data-ttu-id="12a05-127">Utilice las siguientes instrucciones para la configuración del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="12a05-127">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="12a05-128">Windows con contenedores Linux</span><span class="sxs-lookup"><span data-stu-id="12a05-128">Windows using Linux containers</span></span>

<span data-ttu-id="12a05-129">Genere el certificado y configure la máquina local:</span><span class="sxs-lookup"><span data-stu-id="12a05-129">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="12a05-130">En los comandos `{ password here }` anteriores, reemplace por una contraseña.</span><span class="sxs-lookup"><span data-stu-id="12a05-130">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="12a05-131">Cree un archivo _docker-compose.debug.yml_ con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="12a05-131">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx
    volumes:
      - ~/.aspnet/https:/https:ro
```
<span data-ttu-id="12a05-132">La contraseña especificada en el archivo docker compose debe coincidir con la contraseña utilizada para el certificado.</span><span class="sxs-lookup"><span data-stu-id="12a05-132">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="12a05-133">Inicie el contenedor con ASP.NET Core configurado para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="12a05-133">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="macos-or-linux"></a><span data-ttu-id="12a05-134">macOS o Linux</span><span class="sxs-lookup"><span data-stu-id="12a05-134">macOS or Linux</span></span>

<span data-ttu-id="12a05-135">Genere el certificado y configure la máquina local:</span><span class="sxs-lookup"><span data-stu-id="12a05-135">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="12a05-136">`dotnet dev-certs https --trust`solo es compatible con macOS y Windows.</span><span class="sxs-lookup"><span data-stu-id="12a05-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="12a05-137">Debe confiar en los certificados en Linux de la manera que es compatible con su distribución.</span><span class="sxs-lookup"><span data-stu-id="12a05-137">You need to trust certificates on Linux in the way that is supported by your distro.</span></span> <span data-ttu-id="12a05-138">Es probable que necesite confiar en el certificado en su navegador.</span><span class="sxs-lookup"><span data-stu-id="12a05-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="12a05-139">En los comandos `{ password here }` anteriores, reemplace por una contraseña.</span><span class="sxs-lookup"><span data-stu-id="12a05-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="12a05-140">Cree un archivo _docker-compose.debug.yml_ con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="12a05-140">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx
    volumes:
      - ~/.aspnet/https:/https:ro
```
<span data-ttu-id="12a05-141">La contraseña especificada en el archivo docker compose debe coincidir con la contraseña utilizada para el certificado.</span><span class="sxs-lookup"><span data-stu-id="12a05-141">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="12a05-142">Inicie el contenedor con ASP.NET Core configurado para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="12a05-142">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="windows-using-windows-containers"></a><span data-ttu-id="12a05-143">Windows con contenedores de Windows</span><span class="sxs-lookup"><span data-stu-id="12a05-143">Windows using Windows containers</span></span>

<span data-ttu-id="12a05-144">Genere el certificado y configure la máquina local:</span><span class="sxs-lookup"><span data-stu-id="12a05-144">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="12a05-145">En los comandos `{ password here }` anteriores, reemplace por una contraseña.</span><span class="sxs-lookup"><span data-stu-id="12a05-145">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="12a05-146">Cree un archivo _docker-compose.debug.yml_ con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="12a05-146">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=C:\https\aspnetapp.pfx
    volumes:
      - ${USERPROFILE}\.aspnet\https:C:\https:ro
```
<span data-ttu-id="12a05-147">La contraseña especificada en el archivo docker compose debe coincidir con la contraseña utilizada para el certificado.</span><span class="sxs-lookup"><span data-stu-id="12a05-147">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="12a05-148">Inicie el contenedor con ASP.NET Core configurado para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="12a05-148">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```
