---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

1. Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Utwórz nowy projekt platformy .NET Core.

   W systemach macOS i Linux otwórz okno terminala. W systemie Windows otwórz wiersz polecenia. Wprowadź następujące polecenie:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Uruchom aplikację za pomocą następujących poleceń:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Przejdź do [ http://localhost:5000 ](http://localhost:5000).

5. Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world! Czas na serwerze jest @DateTime.Now":

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) i zweryfikować zmiany.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).

2. Utwórz folder dla nowego projektu platformy .NET Core.

   W systemach macOS i Linux otwórz okno terminala. W systemie Windows otwórz wiersz polecenia.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* pliku, aby wybrać 1.0.4 zestawu SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Utwórz nowy projekt platformy .NET Core.

   ```terminal
   dotnet new web
   ```

5. Przywracanie pakietów.

    ```terminal
    dotnet restore
    ```

6. Uruchom aplikację.

   ```terminal
   dotnet run
   ```

   [Dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.

7. Przejdź do `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end