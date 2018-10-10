---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC (1, 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Nowsza wersja tej serii samouczków jest dostępne dla programu Visual Studio 2013, platformy Entity Framework 6 i MVC 5. De aplikacji sieci web przykładowej firmy Contoso University...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b691f718258f98e03513a089ca26b286f284765e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913238"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC (1, 10)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A [nowszą wersję tej serii samouczków](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) dostępnej dla programu Visual Studio 2013, platformy Entity Framework 6 i MVC 5.
> 
> 
> Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje platformy ASP.NET MVC 4, za pomocą programu Entity Framework 5 i Visual Studio 2012. Przykładowa aplikacja jest witryną sieci web dla uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora. Tej serii samouczków opisano sposób tworzenia przykładowej aplikacji Contoso University. Możesz [pobrać gotową aplikację](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Najpierw kod
> 
> Istnieją trzy sposoby pracy z danymi w programie Entity Framework: *Database First*, *pierwszy Model*, i *Code First*. Ten samouczek dotyczy Code First. Aby uzyskać informacje o różnicach między tymi przepływami pracy a wskazówki o tym, jak wybrać najlepszy dla danego scenariusza, zobacz [przepływów pracy programu Entity Framework programowania](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Przykładowa aplikacja jest oparta na [platformy ASP.NET MVC](../../../index.md). Jeśli wolisz pracować z modelu formularzy sieci Web platformy ASP.NET, zobacz [wiązania modelu i formularzy sieci Web](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) serii samouczków i [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Pokazane w tym samouczku** | **Współpracuje również z** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio Express 2012 for Web. Jest to automatycznie instalowany przez zestaw Windows Azure SDK, jeśli nie masz jeszcze programu VS 2012 lub VS 2012 Express for Web. Visual Studio 2013 powinna działać, ale nie przetestowano samouczka z nim, a niektóre opcje menu i okien dialogowych różnią się. [VS 2013 wersję zestawu Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) jest wymagany do wdrażania usługi Windows Azure. |
> | .NET 4.5 | Większość funkcji wyświetlane będą działać w .NET 4, ale niektóre nie. Na przykład wyliczenia dział pomocy technicznej w programie EF wymaga platformy .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Aby pominąć kroki wdrażania platformy Windows Azure, nie trzeba zestawu SDK. Po udostępnieniu nowej wersji zestawu SDK, link będzie Zainstaluj nowszą wersję. W takiej sytuacji może być konieczne dostosowanie niektórych z instrukcjami, aby nowy interfejs użytkownika i funkcje. |
> 
> ## <a name="questions"></a>Pytania
> 
> Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework i LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), lub [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Potwierdzenia
> 
> Zobacz to ostatni samouczek z serii dotyczącej [potwierdzenia i Uwaga dotycząca języka VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Oryginalna wersja samouczka
> 
> Oryginalna wersja tego samouczka jest dostępna w [EF 4.1 / MVC 3-e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Aplikacja sieci Web firmy Contoso University

Aplikacja, której można tworzyć w tych samouczkach znajduje university prostą witrynę sieci web.

Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów. Oto kilka ekranów, które zostaną utworzone.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Styl interfejsu użytkownika w tej lokacji została zachowana blisko co to jest generowany przez wbudowane szablony, dzięki czemu samouczka można skupić się głównie na temat korzystania z programu Entity Framework.

## <a name="prerequisites"></a>Wymagania wstępne

Wskazówki i zrzuty ekranu, w tym samouczku przyjęto założenie, że używasz [programu Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) lub [Visual Studio 2012 Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131), przy użyciu najnowszych aktualizacji i zestawu Azure SDK dla programu .NET zainstalowane począwszy od lipca, 2013. Otrzymasz wszystko to za pomocą następującego linku:

[Zestaw Azure SDK dla platformy .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Jeśli masz zainstalowany program Visual Studio, powyższy link zainstaluje brakujące składniki. Jeśli nie masz programu Visual Studio, łącze zainstaluje program Visual Studio 2012 Express for Web. Można użyć programu Visual Studio 2013, ale niektóre z wymaganych procedur i ekrany będą się różnić.

## <a name="create-an-mvc-web-application"></a>Tworzenie aplikacji sieci Web MVC

Otwórz program Visual Studio i Utwórz nowy projekt C# o nazwie "ContosoUniversity" przy użyciu **aplikacji sieci Web programu ASP.NET MVC 4** szablonu. Upewnij się, docelowych **.NET Framework 4.5** (należy używać [ `enum` właściwości](https://msdn.microsoft.com/data/hh859576.aspx), i która wymaga programu .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

W **nowego projektu programu ASP.NET MVC 4** wybierz okno dialogowe **aplikacji internetowej** szablonu.

Pozostaw **Razor** wyświetlić wybrany aparat i pozostawić **Tworzenie projektu testu jednostkowego** pole wyboru jest wyczyszczone.

Kliknij przycisk **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka prostych zmian zostanie skonfigurowany w menu witryny, układu i strony głównej.

Otwórz *Views\Shared\\_Layout.cshtml*i Zastąp zawartość pliku następującym kodem. Zmiany są wyróżnione.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Ten kod wprowadza następujące zmiany:

- Zamienia wystąpień szablonu "Moja aplikacja MVC platformy ASP.NET" i "Twoje logo" "Uniwersytet firmy Contoso".
- Dodaje kilka łącza akcji, które będą używane w dalszej części tego samouczka.

W *Views\Home\Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby wyeliminować akapitów szablonu o platformie ASP.NET i MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

W *Controllers\HomeController.cs*, zmień wartość `ViewBag.Message` w `Index` metody akcji, aby "Witaj na Contoso University!", jak pokazano w poniższym przykładzie:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Naciśnij klawisze CTRL + F5, aby uruchomić witryny. Zostanie wyświetlona strona główna z menu głównego.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klas jednostek dla aplikacji Contoso University. Będzie rozpoczynać trzech następujących elementach:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Istnieje relacja jeden do wielu między `Student` i `Enrollment` jednostek i relacji jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy uczniem/uczennicą mogą być rejestrowane w dowolnej liczbie kursów i kursu mogą mieć dowolną liczbę uczniów zarejestrowane w nim.

W poniższych sekcjach utworzysz klasy dla każdego z tych jednostek.

> [!NOTE]
> Jeśli zostanie podjęta próba skompilowania projektu, przed zakończeniem, tworzenie wszystkich tych klas jednostek, uzyskasz błędy kompilatora.


### <a name="the-student-entity"></a>Jednostki dla uczniów

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

W *modeli* folderze utwórz *Student.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` Właściwości staną się kolumna klucza podstawowego w tabeli bazy danych, która odnosi się do tej klasy. Domyślnie platforma Entity Framework interpretuje właściwość o nazwie `ID` lub *classname* `ID` jako klucz podstawowy.

`Enrollments` Właściwość *właściwość nawigacji*. Właściwości nawigacji przechowywania innych jednostek, które są powiązane z tej jednostki. W tym przypadku `Enrollments` właściwość `Student` jednostki będą przechowywane wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student` jednostki. Innymi słowy Jeśli danego `Student` wierszy w bazie danych ma dwa powiązane `Enrollment` wierszy (wiersze, które zawierają klucz podstawowy tego uczniów wartość w ich `StudentID` kolumna klucza obcego), które `Student` jednostki `Enrollments` właściwość nawigacji będzie zawierać tych dwóch `Enrollment` jednostek.

Właściwości nawigacji są zazwyczaj definiowane jako `virtual` tak, aby można było korzystać z niektórych funkcji programu Entity Framework takich jak *powolne ładowanie*. (Ładowanie z opóźnieniem zostaną wyjaśnione w dalszej części [odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczków w dalszej części tej serii.

Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w relacji wiele do wielu lub jeden do wielu), jego typ musi być listy, w którym wpisy mogą być dodawane, usuwane lub zaktualizowane, takich jak `ICollection`.

### <a name="the-enrollment-entity"></a>Jednostki rejestracji

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

W *modeli* folderze utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Właściwość klasy jest [wyliczenia](https://msdn.microsoft.com/data/hh859576.aspx). Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość jest [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx). Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, dzięki czemu właściwość może zawierać tylko jeden `Student` jednostki (w przeciwieństwie do `Student.Enrollments` właściwość nawigacji był wyświetlany poprzednio, która zawiera wiele `Enrollment` jednostek).

`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

### <a name="the-course-entity"></a>Jednostki kursu

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

W *modeli* folderze utwórz *Course.cs*, zastępując istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.

Przycisk więcej na temat [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([element DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Brak)] atrybutu w następnym samouczku. Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla kurs zamiast bazy danych, do jego wygenerowania.

## <a name="create-the-database-context"></a>Utwórz kontekst bazy danych

Główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework jest *kontekst bazy danych* klasy. Tworzenie tej klasy, wynikające z [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) klasy. W kodzie należy określić, które jednostki są uwzględnione w modelu danych. Można również dostosować określone zachowanie programu Entity Framework. W tym projekcie nosi nazwę klasy `SchoolContext`.

Utwórz folder o nazwie *DAL* (w przypadku warstwy dostępu do danych). W tym folderze utwórz nowy plik klasy o nazwie *SchoolContext.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Ten kod tworzy [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) właściwości dla każdego zestawu jednostek. W terminologii programu Entity Framework *zestaw jednostek* zazwyczaj odnosi się do tabeli bazy danych i *jednostki* odnosi się do wiersza w tabeli.

`modelBuilder.Conventions.Remove` Instrukcji w [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda zapobiega on pluralized nazwy tabel. Jeśli nie możesz tego zrobić, wygenerowany tabel będą miały postać `Students`, `Courses`, i `Enrollments`. Zamiast tego nazwy tabel będą `Student`, `Course`, i `Enrollment`. Deweloperzy nie zgadzają się na temat tego, czy nazwy tabel należy pluralized czy nie. Ten samouczek używa pojedynczej formularza, ale istotną kwestią jest to, którą można wybrać niezależnie od tego formularza preferowanych przez uwzględnienie lub pominięcie ten wiersz kodu.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) to Uproszczona wersja aparatu programu SQL Server Express bazy danych rozpoczyna się na żądanie, która działa w trybie użytkownika. LocalDB działa w specjalnego trybu wykonania programu SQL Server Express, która umożliwia pracę z bazami danych jako *.mdf* plików. Zazwyczaj pliki bazy danych LocalDB są przechowywane w *aplikacji\_danych* folderu projektu sieci web. Funkcja wystąpienia użytkownika programu SQL Server Express umożliwia także pracować z *.mdf* pliki, ale funkcja wystąpienia użytkownika jest przestarzała; dlatego LocalDB jest zalecane w przypadku pracy z *.mdf* plików.

Zazwyczaj programu SQL Server Express nie jest używana dla aplikacji sieci web w środowisku produkcyjnym. LocalDB w szczególności nie jest zalecane do użytku produkcyjnego z aplikacją sieci web, ponieważ nie jest przeznaczony do pracy z usługami IIS.

W programie Visual Studio 2012 i nowszych wersjach LocalDB jest instalowany domyślnie z programem Visual Studio. W Visual Studio 2010 i starszych, SQL Server Express (bez LocalDB) jest instalowany domyślnie z programem Visual Studio; Musisz zainstalować go ręcznie, jeśli używasz programu Visual Studio 2010.

W tym samouczku będziesz pracować z bazą danych LocalDB, aby baza danych może być przechowywana w *aplikacji\_danych* folder jako *.mdf* pliku. Otwórz katalog główny *Web.config* pliku i Dodaj nowe parametry połączenia do `connectionStrings` kolekcji, jak pokazano w poniższym przykładzie. (Pamiętaj o zaktualizowaniu *Web.config* pliku w folderze głównym projektu. Istnieje również *Web.config* plik znajduje się w *widoków* podfolder, który nie jest potrzebny do zaktualizowania.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Domyślnie platforma Entity Framework szuka parametrów połączenia o nazwie taka sama jak `DbContext` klasy (`SchoolContext` dla tego projektu). Parametry połączenia zostały dodane Określa nazwę bazy danych LocalDB *ContosoUniversity.mdf* na terenie *aplikacji\_danych* folderu. Aby uzyskać więcej informacji, zobacz [parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Nie jest potrzebna określić parametry połączenia. Jeśli nie zostanie podane parametry połączenia, platformy Entity Framework utworzy go dla Ciebie; jednak baza danych nie może być w *aplikacji\_danych* folderu aplikacji. Aby uzyskać informacje, na którym zostanie utworzona baza danych, zobacz [Code First dla nowej bazy danych](https://msdn.microsoft.com/data/jj193542).

`connectionStrings` Kolekcji ma również parametry połączenia o nazwie `DefaultConnection` wykorzystywane są w bazie danych członkostwa. Członkowskiej bazie danych nie będzie używany w ramach tego samouczka. Jedyną różnicą między dwa parametry połączenia jest nazwa bazy danych i wartością atrybutu nazwy.

## <a name="set-up-and-execute-a-code-first-migration"></a>Konfigurowanie i wykonywanie migracji Code First

Przy pierwszym uruchomieniu opracowywania aplikacji, model danych, zmiany często i za każdym razem zmiany modelu, który otrzymuje zsynchronizowana z bazą danych. Można skonfigurować programu Entity Framework, aby automatycznie porzucenia i ponownego utworzenia bazy danych po każdej zmianie modelu danych. To nie jest problemem na wczesnym etapie projektowania, ponieważ dane testowe jest łatwo ponownie utworzyć, ale po wdrożeniu w środowisku produkcyjnym zazwyczaj chcesz zaktualizować schemat bazy danych bez usuwania bazy danych. Funkcja migracji umożliwia Code First aktualizacji bazy danych bez porzucenie i ponowne utworzenie go. Na wczesnym etapie cyklu tworzenia nowego projektu możesz chcieć użyć [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) porzucić i ponownie utwórz ponowne inicjowanie bazy danych zawsze zmiany modelu. Jeden użytkownik przygotowania do wdrożenia aplikacji, można przekonwertować na podejście migracji. W tym samouczku będą używane wyłącznie migracji. Aby uzyskać więcej informacji, zobacz [migracje Code First](https://msdn.microsoft.com/data/jj591621) i [serii zrzut ekranu migracje](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Włącz migracje Code First

1. Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet** i następnie **Konsola Menedżera pakietów**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. W `PM>` wierszu polecenia wprowadź następujące polecenie:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![polecenia enable-migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    To polecenie umożliwia utworzenie *migracje* folderu w projekcie ContosoUniversity i umieszcza w tym folderze *Configuration.cs* pliku, który można edytować w celu konfigurowania migracji.

    ![Folder migracji](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration` Klasa zawiera `Seed` metodę, która jest wywoływana po utworzeniu bazy danych i za każdym razem, gdy jest aktualizowany na zmiany modelu danych.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Celem tego `Seed` metoda polega na umożliwiają wstawianie danych testowych bazy danych po go utworzy lub zaktualizuje Code First.

### <a name="set-up-the-seed-method"></a>Konfigurowanie Seed — metoda

[Inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda jest uruchamiana, gdy migracje Code First tworzy bazę danych i za każdym razem, gdy powoduje zaktualizowanie najnowsze migracji bazy danych. Celem Seed — metoda jest, aby umożliwić wstawianie danych w tabelach przed zastosowaniem uzyskuje dostęp do bazy danych po raz pierwszy.

We wcześniejszych wersjach Code First, zanim został wydany migracje, było często `Seed` metody, aby wstawić dane z badań, ponieważ przy każdej zmianie modelu podczas tworzenia bazy danych musiały być całkowicie usunięta i utworzona ponownie od podstaw. Migracje Code First, test, dane są zachowywane po wprowadzeniu zmian w bazie danych, więc łącznie danych testowych w [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) zazwyczaj metoda nie jest konieczne. W rzeczywistości nie chcesz `Seed` metodę, aby wstawić dane testowe, jeśli będziesz korzystać z migracji do wdrażania bazy danych w środowisku produkcyjnym, ponieważ `Seed` metoda jest uruchamiana w środowisku produkcyjnym. W takim przypadku chcesz `Seed` metody do wstawienia do bazy danych, który ma zostać wstawione w środowisku produkcyjnym. Na przykład można znaleźć nazwy rzeczywistych działów w w bazie danych `Department` tabeli, gdy aplikacja staje się dostępna w środowisku produkcyjnym.

W tym samouczku będziesz korzystać z migracji do wdrożenia, Twoja `Seed` metody powoduje wstawienie danych testowych mimo to ułatwi można zobaczyć, jak działa funkcji aplikacji bez konieczności ręcznego Wstaw dużej ilości danych.

1. Zastąp zawartość *Configuration.cs* pliku następującym kodem, który załaduje dane z badań do nowej bazy danych. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [Inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda przyjmuje obiekt kontekstu bazy danych jako parametr wejściowy, a kod w metodzie używa tego obiektu, aby dodać nowe jednostki w bazie danych. Dla każdego typu jednostki, ten kod tworzy kolekcję nowe jednostki, dodaje je do odpowiednich [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) właściwości, a następnie zapisuje zmiany w bazie danych. Nie trzeba wywoływać [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody po każdej grupie jednostek, tak jak to zrobić w tym miejscu, ale sposób, który pomoże Ci Znajdź źródło problemu, jeśli wystąpi wyjątek podczas pisania kodu w bazie danych.

    Niektóre instrukcje, które wstawiają dane wykorzystują [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody, które można wykonać operacji "upsert". Ponieważ `Seed` metoda uruchamia się za pomocą każdej migracji, po prostu nie można wstawić danych, ponieważ wierszy próbujesz dodać będą już dostępne po pierwszej migracji, który tworzy bazę danych. Operacja "upsert" uniemożliwia błędów, które może się zdarzyć, jeśli podczas próby wstawienia wiersza, który już istnieje, ale ***zastępuje*** wszelkie zmiany w danych, które mogły zostać wprowadzone podczas testowania aplikacji. Zawierającej dane testowe w niektórych tabel nie można było to możliwe: w niektórych przypadkach po zmianie danych podczas testowania mają zmiany po aktualizacji bazy danych. W takim przypadku chcesz zrobić to operacja wstawiania warunkowego: Wstaw wiersz, tylko wtedy, gdy jeszcze nie istnieje. Seed — metoda korzysta z obu metod.

    Pierwszy parametr przekazany do [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody Określa właściwość, można użyć, aby sprawdzić, czy wiersz już istnieje. Dla danych student testu, które są podawane `LastName` właściwość może być używana w tym celu, ponieważ każdy nazwisko na liście jest unikatowa:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Ten kod zakłada, że ostatni nazwy są unikatowe. Jeśli ręcznie dodać uczniów o zduplikowanej nazwie ostatniego, otrzymasz następujący wyjątek podczas następnego przeprowadzić migrację.

    Sekwencja zawiera więcej niż jeden element

    Aby uzyskać więcej informacji na temat `AddOrUpdate` metody, zobacz [powinien zachować ostrożność przy użyciu metody AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

    Kod, który dodaje `Enrollment` nie korzysta z jednostek `AddOrUpdate` metody. Sprawdza, czy jednostka już istnieje i wstawi jednostkę, jeśli nie istnieje. To podejście zachowa zmiany wprowadzone w klasie rejestracji po uruchomieniu migracji. Kod w pętli każdy element członkowski `Enrollment` [listy](https://msdn.microsoft.com/library/6sh2ey19.aspx) i jeśli rejestracja nie zostanie znaleziony w bazie danych, dodaje rejestracji z bazą danych. Po raz pierwszy można zaktualizować bazy danych, bazy danych jest pusta, więc spowoduje to dodanie każdej rejestracji.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Aby uzyskać informacje dotyczące debugowania `Seed` metody i sposób obsługi nadmiarowych danych, takich jak dwóm studentom/uczniom o nazwie "Alexander Carson", zobacz [wstępnego wypełniania i baz danych debugowania Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Ricka Andersona.
2. Skompiluj projekt.

### <a name="create-and-execute-the-first-migration"></a>Tworzenie i wykonywanie pierwszej migracji

1. W oknie Konsola Menedżera pakietów wprowadź następujące polecenia: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Polecenie dodaje do folderu migracje *[DateStamp]\_InitialCreate.cs* pliku, który zawiera kod, który tworzy bazę danych. Pierwszy parametr (`InitialCreate)` służy do pliku nazwy i może być dowolnie; zazwyczaj wybierz wyraz lub frazę, który podsumowuje, co to jest wykonywana w procesie migracji. Na przykład można nazwać późniejszej migracji &quot;AddDepartmentTable&quot;.

    ![Folder migracje o początkowej migracji](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawy jednostek modelu danych i `Down` metoda usuwa je. Migracje wywołania `Up` metodę, aby wdrożyć model danych zmienia się do migracji. Po wprowadzeniu polecenia można wycofać aktualizację, wywołania migracje `Down` metody. Poniższy kod przedstawia zawartość `InitialCreate` pliku:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` Polecenia `Up` uruchamia metodę w celu utworzenia bazy danych, a następnie go `Seed` metodę do wypełniania bazy danych.

Utworzono bazę danych programu SQL Server dla modelu danych. Nazwa bazy danych jest *ContosoUniversity*i *.mdf* plik jest w twoim projekcie *aplikacji\_danych* folderu ponieważ określony w Twojej Parametry połączenia.

Można użyć dowolnego **Eksploratora serwera** lub **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazy danych w programie Visual Studio. W tym samouczku użyjesz **Eksploratora serwera**. W programie Visual Studio Express 2012 for Web **Eksploratora serwera** nosi nazwę **Eksplorator bazy danych**.

1. Z **widoku** menu, kliknij przycisk **Eksploratora serwera**.
2. Kliknij przycisk **Dodaj połączenie** ikony.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Jeśli zostanie wyświetlony monit o **wybierz źródło danych** okno dialogowe, kliknij przycisk **programu Microsoft SQL Server**, a następnie kliknij przycisk **Kontynuuj**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. W **Dodaj połączenie** okna dialogowego wprowadź **(localdb) \v11.0** dla **nazwy serwera**. W obszarze **wybierz lub wprowadź nazwę bazy danych**, wybierz opcję **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Kliknij przycisk **OK**.
6. Rozwiń **SchoolContext** a następnie rozwiń węzeł **tabel**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **Pokaż dane tabeli** kolumn, które zostały utworzone i wierszy, które zostały wstawione do tabeli.

    ![Tabela student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Tworzenie kontrolera dla uczniów i widoków

Następnym krokiem jest do tworzenia widoków i kontrolerów platformy ASP.NET MVC w aplikacji, który może współpracować z jednej z tych tabel.

1. Aby utworzyć `Student` kontrolera, kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań**, wybierz opcję **Dodaj**, a następnie kliknij przycisk **kontrolera** . W **Dodaj kontroler** okna dialogowego pole, wybierz następujące opcje, a następnie kliknij przycisk **Dodaj**: 

   - Nazwa kontrolera: **StudentController**.
   - Szablon: **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework**.
   - Klasa modelu: **uczniów (ContosoUniversity.Models)**. (Jeśli nie widzisz tej opcji na liście rozwijanej, skompiluj projekt i spróbuj ponownie.)
   - Klasa kontekstu danych: **SchoolContext (ContosoUniversity.Models)**.
   - Widoki: **Razor (CSHTML)**. (Ustawienie domyślne).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Zostanie otwarty program Visual Studio *Controllers\StudentController.cs* pliku. Zobaczysz, że zmienna klasa została utworzona, tworzy wystąpienie obiektu kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index` Metody akcji pobiera listę uczniów z *studentów* zestaw, czytając jednostek `Students` właściwości wystąpienia kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml* widoku tej liście są wyświetlane w tabeli:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Naciśnij klawisze CTRL + F5, aby uruchomić projekt.

     Kliknij przycisk **studentów** kartę, aby wyświetlić dane z badań, `Seed` metoda wstawiony.

     ![Strona indeksu dla uczniów](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Konwencje

Ilość kodu, trzeba było pisać w kolejności programu Entity Framework można było utworzyć pełną bazę danych dla Ciebie jest minimalny, z powodu użycia *konwencje*, lub założenia, które czynią Entity Framework. Niektóre z nich zostały już zostały zanotowane:

- Pluralized formy nazwy klas jednostek są używane jako nazwy tabeli.
- Nazwy właściwości jednostki są używane dla nazw kolumn.
- Właściwości jednostki, które są nazwane `ID` lub *classname* `ID` są rozpoznawane jako właściwości klucza podstawowego.

W tym samouczku można zastąpić konwencje (na przykład określono, nazwy tabel nie powinny być pluralized), a dowiesz się więcej na temat Konwencji i jak zastąpić je w [tworzenie więcej złożonego modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) samouczek w dalszej części tej serii. Aby uzyskać więcej informacji, zobacz [pierwszy konwencje związane z](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Podsumowanie

Utworzono prostą aplikację, która korzysta z programu Entity Framework i programu SQL Server Express do przechowywania i wyświetlania danych. W tym samouczku poniższy dowiesz się, jak wykonać podstawowe CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) operacji. Możesz pozostawić opinię w dolnej części tej strony. Daj nam znać sposób zbędne tej części samouczka i jak można było ulepszyć proces jej.

Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
