---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualizowanie danych powiązanych z programu Entity Framework w aplikacji platformy ASP.NET MVC (6 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 227a7fed0ced884db591f0375454d6d0a62518f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875087"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aktualizowanie danych powiązanych z programu Entity Framework w aplikacji platformy ASP.NET MVC (6 10)
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Samouczek serii można uruchomić od początku lub [pobrać projekt starter w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozwiązać, [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i próba odtworzenia problemu. Rozwiązanie tego problemu można znaleźć ogólnie porównując swój kod kompletny kod. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednich instrukcji wyświetlane powiązanych danych; w tym samouczku będziesz aktualizacji powiązanych danych. W przypadku większości relacji można to zrobić, aktualizując odpowiednich pól klucza obcego. Dla relacji wiele do wielu Entity Framework nie ujawnia tabeli sprzężenia w bezpośrednio, więc musi jawnie Dodawanie i usuwanie jednostek do i z właściwości nawigacji odpowiednie.

Na poniższych ilustracjach przedstawiono stron, że będzie działać z.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Dostosowywanie tworzenie i Edycja stron kursów

Po utworzeniu nowego obiektu kursu musi mieć relacji z istniejących działu. Aby to ułatwić, kod z utworzonym szkieletem obejmuje metod kontrolera oraz tworzenie i edytowanie widoków, które zawierają listy rozwijanej wyboru działu. Ustawia listy rozwijanej `Course.DepartmentID` właściwości klucza obcego, a to wszystko programu Entity Framework wymaga, aby załadować `Department` właściwość nawigacji z odpowiednią `Department` jednostki. Będziesz używać szkieletu kodu, ale nieco na dodawanie obsługi błędów i sortowanie listy rozwijanej zmienić.

W *CourseController.cs*, Usuń cztery `Edit` i `Create` metody i Zastąp następujący kod:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList` Metoda pobiera listę wszystkich działach sortowane według nazwy, tworzy `SelectList` kolekcji do listy rozwijanej i przekazuje do widoku w kolekcji `ViewBag` właściwości. Metoda przyjmuje opcjonalny `selectedDepartment` parametr, który umożliwia kod wywołujący określić elementu, który będzie wybierany Po wyrenderowaniu listy rozwijanej. Widok przekazuje nazwę `DepartmentID` do [ `DropDownList` Pomocnika](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), a następnie pomocnika traktował Szukaj w `ViewBag` obiekt do `SelectList` o nazwie `DepartmentID`.

`HttpGet` `Create` Wywołania metody `PopulateDepartmentsDropDownList` metody bez ustawienie wybranego elementu, ponieważ dla nowego kursu działu nie zostanie nawiązane jeszcze:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit` Metoda ustawia wybrany element na podstawie Identyfikatora działu, który jest już przypisany do kursu edytowany:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost` Metod dla obu `Create` i `Edit` również obejmować kod, który ustawia wybranego elementu, gdy ich ponownie wyświetlić stronę po wystąpieniu błędu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Ten kod gwarantuje, że gdy strona zostanie wyświetlony ponownie, aby wyświetlić komunikat o błędzie, niezależnie od działu wybrano pozostaje wybrane.

W *Views\Course\Create.cshtml*, Dodaj wyróżniony kod, aby utworzyć nowe pole Liczba kursu przed **tytuł** pola. Zgodnie z objaśnieniem w samouczku wcześniej, domyślnie nie są szkieletu pól klucza podstawowego, ale ten klucz podstawowy jest opisowy, więc użytkownik ma mieć możliwość wprowadzania wartości klucza.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

W *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, i *Views\Course\Details.cshtml*, Dodaj pole Liczba kursu przed **tytułu**  pola. Ponieważ klucz podstawowy, jest on wyświetlany, ale nie można zmienić.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Uruchom **Utwórz** strony (wyświetlenia strony indeksu ciągu, a następnie kliknij przycisk **Utwórz nowy**), a następnie wprowadź dane nowego kursu:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Kliknij przycisk **Utwórz**. Z nowego kursu ma na liście zostanie wyświetlona strona indeksu ciągu. Nazwa działu na liście strony indeksu pochodzi z właściwości nawigacji, przedstawiający został poprawnie ustanowić relacji.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Uruchom **Edytuj** strony (wyświetlenia strony indeksu ciągu, a następnie kliknij przycisk **Edytuj** na kursu).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Zmień dane na stronie, a następnie kliknij przycisk **zapisać**. Z danymi zaktualizowanych kursów zostanie wyświetlona strona indeksu ciągu.

## <a name="adding-an-edit-page-for-instructors"></a>Dodawanie strony edycji dla instruktorów

Podczas edytowania rekordu instruktora chcesz była możliwa aktualizacja instruktora office przypisania. `Instructor` Jednostka ma relacji jeden do zero lub jeden z `OfficeAssignment` jednostki, co oznacza musi obsługiwać następujących sytuacjach:

- Jeśli pierwotnie miał wartość wyczyszczenie przypisania pakietu office, należy usunąć i usunąć `OfficeAssignment` jednostki.
- Jeśli użytkownik wprowadzi wartość przydziału pakietu office i początkowo był pusty, należy utworzyć nowy `OfficeAssignment` jednostki.
- Jeśli użytkownik zmieni wartość przydziału pakietu office, należy zmienić wartość w istniejącym `OfficeAssignment` jednostki.

Otwórz *InstructorController.cs* i przyjrzyj się `HttpGet` `Edit` metody:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Nie jest utworzony szkielet kodu, co ma. Konfigurowanie danych dla listy rozwijanej, ale co jest potrzebne jest pole tekstowe. Ta metoda Zastąp następujący kod:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Ten kod porzuca `ViewBag` instrukcji i dodaje wczesny ładowania skojarzonych z nim `OfficeAssignment` jednostki. Nie można wykonać ładowania wczesny z `Find` metody, więc `Where` i `Single` metody są używane zamiast tego wybrać instruktora.

Zastąp `HttpPost` `Edit` metodę z następującym kodem. który obsługuje aktualizacje przypisania pakietu office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Kod wykonuje następujące czynności:

- Pobiera bieżący `Instructor` jednostki z bazy danych przy użyciu wczesny ładowania dla `OfficeAssignment` właściwości nawigacji. To jest identyczny jak w `HttpGet` `Edit` metody.
- Aktualizuje pobranej `Instructor` jednostki wartościami z integratora modelu. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) przeciążenia używane umożliwia *dozwolonych* właściwości, które chcesz dołączyć. Pozwala to uniknąć nadmiernego publikowanie zgodnie z objaśnieniem w [drugi samouczek](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Jeśli w lokalizacji biura jest puste, ustawia `Instructor.OfficeAssignment` właściwości na wartość null, aby powiązane wiersza w `OfficeAssignment` tabeli zostaną usunięte.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Zapisuje zmiany w bazie danych.

W *Views\Instructor\Edit.cshtml*, po `div` elementy **Data zatrudnienia** pola, Dodaj nowe pole do edycji lokalizacji pakietu office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Uruchom strony (wybierz **instruktorów** a następnie kliknij pozycję **Edytuj** na instruktora). Zmień **oddział** i kliknij przycisk **zapisać**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Dodawanie przydziałów kursu instruktora edycji strony

Instruktorów może nauczyć dowolną liczbę kursów. Teraz będzie zwiększenia strony Edytuj instruktora przez dodanie możliwości zmiany przypisania kursu za pomocą grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Relacja między `Course` i `Instructor` jednostek jest wiele do wielu, co oznacza, że nie masz bezpośredni dostęp do tabeli sprzężenia. Zamiast tego zostanie Dodawanie i usuwanie jednostek, do i z `Instructor.Courses` właściwości nawigacji.

Interfejs użytkownika, który umożliwia zmianę które kursy instruktora jest przypisane do to grupa pól wyboru. Wyświetlane jest pole wyboru dla porach co w bazie danych, a te, które instruktora jest aktualnie przypisana do są zaznaczone. Użytkownika można zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Gdyby większa liczba kursów będzie prawdopodobnie chcesz użyć innej metody prezentacji danych w widoku, ale może użyć tej samej metody manipulowanie właściwości nawigacji, aby można było utworzyć lub usuwanie relacji.

Aby zapewnić dane do widoku listy pól wyboru, użyjesz klasy modelu widoku. Utwórz *AssignedCourseData.cs* w *ViewModels* folderu i Zamień istniejący kod następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

W *InstructorController.cs*, Zastąp `HttpGet` `Edit` metodę z następującym kodem. Zmiany zostały wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Ten kod dodaje wczesny ładowania dla `Courses` właściwość nawigacji i wywołuje nowe `PopulateAssignedCourseData` metody, aby podać informacje dotyczące używania tablicy pole wyboru `AssignedCourseData` wyświetlić klasy modelu.

Kod w `PopulateAssignedCourseData` metoda odczytuje przez wszystkie `Course` klasa modelu jednostki, aby załadować listę kursów przy użyciu widoku. Dla każdego kursu kodu sprawdza, czy porach występuje w instruktora `Courses` właściwości nawigacji. Aby utworzyć wydajne wyszukiwanie podczas sprawdzania, czy kursu jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w [zestaw HashSet](https://msdn.microsoft.com/library/bb359438.aspx) kolekcji. `Assigned` Właściwość jest ustawiona na `true` kursów instruktora jest przypisany. Widok użyje tej właściwości w celu określenia, które wyboru pola musi być wyświetlane jako wybrane. Na koniec listy jest przekazywany do widoku w `ViewBag` właściwości.

Następnie dodaj kod, który jest wykonywany, gdy użytkownik kliknie **zapisać**. Zastąp `HttpPost` `Edit` metodę z następującym kodem, który wywołuje nowej metody, która aktualizuje `Courses` właściwość nawigacji `Instructor` jednostki. Zmiany zostały wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Ponieważ w widoku nie ma kolekcję `Course` jednostek, integratora modelu nie można automatycznie zaktualizować `Courses` właściwości nawigacji. Zamiast używać integratora modelu do aktualizacji właściwości nawigacji kursów, należy to zrobić w nowym `UpdateInstructorCourses` metody. Dlatego należy wyłączyć `Courses` właściwości z powiązania modelu. To nie wymaga żadnych zmian do kodu, który wywołuje [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) ponieważ używasz *listę dozwolonych podobnej* przeciążenia i `Courses` nie ma na liście include.

Jeśli nie zostały zaznaczone pola wyboru kod w `UpdateInstructorCourses` inicjuje `Courses` właściwości nawigacji o pustej kolekcji:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Kod, a następnie przetwarza w pętli wszystkich kursów w bazie danych i sprawdza każdego kursu względem nich aktualnie przypisane do instruktora od komputerów, które wybrano w widoku. W celu ułatwienia wyszukiwania wydajne, ostatnie dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli zaznaczono pole wyboru dla porach, ale kursu nie znajduje się w `Instructor.Courses` właściwości nawigacji kursu zostanie dodany do kolekcji we właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Jeśli nie zostało zaznaczone pole wyboru dla porach, ale znajduje się w trakcie `Instructor.Courses` właściwość nawigacji, kursu zostanie usunięta z właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

W *Views\Instructor\Edit.cshtml*, Dodaj **kursów** pole z tablicą pola wyboru przez dodanie poniższego wyróżniony kod bezpośrednio po `div` elementy `OfficeAssignment` pola:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Ten kod tworzy tabelę HTML, który zawiera trzy kolumny. W każdej kolumnie ma postać pola wyboru, a po nim tekstem, który składa się z kursu numer i tytuł. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), które informuje o tym integratora modelu, które mają być traktowane jako grupa. `value` Atrybut każdego pola wyboru ma ustawioną wartość `CourseID.` gdy strona jest przesyłana, integratora modelu przekazuje tablicy do kontrolera, który składa się z `CourseID` wartości dla pola wyboru, które zostały wybrane.

Gdy pola wyboru początkowo są renderowane, te, które są przypisane do instruktora kursów ma `checked` atrybuty, które wybierze je (wyświetlanych jest sprawdzany je).

Po zmianie przypisania kursu, należy sprawdzić zmiany, gdy lokacji powróci do `Index` strony. W związku z tym należy dodać kolumnę do tabeli na tej stronie. W takim przypadku nie trzeba używać `ViewBag` obiektu, ponieważ jest już informacje mają być wyświetlane `Courses` właściwość nawigacji `Instructor` jednostki, która przechodząc na stronę jako model.

W *Views\Instructor\Index.cshtml*, Dodaj **kursów** nagłówek bezpośrednio po **Office** nagłówek, jak pokazano w poniższym przykładzie:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Następnie dodaj nowe komórki szczegółów bezpośrednio po komórki szczegółów lokalizacji pakietu office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Uruchom **indeksu instruktora** stronę, aby zobaczyć kursy przypisane do każdego instruktora:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Kliknij przycisk **Edytuj** na instruktora, aby wyświetlić stronę edycji.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Zmiana przypisania niektórych kursu, a następnie kliknij przycisk **zapisać**. Wprowadzone zmiany zostaną odzwierciedlone na stronie indeksu.

 Uwaga: Podejście do edytowania danych kursu instruktora działa również w przypadku, gdy istnieje ograniczona liczba kursów. Kolekcje, które są znacznie większe innego interfejsu użytkownika i różne metody aktualizacji będą wymagane.  
 

## <a name="update-the-delete-method"></a>Metoda usuwania aktualizacji

Zmień kod metody HttpPost Delete, aby rekord przypisania pakietu office (jeśli istnieje) zostanie usunięty po usunięciu instruktora:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Jeśli próbujesz usunąć instruktora, który jest przypisany do działu jako administrator, zostanie wyświetlony błąd integralności referencyjnej. Zobacz [bieżącej wersji tego samouczka](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) dla dodatkowego kodu, który spowoduje automatyczne usunięcie instruktora z dowolnym działu, gdzie instruktora jest przypisany jako administrator.

## <a name="summary"></a>Podsumowanie

To wprowadzenie do pracy z powiązanych danych zostało zakończone. Do tej pory w tych samouczkach wykonaniu operacji pełny zakres CRUD, ale nie zostały omówione problemy ze współbieżnością. Następny samouczek będzie wprowadzenie temat współbieżności, opisano opcje obsługi go, a następnie dodaj Obsługa już napisanych dla typu jednostki jeden kod CRUD współbieżności.

Linki do innych zasobów programu Entity Framework, można znaleźć na końcu [ostatniego samouczku tej serii](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Poprzednie](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
