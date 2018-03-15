---
title: Wprowadzenie do platformy ASP.NET Core 1.1
author: rick-anderson
description: "Postępuj zgodnie z tego samouczka szybkiego do tworzenia i uruchamiania prostej aplikacji Hello World przy użyciu platformy ASP.NET Core 1.1."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: df34c93de265c3e69877c9f68dd69126bb1b850d
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-aspnet-core-11"></a>Wprowadzenie do platformy ASP.NET Core 1.1

> [!NOTE]
> Te instrukcje dotyczą ASP.NET Core 1.1. Szukasz do najnowszej wersji? Zobacz [bieżącej wersji tego samouczka](xref:getting-started).

1. Zainstaluj oprogramowanie .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core 1.0.5 & 1.1.2 strona pliki do pobrania zestawu SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).

2. Utwórz folder dla nowego projektu platformy .NET Core.

   System macOS i Linux Otwórz okno terminala. W systemie Windows otwórz wiersz polecenia.

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

Aby obejrzeć wprowadzenie do platformy ASP.NET podstawowych pojęć i architektury, zobacz [platformy ASP.NET Core wprowadzenie](index.md) i [podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).

Użyć aplikacji platformy ASP.NET Core .NET Core lub biblioteka klas programu .NET Framework podstawowej i środowiska wykonawczego. Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
