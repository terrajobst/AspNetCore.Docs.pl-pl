---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Wprowadzenie do programu Entity Framework 6 Code First przy użyciu MVC 5 | Dokumentacja firmy Microsoft
author: tdykstra
description: 'Dostępna jest nowsza wersja tej serii samouczek: wprowadzenie do platformy ASP.NET Core oraz Entity Framework Core za pomocą programu Visual Studio 2015. Contoso Universi...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 2417a872bb57b18f4a61ef70f5dd35cb3d94ff73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Wprowadzenie do podejścia Code First w programie Entity Framework 6 z wykorzystaniem MVC 5
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Dostępna jest nowsza wersja tego samouczka serii: [wprowadzenie do platformy ASP.NET Core oraz Entity Framework Core za pomocą programu Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu programu Entity Framework 6 i Visual Studio 2013. W tym samouczku używana Code First przepływu pracy. Aby dowiedzieć się, jak dokonać wyboru między Code First, pierwszy bazy danych i Model First, zobacz [przepływów pracy programu Entity Framework programowanie](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> Przykładowa aplikacja jest witryną sieci web dla fikcyjnej uniwersytetu Contoso. Obejmuje funkcje, takie jak wprowadzenia studentów, tworzenie kursu i instruktora przypisania. Ten samouczek serii wyjaśniono sposób zastosowania Contoso University przykładowej aplikacji. Możesz [Pobierz ukończona aplikacja](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Dostępna jest wersja języka Visual Basic, przetłumaczyła Brind Jan: [MVC 5 z programów EF 6 w języku Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) w witrynie Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (pakiet NuGet EntityFramework 6.1.0)
> - [Windows Azure SDK w wersji 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (opcjonalnie)
>   
> 
> Samouczek, również powinny działać z [programu Visual Studio 2013 Express for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) lub Visual Studio 2012. [VS 2012 wersję zestawu Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) jest wymagane do wdrożenia systemu Windows Azure w programie Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Poprzednie wersje tego samouczka można znaleźć [EF 4.1 / MVC 3 e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) i [wprowadzenie 5 EF przy użyciu MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework i składnika LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), lub [ StackOverflow.com](http://stackoverflow.com/).
> 
> Jeśli napotkasz problem, którego nie można rozpoznać rozwiązania tego problemu można znaleźć ogólnie na podstawie porównania ilości kodu ukończone projekt, który można pobrać. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [typowych błędów i rozwiązania lub obejścia dla nich.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Aplikacja sieci Web University firmy Contoso

Aplikację, która będzie układać w tych samouczkach jest witryną sieci web university proste.

Użytkownicy mogą wyświetlać i uczniów, kursu i informacje o wykładowcach aktualizacji. Poniżej przedstawiono niektóre ekrany, które zostaną utworzone.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Edytuj studentów](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Styl interfejsu użytkownika w tej lokacji została zachowana bliski co to jest generowany przez wbudowane szablony, dzięki czemu samouczka można skupić się głównie na temat korzystania z programu Entity Framework.

## <a name="prerequisites"></a>Wymagania wstępne

Zobacz **wersje oprogramowania** w górnej części strony. Entity Framework 6 nie jest wymagane, ponieważ zainstaluj pakiet EF NuGet w ramach tego samouczka.

## <a name="create-an-mvc-web-application"></a>Tworzenie aplikacji sieci Web MVC

Otwórz program Visual Studio i utworzyć nowy projekt sieci Web C# o nazwie "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

W **nowy projekt ASP.NET** wybierz okno dialogowe **MVC** szablonu.

Jeśli **Hostuj w chmurze** pole wyboru w **Microsoft Azure** sekcja jest wybrana, usuń go.

Kliknij przycisk **Zmień uwierzytelnianie**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

W **Zmień uwierzytelnianie** okno dialogowe, wybierz opcję **bez uwierzytelniania**, a następnie kliknij przycisk **OK**. W tym samouczku możesz nie być wymaganie od użytkowników logowania się lub ograniczanie dostępu w oparciu o który jest zalogowany.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

W oknie dialogowym Nowy projekt ASP.NET, kliknij **OK** Aby utworzyć projekt.

## <a name="set-up-the-site-style"></a>Ustawianie stylów lokacji

Kilka prostych zmian zostaną skonfigurowane w lokacji menu, układu i strony głównej.

Otwórz *Views\Shared\\_Layout.cshtml*i wprowadź następujące zmiany:

- Zmienić każde wystąpienie "Moja aplikacja platformy ASP.NET" i "Nazwa aplikacji" na "Contoso University".
- Dodaj elementy menu dla uczniów lub studentów, kursy instruktorów i działów i usunąć wpis kontaktu.

Zmiany zostały wyróżnione.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

W *Views\Home\Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zastąpić tekst o ASP.NET i MVC tekst o tej aplikacji:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Naciśnij klawisze CTRL + F5, aby uruchomić witryny. Zostanie wyświetlona strona główna z poziomu menu głównego.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Instalowanie programu Entity Framework 6

Z **narzędzia** kliknij menu **Menedżera pakietów NuGet** , a następnie kliknij przycisk **Konsola Menedżera pakietów**.

W **Konsola Menedżera pakietów** okna wprowadź następujące polecenie:

`Install-Package EntityFramework`

![EF zainstalowany](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Na ilustracji przedstawiono 6.0.0 instalowane, ale NuGet zainstaluje najnowszej wersji programu Entity Framework (z wyjątkiem wersji wstępnych), który od ostatniej aktualizacji do samouczka jest 6.1.1.

Ten krok jest jednym z kilku kroków, co program w tym samouczku można wykonać ręcznie, ale który można zostały wykonane automatycznie przez funkcję szkieletów ASP.NET MVC. Podczas wykonywania je ręcznie, aby mogli przeglądać kroki wymagane do używania programu Entity Framework. Szkieletów będzie użyć później, aby utworzyć kontroler MVC i widoków. Alternatywą jest umożliwienie szkieletów automatycznie zainstaluj pakiet EF NuGet, Utwórz klasę kontekstu bazy danych i utworzyć parametry połączenia. Jeśli wszystko jest gotowe do wykonania go w ten sposób, wystarczy, że będzie pominąć te kroki i utworzyć szkielet kontroler MVC po utworzeniu klas jednostek.

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Następnie utworzysz klas jednostek dla aplikacji Contoso University. Rozpocznie się o następujące trzy jednostki:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Brak relacji jeden do wielu między `Student` i `Enrollment` jednostek, i ma relacji jeden do wielu między `Course` i `Enrollment` jednostek. Innymi słowy student mogą być rejestrowane w dowolnej liczbie kursy i kursu może mieć dowolną liczbę studentów zarejestrowane w nim.

W poniższych sekcjach zostaną utworzone klasy dla każdego z tych obiektów.

> [!NOTE]
> Jeśli spróbujesz skompilować projekt przed zakończeniem Tworzenie wszystkich tych klas jednostek, uzyskasz błędy kompilatora.


### <a name="the-student-entity"></a>Jednostki dla użytkowników domowych

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

W *modele* folderu, Utwórz plik klasy o nazwie *Student.cs* i Zastąp kod szablonu z następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Właściwość staną się kolumna klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy. Domyślnie program Entity Framework interpretuje właściwość o nazwie `ID` lub *classname* `ID` jako klucz podstawowy.

`Enrollments` Właściwość jest *właściwość nawigacji*. Właściwości nawigacji zawiera inne jednostki, które są powiązane z tą jednostką. W takim przypadku `Enrollments` właściwość `Student` jednostki będą przechowywane wszystkie `Enrollment` jednostek, które są powiązane z których `Student` jednostki. Innymi słowy Jeśli dany `Student` wiersz w bazie danych ma dwie związane z `Enrollment` wierszy (wiersze zawierające klucz podstawowy Studenta dla tej wartości w ich `StudentID` kolumny klucza obcego), że `Student` jednostki `Enrollments` właściwości nawigacji będzie zawierać tych dwóch `Enrollment` jednostek.

Właściwości nawigacji są zazwyczaj definiowane jako `virtual` tak, aby można było korzystać z niektórych funkcji programu Entity Framework takie jak *opóźnionego ładowania*. (Opisano opóźnionego ładowania w dalszej [dane dotyczące odczytywania](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek później w tej serii.)

Jeśli właściwość nawigacji może zawierać wiele jednostek (jak relacje wiele do wielu lub jeden do wielu), w którym wpisów można można dodane, usunięte i zaktualizowane, takie jak listy musi być typu `ICollection`.

### <a name="the-enrollment-entity"></a>Jednostka rejestracji

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

W *modele* folderu, Utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Właściwość będzie klucza podstawowego; używa tej jednostki *classname* `ID` wzorca zamiast `ID` sam jak opisany w `Student` jednostki. Zwykle będzie wybierz jeden wzorzec i używać go w modelu danych. W tym miejscu odmiany przedstawiono służy albo wzorzec. W samouczku nowsze, zobaczysz jak polecenie `ID` bez `classname` ułatwia wdrażanie dziedziczenia w modelu danych.

`Grade` Właściwość jest [wyliczenia](https://msdn.microsoft.com/data/hh859576.aspx). Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość jest [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx). Ma wartość null, która różni się od zera klasy — null oznacza, że klasa nie jest znana lub nie została jeszcze przypisana.

`StudentID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Student`. `Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc właściwości może zawierać tylko jeden `Student` jednostki (w przeciwieństwie do `Student.Enrollments` właściwość nawigacji był wyświetlany poprzednio, która zawiera wiele `Enrollment` jednostek).

`CourseID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Course`. `Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.

Entity Framework interpretuje właściwości jako właściwość klucza obcego, jeśli jest o nazwie *&lt;nazwą właściwości nawigacji&gt;&lt;nazwa właściwości klucza podstawowego&gt;* (na przykład `StudentID`dla `Student` właściwość nawigacji, ponieważ `Student` klucza podstawowego jednostki jest `ID`). Właściwości klucza obcego może również być taką samą nazwę po prostu *&lt;nazwa właściwości klucza podstawowego&gt;* (na przykład `CourseID` ponieważ `Course` klucza podstawowego jednostki jest `CourseID`).

### <a name="the-course-entity"></a>Jednostki ciągu

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

W *modele* folderu, Utwórz *Course.cs*, zastępując kod szablonu z następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` Właściwość jest właściwością nawigacji. A `Course` jednostka może być powiązane z dowolną liczbę `Enrollment` jednostek.

Firma Microsoft będzie więcej powiedzieć o [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) atrybutu w późniejszym samouczku z tej serii. Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla porach zamiast generować go bazy danych.

## <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

Klasy głównym, która koordynuje funkcji programu Entity Framework o dany model danych jest *kontekst bazy danych* klasy. Utworzyć tę klasę przez pochodny [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) klasy. W kodzie należy określić jednostki, które znajdują się w modelu danych. Można również dostosować określone zachowanie programu Entity Framework. W tym projekcie klasy o nazwie `SchoolContext`.

Aby utworzyć folder w projekcie ContosoUniversity, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy Folder**. Nazwa nowego folderu *DAL* (dla warstwy dostępu do danych). W tym folderze utwórz plik klasy o nazwie *SchoolContext.cs*i Zastąp kod szablonu z następującym kodem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Określanie zestawów jednostek

Ten kod tworzy [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) właściwości dla każdego zestawu jednostek. W terminologii programu Entity Framework *zestaw jednostek* zazwyczaj odpowiada tabeli bazy danych i *jednostki* odpowiada wiersza w tabeli.

> [!NOTE] 
> 
> Można mieć pominięto `DbSet<Enrollment>` i `DbSet<Course>` instrukcje i będzie działać w identyczny sposób. Entity Framework to ich niejawnie ponieważ `Student` odwołań do jednostek `Enrollment` jednostki i `Enrollment` odwołań do jednostek `Course` jednostki.


### <a name="specifying-the-connection-string"></a>Określanie parametrów połączenia

Nazwa ciągu połączenia (które można będzie później dodać do pliku Web.config) jest przekazywany do konstruktora.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Można również przekazać w parametrach połączenia zamiast nazwy, który jest przechowywany w pliku Web.config. Aby uzyskać więcej informacji na temat opcji służącą do bazy danych do użycia, zobacz [Entity Framework - połączenia i modele](https://msdn.microsoft.com/data/jj592674).

Jeśli nie zostanie określony ciąg połączenia lub nazwę jednego jawnie, Entity Framework zakłada, że nazwę ciągu połączenia jest taka sama jak nazwa klasy. Nazwa ciągu połączenia, w tym przykładzie będzie wówczas `SchoolContext`, taka sama jak co określasz jawnie.

### <a name="specifying-singular-table-names"></a>Określanie nazw pojedynczej tabeli

`modelBuilder.Conventions.Remove` Instrukcji w [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metody uniemożliwia trwa pluralized nazwy tabeli. Jeśli tego nie zrobisz, będą miały postać wygenerowanego tabele w bazie danych `Students`, `Courses`, i `Enrollments`. Zamiast tego nazwy tabeli będą `Student`, `Course`, i `Enrollment`. Deweloperzy nie zgadzają się na temat tego, czy należy pluralized nazwy tabeli lub nie. Ten samouczek używa pojedynczej formularza, ale istotne jest, można wybrać dowolną wskazaną formularza preferowane przy tym lub pominięcie ten wiersz kodu.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Konfigurowanie EF zainicjować bazy danych z danych testowych

Entity Framework można automatycznie utworzyć (lub porzucenia i ponownego utworzenia) bazy danych dla Ciebie po uruchomieniu aplikacji. Można określić, że należy to zrobić za każdym razem, gdy aplikacja działa lub tylko wtedy, gdy model jest zsynchronizowany z istniejącej bazy danych. Można również napisać `Seed` metody że Entity Framework wymaga automatycznie po utworzeniu bazy danych, aby wypełnić go danymi testu.

Domyślnym zachowaniem jest utworzenie bazy danych tylko wtedy, gdy go nie istnieje (i Zgłoś wyjątek, jeśli model został zmieniony, a baza danych już istnieje). W tej sekcji możesz wskazać czy porzucony i utworzony ponownie przy każdej zmianie modelu bazy danych. Porzucanie bazy danych spowoduje utratę wszystkich danych. Jest to zazwyczaj podczas tworzenia, ponieważ `Seed` metoda będzie uruchamiany, gdy baza danych jest utworzony ponownie i zostaną ponownie utworzone dane testu. Jednak w środowisku produkcyjnym zwykle nie chcesz utracić żadnych danych, za każdym razem, gdy trzeba będzie zmienić schemat bazy danych. Później zobaczysz jak obsługiwać zmiany modelu przy użyciu migracje Code First, aby zmienić schemat bazy danych zamiast usunięcie i ponowne utworzenie bazy danych.

W folderze DAL, Utwórz nowy plik klasy o nazwie *SchoolInitializer.cs* i Zastąp kod szablonu  
Kod, który powoduje, że bazy danych ma zostać utworzony, gdy potrzebne i ładuje dane testowe do nowej bazy danych.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed` Metoda przyjmuje jako parametr wejściowy obiekt kontekstu bazy danych, a następnie używa kod w metodzie  
Ten obiekt, aby dodać nowe jednostki w bazie danych. Dla poszczególnych typów jednostek kodu tworzy kolekcję nowy  
 jednostki, dodaje je do odpowiednich `DbSet` właściwości, a następnie zapisuje zmiany w bazie danych. Nie jest  
należy wywołać `SaveChanges` metody po każdej grupie jednostek, co jest wykonywane, ale wykonanie tej, która pomaga  
źródło problemu możesz znaleźć w przypadku wystąpienia wyjątku, gdy kod jest zapisywania do bazy danych.

Aby przekazać Entity Framework do użycia klasy inicjatora, Dodaj element do `entityFramework` elementu w aplikacji *Web.config* pliku (znajdującego się w folderze głównym projektu), jak pokazano w poniższym przykładzie:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type` Określa kontekstu w pełni kwalifikowaną nazwę klasy i zestawu w, i `databaseinitializer type` określa klasy inicjatora i zestawu jest w pełni kwalifikowaną nazwę. (Gdy nie chcesz EF, aby użyć inicjatora, można ustawić atrybutu na `context` element: `disableDatabaseInitialization="true"`.) Aby uzyskać więcej informacji, zobacz [Entity Framework - ustawienia w pliku Config](https://msdn.microsoft.com/data/jj556606).

Jako alternatywę do ustawienia inicjatora w *Web.config* pliku jest on w kodzie, dodając `Database.SetInitializer` instrukcji `Application_Start` metody w *Global.asax.cs* pliku. Aby uzyskać więcej informacji, zobacz [opis inicjatory bazy danych w Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Aplikacja jest obecnie ustawiona w górę, aby podczas dostępu do bazy danych po raz pierwszy w danym uruchomienia  
aplikacji programu Entity Framework porównuje bazy danych do modelu (Twojej `SchoolContext` oraz klas jednostek). Jeśli tak, aplikacja porzuca i ponownie utworzy bazę danych.

> [!NOTE]
> Podczas wdrażania aplikacji na serwerze sieci web w środowisku produkcyjnym, musisz usunąć lub wyłączyć kodu, który porzuca i ponownie utworzy bazę danych. Należy to zrobić w późniejszym samouczku z tej serii.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Skonfigurowany do korzystania z bazy danych programu SQL Server Express LocalDB EF

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) jest wersja programu SQL Server Express aparatu bazy danych. Jest łatwy do instalowania i konfigurowania, rozpoczyna się na żądanie i działa w trybie użytkownika. LocalDB działa w trybie wykonywania specjalnych programu SQL Server Express, który umożliwia pracę z bazami danych jako *.mdf* plików. Pliki bazy danych LocalDB można umieścić w *aplikacji\_danych* folderu projektu sieci web, jeśli chcesz można było skopiować bazę danych w projekcie. Funkcja wystąpienia użytkownika programu SQL Server Express umożliwia również współpracować *.mdf* pliki, ale funkcja wystąpienia użytkownika jest przestarzały; w związku z tym LocalDB jest zalecane w przypadku pracy z *.mdf* plików. W programie Visual Studio 2012 i nowszych wersjach LocalDB jest instalowany domyślnie z programem Visual Studio.

Zazwyczaj programu SQL Server Express nie jest używany dla aplikacji sieci web w środowisku produkcyjnym. LocalDB w szczególności nie jest zalecane do użytku produkcyjnego z aplikacją sieci web ponieważ nie jest przeznaczony do pracy z usługami IIS.

W tym samouczku będzie działać z bazy danych LocalDB. Otwórz aplikację *Web.config* plik i dodać `connectionStrings` poprzedniego elementu `appSettings` element, jak pokazano w poniższym przykładzie. (Upewnij się, że należy zaktualizować *Web.config* pliku w folderze głównym projektu. Istnieje również *Web.config* plik znajduje się w *widoków* podfolder, który nie należy zaktualizować.)

Jeśli używasz programu Visual Studio 2015, zastąp "v11.0" w parametrach połączenia "MSSQLLocalDB", zgodnie z domyślnej nazwy wystąpienia serwera SQL została zmieniona.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Ciąg połączenia został dodany Określa, że Entity Framework będzie używać bazy danych LocalDB *ContosoUniversity1.mdf*. (Baza danych nie istnieje jeszcze; EF zostanie utworzony.) Jeśli potrzebujesz bazy danych mogą być tworzone w Twojej *aplikacji\_danych* folderu, można dodać `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` w parametrach połączenia. Aby uzyskać więcej informacji dotyczących parametrów połączenia, zobacz [parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Faktycznie nie muszą mieć parametry połączenia w *Web.config* pliku. Jeśli nie zostanie podane parametry połączenia, Entity Framework zostanie użyty domyślny jedną oparte na klasie kontekstu. Aby uzyskać więcej informacji, zobacz [Code First nową bazę danych](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Tworzenie kontrolera dla użytkowników domowych i widoków

Teraz utworzysz strony sieci web, aby wyświetlić dane, a następnie automatycznie wyzwoli proces żądania danych  
Tworzenie bazy danych. Zostaną przez utworzenie nowego kontrolera. Jednak przed tym, skompiluj projekt, aby udostępnić klasy modelu i kontekstu szkieletów kontrolera MVC.

1. Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj**, a następnie kliknij przycisk **nowy element szkieletu**.
2. W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC 5 z widokami używający narzędzia Entity Framework**.

     ![Dodawanie szkieletu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. W oknie dialogowym Dodaj kontroler, wybierz następujące opcje, a następnie kliknij przycisk **Dodaj**:

   - Klasa modelu: **uczniów (ContosoUniversity.Models)**. (Jeśli nie widzisz tej opcji na liście rozwijanej skompilować projekt i spróbuj ponownie.)
   - Klasa kontekstu danych: **SchoolContext (ContosoUniversity.DAL)**.
   - Nazwa kontrolera: **StudentController** (nie StudentsController).
   - Pozostaw wartości domyślne dla innych pól.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Po kliknięciu **Dodaj**, tworzenia szkieletu tworzy plik StudentController.cs i zestaw widoków (pliki cshtml), które współpracują z kontrolerem. W przyszłości, podczas tworzenia projektów, które używają programu Entity Framework mogą również czerpać korzyści z dodatkową funkcjonalnością tworzenia szkieletu: tylko tworzenie pierwszej klasy modelu, nie należy tworzyć parametry połączenia, a następnie w **Dodaj kontroler** polu Określ nową klasę kontekstu. Utworzy tworzenia szkieletu z `DbContext` klasy i połączenia ciągu oraz kontrolera i widoków.
4. Visual Studio otworzy *Controllers\StudentController.cs* pliku. Zobaczysz, że zmienna klasy utworzono tworzącym wystąpienie obiektu kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index` Metody akcji pobiera listę studentów z *studentów* zestaw odczytując jednostek `Students` właściwości wystąpienia kontekstu bazy danych:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml* widoku tej listy są wyświetlane w tabeli:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Naciśnij klawisze CTRL + F5, aby uruchomić projekt. (Jeśli zostanie wyświetlony komunikat o błędzie "Nie można utworzyć kopii w tle", zamknij przeglądarkę i spróbuj ponownie.)

     Kliknij przycisk **studentów** kartę, aby wyświetlić dane testowe który `Seed` dodaje metody. W zależności od sposobu wąskie okna przeglądarki jest, zobaczysz link kartę uczniów na pasku adresu w górnym lub musisz kliknąć prawym górnym rogu, aby zobaczyć łącza.

     ![Przycisk menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Strona indeksu dla użytkowników domowych](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Widok bazy danych

Po uruchomieniu strony studentów i aplikacja próbowała dostęp do bazy danych, EF był wyświetlany nie było żadnej bazy danych, a więc taki plik został utworzony, następnie napotkał metodę inicjatora, aby wypełnić w bazie danych.

Możesz użyć dowolnej **Eksploratora serwera** lub **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazy danych w programie Visual Studio. W tym samouczku użyjesz **Eksploratora serwera**. (W Visual Studio Express wersjach starszych niż 2013, **Eksploratora serwera** jest nazywany **Eksploratora bazy danych**.)

1. Zamknij przeglądarkę.
2. W **Eksploratora serwera**, rozwiń węzeł **połączenia danych**, rozwiń węzeł **kontekstu służbowego (ContosoUniversity)**, a następnie rozwiń węzeł **tabel** Aby wyświetlić tabele w nowej bazy danych.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Kliknij prawym przyciskiem myszy **uczniowie** tabeli, a następnie kliknij przycisk **Pokaż dane tabeli** kolumn, które zostały utworzone i wiersze, które zostały wstawione do tabeli.

    ![Tabela dla użytkowników domowych](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Zamknij **Eksploratora serwera** połączenia.

*ContosoUniversity1.mdf* i *ldf* pliki bazy danych znajdują się w `C:\Users\<yourusername>` folderu.

Ponieważ używasz `DropCreateDatabaseIfModelChanges` inicjatora, użytkownik może teraz zmiany `Student` klasy, uruchom ponownie aplikację i bazy danych będzie automatycznie utworzona ponownie, aby dopasować zmiany. Na przykład, jeśli dodasz `EmailAddress` właściwości `Student` klasy, ponownie uruchom strony studentów i następnie przyjrzeć tabeli ponownie, zostanie wyświetlony nowy `EmailAddress` kolumny.

## <a name="conventions"></a>Konwencje

Ilość kodu musiały zapisu w kolejności Entity Framework można było utworzyć pełnej bazy danych dla Ciebie jest minimalny, z powodu użycia *konwencje*, lub założenia, które wprowadza programu Entity Framework. Niektóre z nich ma już zauważyć lub użyto bez Twojej wiedziały z nich:

- Pluralized formy nazwy klas jednostki są używane jako nazwy tabeli.
- Nazwy właściwości jednostki są używane dla nazw kolumn.
- Właściwości jednostki, które są nazywane `ID` lub *classname* `ID` są rozpoznawane jako właściwości klucza podstawowego.
- Właściwość jest interpretowana jako właściwości klucza obcego, jeśli jest o nazwie *&lt;nazwą właściwości nawigacji&gt;&lt;nazwa właściwości klucza podstawowego&gt;* (na przykład `StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` klucza podstawowego jednostki jest `ID`). Właściwości klucza obcego może również być taką samą nazwę po prostu &lt;nazwa właściwości klucza podstawowego&gt; (na przykład `EnrollmentID` ponieważ `Enrollment` klucza podstawowego jednostki jest `EnrollmentID`).

Przedstawiono konwencje może zostać zastąpiona. Na przykład określić nazwy tabeli nie powinny być pluralized, czy pojawi się później sposobu oznaczania jawnie właściwości jako właściwość klucza obcego. Dowiesz się więcej na temat Konwencji i jak zastąpić je w [tworzenia więcej złożonych modelu danych](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) samouczek później w tej serii. Aby uzyskać więcej informacji na temat Konwencji, zobacz [pierwszy konwencje związane z kodami](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Podsumowanie

Teraz utworzyć prostą aplikację, która używa do przechowywania i wyświetlania danych programu Entity Framework i programu SQL Server Express LocalDB. W samouczku następujące dowiesz się, jak wykonać podstawowe CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) operacji.

Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć. Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
