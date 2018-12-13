---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: getting-started
ms.openlocfilehash: dc85fe9189c93476859a6c00d60ec24d6eeec3fa
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335315"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Samouczek: Wprowadzenie do platformy ASP.NET Core

W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu interfejsu wiersza polecenia platformy .NET Core.

Dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenie projektu aplikacji sieci web.
> * Włączanie obsługi protokołu HTTPS w lokalnym.
> * Uruchom aplikację.
> * Edytuj stronę Razor.

Po zakończeniu będziesz mieć działającą aplikację sieci web uruchomiony na komputerze lokalnym.

![Strona główna aplikacji sieci Web](_static/home-page.png)

## <a name="prerequisites"></a>Wymagania wstępne

* [.NET core SDK 2,2](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>Tworzenie projektu aplikacji sieci web

Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a>Włączanie obsługi protokołu HTTPS w lokalnym

Zaufanie certyfikatu deweloperskiego HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

Poprzednie polecenie wyświetla następujące okno dialogowe:

![Okno dialogowe ostrzeżenia o zabezpieczeniach](_static/cert.png)

Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

Poprzednie polecenie wyświetli następujący komunikat:

*Zażądano ufające certyfikatu deweloperskiego protokołu HTTPS. Jeśli certyfikat nie jest już zaufany możemy uruchom następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.  
* To polecenie może monitować o hasło, aby zainstalować certyfikat w pęku kluczy systemu.

Hasło: *

Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.

---

## <a name="run-the-app"></a>Uruchamianie aplikacji

Uruchom następujące polecenia:

```console
cd aspnetcoreapp
dotnet run
```

Przejdź do [https://localhost:5001](https://localhost:5001). Kliknij przycisk **Akceptuj**, aby zaakceptować zasady ochrony prywatności i plików cookie. Ta aplikacja nie zachowuje informacji osobistych.

## <a name="edit-a-razor-page"></a>Edytuj stronę Razor

Otwórz *Pages/Index.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Przejdź do [ https://localhost:5001 ](https://localhost:5001)i Sprawdź zmiany są wyświetlane.

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie projektu aplikacji sieci web.
> * Włączanie obsługi protokołu HTTPS w lokalnym.
> * Uruchom projekt.
> * Wprowadź zmiany.

Aby dowiedzieć się więcej na temat platformy ASP.NET Core, zobacz wprowadzenie:

> [!div class="nextstepaction"]
> <xref:index>
