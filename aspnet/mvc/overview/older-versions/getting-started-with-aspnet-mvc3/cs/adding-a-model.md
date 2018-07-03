---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Dodawanie modelu (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 797a4b26960881c9df60a47389a0f979b00e45cf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371603"
---
<a name="adding-a-model-c"></a>Dodawanie modelu (C#)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer, przy użyciu kodu źródłowego języka C# jest dostępny powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/adding-a-model.md) po ukończeniu tego samouczka.


## <a name="adding-a-model"></a>Dodawanie modelu

W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych. Te klasy będzie "model" część aplikacji ASP.NET MVC.

Użyjesz technologii dostępu do danych .NET Framework, znane jako Entity Framework do definiowania i pracować z tych klas modelu. Obsługuje platformy Entity Framework (często określanymi jako EF) o nazwie paradygmat programowania *Code First*. Najpierw kod pozwala na tworzenie obiektów modelu, pisząc proste klas. (Te są nazywane także POCO klas, od "obiektów CLR zwykły stary.") Można następnie skonfigurować bazę danych, tworzone na bieżąco z klas, co umożliwia przepływ pracy tworzenia bardzo szybkie i przejrzysty.

## <a name="adding-model-classes"></a>Dodawanie klasy modelu

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folderu, wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.

![](adding-a-model/_static/image1.png)

Nazwa *klasy* "Filmu".

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

Dodaj następujące właściwości pięć `Movie` klasy:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych. Każde wystąpienie `Movie` obiektu odnoszą się do wiersza w tabeli bazy danych, a każda właściwość `Movie` klasy będzie zmapowana do kolumny w tabeli.

W tym samym pliku Dodaj następujący kod `MovieDBContext` klasy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych. `MovieDBContext` Pochodzi od klasy `DbContext` dostarczane przez program Entity Framework klasy bazowej. Aby uzyskać więcej informacji na temat `DbContext` i `DbSet`, zobacz [udoskonalenia dotyczące produktywności programu Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Pełne *Movie.cs* plików znajdują się poniżej.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Tworzenie parametrów połączenia i Praca z użyciem programów SQL Server Compact

`MovieDBContext` Utworzone klasy obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych. Jedno pytanie, które możesz zadawać, jest jednak sposób określić bazę danych, która zostanie nawiązane połączenie. Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.

Otwórz katalog główny aplikacji *Web.config* pliku. (Nie *Web.config* w pliku *widoków* folderu.) Na poniższej ilustracji Pokaż obie *Web.config* plików; otwórz *Web.config* pliku zakreślony na czerwono.

![](adding-a-model/_static/image4.png)

### 

Dodaj poniższe parametry połączenia do `<connectionStrings>` element *Web.config* pliku.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

W poniższym przykładzie pokazano część *Web.config* pliku dodano nowy ciąg połączenia:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Ta niewielką ilość kodu i XML to wszystko, czego potrzebujesz do zapisu w celu reprezentowania i przechowywanie danych filmów w bazie danych.

Następnie utworzysz nowy `MoviesController` klasę, która służy do wyświetlania danych filmów i Zezwalaj użytkownikom na tworzenie nowych list filmu.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)
