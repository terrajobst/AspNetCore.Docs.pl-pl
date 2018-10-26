---
title: Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code
author: rick-anderson
description: Poznaj podstawy tworzenia aplikacji sieci web stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 9ea66134c524a6a1a670d55bae4e66cf38a45274
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089855"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Rozpoczynanie pracy ze stronami ASP.NET Core Razor w programie Visual Studio Code

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony. Firma Microsoft zaleca, po ukończeniu [wprowadzenie do stron Razor](xref:razor-pages/index) przed rozpoczęciem tego samouczka. Strony razor jest zalecany sposób tworzenia interfejsu użytkownika dla aplikacji sieci web w programie ASP.NET Core.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

W terminalu uruchom następujące polecenia:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektów stron Razor. Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.

![Strona główna lub indeks](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Otwórz projekt

Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.

W programie Visual Studio Code (VS Code) wybierz **Plik > Otwórz Folder**, a następnie wybierz pozycję *RazorPagesMovie* folderu.

- Wybierz **tak** do **Ostrzegaj** komunikat "wymagane zasoby do tworzenia i debugowania brakuje"RazorPagesMovie". Dodane?"
- Wybierz **przywrócić** do **informacje** komunikatu "Istnieją nierozwiązane zależności".

### <a name="launch-the-app"></a>Uruchom aplikację

Naciśnij kombinację klawiszy Ctrl + F5, aby uruchomić aplikację bez debugowania. Alternatywnie z **debugowania** menu, wybierz opcję **Rozpocznij bez debugowania**.

W następnym samouczku dodamy modelu do projektu. 

> [!div class="step-by-step"]
> [Następnie: Dodawanie modelu](xref:tutorials/razor-pages-vsc/model)  
