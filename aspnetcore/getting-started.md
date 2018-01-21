---
title: Wprowadzenie do platformy ASP.NET Core 2.0
author: rick-anderson
description: "Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
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

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. Przejdź do [http://localhost: 5000/o](http://localhost:5000/About) i zweryfikować zmiany.

### <a name="next-steps"></a>Następne kroki

Aby uzyskać samouczki dotyczące rozpoczynania pracy, zobacz [platformy ASP.NET Core samouczki](tutorials/index.md)

Aby obejrzeć wprowadzenie do platformy ASP.NET podstawowych pojęć i architektury, zobacz [platformy ASP.NET Core wprowadzenie](index.md) i [podstawowe informacje na temat platformy ASP.NET Core](fundamentals/index.md).

Użyć aplikacji platformy ASP.NET Core .NET Core lub biblioteka klas programu .NET Framework podstawowej i środowiska wykonawczego. Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
