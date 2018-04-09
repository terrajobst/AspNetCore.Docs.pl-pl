---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Tworzenie modelu danych struktury jednostek dla aplikacji platformy ASP.NET MVC (od 1 do 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Dostępne dla programu Visual Studio 2013, Entity Framework 6 i MVC 5 jest nowsza wersja tego samouczka serii. Niemcy aplikacji sieci web przykładowej Contoso University...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a963f26b408f2a54bd9cd3e852bc1e368f86c41f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Tworzenie modelu danych struktury jednostek dla aplikacji platformy ASP.NET MVC (od 1 do 10)
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A [nowsza wersja tego samouczka serii](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) jest dostępna dla programu Visual Studio 2013, Entity Framework 6 i MVC 5.
> 
> 
> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu programu Entity Framework 5 i Visual Studio 2012. Przykładowa aplikacja jest witryną sieci web dla fikcyjnej uniwersytetu Contoso. Obejmuje funkcje, takie jak wprowadzenia studentów, tworzenie kursu i instruktora przypisania. Ten samouczek serii wyjaśniono sposób zastosowania Contoso University przykładowej aplikacji. Możesz [Pobierz ukończona aplikacja](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Najpierw kodu
> 
> Istnieją trzy sposoby pracy z danymi programu Entity Framework: *Database First*, *Model First*, i *Code First*. Ten samouczek jest przeznaczony dla Code First. Aby uzyskać informacji o różnicach między te przepływy pracy i wskazówki na temat wybierania najlepszy dla danego scenariusza, zobacz [przepływów pracy programu Entity Framework programowanie](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Przykładowa aplikacja powstała w oparciu o [ASP.NET MVC](../../../index.md). Jeśli wolisz pracować z modelu formularzy sieci Web ASP.NET, zobacz [powiązanie modelu i formularzy sieci Web](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) samouczka serii i [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Wyświetlany w samouczku** | **Współdziała również z** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio Express 2012 for Web. Jest to automatycznie instalowany przez zestaw SDK systemu Windows Azure, jeśli nie masz jeszcze VS 2012 i VS 2012 Express for Web. Programu Visual Studio 2013 powinny działać, ale samouczek nie był testowany z nim, a niektóre opcje menu i okien dialogowych różnią się. [VS 2013 wersję zestawu Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) jest wymagane do wdrożenia systemu Windows Azure. |
> | .NET 4.5 | Większość funkcji pokazywane będą działać w .NET 4, ale niektóre nie. Na przykład obsługa wyliczenia w EF wymaga platformy .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Aby pominąć kroki wdrażania systemu Windows Azure, nie potrzebujesz zestawu SDK. Po wydaniu nowej wersji zestawu SDK, link będzie Zainstaluj nowszą wersję. W takiej sytuacji może być konieczne dostosowanie niektórych z instrukcjami, aby nowy interfejs użytkownika i funkcje. |
> 
> ## <a name="questions"></a>Pytania
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework i składnika LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), lub [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Potwierdzenia
> 
> Zobacz samouczek ostatniego w serii [potwierdzenia i notatkę dotyczącą VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Oryginalnej wersji samouczka
> 
> Jest dostępna w wersji oryginalnej samouczka [EF 4.1 / MVC 3 e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Aplikacja sieci Web University firmy Contoso

Aplikację, która będzie układać w tych samouczkach jest witryną sieci web university proste.

Użytkownicy mogą wyświetlać i uczniów, kursu i informacje o wykładowcach aktualizacji. Poniżej przedstawiono niektóre ekrany, które zostaną utworzone.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Styl interfejsu użytkownika w tej lokacji została zachowana bliski co to jest generowany przez wbudowane szablony, dzięki czemu samouczka można skupić się głównie na temat korzystania z programu Entity Framework.

## <a name="prerequisites"></a>Wymagania wstępne

Wskazówki i zrzuty ekranu w tym samouczku założono, że używasz [programu Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) lub [programu Visual Studio 2012 Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131), przy użyciu najnowszych aktualizacji i zestawu Azure SDK dla platformy .NET zainstalowane z lipca, 2013. Możesz uzyskać wszystko to z następującego łącza:

[Zestaw Azure SDK dla platformy .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Jeśli masz zainstalowanego programu Visual Studio, powyższe łącze zainstaluje brakujące składniki. Jeśli nie masz programu Visual Studio, link zainstaluje program Visual Studio 2012 Express for Web. Można używać programu Visual Studio 2013, ale niektóre z wymaganych procedur i ekrany będą się różnić.

## <a name="create-an-mvc-web-application"></a>Tworzenie aplikacji sieci Web MVC

Otwórz program Visual Studio i utworzyć nowy projekt C# o nazwie "ContosoUniversity" przy użyciu **aplikacji sieci Web programu ASP.NET MVC 4** szablonu. Upewnij się, że docelowych **.NET Framework 4.5** (należy używać [ `enum` właściwości](https://msdn.microsoft.com/data/hh859576.aspx), i który wymaga platformy .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

W **nowy projekt programu ASP.NET MVC 4** wybierz okno dialogowe **aplikacji internetowej** szablonu.

Pozostaw **Razor** wyświetlić wybrany aparat i pozostawić **Utwórz jednostkowy projekt testowy** pole wyboru jest wyczyszczone.

Kliknij przycisk **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka prostych zmian zostaną skonfigurowane w lokacji menu, układu i strony głównej.

Otwórz *Views\Shared\\_Layout.cshtml*i Zastąp zawartość pliku następującym kodem. Zmiany zostały wyróżnione.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Ten kod wprowadza następujące zmiany:

- Zamienia wystąpienia szablonu "Moja aplikacja platformy ASP.NET MVC" i "tutaj znak logo" "Contoso University".
- Dodaje kilka łącza akcji, które będą używane w dalszej części tego samouczka.

W *Views\Home\Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby wyeliminować akapitów szablonu o ASP.NET MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

W *Controllers\HomeController.cs*, zmień wartość atrybutu `ViewBag.Message` w `Index` metody akcji do "— Zapraszamy! Contoso University!", jak pokazano w poniższym przykładzie:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Naciśnij klawisze CTRL + F5, aby uruchomić witryny. Zostanie wyświetlona strona główna z poziomu menu głównego.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klas jednostek dla aplikacji Contoso University. Rozpocznie się o następujące trzy jednostki:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Brak relacji jeden do wielu między `Student` i `Enrollment` jednostek, i ma relacji jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy student mogą być rejestrowane w dowolnej liczbie kursy i kursu może mieć dowolną liczbę studentów zarejestrowane w nim.

W poniższych sekcjach zostaną utworzone klasy dla każdego z tych obiektów.

> [!NOTE]
> Jeśli spróbujesz skompilować projekt przed zakończeniem Tworzenie wszystkich tych klas jednostek, uzyskasz błędy kompilatora.


### <a name="the-student-entity"></a>Jednostki dla użytkowników domowych

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

W *modele* folderu, Utwórz *Student.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` Właściwość staną się kolumna klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy. Domyślnie program Entity Framework interpretuje właściwość o nazwie `ID` lub *classname* `ID` jako klucz podstawowy.

`Enrollments` Właściwość jest *właściwość nawigacji*. Właściwości nawigacji zawiera inne jednostki, które są powiązane z tą jednostką. W takim przypadku `Enrollments` właściwość `Student` jednostki będą przechowywane wszystkie `Enrollment` jednostek, które są powiązane z których `Student` jednostki. Innymi słowy Jeśli dany `Student` wiersz w bazie danych ma dwie związane z `Enrollment` wierszy (wiersze zawierające klucz podstawowy Studenta dla tej wartości w ich `StudentID` kolumny klucza obcego), że `Student` jednostki `Enrollments` właściwości nawigacji będzie zawierać tych dwóch `Enrollment` jednostek.

Właściwości nawigacji są zazwyczaj definiowane jako `virtual` tak, aby można było korzystać z niektórych funkcji programu Entity Framework takie jak *opóźnionego ładowania*. (Opisano opóźnionego ładowania w dalszej [dane dotyczące odczytywania](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek później w tej serii.

Jeśli właściwość nawigacji może zawierać wiele jednostek (jak relacje wiele do wielu lub jeden do wielu), w którym wpisów można można dodane, usunięte i zaktualizowane, takie jak listy musi być typu `ICollection`.

### <a name="the-enrollment-entity"></a>Jednostka rejestracji

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

W *modele* folderu, Utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Właściwość klasy jest [wyliczenia](https://msdn.microsoft.com/data/hh859576.aspx). Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość jest [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx). Ma wartość null, która różni się od zera klasy — null oznacza, że klasa nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc właściwości może zawierać tylko jeden `Student` jednostki (w przeciwieństwie do `Student.Enrollments` właściwość nawigacji był wyświetlany poprzednio, która zawiera wiele `Enrollment` jednostek).

`CourseID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

### <a name="the-course-entity"></a>Jednostki ciągu

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

W *modele* folderu, Utwórz *Course.cs*, zastępując istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostka może być powiązane z dowolną liczbę `Enrollment` jednostek.

Firma Microsoft będzie więcej powiedzieć o [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([element DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Brak)] atrybutu w następnym samouczku. Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla porach zamiast generować go bazy danych.

## <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

Klasy głównym, która koordynuje funkcji programu Entity Framework o dany model danych jest *kontekst bazy danych* klasy. Utworzyć tę klasę przez pochodny [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) klasy. W kodzie należy określić jednostki, które znajdują się w modelu danych. Można również dostosować określone zachowanie programu Entity Framework. W tym projekcie klasy o nazwie `SchoolContext`.

Utwórz folder o nazwie *DAL* (dla warstwy dostępu do danych). W tym folderze utwórz plik klasy o nazwie *SchoolContext.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Ten kod tworzy [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) właściwości dla każdego zestawu jednostek. W terminologii programu Entity Framework *zestaw jednostek* zazwyczaj odpowiada tabeli bazy danych i *jednostki* odpowiada wiersza w tabeli.

`modelBuilder.Conventions.Remove` Instrukcji w [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metody uniemożliwia trwa pluralized nazwy tabeli. Jeśli tego nie zrobisz, będą miały postać wygenerowanego tabel `Students`, `Courses`, i `Enrollments`. Zamiast tego nazwy tabeli będą `Student`, `Course`, i `Enrollment`. Deweloperzy nie zgadzają się na temat tego, czy należy pluralized nazwy tabeli lub nie. Ten samouczek używa pojedynczej formularza, ale istotne jest, można wybrać dowolną wskazaną formularza preferowane przy tym lub pominięcie ten wiersz kodu.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB.

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) jest wersja programu SQL Server Express aparat bazy danych rozpoczyna się na żądanie, która działa w trybie użytkownika. LocalDB działa w trybie wykonywania specjalnych programu SQL Server Express, który umożliwia pracę z bazami danych jako *.mdf* plików. Zazwyczaj są przechowywane pliki bazy danych LocalDB w *aplikacji\_danych* folderu projektu sieci web. Funkcja wystąpienia użytkownika programu SQL Server Express umożliwia również współpracować *.mdf* pliki, ale funkcja wystąpienia użytkownika jest przestarzały; w związku z tym LocalDB jest zalecane w przypadku pracy z *.mdf* plików.

Zazwyczaj programu SQL Server Express nie jest używany dla aplikacji sieci web w środowisku produkcyjnym. LocalDB w szczególności nie jest zalecane do użytku produkcyjnego z aplikacją sieci web ponieważ nie jest przeznaczony do pracy z usługami IIS.

W programie Visual Studio 2012 i nowszych wersjach LocalDB jest instalowany domyślnie z programem Visual Studio. W programie Visual Studio 2010 i wcześniejszych wersjach programu SQL Server Express (bez LocalDB) jest instalowany domyślnie z programem Visual Studio; należy zainstalować go ręcznie, jeśli używasz programu Visual Studio 2010.

W tym samouczku będziesz pracy z bazy danych LocalDB, aby bazy danych mogą być przechowywane w *aplikacji\_danych* folder jako *.mdf* pliku. Otwórz katalog główny *Web.config* plik i dodać nowe parametry połączenia do `connectionStrings` kolekcji, jak pokazano w poniższym przykładzie. (Upewnij się, że należy zaktualizować *Web.config* pliku w folderze głównym projektu. Istnieje również *Web.config* plik znajduje się w *widoków* podfolder, który nie należy zaktualizować.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Domyślnie program Entity Framework wyszukuje ciąg połączenia o nazwie takiej jak `DbContext` klasy (`SchoolContext` dla tego projektu). Określa ciąg połączenia został dodany, bazy danych LocalDB *ContosoUniversity.mdf* znajduje się w *aplikacji\_danych* folderu. Aby uzyskać więcej informacji, zobacz [parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Faktycznie nie trzeba określić parametry połączenia. Jeśli nie zostanie podane parametry połączenia, Entity Framework utworzy przez; Niemniej jednak bazy danych nie może być w *aplikacji\_danych* folderu aplikacji. Aby uzyskać informacje, na którym zostanie utworzona bazy danych, zobacz [Code First nową bazę danych](https://msdn.microsoft.com/data/jj193542).

`connectionStrings` Kolekcji ma także parametry połączenia o nazwie `DefaultConnection` używany dla bazy danych członkostwa. Bazy danych członkostwa nie będzie używany w tym samouczku. Jedyną różnicą między parametry połączenia dwóch jest nazwa bazy danych i wartości atrybutu name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Konfigurowanie i wykonywanie migracji pierwszy kodu

Przy pierwszym uruchomieniu tworzenie aplikacji, model danych zmiany często i za każdym razem zmian modelu, który pobiera zsynchronizowane z bazą danych. Można skonfigurować programu Entity Framework, aby automatycznie Porzuć i ponownie utworzyć bazę danych w każdej zmianie modelu danych. Nie jest problem na początku programowanie, ponieważ dane testowe jest łatwo ponownego utworzenia, ale po wdrożeniu w środowisku produkcyjnym zwykle chcesz zaktualizować schemat bazy danych bez usuwania bazy danych. Funkcja migracji umożliwia Code First zaktualizować bazy danych bez usunięcie i ponowne utworzenie. Wczesnym etapie cyklu tworzenia nowego projektu można użyć [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) porzucić i ponownie utwórz ponownie inicjatora bazy danych po każdej aktualizacji zmiany modelu. Jeden użytkownik przygotowania do wdrożenia aplikacji, możesz przekształcić w ujęciu migracji. W tym samouczku będą używane wyłącznie migracji. Aby uzyskać więcej informacji, zobacz [migracje Code First](https://msdn.microsoft.com/data/jj591621) i [serii Screencast migracje](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Włącz migracje Code First

1. Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów biblioteki** , a następnie **Konsola Menedżera pakietów**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. W `PM>` monitu wprowadź następujące polecenie:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![polecenia enable-migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    To polecenie tworzy *migracje* folderu projektu ContosoUniversity i umieszcza w tym folderze *Configuration.cs* pliku, który można edytować w celu konfigurowania migracji.

    ![Migrations folder](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration` Klasa zawiera `Seed` metodę, która jest wywoływana po utworzeniu bazy danych oraz za każdym razem, jest on aktualizowany po zmiany modelu danych.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Celem tego `Seed` metodą jest służą do wstawiania danych testowych do bazy danych po jej tworzy lub aktualizuje Code First.

### <a name="set-up-the-seed-method"></a>Konfigurowanie Seed — metoda

[Inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metody uruchamia w bazie danych tworzy migracje Code First i zawsze powoduje zaktualizowanie bazy danych do najnowszej migracji. Seed — metoda służy do wstawiania danych do tabel przed zastosowaniem uzyskuje dostęp do bazy danych po raz pierwszy.

We wcześniejszych wersjach programu Code First, zanim migracje został zwolniony, był często `Seed` metod do wstawiania danych testowych, ponieważ przy każdej zmianie modelu podczas tworzenia bazy danych musiała zostać całkowicie usunięty i utworzony ponownie od początku. Migracje Code First, test dane zostaną zachowane po wprowadzeniu zmian w bazie danych, więc tym dane testowe w [inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) zwykle — metoda nie jest konieczne. W rzeczywistości nie chcesz `Seed` do wstawienia danych testowych, jeśli należy używać migracji do wdrażania bazy danych do środowiska produkcyjnego, ponieważ `Seed` metody zostanie uruchomiony w środowisku produkcyjnym. W takim przypadku ma `Seed` do wstawienia do bazy danych do wstawienia w środowisku produkcyjnym. Na przykład można znaleźć w bazie danych działu rzeczywistej nazwy w `Department` tabeli po udostępnieniu aplikacji, w środowisku produkcyjnym.

W tym samouczku, należy używać migracje dla wdrożenia, ale Twoje `Seed` metody powoduje wstawienie danych testowych mimo to w celu ułatwienia zobaczyć, jak działa funkcji aplikacji bez konieczności wstawiać ręcznie partii danych.

1. Zastąp zawartość *Configuration.cs* pliku z następującym kodem, który będzie ładowanie danych testowych do nowej bazy danych. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [Inicjatora](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda przyjmuje jako parametr wejściowy obiekt kontekstu bazy danych i kod w metodzie używa tego obiektu, aby dodać nowe jednostki w bazie danych. Dla poszczególnych typów jednostek kodu tworzy kolekcję nowych jednostek, dodaje je do odpowiednich [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) właściwości, a następnie zapisuje zmiany w bazie danych. Nie trzeba wywołać [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody po każdej grupie jednostek, jak odbywa się w tym miejscu, ale które pomaga Znajdź źródło problemu w przypadku wystąpienia wyjątku, gdy kod jest zapisywania do bazy danych.

    Niektóre z oświadczeń, które wstawiają dane użycia [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodę można wykonać operacji "upsert". Ponieważ `Seed` metody uruchamiany przy każdej migracji, po prostu nie można wstawić danych, ponieważ wierszy chcesz dodać już będą dostępne po pierwszej migracji, które utworzy bazę danych. Operacja "upsert" uniemożliwia błędów, które może się zdarzyć, jeśli podczas próby wstawienia wiersza, który już istnieje, ale ***zastępuje*** wszelkie zmiany danych, które mogły zostać wprowadzone podczas testowania aplikacji. Z danych testowych w niektórych tabel nie można się zdarzyć, że: w niektórych przypadkach po zmianie danych podczas testowania ma zmiany po aktualizacji bazy danych. W takim przypadku chcesz wykonać operację wstawiania warunkowe: Wstaw wiersz tylko wtedy, gdy jeszcze nie istnieje. Seed — metoda korzysta z obu podejść.

    Pierwszy parametr przekazany do [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody Określa właściwość, można użyć do sprawdzenia, czy wiersz już istnieje. Dla danych uczniów testów, które udostępniasz `LastName` właściwość może być używana w tym celu, ponieważ każdy nazwisko na liście jest unikatowa:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Ten kod przyjęto założenie, że ostatni nazwy są unikatowe. Ręcznie Dodaj uczniów ze zduplikowaną nazwą ostatniego, otrzymasz następujący wyjątek podczas następnego przeprowadzenie migracji.

    Sekwencja zawiera więcej niż jeden element

    Aby uzyskać więcej informacji na temat `AddOrUpdate` metody, zobacz [zajmie się za pomocą metody AddOrUpdate EF 4.3](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

    Kod, który dodaje `Enrollment` jednostek nie są używane `AddOrUpdate` metody. Sprawdza, czy jednostka już istnieje i wstawi jednostkę, jeśli nie istnieje. Takie podejście zostanie zachować zmiany wprowadzone do klasy rejestracji podczas uruchamiania migracji. Kod w pętli każdy element członkowski `Enrollment` [listy](https://msdn.microsoft.com/library/6sh2ey19.aspx) i jeśli rejestracja nie zostanie znaleziony w bazie danych, dodaje rejestracji z bazą danych. Po raz pierwszy aktualizacji bazy danych, bazy danych jest pusta, tak spowoduje dodanie każdego rejestracji.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Aby uzyskać informacje dotyczące debugowania `Seed` — metoda i sposób obsługi nadmiarowych danych, takich jak dwóm studentom / o nazwie "Alexander Carson", zobacz [wstępnego wypełniania i bazami danych debugowanie Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Ricka Andersona.
2. Skompiluj projekt.

### <a name="create-and-execute-the-first-migration"></a>Tworzenie i wykonywanie pierwszej migracji

1. W oknie Konsola Menedżera pakietów wprowadź następujące polecenia: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Polecenie dodaje do folderu migracje *[DateStamp]\_InitialCreate.cs* pliku, który zawiera kod, który tworzy bazy danych. Pierwszy parametr (`InitialCreate)` jest używane dla pliku nazwy i może być dowolne; zazwyczaj wybierz słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji. Na przykład możesz nazwać nowsze migracji &quot;AddDepartmentTable&quot;.

    ![Folder migrations początkowej migracji](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych, i `Down` metoda usuwa je. Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji. Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody. Poniższy kod przedstawia zawartość `InitialCreate` pliku:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` Polecenia `Up` metodę w celu utworzenia bazy danych, a następnie go uruchamia `Seed` metodę, aby Wypełnianie bazy danych.

Utworzono bazę danych programu SQL Server dla modelu danych. Nazwa bazy danych jest *ContosoUniversity*i *.mdf* plik znajduje się w projektu *aplikacji\_danych* folderu ponieważ określony w sieci Parametry połączenia.

Możesz użyć dowolnej **Eksploratora serwera** lub **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazy danych w programie Visual Studio. W tym samouczku użyjesz **Eksploratora serwera**. W programie Visual Studio Express 2012 for Web **Eksploratora serwera** jest nazywany **Eksploratora bazy danych**.

1. Z **widoku** menu, kliknij przycisk **Eksploratora serwera**.
2. Kliknij przycisk **Dodawanie połączenia** ikony.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Jeśli zostanie wyświetlony monit o **wybierz źródło danych** okna dialogowego, kliknij przycisk **programu Microsoft SQL Server**, a następnie kliknij przycisk **Kontynuuj**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. W **Dodawanie połączenia** okna dialogowego wprowadź **(localdb) \v11.0** dla **nazwy serwera**. W obszarze **wybierz lub wprowadź nazwę bazy danych**, wybierz pozycję **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Kliknij przycisk **OK**.
6. Rozwiń węzeł **SchoolContext** , a następnie rozwiń węzeł **tabel**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Kliknij prawym przyciskiem myszy **uczniowie** tabeli, a następnie kliknij przycisk **Pokaż dane tabeli** kolumn, które zostały utworzone i wiersze, które zostały wstawione do tabeli.

    ![Tabela dla użytkowników domowych](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Tworzenie kontrolera dla użytkowników domowych i widoków

Następnym krokiem jest do tworzenia widoków i kontrolera ASP.NET MVC w aplikacji, który może współpracować z jednej z tych tabel.

1. Można utworzyć `Student` kontrolera, kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj**, a następnie kliknij przycisk **kontrolera** . W **Dodaj kontroler** okno dialogowe pola, wybierz następujące opcje, a następnie kliknij przycisk **Dodaj**: 

   - Nazwa kontrolera: **StudentController**.
   - Szablon: **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework**.
   - Klasa modelu: **uczniów (ContosoUniversity.Models)**. (Jeśli nie widzisz tej opcji na liście rozwijanej skompilować projekt i spróbuj ponownie.)
   - Klasa kontekstu danych: **SchoolContext (ContosoUniversity.Models)**.
   - Widoki: **Razor (CSHTML)**. (Ustawienie domyślne).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio otworzy *Controllers\StudentController.cs* pliku. Zobaczysz, że zmienna klasy utworzono tworzącym wystąpienie obiektu kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index` Metody akcji pobiera listę studentów z *studentów* zestaw odczytując jednostek `Students` właściwości wystąpienia kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml* widoku tej listy są wyświetlane w tabeli:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Naciśnij klawisze CTRL + F5, aby uruchomić projekt.

     Kliknij przycisk **studentów** kartę, aby wyświetlić dane testowe który `Seed` dodaje metody.

     ![Strona indeksu dla użytkowników domowych](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Konwencje

Ilość kodu musiały zapisu w kolejności Entity Framework można było utworzyć pełnej bazy danych dla Ciebie jest minimalny, z powodu użycia *konwencje*, lub założenia, które wprowadza programu Entity Framework. Niektóre z nich już został zapisany:

- Pluralized formy nazwy klas jednostki są używane jako nazwy tabeli.
- Nazwy właściwości jednostki są używane dla nazw kolumn.
- Właściwości jednostki, które są nazywane `ID` lub *classname* `ID` są rozpoznawane jako właściwości klucza podstawowego.

W tym samouczku można zastąpić konwencje (na przykład określono czy nazwy tabeli nie powinny być pluralized), i dowiesz się więcej na temat Konwencji i jak zastąpić je w [tworzenia więcej złożonych modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) samouczka w dalszej części tej serii. Aby uzyskać więcej informacji, zobacz [pierwszy konwencje związane z kodami](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Podsumowanie

Teraz utworzyć prostą aplikację, która korzysta z programu Entity Framework i programu SQL Server Express do przechowywania i wyświetlania danych. W samouczku następujące dowiesz się, jak wykonać podstawowe CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) operacji. Możesz pozostawić opinii u dołu tej strony. Prosimy o kontakt jak zbędne części tego samouczka i jak firma Microsoft może ją udoskonalać.

Linki do innych zasobów programu Entity Framework, można znaleźć w [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
