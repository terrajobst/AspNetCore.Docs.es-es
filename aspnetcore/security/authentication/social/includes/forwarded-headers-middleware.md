## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>Reenvío de información de solicitud con un servidor proxy o un equilibrador de carga

Si la aplicación se implementa detrás de un servidor proxy o de un equilibrador de carga, parte de la información de solicitud original podría reenviarse a la aplicación en los encabezados de solicitud. Normalmente, esta información incluye el esquema de solicitud seguro (`https`), el host y la dirección IP del cliente. Las aplicaciones no leen automáticamente estos encabezados de solicitud para detectar y usar la información de solicitud original.

El esquema se usa en la generación de vínculos que afecta al flujo de autenticación con proveedores externos. El resultado de perder el esquema seguro (`https`) es que la aplicación genera direcciones URL incorrectas poco seguras.

Use middleware de encabezados reenviados para que la información de solicitud original esté disponible para la aplicación para procesar las solicitudes.

Para más información, consulte <xref:host-and-deploy/proxy-load-balancer>.
