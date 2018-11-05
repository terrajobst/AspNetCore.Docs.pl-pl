---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Wprowadzenie do First w programie Entity Framework 6 bazy danych za pomocą MVC 5 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 7fcb2b82dfa27ae192e1890c0c771d68658760a4
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021147"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Wprowadzenie do podejścia Database First w programie Entity Framework 6 z wykorzystaniem wzorca MVC 5
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych. W ostatniej części serii zostanie wdrożona lokacji i bazy danych na platformie Azure.
> 
> Ta część serii koncentruje się na utworzenie bazy danych i zapełnianie danymi.
> 
> W tej serii został napisany z uwzględnieniem materiałów przekazanych z Tom Dykstra i Ricka Andersona. Został udoskonalony na podstawie informacji pochodzących od użytkowników w sekcji komentarzy.


## <a name="introduction"></a>Wprowadzenie

W tym temacie pokazano, jak rozpocząć od istniejącej bazy danych i szybkie tworzenie aplikacji sieci web, która umożliwia użytkownikom na interakcję z danymi. Aby utworzyć aplikację sieci web używa platformy Entity Framework 6 i MVC 5. Funkcja tworzenia szkieletu ASP.NET umożliwia automatyczne generowanie kodu na wyświetlanie, aktualizowanie, tworzenie i usuwanie danych. Za pomocą narzędzia publikowania w programie Visual Studio, można łatwo wdrożysz lokacji i bazy danych na platformie Azure.

Ten temat dotyczy sytuacji, w którym masz bazę danych i chcesz wygenerować kod dla aplikacji sieci web na podstawie pól tej bazy danych. To podejście jest nazywane tworzenie pierwszej bazy danych. Jeśli nie masz już istniejącą bazę danych, możesz zamiast tego użyć podejścia o nazwie rozwiązania deweloperskiego Code First, który obejmuje Definiowanie klas danych i generowanie bazy danych przy użyciu właściwości klasy.

Wprowadzający przykład rozwiązania deweloperskiego Code First, zobacz [wprowadzenie do ASP.NET MVC 5](../introduction/getting-started.md). Na przykład bardziej zaawansowanych, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji platformy ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Aby uzyskać wskazówki dotyczące wybierania rozwiązaniem platformy Entity Framework, zobacz [podejścia do projektowania struktury jednostki](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Wymagania wstępne

Visual Studio 2013 lub Visual Studio Express 2013 for Web

## <a name="set-up-the-database"></a>Konfigurowanie bazy danych

Aby mógł naśladować środowisko o istniejącą bazę danych, zostanie najpierw utwórz bazę danych z pewnymi wstępnie wypełniony danymi, a następnie utwórz aplikację sieci web, który nawiązuje połączenie z bazą danych.

W tym samouczku został opracowany, instalacja programu LocalDB za pomocą programu Visual Studio 2013 lub Visual Studio Express 2013 for Web. Można użyć istniejącego serwera baz danych, zamiast programu LocalDB, ale w zależności od używanej wersji programu Visual Studio i typ bazy danych, wszystkie narzędzia danych w programie Visual Studio może nie być obsługiwany. Jeśli narzędzia nie są dostępne dla bazy danych, może być konieczne do wykonania niektórych kroków określonej bazy danych w pakiecie zarządzania z bazą danych.

Jeśli masz problem z narzędzia graficzne bazy danych w wersji programu Visual Studio, upewnij się, że zainstalowano najnowszą wersję narzędzia graficzne bazy danych. Aby dowiedzieć się, czy instalujesz narzędzia graficzne bazy danych, zobacz [programu Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Uruchom program Visual Studio i Utwórz **projekt bazy danych programu SQL Server**. Nadaj projektowi nazwę **ContosoUniversityData**.

![Utwórz projekt bazy danych](setting-up-database/_static/image1.png)

Masz teraz projekt pustej bazy danych. Ta baza danych zostanie wdrożona na platformie Azure w dalszej części tego samouczka, należy ustawić usługi Azure SQL Database jako platformę docelową dla projektu. Ustawienie platforma docelowa nie jest faktycznie wdrażana bazy danych. oznacza jedynie, że projekt bazy danych sprawdzi, czy projekt bazy danych jest zgodna z platformą docelową. Aby ustawić platformę docelową, otwórz **właściwości** dla projektu i wybierz **Microsoft Azure SQL Database** dla platformy docelowej.

![Platforma docelowa zestawu](setting-up-database/_static/image2.png)

Możesz utworzyć tabele wymagane na potrzeby tego samouczka, dodając skrypty SQL, które tabel. Kliknij prawym przyciskiem myszy projekt i Dodaj nowy element.

![Dodaj nowy element](setting-up-database/_static/image3.png)

Dodaj nową tabelę o nazwie Student.

![Dodaj tabelę dla uczniów](setting-up-database/_static/image4.png)

W pliku tabeli Zastąp następujący kod, aby utworzyć tabelę polecenia języka T-SQL.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Należy zauważyć, że okna projektu automatycznie synchronizuje się z kodem. Możesz pracować z kodem lub projektanta.

![Pokaż kod i projekt](setting-up-database/_static/image5.png)

Dodaj inną tabelę. Ten czas, nadaj jej nazwę na kursie i użyj następującego polecenia języka T-SQL.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

I powtórz jeszcze raz, aby utworzyć tabelę o nazwie rejestracji.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Możesz wypełnić bazę danych danymi za pomocą skryptu, który jest uruchamiany po wdrożeniu bazy danych. Skrypt po wdrożeniu należy dodać do projektu. Można użyć domyślnej nazwy.

![Dodaj skrypt po wdrożeniu](setting-up-database/_static/image6.png)

Dodaj poniższy kod T-SQL do skryptu po wdrożeniu. Ten skrypt po prostu dodaje dane do bazy danych po znalezieniu odpowiedniego rekordu. Nie zastąpić lub Usuń wszystkie dane, który został wprowadzony w bazie danych.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Należy zauważyć, że skrypt po wdrożeniu jest uruchamiany za każdym razem, aby wdrożyć projekt bazy danych. W związku z tym należy starannie wziąć pod uwagę wymogi podczas pisania tego skryptu. W niektórych przypadkach warto zacząć od nowa z znanego zestawu danych, za każdym razem, gdy projekt został wdrożony. W innych przypadkach może nie chcieć zmienić istniejące dane w dowolny sposób. W zależności od wymagań, możesz zdecydować, czy potrzebujesz skrypt po wdrożeniu lub potrzebnych do uwzględnienia w skrypcie. Aby uzyskać więcej informacji na temat Wypełnianie bazy danych za pomocą skryptu po wdrożeniu, zobacz [łącznie z danymi w projekt bazy danych serwera SQL](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Masz teraz 4 pliki skryptów SQL, ale nie rzeczywiste tabele. Jesteś gotowy wdrożyć projekt bazy danych localdb. W programie Visual Studio kliknij przycisk Start (lub F5) do tworzenia i wdrażania projektu bazy danych. Sprawdź kartę danych wyjściowych, aby sprawdzić, czy kompilacja i wdrażanie zakończyło się pomyślnie.

![Pokaż dane wyjściowe](setting-up-database/_static/image7.png)

Aby sprawdzić, czy nowa baza danych została utworzona, otwórz **Eksplorator obiektów SQL Server** i wyszukaj nazwę projektu na serwerze poprawne lokalnej bazy danych (w tym przypadku **\ProjectsV12 (localdb)**).

![Pokaż nowej bazy danych](setting-up-database/_static/image8.png)

Aby zobaczyć, że tabele są wypełniane przy użyciu danych, kliknij prawym przyciskiem myszy tabelę, a następnie wybierz pozycję **dane widoku**.

![Pokaż dane tabeli](setting-up-database/_static/image9.png)

Wyświetlany jest widok edycji danych tabeli.

![Pokaż wyniki danych tabeli](setting-up-database/_static/image10.png)

Baza danych jest teraz i wypełniony danymi. W następnym samouczku utworzysz aplikację sieci web, dla bazy danych.

> [!div class="step-by-step"]
> [Next](creating-the-web-application.md)
