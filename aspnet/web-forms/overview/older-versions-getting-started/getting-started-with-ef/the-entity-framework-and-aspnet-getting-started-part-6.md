---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 sieci Web Forms — część 6 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy Entity Framework. Przykładowa aplikacja jest...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 57ba4a47dcc33a1fcc449bc32e9ce186e76cfb5b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753510"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 Web Forms — część 6
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Wdrażanie Tabela wg hierarchii dziedziczenia

W poprzednim samouczku doświadczenie w pracy z powiązanych danych, dodając i usuwając relacje i dodając nowe jednostki, która ma relację z istniejącej jednostki. Ten samouczek przedstawia sposób implementowania dziedziczenia w modelu danych.

W programowanie zorientowane obiektowo, można użyć dziedziczenia w celu ułatwienia pracy z powiązanymi klasami. Na przykład można utworzyć `Instructor` i `Student` klas, które wynikają z `Person` klasy bazowej. Można utworzyć tego samego rodzaju struktury dziedziczenia między jednostkami platformy Entity Framework.

W tej części samouczka nie będzie tworzenie nowych stron sieci web. Zamiast tego dodasz pochodne jednostki do modelu danych i modyfikowanie istniejących stron, aby użyć nowych jednostek.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabela wg hierarchii i tabeli na typ dziedziczenia

Bazę danych można przechowywać informacje o powiązanych obiektów, w jednej tabeli lub w wielu tabel. Na przykład w `School` bazy danych, `Person` tabela zawiera informacje o uczniów i Instruktorzy w jednej tabeli. Niektóre kolumny mają zastosowanie tylko do Instruktorzy (`HireDate`), niektóre tylko dla uczniów i studentów (`EnrollmentDate`), a niektóre zarówno (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Można skonfigurować programu Entity Framework do tworzenia `Instructor` i `Student` jednostek, które dziedziczą z `Person` jednostki. Ten wzorzec generowania struktury dziedziczenie jednostki z tabeli pojedynczej bazy danych jest nazywany *Tabela wg hierarchii* dziedziczenia (TPH).

Kursów `School` baza danych używa innego wzorca. Kursy na miejscu i kursom online są przechowywane w oddzielnych tabelach, z których każdy ma element foreign key, który wskazuje na `Course` tabeli. Informacje wspólne dla obu typów kurs, są przechowywane wyłącznie w `Course` tabeli.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Model danych Entity Framework można skonfigurować tak, aby `OnlineCourse` i `OnsiteCourse` jednostek dziedziczyć `Course` jednostki. Ten wzorzec generowania struktury dziedziczenie jednostki w oddzielnych tabelach dla każdego typu z każdej tabeli oddzielne dotyczące tabeli, która przechowuje dane wspólne dla wszystkich typów, jest nazywany *tabeli na typ* dziedziczenia (TPT).

Wzorce dziedziczenia TPH ogólnie zapewniają lepszą wydajność platformy Entity Framework niż TPT dziedziczenia wzorców, ponieważ wzorców TPT może spowodować sprzężenie złożonych zapytań. Ten przewodnik demonstruje sposób implementacji TPH dziedziczenia. Należy to zrobić, wykonując następujące czynności:

- Tworzenie `Instructor` i `Student` typów jednostek, które wynikają z `Person`.
- Właściwości przenoszenia, które odnoszą się do jednostek pochodne z `Person` jednostki pochodnej jednostek.
- Ustaw ograniczenia dotyczące właściwości w typach pochodnych.
- Wprowadź `Person` jednostki abstrakcyjne jednostki.
- Mapa każdego pochodne jednostki do `Person` tabelę zawierającą warunek, który określa, jak ustalić, czy `Person` wiersz reprezentująca typu pochodnego.

## <a name="adding-instructor-and-student-entities"></a>Dodawanie instruktora oraz przypisane jednostki dla uczniów

Otwórz <em>SchoolModel.edmx</em> plików, kliknij prawym przyciskiem myszy wolnych obszar w Projektancie wybierz <strong>Dodaj</strong>, a następnie wybierz <strong>jednostki</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

W **Dodaj jednostkę** okno dialogowe, nazwa jednostki `Instructor` i ustaw jego **typ podstawowy** opcję `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Kliknij przycisk **OK**. Tworzy projektanta `Instructor` jednostki, która pochodzi od klasy `Person` jednostki. Nowa jednostka nie ma jeszcze żadnych właściwości.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Powtórz procedurę, aby utworzyć `Student` jednostki, która również jest pochodną `Person`.

Tylko Instruktorzy dat, więc musisz przenieść tę właściwość z `Person` jednostki do `Instructor` jednostki. W `Person` jednostki, kliknij prawym przyciskiem myszy `HireDate` właściwości i kliknij przycisk **Wytnij**. Kliknij prawym przyciskiem myszy **właściwości** w `Instructor` jednostki i kliknij przycisk **Wklej**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Data zatrudnienia `Instructor` jednostki nie może mieć wartości null. Kliknij prawym przyciskiem myszy `HireDate` właściwości, kliknij przycisk **właściwości**, a następnie w polu **właściwości** okna zmiany `Nullable` do `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Powtórz procedurę, aby przenieść `EnrollmentDate` właściwość `Person` jednostki do `Student` jednostki. Upewnij się, że można również ustawić `Nullable` do `False` dla `EnrollmentDate` właściwości.

Teraz, gdy `Person` jednostka ma właściwości, które są wspólne dla `Instructor` i `Student` jednostki (jako uzupełnienie właściwości nawigacji, które nie przenosisz), jednostki może służyć jedynie jako jednostka podstawowa w strukturze dziedziczenia. W związku z tym należy upewnić się, że nigdy nie jest ona traktowana jako niezależne jednostki. Kliknij prawym przyciskiem myszy `Person` jednostki, wybierz opcję **właściwości**, a następnie w polu **właściwości** okna, zmień wartość właściwości **abstrakcyjne** właściwości  **Wartość true,**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapowanie przez instruktorów i uczniów jednostki do tabeli osoby

Teraz należy przekazać Entity Framework jak do rozróżnienia między `Instructor` i `Student` jednostek w bazie danych.

Kliknij prawym przyciskiem myszy `Instructor` jednostki, a następnie wybierz pozycję **mapowania tabeli,**. W **szczegóły mapowania** okna, kliknij przycisk **Dodaj tabelę lub widok** i wybierz **osoby**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Kliknij przycisk **Dodaj warunek**, a następnie wybierz pozycję **DataZatrudnienia**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Zmiana **Operator** do **jest** i **wartość / właściwości** do **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Powtórz procedurę dla `Students` jednostki, określając mapuje dla tej jednostki `Person` tabeli, gdy `EnrollmentDate` kolumna nie ma wartość null. Następnie zapisz i zamknij modelu danych.

Skompiluj projekt, aby można było tworzyć nowe jednostki jako klasy i zapewnić ich dostępność w projektancie.

## <a name="using-the-instructor-and-student-entities"></a>Za pomocą przez instruktorów i jednostek dla uczniów

Podczas tworzenia stron sieci web, które działają z danymi uczniów i instruktora z danymi możesz je do `Person` zestawu jednostek i filtrowane wg `HireDate` lub `EnrollmentDate` właściwość, aby ograniczyć zwróconych danych do uczniów lub instruktorów. Jednak teraz po powiązaniu każdego formantu źródła danych do `Person` zestaw jednostek, można określić tylko `Student` lub `Instructor` typów jednostek, które powinny być zaznaczone. Ponieważ platforma Entity Framework wie, jak do odróżnienia studentów i Instruktorzy w `Person` zestaw jednostek, możesz usunąć `Where` ustawienia właściwości wprowadzane ręcznie to zrobić.

W projektancie programu Visual Studio, można określić typu jednostki `EntityDataSource` wybierz formant w **EntityTypeFilter** pole listy rozwijanej `Configure Data Source` kreatora, jak pokazano w poniższym przykładzie.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

A następnie w **właściwości** okna, możesz usunąć `Where` klauzuli wartości, które nie są już potrzebne, jak pokazano w poniższym przykładzie.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Jednak ponieważ zmieniono kod znaczników dla `EntityDataSource` kontrolek używanych `ContextTypeName` atrybutu, nie można uruchomić **skonfigurować źródło danych** kreatora na `EntityDataSource` formantów, które zostały już utworzone. W związku z tym wprowadzisz wymagane zmiany, zmieniając znaczników, zamiast tego.

Otwórz *Students.aspx* strony. W `StudentsEntityDataSource` kontrolować, Usuń `Where` atrybutu i Dodaj `EntityTypeFilter="Student"` atrybutu. Znaczniki będzie teraz wyglądać następująco:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Ustawienie `EntityTypeFilter` atrybutu, masz pewność, że `EntityDataSource` kontroli wybierze tylko określonego typu jednostek. Jeśli chcesz pobrać zarówno `Student` i `Instructor` typów jednostek, czy nie ten atrybut zostanie ustawiony. (Istnieje możliwość pobierania typów jednostek przy użyciu jednego `EntityDataSource` kontrolować tylko wtedy, gdy używasz kontroli dostępu do danych tylko do odczytu. Jeśli używasz `EntityDataSource` kontrolę Wstawianie, aktualizowanie lub usuwanie jednostek, a jeśli jest on powiązany z zestawu jednostek może zawierać wiele typów może pracować tylko z jednej jednostki typu i należy ustawić ten atrybut.)

Powtórz procedurę dla `SearchEntityDataSource` kontrolować, z wyjątkiem Usuń tylko część `Where` atrybut, który wybiera `Student` jednostki zamiast usuwania całkowitego właściwości. Otwierający tag formantu będzie teraz wyglądać następująco:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Uruchom stronę, aby sprawdzić, czy nadal działa tak jak poprzednio.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Zaktualizuj następujące strony, utworzone w starszych samouczki, tak aby używały nowej `Student` i `Instructor` jednostki zamiast `Person` jednostek, uruchomić je, aby sprawdzić, czy działają tak samo jak przed:

- W *StudentsAdd.aspx*, Dodaj `EntityTypeFilter="Student"` do `StudentsEntityDataSource` kontroli. Znaczniki będzie teraz wyglądać następująco: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- W *About.aspx*, Dodaj `EntityTypeFilter="Student"` do `StudentStatisticsEntityDataSource` kontroli i Usuń `Where="it.EnrollmentDate is not null"`. Znaczniki będzie teraz wyglądać następująco: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- W *Instructors.aspx* i *InstructorsCourses.aspx*, Dodaj `EntityTypeFilter="Instructor"` do `InstructorsEntityDataSource` kontroli i Usuń `Where="it.HireDate is not null"`. Kod znaczników w *Instructors.aspx* teraz podobnego do następującego: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Kod znaczników w *InstructorsCourses.aspx* będzie teraz wyglądać następująco:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

W wyniku tych zmian został ulepszony, łatwości aplikacji Contoso University na kilka sposobów. Po przeniesieniu wybór i sprawdzanie poprawności logiki poza warstwy Interfejsu (*.aspx* znaczników) i integralną częścią warstwy dostępu do danych. Ułatwia izolowanie kodu aplikacji od zmian, które można wprowadzać w przyszłości schemat bazy danych lub model danych. Na przykład można zdecydować, że studenci może zatrudnić jako pomocy nauczycieli i w związku z tym otrzymamy Data zatrudnienia. Następnie można dodać nową właściwość odróżnianie studentów z instruktorów i aktualizowanie modelu danych. Brak kodu w aplikacji sieci web należy zmienić z wyjątkiem sytuacji, w której chcesz wyświetlić Data zatrudnienia dla uczniów lub studentów. Inną zaletą Dodawanie `Instructor` i `Student` jednostek jest, że kod będzie łatwiej zrozumiałe, niż gdy go określonym `Person` obiektów, które zostały faktycznie uczniów lub instruktorów.

Teraz wiesz jednym ze sposobów, aby zaimplementować wzorzec dziedziczenia platformy Entity Framework. W następującego samouczka dowiesz się, jak używanie procedur składowanych, aby mogła mieć większą kontrolę nad jak Entity Framework uzyskuje dostęp do bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-7.md)
