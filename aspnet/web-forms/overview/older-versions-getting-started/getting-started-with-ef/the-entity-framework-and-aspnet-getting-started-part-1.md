---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 formularzy sieci Web | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: ad504b02d801f9513787f9fde1a4d00d7b0afff0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 formularzy sieci Web
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Przykładowa aplikacja jest witryną internetową uniwersytetu fikcyjnej firmy Contoso. Obejmuje funkcje, takie jak wprowadzenia studentów, tworzenie kursu i instruktora przypisania.
> 
> Samouczek zawiera przykłady w języku C#. [Pobrania](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) zawiera kod w języku C# i Visual Basic.
> 
> ## <a name="database-first"></a>Najpierw bazy danych
> 
> Istnieją trzy sposoby pracy z danymi programu Entity Framework: *Database First*, *Model First*, i *Code First*. Ten samouczek jest przeznaczony dla pierwszej bazy danych. Aby uzyskać informacji o różnicach między te przepływy pracy i wskazówki na temat wybierania najlepszy dla danego scenariusza, zobacz [przepływów pracy programu Entity Framework programowanie](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Formularze sieci Web
> 
> Ten samouczek serii korzysta z modelu formularzy sieci Web ASP.NET i przyjęto założenie, że wiesz, jak pracować z formularzy sieci Web ASP.NET w programie Visual Studio. Jeśli nie zobacz [wprowadzenie do formularzy sieci Web programu ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Jeśli wolisz pracować z platformę ASP.NET MVC, zobacz [wprowadzenie do korzystania z programu Entity Framework za pomocą platformy ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> | **Wyświetlany w samouczku** | **Współdziała również z** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio Express 2010 for Web. Samouczek nie był testowany z nowszej wersji programu Visual Studio. Różnią się wiele opcji menu, okna dialogowe i szablony. |
> | .NET 4 | .NET 4.5 jest zgodna z .NET 4, ale nie została przetestowana samouczek z .NET 4.5. |
> | Entity Framework w wersji 4 | Samouczek nie był testowany z nowszej wersji programu Entity Framework. Począwszy od programu Entity Framework 5, domyślnie używa EF `DbContext API` wprowadzono w systemie EF 4.1. Formant EntityDataSource został zaprojektowany do użycia `ObjectContext` interfejsu API. Aby uzyskać informacje o sposobie używania obiektu EntityDataSource sterować za pomocą `DbContext` interfejsu API, zobacz [ten wpis w blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Pytania
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework i składnika LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), lub [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

Aplikację, która będzie układać w tych samouczkach jest proste university witryny sieci Web.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Użytkownicy mogą wyświetlać i uczniów, kursu i informacje o wykładowcach aktualizacji. Poniżej przedstawiono niektóre ekrany, które zostaną utworzone.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Tworzenie aplikacji sieci Web

Aby uruchomić samouczek, Otwórz program Visual Studio i Utwórz nowy projekt aplikacji sieci Web ASP.NET przy użyciu **aplikacji sieci Web ASP.NET** szablonu:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Ten szablon tworzy projekt aplikacji sieci web, która zawiera już arkusz stylów i stron wzorcowych:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Otwórz *Site.Master* plików i zmień "Moja aplikacja platformy ASP.NET" na "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Znajdź *Menu* formantu o nazwie `NavigationMenu` i Zastąp następujący kod, który dodaje elementy menu dla stron, trzeba utworzyć.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Otwórz *Default.aspx* strony i zmienić `Content` formantu o nazwie `BodyContent` do poniższego:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Masz teraz proste strony głównej zawiera łącza do różnych stron, którymi trzeba utworzyć:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Tworzenie bazy danych

Te samouczki użyjesz Projektant modelu danych programu Entity Framework do automatycznego tworzenia modelu danych, na podstawie istniejącej bazy danych (często nazywane *pierwszej bazy danych* podejście). Alternatywa, które nie są ujęte w tym samouczku jest może ręcznie utworzyć model danych, a następnie wygeneruj projektanta skrypty, które utworzył bazę danych ( *pierwszy model* podejście).

W przypadku metody pierwszej bazy danych używanych w tym samouczku następnym krokiem jest dodawanie bazy danych do lokacji. Najprostszym sposobem jest najpierw pobrać projekt, który łączy się z tego samouczka. Kliknij prawym przyciskiem myszy *aplikacji\_danych* folderu, wybierz opcję **Dodaj istniejący element**i wybierz *School.mdf* pliku bazy danych z pobranego projektu.

Alternatywą jest postępuj zgodnie z instrukcjami w [tworzenie przykładowej bazy danych służbowych](https://msdn.microsoft.com/library/bb399731.aspx). Czy pobrać bazy danych lub utwórz go, skopiować *School.mdf* plik z folderu do aplikacji *aplikacji\_danych* folderu:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Ta lokalizacja *.mdf* pliku zakłada używasz programu SQL Server 2008 Express.)

Jeśli utworzysz bazę danych ze skryptu, wykonaj następujące kroki, aby utworzyć schemat bazy danych:

1. W **Eksploratora serwera**, rozwiń węzeł **połączenia danych**, rozwiń węzeł *School.mdf*, kliknij prawym przyciskiem myszy **diagramy**i wybierz **Dodaj nowy Diagram**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Wybierz wszystkie tabele, a następnie kliknij przycisk **Dodaj**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    Program SQL Server tworzy schemat bazy danych, który pokazuje tabel, kolumn w tabelach i relacje między tabelami. Można przenieść tabele, około, aby je zorganizować w dowolny sposób.
3. Diagram jako "SchoolDiagram" Zapisz i zamknij go.

W przypadku pobrania *School.mdf* pliku, która łączy się z tego samouczka schemat bazy danych można wyświetlić, klikając dwukrotnie **SchoolDiagram** w obszarze **diagramów bazy danych** w **Eksploratora serwera**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Diagram wygląda następująco (tabele mogą być w różnych lokalizacjach od przedstawionego poniżej):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Tworzenie modelu danych programu Entity Framework

Teraz możesz utworzyć Entity Framework modelu danych z tej bazy danych. Można utworzyć modelu danych w katalogu głównym aplikacji, ale w tym samouczku będziesz umieścić go w folderze o nazwie *DAL* (dla warstwy dostępu do danych).

W **Eksploratora rozwiązań**, dodać folder projektu o nazwie *DAL* (Upewnij się, ponieważ nie znajduje się w projekcie, nie w ramach rozwiązania).

Kliknij prawym przyciskiem myszy *DAL* folder, a następnie wybierz **Dodaj** i **nowy element**. W obszarze **zainstalowane szablony**, wybierz pozycję **danych**, wybierz pozycję **modelu danych jednostki ADO.NET** szablonu, nadaj jej nazwę *SchoolModel.edmx*, i następnie kliknij przycisk **Dodaj**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Spowoduje to uruchomienie Kreatora modelu danych jednostki. W pierwszym kroku kreatora **generowania z bazy danych** opcja jest domyślnie zaznaczona. Kliknij przycisk **Dalej**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

W **wybierz połączenie danych** krok, pozostaw wartości domyślne i kliknij przycisk **dalej**. Służbowe bazy danych jest domyślnie zaznaczony, a ustawienia połączenia są zapisywane w *Web.config* pliku jako **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

W **wybierz obiekty bazy danych użytkownika** kroku kreatora, wybierz wszystkie tabele, z wyjątkiem `sysdiagrams` (który został utworzony dla diagramu wygenerowanego wcześniej), a następnie kliknij przycisk **Zakończ**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Po zakończeniu tworzenia modelu, Visual Studio zawiera graficzną reprezentację obiektów programu Entity Framework (jednostki), które odpowiadają tabel bazy danych. (Tak jak w przypadku schemat bazy danych, lokalizacja poszczególne elementy mogą się różnić od informacje wyświetlane na tej ilustracji. Można przeciągnąć elementy około do dopasowania na ilustracji, jeśli chcesz.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Eksploracja modelu danych programu Entity Framework

Widać, że diagram jednostek wygląda bardzo podobnie do diagramu bazy danych na kilka różnic. Jeden różnica polega na dodanie symbole pod koniec każdego skojarzenia, wskazujące typ powiązania (relacje między tabelami są nazywane skojarzenia jednostek w modelu danych):

- To skojarzenie jeden do zero lub jeden jest reprezentowana przez "1" i "od 0 do 1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    W takim przypadku `Person` jednostki mogą lub nie mogą być skojarzone z `OfficeAssignment` jednostki. `OfficeAssignment` Jednostki musi być skojarzone z `Person` jednostki. Innymi słowy instruktora może lub nie może być przypisany do pakietu office, a wszelkie pakietu office można przypisać do instruktora tylko jeden.
- Odpowiada to skojarzenie jeden do wielu "1" i "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    W takim przypadku `Person` jednostki mogą lub nie mogą być powiązane `StudentGrade` jednostek. A `StudentGrade` jednostki musi być skojarzony z jednym `Person` jednostki. `StudentGrade` jednostek faktycznie stanowi zarejestrowanych szkoleń w tej bazie danych; student jest zarejestrowane w toku, jeśli nie ma żadnych klasy jeszcze `Grade` właściwość ma wartość null. Innymi słowy studenta nie mogą być rejestrowane w żadnych kursów jeszcze, mogą być rejestrowane w jednym przebiegu lub mogą być rejestrowane w wielu kursów. Każda klasa w zarejestrowany kursu dotyczy uczniów tylko jeden.
- Odpowiada to skojarzenie wiele do wielu "\*"i"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    W takim przypadku `Person` jednostki mogą lub nie mogą być powiązane `Course` jednostek i odwrotnie również ma wartość true: `Course` jednostki mogą lub nie mogą być powiązane `Person` jednostek. Innymi słowy instruktora może nauczyć wielu kursów i kursu może organizowane jednocześnie przez wiele instruktorów. (W tej bazie danych, tę relację ma zastosowanie tylko do instruktorów; nie zawiera łączy studentów do szkolenia. Studenci są połączone z kursów przez tabelę StudentGrades.)

Inna różnica między schemat bazy danych i modelu danych jest dodatkowy **właściwości nawigacji** sekcji dla każdej jednostki. Powiązanych jednostek odwołuje się do właściwości nawigacji jednostki. Na przykład `Courses` właściwości w `Person` jednostki zawiera kolekcję wszystkich `Course` jednostek, które są powiązane z których `Person` jednostki.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Jeszcze inna różnica między modelu i bazy danych jest Brak `CourseInstructor` skojarzenia tabeli, która jest używana w bazie danych, aby połączyć `Person` i `Course` tabel w relacji wiele do wielu. Właściwości nawigacji umożliwia uzyskiwanie związane z `Course` jednostek z `Person` jednostki i pokrewnych `Person` jednostek z `Course` jednostki, więc nie istnieje potrzeba do reprezentowania tabeli skojarzenia w modelu danych.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Do celów tego samouczka, załóżmy, że `FirstName` kolumny `Person` tabela zawiera zarówno osoby imię i drugie imię. Aby zmienić nazwę pola, aby odzwierciedlić, ale administrator bazy danych (DBA) nie może być wymagana zmiana bazy danych. Można zmienić nazwę `FirstName` właściwości w modelu danych, pozostawiając równoważne swojej bazy danych bez zmian.

W projektancie, kliknij prawym przyciskiem myszy **imię** w `Person` jednostki, a następnie wybierz **zmienić**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Wpisz nową nazwę "FirstMidName". Spowoduje to zmianę sposobu odwoływać się do kolumny w kodzie bez zmieniania bazy danych.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Przeglądarka modeli zawiera inny sposób wyświetlenia struktury bazy danych, strukturę modelu danych i mapowanie między nimi. Aby je wyświetlić, kliknij prawym przyciskiem myszy pusty obszar w programie entity designer, a następnie kliknij przycisk **przeglądarki modelu**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**Przeglądarki modelu** okienku są wyświetlane w widoku drzewa. ( **Przeglądarki modelu** może być zadokowane okienko **Eksploratora rozwiązań** okienko.) **SchoolModel** węzeł reprezentuje strukturę modelu danych i **SchoolModel.Store** reprezentuje węzeł struktury bazy danych.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Rozwiń węzeł **SchoolModel.Store** aby zobaczyć tabele, rozwiń węzeł **tabele / widoki** Aby wyświetlić tabele, a następnie rozwiń węzeł **kursu** wyświetlić kolumny w tabeli.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Rozwiń węzeł **SchoolModel**, rozwiń węzeł **typów jednostek**, a następnie rozwiń węzeł **kursu** węzeł, aby zobaczyć jednostki i właściwości w ramach jednostkami.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

W Projektancie albo lub **przeglądarki modelu** okienko widać powiązań obiektów dwóch modeli Entity Framework. Kliknij prawym przyciskiem myszy `Person` jednostki i wybierz **mapowania tabeli,**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Spowoduje to otwarcie **szczegóły mapowania** okna. Powiadomienie, że to okno pozwala zobaczyć, które kolumny bazy danych `FirstName` jest mapowany na `FirstMidName`, która jest, co można zmienić nazwy jej w modelu danych.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework jest używany plik XML do przechowywania informacji o bazie danych, modelu danych i mapowania między nimi. *SchoolModel.edmx* plik jest rzeczywiście plik XML, który zawiera te informacje. Projektant renderuje informacje w postaci graficznej, ale można również wyświetlić pliku XML, klikając prawym przyciskiem myszy *edmx* w pliku **Eksploratora rozwiązań**, klikając pozycję **Otwórz za pomocą**, a następnie wybierając polecenie **edytora XML (tekst)**. (Projektant modelu danych i edytora XML są tylko dwa różne sposoby otwierania i pracy z tego samego pliku, więc nie może mieć designer Otwórz i Otwórz plik w edytorze XML w tym samym czasie).

Witryny sieci Web, bazy danych i model danych został już utworzony. W następnym wskazówki będzie rozpocząć pracę z danymi przy użyciu modelu danych i ASP.NET `EntityDataSource` formantu.

> [!div class="step-by-step"]
> [Next](the-entity-framework-and-aspnet-getting-started-part-2.md)
