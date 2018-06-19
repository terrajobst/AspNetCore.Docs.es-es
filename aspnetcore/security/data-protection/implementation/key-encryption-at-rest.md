---
title: Cifrado de claves en reposo en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los detalles de implementación del cifrado de clave de protección de datos de ASP.NET Core en reposo.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: e5082d831dd4822fad0fb3211fe2b8c76ff967bf
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851123"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="198cf-103">Cifrado de claves en reposo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="198cf-103">Key encryption at rest in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="198cf-104">De forma predeterminada, el sistema de protección de datos [emplea un método heurístico](xref:security/data-protection/configuration/default-settings) para determinar cómo criptográfico material de clave debe cifrarse en reposo.</span><span class="sxs-lookup"><span data-stu-id="198cf-104">By default, the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="198cf-105">El desarrollador puede invalidar la heurística y especificar manualmente cómo se deben cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="198cf-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="198cf-106">Si especifica un cifrado de clave explícita en el mecanismo de rest, el sistema de protección de datos se anular el registro el mecanismo de almacenamiento de claves predeterminado que proporciona la heurística.</span><span class="sxs-lookup"><span data-stu-id="198cf-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="198cf-107">Debe [especifican un mecanismo de almacenamiento de claves explícitas](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), en caso contrario, no podrá iniciar el sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="198cf-107">You must [specify an explicit key storage mechanism](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="198cf-108">El sistema de protección de datos que se suministra con tres mecanismos de cifrado de claves de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="198cf-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="198cf-109">DPAPI de Windows</span><span class="sxs-lookup"><span data-stu-id="198cf-109">Windows DPAPI</span></span>

<span data-ttu-id="198cf-110">*Este mecanismo solo está disponible en Windows.*</span><span class="sxs-lookup"><span data-stu-id="198cf-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="198cf-111">Cuando se utiliza DPAPI de Windows, material de clave se cifrará a través de [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) antes de que se conservan en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="198cf-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="198cf-112">DPAPI es un mecanismo de cifrado adecuado para los datos que nunca se leerán fuera de la máquina actual (aunque es posible hacer copia de estas claves en Active Directory; vea [DPAPI y perfiles móviles](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="198cf-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="198cf-113">Por ejemplo configurar el cifrado de clave en el resto DPAPI.</span><span class="sxs-lookup"><span data-stu-id="198cf-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

<span data-ttu-id="198cf-114">Si `ProtectKeysWithDpapi` se llaman sin ningún parámetro, sólo la cuenta de usuario de Windows actual puede descifrar el material de clave persistente.</span><span class="sxs-lookup"><span data-stu-id="198cf-114">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="198cf-115">Opcionalmente, puede especificar que cualquier cuenta de usuario en el equipo (no solo la cuenta de usuario actual) debe ser capaz de descifrar el material de clave, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="198cf-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a><span data-ttu-id="198cf-116">Certificado X.509</span><span class="sxs-lookup"><span data-stu-id="198cf-116">X.509 certificate</span></span>

<span data-ttu-id="198cf-117">*Este mecanismo no está disponible en `.NET Core 1.0` o `1.1`.*</span><span class="sxs-lookup"><span data-stu-id="198cf-117">*This mechanism isn't available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="198cf-118">Si la aplicación se reparte entre varias máquinas, puede ser conveniente para distribuir un certificado X.509 compartido entre las máquinas y configurar aplicaciones para usar este certificado para el cifrado de claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="198cf-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="198cf-119">Vea a continuación para obtener un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="198cf-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

<span data-ttu-id="198cf-120">Debido a limitaciones de .NET Framework se admiten sólo los certificados con claves privadas de CAPI.</span><span class="sxs-lookup"><span data-stu-id="198cf-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="198cf-121">Vea [basada en certificados de cifrado con Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) a continuación para buscar posibles soluciones para estas limitaciones.</span><span class="sxs-lookup"><span data-stu-id="198cf-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="198cf-122">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="198cf-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="198cf-123">*Este mecanismo solo está disponible en Windows 8 / Windows Server 2012 y versiones posterior.*</span><span class="sxs-lookup"><span data-stu-id="198cf-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="198cf-124">A partir de Windows 8, el sistema operativo admite DPAPI-NG (también denominado CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="198cf-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="198cf-125">Microsoft distribuye su escenario de uso como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="198cf-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="198cf-126">Informática en nube, sin embargo, a menudo requiere que ese contenido cifrado en un equipo pueden descifrar en otro.</span><span class="sxs-lookup"><span data-stu-id="198cf-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="198cf-127">Por lo tanto, a partir de Windows 8, Microsoft ampliado la idea de usar una API relativamente sencilla para abarcar escenarios de nube.</span><span class="sxs-lookup"><span data-stu-id="198cf-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="198cf-128">Esta nueva API, denominada DPAPI-NG, permite compartir de forma segura secretos (claves, contraseñas, material de clave) y mensajes protegiéndolos a un conjunto de entidades de seguridad que puede utilizarse para desproteger en equipos diferentes después de la autorización y la autenticación correcta.</span><span class="sxs-lookup"><span data-stu-id="198cf-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="198cf-129">Desde [sobre DPAPI CNG](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="198cf-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="198cf-130">La entidad de seguridad se codifica como una regla de descriptor de protección.</span><span class="sxs-lookup"><span data-stu-id="198cf-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="198cf-131">Considere el ejemplo siguiente, que cifra el material de clave de modo que solo el usuario unido al dominio con el SID especificado puede descifrar el material de clave.</span><span class="sxs-lookup"><span data-stu-id="198cf-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="198cf-132">También hay una sobrecarga sin parámetros de `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="198cf-132">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="198cf-133">Se trata de un método útil para especificar la regla "SID = mío", donde la extraiga es el SID de la cuenta de usuario de Windows actual.</span><span class="sxs-lookup"><span data-stu-id="198cf-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

<span data-ttu-id="198cf-134">En este escenario, el controlador de dominio de AD es responsable de distribuir las claves de cifrado que se utiliza en las operaciones de NG DPAPI.</span><span class="sxs-lookup"><span data-stu-id="198cf-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="198cf-135">El usuario de destino podrá descifrar la carga cifrada desde cualquier equipo unido al dominio (siempre que el proceso se ejecuta bajo su identidad).</span><span class="sxs-lookup"><span data-stu-id="198cf-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="198cf-136">Cifrado basada en certificados con Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="198cf-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="198cf-137">Si se está ejecutando en Windows 8.1 / Windows Server 2012 R2 o versiones posteriores, puede usar Windows DPAPI-NG para realizar el cifrado basada en certificados, incluso si la aplicación se ejecuta en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="198cf-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on .NET Core.</span></span> <span data-ttu-id="198cf-138">Para aprovechar estas características, utilice la cadena de descriptor de la regla "certificado = HashId:thumbprint", donde la huella digital es la huella digital con codificación hexadecimal SHA1 del certificado que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="198cf-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="198cf-139">Vea a continuación para obtener un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="198cf-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="198cf-140">Cualquier aplicación que se señala en este repositorio debe ejecutarse en Windows 8.1 / Windows Server 2012 R2 o posterior para que pueda descifrar esta clave.</span><span class="sxs-lookup"><span data-stu-id="198cf-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="198cf-141">Cifrado de clave personalizado</span><span class="sxs-lookup"><span data-stu-id="198cf-141">Custom key encryption</span></span>

<span data-ttu-id="198cf-142">Si no resultan adecuados los mecanismos de forma predeterminada, el programador puede especificar su propio mecanismo de cifrado de claves proporcionando un personalizado `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="198cf-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom `IXmlEncryptor`.</span></span>
