---
title: Encabezados de contexto en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los detalles de implementación de los encabezados de contexto de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 518423f5df93924d3df144994e4beb1755cd0bfc
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814013"
---
# <a name="context-headers-in-aspnet-core"></a>Encabezados de contexto en ASP.NET Core

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>En segundo plano y la teoría

En el sistema de protección de datos, una "clave" significa autenticado de un objeto que puede proporcionar servicios de cifrado. Cada clave se identifica mediante un identificador único (GUID) y lo lleva información algorítmica y material entropic. Se pretende que llevar a cada clave única entropía, pero no puede aplicar el sistema, y también es necesario tener en cuenta los desarrolladores que podrían cambiar el conjunto de claves manualmente mediante la modificación de la información de una clave existente en el conjunto de claves algorítmica. Para lograr los requisitos de seguridad dados en estos casos, el sistema de protección de datos tiene un concepto de [agilidad criptográfica](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), lo que permite de forma segura con un único valor entropic entre varios algoritmos criptográficos.

La mayoría de los sistemas que son compatibles con agilidad criptográfica hacerlo mediante la inclusión de cierta información de identificación sobre el algoritmo dentro de la carga. OID del algoritmo suele ser un buen candidato para esto. Sin embargo, un problema que nos enfrentamos es que hay varias maneras de especificar el mismo algoritmo: "AES" (CNG) y el administrado Aes, AesManaged, AesCryptoServiceProvider, AesCng y RijndaelManaged (determinados parámetros específicos) las clases son realmente todo lo mismo, y sería necesario mantener una asignación de todos ellos al OID correcto. Si un desarrollador desea proporcionar un algoritmo personalizado (o incluso otra implementación de AES), tendría para indicarnos su OID. Este paso de registro adicional hace que la configuración del sistema que especialmente complicado.

Volviendo atrás, decidimos que nos estábamos abordar el problema de la dirección equivocada. Un OID indica cuál es el algoritmo, pero no nos realmente interesa esto. Si es necesario usar un único valor entropic de forma segura en dos diferentes algoritmos, no es necesario para que podamos saber cuáles son realmente los algoritmos. Lo que realmente importa es que su comportamiento. Cualquier algoritmo de cifrado de bloques simétrico decente también es una permutación pseudoaleatorio segura (PRP): corrija las entradas (clave, texto simple, IV, el modo de encadenamiento) y la salida de texto cifrado con una sobrecarga de probabilidad sea distinta de cualquier otro cifrado por bloques simétrico algoritmo dada las entradas de la mismas. De forma similar, cualquier función de hash con clave decente también es una función pseudoaleatorio segura (PRF) y, dado un conjunto fijo de entrada su salida inmensa mayoría será distinta de cualquier otra función de hash con clave.

Este concepto de seguro PRPs y PRFs se usa para crear un encabezado de contexto. Este encabezado de contexto actúa esencialmente como una huella digital del estable a través de los algoritmos en uso para una operación determinada y proporciona la agilidad criptográfica necesaria para el sistema de protección de datos. Este encabezado es reproducible y se utiliza posteriormente como parte de la [proceso de derivación de subclave](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). Hay dos maneras de crear el encabezado de contexto dependiendo de los modos de funcionamiento de los algoritmos subyacentes.

## <a name="cbc-mode-encryption--hmac-authentication"></a>Cifrado de modo CBC + autenticación HMAC

<a name="data-protection-implementation-context-headers-cbc-components"></a>

El encabezado de contexto se compone de los siguientes componentes:

* [16 bits] El valor 00 00, que es un marcador de lo que significa "cifrado CBC + autenticación HMAC".

* [32 bits] La longitud de clave (en bytes, big-endian) del algoritmo de cifrado de bloques simétricos.

* [32 bits] El tamaño de bloque (en bytes, big-endian) del algoritmo de cifrado de bloques simétricos.

* [32 bits] La longitud de clave (en bytes, big-endian) del algoritmo HMAC. (Actualmente el tamaño de clave siempre coincide con el tamaño de texto implícita.)

* [32 bits] El tamaño de la síntesis (en bytes, big-endian) del algoritmo HMAC.

* EncCBC (K_E, IV, ""), que es el resultado del algoritmo de cifrado de bloques simétrico según una entrada de cadena vacía y donde el vector de inicialización es un vector de ceros. A continuación se describe la construcción de K_E.

* MAC (K_H, ""), que es el resultado del algoritmo HMAC según una entrada de cadena vacía. A continuación se describe la construcción de K_H.

Idealmente, podemos pasar vectores ceros para K_E y K_H. Sin embargo, queremos evitar la situación donde el algoritmo subyacente comprueba la existencia de claves débiles antes de realizar cualquier operación (especialmente DES y 3DES), que impide utilizando un modelo repetible o simple como un vector de ceros.

En su lugar, usamos NIST SP800-108 KDF en el modo contador (consulte [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) con una clave de longitud cero, etiqueta y contexto y HMACSHA512 como el PRF subyacente. Se derivan | K_E | + | K_H | bytes de salida, a continuación, descomponer el resultado en K_E y K_H a sí mismos. Matemáticamente, esto se representa como sigue.

( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Ejemplo: AES-192-CBC + HMACSHA256

Por ejemplo, considere el caso donde el algoritmo de cifrado de bloques simétrico es AES-CBC-192 y el algoritmo de validación es HMACSHA256. El sistema generaría el encabezado de contexto mediante los pasos siguientes.

First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms. Esto conduce a K_E = 5BB6... 21DD y K_H = A04A... 00A9 en el ejemplo siguiente:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

A continuación, calcular Enc_CBC (K_E, IV, "") de AES-192-CBC dado IV = 0 * y K_E anterior.

result := F474B1872B3B53E4721DE19C0841DB6F

A continuación, calcular MAC (K_H, "") para HMACSHA256 dado K_H anterior.

resultado: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

Esto genera el encabezado de contexto siguiente:

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Este encabezado de contexto es la huella digital del par de algoritmo de cifrado autenticado (cifrado AES-192-CBC + HMACSHA256 validación). Los componentes, como se describe [anteriormente](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) son:

* el marcador (00 00)

* la longitud de clave de cifrado de bloque (00 00 00 18)

* el tamaño de bloque de cifrado de bloque (00 00 00 10)

* la longitud de clave de HMAC (00 00 00 20)

* el tamaño de la síntesis HMAC (00 00 00 20)

* el cifrado de bloques salida PRP (74 F4 - DB 6F) y

* la salida de PRF de HMAC (D4 79 - final).

> [!NOTE]
> El cifrado de modo CBC + HMAC encabezado de contexto de autenticación se basa en la misma manera independientemente de si se proporcionan las implementaciones de algoritmos CNG de Windows o los tipos administrados SymmetricAlgorithm y KeyedHashAlgorithm. Esto permite que las aplicaciones que se ejecutan en diferentes sistemas operativos producir el mismo encabezado de contexto de forma confiable, aunque las implementaciones de los algoritmos se diferencian entre los sistemas operativos. (En la práctica, la KeyedHashAlgorithm no tiene que ser un HMAC adecuado. Puede ser cualquier tipo de algoritmo hash con clave.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Ejemplo: 3DES-192-CBC + HMACSHA1

First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms. Esto conduce a K_E = A219... E2BB y K_H = DC4A... B464 en el ejemplo siguiente:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

A continuación, calcular Enc_CBC (K_E, IV, "") para 3DES-192-CBC dado IV = 0 * y K_E anterior.

resultado: = ABB100F81E53E10E

A continuación, calcular MAC (K_H, "") para HMACSHA1 dado K_H anterior.

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Esto genera el encabezado de contexto que es una huella digital de los autenticados par de algoritmo de cifrado (cifrado 3DES-192-CBC + validación HMACSHA1), se muestra a continuación:

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

Los componentes se dividen como sigue:

* el marcador (00 00)

* la longitud de clave de cifrado de bloque (00 00 00 18)

* el tamaño de bloque de cifrado de bloque (00 00 00 08)

* la longitud de clave de HMAC (00 00 00 14)

* el tamaño de la síntesis HMAC (00 00 00 14)

* el cifrado de bloques salida PRP (B1 AB - E1 0E) y

* la salida de PRF de HMAC (76 EB - final).

## <a name="galoiscounter-mode-encryption--authentication"></a>El cifrado de modo contador/Galois + autenticación

El encabezado de contexto se compone de los siguientes componentes:

* [16 bits] El valor 00 01, que es un marcador de lo que significa "cifrado de GCM + autenticación".

* [32 bits] La longitud de clave (en bytes, big-endian) del algoritmo de cifrado de bloques simétricos.

* [32 bits] El tamaño (en bytes, big-endian) nonce que usa durante las operaciones de cifrado autenticado. (Para nuestro sistema, esto se ha corregido en tamaño nonce = 96 bits.)

* [32 bits] El tamaño de bloque (en bytes, big-endian) del algoritmo de cifrado de bloques simétricos. (Para GCM, esto se fija en el tamaño de bloque = 128 bits.)

* [32 bits] La autenticación tamaño de etiqueta (en bytes, big-endian) creado por la función de cifrado autenticado. (Para nuestro sistema, esto se fija en el tamaño de etiqueta = 128 bits.)

* [128 bits] La etiqueta de Enc_GCM (K_E, nonce, ""), que es el resultado del algoritmo de cifrado de bloques simétrico según una entrada de cadena vacía y donde nonce es un vector de ceros de 96 bits.

K_E se deduce usando el mismo mecanismo como se muestra en el cifrado CBC + el escenario de autenticación de HMAC. Sin embargo, puesto que no hay ningún K_H en juego aquí, básicamente, tenemos | K_H | = 0, y el algoritmo se contrae en el siguiente formulario.

K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-256-gcm"></a>Ejemplo: AES-256-GCM

First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

A continuación, calcular la etiqueta a la autenticación de Enc_GCM (K_E, nonce, "") de AES-256-GCM dado nonce = 096 y K_E anterior.

resultado: = E7DCCE66DF855A323A6BB7BD7A59BE45

Esto genera el encabezado de contexto siguiente:

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

Los componentes se dividen como sigue:

* el marcador (00 01)

* la longitud de clave de cifrado de bloque (00 00 00 20)

* el tamaño de nonce (00 00 00 0c)

* el tamaño de bloque de cifrado de bloque (00 00 00 10)

* el tamaño de la etiqueta de autenticación (00 00 00 10) y

* la etiqueta a la autenticación de la ejecución el cifrado de bloques (controlador de dominio E7 - final).
