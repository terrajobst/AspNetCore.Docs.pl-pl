---
uid: whitepapers/aspnet-data-access-content-map
title: Dostęp do danych w programie ASP.NET — zalecane zasoby | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten temat zawiera łącza do zasobów dokumentacji o tym, jak uzyskać dostęp do danych w aplikacjach sieci web platformy ASP.NET, przede wszystkim za pomocą programu Entity Framework i SQL Se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 3a8d7de622fd2d0b229dba3af31557a172b90df8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396367"
---
<a name="aspnet-data-access---recommended-resources"></a>Dostęp do danych w programie ASP.NET — zalecane zasoby
====================
> Ten temat zawiera łącza do dokumentacji zasoby o tym, jak uzyskać dostęp do danych w aplikacjach sieci web platformy ASP.NET, przede wszystkim za pomocą platformy Entity Framework i programu SQL Server.
> 
> Jeśli znasz bardzo blogu, [stackoverflow](http://stackoverflow.com) wątku lub dowolny link, który powinien być przydatny, [Wyślij do nas wiadomość e-mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) z linkiem.
> 
> Zaktualizowano ostatnie 4/3/2014


Temat zawiera następujące sekcje:

- [Wprowadzenie do dostępu do danych w programie ASP.NET:](#gettingstarted)
- [Używający narzędzia Entity Framework](#ef)

    - [Najpierw za pomocą kodu struktury jednostki](#cf)
    - [Za pomocą migracje Code First Framework jednostki](#efcfmigrations)
    - [Najpierw za pomocą bazy danych programu Entity Framework, lub Modeluj pierwszy (projektancie platformy EF)](#efdbf)
    - [Ładowanie powiązanych danych platformy Entity Framework (Ładowanie z opóźnieniem, ładowanie Eager i jawne ładowanie)](#efrelateddata)
    - [Optymalizacja wydajności programu Entity Framework](#optimizingef)
    - [Obsługa współbieżności w aplikacji Entity Framework](#efconcurrency)
    - [Książki na temat programu Entity Framework](#efbooks)
    - [Dodatkowe zasoby programu Entity Framework](#otherefresources)
- [Powiązywanie danych w sieci Web platformy ASP.NET formularzy aplikacji](#wfdatabinding)

    - [Za pomocą sieci Web Forms wiązanie modelu](#wfmodelbinding)
    - [Za pomocą sieci Web Forms kontrolki źródła danych](#wfdsc)
    - [Za pomocą sieci Web Forms, formanty powiązane z danymi i wyrażenia wiązania danych](#wfdbc)
- [Praca z bazami danych programu SQL Server](#sqlserver)

    - [Praca z bazy danych programu SQL Server Express LocalDB](#sslocaldb)
    - [Praca z bazami danych SQL Server Express](#sse)
    - [Praca z usługą Windows Azure SQL Database](#ssdb)
    - [Wybieranie między programami SQL Server i Windows Azure SQL Database](#ssdbchoosing)
- [Praca z systemy zarządzania bazami danych NoSQL](#nosql)
- [Za pomocą zapytań LINQ w aplikacjach ASP.NET](#linq)
- [Szkieletów danych dynamicznych](#dd)
- [Zabezpieczanie dostępu do danych](#securing)
- [Optymalizacja wydajności dostępu do danych](#optimizingdataaccess)
- [Wdrażanie bazy danych](#deploying)
- [Uzyskiwanie dostępu do danych za pośrednictwem usługi sieci Web](#webservice)
- [Dodatkowe zasoby](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Wprowadzenie do dostępu do danych w programie ASP.NET:

- [Opcje magazynu danych (tworzenie rzeczywistych aplikacji w chmurze przy użyciu platformy Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Rozdział książki elektronicznej dotyczące programowania dla chmury. Wprowadza bazy danych NoSQL jako alternatywę, które zwykle pomijane przez wielu deweloperów zapoznać się z relacyjnych baz danych. Przedstawia informacje o wytycznych dotyczących co wziąć pod uwagę podczas wybierania relacyjnych NoSQL i/lub wybranie konkretnej platformy.
- [Opcje dostępu do danych w programie ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Wprowadzenie do opcji dostępu do danych relacyjnych baz danych dla platformy ASP.NET i wskazówki dotyczące wybierz platformy, a dostęp do metod, które są odpowiednie dla danego scenariusza.
- [Relacyjna baza danych](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Jeśli jeszcze nie znasz relacyjnych baz danych, zobacz tę stronę, aby zapoznać się z relacyjnej bazy danych terminy i pojęcia. Wprowadzenie do programu SQL Server w szczególności zobacz [Praca z bazami danych programu SQL Server](#sqlserver) w dalszej części tego tematu.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Używający narzędzia Entity Framework

- [Podejścia do projektowania struktury jednostki](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Wskazówki dotyczące sposobu wybierania platformy Entity Framework podejście do tworzenia pierwszej bazy danych modelu pierwszej lub Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Najpierw za pomocą kodu struktury jednostki
  

Następujące samouczki oferty aplikacji przykładowych możliwych do pobrania:

- [Wprowadzenie do programów EF 6 z wykorzystaniem MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Obejmuje szerokiej gamy scenariuszy Entity Framework Code First, w tym migracje i programów EF 6 takich funkcji, połączeń, przejmowanie poleceń i asynchronicznych. Jest to zaktualizowaną wersję [programów EF 5 / serii MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Wcześniej serii zawiera samouczek, w repozytorium i jednostki pracy wzorców, które nie wchodzi w nową serię.
- [Wprowadzenie do platformy ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Obejmuje mniejszą niż pierwszy scenariuszy zakresu Entity Framework kodu, ale nie bardziej złożone zadania wprowadzenia funkcji MVC.
- [Wiązania modelu i formularzy sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). Używa Code First w aplikacji formularzy sieci Web.
- [Wprowadzenie do wzorca ASP.NET 4.5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wprowadzenie do formularzy sieci Web za pomocą część pokrycia Code First. Zastosowań wiązanie modelu.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Używa Code First w aplikacji MVC 3 handlu elektronicznego, która implementuje również członkostwo i autoryzacja. Wersja MVC i systemu członkostwa programu ASP.NET (uwierzytelnianie i autoryzacja) używane w tym miejscu są przestarzałe; Aby uzyskać więcej aktualności informacji dotyczących członkostwa ASP.NET, zobacz [ https://asp.net/identity ](https://asp.net/identity).

Inne zasoby dotyczące oprogramowania:

- [Entity Framework - kod najpierw do istniejącej bazy danych](https://msdn.microsoft.com/data/jj200620). MSDN. Wideo i wskazówki, który pokazuje, jak za pomocą Code First istniejącej bazy danych.
- [Centrum deweloperów danych — programu Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Przewodnik dotyczący dokumentacji programu Entity Framework, który został utworzony i utrzymywany przez zespół programu Entity Framework, zobacz [wprowadzenie](https://msdn.microsoft.com/data/ee712907) łącza.

Zobacz też [Książki poświęcone Entity Framework](#efbooks) i [dodatkowe zasoby programu Entity Framework](#otherefresources) w dalszej części tego tematu.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Za pomocą migracje Code First Framework jednostki
  

Większość Code First samouczków wymienionych powyżej cover migracji. Zobacz też następujące zasoby.

- [Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 2-częściowych serii samouczków pokazano, jak użyć migracje Code First wdrażanie bazy danych.
- [Wdrażanie bezpiecznej aplikacji ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w witrynie sieci Web platformy Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Jak użyć migracje do wdrożenia danych członkostwa i aplikacji na platformie Azure.
- [Omówienie wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Zobacz **Konfigurowanie wdrażania bazy danych w programie Visual Studio** sekcji, aby dowiedzieć się jak migracje Code First jest zintegrowana z funkcji wdrażania w sieci web programu Visual Studio.
- [Centrum deweloperów danych — migracje Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Dokumentacja migracje zespołu programu Entity Framework.
- [Migracje zrzut ekranu serii](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog platformy EF). Trzy pliki wideo na zaawansowanie zagadnienia dotyczące migracji Code First.
- [Kod pierwszej migracji z witryn stron sieci Web platformy ASP.NET](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting blog). Pokazuje, jak za pomocą migracje Code First witryny ASP.NET Web Pages, umieszczając kontekstu danych w projekcie biblioteki klas programu Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Najpierw za pomocą bazy danych programu Entity Framework, lub Modeluj pierwszy (projektancie platformy EF)

- [Wprowadzenie do First w programie Entity Framework 6 bazy danych za pomocą MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Uruchamianie skryptu w Eksploratorze serwera, aby utworzyć bazę danych, a następnie utworzyć model danych za pomocą projektanta programu Entity Framework. Pokazuje, jak utworzyć proste CRUD web pages i innych danych, funkcje, które można wykonać jednym z samouczków Code First od wszystkich EF przepływów pracy za pomocą tego samego interfejsu API typu DbContext obsługi.

Starsze są następujące zasoby. Są one przydatne, jeśli chcesz używać programu Entity Framework w wersji 4.0 i chcesz użyć kontroli źródła danych dla powiązania danych w aplikacji formularzy sieci Web.

- [Wprowadzenie do programu Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Ilustruje sposób używania **EntityDataSource** kontroli.
- [Kontynuowanie pracy z programem Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(pokazano, jak **ObjectDataSource** kontroli. Zawiera samouczek dotyczący obsługi współbieżności, samouczek dotyczący wydajności EF i zapoznać się z samouczkiem na tym, co nowego w programie EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Obsługa powiązanych danych platformy Entity Framework (Ładowanie z opóźnieniem, ładowanie Eager i jawne ładowanie)

- [Odczytywanie danych powiązanych z platformą Entity Framework w aplikacji ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Możesz pisać kod najpierw MVC przykładowej aplikacji. Metody pokazano stosuje się również do wiązania modelu formularzy sieci Web i bazy danych pierwszego przepływu pracy.
- [Centrum deweloperów danych — trwa ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232) (MSDN). Zespół platformy Entity Framework dokumentacji dotyczącej ładowanie powiązanych danych.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optymalizacja wydajności programu Entity Framework

- [Zaawansowane scenariusze platformy Entity Framework dla aplikacji ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Pokazuje sposób wykonania instrukcji SQL lub zadzwoń do przechowywanych procedur, jak wyłączyć wykrywania zmian i jak wyłączyć sprawdzanie poprawności podczas zapisywania zmian.
- [Zagadnienia dotyczące wydajności dla programu Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Zagadnienia dotyczące wydajności (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maksymalizacja wydajności przy użyciu programu Entity Framework na aplikację sieci Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Ma zastosowanie do programu Entity Framework 4.0.
- Zobacz też [dostęp do danych ASP.NET optymalizacji](#optimizingdataaccess) w dalszej części tego tematu.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Obsługa współbieżności w aplikacji Entity Framework

- [Obsługa współbieżności przy użyciu platformy Entity Framework w aplikacji ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Możesz pisać kod najpierw DbContext interfejsu API za pomocą aplikacji przykładowej MVC.
- [Centrum deweloperów danych — wzorce optymistycznej współbieżności](https://msdn.microsoft.cus/data/jj592904) (MSDN). Dokumentacja współbieżności zespołu programu Entity Framework.
- [Obsługa współbieżności przy użyciu programu Entity Framework na aplikację sieci Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Ma zastosowanie do programu Entity Framework 4.0. Bazy danych, najpierw ObjectContext interfejsu API za pomocą aplikacji przykładowej formularzy sieci Web.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Książki na temat programu Entity Framework

- [Platformy Entity Framework programowania: Kontekst DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman i Rowan Miller.
- [Platformy Entity Framework programowania: Kod: pierwszy](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman i Rowan Miller.

Oba te książki są aktualne z bieżącym zalecane techniki. Zapewniają one bardziej kompleksowy, ale łatwa do wykonania wprowadzenie do narzędzia Entity Framework niż niczego, które są dostępne w Internecie. Inną książkę [programowania programu Entity Framework](http://shop.oreilly.com/product/9780596807252.do) przez Julie Lerman jest większych i bardziej kompleksowe, ale jego starsze i wiele technik poruszono w nim nie są już zalecany sposób używać programu Entity Framework. Zobacz też listy książek zalecane przez zespół programu Entity Framework [Centrum deweloperów danych — książki](https://msdn.microsoft.com/data/aa937716) w witrynie MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Inne zasoby programu Entity Framework

- [Blog zespołu programu Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Jedna z najlepszych zasobów najbardziej aktualne informacje i zapowiedzi nowe ulepszenia. W przypadku innych blogów związane z platformą EF, zobacz Lista blogów w [Rozpoczynanie pracy z platformą Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Zobacz **punktów danych** kolumny, która jest często na tematy związane z programu Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Powiązywanie danych w sieci Web platformy ASP.NET formularzy aplikacji

- [ASP.NET Web Forms Data Access Options](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Za pomocą sieci Web Forms wiązanie modelu

- [Wiązania modelu i formularzy sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). Za pomocą funkcji EF Code First serii samouczków.
- [Web Forms modelu powiązania część 1: Wybór danych](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog Scotta Guthrie). W wpisy na blogu starszy właściwość, która nosi nazwę aktualnie ItemType nosiła nazwę ModelType, ale w przeciwnym razie informacje, które zawierają jest nieprawidłowy.
- [Web Forms modelu powiązania część 2: Filtrowanie danych](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog Scotta Guthrie).
- [Sieci Web Forms modelu powiązania część 3: Aktualizowanie i sprawdzanie poprawności](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog Scotta Guthrie).
- [Wiązanie modelu programu ASP.NET 4.5 Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (wideo).
- [Powiązanie modelu, część 1 — Wybieranie danych](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (wideo).
- [Powiązanie modelu, część 2 — filtrowanie](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (wideo).
- [Wprowadzenie do wzorca ASP.NET 4.5 Web Forms — wyświetlanie danych elementów i szczegóły](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Za pomocą sieci Web Forms kontrolki źródła danych

- [Formanty serwera sieci Web źródła danych](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Przedstawiamy wersję dostawcy danych dynamicznych i kontrola EntityDataSource dla platformy Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog tworzenie aplikacji sieci Web firmy Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Za pomocą sieci Web Forms, formanty powiązane z danymi i wyrażenia wiązania danych

- [Wiązania modelu i formularzy sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). Serii samouczków, który używa programu EF Code First.
- [Wprowadzenie do wzorca ASP.NET 4.5 Web Forms — wyświetlanie danych elementów i szczegóły](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Silnie Typizowane kontrolki danych](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog Scotta Guthrie).
- [Silnie Typizowane kontrolki danych](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (wideo).
- [Program ASP.NET 4.5 Typizowane kontrolki danych sieci Web formularzy silne](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (wideo).
- [Powiązane z danymi formanty serwera sieci Web](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Omówienie wyrażenia wiązania danych](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Ta strona obejmuje tylko **Eval** i **powiązać**; nie został zaktualizowany do uwzględnienia **elementu** i **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Praca z bazami danych programu SQL Server

- [Funkcje bazy danych serwera SQL](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Aby uzyskać ogólne wprowadzenie do szerokiego zakresu tematów programu SQL Server zobacz wpisy w niej w spisie treści.
- [Wersje programu SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Podsumowanie dostępne wersje programu SQL Server, wraz z łączami do informacji na temat każdego z nich.)
- [Parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [W przypadku aplikacji sieci Web platformy ASP.NET przy użyciu programu SQL Server Compact](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Program Microsoft SQL Server: Bazy danych przykładów produktu](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Przykładowe bazy danych AdventureWorks.
- [Instalowanie przykładowych baz danych](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Oprócz metod, pokazano poniżej, możesz również pobrać jednego z przykładowych plików .mdf do aplikacji\_folderu danych projektu sieci web, przekonwertować bazy danych LocalDB, a następnie utworzyć parametry połączenia LocalDB. Aby uzyskać informacje o tym, jak to zrobić, zobacz [porady: uaktualnienie do LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Zobacz też następujące sekcje w pracy z programu SQL Server Express i LocalDB i Wybieranie między programami SQL Server i bazy danych SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Praca z bazy danych programu SQL Server Express LocalDB

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Oficjalna MSDN wprowadzenie do LocalDB.
- [Parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Porady: uaktualnienie do LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Jak przeprowadzić migrację pliku MDF z wcześniejszej wersji programu SQL Server Express LocalDB. Masz również przechodzić przez ten proces, jeśli Pobierz jeden z [bazy danych programu SQL Server 2012 przykładowej](https://go.microsoft.com/fwlink/?linkid=117483).
- [Wprowadzenie do LocalDB, ulepszone programu SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog programu SQL Server Express). Ma większej ilości informacji kontekstowych na Dlaczego LocalDB został utworzony, nie znajduje się w bibliotece MSDN.
- [LocalDB: Gdzie jest Moja bazy danych?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog programu SQL Server Express). Informacje na temat której tworzone są pliki bazy danych LocalDB.
- [LocalDB przy użyciu usługi IIS, część 1: profil użytkownika](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog programu SQL Server Express). LocalDB nie jest przeznaczona do pracy z usługami IIS. Ta seria wpisów w blogu wyjaśnia problemy i obejścia.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Praca z bazami danych SQL Server Express

- [Parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Jeśli używasz ustawienie parametrów połączenia AttachDBFileName przy użyciu programu SQL Server Express, sekcji szczególnie wystąpienia użytkownika na tej stronie.
- [Jak przejęcie na własność swoje lokalne programu SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog programu SQL Server Express). Typowy problem nie jest możliwe do pracy z programu SQL Server Express bazy danych, ponieważ nie jesteś administratorem w wystąpieniu programu SQL Server Express. Domyślnie tylko osoba, która zainstalowała program SQL Server Express jest administratorem. Ten blog wyjaśnia, jak przyznać sobie administratora programu SQL Server Express, jeśli jesteś administratorem na komputerze.
- [Moja aplikacja sieci web platformy ASP.NET można użyć bazy danych programu SQL Server Express w środowisku produkcyjnym?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Praca z usługą Windows Azure SQL Database

- [Wdrażanie aplikacji zabezpieczenia platformy ASP.NET MVC z członkostwa, uwierzytelnianiem OAuth i bazy danych SQL Database to a Windows Azure Web Site](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (witryny Microsoft Azure).
- [Bazy danych SQL](https://docs.microsoft.com/azure/sql-database/) (witryny Microsoft Azure). Rozpoczynanie pracy — samouczki i przewodniki z instrukcjami.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Węzeł najwyższego poziomu w spisie treści dla usługi SQL Database w witrynie MSDN.
- [Indeks artykułów typu wiki biblioteki TechNet bazy danych Windows Azure SQL](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (w witrynie Microsoft TechNet).
- [Blok aplikacji do obsługi błędów przejściowych](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Struktura, która umożliwia obsługę przejściowych błędów sieci i połączenia błędy powstałe w wyniku ograniczenia przepustowości. Dostępne w pakiecie NuGet: [Enterprise Library 5.0 — blok aplikacji obsługi błędów przejściowych](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Wprowadzenie do bazy danych SQL Database i programu Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Zestaw szkoleniowy systemu Azure Windows](https://www.microsoft.com/download/details.aspx?id=8396) (Centrum pobierania Microsoft). Zawiera warsztaty dla bazy danych SQL.
- [Forum społeczności systemu Windows usługi Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Przenoszenie do systemu Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Jeden rozdziału kompleksowy scenariusz end-to-end przez zespół Microsoft Patterns and Practices. Dlaczego warto przeprowadzić migrację obejmuje i jak przeprowadzić migrację z programu SQL Server do bazy danych SQL.
- [Migrowanie bazy danych SQL Server do usługi Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Kreator migracji bazy danych SQL](http://sqlazuremw.codeplex.com/). To narzędzie typu open source do migracji baz danych do i z bazy danych SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Wybieranie między programami SQL Server i Windows Azure SQL Database

- [Porównaj programu SQL Server w usłudze Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (w witrynie Microsoft TechNet).
- [Migracja danych do usługi Windows Azure SQL Database: narzędzia i techniki](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Zawiera sekcje, porównaj programu SQL Server do usługi SQL Database, które zawiera wskazówek na czas migracji z programu SQL Server do bazy danych SQL.
- [Przewodnik po dostarczania bazy danych SQL Azure Windows](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (w witrynie Microsoft TechNet).
- [Ograniczenia funkcji programu SQL Server (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage i bazy danych Azure SQL Windows — porównanie i różnice](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Dla aplikacji, który można wdrożyć na platformie Windows Azure usługi Windows Azure Table storage mogą być alternatywę dla systemu Windows Azure SQL Database. Ten temat ułatwia podjęcie decyzji o następujących alternatyw.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Wytyczne i ograniczenia (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Praca z systemy zarządzania bazami danych NoSQL

- [Data Services — Azure Windows](https://www.windowsazure.com/develop/net/data/) (witryny Microsoft Azure). Zobacz [Przewodnik po funkcjach usługi Table Service](https://docs.microsoft.com/azure/) i **danych Big Data** części strony.
- [Przy użyciu tabel magazynu ASP.NET obejmujące wiele warstw aplikacji, kolejek i obiektów blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (witryny Microsoft Azure). Samouczek end-to-end z aplikacją do pobrania próbki, która używa tabel NoSQL magazynu Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Za pomocą zapytań LINQ w aplikacjach ASP.NET

- [Opcje dostępu do danych w programie ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Zawiera wprowadzenie do LINQ.
- [Filmy szkoleniowe LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog Stagner Jan).
- [Wątku Forum programu ASP.NET, wraz z łączami do dynamicznego zasoby LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Szkieletów danych dynamicznych

- [Szablony projektów dla danych dynamicznych](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Wskazówki dotyczące kiedy należy używać projektów danych dynamicznych.
- [Dane dynamiczne ASP.NET](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Zabezpieczanie dostępu do danych

- [Zabezpieczanie dostępu do danych w programie ASP.NET:](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Zagadnienia dotyczące zabezpieczeń (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Instrukcje: Zabezpieczanie parametry połączenia, korzystając z kontrolki źródła danych](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optymalizacja wydajności dostępu do danych

- [Przegląd wydajności ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Program ASP.NET buforowanie](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Poprawianie wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Istnieje ostrzeżenie "Zawartość wycofana" u góry tej strony, ale większość informacji jest nadal są prawidłowe i nie nie porównywalne do zaktualizowanego zasobu.
- [Poprawianie wydajności serwera SQL](https://msdn.microsoft.com/library/ff647793) (MSDN). Ten sam komentarz jako poprzedniej konsolidacji.

Zobacz też [wydajności Optymalizacja programu Entity Framework](#optimizingef) we wcześniejszej części tego tematu.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Wdrażanie bazy danych

- [Wdrażanie sieci Web platformy ASP.NET — zalecane zasoby](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Uzyskiwanie dostępu do danych za pośrednictwem usługi sieci Web

- [Uzyskiwanie dostępu do danych za pośrednictwem usługi internetowej](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Wskazówki dotyczące kiedy należy używać interfejsu API sieci Web i usługi WCF.
- [Wprowadzenie do wzorca ASP.NET Web API](../web-api/index.md).
- [Usługi danych WCF](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Dostęp do danych w programie ASP.NET — często zadawane pytania](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Web Forms samouczków — danych](../web-forms/overview/data-access/index.md). Większość z tych samouczków jest stosunkowo stare; Upewnij się, możesz przeczytać [opcje dostępu do danych platformy ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) i [opcje magazynu danych (tworzenie rzeczywistych aplikacji w chmurze przy użyciu platformy Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) pierwszy tak, aby nie zagłębieniem się zbyt daleko metoda dostępu do danych, który nie jest PRAWDA dla danego scenariusza.
- [Mapa zawartości platformy ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages samouczków — danych](../web-pages/overview/data/index.md).
- [Uzyskiwanie dostępu do danych w programie Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Zawiera listę tych łączy podobną do tej zawartości mapy, ale ze szczególnym uwzględnieniem programu Visual Studio zamiast programu ASP.NET.
