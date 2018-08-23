---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Dodawanie modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7326cefd52feb63fc2f602210ed560cdf28706a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753089"
---
<a name="adding-a-model"></a>Dodawanie modelu
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych. Te klasy będzie &quot;modelu&quot; wchodzi w skład aplikacji ASP.NET MVC.

Użyjesz technologii dostępu do danych .NET Framework, znane jako [Entity Framework](https://docs.microsoft.com/ef/) do definiowania i pracować z tych klas modelu. Obsługuje platformy Entity Framework (często określanymi jako EF) o nazwie paradygmat programowania *Code First*. Najpierw kod pozwala na tworzenie obiektów modelu, pisząc proste klas. (Te są również nazywane klasami POCO z &quot;obiektów CLR zwykły stary.&quot;) Można następnie skonfigurować bazę danych, tworzone na bieżąco z klas, co umożliwia przepływ pracy tworzenia bardzo szybkie i przejrzysty. Jeśli musisz najpierw utworzyć bazę danych, możesz wykonać ten samouczek, aby dowiedzieć się więcej o programowaniu aplikacji MVC i programem EF. Następnie można wykonać Tom Fizmakens [funkcja tworzenia szkieletu ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) samouczka, który dotyczy pierwszego podejścia bazy danych.

## <a name="adding-model-classes"></a>Dodawanie klasy modelu

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folderu, wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.

![](adding-a-model/_static/image1.png)

Wprowadź *klasy* nazwa &quot;filmu&quot;.

Dodaj następujące właściwości pięć `Movie` klasy:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych. Każde wystąpienie `Movie` obiektu odnoszą się do wiersza w tabeli bazy danych, a każda właściwość `Movie` klasy będzie zmapowana do kolumny w tabeli.

Uwaga: Aby można było używać System.Data.Entity i pokrewne klasy, należy zainstalować [pakietu NuGet programu Entity Framework](https://www.nuget.org/packages/EntityFramework/). Kliknij link, aby uzyskać dalsze instrukcje.

W tym samym pliku Dodaj następujący kod `MovieDBContext` klasy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych. `MovieDBContext` Pochodzi od klasy `DbContext` dostarczane przez program Entity Framework klasy bazowej.

Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Można to zrobić przez ręczne dodanie do przy użyciu instrukcji lub należy umieść kursor nad czerwonych falistych linii, kliknij przycisk `Show potential fixes` i kliknij przycisk `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Uwaga: Kilka nieużywane `using` instrukcje zostały usunięte. Program Visual Studio wyświetli nieużywane zależności jako szary. Możesz usunąć nieużywane zależności, ustawiając kursor nad szarego zależności, kliknij przycisk `Show potential fixes` i kliknij przycisk **Usuń nieużywane deklaracje Using.**

![](adding-a-model/_static/image3.png)

Na koniec dodaliśmy modelu (M MVC). W następnej sekcji, którą będziesz pracować parametry połączenia bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-view.md)
> [dalej](creating-a-connection-string.md)
