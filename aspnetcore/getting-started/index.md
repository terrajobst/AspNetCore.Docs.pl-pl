---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4c899ff3c087f1b569521c6e2e8458fca01d6061
ms.sourcegitcommit: bdfba5e7575b2a786ef27c0edf688c7dbd09ee95
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52288645"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Samouczek: Rozpoczynanie pracy z platformą ASP.NET Core

W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu interfejsu wiersza polecenia platformy .NET Core. Dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenie projektu aplikacji sieci web.
> * Włączanie obsługi protokołu HTTPS w lokalnym.
> * Uruchom aplikację.
> * Edytuj stronę Razor.

Po zakończeniu będziesz mieć działającą aplikację sieci web uruchomiony na komputerze lokalnym.

![Strona główna aplikacji sieci Web](_static/home-page.png)


## <a name="prerequisites"></a>Wymagania wstępne

* Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].

## <a name="create-a-web-app-project"></a>Tworzenie projektu aplikacji sieci web

* Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a>Włączanie obsługi protokołu HTTPS w lokalnym

* Zaufanie certyfikatu deweloperskiego HTTPS:

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

* Uruchom następujące polecenia:

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* Przejdź do [https://localhost:5001](https://localhost:5001). Kliknij przycisk **Akceptuj**, aby zaakceptować zasady ochrony prywatności i plików cookie. Ta aplikacja nie zachowuje informacji osobistych.

## <a name="edit-a-razor-page"></a>Edytuj stronę Razor

* Otwórz *Pages/About.cshtml* i zmodyfikuj stronę w wyróżnionym miejscu w następującym przykładzie:

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* Przejdź do [https://localhost:5001/About](https://localhost:5001/About) i sprawdź, czy zmiany są wyświetlane.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób:

> [!div class="checklist"]
> * Tworzenie projektu aplikacji sieci web.
> * Włączanie obsługi protokołu HTTPS w lokalnym.
> * Uruchom projekt.
> * Wprowadź zmiany.

Aby dowiedzieć się więcej na temat platformy ASP.NET Core, zobacz wprowadzenie:

> [!div class="nextstepaction"]
> <xref:index>



> [!NOTE]
> Testujemy użyteczność proponowaną nową strukturę dla platformy ASP.NET Core spis treści.  Jeśli masz kilka minut, spróbuj wykonywania, znajdowanie 7 różne tematy w bieżącej lub proponowanej spis treści, [kliknij tutaj, aby wziąć udział w badaniach](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).