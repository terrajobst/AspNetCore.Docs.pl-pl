---
title: Wprowadzenie do platformy ASP.NET Core 1.1
author: rick-anderson
description: Postępuj zgodnie z tego samouczka szybkiego do tworzenia i uruchamiania prostej aplikacji Hello World przy użyciu platformy ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a>Wprowadzenie do platformy ASP.NET Core 1.1

> [!NOTE]
> Te instrukcje dotyczą ASP.NET Core 1.1. Szukasz do najnowszej wersji? Zobacz [bieżącej wersji tego samouczka](xref:getting-started).

1. Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).

2. Utwórz folder dla nowego projektu platformy .NET Core.

   W systemach macOS i Linux otwórz okno terminala. W systemie Windows otwórz wiersz polecenia.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* pliku, aby wybrać 1.0.4 zestawu SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Utwórz nowy projekt platformy .NET Core.

   ```terminal
   dotnet new web
   ```
   
3.  Przywracanie pakietów.

    ```terminal
    dotnet restore
    ```

4. Uruchom aplikację.

   [Dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.

   ```terminal
   dotnet run
   ```

5. Przejdź do `http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Następne kroki

Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)

Aby zapoznać się z wprowadzeniem do podstawowych pojęć i architektury platformy ASP.NET, zobacz [Wprowadzenie do platformy ASP.NET Core](index.md) i [Podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).

Aplikacja ASP.NET Core może używać biblioteki klas bazowych i środowiska uruchomieniowego .NET Core lub .NET Framework. Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
