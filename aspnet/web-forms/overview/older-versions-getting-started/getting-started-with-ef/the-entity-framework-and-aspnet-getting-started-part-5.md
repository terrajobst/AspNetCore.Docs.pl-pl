---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 sieci Web Forms — część 5 | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy Entity Framework. Przykładowa aplikacja jest...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 3209ab3bca58e0dde90cf279732d177418b034e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752182"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Wprowadzenie do bazy danych programu Entity Framework 4.0 First i platformy ASP.NET 4 Web Forms — część 5
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Entity Framework 4.0 i Visual Studio 2010. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Praca z powiązanych danych, kontynuowanie

W poprzednim samouczku rozpoczęcia używania `EntityDataSource` kontrolkę powiązane dane. Wyświetlane na wielu poziomach hierarchii i edytować dane w właściwości nawigacji. W tym samouczku będziesz nadal pracować powiązanych danych, dodając i usuwając relacje i przez dodanie nowego obiektu, który ma ustanowioną relację do istniejącej jednostki.

Utworzysz strony, który dodaje kursy, które są przypisane do działów. Działy już istnieje, a podczas tworzenia nowego kursu, w tym samym czasie będzie ustanawiania relacji między nim a istniejących działu.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Utworzysz też strona, która współdziała z relacją wiele do wielu, przypisując pod kierunkiem instruktora do kursu (Dodawanie relacji między dwiema jednostkami, które można wybrać) lub usunięcie pod kierunkiem instruktora z kursu (usuwanie relacji między dwiema jednostkami, Wybierz). W bazie danych, Dodawanie relacji między instruktora oraz przypisane kurs wyniki w nowym wierszu, do którego jest dodawany `CourseInstructor` Tabela skojarzenia; usuwanie relacji pociąga za sobą usunięcie wierszy z `CourseInstructor` tabeli skojarzenia. Jednak możesz to zrobić w Entity Framework przez ustawienie właściwości nawigacji, bez odwołujące się do `CourseInstructor` tabeli w sposób jawny.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Dodawanie jednostki z relacją do istniejącej jednostki

Utwórz nową stronę sieci web o nazwie *CoursesAdd.aspx* , który używa *Site.Master* strony wzorcowej, a następnie dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Ten kod znaczników tworzy `EntityDataSource` formant, który wybiera kursów, który umożliwia wstawianie i program obsługi, który określa `Inserting` zdarzeń. Użyjesz programu obsługi, aby zaktualizować `Department` właściwość nawigacji, gdy nowy `Course` zostanie utworzona jednostka.

Tworzy również znaczników `DetailsView` kontrolki używanej do dodawania nowych `Course` jednostek. Znaczniki używa powiązanych pól `Course` właściwości jednostki. Należy wprowadzić `CourseID` wartości, ponieważ nie jest polem Identyfikator generowanych przez system. Zamiast tego jest liczba kurs, która musi być określona ręcznie, gdy jest tworzony w czasie.

Możesz użyć pola szablonu dla `Department` właściwość nawigacji, ponieważ nie można używać właściwości nawigacji z `BoundField` kontrolki. Pole szablon zawiera listy rozwijanej, aby wybrać działu. Listy rozwijanej jest powiązany z `Departments` zestawu przy użyciu jednostek `Eval` zamiast `Bind`, ponownie ponieważ bezpośrednio nie można powiązać właściwości nawigacji, aby można było je zaktualizować. Określ program obsługi `DropDownList` kontrolki `Init` zdarzeń, aby przechowujesz odwołanie do formantu do użytku przez kod, który aktualizuje `DepartmentID` klucza obcego.

W *CoursesAdd.aspx.cs* zaraz po deklaracji klasy częściowe, Dodaj pole klasy do przechowywania odwołań do `DepartmentsDropDownList` sterowania:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Dodaj program obsługi `DepartmentsDropDownList` kontrolki `Init` zdarzeń, dzięki czemu można przechowywać odwołanie do formantu. Dzięki temu można uzyskać wartość użytkownik wprowadził i używać go do aktualizacji `DepartmentID` wartość `Course` jednostki.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Dodaj program obsługi `DetailsView` kontrolki `Inserting` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Kiedy użytkownik kliknie `Insert`, `Inserting` zdarzenie jest wywoływane przed wstawieniem nowy rekord. Pobiera kod obsługi `DepartmentID` z `DropDownList` kontroli i używa go do ustawiania wartości, która będzie służyć do `DepartmentID` właściwość `Course` jednostki.

Entity Framework zajmie się dodawania tego kursu, aby `Courses` właściwość nawigacji skojarzona `Department` jednostki. Dodaje także wydziałowi na `Department` właściwość nawigacji `Course` jednostki.

Uruchom stronę.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Wprowadź identyfikator, tytuł, ilość środków i wybierz dział, a następnie kliknij przycisk **Wstaw**.

Uruchom *Courses.aspx* stronie, a następnie wybierz tego samego działu, aby zobaczyć nowe kursu.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Praca z relacji wiele do wielu

Relacja między `Courses` zestawu jednostek i `People` zestaw jednostek znajduje relacji wiele do wielu. A `Course` jednostka ma właściwość nawigacji o nazwie `People` zawierających zero, jeden lub więcej powiązanych `Person` jednostki (reprezentującej Instruktorzy przypisane do nauki danego kursu). I `Person` jednostka ma właściwość nawigacji o nazwie `Courses` zawierających zero, jeden lub więcej powiązanych `Course` jednostki (reprezentujący kursy tego instruktora jest przypisany do nauki). Jeden instruktora może uczyć wielu kursów i jednego kursu mogą być prowadzone przez instruktorów wielu. W tej sekcji tego przewodnika będziesz dodawać i usuwać relacje między `Person` i `Course` jednostek, aktualizując właściwości nawigacji powiązanych jednostek.

Utwórz nową stronę sieci web o nazwie *InstructorsCourses.aspx* , który używa *Site.Master* strony wzorcowej, a następnie dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Ten kod znaczników tworzy `EntityDataSource` formant, który pobiera nazwę i `PersonID` z `Person` jednostki dla instruktorów. A `DropDrownList` kontrolka jest powiązana z `EntityDataSource` kontroli. `DropDownList` Kontroli określa program obsługi `DataBound` zdarzeń. Użyjesz tej obsługi na elementu databind dwóch list rozwijanych, wyświetlające kursów.

Znaczniki wzrasta, powstaje następującej grupy kontrolek używanych w przypadku przypisywania kursu do wybranego przez instruktorów:

- A `DropDownList` kontrolka służąca do wybierania kurs, aby przypisać. Ten formant zostanie wypełniony kursy, które obecnie nie są przypisane do wybranego przez instruktorów.
- A `Button` formantu, aby zainicjować przypisania.
- A `Label` formantu, aby wyświetlić komunikat o błędzie, jeśli przypisanie zakończy się niepowodzeniem.

Na koniec znaczników wzrasta, powstaje grupy formantów służące do usuwania kurs z wybranych przez instruktorów.

W *InstructorsCourses.aspx.cs*, Dodaj instrukcję using instrukcji:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Dodaj metodę do zapełniania dwóch list rozwijanych, wyświetlające kursy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Ten kod pobiera wszystkie kursy z `Courses` jednostki ustaw i pobiera kursów z `Courses` właściwość nawigacji `Person` jednostki dla wybranego przez instruktorów. Następnie określa, które kursy są przypisane do tego przez instruktorów i w związku z tym umożliwia wypełnienie listy rozwijanej.

Dodaj program obsługi `Assign` przycisku `Click` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Ten kod pobiera `Person` jednostki dla wybranego przez instruktorów, pobiera `Course` jednostki dla wybranego kursu i dodaje wybrany kurs, aby `Courses` właściwość nawigacji instruktora `Person` jednostki. Następnie zapisuje zmiany w bazie danych i repopulates list rozwijanych, aby wyniki są widoczne natychmiast.

Dodaj program obsługi `Remove` przycisku `Click` zdarzeń:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Ten kod pobiera `Person` jednostki dla wybranego przez instruktorów, pobiera `Course` jednostki dla wybranego kursu i spowoduje usunięcie wybranych kursów z `Person` jednostki `Courses` właściwości nawigacji. Następnie zapisuje zmiany w bazie danych i repopulates list rozwijanych, aby wyniki są widoczne natychmiast.

Dodaj kod, aby `Page_Load` metodę, która zapewnia, że komunikaty o błędach nie są widoczne, gdy nie ma błędów do raportu i dodać procedury obsługi dla `DataBound` i `SelectedIndexChanged` zdarzenia z listy rozwijanej Instruktorzy do wypełnienia listy rozwijanej kursy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Uruchom stronę.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Wybierz pod kierunkiem instruktora. <strong>Przypisać kurs</strong> listy rozwijanej wyświetla kursy, które nie uczyć instruktora, i <strong>Usuń kurs</strong> listy rozwijanej wyświetla kursy, które instruktora jest już przypisana do. W <strong>przypisać kurs</strong> Wybierz kurs a następnie kliknij przycisk <strong>przypisać</strong>. Kurs przenosi do <strong>Usuń kurs</strong> listy rozwijanej. Wybierz kurs w <strong>Usuń kurs</strong> sekcji, a następnie kliknij przycisk <strong>Usuń</strong><em>.</em> Kurs przenosi do <strong>przypisać kurs</strong> listy rozwijanej.

Teraz wiesz niektóre inne sposoby pracy z powiązanych danych. W następującego samouczka dowiesz się, jak używać dziedziczenia w modelu danych w celu konserwacji aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-6.md)
