* Para confiar en el certificado de desarrollo de HTTPS, ejecute el comando siguiente:

    ```console
    dotnet dev-certs https --trust
    ```

* El comando anterior muestra la siguiente salida:

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* Si se le solicita, escriba el nombre de usuario administrador y la contraseña.  El certificado se instalará ahora y será de confianza.

    Para obtener más información, vea [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).