---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualizowanie danych powiązanych z programu Entity Framework w aplikacji platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cf4a6183e068e8668eb706d9a9e311616649e863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Aktualizowanie danych powiązanych z programu Entity Framework w aplikacji platformy ASP.NET MVC
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednich instrukcji wyświetlane powiązanych danych; w tym samouczku będziesz aktualizacji powiązanych danych. W przypadku większości relacji można to zrobić, aktualizując pól klucza obcego lub właściwości nawigacji. W przypadku relacji wiele do wielu Entity Framework nie ujawnia tabeli sprzężenia w bezpośrednio, dodawanie i usuwanie jednostek do i z właściwości nawigacji odpowiednie.

Na poniższych ilustracjach przedstawiono niektóre stron, którymi będzie współpracować.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Edytuj instruktora z kursów](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Dostosowywanie tworzenie i Edycja stron kursów

Po utworzeniu nowego obiektu kursu musi mieć relacji z istniejących działu. Aby to ułatwić, kod z utworzonym szkieletem obejmuje metod kontrolera oraz tworzenie i edytowanie widoków, które zawierają listy rozwijanej wyboru działu. Ustawia listy rozwijanej `Course.DepartmentID` właściwości klucza obcego, a to wszystko programu Entity Framework wymaga, aby załadować `Department` właściwość nawigacji z odpowiednią `Department` jednostki. Będziesz używać szkieletu kodu, ale nieco na dodawanie obsługi błędów i sortowanie listy rozwijanej zmienić.

W *CourseController.cs*, Usuń cztery `Create` i `Edit` metody i Zastąp następujący kod:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Dodaj następujące `using` instrukcji na początku pliku:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList` Metoda pobiera listę wszystkich działach sortowane według nazwy, tworzy `SelectList` kolekcji do listy rozwijanej i przekazuje do widoku w kolekcji `ViewBag` właściwości. Metoda przyjmuje opcjonalny `selectedDepartment` parametr, który umożliwia kod wywołujący określić elementu, który będzie wybierany Po wyrenderowaniu listy rozwijanej. Widok przekazuje nazwę `DepartmentID` do [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) pomocnika i pomocnika następnie traktował Szukaj w `ViewBag` obiekt do `SelectList` o nazwie `DepartmentID`.

`HttpGet` `Create` Wywołania metody `PopulateDepartmentsDropDownList` metody bez ustawienie wybranego elementu, ponieważ dla nowego kursu działu nie zostanie nawiązane jeszcze:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit` Metoda ustawia wybrany element na podstawie Identyfikatora działu, który jest już przypisany do kursu edytowany:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost` Metod dla obu `Create` i `Edit` również obejmować kod, który ustawia wybranego elementu, gdy ich ponownie wyświetlić stronę po wystąpieniu błędu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Ten kod gwarantuje, że gdy strona zostanie wyświetlony ponownie, aby wyświetlić komunikat o błędzie, niezależnie od działu wybrano pozostaje wybrane.

Widoki kursu są już szkieletu z listy rozwijanej w polu Dział, ale nie ma podpis DepartmentID dla tego pola, sprawdź następujące wyróżnione Zmień *Views\Course\Create.cshtml* pliku Zmiana podpisu.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Wprowadzanie tych samych zmian w *Views\Course\Edit.cshtml*.

Zwykle tworzenia szkieletu nie szkieletu klucza podstawowego, ponieważ wartości klucza jest generowany przez bazę danych i nie można zmienić, a nie jest wartością znaczenie ma być wyświetlony dla użytkowników. Dla porach jednostek tworzenia szkieletu zawiera pole tekstowe `CourseID` pola, ponieważ zakłada, że `DatabaseGeneratedOption.None` atrybut oznacza, powinni być w stanie użytkownika wprowadź wartość klucza podstawowego. Ale nie rozpoznaje, ponieważ kod jest łatwy do rozpoznania chcesz wyświetlić w innych widokach, więc musisz ręcznie dodać.

W *Views\Course\Edit.cshtml*, Dodaj pole Liczba kursu przed **tytuł** pola. Ponieważ klucz podstawowy, jest on wyświetlany, ale nie można zmienić.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Istnieje już ukryte pole (`Html.HiddenFor` pomocnika) numeru kursu w widoku edycji. Dodawanie *Html.LabelFor* pomocnika nie eliminuje potrzebę stosowania ukryte pola, ponieważ nie powoduje numer kursu mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **zapisać** na stronie edycji.

W *Views\Course\Delete.cshtml* i *Views\Course\Details.cshtml*, zmienianie podpisu nazwę działu z "Name" do "Dział" i Dodaj pole Liczba kursu przed **tytułu**  pola.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Uruchom **Utwórz** strony (wyświetlenia strony indeksu ciągu, a następnie kliknij przycisk **Utwórz nowy**), a następnie wprowadź dane nowego kursu:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Kliknij przycisk **Utwórz**. Z nowego kursu ma na liście zostanie wyświetlona strona indeksu ciągu. Nazwa działu na liście strony indeksu pochodzi z właściwości nawigacji, przedstawiający został poprawnie ustanowić relacji.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Uruchom **Edytuj** strony (wyświetlenia strony indeksu ciągu, a następnie kliknij przycisk **Edytuj** na kursu).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Zmień dane na stronie, a następnie kliknij przycisk **zapisać**. Z danymi zaktualizowanych kursów zostanie wyświetlona strona indeksu ciągu.

## <a name="adding-an-edit-page-for-instructors"></a>Dodawanie strony edycji dla instruktorów

Podczas edytowania rekordu instruktora chcesz była możliwa aktualizacja instruktora office przypisania. `Instructor` Jednostka ma relacji jeden do zero lub jeden z `OfficeAssignment` jednostki, co oznacza musi obsługiwać następujących sytuacjach:

- Jeśli pierwotnie miał wartość wyczyszczenie przypisania pakietu office, należy usunąć i usunąć `OfficeAssignment` jednostki.
- Jeśli użytkownik wprowadzi wartość przydziału pakietu office i początkowo był pusty, należy utworzyć nowy `OfficeAssignment` jednostki.
- Jeśli użytkownik zmieni wartość przydziału pakietu office, należy zmienić wartość w istniejącym `OfficeAssignment` jednostki.

Otwórz *InstructorController.cs* i przyjrzyj się `HttpGet` `Edit` metody:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Nie jest utworzony szkielet kodu, co ma. Konfigurowanie danych dla listy rozwijanej, ale co jest potrzebne jest pole tekstowe. Ta metoda Zastąp następujący kod:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Ten kod porzuca `ViewBag` instrukcji i dodaje wczesny ładowania skojarzonych z nim `OfficeAssignment` jednostki. Nie można wykonać ładowania wczesny z `Find` metody, więc `Where` i `Single` metody są używane zamiast tego wybrać instruktora.

Zastąp `HttpPost` `Edit` metodę z następującym kodem. który obsługuje aktualizacje przypisania pakietu office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Odwołanie do `RetryLimitExceededException` wymaga `using` instrukcji; Aby dodać go, kliknij prawym przyciskiem myszy `RetryLimitExceededException`, a następnie kliknij przycisk **rozwiązać** - **przy użyciu System.Data.Entity.Infrastructure**.

![Rozwiąż wyjątku ponowienia próby](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Kod wykonuje następujące czynności:

- Zmienia nazwę metody, aby `EditPost` ponieważ podpis teraz jest taka sama jak `HttpGet` — metoda ( `ActionName` atrybut określa, czy adres URL /Edit/ jest nadal używane).
- Pobiera bieżący `Instructor` jednostki z bazy danych przy użyciu wczesny ładowania dla `OfficeAssignment` właściwości nawigacji. To jest identyczny jak w `HttpGet` `Edit` metody.
- Aktualizuje pobranej `Instructor` jednostki wartościami z integratora modelu. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) przeciążenia używane umożliwia *dozwolonych* właściwości, które chcesz dołączyć. Pozwala to uniknąć nadmiernego publikowanie zgodnie z objaśnieniem w [drugi samouczek](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Jeśli w lokalizacji biura jest puste, ustawia `Instructor.OfficeAssignment` właściwości na wartość null, aby powiązane wiersza w `OfficeAssignment` tabeli zostaną usunięte.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Zapisuje zmiany w bazie danych.

W *Views\Instructor\Edit.cshtml*, po `div` elementy **Data zatrudnienia** pola, Dodaj nowe pole do edycji lokalizacji pakietu office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Uruchom strony (wybierz **instruktorów** a następnie kliknij pozycję **Edytuj** na instruktora). Zmień **oddział** i kliknij przycisk **zapisać**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Dodawanie przydziałów kursu instruktora edycji strony

Instruktorów może nauczyć dowolną liczbę kursów. Teraz będzie zwiększenia strony Edytuj instruktora przez dodanie możliwości zmiany przypisania kursu za pomocą grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Relacja między `Course` i `Instructor` jednostek jest wiele do wielu, co oznacza, że nie masz bezpośredni dostęp do właściwości klucza obcego, które znajdują się w tabeli sprzężenia. Zamiast tego, dodawanie i usuwanie jednostek, do i z `Instructor.Courses` właściwości nawigacji.

Interfejs użytkownika, który umożliwia zmianę które kursy instruktora jest przypisane do to grupa pól wyboru. Wyświetlane jest pole wyboru dla porach co w bazie danych, a te, które instruktora jest aktualnie przypisana do są zaznaczone. Użytkownika można zaznaczyć lub wyczyścić pola wyboru, aby zmienić przypisania kursu. Gdyby większa liczba kursów będzie prawdopodobnie chcesz użyć innej metody prezentacji danych w widoku, ale może użyć tej samej metody manipulowanie właściwości nawigacji, aby można było utworzyć lub usuwanie relacji.

Aby zapewnić dane do widoku listy pól wyboru, użyjesz klasy modelu widoku. Utwórz *AssignedCourseData.cs* w *ViewModels* folderu i Zamień istniejący kod następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

W *InstructorController.cs*, Zastąp `HttpGet` `Edit` metodę z następującym kodem. Zmiany zostały wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Ten kod dodaje wczesny ładowania dla `Courses` właściwość nawigacji i wywołuje nowe `PopulateAssignedCourseData` metody, aby podać informacje dotyczące używania tablicy pole wyboru `AssignedCourseData` wyświetlić klasy modelu.

Kod w `PopulateAssignedCourseData` metoda odczytuje przez wszystkie `Course` klasa modelu jednostki, aby załadować listę kursów przy użyciu widoku. Dla każdego kursu kodu sprawdza, czy porach występuje w instruktora `Courses` właściwości nawigacji. Aby utworzyć wydajne wyszukiwanie podczas sprawdzania, czy kursu jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w [zestaw HashSet](https://msdn.microsoft.com/library/bb359438.aspx) kolekcji. `Assigned` Właściwość jest ustawiona na `true` kursów instruktora jest przypisany. Widok użyje tej właściwości w celu określenia, które wyboru pola musi być wyświetlane jako wybrane. Na koniec listy jest przekazywany do widoku w `ViewBag` właściwości.

Następnie dodaj kod, który jest wykonywany, gdy użytkownik kliknie **zapisać**. Zastąp `EditPost` metodę z następującym kodem, który wywołuje nowej metody, która aktualizuje `Courses` właściwość nawigacji `Instructor` jednostki. Zmiany zostały wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Podpis metody teraz różni się od `HttpGet` `Edit` metody, więc zmienia nazwę metody z `EditPost` do `Edit`.

Ponieważ w widoku nie ma kolekcję `Course` jednostek, integratora modelu nie można automatycznie zaktualizować `Courses` właściwości nawigacji. Zamiast integratora modelu do zaktualizowania `Courses` właściwość nawigacji, należy to zrobić w nowym `UpdateInstructorCourses` metody. Dlatego należy wyłączyć `Courses` właściwości z powiązania modelu. To nie wymaga żadnych zmian do kodu, który wywołuje [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) ponieważ używasz *listę dozwolonych podobnej* przeciążenia i `Courses` nie ma na liście include.

Jeśli nie zostały zaznaczone pola wyboru kod w `UpdateInstructorCourses` inicjuje `Courses` właściwości nawigacji o pustej kolekcji:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Kod, a następnie przetwarza w pętli wszystkich kursów w bazie danych i sprawdza każdego kursu względem nich aktualnie przypisane do instruktora od komputerów, które wybrano w widoku. W celu ułatwienia wyszukiwania wydajne, ostatnie dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli zaznaczono pole wyboru dla porach, ale kursu nie znajduje się w `Instructor.Courses` właściwości nawigacji kursu zostanie dodany do kolekcji we właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Jeśli nie zostało zaznaczone pole wyboru dla porach, ale znajduje się w trakcie `Instructor.Courses` właściwość nawigacji, kursu zostanie usunięta z właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

W *Views\Instructor\Edit.cshtml*, Dodaj **kursy** pole z tablicą pola wyboru przez dodanie poniższego kodu bezpośrednio po `div` elementy `OfficeAssignment` pola i przed `div` elementu **zapisać** przycisk:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Po wklejeniu kodu, jeśli podziały wierszy i wcięcia wyglądają, jak w tym miejscu, ręcznie usuń wszystkie elementy tak, aby wyglądało tu wyświetlić. Wcięcie nie musi być idealne, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@</tr>` linie muszą być w jednym wierszu pokazany lub zostanie wyświetlony błąd w czasie wykonywania.

Ten kod tworzy tabelę HTML, który zawiera trzy kolumny. W każdej kolumnie ma postać pola wyboru, a po nim tekstem, który składa się z kursu numer i tytuł. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), które informuje o tym integratora modelu, które mają być traktowane jako grupa. `value` Atrybut każdego pola wyboru ma ustawioną wartość `CourseID.` gdy strona jest przesyłana, integratora modelu przekazuje tablicy do kontrolera, który składa się z `CourseID` wartości dla pola wyboru, które zostały wybrane.

Gdy pola wyboru początkowo są renderowane, te, które są przypisane do instruktora kursów ma `checked` atrybuty, które wybierze je (wyświetlanych jest sprawdzany je).

Po zmianie przypisania kursu, należy sprawdzić zmiany, gdy lokacji powróci do `Index` strony. W związku z tym należy dodać kolumnę do tabeli na tej stronie. W takim przypadku nie trzeba używać `ViewBag` obiektu, ponieważ jest już informacje mają być wyświetlane `Courses` właściwość nawigacji `Instructor` jednostki, która przechodząc na stronę jako model.

W *Views\Instructor\Index.cshtml*, Dodaj **kursów** nagłówek bezpośrednio po **Office** nagłówek, jak pokazano w poniższym przykładzie:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Następnie dodaj nowe komórki szczegółów bezpośrednio po komórki szczegółów lokalizacji pakietu office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Uruchom **indeksu instruktora** stronę, aby zobaczyć kursy przypisane do każdego instruktora:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Kliknij przycisk **Edytuj** na instruktora, aby wyświetlić stronę edycji.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Zmiana przypisania niektórych kursu, a następnie kliknij przycisk **zapisać**. Wprowadzone zmiany zostaną odzwierciedlone na stronie indeksu.

 Uwaga: w tym miejscu podejście do edytowania danych kursu instruktora dobrze działa w przypadku istnieje ograniczona liczba kursów. Kolekcje, które są znacznie większe innego interfejsu użytkownika i różne metody aktualizacji będą wymagane.  
 

## <a name="update-the-deleteconfirmed-method"></a>Metoda DeleteConfirmed aktualizacji

W *InstructorController.cs*, Usuń `DeleteConfirmed` — metoda i wstaw poniższy kod w jego miejscu.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Ten kod wprowadza następujące zmiany:

- Jeśli instruktora zostanie przypisany jako administrator dział, usuwa przypisanie instruktora z działu. Bez tego kodu jak błąd integralności referencyjnej w przypadku próby usunięcia instruktora, który został przypisany jako administrator dla działu.

Ten kod nie obsługuje scenariusza instruktora jeden przypisany jako administrator dla wielu działów. W ostatnim Samouczek zostanie dodany kod, który uniemożliwia w tym scenariuszu w toku.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Dodaj lokalizację pakietu office i kursy do tworzenia strony

W *InstructorController.cs*, Usuń `HttpGet` i `HttpPost` `Create` metod, a następnie dodaj następujący kod w ich miejscu:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Ten kod jest podobna do instrukcji dotyczących metod edycji z tą różnicą, że początkowo nie zaznaczono żadnych kursów. `HttpGet` `Create` Wywołania metody `PopulateAssignedCourseData` — metoda nie może być kursy wybrane, ale w celu Podaj pustą kolekcję dla `foreach` pętli w widoku (w przeciwnym razie kod widoku spowoduje zgłoszenie wyjątku odwołanie o wartości null ).

Metoda HttpPost utworzyć dodaje każdego wybranego kursu właściwość nawigacji kursy przed kod szablonu, który sprawdza, czy błędy weryfikacji i dodaje nowe instruktora do bazy danych. Kursy są dodawane, nawet jeśli istnieją błędy modelu, aby podczas występują błędy modelu (na przykład użytkownik z kluczem usługi nieprawidłową datę) tak, aby po stronie zostanie wyświetlony ponownie z komunikatem o błędzie, wprowadzone wybrane opcje kursu automatycznie zostaną przywrócone.

Zwróć uwagę, że aby można było dodać kursy do `Courses` właściwość nawigacji, należy zainicjować właściwość jako pustej kolekcji:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Alternatywą wobec tej czynności w kodzie kontrolera nakładu pracy w modelu instruktora zmieniając metody pobierającej właściwości do automatycznego tworzenia kolekcji, jeśli nie istnieje, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Jeśli zmodyfikujesz `Courses` właściwości w ten sposób można usunąć kod inicjujący jawną właściwość w kontrolerze.

W *Views\Instructor\Create.cshtml*, Dodaj pole tekstowe lokalizacji pakietu office i kursu pola wyboru po pole daty zatrudnienia i przed **przesyłania** przycisku.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Po wklejeniu kod napraw podziały wiersza i wcięcia tak jak wcześniej do edycji strony.

Uruchom tworzenia strony, a następnie dodaj instruktora.

![Utwórz instruktora z kursów](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Obsługa transakcji

Zgodnie z objaśnieniem w [samouczek podstawowe funkcje CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), domyślnie programu Entity Framework niejawnie implementuje transakcji. Scenariusze, w którym należy więcej kontrolujesz — na przykład, jeśli chcesz dołączyć operacje wykonywane poza Entity Framework w transakcji — można znaleźć [Praca z transakcji](https://msdn.microsoft.com/data/dn456843) w witrynie MSDN.

## <a name="summary"></a>Podsumowanie

To wprowadzenie do pracy z powiązanych danych zostało zakończone. Do tej pory w tych samouczkach już doświadczenie z kod, który obsługuje synchronicznych operacji We/Wy. Możesz wprowadzić bardziej efektywnie korzystać z zasobów serwera sieci web zaimplementowanie asynchroniczne kodu aplikacji, a to co można zrobić w następnym samouczku.

Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć. Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
