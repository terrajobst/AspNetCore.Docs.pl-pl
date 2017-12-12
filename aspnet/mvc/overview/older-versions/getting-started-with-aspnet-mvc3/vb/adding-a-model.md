---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Dodawanie modelu (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a>Dodawanie modelu (VB)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-a-model.md) tego samouczka.


## <a name="adding-a-model"></a>Dodawanie modelu

W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych. Te klasy będzie "modelu" części aplikacji ASP.NET MVC.

Użyjesz technologii dostępu do danych .NET Framework, znany jako Entity Framework do definiowania i pracy z tych klas modelu. Obsługuje programu Entity Framework (nazywanej często EF) o nazwie modelu programowania *Code First*. Kod umożliwia najpierw utworzyć obiekty modelu pisząc proste klasy. (Te są nazywane także POCO klasy, z "zwykły stary CLR obiekty".) Następnie możesz wybrać bazy danych na bieżąco z klas, dzięki czemu bardzo czyste i szybkie programowanie przepływu pracy.

## <a name="adding-model-classes"></a>Dodawanie klas modelu

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie wybierz **klasy**.

![](adding-a-model/_static/image1.png)

Nazwa klasy "Filmu".

Dodaj następujące właściwości pięć `Movie` klasy:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych. Każde wystąpienie `Movie` obiektu odpowiada wiersz w tabeli bazy danych, a każda właściwość `Movie` klasy przypisze do kolumny w tabeli.

W tym samym pliku Dodaj następujące `MovieDBContext` klasy:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych. `MovieDBContext` Pochodną `DbContext` pochodzącymi z programu Entity Framework w klasie podstawowej. Aby uzyskać więcej informacji na temat `DbContext` i `DbSet`, zobacz [poprawy wydajności Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Aby można było odwołać `DbContext` i `DbSet`, należy dodać następujące `imports` instrukcji w górnej części pliku:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Pełną *Movie.vb* plików są wyświetlane poniżej.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Tworzenie parametrów połączenia i Praca z programu SQL Server Compact

`MovieDBContext` Obsługuje klasy utworzone zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych. Jedno pytanie, który może poprosić o jest jednak sposób Określ bazę danych, które będą łączyć się. Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.

Otwórz katalog główny aplikacji *Web.config* pliku. (Nie *Web.config* w pliku *widoków* folderu.) Na poniższym obrazie Pokaż zarówno *Web.config* plików; otwórz *Web.config* w czerwonym kółku pliku.

![](adding-a-model/_static/image2.png)

## 

Dodaj następujące parametry połączenia do `<connectionStrings>` element *Web.config* pliku.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

W poniższym przykładzie przedstawiono część *Web.config* plik o nowe parametry połączenia dodany:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Mała ilość kodu i XML to wszystko, co potrzebne do zapisu w celu reprezentowania i przechowywania danych filmu w bazie danych.

Następnie będzie utworzyć nowy `MoviesController` klasy, która służy do wyświetlania danych filmu i Zezwalaj użytkownikom na tworzenie nowych list filmu.

>[!div class="step-by-step"]
[Poprzednie](adding-a-view.md)
[dalej](accessing-your-models-data-from-a-controller.md)
