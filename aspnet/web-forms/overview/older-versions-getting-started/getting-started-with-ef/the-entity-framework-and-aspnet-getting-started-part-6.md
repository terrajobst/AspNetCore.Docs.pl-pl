---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i platformy ASP.NET 4 sieci Web Forms — część 6 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework. Przykładowa aplikacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888802"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Wprowadzenie do korzystania z bazy danych programu Entity Framework 4.0 najpierw i formularzy sieci Web 4 ASP.NET - część 6
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Entity Framework 4.0 i Visual Studio 2010. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Wdrażanie tabeli na hierarchii dziedziczenia

W poprzednich instrukcji pracy z powiązanych danych przez dodawanie i usuwanie relacji i dodając nowe jednostki, która ma relację z istniejącej jednostki. Ten samouczek przedstawia sposób wykonania dziedziczenia w modelu danych.

W programowanie zorientowane obiektowo dziedziczenia służy do ułatwienia pracy z powiązanymi klasami. Na przykład można utworzyć `Instructor` i `Student` klasy, które pochodzą z `Person` klasy podstawowej. Możesz utworzyć te same rodzaje dziedziczenia struktury jednostek w programu Entity Framework.

W tej części samouczka nie tworzenie nowych stron sieci web. Zamiast tego będzie dodać pochodnych jednostki do modelu danych i modyfikowania istniejących stron do używania nowych jednostek.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabela na hierarchii i dziedziczenie tabeli według typu

Bazy danych można przechowywać informacje o powiązanych obiektów, w jednej tabeli lub wiele tabel. Na przykład w `School` bazy danych, `Person` tabela zawiera informacje o studentów i instruktorów w jednej tabeli. Niektóre kolumny dotyczą tylko instruktorów (`HireDate`), niektórych tylko dla uczniów lub studentów (`EnrollmentDate`), a niektóre zarówno (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Można skonfigurować programu Entity Framework, aby utworzyć `Instructor` i `Student` jednostek, które dziedziczą z `Person` jednostki. Ten wzorzec generowania strukturę jednostek dziedziczenia z tabeli pojedynczej bazy danych jest nazywany *tabeli na hierarchii* dziedziczenia (TPH).

Kursów `School` baza danych używa innego wzorca. Szkoleń w trybie online i szkoleń w siedzibie organizacji są przechowywane w oddzielnych tabelach, z których każdy ma element foreign key, który wskazuje `Course` tabeli. Informacje o wspólnych dla obu typów kursu są przechowywane tylko w `Course` tabeli.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Można skonfigurować programu Entity Framework modelu danych, aby `OnlineCourse` i `OnsiteCourse` jednostek dziedziczyć `Course` jednostki. Ten wzorzec generowania strukturze dziedziczenia obiektu z oddzielnych tabelach dla każdego typu z każdym osobnej tabeli dotyczące tabeli, która przechowuje dane wspólne dla wszystkich typów, jest nazywany *tabel na typ* dziedziczenia (TPT).

Wzorce dziedziczenia TPH zazwyczaj zapewniają lepszą wydajność w ramach jednostki niż w przypadku wzorców dziedziczenia TPT wzorce TPT może powodować sprzężenia złożonych zapytań. W tym przewodniku pokazano, jak zaimplementować dziedziczenia TPH. Należy to zrobić, wykonując następujące czynności:

- Utwórz `Instructor` i `Student` typy jednostek, które pochodzą z `Person`.
- Przenoszenie właściwości, które odnoszą się do jednostek pochodnych z `Person` jednostki do jednostki pochodne.
- Ustaw ograniczenia dotyczące właściwości w typach pochodnych.
- Wprowadź `Person` jednostki abstrakcyjnej jednostki.
- Mapa każdego pochodnych jednostki do `Person` tabeli z warunek, który określa, jak ustalić, czy `Person` wiersz reprezentuje, które typu pochodnego.

## <a name="adding-instructor-and-student-entities"></a>Dodawanie instruktora i uczniów jednostki

Otwórz <em>SchoolModel.edmx</em> pliku, kliknij prawym przyciskiem myszy obszar wolne w projektancie, wybierz opcję <strong>Dodaj</strong>, a następnie wybierz pozycję <strong>jednostki</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

W **Dodaj jednostki** okno dialogowe, nazwa jednostki `Instructor` i ustawić jej **typ podstawowy** opcji w celu `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Kliknij przycisk **OK**. Tworzy projektanta `Instructor` jednostki, która jest pochodną `Person` jednostki. Nowa jednostka nie ma jeszcze żadnych właściwości.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Powtórz procedurę, aby utworzyć `Student` jednostki, która również jest pochodną `Person`.

Tylko instruktorów mają dat, więc musisz przenieść tej właściwości z `Person` jednostki do `Instructor` jednostki. W `Person` jednostki, kliknij prawym przyciskiem myszy `HireDate` właściwości i kliknij przycisk **Wytnij**. Kliknij prawym przyciskiem myszy **właściwości** w `Instructor` jednostki i kliknij przycisk **Wklej**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Daty zatrudnienia `Instructor` jednostki nie może mieć wartości null. Kliknij prawym przyciskiem myszy `HireDate` właściwości, kliknij przycisk **właściwości**, a następnie w **właściwości** okna zmiany `Nullable` do `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Powtórz procedurę, aby przenieść `EnrollmentDate` właściwość z `Person` jednostki do `Student` jednostki. Upewnij się, że można również ustawić `Nullable` do `False` dla `EnrollmentDate` właściwości.

Teraz, gdy `Person` jednostki ma właściwości, które są wspólne dla `Instructor` i `Student` jednostek (jako uzupełnienie właściwości nawigacji, które nie przenosisz), można użyć tylko jednostki jako podstawowa jednostka w strukturze dziedziczenia. W związku z tym należy się upewnić, że nigdy nie jest traktowany jako niezależne jednostki. Kliknij prawym przyciskiem myszy `Person` jednostki, wybierz opcję **właściwości**, a następnie w **właściwości** okna Zmień wartość **abstrakcyjny** właściwości  **Wartość true,**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapowanie instruktora i uczniów jednostek do tabeli osoby

Teraz należy sprawdzić programu Entity Framework, jak do rozróżniania `Instructor` i `Student` jednostek w bazie danych.

Kliknij prawym przyciskiem myszy `Instructor` jednostki i wybierz **mapowania tabeli,**. W **szczegóły mapowania** okna, kliknij przycisk **Dodaj tabelę lub widok** i wybierz **osoby**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Kliknij przycisk **Dodaj warunek**, a następnie wybierz **DataZatrudnienia**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Zmień **Operator** do **jest** i **wartość / właściwości** do **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Powtórz procedurę `Students` jednostki, określając, że ta jednostka jest mapowana do `Person` tabeli, gdy `EnrollmentDate` kolumna nie jest zerowa. Następnie zapisz i zamknij modelu danych.

Skompiluj projekt do tworzenia nowych obiektów w postaci klas i udostępnić je w projektancie.

## <a name="using-the-instructor-and-student-entities"></a>Przy użyciu instruktora i uczniów jednostek

Podczas tworzenia stron sieci web, które współpracują z danych dla użytkowników domowych i instruktora z danymi należy je, aby `Person` zestaw jednostek i filtrowane wg `HireDate` lub `EnrollmentDate` właściwości, aby ograniczyć dane zwrotne do studentów lub instruktorów. Jednak obecnie po powiązaniu każdego formantu źródła danych do `Person` zestaw jednostek, można określić jedynie `Student` lub `Instructor` należy wybrać typy jednostek. Ponieważ programu Entity Framework potrafi rozróżnianie studentów i instruktorów w `Person` zestaw jednostek, możesz usunąć `Where` wprowadzić ręcznie w tym celu ustawienia właściwości.

W projektancie programu Visual Studio, można określić typu jednostki `EntityDataSource` kontroli należy wybrać w **właściwość EntityTypeFilter** w polu listy rozwijanej `Configure Data Source` kreatora, jak pokazano w poniższym przykładzie.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

I w **właściwości** można usunąć okno `Where` klauzuli wartości, które nie są już potrzebne, jak pokazano w poniższym przykładzie.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Jednak ponieważ zmieniono kod znaczników dla `EntityDataSource` kontrolek używanych `ContextTypeName` atrybutu, nie można uruchomić **skonfiguruj źródło danych** kreatora na `EntityDataSource` formantów, które zostały już utworzone. W związku z tym będzie wprowadzić wymagane zmiany, zmieniając zamiast tego znacznika.

Otwórz *Students.aspx* strony. W `StudentsEntityDataSource` kontrolować, Usuń `Where` atrybutu i Dodaj `EntityTypeFilter="Student"` atrybutu. Kod znaczników będzie teraz podobne do następujących:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Ustawienie `EntityTypeFilter` atrybutu upewnia się, że `EntityDataSource` formant będzie wybierać tylko określony typ jednostki. Jeśli chcesz pobrać zarówno `Student` i `Instructor` typy jednostek, czy nie ustawisz ten atrybut. (Jest dostępna opcja pobierania wiele typów jednostek o jednym `EntityDataSource` kontroli tylko wtedy, gdy formant używaną na potrzeby dostępu do danych tylko do odczytu. Jeśli używasz `EntityDataSource` do wstawiania, aktualizacji lub usuwania jednostek kontrolować, a jeśli zestaw jednostek, jest on powiązany z może zawierać wiele typów może pracować tylko z jedną jednostkę typu, i należy ustawić wartość tego atrybutu.)

Powtórz procedurę `SearchEntityDataSource` kontrolować, z wyjątkiem usunąć tylko część `Where` atrybut, który wybiera `Student` jednostek zamiast całkowicie usunąć właściwość. Otwierający tag formantu zostanie teraz podobne do następujących:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Uruchomić stronę, aby sprawdzić, czy nadal działa tak jak poprzednio.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Zaktualizuj następujące strony utworzonych w starszych samouczki tak, aby były używane nowe `Student` i `Instructor` jednostek zamiast `Person` jednostek, i uruchom je, aby sprawdzić, czy działają tak samo jak przed:

- W *StudentsAdd.aspx*, Dodaj `EntityTypeFilter="Student"` do `StudentsEntityDataSource` formantu. Kod znaczników będzie teraz podobne do następujących: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- W *About.aspx*, Dodaj `EntityTypeFilter="Student"` do `StudentStatisticsEntityDataSource` kontroli i Usuń `Where="it.EnrollmentDate is not null"`. Kod znaczników będzie teraz podobne do następujących: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- W *Instructors.aspx* i *InstructorsCourses.aspx*, Dodaj `EntityTypeFilter="Instructor"` do `InstructorsEntityDataSource` kontroli i Usuń `Where="it.HireDate is not null"`. Znaczniki w *Instructors.aspx* teraz podobnego do następującego: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Znaczniki w *InstructorsCourses.aspx* będzie teraz wyglądać następująco:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

W wyniku tych zmian została ulepszona utrzymanie aplikacji Contoso University na kilka sposobów. Przeniósł wybór i weryfikacji logiki poza warstwy interfejsu użytkownika (*.aspx* znaczników) i jego integralną częścią Warstwa dostępu do danych. Pozwala to izolować kod aplikacji z zmiany, które można wprowadzać w przyszłości schemat bazy danych lub modelu danych. Na przykład można zdecydować, że studentów może dzierżawione jako nauczycieli pomocy i w związku z tym jak data zatrudnienia. Następnie można dodać nowych właściwości w celu odróżnienia studentów z instruktorów i aktualizowanie modelu danych. Brak kodu w aplikacji sieci web należy zmienić z wyjątkiem przypadków, w którym chcesz wyświetlić datę zatrudnienia dla uczniów lub studentów. Inną zaletą Dodawanie `Instructor` i `Student` jednostek jest czy kod jest łatwiej zrozumiałe niż podczas jego określonych `Person` obiektów, które zostały faktycznie studentów lub instruktorów.

Teraz przedstawiono jeden ze sposobów implementują wzorzec dziedziczenia w ramach jednostki. Następujące samouczka dowiesz się, jak używane procedury składowane w celu większą kontrolę nad jak Entity Framework uzyskuje dostęp do bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-7.md)
