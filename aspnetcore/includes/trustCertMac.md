* <span data-ttu-id="8194d-101">Para confiar en el certificado de desarrollo de HTTPS, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="8194d-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

* <span data-ttu-id="8194d-102">El comando anterior muestra la siguiente salida:</span><span class="sxs-lookup"><span data-stu-id="8194d-102">The preceding command displays the following output:</span></span>

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* <span data-ttu-id="8194d-103">Si se le solicita, escriba el nombre de usuario administrador y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="8194d-103">Enter the admin username and password if prompted.</span></span>  <span data-ttu-id="8194d-104">El certificado se instalará ahora y será de confianza.</span><span class="sxs-lookup"><span data-stu-id="8194d-104">The certificate will now be installed and trusted.</span></span>

    <span data-ttu-id="8194d-105">Para obtener más información, vea [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="8194d-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>