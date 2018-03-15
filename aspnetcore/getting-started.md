---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: "Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: f772bd922d9e474d5ad99c08af19c90fe06027af
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="get-started-with-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core

> [!NOTE]
> Te instrukcje dotyczą najnowszą wersję platformy ASP.NET Core. Wyszukiwanie, aby rozpocząć korzystanie z wcześniejszej wersji? Zobacz [w wersji 1.1 tego samouczka](xref:getting-started-1.1).

1. Zainstaluj [.NET Core](https://www.microsoft.com/net/core/).

2. Utwórz nowy projekt platformy .NET Core.

   System macOS i Linux Otwórz okno terminala. W systemie Windows otwórz wiersz polecenia. Wprowadź następujące polecenie:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. Uruchom aplikację.

    Użyj następujących poleceń, aby uruchomić aplikację:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. Przejdź do [http://localhost: 5000](http://localhost:5000)

6. Otwórz *Pages/About.cshtml* i modyfikować strony, aby wyświetlić komunikat "Hello, world! Czas na serwerze jest @DateTime.Now ":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. Przejdź do [http://localhost: 5000/o](http://localhost:5000/About) i zweryfikować zmiany.

### <a name="next-steps"></a>Następne kroki

Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)

Aby obejrzeć wprowadzenie do platformy ASP.NET podstawowych pojęć i architektury, zobacz [platformy ASP.NET Core wprowadzenie](index.md) i [podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).

Użyć aplikacji platformy ASP.NET Core .NET Core lub biblioteka klas programu .NET Framework podstawowej i środowiska wykonawczego. Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
