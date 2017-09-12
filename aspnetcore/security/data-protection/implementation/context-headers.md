---
title: Encabezados de contexto
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 7befd983f6a45839868639708ec5cf45bf2df35f
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="context-headers"></a><span data-ttu-id="a9c5c-103">Encabezados de contexto</span><span class="sxs-lookup"><span data-stu-id="a9c5c-103">Context headers</span></span>

<a name=data-protection-implementation-context-headers></a>

## <a name="background-and-theory"></a><span data-ttu-id="a9c5c-104">Segundo plano y la teoría</span><span class="sxs-lookup"><span data-stu-id="a9c5c-104">Background and theory</span></span>

<span data-ttu-id="a9c5c-105">En el sistema de protección de datos, una "clave" significa autenticado de un objeto que puede proporcionar servicios de cifrado.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="a9c5c-106">Cada clave está identificada por un identificador único (GUID) y lleva con él algorítmica información y al material entropic.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="a9c5c-107">Se pretende que cada clave llevar entropía único, pero el sistema no puede exigir y también es necesario tener en cuenta para los desarrolladores que cambiaría el anillo de clave manualmente mediante la modificación de la información de una clave existente en el anillo de clave algorítmica.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-107">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="a9c5c-108">Para lograr los requisitos de seguridad tiene estos casos, el sistema de protección de datos tiene un concepto de [agilidad criptográfica](https://www.microsoft.com/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D121045), que permite de forma segura mediante un único valor entropic entre varios algoritmos criptográficos.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D121045), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="a9c5c-109">Mayoría de los sistemas que son compatibles con agilidad criptográfica hacerlo mediante la inclusión de cierta información de identificación sobre el algoritmo en la carga.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="a9c5c-110">OID del algoritmo suele ser un buen candidato para esto.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="a9c5c-111">Sin embargo, un problema que encontramos es que hay varias maneras de especificar el mismo algoritmo: "AES" (CNG) y los administrados Aes, AesManaged, AesCryptoServiceProvider, AesCng y RijndaelManaged (determinados parámetros específicos) clases todo realmente son las mismas lo y se tendría que mantener una asignación de todos estos para el OID correcto.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="a9c5c-112">Si un desarrollador desea proporcionar un algoritmo personalizado (o incluso otra implementación de AES), tendría que Díganos su OID.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="a9c5c-113">Este paso de registro adicional, la configuración de sistema es especialmente muy complicada.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="a9c5c-114">Ejecución paso a paso atrás, decidimos que estábamos se está aproximando al problema de la dirección equivocada.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="a9c5c-115">Un OID indica cuál es el algoritmo, pero se no realmente le interesa esto.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="a9c5c-116">Si se necesita usar un único valor entropic de forma segura en los dos algoritmos diferentes, no es necesario para que podamos saber cuáles son en realidad los algoritmos.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="a9c5c-117">¿Qué nos realmente importa es su comportamiento.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="a9c5c-118">Cualquier algoritmo de cifrado de bloques simétrico decente también es una permutación pseudoaleatoria segura (PRP): corrija las entradas (clave, el encadenamiento de texto simple de modo, IV) y la salida de texto cifrado con una sobrecarga probabilidad será distinta de cualquier otro cifrado por bloques simétrico algoritmo dada las entradas de la mismas.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="a9c5c-119">Del mismo modo, cualquier función de hash con clave decente también es una función pseudoaleatoria segura (PRF), y debido a un conjunto de entrada fijo su salida muy será distinta de cualquier otra función de hash con clave.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="a9c5c-120">Este concepto de PRPs y PRFs seguros se usa para crear un encabezado de contexto.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="a9c5c-121">Este encabezado de contexto actúa esencialmente como una huella digital estable sobre los algoritmos en uso para una operación determinada, así como la agilidad criptográfica necesaria para el sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="a9c5c-122">Este encabezado es "reproducible" y se utiliza posteriormente como parte de la [proceso de derivación de la subclave](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="a9c5c-122">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="a9c5c-123">Hay dos maneras diferentes para generar el encabezado de contexto de función de los modos de funcionamiento de los algoritmos subyacentes.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="a9c5c-124">Cifrado del modo CBC + autenticación HMAC</span><span class="sxs-lookup"><span data-stu-id="a9c5c-124">CBC-mode encryption + HMAC authentication</span></span>

<a name=data-protection-implementation-context-headers-cbc-components></a>

<span data-ttu-id="a9c5c-125">El encabezado de contexto está formada por los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="a9c5c-126">[16 bits] El valor 00 00, que es un marcador de lo que significa "cifrado CBC + autenticación HMAC".</span><span class="sxs-lookup"><span data-stu-id="a9c5c-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="a9c5c-127">[32 bits] La longitud de clave (en bytes, big-endian) del algoritmo de cifrado de bloques simétrico.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="a9c5c-128">[32 bits] El tamaño de bloque (en bytes, big-endian) del algoritmo de cifrado de bloques simétrico.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="a9c5c-129">[32 bits] La longitud de clave (en bytes, big-endian) del algoritmo HMAC.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="a9c5c-130">(Actualmente el tamaño de clave siempre coincide con el tamaño de texto implícita.)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="a9c5c-131">[32 bits] El tamaño de texto implícita (en bytes, big-endian) del algoritmo HMAC.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="a9c5c-132">EncCBC (K_E, IV, ""), que es el resultado del algoritmo de cifrado de bloques simétrico dado una entrada de cadena vacía y donde IV es un vector de ceros.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="a9c5c-133">La construcción de K_E se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="a9c5c-134">MAC (K_H, ""), que es el resultado del algoritmo HMAC dado una entrada de cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="a9c5c-135">La construcción de K_H se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="a9c5c-136">Lo ideal es que podemos pasar como vectores de ceros para K_E y K_H.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-136">Ideally we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="a9c5c-137">Sin embargo, debe evitar la situación donde el algoritmo subyacente comprueba la existencia de claves débiles antes de realizar cualquier operación (especialmente DES y 3DES), lo que impide utilizar un modelo simple o repeatable como un vector de ceros.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="a9c5c-138">En su lugar, usamos NIST SP800-108 KDF en modo de contador (vea [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) con una clave de longitud cero, etiqueta y contexto y HMACSHA512 como el PRF subyacente.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="a9c5c-139">Se derivan | K_E | + | K_H | bytes de salida, a continuación, descomponer el resultado en K_E y K_H por sí mismos.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="a9c5c-140">Matemáticamente, se representa como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="a9c5c-141">(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", etiqueta = "", contexto = "")</span><span class="sxs-lookup"><span data-stu-id="a9c5c-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="a9c5c-142">Ejemplo: AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="a9c5c-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="a9c5c-143">Por ejemplo, considere el caso donde el algoritmo de cifrado de bloques simétrico es AES-192-CBC y el algoritmo de validación es HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="a9c5c-144">El sistema generaría el encabezado de contexto mediante los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="a9c5c-145">En primer lugar, se permiten (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", etiqueta = "", contexto = ""), donde | K_E | = 192 bits y | K_H | = 256 bits por los algoritmos especificados.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="a9c5c-146">Esto conduce al K_E = 5BB6... 21DD y K_H = A04A... 00A9 en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
   61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
   49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
   B7 92 3D BF 59 90 00 A9
   ```

<span data-ttu-id="a9c5c-147">A continuación, calcular Enc_CBC (K_E, IV, "") de AES-192-CBC dado IV = 0 * y K_E como anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="a9c5c-148">resultado: = F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="a9c5c-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="a9c5c-149">A continuación, calcular MAC (K_H, "") para HMACSHA256 dado K_H como anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="a9c5c-150">resultado: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="a9c5c-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="a9c5c-151">Esto produce el encabezado de contexto completo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-151">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
   00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
   DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
   8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
   22 0C
   ```

<span data-ttu-id="a9c5c-152">Este encabezado de contexto es la huella digital del par de algoritmo de cifrado autenticado (cifrado de AES-192-CBC + HMACSHA256 validación).</span><span class="sxs-lookup"><span data-stu-id="a9c5c-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="a9c5c-153">Los componentes, como se describe [anteriormente](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) son:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="a9c5c-154">el marcador (00 00)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-154">the marker (00 00)</span></span>

* <span data-ttu-id="a9c5c-155">la longitud de clave de cifrado de bloque (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="a9c5c-156">el tamaño de bloque de cifrado de bloque (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="a9c5c-157">la longitud de clave de HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="a9c5c-158">el tamaño de la síntesis HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="a9c5c-159">el cifrado por bloques salida PRP (F4 74 - DB 6F) y</span><span class="sxs-lookup"><span data-stu-id="a9c5c-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="a9c5c-160">la salida de HMAC PRF (D4 79 - final).</span><span class="sxs-lookup"><span data-stu-id="a9c5c-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="a9c5c-161">El cifrado del modo CBC + HMAC encabezado de contexto de autenticación se basa en la misma forma, independientemente de si se proporcionan las implementaciones de algoritmos CNG de Windows o tipos administrados SymmetricAlgorithm y KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="a9c5c-162">Esto permite que aplicaciones que se ejecutan en sistemas operativos diferentes generar de forma confiable el mismo encabezado de contexto, aunque las implementaciones de los algoritmos difieren entre sistemas operativos.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="a9c5c-163">(En la práctica, la KeyedHashAlgorithm no tiene que ser un HMAC correcto.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="a9c5c-164">Puede ser cualquier tipo de algoritmo hash con clave.)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="a9c5c-165">Ejemplo: 3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="a9c5c-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="a9c5c-166">En primer lugar, se permiten (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", etiqueta = "", contexto = ""), donde | K_E | = 192 bits y | K_H | = 160 bits por los algoritmos especificados.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="a9c5c-167">Esto conduce al K_E = A219... E2BB y K_H = DC4A... B464 en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
   61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
   D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
   ```

<span data-ttu-id="a9c5c-168">A continuación, calcular Enc_CBC (K_E, IV, "") para 3DES-192-CBC dado IV = 0 * y K_E como anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="a9c5c-169">resultado: = ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="a9c5c-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="a9c5c-170">A continuación, calcular MAC (K_H, "") para HMACSHA1 dado K_H como anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="a9c5c-171">resultado: = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="a9c5c-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="a9c5c-172">Esto genera el encabezado de contexto completo que es una huella digital de los autenticados par de algoritmo de cifrado (cifrado 3DES-192-CBC + validación HMACSHA1), se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
   00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
   03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
   ```

<span data-ttu-id="a9c5c-173">Los componentes se dividen como sigue:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-173">The components break down as follows:</span></span>

* <span data-ttu-id="a9c5c-174">el marcador (00 00)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-174">the marker (00 00)</span></span>

* <span data-ttu-id="a9c5c-175">la longitud de clave de cifrado de bloque (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="a9c5c-176">el tamaño de bloque de cifrado de bloque (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="a9c5c-177">la longitud de clave de HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="a9c5c-178">el tamaño de la síntesis HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="a9c5c-179">el cifrado por bloques salida PRP (B1 AB - E1 0E) y</span><span class="sxs-lookup"><span data-stu-id="a9c5c-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="a9c5c-180">la salida de HMAC PRF (76 EB - final).</span><span class="sxs-lookup"><span data-stu-id="a9c5c-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="a9c5c-181">El cifrado del modo de Galois/contador + autenticación</span><span class="sxs-lookup"><span data-stu-id="a9c5c-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="a9c5c-182">El encabezado de contexto está formada por los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="a9c5c-183">[16 bits] El valor 00 01, que es un marcador de lo que significa "cifrado de GCM + autenticación".</span><span class="sxs-lookup"><span data-stu-id="a9c5c-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="a9c5c-184">[32 bits] La longitud de clave (en bytes, big-endian) del algoritmo de cifrado de bloques simétrico.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="a9c5c-185">[32 bits] El tamaño (en bytes, big-endian) nonce que usa durante las operaciones de cifrado autenticado.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="a9c5c-186">(En nuestro sistema, esto se fija en tamaño nonce = 96 bits.)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="a9c5c-187">[32 bits] El tamaño de bloque (en bytes, big-endian) del algoritmo de cifrado de bloques simétrico.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="a9c5c-188">(Para GCM, esto se fija en el tamaño de bloque = 128 bits.)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="a9c5c-189">[32 bits] La autenticación etiqueta tamaño (en bytes, big-endian) creado por la función de cifrado autenticado.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="a9c5c-190">(En nuestro sistema, esto se fija en el tamaño de la etiqueta = 128 bits.)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="a9c5c-191">[128 bits] La etiqueta de Enc_GCM (K_E, nonce, ""), que es el resultado del algoritmo de cifrado de bloques simétrico dado una entrada de cadena vacía y donde nonce es un vector de ceros de 96 bits.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="a9c5c-192">K_E se deduce usando el mismo mecanismo como en el cifrado CBC + el escenario de autenticación de HMAC.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="a9c5c-193">Sin embargo, puesto que no hay ninguna K_H en play aquí, se suelen tener | K_H | = 0, y el algoritmo se contrae en el siguiente formulario.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-193">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="a9c5c-194">K_E = SP800_108_CTR (prf = HMACSHA512, key = "", etiqueta = "", contexto = "")</span><span class="sxs-lookup"><span data-stu-id="a9c5c-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="a9c5c-195">Ejemplo: AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="a9c5c-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="a9c5c-196">En primer lugar, permiten K_E = SP800_108_CTR (prf = HMACSHA512, key = "", etiqueta = "", contexto = ""), donde | K_E | = 256 bits.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="a9c5c-197">K_E: = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="a9c5c-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="a9c5c-198">A continuación, calcular la etiqueta de autenticación de Enc_GCM (K_E, nonce, "") de AES-256-GCM dado nonce = 096 y K_E como anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a9c5c-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="a9c5c-199">resultado: = E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="a9c5c-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="a9c5c-200">Esto produce el encabezado de contexto completo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-200">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
   00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
   BE 45
   ```

<span data-ttu-id="a9c5c-201">Los componentes se dividen como sigue:</span><span class="sxs-lookup"><span data-stu-id="a9c5c-201">The components break down as follows:</span></span>

   * <span data-ttu-id="a9c5c-202">el marcador (00 01)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-202">the marker (00 01)</span></span>

   * <span data-ttu-id="a9c5c-203">la longitud de clave de cifrado de bloque (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-203">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="a9c5c-204">el tamaño del valor de seguridad (00 00 00 0c)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-204">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="a9c5c-205">el tamaño de bloque de cifrado de bloque (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="a9c5c-205">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="a9c5c-206">el tamaño de la etiqueta de autenticación (00 00 00 10) y</span><span class="sxs-lookup"><span data-stu-id="a9c5c-206">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="a9c5c-207">la etiqueta de autenticación el cifrado de bloques de ejecución (controlador de dominio E7 - final).</span><span class="sxs-lookup"><span data-stu-id="a9c5c-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
