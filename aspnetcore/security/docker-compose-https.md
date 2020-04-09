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
# <a name="hosting-aspnet-core-images-with-docker-compose-over-https"></a>Hospedar imágenes de ASP.NET Core con Docker Compose a través de HTTPS


ASP.NET Core usa HTTPS de [forma predeterminada.](/aspnet/core/security/enforcing-ssl) [HTTPS](https://en.wikipedia.org/wiki/HTTPS) se basa en [certificados](https://en.wikipedia.org/wiki/Public_key_certificate) para la confianza, la identidad y el cifrado.

Este documento explica cómo ejecutar las imágenes de contenedor precompiladas con HTTPS.

Consulte [Desarrollo de aplicaciones ASP.NET principales con Docker a través](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md) de HTTPS para escenarios de desarrollo.

Este ejemplo requiere [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) o posterior del cliente de [Docker.](https://www.docker.com/products/docker)

## <a name="prerequisites"></a>Prerrequisitos

El SDK de [.NET Core 2.2](https://dotnet.microsoft.com/download) o posterior es necesario para algunas de las instrucciones de este documento.

## <a name="certificates"></a>Certificados

Se requiere un certificado de una entidad de [certificación](https://wikipedia.org/wiki/Certificate_authority) para el hospedaje de [producción](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) para un dominio. [Let's Encrypt](https://letsencrypt.org/)es una entidad de certificación que ofrece certificados gratuitos.

Este documento utiliza [los Certificados de desarrollo autofirmados](https://wikipedia.org/wiki/Self-signed_certificate) para alojar las imágenes precompiladas sobre `localhost`. Las instrucciones son similares al uso de certificados de producción.

Para certificados de producción:

* La `dotnet dev-certs` herramienta no es necesaria.
* No es necesario almacenar certificados en la ubicación utilizada en las instrucciones. Almacene los certificados en cualquier ubicación fuera del directorio del sitio.

Las instrucciones contenidas en la sección siguiente `volumes` los certificados de montaje de volumen en contenedores mediante la propiedad *docker-compose.yml.* Puede agregar certificados a imágenes de contenedor con un `COPY` comando en un *Dockerfile,* pero no se recomienda. No se recomienda copiar certificados en una imagen por las siguientes razones:

* Hace que sea difícil usar la misma imagen para realizar pruebas con certificados de desarrollador.
* Hace que sea difícil usar la misma imagen para hosting con certificados de producción.
* Existe un riesgo significativo de divulgación de certificados.

## <a name="starting-a-container-with-https-support-using-docker-compose"></a>Inicio de un contenedor con compatibilidad con https mediante docker compose

Utilice las siguientes instrucciones para la configuración del sistema operativo.

### <a name="windows-using-linux-containers"></a>Windows con contenedores Linux

Genere el certificado y configure la máquina local:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

En los comandos `{ password here }` anteriores, reemplace por una contraseña.

Cree un archivo _docker-compose.debug.yml_ con el siguiente contenido:

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
La contraseña especificada en el archivo docker compose debe coincidir con la contraseña utilizada para el certificado.

Inicie el contenedor con ASP.NET Core configurado para HTTPS:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="macos-or-linux"></a>macOS o Linux

Genere el certificado y configure la máquina local:

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

`dotnet dev-certs https --trust`solo es compatible con macOS y Windows. Debe confiar en los certificados en Linux de la manera que es compatible con su distribución. Es probable que necesite confiar en el certificado en su navegador.

En los comandos `{ password here }` anteriores, reemplace por una contraseña.

Cree un archivo _docker-compose.debug.yml_ con el siguiente contenido:

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
La contraseña especificada en el archivo docker compose debe coincidir con la contraseña utilizada para el certificado.

Inicie el contenedor con ASP.NET Core configurado para HTTPS:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="windows-using-windows-containers"></a>Windows con contenedores de Windows

Genere el certificado y configure la máquina local:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

En los comandos `{ password here }` anteriores, reemplace por una contraseña.

Cree un archivo _docker-compose.debug.yml_ con el siguiente contenido:

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
La contraseña especificada en el archivo docker compose debe coincidir con la contraseña utilizada para el certificado.

Inicie el contenedor con ASP.NET Core configurado para HTTPS:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```
