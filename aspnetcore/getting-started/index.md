---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216216"
---
# <a name="get-started-with-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Tworzenie projektu platformy ASP.NET Core. Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [! OBEJMUJĄ [] (~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]

3. Zaufanie certyfikatu deweloperskiego HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. Uruchom aplikację:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Przejdź do [ http://localhost:5001 ](http://localhost:5001).  Kliknij przycisk **Akceptuj** zaakceptować zasady ochrony prywatności i plików cookie. Ta aplikacja nie zachowuje informacji osobistych.

6. Otwórz *Pages/About.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Przejdź do [ http://localhost:5001/About ](http://localhost:5001/About) i Sprawdź zmiany są wyświetlane.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Utwórz nowy projekt ASP.NET Core.

   Otwórz powłokę wiersza polecenia. Wprowadź następujące polecenie:

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Uruchom aplikację za pomocą następujących poleceń:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Przejdź do [ http://localhost:5000 ](http://localhost:5000).

5. Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world! Czas na serwerze jest @DateTime.Now":

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) a następnie zweryfikować zmiany.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Zainstaluj program .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).

2. Utwórz folder dla nowego projektu ASP.NET Core.

   Otwórz powłokę wiersza polecenia. Wprowadź następujące polecenia:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* plik, aby zaznaczyć 1.0.4 zestawu SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Utwórz nowy projekt ASP.NET Core.

   ```console
   dotnet new web
   ```

5. Przywróć pakiety.

    ```console
    dotnet restore
    ```

6. Uruchom aplikację.

   ```console
   dotnet run
   ```

   [Dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.

7. Przejdź do `http://localhost:5000`.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
