---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Dodawanie modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867453"
---
<a name="adding-a-model"></a>Dodawanie modelu
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych. Te klasy będzie &quot;modelu&quot; częścią aplikacji ASP.NET MVC.

Użyjesz technologii dostępu do danych .NET Framework, znany jako [Entity Framework](https://docs.microsoft.com/ef/) do definiowania i pracy z tych klas modelu. Obsługuje programu Entity Framework (nazywanej często EF) o nazwie modelu programowania *Code First*. Kod umożliwia najpierw utworzyć obiekty modelu pisząc proste klasy. (Te są nazywane także klasy POCO z &quot;obiektów CLR starego zwykłego.&quot;) Następnie możesz wybrać bazy danych na bieżąco z klas, dzięki czemu bardzo czyste i szybkie programowanie przepływu pracy. Jeśli musisz najpierw utworzyć bazę danych, można wykonać w tym samouczku, aby dowiedzieć się więcej o rozwoju aplikacji MVC i EF. Następnie można wykonać Tomasz Fizmakens [szkieletów ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) samouczek, która obejmuje pierwszym sposobem bazy danych.

## <a name="adding-model-classes"></a>Dodawanie klas modelu

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie wybierz **klasy**.

![](adding-a-model/_static/image1.png)

Wprowadź *klasy* nazwa &quot;film&quot;.

Dodaj następujące właściwości pięć `Movie` klasy:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych. Każde wystąpienie `Movie` obiektu odpowiada wiersz w tabeli bazy danych, a każda właściwość `Movie` klasy przypisze do kolumny w tabeli.

Uwaga: Aby można było używać System.Data.Entity i powiązanymi klasami, musisz zainstalować [pakietu NuGet programu Entity Framework](https://www.nuget.org/packages/EntityFramework/). Skorzystaj z łącza, aby uzyskać dalsze instrukcje.

W tym samym pliku Dodaj następujące `MovieDBContext` klasy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych. `MovieDBContext` Pochodną `DbContext` pochodzącymi z programu Entity Framework w klasie podstawowej.

Aby można było odwołać `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Można to zrobić przez ręczne dodanie przy użyciu instrukcji, lub umieść kursor nad czerwoną linie dowolnym kształcie, kliknij przycisk `Show potential fixes` i kliknij przycisk `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Uwaga: Niektóre nieużywane `using` instrukcje zostały usunięte. Visual Studio wyświetli nieużywane zależności jako szary. Możesz usunąć nieużywane zależności, ustawiając kursor nad szarego zależności, kliknij przycisk `Show potential fixes` i kliknij przycisk **Usuń nieużywane deklaracje Using.**

![](adding-a-model/_static/image3.png)

Na koniec dodaliśmy modelu (M MVC). W następnej sekcji będzie współpracować parametry połączenia bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-view.md)
> [dalej](creating-a-connection-string.md)
