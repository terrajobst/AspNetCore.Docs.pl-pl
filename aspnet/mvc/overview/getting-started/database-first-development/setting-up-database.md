---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: "Wprowadzenie do programu Entity Framework 6 Database First przy użyciu MVC 5 | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 9ecde847841eb727a0440f0483c69d1df6757815
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Wprowadzenie do programu Entity Framework 6 Database First przy użyciu MVC 5
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych. W ostatniej części serii zostanie wdrożona lokacji i bazy danych do platformy Azure.
> 
> Ta część serii koncentruje się na utworzenie bazy danych i zapełnianie danymi.
> 
> Ta seria zostało zapisane z udziałami Tomasz Dykstra i Ricka Andersona. Zostało ulepszone oparte na opinii użytkowników w sekcji uwag.


## <a name="introduction"></a>Wprowadzenie

W tym temacie przedstawiono sposób do uruchomienia z istniejącej bazy danych i szybkie tworzenie aplikacji sieci web, która umożliwia użytkownikom na interakcję z danymi. Do tworzenia aplikacji sieci web używa Entity Framework 6 i MVC 5. Funkcja szkieletów ASP.NET umożliwia automatyczne generowanie kodu dla wyświetlanie, aktualizowanie, tworzenie i usuwanie danych. Korzystając z narzędzi publikowania w programie Visual Studio, można łatwo wdrożyć lokacji i bazy danych na platformie Azure.

Ten temat dotyczy sytuacji, w których bazy danych i generowanie kodu dla aplikacji sieci web na podstawie pól tej bazy danych. Takie podejście jest nazywany programowanie pierwszej bazy danych. Jeśli nie masz już istniejącą bazę danych, zamiast tego można użyć metody o nazwie rozwoju Code First, który obejmuje Definiowanie klas danych i generowania bazy danych na podstawie właściwości klasy.

Przykład wprowadzające programowanie Code First, zobacz [wprowadzenie do platformy ASP.NET MVC 5](../introduction/getting-started.md). Na przykład bardziej zaawansowanych, zobacz [tworzenia modelu danych struktury jednostek dla aplikacji ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Aby uzyskać wskazówki dotyczące wybierania rozwiązaniem Entity Framework, zobacz [Entity Framework programowanie podejścia](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Wymagania wstępne

Visual Studio 2013 lub Visual Studio Express 2013 for Web

## <a name="set-up-the-database"></a>Baza danych

Aby używane środowisko przypominało środowisko o istniejącej bazy danych, zostanie najpierw utworzyć bazę danych z niektórych wstępnie wypełnione danych, a następnie utwórz aplikację sieci web, który nawiązuje połączenie z bazą danych.

W tym samouczku został opracowany przy użyciu bazy danych LocalDB programu Visual Studio 2013 lub Visual Studio Express 2013 for Web. Można użyć istniejącego serwera baz danych, zamiast LocalDB, ale w zależności od używanej wersji programu Visual Studio i typ bazy danych, wszystkie narzędzia danych w programie Visual Studio może nie być obsługiwany. Jeśli narzędzia nie są dostępne dla Twojej bazy danych, może być konieczne do wykonania niektórych kroków specyficzny dla bazy danych w ramach pakietu administracyjnego dla Twojej bazy danych.

Jeśli masz problem z narzędzia bazy danych w wersji programu Visual Studio, upewnij się, że zainstalowano najnowszą wersję narzędzia bazy danych. Informacje o aktualizacji lub instalowanie narzędzi bazy danych, zobacz [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/en-us/data/hh297027).

Uruchom program Visual Studio i utworzyć **projekt bazy danych programu SQL Server**. Nazwij projekt **ContosoUniversityData**.

![Utwórz projekt bazy danych](setting-up-database/_static/image1.png)

Masz teraz projektu pustej bazy danych. Ta baza danych zostanie wdrożona na platformie Azure w dalszej części tego samouczka, musisz ustawić bazy danych SQL Azure jako platforma docelowa projektu. Ustawienie platformy docelowej faktycznie niewdrażającej bazy danych. oznacza jedynie, że projekt bazy danych sprawdzi, czy projekt bazy danych jest zgodna z platformą docelową. Aby ustawić platformę docelową, otwórz **właściwości** dla projektu i wybierz **Microsoft Azure SQL Database** dla platformy docelowej.

![Platforma docelowa zestawu](setting-up-database/_static/image2.png)

Można tworzyć tabel potrzebne w tym samouczku, dodając skrypty SQL, które definiują tabel. Kliknij prawym przyciskiem myszy projektu i Dodaj nowy element.

![Dodaj nowy element](setting-up-database/_static/image3.png)

Dodaj nową tabelę o nazwie studenta.

![Dodaj tabelę dla użytkowników domowych](setting-up-database/_static/image4.png)

W pliku tabeli Zastąp następujący kod, aby utworzyć tabelę polecenia T-SQL.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Należy zauważyć, że okno projektowania automatycznie zsynchronizowana ponownie z kodem. Możesz pracować z kodu lub projektanta.

![Pokaż kod i projekt](setting-up-database/_static/image5.png)

Dodaj inną tabelą. Ten czas nadaj jej nazwę kursu i użyj następującego polecenia T-SQL.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

I powtórz jeszcze raz, aby utworzyć tabelę o nazwie rejestracji.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Należy wypełnić bazę danych z danymi za pomocą skryptu uruchamianego po wdrożeniu bazy danych. Skrypt po wdrożeniu do projektu. Można użyć domyślnej nazwy.

![Dodaj skrypt po wdrożeniu](setting-up-database/_static/image6.png)

Dodaj poniższy kod T-SQL do skryptu po wdrożeniu. Ten skrypt po prostu dodaje dane do bazy danych, gdy nie znaleziono żadnego pasującego rekordu. Nie Zastąp lub Usuń wszystkie dane, które zostały wprowadzone do bazy danych.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Należy pamiętać, że skrypt po wdrożeniu jest uruchamiany za każdym razem, gdy wdrażania projektu bazy danych. Dlatego należy uważnie wziąć pod uwagę wymogi dotyczące pisania skryptu. W niektórych przypadkach warto zacząć od nowa z znanego zestawu danych za każdym razem, gdy projekt jest wdrażany. W innych przypadkach może nie być zmienia istniejące dane w dowolny sposób. W zależności od wymagań można zdecydować, czy potrzebujesz skrypt po wdrożeniu lub co jest potrzebne do uwzględnienia w skrypcie. Aby uzyskać więcej informacji o wprowadzaniu bazy danych za pomocą skryptu po wdrożeniu, zobacz [w tym dane w projekcie bazy danych programu SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Masz teraz 4 plików skryptów SQL, ale nie rzeczywiste tabele. Można przystąpić do wdrażania projektu bazy danych do localdb. W programie Visual Studio kliknij przycisk Start (lub F5) do tworzenia i wdrażania projektu bazy danych. Sprawdź kartę danych wyjściowych, aby zweryfikować, że kompilowanie i wdrażanie zakończyło się pomyślnie.

![Pokaż dane wyjściowe](setting-up-database/_static/image7.png)

Aby zobaczyć, czy nowa baza danych została utworzona, otwórz **Eksplorator obiektów SQL Server** i wyszukaj nazwę projektu na serwerze poprawne lokalnej bazy danych (w tym przypadku **\ProjectsV12 (localdb)**).

![Pokaż nowej bazy danych](setting-up-database/_static/image8.png)

Aby sprawdzić, czy tabele są wypełniane przy użyciu danych, kliknij prawym przyciskiem myszy tabelę, a następnie wybierz **danych widoku**.

![Pokaż tabelę danych](setting-up-database/_static/image9.png)

Zostanie wyświetlony widok można edytować danych tabeli.

![Pokaż wyniki z danymi tabeli](setting-up-database/_static/image10.png)

Baza danych jest teraz i wypełniane przy użyciu danych. W następnym samouczku utworzysz aplikację sieci web dla bazy danych.

>[!div class="step-by-step"]
[Dalej](creating-the-web-application.md)
