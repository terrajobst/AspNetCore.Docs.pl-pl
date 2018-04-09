---
title: Wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code
author: rick-anderson
description: Poznaj podstawy tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony z kodem Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 11164fda108e7cc87791e777a66e58f33fe558ae
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Wprowadzenie do platformy ASP.NET Core Razor stron w programie Visual Studio Code

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka. Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

W terminalu uruchom następujące polecenia:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor. Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otwórz projekt

Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.

Z programu Visual Studio (kod VS), wybierz **Plik > Otwórz Folder**, a następnie wybierz *RazorPagesMovie* folderu.

- Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"RazorPagesMovie". Dodaj je?"
- Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".

### <a name="launch-the-app"></a>Uruchom aplikację

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację bez debugowania. Alternatywnie z **debugowania** menu, wybierz opcję **uruchomić bez debugowania**.

W następnym samouczku modelu dodania do projektu. 

> [!div class="step-by-step"]
> [Następnie: Dodawanie modelu](xref:tutorials/razor-pages-vsc/model)  
