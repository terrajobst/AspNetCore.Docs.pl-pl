---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 formularzy sieci Web | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: b5c2ad33452cacdc1d3aca99c13b8c51e8b58b7c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810579"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 formularzy sieci Web
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Przykładowa aplikacja jest witryną internetową uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora.
> 
> Samouczek zawiera przykłady w języku C#. [Przykładowe do pobrania](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) zawiera kod w językach C# i Visual Basic.
> 
> ## <a name="database-first"></a>Najpierw bazy danych
> 
> Istnieją trzy sposoby pracy z danymi w programie Entity Framework: *Database First*, *pierwszy Model*, i *Code First*. Niniejszy samouczek jest dla pierwszej bazy danych. Aby uzyskać informacje o różnicach między tymi przepływami pracy a wskazówki o tym, jak wybrać najlepszy dla danego scenariusza, zobacz [przepływów pracy programu Entity Framework programowania](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Formularze sieci Web
> 
> W tej serii samouczków używa modelu formularzy sieci Web platformy ASP.NET i przyjęto założenie, że wiesz, jak pracować z wzorca ASP.NET Web Forms w programie Visual Studio. Jeśli nie widzisz [wprowadzenie do wzorca ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Jeśli wolisz pracować z platformę ASP.NET MVC, zobacz [rozpoczęcie korzystania z programu Entity Framework przy użyciu platformy ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Pokazane w tym samouczku** | **Współpracuje również z** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Program Visual Studio 2010 Express for Web. Samouczek nie został przetestowany przy użyciu nowszej wersji programu Visual Studio. Istnieje wiele różnic w opcje menu, okna dialogowe i szablony. |
> | .NET 4 | .NET 4.5 jest zgodny z poprzednimi wersjami z .NET 4, ale nie przetestowano tego samouczka przy użyciu platformy .NET 4.5. |
> | Entity Framework 4 | Samouczek nie został przetestowany w nowszych wersjach platformy Entity Framework. Począwszy od programu Entity Framework 5, domyślnie używa EF `DbContext API` wprowadzono w systemie EF 4.1. Formant EntityDataSource został zaprojektowany do użycia `ObjectContext` interfejsu API. Aby uzyskać informacje o sposobie używania EntityDataSource kontrolką `DbContext` interfejsu API, zobacz [ten wpis w blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Pytania
> 
> Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework i LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), lub [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

Aplikacja, której można tworzyć w tych samouczkach znajduje university prostą witrynę sieci Web.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów. Poniżej przedstawiono kilka ekranów, które zostaną utworzone.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Tworzenie aplikacji sieci Web

Aby uruchomić samouczka, Otwórz program Visual Studio, a następnie utwórz nowy projekt aplikacji sieci Web platformy ASP.NET przy użyciu **aplikacji sieci Web ASP.NET** szablonu:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Ten szablon umożliwia utworzenie projektu aplikacji sieci web, która zawiera już arkusza stylów i stronami wzorcowymi:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Otwórz *Site.Master* pliku i zmienić "Moja aplikacja platformy ASP.NET" do "University firmy Contoso".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Znajdź *Menu* formantu o nazwie `NavigationMenu` i zastąp go następującym kodem, który dodaje elementy menu dla stron, zostanie utworzona.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Otwórz *Default.aspx* strony i zmienić `Content` formantu o nazwie `BodyContent` do tego:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Teraz masz prostą stronę główną wraz z łączami do różnych stron, które zostanie utworzona:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Tworzenie bazy danych

W tych samouczkach użyjemy Projektant modelu danych Entity Framework automatyczne tworzenie modelu danych, w oparciu o istniejącą bazę danych (często nazywany *bazy danych* podejście). Alternatywą, który nie pasuje do w tej serii samouczków jest ręczne tworzenie modelu danych, a następnie skrypty Generuj projektanta, służące do tworzenia bazy danych ( *modelu* podejście).

Dla metody bazy danych w ramach tego samouczka następnym krokiem jest dodanie bazy danych do witryny. Najprostszym sposobem jest najpierw pobrać projekt, który łączy się z tego samouczka. Kliknij prawym przyciskiem myszy *aplikacji\_danych* folderu, wybierz **Dodaj istniejący element**i wybierz *School.mdf* plik bazy danych z pobranego projektu.

Alternatywą jest postępuj zgodnie z instrukcjami w artykule [tworzenie przykładowej bazy danych School](https://msdn.microsoft.com/library/bb399731.aspx). Czy pobrać bazy danych lub utwórz go, skopiować *School.mdf* plik z folderu do Twojej aplikacji *aplikacji\_danych* folderu:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Ta lokalizacja *.mdf* pliku przyjęto założenie, korzystania z programu SQL Server 2008 Express.)

Jeśli tworzysz bazy danych ze skryptu, wykonaj następujące kroki, aby utworzyć diagram bazy danych:

1. W **Eksploratora serwera**, rozwiń węzeł **połączeń danych**, rozwiń węzeł *School.mdf*, kliknij prawym przyciskiem myszy **diagramów bazy danych**i wybierz **Dodaj nowy Diagram**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Zaznacz wszystkie tabele, a następnie kliknij przycisk **Dodaj**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    Serwer SQL tworzy diagramu bazy danych, który pokazuje tabel, kolumn w tabelach i relacje między tabelami. Można przenieść tabele, około, aby je zorganizować w dowolny sposób.
3. Diagram jako "SchoolDiagram" Zapisz i zamknij go.

W przypadku pobrania *School.mdf* pliku, która łączy się z tego samouczka diagramu bazy danych można wyświetlić, klikając dwukrotnie plik **SchoolDiagram** w obszarze **diagramów bazy danych** w **Eksploratora serwera**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Diagram wygląda następująco (tabele mogą się w różnych lokalizacjach względem przedstawionego poniżej):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Tworzenie modelu danych Entity Framework

Teraz możesz utworzyć model danych Entity Framework z tej bazy danych. Można utworzyć model danych w folderze głównym aplikacji, ale na potrzeby tego samouczka będziesz umieść go w folderze o nazwie *DAL* (w przypadku warstwy dostępu do danych).

W **Eksploratora rozwiązań**, Dodaj folder projektu o nazwie *DAL* (Upewnij się, że jest w projekcie, nie w ramach rozwiązania).

Kliknij prawym przyciskiem myszy *DAL* folder, a następnie wybierz **Dodaj** i **nowy element**. W obszarze **zainstalowane szablony**, wybierz opcję **danych**, wybierz opcję **ADO.NET Entity Data Model** szablonu, nadaj jej nazwę *SchoolModel.edmx*, i następnie kliknij przycisk **Dodaj**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Spowoduje to uruchomienie Kreatora modelu danych jednostki. W pierwszym kroku w kreatorze **Generuj z bazy danych** opcja jest zaznaczona domyślnie. Kliknij przycisk **Dalej**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

W **wybierz połączenie danych** , pozostaw wartości domyślne i kliknij pozycję **dalej**. Bazę danych School jest domyślnie zaznaczona, a ustawienia połączenia są zapisywane w *Web.config* plik jako **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

W **wybierz obiekty bazy danych** kroku kreatora, zaznacz wszystkie tabele z wyjątkiem `sysdiagrams` (który został utworzony dla diagramu, wcześniej wygenerowany) a następnie kliknij przycisk **Zakończ**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Po zakończeniu tworzenia modelu, Visual Studio Wyświetla graficzną reprezentacją obiektów programu Entity Framework (jednostki), które odnoszą się do tabel bazy danych. (Podobnie jak w przypadku diagramu bazy danych lokalizacji poszczególne elementy mogą się różnić od tego, co widać na ilustracji. Można przeciągnąć elementy w około do dopasowania na ilustracji, jeśli chcesz, aby.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Eksplorowanie modelu danych Entity Framework

Aby zobaczyć, że diagram jednostki wygląda bardzo podobnie do diagramu bazy danych, z kilkoma różnicami. Jedną różnicaą jest dodanie symboli na końcu każdego skojarzenia, które wskazują na typ skojarzenia (relacje między tabelami są nazywane jednostki skojarzeń w modelu danych):

- Skojarzenia typu jeden do zero lub jeden jest reprezentowany przez "1" i "od 0 do 1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    W tym przypadku `Person` jednostki może lub nie mogą być skojarzone z `OfficeAssignment` jednostki. `OfficeAssignment` Jednostki musi być skojarzone z `Person` jednostki. Innymi słowy pod kierunkiem instruktora może lub nie może być przypisany do pakietu office, a każde biuro, które można przypisać do instruktora tylko jeden.
- Skojarzenia typu jeden do wielu jest reprezentowany przez "1" i "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    W tym przypadku `Person` jednostki może lub nie mogą być powiązane `StudentGrade` jednostek. A `StudentGrade` jednostka musi być skojarzony z jednym `Person` jednostki. `StudentGrade` jednostki reprezentują faktycznie zarejestrowane kursy w tej bazie danych; uczniem/uczennicą jest zarejestrowane w szkoleniu, jeśli nie ma żadnych klasy korporacyjnej, `Grade` właściwość ma wartość null. Innymi słowy uczniem/uczennicą nie może być zarejestrowane w żadnych kursów jeszcze, może być zarejestrowane w ramach jednego kursu lub mogą być rejestrowane w wielu kursów. Każdej grupy zaszeregowania w zarejestrowanych kurs dotyczy tylko jeden dla uczniów.
- Skojarzenie wiele do wielu jest reprezentowane przez "\*"i"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    W tym przypadku `Person` jednostki może lub nie mogą być powiązane `Course` jednostek i odwrotnie również ma wartość true: `Course` jednostki może lub nie mogą być powiązane `Person` jednostek. Innymi słowy pod kierunkiem instruktora może uczyć wielu kursów i kursu mogą być prowadzone przez instruktorów wielu. W tej bazie danych, tę relację ma zastosowanie tylko do instruktorów, (nie łączy uczniów do kursów. Studenci są połączone z kursów przez tabelę StudentGrades.)

Inna różnica między diagramu bazy danych oraz model danych jest dodatkowy **właściwości nawigacji** sekcji dla każdej jednostki. Właściwości nawigacji jednostki odwołuje się do powiązanych jednostek. Na przykład `Courses` właściwość `Person` jednostka zawiera zbiór wszystkich `Course` jednostek, które są powiązane z tym, które `Person` jednostki.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Inna różnica między bazy danych i model danych jest brak jeszcze `CourseInstructor` tabeli skojarzenia, która jest używana w bazie danych, aby połączyć `Person` i `Course` tabel w relacji wiele do wielu. Właściwości nawigacji umożliwia pobieranie powiązane `Course` jednostek z `Person` jednostki i pokrewnych `Person` jednostek z `Course` jednostki, więc nie ma potrzeby do reprezentowania tabela skojarzeń w modelu danych.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Do celów tego samouczka, załóżmy, że `FirstName` kolumny `Person` tabela zawiera zarówno imię i osoby drugie imię. Aby zmienić nazwę pola, aby odzwierciedlić, ale administrator bazy danych (DBA) nie może wystąpić potrzeba zmiany bazy danych. Można zmienić nazwę `FirstName` właściwości w modelu danych, przy równoczesnym zachowaniu jego bazy danych jest równoważne bez zmian.

W projektancie, kliknij prawym przyciskiem myszy **FirstName** w `Person` jednostki, a następnie wybierz **Zmień nazwę**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Wpisz nową nazwę "FirstMidName". Spowoduje to zmianę sposobu odwołasz się do kolumny w kodzie bez wprowadzania zmian w bazie danych.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Przeglądarka modelu umożliwia innym sposobem wyświetlenia struktury bazy danych, struktura modelu danych i mapowanie między nimi. Aby sprawdzić, kliknij prawym przyciskiem myszy pusty obszar w Projektancie jednostki, a następnie kliknij przycisk **przeglądarka modeli**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**Przeglądarka modeli** okienku są wyświetlane w widoku drzewa. ( **Przeglądarka modeli** okienko może być zadokowane przy użyciu **Eksploratora rozwiązań** okienka.) **SchoolModel** węzeł reprezentuje strukturę modelu danych, a **SchoolModel.Store** reprezentuje węzeł struktury bazy danych.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Rozwiń **SchoolModel.Store** aby zobaczyć tabele, rozwiń węzeł **tabele / widoki** Aby wyświetlić tabele, a następnie rozwiń węzeł **kurs** się kolumn w tabeli.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Rozwiń **SchoolModel**, rozwiń węzeł **typów jednostek**, a następnie rozwiń węzeł **kurs** węzeł, aby zobaczyć jednostek i właściwości w ramach jednostek.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

W Projektancie albo lub **przeglądarka modeli** okienka możesz zobaczyć relację obiektów z dwóch modeli Entity Framework. Kliknij prawym przyciskiem myszy `Person` jednostki, a następnie wybierz pozycję **mapowania tabeli,**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Spowoduje to otwarcie **szczegóły mapowania** okna. Zwróć uwagę, że to okno umożliwia widzimy, że kolumna bazy danych `FirstName` jest mapowany na `FirstMidName`, czyli co nazwa została zmieniona jego w modelu danych.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework używa XML do przechowywania informacji o bazie danych, modelu danych i mapowania między nimi. *SchoolModel.edmx* plik jest faktycznie pliku XML, który zawiera te informacje. Projektant renderuje informacje w postaci graficznej, ale można również wyświetlić plik jako XML, klikając prawym przyciskiem myszy *edmx* w pliku **Eksploratora rozwiązań**, klikając pozycję **Otwórz za pomocą**i wybierając polecenie **Edytor (tekstu) XML**. (Projektant modelu danych i edytora XML są tylko dwa różne sposoby otwierania i pracy z tym samym pliku, więc nie może mieć projektanta, Otwórz, a następnie otwórz plik w edytorze XML, w tym samym czasie).

Teraz utworzono witrynę sieci Web, bazy danych i modelu danych. W następnym instruktażu będzie rozpocząć pracę z danymi przy użyciu modelu danych oraz platformę ASP.NET `EntityDataSource` kontroli.

> [!div class="step-by-step"]
> [Next](the-entity-framework-and-aspnet-getting-started-part-2.md)
