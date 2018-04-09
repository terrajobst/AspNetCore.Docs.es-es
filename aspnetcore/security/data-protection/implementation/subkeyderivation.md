---
title: Subclave derivación y cifrado autenticado en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los detalles de implementación de protección de datos de ASP.NET Core subclave derivación y autentican el cifrado.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 8c83da40a524896becc07c94c01d5e2b684e4386
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Subclave derivación y cifrado autenticado en ASP.NET Core

<a name="data-protection-implementation-subkey-derivation"></a>

La mayoría de las teclas en el anillo de clave contiene alguna forma de entropía y tendrá algorítmica información que indica "cifrado de modo CBC + validación HMAC" o "cifrado de GCM + validación". En estos casos, nos referimos a la entropía incrustada como el material de creación de claves maestras (o KM) para esta clave y llevamos a cabo una función de derivación de claves para derivar las claves que se usará para las operaciones criptográficas reales.

> [!NOTE]
> Las claves son abstractas, y una implementación personalizada posible que no funcionen como sigue. Si la clave proporciona su propia implementación de `IAuthenticatedEncryptor` en lugar de usar una de nuestras fábricas integradas, el mecanismo se describe en esta sección ya no es aplicable.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Datos autenticados adicionales y subclave derivación

El `IAuthenticatedEncryptor` interfaz actúa como la interfaz básica para todas las operaciones de cifrado autenticado. Su `Encrypt` método toma dos búferes: texto sin formato y additionalAuthenticatedData (AAD). El flujo de contenido de texto simple sin modificar la llamada a `IDataProtector.Protect`, pero el AAD generada por el sistema y consta de tres componentes:

1. El encabezado de mágico de 32 bits 09 F0 C9 F0 que identifica esta versión del sistema de protección de datos.

2. El identificador de clave de 128 bits.

3. Una cadena de longitud variable formado a partir de la cadena de fin que creó el `IDataProtector` que está realizando esta operación.

Dado que el AAD es única para la tupla de los tres componentes, podemos usar se pueden para derivar nuevas claves KM en lugar de usar KM propio en todos nuestros operaciones de cifrado. Para todas las llamadas a `IAuthenticatedEncryptor.Encrypt`, realiza el proceso de derivación de claves siguiente:

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

En este caso, estamos llamando a KDF SP800-108 NIST en modo de contador (vea [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) con los siguientes parámetros:

* Clave de derivación de claves (KDK) = K_M

* PRF = HMACSHA512

* label = additionalAuthenticatedData

* contexto = contextHeader || keyModifier

El encabezado de contexto es de longitud variable y actúa esencialmente como una huella digital de los algoritmos para el que nos estamos derivación K_E y K_H. El modificador de clave es una cadena de 128 bits que se genera de forma aleatoria para cada llamada a `Encrypt` y sirve para asegurarse de con una sobrecarga de probabilidad que KE y KH son únicos para esta operación de cifrado de autenticación específico, incluso si todos los demás entrada KDF es constante.

Para el cifrado del modo CBC + las operaciones de validación de HMAC, | K_E | es la longitud de la clave de cifrado de bloques simétrico y | K_H | es el tamaño de resumen de la rutina HMAC. Para el cifrado de GCM + las operaciones de validación, | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Cifrado del modo CBC + validación HMAC

Una vez K_E se genera mediante el mecanismo anterior, se genera un vector de inicialización aleatorio y ejecutar el algoritmo de cifrado de bloques simétrico para cifrar el texto sin formato. El vector de inicialización y el texto cifrado, a continuación, se ejecutan a través de la rutina HMAC que se inicializa con la clave K_H para generar el equipo Mac. Este proceso y el valor devuelto se representa gráficamente a continuación.

![Valor devuelto y el proceso de modo CBC](subkeyderivation/_static/cbcprocess.png)

*output:= keyModifier || iv || E_cbc (K_E,iv,data) || HMAC(K_H, iv || E_cbc (K_E,iv,data))*

> [!NOTE]
> El `IDataProtector.Protect` implementación le [anteponer el encabezado mágica y el Id. de clave](xref:security/data-protection/implementation/authenticated-encryption-details) a salida antes de devolverlo al llamador. Dado que el encabezado mágica y el Id. de clave son implícitamente forma parte de [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), y dado que el modificador de tecla se introduce como entrada a KDF, esto significa que cada byte único de la última carga devuelta es autenticado por el equipo Mac.

## <a name="galoiscounter-mode-encryption--validation"></a>El cifrado del modo de Galois/contador + validación

Una vez K_E se genera mediante el mecanismo anterior, se genera un valor aleatorio de 96 bits nonce y ejecutar el algoritmo de cifrado de bloques simétrico para cifrar el texto sin formato y generar la etiqueta de autenticación de 128 bits.

![Valor devuelto y proceso en modo de GCM](subkeyderivation/_static/galoisprocess.png)

*salida: = keyModifier || nonce || E_gcm (K_E, nonce, de datos) || authTag*

> [!NOTE]
> Aunque GCM forma nativa es compatible con el concepto de AAD, nos estamos todavía alimentación AAD solo KDF original, para pasar una cadena vacía a GCM para su parámetro AAD. La razón para esto es dos vertientes. En primer lugar, [para admitir la agilidad](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) nunca queremos usar K_M directamente como la clave de cifrado. Además, GCM impone requisitos de unicidad muy estrictos en sus entradas. La probabilidad de que la rutina de cifrado de GCM alguna vez invocado con dos o más distintos conjuntos de datos de entrada con el mismo (clave, nonce) par no debe superar los 2 ^ 32. Si se soluciona K_E no podemos realizar más de 2 ^ 32 operaciones de cifrado antes de que se ejecute mantiene del 2 ^ limitar -32. Esto puede parecer un gran número de operaciones, pero un servidor web de tráfico elevado puede ir a través de solicitudes de 4 mil millones en días simples, bien dentro de la duración normal de estas claves. A estar al día de 2 ^ límite de probabilidad-32, seguimos utilizar un modificador de clave de 128 bits y 96 bits nonce, que extiende radicalmente el número de operaciones puede usar para cualquier K_M determinado. Para simplificar el trabajo de diseño compartimos la ruta de acceso del código KDF entre las operaciones de cifrado CBC y GCM y, puesto que ya se considera AAD en KDF no es necesario que se reenvíe a la rutina GCM.
