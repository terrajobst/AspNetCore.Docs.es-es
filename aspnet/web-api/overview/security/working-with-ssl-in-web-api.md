---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Trabajar con SSL en API Web | Microsoft Docs
author: MikeWasson
description: Muestra cómo usar SSL con ASP.NET Web API, incluido el uso de certificados de cliente SSL.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: b11b35f58a1f033423f5e6ea5f5373df0d1fcb5f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837194"
---
<a name="working-with-ssl-in-web-api"></a>Trabajar con SSL en Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Varios esquemas de autenticación habituales no son seguras a través de HTTP sin formato. En concreto, la autenticación básica y autenticación mediante formularios envían credenciales no cifradas. Para ser seguro, estos esquemas de autenticación *debe* utilizar SSL. Además, se pueden usar certificados de cliente SSL para autenticar a los clientes.

## <a name="enabling-ssl-on-the-server"></a>Habilitación de SSL en el servidor

Para configurar SSL en IIS 7 o posterior:

- Crear u obtener un certificado. Para las pruebas, puede crear un certificado autofirmado.
- Agregar un enlace HTTPS.

Para obtener más información, consulte [cómo configurar SSL en IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Para realizar pruebas locales, puede habilitar SSL en IIS Express de Visual Studio. En la ventana Propiedades, establezca **SSL habilitado** a **True**. Tenga en cuenta el valor de **dirección URL de SSL**; use esta dirección URL para probar las conexiones HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Exigir SSL en un controlador de API Web

Si tiene un enlace HTTP y HTTPS, los clientes aún pueden usar HTTP para acceder al sitio. Puede permitir que algunos recursos estén disponibles a través de HTTP, mientras que otros recursos requieren SSL. En ese caso, utilice un filtro de acción para requerir SSL para los recursos protegidos. El código siguiente muestra un filtro de autenticación de API Web que busca SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Agregue este filtro para las acciones de API Web que requieran SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificados de cliente SSL

SSL proporciona autenticación mediante certificados de infraestructura de clave pública. El servidor debe proporcionar un certificado para autenticar el servidor al cliente. Es menos común para el cliente proporcionar un certificado al servidor, pero se trata de una opción para autenticar los clientes. Para usar certificados de cliente con SSL, necesita una forma de distribuir los certificados autofirmados a los usuarios. Para muchos tipos de aplicaciones, esto no será una buena experiencia del usuario, pero en algunos entornos (por ejemplo, enterprise) puede ser factible.

| Ventajas | Desventajas |
| --- | --- |
| -Credenciales de certificado son más seguras que el nombre de usuario/contraseña. -SSL proporciona un canal seguro completando, con autenticación, integridad del mensaje y el cifrado de mensajes. | -Debe obtener y administrar certificados PKI. -La plataforma de cliente debe admitir los certificados de cliente SSL. |

Para configurar IIS para que acepte certificados de cliente, abra el Administrador de IIS y realice los pasos siguientes:

1. Haga clic en el nodo de sitio en la vista de árbol.
2. Haga doble clic en el **configuración SSL** característica en el panel central.
3. En **certificados de cliente**, seleccione una de estas opciones: 

    - **Aceptar**: IIS aceptará un certificado desde el cliente, pero no requiere una.
    - **Requerir**: requieren un certificado de cliente. (Para habilitar esta opción, también debe seleccionar "Requerir SSL")

También puede establecer estas opciones en el archivo ApplicationHost.config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

El **SslNegotiateCert** marca significa IIS aceptará un certificado desde el cliente, pero no requiere una (equivalente a la opción "Aceptar" en el Administrador de IIS). Para solicitar un certificado, establezca el **SslRequireCert** marca. Para las pruebas, también puede establecer estas opciones en IIS Express, en el applicationhost local. Archivo de configuración, ubicado en "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Creación de un certificado de cliente de prueba

Con fines de prueba, puede usar [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) para crear un certificado de cliente. En primer lugar, cree una entidad emisora raíz de prueba:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert le solicitará que escriba una contraseña para la clave privada.

A continuación, agregue el certificado a la prueba del servidor "de confianza raíz almacén entidades de certificación", como se indica a continuación:

1. Abra MMC.
2. En **archivo**, seleccione **Add/Remove Snap-In**.
3. Seleccione **cuenta de equipo**.
4. Seleccione **ordenador** y complete el asistente.
5. En el panel de navegación, expanda el nodo de "Entidades de certificación de raíz de confianza".
6. En el **acción** menú, elija **todas las tareas**y, a continuación, haga clic en **importación** para iniciar el Asistente para importación de certificados.
7. Busque el archivo de certificado, TempCA.cer.
8. Haga clic en **abierto**, a continuación, haga clic en **siguiente** y complete el asistente. (Se le indicará que vuelva a escribir la contraseña.)

Ahora cree un certificado de cliente que está firmado por el primer certificado:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Uso de certificados de cliente en API Web

En el servidor, puede obtener el certificado de cliente mediante una llamada a [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) en el mensaje de solicitud. El método devuelve null si no hay ningún certificado de cliente. De lo contrario, devuelve un **X509Certificate2** instancia. Utilice este objeto para obtener información del certificado, como el emisor y el asunto. A continuación, puede usar esta información para la autenticación o autorización.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
