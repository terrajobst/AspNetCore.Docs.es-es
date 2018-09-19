---
title: Protección de datos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre el concepto de protección de datos y los principios de diseño de las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/introduction
ms.openlocfilehash: a49eee89e8c11b26c76ba167215c141482159933
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292302"
---
# <a name="aspnet-core-data-protection"></a>Protección de datos de ASP.NET Core

Las aplicaciones Web necesitan a menudo almacenar datos confidenciales de seguridad. Windows proporciona DPAPI para aplicaciones de escritorio, pero esto no es apropiada para las aplicaciones web. La pila de protección de datos de ASP.NET Core proporcionan una API criptográfica sencilla y fácil de usar un programador puede utilizar para proteger los datos, incluida la rotación y la administración de claves.

La pila de protección de datos de ASP.NET Core está diseñada para que actúe como el sustituto a largo plazo para el &lt;machineKey&gt; elemento en ASP.NET 1.x - 4.x. Se diseñó para tratar muchos de los defectos de la pila antigua criptográfico al tiempo que proporciona una solución para la mayoría de casos de uso de aplicaciones modernas es probable que encuentre.

## <a name="problem-statement"></a>Declaración del problema

En pocas palabras la declaración del problema general en una sola frase: necesito conservar información de confianza para su recuperación posterior, pero no confía en el mecanismo de persistencia. En términos de web, esto podría escribirse como "Necesito para el estado de confianza de ida y vuelta a través de un cliente que no se confía".

El ejemplo canónico de esta es una cookie de autenticación o bearer token. El servidor genera un "Soy Groot y tener permisos de xyz" del token y lo entrega al cliente. En el futuro, el cliente presentará ese token al servidor, pero el servidor necesita algún tipo de garantía de que el cliente no ha falsificado el token. Por lo tanto, el primer requisito: autenticidad (conocido como) integridad, altera y).

Puesto que el estado persistente es de confianza del servidor, se prevé que este estado podría contener información específica para el entorno operativo. Esto podría ser en forma de una ruta de acceso de archivo, un permiso, un identificador u otra referencia indirecta o algún otro elemento de datos específicos del servidor. No generalmente deba ser divulgado dicha información a un cliente de confianza. Por lo tanto, el segundo requisito: confidencialidad.

Por último, dado que las aplicaciones modernas están divididas en componentes que hemos visto es que los componentes individuales desea aprovechar las ventajas de este sistema sin tener en cuenta otros componentes del sistema. Por ejemplo, si un componente de token de portador se usa esta pila, debe funcionar sin interferencias de un mecanismo de anti-CSRF que también podría estar usando la misma pila. Por lo tanto, el último requisito: aislamiento.

Podemos proporcionar aún más las restricciones con el fin de restringir el ámbito de nuestros requisitos. Se supone que todos los servicios que funcionan en los sistemas de cifrado se confía por igual y que los datos no necesitan generar o consuman fuera de los servicios de nuestro control directo. Además, se requieren que las operaciones sean tan rápidas como sea posible, ya que cada solicitud al servicio web podría pasar por el sistema de cifrado una o varias veces. Esto hace que la criptografía simétrica sea ideal para nuestro escenario y nos podemos descuento criptografía asimétrica hasta que tal vez que lo necesite.

## <a name="design-philosophy"></a>Filosofía de diseño

Comenzamos mediante la identificación de problemas con la pila existente. Una vez que tenemos, hemos encuestadas el panorama de las soluciones existentes y concluye que no hay ninguna solución existente tenía las capacidades que se busca. A continuación, diseñamos una solución basada en varios principios fundamentales.

* El sistema debe ofrecer sencillez de configuración. Lo ideal es que el sistema sería sin configuración y los desarrolladores podrían ponerlo en marcha. En situaciones donde los desarrolladores deben configurar un aspecto específico (por ejemplo, el repositorio de claves), prestarse a hacer que esas configuraciones específicas simple.

* Ofrecen una sencilla API orientadas al consumidor. La API deben ser fácil de usar correctamente y difícil de usar incorrectamente.

* Los desarrolladores no deben aprender los principios de administración de claves. El sistema debe controlar la selección del algoritmo y la vigencia de la clave en nombre del desarrollador. Lo ideal es que el desarrollador ni tan siquiera debe tener acceso al material de clave sin procesar.

* Las claves deben estar protegidas en reposo cuando sea posible. El sistema debe descubrir un mecanismo de protección predeterminado adecuado y lo aplica automáticamente.

Con estos principios en mente, hemos desarrollado una sencilla [fácil de usar](xref:security/data-protection/using-data-protection) pila de protección de datos.

Las API de protección de datos de ASP.NET Core no están pensados principalmente para la persistencia indefinido de cargas confidenciales. Otras tecnologías como [DPAPI de Windows CNG](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) y [Azure Rights Management](https://docs.microsoft.com/rights-management/) son más adecuados para el escenario de almacenamiento ilimitada, y tienen capacidades de administración de claves seguro según corresponda. Es decir, no hay nada que prohíben un desarrollador del uso de las API de protección de datos de ASP.NET Core para la protección a largo plazo de los datos confidenciales.

## <a name="audience"></a>Audiencia

El sistema de protección de datos se divide en cinco paquetes principales. Destino de diversos aspectos de estas API tres tipos de destinatarios principales;

1. El [información general sobre la API de consumidor](xref:security/data-protection/consumer-apis/overview) a los desarrolladores de aplicaciones y el marco de destino.

   "No quiero obtener información acerca de cómo funciona la pila o acerca de cómo esté configurada. Simplemente quiero realizar una manera alguna operación tan simple como sea posible con gran probabilidad de que usar la API correctamente."

2. El [API de configuración](xref:security/data-protection/configuration/overview) los desarrolladores de aplicaciones y los administradores del sistema de destino.

   "Necesito indicar al sistema de protección de datos que mi entorno requiere configuración o las rutas de acceso no predeterminada".

3. Desarrolladores de destino de las API de extensibilidad responsable de implementar la directiva personalizada. Había limitado a situaciones excepcionales y ha experimentado, los desarrolladores de seguridad compatible con el uso de estas API.

   "Necesito reemplazar un componente completo dentro del sistema, ya que tengo requisitos de comportamiento realmente únicos. Estoy dispuesto a aprender con escasa frecuencia utiliza partes de la superficie de API con el fin de crear un complemento que cumple Mis requisitos."

## <a name="package-layout"></a>Diseño del paquete

La pila de protección de datos consta de cinco paquetes.

* [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) contiene el <xref:Microsoft.AspNetCore.DataProtection.IDataProtectionProvider> y <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> interfaces para crear servicios de protección de datos. También contiene métodos de extensión útiles para trabajar con estos tipos (por ejemplo, [IDataProtector.Protect](xref:Microsoft.AspNetCore.DataProtection.DataProtectionCommonExtensions.Protect*)). Si el sistema de protección de datos se crea una instancia en otro lugar y consumen la API, referencia `Microsoft.AspNetCore.DataProtection.Abstractions`.

* [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) contiene la implementación del núcleo del sistema de protección de datos, incluidas las operaciones criptográficas de core, administración de claves, configuración y extensibilidad. Para crear una instancia del sistema de protección de datos (por ejemplo, éste se agrega a un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>) o hacer referencia a modificar o extender su comportamiento, `Microsoft.AspNetCore.DataProtection`.

* [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene API adicionales que los desarrolladores pueden resultar útiles pero que no pertenecen en el paquete principal. Por ejemplo, este paquete contiene métodos de generador para crear una instancia del sistema de protección de datos para almacenar las claves en una ubicación en el sistema de archivos sin la inserción de dependencias (consulte <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>). También contiene métodos de extensión para limitar la duración de cargas protegidas (consulte <xref:Microsoft.AspNetCore.DataProtection.ITimeLimitedDataProtector>).

* [Microsoft.AspNetCore.DataProtection.SystemWeb](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.SystemWeb/) puede instalarse en una aplicación existente de 4.x ASP.NET para redirigir su `<machineKey>` operaciones para usar la nueva pila de protección de datos de ASP.NET Core. Para obtener más información, consulta <xref:security/data-protection/compatibility/replacing-machinekey>.

* [Microsoft.AspNetCore.Cryptography.KeyDerivation](https://www.nuget.org/packages/Microsoft.AspNetCore.Cryptography.KeyDerivation/) proporciona una implementación de la contraseña PBKDF2 hash rutina y se puede usar los sistemas que deben administrar de forma segura las contraseñas de usuario. Para obtener más información, consulta <xref:security/data-protection/consumer-apis/password-hashing>.

## <a name="additional-resources"></a>Recursos adicionales

<xref:host-and-deploy/web-farm>
