---
uid: whitepapers/aspnet-data-access-content-map
title: Dostęp do danych programu ASP.NET - zalecane zasoby | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten temat zawiera linki do zasobów dokumentacji dotyczących dostępu do danych w aplikacji sieci web programu ASP.NET, przede wszystkim za pomocą programu Entity Framework i SQL Se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 16364951544dff33c9cdb8c8e330cc93de192c47
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28048262"
---
<a name="aspnet-data-access---recommended-resources"></a>Dostęp do danych programu ASP.NET - zalecane zasobów
====================
> Ten temat zawiera linki do zasobów dokumentacji dotyczących dostępu do danych w aplikacji sieci web programu ASP.NET, przede wszystkim za pomocą programu Entity Framework i programu SQL Server.
> 
> Jeśli znasz dużą blogu, [stackoverflow](http://stackoverflow.com) wątku lub innych łącza, które będą przydatne [Wyślij do nas wiadomość e-mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) z łączem.
> 
> Ostatni zaktualizowano 4 3 2014-


Temat zawiera następujące sekcje:

- [Wprowadzenie do korzystania z dostępu do danych w programie ASP.NET](#gettingstarted)
- [Przy użyciu programu Entity Framework](#ef)

    - [Najpierw przy użyciu Entity Framework Code](#cf)
    - [Przy użyciu migracje Code First Framework jednostki](#efcfmigrations)
    - [Najpierw przy użyciu bazy danych programu Entity Framework lub modelu pierwszej (programie EF Designer)](#efdbf)
    - [Trwa ładowanie powiązanych danych w programie Entity Framework (ładowania opóźnionego ładowania wczesny i jawnego ładowania)](#efrelateddata)
    - [Optymalizacja wydajności Framework jednostki](#optimizingef)
    - [Obsługa współbieżności w aplikacji Entity Framework](#efconcurrency)
    - [Publikacje na temat programu Entity Framework](#efbooks)
    - [Dodatkowe zasoby programu Entity Framework](#otherefresources)
- [Wiązanie danych w sieci Web ASP.NET formularzy aplikacji](#wfdatabinding)

    - [Używanie składnika Web Forms wiązania modelu](#wfmodelbinding)
    - [Używanie składnika Web Forms kontrolki źródła danych](#wfdsc)
    - [Używanie składnika Web Forms formanty powiązane z danymi i wyrażenia wiązania danych](#wfdbc)
- [Praca z bazy danych programu SQL Server](#sqlserver)

    - [Praca z bazy danych programu SQL Server Express LocalDB](#sslocaldb)
    - [Praca z bazy danych programu SQL Server Express](#sse)
    - [Baza danych Azure SQL z systemem Windows](#ssdb)
    - [Wybór między programu SQL Server i bazy danych Azure SQL z systemem Windows](#ssdbchoosing)
- [Praca z systemy zarządzania bazami danych NoSQL](#nosql)
- [W aplikacjach ASP.NET przy użyciu zapytań LINQ](#linq)
- [Przy użyciu funkcji szkieletów danych dynamicznych](#dd)
- [Zabezpieczanie dostępu do danych](#securing)
- [Optymalizowanie wydajności dostępu do danych](#optimizingdataaccess)
- [Wdrożenie bazy danych](#deploying)
- [Uzyskiwanie dostępu do danych za pośrednictwem usługi sieci Web](#webservice)
- [Dodatkowe zasoby](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Wprowadzenie do korzystania z dostępu do danych w programie ASP.NET

- [Opcje magazynu danych (Tworzenie chmury rzeczywistych aplikacji z systemu Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Rozdział książkę elektroniczną o tworzeniu aplikacji w chmurze. Wprowadza bazy danych NoSQL jako alternatywę wielu deweloperów zapoznać się z relacyjnych baz danych często pomijane. Przedstawia wskazówki co zastanowić, wybierając relacyjne lub NoSQL lub wybranie konkretnej platformy.
- [Opcje dostępu do danych w programie ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Wprowadzenie do opcji dostępu do danych relacyjnych baz danych programu ASP.NET oraz wskazówki na temat wybierz platformy i metody, które są odpowiednie dla danego scenariusza dostępu.
- [Relacyjna baza danych](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Jeśli nie zostało to jeszcze pracy z relacyjnych baz danych, zobacz tę stronę, aby obejrzeć wprowadzenie do relacyjnej bazy danych terminy i pojęcia dotyczące. Aby obejrzeć wprowadzenie do programu SQL Server w szczególności zobacz [pracy z bazy danych programu SQL Server](#sqlserver) dalszej części tego tematu.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Przy użyciu programu Entity Framework

- [Entity Framework programowanie podejścia](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Wskazówki na temat wybierania programu Entity Framework podejście do tworzenia pierwszej bazy danych Model First "lub" Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Najpierw przy użyciu Entity Framework Code
  

Następujące samouczki oferują do pobrania przykładowe aplikacje:

- [Wprowadzenie do programów EF 6 przy użyciu MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Obejmuje szeroką gamę Entity Framework Code First scenariuszy, w tym migracji i EF 6 takich funkcji, elastyczności połączenia, polecenie zatrzymania oraz asynchronicznego. To jest zaktualizowana wersja [EF 5 / serii MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Wcześniej seria zawiera samouczek w repozytorium i jednostki pracy wzorców, które nie jest uwzględnione w nowej serii.
- [Wprowadzenie do platformy ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Obejmuje mniejszą niż scenariusze pierwszego zakresu Entity Framework kodu, ale nie bardziej zaawansowane zadania wprowadzenia funkcji MVC.
- [Wiązania modelu i formularzy sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). W aplikacji formularzy sieci Web używa Code First.
- [Wprowadzenie do składnika ASP.NET 4.5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wprowadzenie do formularzy sieci Web z niektórych pokryciem Code First. Używa modelu powiązania.
- [Magazyn utworów muzycznych MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Używa pierwszego kodu w aplikacji MVC 3 handlu elektronicznego, który też implementuje członkostwa i autoryzacji. Wersja MVC i systemu członkostwa programu ASP.NET (uwierzytelniania i autoryzacji) używane w tym miejscu są przestarzałe; Aby uzyskać więcej aktualne informacje o członkostwie w ASP.NET, zobacz [https://asp.net/identity](https://asp.net/identity).

Inne zasoby:

- [Entity Framework - kod najpierw do istniejącej bazy danych](https://msdn.microsoft.com/data/jj200620). MSDN. Wideo i wskazówki, która przedstawia sposób użycia Code First z istniejącej bazy danych.
- [Centrum deweloperów danych - programu Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Przewodnik dotyczący dokumentacji programu Entity Framework, który został utworzony i jest obsługiwany przez zespół Entity Framework, zobacz [wprowadzenie](https://msdn.microsoft.com/data/ee712907) łącza.

Zobacz też [książki o Entity Framework](#efbooks) i [dodatkowe zasoby struktury jednostek](#otherefresources) dalszej części tego tematu.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Przy użyciu migracje Code First Framework jednostki
  

Większość Code First samouczków wymienionych powyżej okładce migracji. Zobacz też następujące zasoby.

- [Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 2-częściowych serii samouczek przedstawia sposób użycia migracje Code First, aby wdrożyć bazę danych.
- [Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL do witryny sieci Web platformy Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Jak używać migracji do wdrażania danych członkostwa i aplikacji na platformie Azure.
- [Omówienie wdrażania w sieci Web dla platformy ASP.NET i Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Zobacz **Konfigurowanie wdrażania bazy danych w programie Visual Studio** sekcji, aby uzyskać informacje o sposobie migracje Code First jest zintegrowany funkcje wdrażania sieci web programu Visual Studio.
- [Centrum deweloperów danych - migracje Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Dokumentacja migracje zespołu programu Entity Framework.
- [Migracje Screencast serii](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF blog). Trzy wideo na tematy Zaawansowane w migracje Code First.
- [Kod pierwszego migracji z lokacjami stron sieci Web ASP.NET](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting blog). Przedstawia sposób użycia migracje Code First z lokacją stron ASP.NET Web Pages przez umieszczenie kontekstu danych w projektach biblioteki klas programu Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Najpierw przy użyciu bazy danych programu Entity Framework lub modelu pierwszej (programie EF Designer)

- [Wprowadzenie do programu Entity Framework 6 Database First przy użyciu MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Uruchom skrypt w Eksploratorze serwera, aby utworzyć bazę danych, a następnie użyj programu Entity Framework designer do tworzenia modelu danych. Pokazuje, jak utworzyć prosty CRUD web pages i innych danych można wykonaj jedną z Code First samouczki od wszystkich EF przepływów pracy za pomocą tego samego interfejsu API DbContext funkcje obsługi.

Starsze są następujące zasoby. Są one przydatne, jeśli chcesz użyć programu Entity Framework w wersji 4.0 i chcesz użyć kontroli źródła danych dla powiązania danych w aplikacji formularzy sieci Web.

- [Wprowadzenie do korzystania z programu Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Przedstawia sposób użycia **obiektu EntityDataSource** formantu.
- [Kontynuowanie z programu Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(przedstawia sposób użycia **ObjectDataSource** formantu. Zawiera samouczek dotyczący Obsługa współbieżności, samouczek dotyczący EF wydajności i samouczek dotyczący what's new in EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Obsługa powiązanych danych w programie Entity Framework (ładowania opóźnionego ładowania wczesny i jawnego ładowania)

- [Odczytywanie danych powiązanych z programu Entity Framework w aplikacji platformy ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Po pierwsze, kodu MVC przykładowej aplikacji. Metody pokazano również zastosowanie do wiązania modelu formularzy sieci Web i bazy danych pierwszy przepływ pracy.
- [Centrum deweloperów danych - ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232) (MSDN). Dane dotyczące dokumentacji ładowania zespołu programu Entity Framework.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optymalizacja wydajności programu Entity Framework

- [Zaawansowanych scenariuszy struktury jednostek dla aplikacji ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Przedstawia sposób wykonania instrukcji SQL lub zadzwoń własnych procedur składowanych, jak wyłączyć wykrywania zmian i wyłączanie sprawdzania poprawności podczas zapisywania zmian.
- [Zagadnienia dotyczące wydajności Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Zagadnienia dotyczące wydajności (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maksymalizacja wydajności przy użyciu programu Entity Framework w aplikacji sieci Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Dotyczy programu Entity Framework 4.0.
- Zobacz też [ASP.NET optymalizacji dostępu do danych](#optimizingdataaccess) dalszej części tego tematu.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Obsługa współbieżności w aplikacji Entity Framework

- [Obsługa współbieżności Entity Framework w aplikacji platformy ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Po pierwsze, kodu interfejsu API DbContext, za pomocą przykładowej aplikacji MVC.
- [Dane w Centrum deweloperów — wzorce optymistycznej współbieżności](https://msdn.microsoft.cus/data/jj592904) (MSDN). Dokumentacja współbieżności zespołu programu Entity Framework.
- [Obsługa współbieżności Entity Framework w aplikacji sieci Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Dotyczy programu Entity Framework 4.0. Bazy danych, interfejsu API ObjectContext, przy użyciu formularzy sieci Web przykładowej aplikacji.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Publikacje na temat programu Entity Framework

- [Programowanie Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman i Tomaszewski Rowan.
- [Programu Entity Framework programowania: "Code First"](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman i Tomaszewski Rowan.

Oba te książki są aktualne z bieżącym zalecane techniki. Udostępniają one bardziej kompleksowe jeszcze łatwe do wykonania wprowadzenie do narzędzia Entity Framework niż niczego, które są dostępne w Internecie. Inną książkę [programowania Entity Framework](http://shop.oreilly.com/product/9780596807252.do) przez Julie Lerman jest większej i bardziej kompleksowe, a jest starsza i wiele metod obejmuje nie są już zalecany sposób użycia programu Entity Framework. Zobacz też listy książek zalecane przez zespół Entity Framework [danych Centrum deweloperów - książki](https://msdn.microsoft.com/data/aa937716) w witrynie MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Inne zasoby struktury jednostek

- [Blog zespołu programu Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Jednym z najlepszych zasobów najbardziej aktualne informacje i anonsów nowe ulepszenia. W przypadku innych związane z platformą EF blogów, zobacz Lista blogów w [Rozpoczynanie pracy z programu Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Zobacz **punktów danych** kolumny, która jest często tematy związane z programu Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Wiązanie danych w sieci Web ASP.NET formularzy aplikacji

- [Opcje dostępu do danych formularzy sieci Web ASP.NET](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Używanie składnika Web Forms wiązania modelu

- [Wiązania modelu i formularzy sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). Samouczek serii za pomocą funkcji EF Code First.
- [Web Forms modelu powiązania część 1: Wybór danych](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog Scott Guthrie). W starsze wpisy na blogu właściwość, która jest obecnie o nazwie ItemType miał nazwę ModelType, ale w przeciwnym razie informacje, które zawierają jest nieprawidłowy.
- [Web Forms modelu powiązania część 2: Filtrowanie danych](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog Scott Guthrie).
- [Web Forms modelu powiązania część 3: Aktualizowanie i weryfikacja](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog Scott Guthrie).
- [Powiązanie modelu formularzy sieci Web 4.5 ASP.NET](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (klip wideo).
- [Model powiązanie, część 1 - wybranie danych](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (klip wideo).
- [Model powiązanie część 2 - filtrowania](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (klip wideo).
- [Pobieranie wprowadzenie do składnika ASP.NET 4.5 Web Forms - wyświetlania danych elementów i szczegóły](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Używanie składnika Web Forms kontrolki źródła danych

- [Kontrolki serwera sieci Web źródła danych](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Anonsowanie wersji dostawcy danych dynamicznych i kontroli obiektu EntityDataSource Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog projektowanie witryn sieci Web firmy Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Używanie składnika Web Forms formanty powiązane z danymi i wyrażenia wiązania danych

- [Wiązania modelu i formularzy sieci Web](https://go.microsoft.com/fwlink/?LinkId=286117). Samouczek serii, która używa EF Code First.
- [Pobieranie wprowadzenie do składnika ASP.NET 4.5 Web Forms - wyświetlania danych elementów i szczegóły](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Silnie Typizowanej formantów danych](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog Scott Guthrie).
- [Silnie Typizowanej formantów danych](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (klip wideo).
- [Program ASP.NET 4.5 wpisane formantów danych sieci Web Forms silne](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (klip wideo).
- [Formanty serwera sieci Web powiązanych z danymi](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Omówienie wyrażenia wiązania danych](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). To strony tylko obejmuje **Eval** i **powiązać**; nie został jeszcze zaktualizowany do uwzględnienia **elementu** i **metody BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Praca z bazy danych programu SQL Server

- [Funkcje bazy danych programu SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Aby uzyskać ogólne wprowadzenie do różnych tematów programu SQL Server zobacz wpisy w tym typie w spisie treści.
- [Wersje programu SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Podsumowanie dostępne wersje programu SQL Server, z łącza do dodatkowych informacji o każdym z nich.)
- [Parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Dla aplikacji sieci Web ASP.NET przy użyciu programu SQL Server Compact](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Programu Microsoft SQL Server: Przykłady produktu bazy danych](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Przykładowe bazy danych AdventureWorks.
- [Instalowanie przykładowej bazy danych](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Oprócz metod pokazane, możesz również pobrać jednego z przykładowych plików .mdf do aplikacji\_danych folderu projektu sieci web, przekonwertować bazy danych LocalDB, a następnie utworzyć parametry połączenia bazy danych LocalDB. Aby dowiedzieć się, jak to zrobić, zobacz [porady: uaktualnienie do LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Zobacz także następujące sekcje w pracy z programu SQL Server Express i LocalDB, a następnie wybierając między programu SQL Server i bazy danych SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Praca z bazy danych programu SQL Server Express LocalDB

- [Program SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Oficjalna MSDN wprowadzenie do LocalDB.
- [Parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Porady: uaktualnienie do LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Jak przeprowadzić migrację plików .mdf ze starszej wersji programu SQL Server Express do LocalDB. Masz również przechodzić przez ten proces, jeśli pobranie jednej z [bazy danych programu SQL Server 2012 przykładowej](https://go.microsoft.com/fwlink/?linkid=117483).
- [Wprowadzenie do LocalDB ulepszone SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog programu SQL Server Express). Ma więcej tło na Dlaczego LocalDB został utworzony, nie znajduje się w witrynie MSDN.
- [LocalDB: Gdzie znajduje się Moje bazy danych?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog programu SQL Server Express). Informacje o którym zostaną utworzone pliki bazy danych LocalDB.
- [Przy użyciu LocalDB z usługi IIS, część 1: profil użytkownika](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog programu SQL Server Express). LocalDB nie jest przeznaczona do pracy z usługami IIS. Ta seria blogach opisano problemy i rozwiązania niektórych.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Praca z bazy danych programu SQL Server Express

- [Parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Jeśli ustawienie parametrów połączenia AttachDBFileName użytkownik korzysta z programu SQL Server Express, zobacz zwłaszcza sekcję wystąpienia użytkownika tej strony.
- [Jak przejąć na własność użytkownika lokalnego programu SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog programu SQL Server Express). To powszechny problem nie jest objęty pracować z programu SQL Server Express baz danych, ponieważ nie jesteś administratorem w wystąpieniu programu SQL Server Express. Domyślnie osoba, który zainstalował program SQL Server Express jest administratorem. Ten blog wyjaśniono, jak z siebie uczynić użytkownika administratora programu SQL Server Express, jeśli masz uprawnienia administratora na komputerze.
- [Moja aplikacja sieci web platformy ASP.NET można użyć bazy danych programu SQL Server Express w środowisku produkcyjnym?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Baza danych Azure SQL z systemem Windows

- [Wdrażanie aplikacji platformy ASP.NET MVC Secure z członkostwa, OAuth i bazy danych SQL do witryny sieci Web systemu Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (witryny Microsoft Azure).
- [Bazy danych SQL](https://docs.microsoft.com/azure/sql-database/) (witryny Microsoft Azure). Pobieranie porad przewodniki i samouczki wprowadzenie.
- [Baza danych Azure SQL z systemem Windows](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Węzeł najwyższego poziomu spisu treści dla bazy danych SQL w witrynie MSDN.
- [Indeks artykuły Wiki TechNet bazy danych systemu Windows Azure SQL](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (w witrynie Microsoft TechNet).
- [Obsługa błędu przejściowego bloku aplikacji](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Platforma, która umożliwia obsługę błędów sieci i błędów dotyczących połączeń wynikających z ograniczenia przepustowości. Dostępne w pakiecie NuGet: [Enterprise biblioteki 5.0 - bloku aplikacji obsługi błędów przejściowych](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Wprowadzenie do korzystania z bazy danych SQL i programu Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Zestaw szkoleniowy Azure Windows](https://www.microsoft.com/download/details.aspx?id=8396) (Centrum pobierania Microsoft). Obejmuje praktyczne labs bazy danych SQL.
- [Forum społeczności bazy danych Azure SQL dla systemu Windows](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Przenoszenie do systemu Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Rozdziału jeden kompleksowe scenariusza na trasie przez zespół Microsoft Patterns and Practices. Dlaczego warto migracji obejmuje i przeprowadzanie migracji z programu SQL Server do bazy danych SQL.
- [Migrowanie bazy danych programu SQL Server do systemu Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Kreator migracji bazy danych SQL](http://sqlazuremw.codeplex.com/). Narzędzie typu open source migracji baz danych do i z bazy danych SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Wybór między programu SQL Server i bazy danych Azure SQL z systemem Windows

- [Porównanie programu SQL Server w usłudze Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (w witrynie Microsoft TechNet).
- [Migracji danych do systemu Windows Azure SQL Database: narzędzi i technik](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Zawiera sekcje porównywania programu SQL Server z bazą danych SQL i zapewnianie wskazówek w przypadku migracji z programu SQL Server z bazą danych SQL.
- [Podręcznik dostarczania bazy danych SQL Azure z systemem Windows](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (w witrynie Microsoft TechNet).
- [Ograniczenia funkcji serwera SQL (baza danych Azure SQL z systemem Windows)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage i baza danych Azure SQL z systemem Windows — porównywane i odróżniające](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Aplikacji wdrażanej w systemie Windows Azure magazyn tabel Azure systemu Windows może być alternatywę dla bazy danych SQL Azure z systemem Windows. Ten temat ułatwia podjęcie decyzji o następujących alternatyw.
- [Baza danych Azure SQL z systemem Windows](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Wskazówki i ograniczenia (baza danych Azure SQL z systemem Windows)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Praca z systemy zarządzania bazami danych NoSQL

- [Usługi danych systemu Windows Azure](https://www.windowsazure.com/develop/net/data/) (witryny Microsoft Azure). Zobacz [Przewodnik po funkcji usługi tabel](https://docs.microsoft.com/azure/) i **danych Big Data** części strony.
- [Przy użyciu tabel magazynu ASP.NET wielowarstwowych aplikacji, kolejek i obiektów blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (witryny Microsoft Azure). Samouczek na trasie o do pobrania Przykładowa aplikacja korzystająca z tabel NoSQL magazynu systemu Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>W aplikacjach ASP.NET przy użyciu zapytań LINQ

- [Opcje dostępu do danych w programie ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Zawiera wprowadzenie do LINQ.
- [Filmy szkoleniowe LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog Jan Stagner).
- [Wątek Forum platformy ASP.NET z łączami do dynamicznego zasoby LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Przy użyciu funkcji szkieletów danych dynamicznych

- [Szablony projektu z danymi dynamicznymi](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Wskazówki dotyczące użycie danych dynamicznych projektów.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Zabezpieczanie dostępu do danych

- [Zabezpieczanie dostępu do danych w programie ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Zagadnienia dotyczące zabezpieczeń (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Porady: Secure parametry połączenia, korzystając z kontrolki źródła danych](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optymalizowanie wydajności dostępu do danych

- [Omówienie wydajności programu ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET buforowanie](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Poprawianie wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Znajduje się ostrzeżenie "Wycofane zawartość" w górnej części strony, ale większość informacji jest nadal ważna i nie ma nie można porównywać pod względem zaktualizowanego zasobu.
- [Poprawianie wydajności serwera SQL](https://msdn.microsoft.com/library/ff647793) (MSDN). Tym samym komentarz jako poprzedniej konsolidacji.

Zobacz też [programu Entity Framework optymalizacji wydajności](#optimizingef) wcześniej w tym temacie.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Wdrożenie bazy danych

- [Wdrażanie sieci Web platformy ASP.NET — zalecane zasobów](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Uzyskiwanie dostępu do danych za pośrednictwem usługi sieci Web

- [Uzyskiwanie dostępu do danych za pośrednictwem usługi sieci Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Wskazówki dotyczące użycie interfejsu API sieci Web lub usługi WCF.
- [Wprowadzenie do korzystania z interfejsu API sieci Web ASP.NET](../web-api/index.md).
- [Usługi danych WCF](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Dostęp do danych programu ASP.NET — często zadawane pytania](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Samouczki - danych formularzy sieci Web ASP.NET](../web-forms/overview/data-access/index.md). Większość tego samouczka jest stosunkowo starego; Upewnij się, że przeczytanie [opcje dostępu do danych programu ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) i [opcje magazynu danych (kompilowanie praktyczne aplikacje w chmurze z systemu Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) pierwszy tak, aby nie pobieraj zbyt daleko do metody dostępu, która nie jest prawo dla danego scenariusza.
- [Mapa zawartości platformy ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages samouczki - danych](../web-pages/overview/data/index.md).
- [Uzyskiwanie dostępu do danych w programie Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Lista łączy podobną do tej zawartości mapy, ale z fokus w Visual Studio, a nie ASP.NET.
