---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Krótki samouczek, który tworzy i uruchamia podstawową aplikację Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 5/15/2019
uid: getting-started
ms.openlocfilehash: 9227dcfbc84376d9d73bc6fc0dd76085779acae1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610307"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Samouczek: Wprowadzenie do platformy ASP.NET Core

W tym samouczku przedstawiono sposób tworzenia i uruchamiania aplikacji sieci web platformy ASP.NET Core przy użyciu interfejsu wiersza polecenia platformy .NET Core.

Dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenie projektu aplikacji sieci web.
> * Zaufanie certyfikatu deweloperskiego.
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

### <a name="trust-the-development-certificate"></a>Zaufanie certyfikatu deweloperskiego

Zaufanie certyfikatu deweloperskiego HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

Poprzednie polecenie wyświetla następujące okno dialogowe:

![Okno dialogowe ostrzeżenia o zabezpieczeniach](~/getting-started/_static/cert.png)

Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

Poprzednie polecenie wyświetli następujący komunikat:

*Zażądano ufające certyfikatu deweloperskiego protokołu HTTPS. Jeśli certyfikat nie jest zaufany, firma Microsoft będzie uruchom następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

To polecenie może monitować o hasło, aby zainstalować certyfikat w pęku kluczy systemu. Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Dla podsystemu Windows dla systemu Linux, zobacz [z podsystemu Windows dla systemu Linux przy użyciu certyfikatu HTTPS zaufania](xref:security/enforcing-ssl#wsl).

Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.

---

Aby uzyskać więcej informacji, zobacz [ufać certyfikatowi rozwoju platformy ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Uruchamianie aplikacji

Uruchom następujące polecenia:

```console
cd aspnetcoreapp
dotnet run
```

Po powłoki poleceń wskazuje, że aplikacja została uruchomiona, przejdź do [ https://localhost:5001 ](https://localhost:5001). Kliknij przycisk **Akceptuj**, aby zaakceptować zasady ochrony prywatności i plików cookie. Ta aplikacja nie zachowuje informacji osobistych.

## <a name="edit-a-razor-page"></a>Edytuj stronę Razor

Otwórz *Pages/Index.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Przejdź do [ https://localhost:5001 ](https://localhost:5001)i Sprawdź zmiany są wyświetlane.

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie projektu aplikacji sieci web.
> * Zaufanie certyfikatu deweloperskiego.
> * Uruchom projekt.
> * Wprowadź zmiany.

Aby dowiedzieć się więcej na temat platformy ASP.NET Core, zobacz ze ścieżką szkoleniową zalecany we wstępie:

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
