---
title: Derivación de subclave y cifrado autenticado en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los detalles de implementación de protección de datos de ASP.NET Core derivación de subclave y autentican el cifrado.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: bbfde378755b09cd5b1217b8cf66249b9fa1d6ad
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814387"
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Derivación de subclave y cifrado autenticado en ASP.NET Core

<a name="data-protection-implementation-subkey-derivation"></a>

La mayoría de teclas en el conjunto de claves contendrá alguna forma de entropía y tendrá algorítmico información que indique "el cifrado de modo CBC + validación HMAC" o "GCM cifrado y validación". En estos casos, nos referimos a la entropía incrustada como el material de claves principal (o KM) para esta clave y llevamos a cabo una función de derivación de claves para derivar las claves que se usará para las operaciones criptográficas reales.

> [!NOTE]
> Las claves son abstractas y una implementación personalizada no es posible que se comportan como sigue. Si la clave proporciona su propia implementación de `IAuthenticatedEncryptor` en lugar de usar una de nuestras fábricas integradas, el mecanismo descrito en esta sección ya no es aplicable.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Derivación de subclave y de datos autenticados adicionales

El `IAuthenticatedEncryptor` interfaz actúa como la interfaz principal para todas las operaciones de cifrado autenticado. Su `Encrypt` método toma dos búferes: texto sin formato y additionalAuthenticatedData (AAD). El flujo de contenido de texto simple sin modificar la llamada a `IDataProtector.Protect`, pero es generada por el sistema AAD y consta de tres componentes:

1. El encabezado mágico de 32 bits 09 F0 C9 F0 que identifica esta versión del sistema de protección de datos.

2. El identificador de clave de 128 bits.

3. Una cadena de longitud variable tiene el formato de la cadena de propósito que creó el `IDataProtector` que está realizando esta operación.

Dado que es única para la tupla de los tres componentes AAD, se puede usar para derivar nuevas claves de KM en lugar de usar KM de sí mismo en todos los de nuestras operaciones criptográficas. Para cada llamada a `IAuthenticatedEncryptor.Encrypt`, tiene lugar el proceso de derivación de claves siguiente:

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

En este caso, estamos llamando a NIST SP800-108 KDF en el modo contador (consulte [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) con los siguientes parámetros:

* Clave de derivación de claves (KDK) = K_M

* PRF = HMACSHA512

* etiqueta = additionalAuthenticatedData

* context = contextHeader || keyModifier

El encabezado de contexto es de longitud variable y actúa esencialmente como una huella digital de los algoritmos para el que nos estamos derivan K_E y K_H. El modificador de tecla es una cadena de 128 bits generada aleatoriamente para cada llamada a `Encrypt` y sirve para asegurarse de sobrecargar la probabilidad de que sean únicos para esta operación de cifrado de autenticación específico, KE y KH incluso si todas las demás entradas a KDF es constante.

Para el cifrado de modo CBC + las operaciones de validación de HMAC, | K_E | es la longitud de la clave de cifrado de bloques simétricos y | K_H | es el tamaño de la síntesis de la rutina HMAC. Para el cifrado de GCM y las operaciones de validación, | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Cifrado de modo CBC + validación HMAC

Una vez que K_E se genera mediante el mecanismo anterior, se genera un vector de inicialización aleatorio y se ejecuta el algoritmo de cifrado de bloques simétrico para cifrar el texto no cifrado. El vector de inicialización y el texto cifrado, a continuación, se ejecutan a través de la rutina HMAC que se inicializa con la clave K_H para generar el equipo Mac. Este proceso y el valor devuelto se representa gráficamente a continuación.

![Valor devuelto y proceso en modo CBC](subkeyderivation/_static/cbcprocess.png)

*output:= keyModifier || iv || E_cbc (K_E,iv,data) || HMAC(K_H, iv || E_cbc (K_E,iv,data))*

> [!NOTE]
> El `IDataProtector.Protect` implementación le [anteponer el encabezado mágico y el Id. de clave](xref:security/data-protection/implementation/authenticated-encryption-details) a salida antes de devolverla al llamador. Dado que el encabezado mágico y el Id. de clave son implícitamente forma parte de [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), y dado que el modificador de tecla se introduce como entrada a KDF, esto significa que cada byte único de la carga final devuelta es autenticado por el equipo Mac.

## <a name="galoiscounter-mode-encryption--validation"></a>El cifrado de modo contador/Galois + validación

Una vez que K_E se genera mediante el mecanismo anterior, se genere un nonce de 96 bits aleatorio y se ejecuta el algoritmo de cifrado de bloques simétrico para cifrar el texto no cifrado y generar la etiqueta a la autenticación de 128 bits.

![Valor devuelto y proceso en modo de GCM](subkeyderivation/_static/galoisprocess.png)

*salida: = keyModifier || valor nonce || E_gcm (K_E, nonce, los datos) || authTag*

> [!NOTE]
> Aunque GCM admite de forma nativa el concepto de AAD, nos estamos aún la alimentación de AAD solo KDF original, para pasar una cadena vacía a GCM para su parámetro AAD. La razón de esto es doble. Primero, [para admitir la agilidad](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) nunca deseamos usar K_M directamente como la clave de cifrado. Además, GCM impone requisitos de unicidad muy estrictos en sus entradas. La probabilidad de que la rutina de cifrado de GCM sea invocado nunca con dos o más distintos conjuntos de datos de entrada con el mismo (clave, nonce) par no debe superar los 2 ^ 32. Si se corrige K_E nos no podemos realizar más de 2 ^ 32 operaciones de cifrado antes de ejecutar que realizamos mantiene de las 2 ^ limitar -32. Esto puede parecer un gran número de operaciones, pero un servidor web de tráfico elevado puede ir a través de solicitudes de 4 mil millones en días simples, bien dentro de la duración normal de estas claves. Para estar al día de 2 ^ límite probabilidad-32, seguiremos usando un modificador de clave de 128 bits y 96 bits nonce, que amplía radicalmente el número de operaciones pueden usar para cualquier K_M determinado. Por motivos de simplicidad del diseño que compartimos la ruta de acceso del código KDF entre las operaciones CBC y GCM y, puesto que ya se considera AAD en KDF no es necesario que se reenvíe a la rutina GCM.
