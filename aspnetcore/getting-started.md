---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core

> [!NOTE]
> Te instrukcje dotyczą najnowszej wersjiplatformy ASP.NET Core. Zobacz [wprowadzenie do platformy ASP.NET Core 1.1](xref:getting-started-1.1) wersji 1.1 tego dokumentu.

1. Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Utwórz nowy projekt platformy .NET Core.

   W systemach macOS i Linux otwórz okno terminala. W systemie Windows otwórz wiersz polecenia. Wprowadź następujące polecenie:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Uruchom aplikację.

    Użyj następujących poleceń, aby uruchomić aplikację:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Przejdź do [http://localhost:5000](http://localhost:5000)

5. Otwórz <em>Pages/About.cshtml</em>i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world! The time on the server is @DateTime.Now ":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) i zweryfikować zmiany.

### <a name="next-steps"></a>Następne kroki

Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)

Aby zapoznać się z wprowadzeniem do podstawowych pojęć i architektury platformy ASP.NET, zobacz [Wprowadzenie do platformy ASP.NET Core](index.md) i [Podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).

Aplikacja ASP.NET Core może używać biblioteki klas bazowych i środowiska uruchomieniowego .NET Core lub .NET Framework. Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
