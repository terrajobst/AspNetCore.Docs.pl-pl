---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Dodawanie modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 304c428b0d787e902f30c1989471c476f54d3b39
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-model"></a>Dodawanie modelu
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych. Te klasy będzie &quot;modelu&quot; częścią aplikacji ASP.NET MVC.

Użyjesz technologii dostępu do danych .NET Framework, znany jako [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) do definiowania i pracy z tych klas modelu. Obsługuje programu Entity Framework (nazywanej często EF) o nazwie modelu programowania *Code First*. Kod umożliwia najpierw utworzyć obiekty modelu pisząc proste klasy. (Te są nazywane także klasy POCO z &quot;obiektów CLR starego zwykłego.&quot;) Następnie możesz wybrać bazy danych na bieżąco z klas, dzięki czemu bardzo czyste i szybkie programowanie przepływu pracy.

## <a name="adding-model-classes"></a>Dodawanie klas modelu

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie wybierz **klasy**.

![](adding-a-model/_static/image1.png)

Wprowadź *klasy* nazwa &quot;film&quot;.

Dodaj następujące właściwości pięć `Movie` klasy:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych. Każde wystąpienie `Movie` obiektu odpowiada wiersz w tabeli bazy danych, a każda właściwość `Movie` klasy przypisze do kolumny w tabeli.

W tym samym pliku Dodaj następujące `MovieDBContext` klasy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych. `MovieDBContext` Pochodną `DbContext` pochodzącymi z programu Entity Framework w klasie podstawowej.

Aby można było odwołać `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Pełną *Movie.cs* plików są wyświetlane poniżej. (Kilka przy użyciu instrukcji, które nie są potrzebne zostały usunięte.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i pracy z bazy danych LocalDB programu SQL Server

`MovieDBContext` Obsługuje klasy utworzone zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych. Jedno pytanie, który może poprosić o jest jednak sposób Określ bazę danych, które będą łączyć się. Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.

Otwórz katalog główny aplikacji *Web.config* pliku. (Nie *Web.config* w pliku *widoków* folderu.) Otwórz *Web.config* pliku wyróżnione kolorem czerwonym.

![](adding-a-model/_static/image2.png)

Dodaj następujące parametry połączenia do `<connectionStrings>` element *Web.config* pliku.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

W poniższym przykładzie przedstawiono część *Web.config* plik o nowe parametry połączenia dodany:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Mała ilość kodu i XML to wszystko, co potrzebne do zapisu w celu reprezentowania i przechowywania danych filmu w bazie danych.

Następnie będzie utworzyć nowy `MoviesController` klasy, która służy do wyświetlania danych filmu i Zezwalaj użytkownikom na tworzenie nowych list filmu.

>[!div class="step-by-step"]
[Poprzednie](adding-a-view.md)
[dalej](accessing-your-models-data-from-a-controller.md)
