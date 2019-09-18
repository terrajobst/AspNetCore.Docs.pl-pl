* Aby ufać certyfikatowi programistycznemu HTTPS, należy uruchomić następujące polecenie:

    ```dotnetcli
    dotnet dev-certs https --trust
    ```

* Poprzednie polecenie wyświetla następujące dane wyjściowe:

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* Jeśli zostanie wyświetlony monit, wprowadź nazwę użytkownika i hasło administratora.  Certyfikat zostanie teraz zainstalowany i zaufany.

    Aby uzyskać więcej informacji, zobacz artykuł [Ufaj certyfikatowi deweloperskim protokołu HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) .