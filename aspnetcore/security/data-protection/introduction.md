---
title: Protección de datos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre el concepto de protección de datos y los principios de diseño de las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/introduction
ms.openlocfilehash: c043de3fc472357c0722a5a736d7e811f1b77002
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273915"
---
# <a name="aspnet-core-data-protection"></a>Protección de datos de ASP.NET Core

Las aplicaciones Web a menudo se necesitan almacenar datos confidenciales de seguridad. Windows proporciona DPAPI para aplicaciones de escritorio, pero esto no es apropiada para las aplicaciones web. La pila de protección de datos de ASP.NET Core proporcionan una API cifrada sencilla y fácil de usar un programador puede utilizar para proteger los datos, incluida la rotación y la administración de claves.

La pila de protección de datos de ASP.NET Core está diseñada para que actúe como la sustituirá a largo plazo para la &lt;machineKey&gt; elemento en ASP.NET 1.x - 4.x. Se diseñó para solucionar muchos de los defectos de la pila cifrado anterior al proporcionar una solución para la mayoría de los casos de uso de aplicaciones modernas están probables que encuentre.

## <a name="problem-statement"></a>Declaración del problema

En pocas palabras la declaración del problema general en una sola frase: se necesita para conservar información de confianza para su recuperación posterior, pero no confía en el mecanismo de persistencia. En términos de web, esto puede escribirse como "Necesito al estado de confianza de ida y vuelta a través de un cliente no es de confianza".

El ejemplo canónico de esto es una cookie de autenticación o portador símbolo (token). El servidor genera un "Estoy Groot y tiene permisos de xyz" símbolo (token) y los entrega al cliente. En una fecha futura, el cliente presentará ese token en el servidor, pero el servidor necesita algún tipo de garantía de que el cliente no ha falsificado el token. Por lo tanto, el primer requisito: autenticidad (conocido como) integridad, alteración de corrección).

Puesto que el estado persistente es de confianza para el servidor, prevemos que este estado podría contener información específica para el entorno operativo. Podría tratarse con el formato de una ruta de acceso de archivo, un permiso, un identificador u otra referencia indirecta o alguna otra parte de los datos específicos del servidor. Esta información normalmente no debe divulgarse a un cliente no es de confianza. Por lo tanto, el segundo requisito: confidencialidad.

Por último, puesto que están dividida en componentes de aplicaciones modernas, lo que hemos visto es que los componentes individuales desea aprovechar las ventajas de este sistema sin tener en cuenta otros componentes en el sistema. Por ejemplo, si un componente de token de portador está usando esta pila, debe funcionar sin la interferencia de un mecanismo de anti-CSRF que también puedan estar usando la misma pila. Por lo tanto, el requisito final: aislamiento.

Podemos proporcionar más restricciones con el fin de restringir el ámbito de los requisitos. Se supone que todos los servicios que funcionan en el sistema de cifrado se confía por igual y que no necesitan los datos que se genera o consume fuera de los servicios en nuestro control directo. Además, es necesario que las operaciones son lo más rápidas posible, ya que cada solicitud al servicio web puede pasar por el sistema de cifrado una o varias veces. Esto hace que la criptografía simétrica ideal para nuestro escenario y se podemos descuentos criptografía asimétrica hasta como uno que sea necesaria.

## <a name="design-philosophy"></a>Filosofía de diseño

Hemos iniciado mediante la identificación de problemas con la pila del existente. Una vez que tuvimos, hemos evaluar el panorama de las soluciones existentes y concluye que no hay ninguna solución existente bastante tuvieron las capacidades que se busca. A continuación, diseñamos una solución basada en varias orientaciones.

* El sistema debe ofrecer la sencillez de configuración. Lo ideal es que el sistema sería sin configuración y los desarrolladores podrían llegar a cero que se ejecuta. En situaciones donde los desarrolladores deben configurar un aspecto concreto (por ejemplo, el repositorio de claves), se debe prestar atención especial a hacer que esas configuraciones específicas simple.

* Ofrecen una API sencilla de consumo. Las API deben ser fáciles de usar correctamente y difíciles de usar de forma incorrecta.

* Los desarrolladores no deben aprender los principios de administración de claves. El sistema debe controlar la selección de algoritmo y la vigencia de la clave en nombre del desarrollador. Lo ideal es que el programador ni siquiera debe tener acceso a material de la clave sin formato.

* Las claves deben estar protegidas en reposo cuando sea posible. El sistema debe determinar un mecanismo de protección predeterminado adecuado y lo aplicará automáticamente.

Con estos principios en cuenta que desarrollamos una sencilla [fácil de usar](xref:security/data-protection/using-data-protection) pila de protección de datos.

La API de protección de datos de ASP.NET Core no están pensados principalmente para la persistencia indefinido de cargas confidenciales. Otras tecnologías, como [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) y [Azure Rights Management](https://docs.microsoft.com/rights-management/) son más adecuados para el escenario de almacenamiento indefinido, y tienen capacidades de administración de claves seguro según corresponda. Es decir, no hay nada prohibir a un desarrollador usa las API de protección de datos de ASP.NET Core para la protección a largo plazo de información confidencial.

## <a name="audience"></a>Audiencia

El sistema de protección de datos se divide en cinco paquetes principales. Destino de distintos aspectos de estas API tres audiencias principales;

1. El [información general de las API de consumidor](xref:security/data-protection/consumer-apis/overview) a los desarrolladores de aplicaciones y un marco de destino.

   "No desea obtener información acerca de cómo funciona la pila o acerca de cómo esté configurada. Simplemente desea realizar alguna operación como simple forma como sea posible con una probabilidad alta de usar las API correctamente."

2. El [API de configuración](xref:security/data-protection/configuration/overview) dirigidas a los desarrolladores de aplicaciones y los administradores del sistema.

   "Necesito indicar que el sistema de protección de datos que mi entorno requiere configuración o las rutas de acceso no predeterminada".

3. Desarrolladores de destino de las API de extensibilidad responsable de implementar la directiva personalizada. Uso de estas API se usaría limitarse a situaciones excepcionales y experimentado, los desarrolladores de cuenta de seguridad.

   "Necesito reemplazar un componente completo dentro del sistema porque tienen requisitos de comportamiento verdaderamente únicos. Estoy dispuesto a aprender uncommonly utiliza partes de la superficie de API para compilar un complemento que cumple los requisitos de mi."

## <a name="package-layout"></a>Diseño del paquete

La pila de protección de datos consta de cinco paquetes.

* Microsoft.AspNetCore.DataProtection.Abstractions contiene las interfaces básicas de IDataProtectionProvider y IDataProtector. También contiene métodos de extensión útil que pueden ayudar a trabajar con estos tipos (p. ej., las sobrecargas de IDataProtector.Protect). Vea la sección de interfaces de consumidor para obtener más información. Si otra persona es responsable de crear una instancia del sistema de protección de datos y simplemente están utilizando las API, deseará referencia Microsoft.AspNetCore.DataProtection.Abstractions.

* Microsoft.AspNetCore.DataProtection contiene la implementación básica del sistema de protección de datos, incluida la administración de claves, configuración, extensibilidad y las operaciones criptográficas de núcleo. Si eres responsable de crear una instancia del sistema de protección de datos (p. ej., éste se agrega a un IServiceCollection) o modificar o extender su comportamiento, deseará referencia Microsoft.AspNetCore.DataProtection.

* Microsoft.AspNetCore.DataProtection.Extensions contiene algunas API adicionales que los desarrolladores que resulte útiles, pero no en el paquete de núcleo que pertenecen. Por ejemplo, este paquete contiene una API sencilla "crear una instancia del sistema que señala a un directorio de almacenamiento de claves específico con ninguna instalación de inyección de dependencia" (más información). También contiene métodos de extensión para limitar la duración de cargas protegidos (más información).

* Microsoft.AspNetCore.DataProtection.SystemWeb puede instalarse en una aplicación existente de 4.x ASP.NET para redirigir su &lt;machineKey&gt; operaciones usar en su lugar la nueva pila de protección de datos. Vea [compatibilidad](xref:security/data-protection/compatibility/replacing-machinekey#compatibility-replacing-machinekey) para obtener más información.

* Microsoft.AspNetCore.Cryptography.KeyDerivation proporciona una implementación de la contraseña PBKDF2 hash rutina y puede usarse en sistemas que necesitan para administrar las contraseñas de usuario de forma segura. Vea [Hash a contraseñas](xref:security/data-protection/consumer-apis/password-hashing) para obtener más información.
