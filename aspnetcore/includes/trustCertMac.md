---
ms.openlocfilehash: 2ec079606cb48670dbc3852482fd8d401e7db44b
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472297"
---
* Zaufanie certyfikatu deweloperskiego HTTPS, uruchamiając następujące polecenie:

    ```console
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

* Jeśli zostanie wyświetlony monit, wprowadź nazwę i hasło administratora.  Certyfikat zostanie teraz zainstalowany i zaufany.

    Zobacz [ufać certyfikatowi rozwoju platformy ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) Aby uzyskać więcej informacji.