---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Krótki samouczek, który tworzy i uruchamia podstawową aplikację Hello world przy użyciu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: 4f7e67e1e422afe3f7e2970e0c40380f065390ac
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928315"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Samouczek: wprowadzenie do ASP.NET Core

W tym samouczku pokazano, jak za pomocą interfejs wiersza polecenia platformy .NET Core utworzyć i uruchomić aplikację internetową ASP.NET Core.

Dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz projekt aplikacji sieci Web.
> * Zaufaj certyfikatowi Deweloperskiemu.
> * Uruchom aplikację.
> * Edytuj stronę Razor.

Na końcu będziesz mieć działającą aplikację sieci Web na komputerze lokalnym.

![Strona główna aplikacji sieci Web](_static/home-page.png)

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a>Tworzenie projektu aplikacji sieci Web

Otwórz powłokę poleceń i wprowadź następujące polecenie:

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

Poprzednie polecenie:

* Tworzy nową aplikację sieci Web.  
* Parametr `-o aspnetcoreapp` tworzy katalog o nazwie *aspnetcoreapp* z plikami źródłowymi aplikacji.

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

*Zażądano zaufania certyfikatu deweloperskiego https. Jeśli certyfikat nie jest już zaufany, zostanie uruchomione następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

To polecenie może wyświetlać monit o podanie hasła w celu zainstalowania certyfikatu w łańcuchu kluczy systemu. Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.

---

Aby uzyskać więcej informacji, zobacz artykuł [ufanie certyfikatowi Deweloperskiemu protokołu HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Uruchamianie aplikacji

Uruchom następujące polecenia:

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

Po powłoce poleceń wskazuje, że aplikacja została uruchomiona, przejdź do [https://localhost:5001](https://localhost:5001).

## <a name="edit-a-razor-page"></a>Edytowanie strony Razor

Otwórz stronę *Pages/index. cshtml* i zmodyfikuj ją i Zapisz przy użyciu następującego wyróżnionego znacznika:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Przejdź do [https://localhost:5001](https://localhost:5001), Odśwież stronę i sprawdź, czy są wyświetlane zmiany.

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
