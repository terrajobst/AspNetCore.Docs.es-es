---
title: Azure Key Vault proveedor de configuración en ASP.NET Core
author: guardrex
description: Aprenda a usar el proveedor de configuración de Azure Key Vault para configurar una aplicación mediante pares de nombre y valor cargados en tiempo de ejecución.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/16/2019
uid: security/key-vault-configuration
ms.openlocfilehash: cd31094884f53f34d55c0bceabae41ca2bacba4c
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928546"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="932ef-103">Azure Key Vault proveedor de configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="932ef-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="932ef-104">Por [Luke Latham](https://github.com/guardrex) y [Andrew Stanton-enfermera](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="932ef-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="932ef-105">En este documento se explica cómo usar el proveedor de configuración de [key Vault de Microsoft Azure](https://azure.microsoft.com/services/key-vault/) para cargar los valores de configuración de la aplicación de Azure Key Vault secretos.</span><span class="sxs-lookup"><span data-stu-id="932ef-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="932ef-106">Azure Key Vault es un servicio basado en la nube que ayuda a proteger las claves criptográficas y los secretos que usan las aplicaciones y los servicios.</span><span class="sxs-lookup"><span data-stu-id="932ef-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="932ef-107">Entre los escenarios comunes para usar Azure Key Vault con ASP.NET Core aplicaciones se incluyen:</span><span class="sxs-lookup"><span data-stu-id="932ef-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="932ef-108">Controlar el acceso a los datos de configuración confidenciales.</span><span class="sxs-lookup"><span data-stu-id="932ef-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="932ef-109">Cumplir los requisitos de los módulos de seguridad de hardware (HSM) de FIPS 140-2 nivel 2 validados al almacenar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="932ef-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="932ef-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="932ef-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="932ef-111">Paquetes</span><span class="sxs-lookup"><span data-stu-id="932ef-111">Packages</span></span>

<span data-ttu-id="932ef-112">Agregue una referencia de paquete al paquete [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .</span><span class="sxs-lookup"><span data-stu-id="932ef-112">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="932ef-113">Aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="932ef-113">Sample app</span></span>

<span data-ttu-id="932ef-114">La aplicación de ejemplo se ejecuta en uno de los dos modos determinados por la instrucción `#define` en la parte superior del archivo *Program.CS* :</span><span class="sxs-lookup"><span data-stu-id="932ef-114">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="932ef-115">`Certificate` &ndash; muestra el uso de un identificador de cliente de Azure Key Vault y un certificado X. 509 para tener acceso a los secretos almacenados en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="932ef-115">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="932ef-116">Esta versión del ejemplo se puede ejecutar desde cualquier ubicación, implementada en Azure App Service o cualquier host capaz de servir a una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="932ef-116">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="932ef-117">en `Managed` &ndash; se muestra cómo usar [identidades administradas para los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview) para autenticar la aplicación en Azure Key Vault con Azure ad autenticación sin credenciales almacenadas en el código o la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-117">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="932ef-118">Al usar identidades administradas para autenticar, no se requieren un identificador de aplicación Azure AD y una contraseña (secreto de cliente).</span><span class="sxs-lookup"><span data-stu-id="932ef-118">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="932ef-119">La versión `Managed` del ejemplo debe implementarse en Azure.</span><span class="sxs-lookup"><span data-stu-id="932ef-119">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="932ef-120">Siga las instrucciones de la sección [uso de las identidades administradas para los recursos de Azure](#use-managed-identities-for-azure-resources) .</span><span class="sxs-lookup"><span data-stu-id="932ef-120">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="932ef-121">Para obtener más información sobre cómo configurar una aplicación de ejemplo mediante directivas de preprocesador (`#define`), consulte <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="932ef-121">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="932ef-122">Almacenamiento de secretos en el entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="932ef-122">Secret storage in the Development environment</span></span>

<span data-ttu-id="932ef-123">Establezca secretos localmente mediante la [herramienta de administrador de secretos](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="932ef-123">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="932ef-124">Cuando la aplicación de ejemplo se ejecuta en el equipo local en el entorno de desarrollo, los secretos se cargan desde el almacén del administrador secreto local.</span><span class="sxs-lookup"><span data-stu-id="932ef-124">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="932ef-125">La herramienta Administrador de secretos requiere una `<UserSecretsId>` propiedad en el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-125">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="932ef-126">Establezca el valor de propiedad (`{GUID}`) en cualquier GUID único:</span><span class="sxs-lookup"><span data-stu-id="932ef-126">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="932ef-127">Los secretos se crean como pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="932ef-127">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="932ef-128">Los valores jerárquicos (secciones de configuración) usan un `:` (dos puntos) como separador en ASP.NET Core nombres de clave de [configuración](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="932ef-128">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="932ef-129">El administrador de secretos se usa desde un shell de comandos abierto a la [raíz del contenido](xref:fundamentals/index#content-root)del proyecto, donde `{SECRET NAME}` es el nombre y `{SECRET VALUE}` es el valor:</span><span class="sxs-lookup"><span data-stu-id="932ef-129">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="932ef-130">Ejecute los siguientes comandos en un shell de comandos desde la [raíz del contenido](xref:fundamentals/index#content-root) del proyecto para establecer los secretos de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="932ef-130">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="932ef-131">Cuando estos secretos se almacenan en Azure Key Vault en el [almacenamiento de secretos en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección, el sufijo de `_dev` se cambia a `_prod`.</span><span class="sxs-lookup"><span data-stu-id="932ef-131">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="932ef-132">El sufijo proporciona una indicación visual en la salida de la aplicación que indica el origen de los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="932ef-132">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="932ef-133">Almacenamiento secreto en el entorno de producción con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="932ef-133">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="932ef-134">Las instrucciones que se proporcionan en la guía de [Inicio rápido: establecer y recuperar un secreto de Azure Key Vault con CLI de Azure](/azure/key-vault/quick-create-cli) tema se resumen aquí para crear un Azure Key Vault y almacenar secretos usados por la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="932ef-134">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="932ef-135">Consulte el tema para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="932ef-135">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="932ef-136">Abra Azure Cloud Shell con cualquiera de los métodos siguientes en el [Azure portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="932ef-136">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="932ef-137">Seleccione **pruébelo** en la esquina superior derecha de un bloque de código.</span><span class="sxs-lookup"><span data-stu-id="932ef-137">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="932ef-138">Use la cadena de búsqueda "CLI de Azure" en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="932ef-138">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="932ef-139">Abra Cloud Shell en el explorador con el botón **iniciar Cloud Shell** .</span><span class="sxs-lookup"><span data-stu-id="932ef-139">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="932ef-140">Seleccione el botón **Cloud Shell** en el menú de la esquina superior derecha del Azure portal.</span><span class="sxs-lookup"><span data-stu-id="932ef-140">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="932ef-141">Para obtener más información, vea [CLI de Azure](/cli/azure/) e [información general de Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="932ef-141">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="932ef-142">Si aún no está autenticado, inicie sesión con el comando `az login`.</span><span class="sxs-lookup"><span data-stu-id="932ef-142">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="932ef-143">Cree un grupo de recursos con el siguiente comando, donde `{RESOURCE GROUP NAME}` es el nombre del grupo de recursos para el nuevo grupo de recursos y `{LOCATION}` es la región de Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="932ef-143">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="932ef-144">Cree un almacén de claves en el grupo de recursos con el siguiente comando, donde `{KEY VAULT NAME}` es el nombre del nuevo almacén de claves y `{LOCATION}` es la región de Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="932ef-144">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="932ef-145">Cree secretos en el almacén de claves como pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="932ef-145">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="932ef-146">Azure Key Vault los nombres de los secretos se limitan a caracteres alfanuméricos y guiones.</span><span class="sxs-lookup"><span data-stu-id="932ef-146">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="932ef-147">Los valores jerárquicos (secciones de configuración) usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="932ef-147">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="932ef-148">Los dos puntos, que se suelen usar para delimitar una sección de una subclave en [ASP.net Core configuración](xref:fundamentals/configuration/index), no se permiten en los nombres de secreto del almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="932ef-148">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="932ef-149">Por lo tanto, se usan dos guiones y se intercambian por dos puntos cuando se cargan los secretos en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-149">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="932ef-150">Los siguientes secretos son para su uso con la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="932ef-150">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="932ef-151">Los valores incluyen un sufijo `_prod` para distinguirlos de los valores de sufijo `_dev` cargados en el entorno de desarrollo de los secretos de usuario.</span><span class="sxs-lookup"><span data-stu-id="932ef-151">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="932ef-152">Reemplace `{KEY VAULT NAME}` por el nombre del almacén de claves que creó en el paso anterior:</span><span class="sxs-lookup"><span data-stu-id="932ef-152">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="932ef-153">Usar el identificador de aplicación y el certificado X. 509 para aplicaciones no hospedadas en Azure</span><span class="sxs-lookup"><span data-stu-id="932ef-153">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="932ef-154">Configure Azure AD, Azure Key Vault y la aplicación para que use un identificador de aplicación Azure Active Directory y un certificado X. 509 para autenticarse en un almacén de claves **cuando la aplicación se hospeda fuera de Azure**.</span><span class="sxs-lookup"><span data-stu-id="932ef-154">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="932ef-155">Para obtener más información, vea [acerca de las claves, los secretos y los certificados](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="932ef-155">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="932ef-156">Aunque el uso de un identificador de aplicación y un certificado X. 509 es compatible con las aplicaciones hospedadas en Azure, se recomienda usar [identidades administradas para los recursos de Azure](#use-managed-identities-for-azure-resources) al hospedar una aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="932ef-156">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="932ef-157">Las identidades administradas no requieren el almacenamiento de un certificado en la aplicación o en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="932ef-157">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="932ef-158">La aplicación de ejemplo usa un identificador de aplicación y un certificado X. 509 cuando la instrucción `#define` en la parte superior del archivo *Program.CS* se establece en `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="932ef-158">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="932ef-159">Cree un certificado de archivo PKCS # 12 ( *. pfx*).</span><span class="sxs-lookup"><span data-stu-id="932ef-159">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="932ef-160">Las opciones para crear certificados incluyen [Makecert en Windows](/windows/desktop/seccrypto/makecert) y [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="932ef-160">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="932ef-161">Instale el certificado en el almacén de certificados personales del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="932ef-161">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="932ef-162">Marcar la clave como exportable es opcional.</span><span class="sxs-lookup"><span data-stu-id="932ef-162">Marking the key as exportable is optional.</span></span> <span data-ttu-id="932ef-163">Anote la huella digital del certificado, que se usa más adelante en este proceso.</span><span class="sxs-lookup"><span data-stu-id="932ef-163">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="932ef-164">Exporte el certificado de archivo PKCS # 12 ( *. pfx*) como un certificado codificado en der ( *. cer*).</span><span class="sxs-lookup"><span data-stu-id="932ef-164">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="932ef-165">Registre la aplicación con Azure AD (**registros de aplicaciones**).</span><span class="sxs-lookup"><span data-stu-id="932ef-165">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="932ef-166">Cargue el certificado codificado en DER ( *. cer*) en Azure ad:</span><span class="sxs-lookup"><span data-stu-id="932ef-166">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="932ef-167">Seleccione la aplicación en Azure AD.</span><span class="sxs-lookup"><span data-stu-id="932ef-167">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="932ef-168">Vaya a **certificados & Secrets**.</span><span class="sxs-lookup"><span data-stu-id="932ef-168">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="932ef-169">Seleccione **upload Certificate (cargar certificado** ) para cargar el certificado, que contiene la clave pública.</span><span class="sxs-lookup"><span data-stu-id="932ef-169">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="932ef-170">Un certificado *. cer*, *. pem*o *. CRT* es aceptable.</span><span class="sxs-lookup"><span data-stu-id="932ef-170">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="932ef-171">Almacene el nombre del almacén de claves, el identificador de la aplicación y la huella digital del certificado en el archivo *appSettings. JSON* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-171">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="932ef-172">Vaya a **almacenes de claves** en el Azure portal.</span><span class="sxs-lookup"><span data-stu-id="932ef-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="932ef-173">Seleccione el almacén de claves que creó en el [almacenamiento de secretos en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección.</span><span class="sxs-lookup"><span data-stu-id="932ef-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="932ef-174">Seleccione **directivas de acceso**.</span><span class="sxs-lookup"><span data-stu-id="932ef-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="932ef-175">Seleccione **Agregar Directiva de acceso**.</span><span class="sxs-lookup"><span data-stu-id="932ef-175">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="932ef-176">Abra los **permisos de secreto** y proporcione la aplicación con los permisos **Get** y **List** .</span><span class="sxs-lookup"><span data-stu-id="932ef-176">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="932ef-177">Seleccione **seleccionar entidad** de seguridad y seleccione la aplicación registrada por nombre.</span><span class="sxs-lookup"><span data-stu-id="932ef-177">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="932ef-178">Seleccione el botón **seleccionar** .</span><span class="sxs-lookup"><span data-stu-id="932ef-178">Select the **Select** button.</span></span>
1. <span data-ttu-id="932ef-179">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="932ef-179">Select **OK**.</span></span>
1. <span data-ttu-id="932ef-180">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="932ef-180">Select **Save**.</span></span>
1. <span data-ttu-id="932ef-181">Implemente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-181">Deploy the app.</span></span>

<span data-ttu-id="932ef-182">La aplicación de ejemplo `Certificate` obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el secreto:</span><span class="sxs-lookup"><span data-stu-id="932ef-182">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="932ef-183">Valores no jerárquicos: el valor de `SecretName` se obtiene con `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="932ef-183">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="932ef-184">Valores jerárquicos (secciones): Use `:` notación (dos puntos) o el método de extensión `GetSection`.</span><span class="sxs-lookup"><span data-stu-id="932ef-184">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="932ef-185">Use cualquiera de estos métodos para obtener el valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="932ef-185">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="932ef-186">El certificado X. 509 se administra mediante el sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="932ef-186">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="932ef-187">La aplicación llama a <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> con los valores proporcionados por el archivo *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="932ef-187">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

<span data-ttu-id="932ef-188">Valores de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="932ef-188">Example values:</span></span>

* <span data-ttu-id="932ef-189">Nombre del almacén de claves: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="932ef-189">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="932ef-190">IDENTIFICADOR de la aplicación: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="932ef-190">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="932ef-191">Huella digital del certificado: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="932ef-191">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="932ef-192">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="932ef-192">*appsettings.json*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

<span data-ttu-id="932ef-193">Al ejecutar la aplicación, se muestran los valores de secreto cargados en una página web.</span><span class="sxs-lookup"><span data-stu-id="932ef-193">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="932ef-194">En el entorno de desarrollo, los valores secretos se cargan con el sufijo `_dev`.</span><span class="sxs-lookup"><span data-stu-id="932ef-194">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="932ef-195">En el entorno de producción, los valores se cargan con el sufijo `_prod`.</span><span class="sxs-lookup"><span data-stu-id="932ef-195">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="932ef-196">Uso de identidades administradas para los recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="932ef-196">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="932ef-197">**Una aplicación implementada en Azure** puede aprovechar las [identidades administradas para los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview), lo que permite que la aplicación se autentique con Azure Key Vault mediante la autenticación de Azure ad sin credenciales (identificador de aplicación y contraseña/secreto de cliente) almacenadas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-197">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="932ef-198">La aplicación de ejemplo usa identidades administradas para los recursos de Azure cuando la instrucción `#define` en la parte superior del archivo *Program.CS* se establece en `Managed`.</span><span class="sxs-lookup"><span data-stu-id="932ef-198">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="932ef-199">Escriba el nombre del almacén en el archivo *appSettings. JSON* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-199">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="932ef-200">La aplicación de ejemplo no requiere un identificador de aplicación y una contraseña (secreto de cliente) cuando se establece en la versión `Managed`, por lo que puede omitir esas entradas de configuración.</span><span class="sxs-lookup"><span data-stu-id="932ef-200">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="932ef-201">La aplicación se implementa en Azure y Azure autentica la aplicación para acceder a Azure Key Vault solo mediante el nombre del almacén almacenado en el archivo *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="932ef-201">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="932ef-202">Implemente la aplicación de ejemplo en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="932ef-202">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="932ef-203">Una aplicación implementada en Azure App Service se registra automáticamente con Azure AD cuando se crea el servicio.</span><span class="sxs-lookup"><span data-stu-id="932ef-203">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="932ef-204">Obtenga el identificador de objeto de la implementación para usarlo en el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="932ef-204">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="932ef-205">El identificador de objeto se muestra en el Azure Portal en el panel **identidad** del App Service.</span><span class="sxs-lookup"><span data-stu-id="932ef-205">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="932ef-206">Con CLI de Azure y el identificador de objeto de la aplicación, proporcione la aplicación con los permisos `list` y `get` para acceder al almacén de claves:</span><span class="sxs-lookup"><span data-stu-id="932ef-206">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="932ef-207">**Reinicie la aplicación** con CLI de Azure, PowerShell o el Azure portal.</span><span class="sxs-lookup"><span data-stu-id="932ef-207">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="932ef-208">La aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="932ef-208">The sample app:</span></span>

* <span data-ttu-id="932ef-209">Crea una instancia de la clase `AzureServiceTokenProvider` sin una cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="932ef-209">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="932ef-210">Cuando no se proporciona una cadena de conexión, el proveedor intenta obtener un token de acceso de identidades administradas para los recursos de Azure.</span><span class="sxs-lookup"><span data-stu-id="932ef-210">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="932ef-211">Se crea un nuevo <xref:Microsoft.Azure.KeyVault.KeyVaultClient> con la devolución de llamada del token de instancia `AzureServiceTokenProvider`.</span><span class="sxs-lookup"><span data-stu-id="932ef-211">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="932ef-212">La instancia de <xref:Microsoft.Azure.KeyVault.KeyVaultClient> se utiliza con una implementación predeterminada de <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> que carga todos los valores secretos y reemplaza los dobles guiones (`--`) por dos puntos (`:`) en los nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="932ef-212">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

<span data-ttu-id="932ef-213">Valor de ejemplo de nombre del almacén de claves: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="932ef-213">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="932ef-214">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="932ef-214">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="932ef-215">Al ejecutar la aplicación, se muestran los valores de secreto cargados en una página web.</span><span class="sxs-lookup"><span data-stu-id="932ef-215">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="932ef-216">En el entorno de desarrollo, los valores secretos tienen el sufijo `_dev` porque son proporcionados por los secretos del usuario.</span><span class="sxs-lookup"><span data-stu-id="932ef-216">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="932ef-217">En el entorno de producción, los valores se cargan con el sufijo `_prod` porque los proporciona Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="932ef-217">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="932ef-218">Si recibe un error de `Access denied`, confirme que la aplicación está registrada con Azure AD y que proporciona acceso al almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="932ef-218">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="932ef-219">Confirme que ha reiniciado el servicio en Azure.</span><span class="sxs-lookup"><span data-stu-id="932ef-219">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="932ef-220">Para obtener información sobre el uso del proveedor con una identidad administrada y una canalización DevOps de Azure, consulte [creación de una conexión de servicio Azure Resource Manager a una máquina virtual con una identidad de servicio administrada](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span><span class="sxs-lookup"><span data-stu-id="932ef-220">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configuration-options"></a><span data-ttu-id="932ef-221">Opciones de configuración</span><span class="sxs-lookup"><span data-stu-id="932ef-221">Configuration options</span></span>

<span data-ttu-id="932ef-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> puede aceptar un <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span><span class="sxs-lookup"><span data-stu-id="932ef-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> can accept an <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span></span>

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| <span data-ttu-id="932ef-223">La propiedad</span><span class="sxs-lookup"><span data-stu-id="932ef-223">Property</span></span>         | <span data-ttu-id="932ef-224">Descripción</span><span class="sxs-lookup"><span data-stu-id="932ef-224">Description</span></span> |
| ---------------- | ----------- |
| `Client`         | <span data-ttu-id="932ef-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> que se va a usar para recuperar valores.</span><span class="sxs-lookup"><span data-stu-id="932ef-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> to use for retrieving values.</span></span> |
| `Manager`        | <span data-ttu-id="932ef-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instancia usada para controlar la carga de secretos.</span><span class="sxs-lookup"><span data-stu-id="932ef-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance used to control secret loading.</span></span> |
| `ReloadInterval` | <span data-ttu-id="932ef-227">`Timespan` para esperar entre los intentos de sondeo del almacén de claves en busca de cambios.</span><span class="sxs-lookup"><span data-stu-id="932ef-227">`Timespan` to wait between attempts at polling the key vault for changes.</span></span> <span data-ttu-id="932ef-228">El valor predeterminado es `null` (la configuración no se recarga).</span><span class="sxs-lookup"><span data-stu-id="932ef-228">The default value is `null` (configuration isn't reloaded).</span></span> |
| `Vault`          | <span data-ttu-id="932ef-229">URI del almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="932ef-229">Key vault URI.</span></span> |

::: moniker-end

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="932ef-230">Usar un prefijo de nombre de clave</span><span class="sxs-lookup"><span data-stu-id="932ef-230">Use a key name prefix</span></span>

<span data-ttu-id="932ef-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> proporciona una sobrecarga que acepta una implementación de <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, que le permite controlar cómo se convierten los secretos del almacén de claves en claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="932ef-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="932ef-232">Por ejemplo, puede implementar la interfaz para cargar valores de secreto basados en un valor de prefijo que se proporciona al inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-232">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="932ef-233">Esto le permite, por ejemplo, cargar secretos en función de la versión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-233">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="932ef-234">No use prefijos en secretos del almacén de claves para colocar secretos para varias aplicaciones en el mismo almacén de claves o para colocar secretos de entorno (por ejemplo, *desarrollo* frente a secretos de *producción* ) en el mismo almacén.</span><span class="sxs-lookup"><span data-stu-id="932ef-234">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="932ef-235">Se recomienda que las distintas aplicaciones y entornos de desarrollo y producción usen almacenes de claves independientes para aislar los entornos de aplicación para obtener el máximo nivel de seguridad.</span><span class="sxs-lookup"><span data-stu-id="932ef-235">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="932ef-236">En el siguiente ejemplo, se establece un secreto en el almacén de claves (y el uso de la herramienta de administración de secretos para el entorno de desarrollo) para `5000-AppSecret` (no se permiten puntos en los nombres de secreto del almacén de claves).</span><span class="sxs-lookup"><span data-stu-id="932ef-236">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="932ef-237">Este secreto representa un secreto de aplicación para la versión 5.0.0.0 de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-237">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="932ef-238">Para otra versión de la aplicación, 5.1.0.0, se agrega un secreto al almacén de claves (y con la herramienta de administración de secretos) para `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="932ef-238">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="932ef-239">Cada versión de la aplicación carga su valor de secreto con versión en su configuración como `AppSecret`, lo que elimina la versión mientras carga el secreto.</span><span class="sxs-lookup"><span data-stu-id="932ef-239">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="932ef-240">se llama a <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> con un <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>personalizado:</span><span class="sxs-lookup"><span data-stu-id="932ef-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="932ef-241">La implementación de <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> reacciona a los prefijos de versión de los secretos para cargar el secreto adecuado en la configuración:</span><span class="sxs-lookup"><span data-stu-id="932ef-241">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="932ef-242">`Load` carga un secreto cuando su nombre comienza con el prefijo.</span><span class="sxs-lookup"><span data-stu-id="932ef-242">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="932ef-243">No se cargan otros secretos.</span><span class="sxs-lookup"><span data-stu-id="932ef-243">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="932ef-244">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="932ef-244">`GetKey`:</span></span>
  * <span data-ttu-id="932ef-245">Quita el prefijo del nombre del secreto.</span><span class="sxs-lookup"><span data-stu-id="932ef-245">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="932ef-246">Reemplaza dos guiones en cualquier nombre por el `KeyDelimiter`, que es el delimitador usado en la configuración (normalmente un signo de dos puntos).</span><span class="sxs-lookup"><span data-stu-id="932ef-246">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="932ef-247">Azure Key Vault no permite un signo de dos puntos en los nombres de secreto.</span><span class="sxs-lookup"><span data-stu-id="932ef-247">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="932ef-248">Un algoritmo de proveedor llama al método `Load` que recorre en iteración los secretos del almacén para encontrar los que tienen el prefijo de versión.</span><span class="sxs-lookup"><span data-stu-id="932ef-248">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="932ef-249">Cuando se encuentra un prefijo de versión con `Load`, el algoritmo utiliza el método `GetKey` para devolver el nombre de configuración del nombre de secreto.</span><span class="sxs-lookup"><span data-stu-id="932ef-249">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="932ef-250">Elimina el prefijo de versión del nombre del secreto y devuelve el resto del nombre de secreto para cargarlo en los pares de nombre y valor de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-250">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="932ef-251">Cuando se implementa este enfoque:</span><span class="sxs-lookup"><span data-stu-id="932ef-251">When this approach is implemented:</span></span>

1. <span data-ttu-id="932ef-252">La versión de la aplicación especificada en el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-252">The app's version specified in the app's project file.</span></span> <span data-ttu-id="932ef-253">En el ejemplo siguiente, la versión de la aplicación se establece en `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="932ef-253">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="932ef-254">Confirme que una propiedad de `<UserSecretsId>` está presente en el archivo de proyecto de la aplicación, donde `{GUID}` es un GUID proporcionado por el usuario:</span><span class="sxs-lookup"><span data-stu-id="932ef-254">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="932ef-255">Guarde los siguientes secretos localmente con la [herramienta de administración de secretos](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="932ef-255">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="932ef-256">Los secretos se guardan en Azure Key Vault mediante los siguientes comandos de CLI de Azure:</span><span class="sxs-lookup"><span data-stu-id="932ef-256">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="932ef-257">Cuando se ejecuta la aplicación, se cargan los secretos del almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="932ef-257">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="932ef-258">El secreto de cadena de `5000-AppSecret` coincide con la versión de la aplicación especificada en el archivo de proyecto de la aplicación (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="932ef-258">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="932ef-259">La versión, `5000` (con el guion), se elimina del nombre de la clave.</span><span class="sxs-lookup"><span data-stu-id="932ef-259">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="932ef-260">En toda la aplicación, la lectura de la configuración con la clave `AppSecret` carga el valor del secreto.</span><span class="sxs-lookup"><span data-stu-id="932ef-260">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="932ef-261">Si se cambia la versión de la aplicación en el archivo de proyecto a `5.1.0.0` y se vuelve a ejecutar la aplicación, el valor secreto devuelto se `5.1.0.0_secret_value_dev` en el entorno de desarrollo y `5.1.0.0_secret_value_prod` en producción.</span><span class="sxs-lookup"><span data-stu-id="932ef-261">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="932ef-262">También puede proporcionar su propia implementación de <xref:Microsoft.Azure.KeyVault.KeyVaultClient> a <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span><span class="sxs-lookup"><span data-stu-id="932ef-262">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="932ef-263">Un cliente personalizado permite compartir una única instancia del cliente a través de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="932ef-263">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="932ef-264">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="932ef-264">Bind an array to a class</span></span>

<span data-ttu-id="932ef-265">El proveedor es capaz de leer valores de configuración en una matriz para enlazar a una matriz POCO.</span><span class="sxs-lookup"><span data-stu-id="932ef-265">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="932ef-266">Al leer desde un origen de configuración que permite que las claves contengan separadores de dos puntos (`:`), se usa un segmento de clave numérica para distinguir las claves que componen una matriz (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="932ef-266">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="932ef-267">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="932ef-267">`:{n}:`).</span></span> <span data-ttu-id="932ef-268">Para obtener más información, vea [Configuración: enlazar una matriz a una clase](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="932ef-268">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="932ef-269">Azure Key Vault teclas no pueden usar un signo de dos puntos como separador.</span><span class="sxs-lookup"><span data-stu-id="932ef-269">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="932ef-270">El enfoque que se describe en este tema utiliza los guiones dobles (`--`) como separador de valores jerárquicos (secciones).</span><span class="sxs-lookup"><span data-stu-id="932ef-270">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="932ef-271">Las claves de matriz se almacenan en Azure Key Vault con dobles guiones y segmentos de clave numérica (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="932ef-271">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="932ef-272">Examine la siguiente configuración de proveedor de registro de [Serilog](https://serilog.net/) proporcionada por un archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="932ef-272">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="932ef-273">Hay dos literales de objeto definidos en la matriz `WriteTo` que reflejan dos *receptores*Serilog, que describen los destinos para el registro de la salida:</span><span class="sxs-lookup"><span data-stu-id="932ef-273">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="932ef-274">La configuración que se muestra en el archivo JSON anterior se almacena en Azure Key Vault mediante la notación de doble guión (`--`) y los segmentos numéricos:</span><span class="sxs-lookup"><span data-stu-id="932ef-274">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="932ef-275">Key</span><span class="sxs-lookup"><span data-stu-id="932ef-275">Key</span></span> | <span data-ttu-id="932ef-276">{2&gt;Value&lt;2}</span><span class="sxs-lookup"><span data-stu-id="932ef-276">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="932ef-277">Volver a cargar los secretos</span><span class="sxs-lookup"><span data-stu-id="932ef-277">Reload secrets</span></span>

<span data-ttu-id="932ef-278">Los secretos se almacenan en caché hasta que se llama a `IConfigurationRoot.Reload()`.</span><span class="sxs-lookup"><span data-stu-id="932ef-278">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="932ef-279">La aplicación no respeta los secretos expirados, deshabilitados y actualizados en el almacén de claves hasta que se ejecute `Reload`.</span><span class="sxs-lookup"><span data-stu-id="932ef-279">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="932ef-280">Secretos deshabilitados y expirados</span><span class="sxs-lookup"><span data-stu-id="932ef-280">Disabled and expired secrets</span></span>

<span data-ttu-id="932ef-281">Los secretos deshabilitados y expirados inician una <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span><span class="sxs-lookup"><span data-stu-id="932ef-281">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="932ef-282">Para evitar que se inicie la aplicación, proporcione la configuración mediante un proveedor de configuración diferente o actualice el secreto deshabilitado o expirado.</span><span class="sxs-lookup"><span data-stu-id="932ef-282">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="932ef-283">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="932ef-283">Troubleshoot</span></span>

<span data-ttu-id="932ef-284">Cuando la aplicación no carga la configuración mediante el proveedor, se escribe un mensaje de error en la [infraestructura de registro de ASP.net Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="932ef-284">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="932ef-285">Las siguientes condiciones impedirán que se cargue la configuración:</span><span class="sxs-lookup"><span data-stu-id="932ef-285">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="932ef-286">La aplicación o el certificado no están configurados correctamente en Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="932ef-286">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="932ef-287">El almacén de claves no existe en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="932ef-287">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="932ef-288">La aplicación no está autorizada para acceder al almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="932ef-288">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="932ef-289">La Directiva de acceso no incluye los permisos `Get` y `List`.</span><span class="sxs-lookup"><span data-stu-id="932ef-289">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="932ef-290">En el almacén de claves, los datos de configuración (par nombre-valor) tienen un nombre incorrecto, faltan, están deshabilitados o han expirado.</span><span class="sxs-lookup"><span data-stu-id="932ef-290">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="932ef-291">La aplicación tiene un nombre de almacén de claves (`KeyVaultName`), Azure AD ID. de aplicación (`AzureADApplicationId`) o una huella digital de certificado de Azure AD (`AzureADCertThumbprint`) incorrectos.</span><span class="sxs-lookup"><span data-stu-id="932ef-291">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="932ef-292">La clave de configuración (nombre) es incorrecta en la aplicación para el valor que está intentando cargar.</span><span class="sxs-lookup"><span data-stu-id="932ef-292">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="932ef-293">Al agregar la Directiva de acceso de la aplicación al almacén de claves, la Directiva se creó, pero no se seleccionó el botón **Guardar** en la interfaz de usuario de **directivas de acceso** .</span><span class="sxs-lookup"><span data-stu-id="932ef-293">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="932ef-294">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="932ef-294">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="932ef-295">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="932ef-295">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="932ef-296">Microsoft Azure: documentación de Key Vault</span><span class="sxs-lookup"><span data-stu-id="932ef-296">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="932ef-297">Cómo generar y transferir claves protegidas con HSM para Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="932ef-297">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="932ef-298">Clase KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="932ef-298">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="932ef-299">Inicio rápido: establecimiento y recuperación de un secreto desde Azure Key Vault mediante una aplicación Web de .NET</span><span class="sxs-lookup"><span data-stu-id="932ef-299">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="932ef-300">Tutorial: uso de Azure Key Vault con máquinas virtuales Windows de Azure en .NET</span><span class="sxs-lookup"><span data-stu-id="932ef-300">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
