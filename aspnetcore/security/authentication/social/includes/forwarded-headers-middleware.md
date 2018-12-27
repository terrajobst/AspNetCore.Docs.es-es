## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>Solicitar información con un proxy hacia delante o equilibrador de carga

Si la aplicación se implementa detrás de un servidor proxy o un equilibrador de carga, parte de la información de la solicitud original se puede reenviar a la aplicación en los encabezados de solicitud. Normalmente, esta información incluye el esquema de solicitud seguro (`https`), host y dirección IP del cliente. Las aplicaciones no leen automáticamente estos encabezados de solicitud para detectar y usar la información de la solicitud original.

Se usa el esquema de generación de vínculos que afecta el flujo de autenticación con proveedores externos. Perder el esquema seguro (`https`) da como resultado la aplicación de generación de direcciones URL de redirección no seguro incorrecta.

Usar software intermedio de encabezados reenviados a disposición la información de la solicitud original a la aplicación para procesar las solicitudes.

Para obtener más información, consulta <xref:host-and-deploy/proxy-load-balancer>.
