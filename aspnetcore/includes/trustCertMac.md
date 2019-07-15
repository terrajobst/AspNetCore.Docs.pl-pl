* <span data-ttu-id="b9f18-101">Zaufanie certyfikatu deweloperskiego HTTPS, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b9f18-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

* <span data-ttu-id="b9f18-102">Poprzednie polecenie wyświetla następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="b9f18-102">The preceding command displays the following output:</span></span>

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* <span data-ttu-id="b9f18-103">Jeśli zostanie wyświetlony monit, wprowadź nazwę i hasło administratora.</span><span class="sxs-lookup"><span data-stu-id="b9f18-103">Enter the admin username and password if prompted.</span></span>  <span data-ttu-id="b9f18-104">Certyfikat zostanie teraz zainstalowany i zaufany.</span><span class="sxs-lookup"><span data-stu-id="b9f18-104">The certificate will now be installed and trusted.</span></span>

    <span data-ttu-id="b9f18-105">Zobacz [ufać certyfikatowi rozwoju platformy ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b9f18-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>