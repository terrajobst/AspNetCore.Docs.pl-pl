---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Krótki samouczek, który tworzy i uruchamia podstawową aplikację Hello world przy użyciu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: 0f05ab120d64832a4bc2fd70921efc7238ee9eac
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187061"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Samouczek: Wprowadzenie do platformy ASP.NET Core

W tym samouczku pokazano, jak utworzyć i uruchomić ASP.NET Core aplikację sieci Web przy użyciu interfejsu wiersza polecenia platformy .NET Core.

Dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz projekt aplikacji sieci Web.
> * Zaufaj certyfikatowi Deweloperskiemu.
> * Uruchom aplikację.
> * Edytuj stronę Razor.

Na końcu będziesz mieć działającą aplikację sieci Web na komputerze lokalnym.

![Strona główna aplikacji sieci Web](_static/home-page.png)

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a>Tworzenie projektu aplikacji sieci Web

Otwórz powłokę poleceń i wprowadź następujące polecenie:

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

Poprzednie polecenie:

* Tworzy nową aplikację sieci Web.  
* Parametr tworzy katalog o nazwie aspnetcoreapp z plikami źródłowymi aplikacji. `-o`

### <a name="trust-the-development-certificate"></a>Ufanie certyfikatowi Deweloperskiemu

Ufaj certyfikatowi deweloperskim HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

Poprzednie polecenie wyświetla następujące okno dialogowe:

![Okno dialogowe ostrzeżenia o zabezpieczeniach](~/getting-started/_static/cert.png)

Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

Poprzednie polecenie wyświetla następujący komunikat:

*Zażądano zaufania certyfikatu deweloperskiego HTTPS. Jeśli certyfikat nie jest już zaufany, zostanie uruchomione następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

To polecenie może wyświetlać monit o podanie hasła w celu zainstalowania certyfikatu w łańcuchu kluczy systemu. Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

W przypadku podsystemu Windows dla systemu Linux Zobacz temat [zaufanie certyfikatu HTTPS z podsystemu Windows dla systemu Linux](xref:security/enforcing-ssl#wsl).

Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.

---

Aby uzyskać więcej informacji, zobacz artykuł [ufanie certyfikatowi Deweloperskiemu protokołu HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Uruchamianie aplikacji

Uruchom następujące polecenia:

```dotnetcli
cd aspnetcoreapp
dotnet run
```

Po powłoce poleceń wskazuje, że aplikacja została uruchomiona, przejdź do [https://localhost:5001](https://localhost:5001). Kliknij przycisk **Akceptuj**, aby zaakceptować zasady ochrony prywatności i plików cookie. Ta aplikacja nie zachowuje informacji osobistych.

## <a name="edit-a-razor-page"></a>Edytowanie strony Razor

Otwórz stronę *Pages/index. cshtml* i zmodyfikuj ją, korzystając z następującego wyróżnionego znacznika:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Przejdź do [https://localhost:5001](https://localhost:5001)okna i sprawdź, czy zmiany są wyświetlane.

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz projekt aplikacji sieci Web.
> * Zaufaj certyfikatowi Deweloperskiemu.
> * Uruchom projekt.
> * Wprowadź zmianę.

Aby dowiedzieć się więcej na temat ASP.NET Core, zapoznaj się ze zalecaną ścieżką szkoleniową we wprowadzeniu:

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
