---
title: Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core z kodem Visual Studio
author: rick-anderson
description: "Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core za pomocą programu Visual Studio Code"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 7c01d802e59951281c86c8eab64b7c6b9d646fbf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Wprowadzenie do korzystania z elementu Razor strony platformy ASP.NET Core z kodem Visual Studio

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core Razor strony. Firma Microsoft zaleca, należy wykonać [wprowadzenie do stron Razor](xref:mvc/razor-pages/index) przed rozpoczęciem tego samouczka. Stron razor jest zalecanym sposobem tworzenia interfejsu użytkownika dla aplikacji sieci web w ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące czynności:

* [Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy
* [Visual Studio Code](https://code.visualstudio.com)
* Kod VS [rozszerzenia C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

W terminalu uruchom następujące polecenia:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektu stron Razor. Otwórz w przeglądarce http://localhost: 5000, aby wyświetlić aplikację.

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otwórz projekt

Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.

Z programu Visual Studio (kod VS), wybierz **Plik > Otwórz Folder**, a następnie wybierz *RazorPagesMovie* folderu.

- Wybierz **tak** do **Ostrzegaj** komunikatu "zasoby wymagane do tworzenia i debugowania brakuje"RazorPagesMovie". Dodaj je?"
- Wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".

### <a name="launch-the-app"></a>Uruchom aplikację

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację bez debugowania. Alternatywnie z **debugowania** menu, wybierz opcję **uruchomić bez debugowania**.

W następnym samouczku modelu dodania do projektu. 

>[!div class="step-by-step"]
[Następnie: Dodawanie modelu](xref:tutorials/razor-pages-vsc/model)  
