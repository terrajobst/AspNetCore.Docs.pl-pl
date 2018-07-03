---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualizowanie powiązanych danych z platformą Entity Framework w aplikacji ASP.NET MVC (6 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dde9d5022823b252e59949144e3021a53c0bdd3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371675"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aktualizowanie powiązanych danych z platformą Entity Framework w aplikacji ASP.NET MVC (6 10)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednim samouczku wyświetlane powiązanych danych; w tym samouczku zostaną zaktualizowane powiązane dane. W przypadku większości relacji można to zrobić, aktualizując odpowiednie pola kluczy obcych. W przypadku relacji wiele do wielu platformy Entity Framework nie ujawnia tabelę sprzężenia bezpośrednio, więc należy jawnie Dodawanie i usuwanie jednostek do i z właściwości nawigacji odpowiednie.

Na poniższych ilustracjach przedstawiono strony, którą będziesz pracować.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Dostosowywanie tworzenie i Edycja stron dla kursów

Po utworzeniu nowej jednostki kurs, musi on mieć relacji do istniejących działu. Aby ułatwić to zadanie, utworzony szkielet kodu zawiera metod kontrolera oraz tworzyć i edytować widoki, które zawierają listy rozwijanej, służąca do wybierania działu. Z listy rozwijanej zestawów `Course.DepartmentID` właściwości klucza obcego, i to wszystko Entity Framework wymaga, aby mogła załadować `Department` właściwość nawigacji z odpowiednią `Department` jednostki. Będziesz używać utworzony szkielet kodu, ale Zmień ją nieco na dodawanie obsługi błędów i sortowanie listy rozwijanej.

W *CourseController.cs*, Usuń cztery `Edit` i `Create` metody i Zastąp następujący kod:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList` Metoda pobiera listę wszystkich działach sortowane według nazwy, tworzy `SelectList` kolekcji dla listy rozwijanej, a następnie przekazuje kolekcji do widoku `ViewBag` właściwości. Metoda przyjmuje opcjonalną `selectedDepartment` parametr, który umożliwia kod wywołujący, aby określić element, który zostanie wybrany podczas renderowania listy rozwijanej. Widok będzie przekazywać nazwę jednostki `DepartmentID` do [ `DropDownList` Pomocnika](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), i pomocnika wie, następnie do przeszukania `ViewBag` dla obiektu `SelectList` o nazwie `DepartmentID`.

`HttpGet` `Create` Wywołania metody `PopulateDepartmentsDropDownList` metody bez ustawienia wybranego elementu, ponieważ dla nowych kursu działu nie zostanie nawiązane jeszcze:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit` Metoda ustawia wybranego elementu, na podstawie Identyfikatora działu, który jest już przypisana do kursu edytowanym:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost` Metody dla obu `Create` i `Edit` również dołączyć kod, który ustawia wybranego elementu, gdy ich ponownego wyświetlenia strony po wystąpieniu błędu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Ten kod zapewnia, że po stronie zostanie wyświetlony ponownie, aby wyświetlić komunikat o błędzie, niezależnie od działu wybrano pozostaje wybrane.

W *Views\Course\Create.cshtml*, Dodaj wyróżniony kod do utworzenia nowego pola liczbowego kurs przed **tytuł** pola. Jak wyjaśniono w samouczku wcześniej, pól klucza podstawowego nie są szkieletu domyślnie, ale ten klucz podstawowy jest znaczący, więc użytkownik, aby można było wprowadzić wartość klucza.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

W *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, i *Views\Course\Details.cshtml*, Dodaj pole Liczba kurs przed **tytułu**  pola. Ponieważ klucz podstawowy, która jest wyświetlana, ale nie można zmienić.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Uruchom **Utwórz** strony (wyświetlenia strony indeksu kursów, a następnie kliknij przycisk **Utwórz nowy**) i wprowadź dane dla nowego kursu:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Kliknij przycisk **Utwórz**. Za pomocą nowego kursu dodany do listy zostanie wyświetlona strona indeksu kursu. Nazwa działu, na liście strony indeksu pochodzi z właściwości nawigacji, pokazujący, że relacja zostało ustanowione prawidłowo.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Uruchom **Edytuj** strony (wyświetlenia strony indeksu kursów, a następnie kliknij przycisk **Edytuj** na kurs).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Zmiany danych na stronie, a następnie kliknij przycisk **Zapisz**. Z danymi zaktualizowany kurs zostanie wyświetlona strona indeksu kursu.

## <a name="adding-an-edit-page-for-instructors"></a>Dodawanie strony edytowania dla instruktorów

Podczas edytowania rekordu przez instruktorów chcesz można zaktualizować przez instruktorów biuro. `Instructor` Jednostka ma relacji jeden do zero lub jeden z `OfficeAssignment` jednostki, co oznacza, musi obsługiwać następujące sytuacje:

- Jeśli użytkownik usunie zaznaczenie przypisania pakietu office i pierwotnie miały wartość, należy usunąć i Usuń `OfficeAssignment` jednostki.
- Jeśli użytkownik wprowadza wartość przydziału pakietu office i pierwotnie był pusty, należy utworzyć nowy `OfficeAssignment` jednostki.
- Jeśli użytkownik zmieni wartość biuro, konieczna jest zmiana wartości w istniejącym `OfficeAssignment` jednostki.

Otwórz *InstructorController.cs* i przyjrzyj się `HttpGet` `Edit` metody:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Nie jest utworzony szkielet kodu, co chcesz. Konfigurowanie danych dla listy rozwijanej, ale, co jest potrzebne jest polem tekstowym. Zastąp tę metodę z następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Ten kod spada `ViewBag` instrukcji i dodaje wczesne ładowanie skojarzonych z nim `OfficeAssignment` jednostki. Nie można wykonać wczesne ładowanie z `Find` metody, więc `Where` i `Single` metody umożliwiają zamiast tego wybierz instruktora.

Zastąp `HttpPost` `Edit` metoda następującym kodem. który obsługuje aktualizacje przypisania pakietu office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Kod wykonuje następujące czynności:

- Pobiera bieżący `Instructor` jednostki z bazy danych przy użyciu wczesne ładowanie dla `OfficeAssignment` właściwości nawigacji. Jest to taka sama jak zrobiono `HttpGet` `Edit` metody.
- Aktualizuje pobrany `Instructor` jednostki z wartościami z integratora modelu. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) używane przeciążenie umożliwia *dozwolonych* właściwości, które chcesz dołączyć. Pozwala to uniknąć nadmiernego ogłaszania, zgodnie z objaśnieniem w [drugim samouczku](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Jeśli lokalizacji biura jest puste, ustawia `Instructor.OfficeAssignment` właściwości na wartość null, aby powiązane wiersza w `OfficeAssignment` tabeli zostaną usunięte.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Zapisuje zmiany w bazie danych.

W *Views\Instructor\Edit.cshtml*po `div` elementy **Data zatrudnienia** pola, Dodaj nowe pole do edycji w lokalizacji biura:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Uruchom stronę (wybierz **Instruktorzy** kartę, a następnie kliknij przycisk **Edytuj** na pod kierunkiem instruktora). Zmiana **lokalizacji biura** i kliknij przycisk **Zapisz**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Dodawanie przypisania kursu do instruktora edycji strony

Instruktorzy może nauczyć dowolnej liczby kursów. Teraz będzie ulepszenia strony edytowania przez instruktorów, dodając możliwość zmiany przypisania kurs przy użyciu grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Relacja między `Course` i `Instructor` jednostek jest wiele do wielu, co oznacza, że nie masz bezpośredni dostęp do tabeli sprzężenia. Zamiast tego zostanie dodawania i usuwania jednostek, do i z `Instructor.Courses` właściwości nawigacji.

Interfejs użytkownika, który umożliwia zmianę kursy pod kierunkiem instruktora przypisane do jest grupą pola wyboru. Pole wyboru dla każdego kursu w bazie danych jest wyświetlany, a te, które instruktora jest aktualnie przypisana do są zaznaczone. Użytkownik może zaznacz lub wyczyść pola wyboru, aby zmienić przypisania kursu. Gdyby większa liczba kursów, prawdopodobnie chcesz użyć innej metody przedstawiania danych w widoku, ale zostanie wykorzystany ten sam sposób manipulowanie właściwości nawigacji, aby można było tworzenie lub usuwanie relacji.

Aby zapewnić dane do widoku listy pól wyboru, użyjesz klasy modelu widoku. Tworzenie *AssignedCourseData.cs* w *modele widoków* folder i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

W *InstructorController.cs*, Zastąp `HttpGet` `Edit` metoda następującym kodem. Zmiany są wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Ten kod dodaje wczesne ładowanie dla `Courses` właściwość nawigacji i wywołuje nową `PopulateAssignedCourseData` metodę w celu udostępnienia informacji za pomocą tablicy pole wyboru `AssignedCourseData` wyświetlić klasy modelu.

Kod w `PopulateAssignedCourseData` metoda czyta za pośrednictwem wszystkich `Course` klasa modelu jednostek, aby załadować listę kursów, korzystając z widoku. Dla każdego kursu kod sprawdza, czy kurs istnieje w instruktora `Courses` właściwości nawigacji. Aby utworzyć wydajne wyszukiwanie, podczas sprawdzania, czy kurs jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w [hashset —](https://msdn.microsoft.com/library/bb359438.aspx) kolekcji. `Assigned` Właściwość jest ustawiona na `true` kursów przypisano instruktora. Widok użyje tej właściwości w celu określenia, sprawdź, które pola muszą być wyświetlany jako zaznaczone. Na koniec listy jest przekazywany do widoku `ViewBag` właściwości.

Następnie dodaj kod, który jest wykonywany, gdy użytkownik kliknie **Zapisz**. Zastąp `HttpPost` `Edit` metody z następujący kod, który wywołuje nową metodę, która aktualizuje `Courses` właściwość nawigacji `Instructor` jednostki. Zmiany są wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Ponieważ widok nie zawiera zbiór `Course` jednostki integratora modelu nie może automatycznie aktualizować `Courses` właściwości nawigacji. Zamiast używania integratora modelu do aktualizacji właściwości nawigacji kursów, należy to zrobić w nowym `UpdateInstructorCourses` metody. W związku z tym należy wykluczyć `Courses` właściwości z wiązania modelu. To nie wymaga żadnych zmian do kodu, który wywołuje [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) ponieważ używasz *umieszczania na białej liście* przeciążenia i `Courses` nie ma na liście include.

Jeśli nie zostały zaznaczone pola wyboru kod w `UpdateInstructorCourses` inicjuje `Courses` właściwości nawigacji o pustej kolekcji:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Kod, a następnie przetwarza wszystkie kursy w bazie danych w pętli i sprawdza, czy każdego kursu względem nich aktualnie przypisane do przez instruktorów i te, które wybrano w widoku. W celu ułatwienia wyszukiwania wydajne, ostatnie dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli zaznaczono pole wyboru dla danego kursu, ale kurs nie znajduje się w `Instructor.Courses` właściwość nawigacji kurs jest dodawany do kolekcji we właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Jeśli to pole wyboru dla danego kursu nie zostało zaznaczone, ale jest kurs `Instructor.Courses` właściwość nawigacji, kursu zostanie usunięta z właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

W *Views\Instructor\Edit.cshtml*, Dodaj **kursów** pole tablicy pól wyboru, dodając następujący wyróżniony kod bezpośrednio po `div` elementy `OfficeAssignment` pole:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Ten kod tworzy tabelę HTML, który ma trzy kolumny. W każdej kolumnie to pole wyboru, następuje podpis, który składa się z kursu numer i tytuł. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), która informuje o integratora modelu, które mają być traktowane jako grupa. `value` Atrybut każdego pola wyboru jest ustawiony na wartość `CourseID.` po opublikowaniu strony, integratora modelu przekazuje tablicę do kontrolera, który składa się z `CourseID` wartości dla pola wyboru, które zostały wybrane.

W przypadku pola wyboru są wstępnie renderowane, te, które są przypisane do instruktora kursów mieć `checked` atrybuty, które wybierze je (wyświetlanych ich zaznaczeniu tej opcji).

Po zmianie przypisania kursu, należy mieć możliwość weryfikacji zmian, gdy witryna powróci do `Index` strony. W związku z tym należy dodać kolumnę do tabeli na tej stronie. W takim przypadku nie trzeba używać `ViewBag` obiektu, ponieważ jest już dane, które mają być wyświetlane `Courses` właściwość nawigacji `Instructor` jednostki, przechodząc do strony jako model.

W *Views\Instructor\Index.cshtml*, Dodaj **kursów** nagłówka, natychmiast po **Office** nagłówka, jak pokazano w poniższym przykładzie:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Następnie dodaj nową komórkę szczegółów natychmiast po komórki szczegółów lokalizacji pakietu office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Uruchom **indeksu przez instruktorów** strony, aby zobaczyć kursy przypisane do każdego instruktora:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Kliknij przycisk **Edytuj** na pod kierunkiem instruktora, aby wyświetlić stronę edycji.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Zmienić niektóre przypisania kursów, a następnie kliknij przycisk **Zapisz**. Wprowadzone zmiany zostaną odzwierciedlone na stronę indeksu.

 Uwaga: Podejście do edycji przez instruktorów kurs danych działa dobrze w przypadku, gdy istnieje ograniczona liczba kursów. Dla kolekcji, które są znacznie większe innego interfejsu użytkownika i inną metodę aktualizacji będą wymagane.  
 

## <a name="update-the-delete-method"></a>Zaktualizuj metodę Delete

Zmień kod metody HttpPost Delete, aby rekordu przypisania pakietu office (jeśli istnieje) zostanie usunięty po usunięciu instruktora:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Jeśli spróbujesz usunąć instruktora, który jest przypisany do działu jako administrator, otrzymasz błąd integralności referencyjnej. Zobacz [bieżącą wersję tego samouczka](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) dla dodatkowego kodu, który automatycznie usunie instruktora działu gdzie instruktora jest przypisany jako administrator.

## <a name="summary"></a>Podsumowanie

To wprowadzenie do pracy z powiązanych danych zostało zakończone. Do tej pory w tych samouczkach wykonano operacji pełny zakres operacji CRUD, ale nie zostały omówione problemy ze współbieżnością. Następnego samouczka będzie wprowadzenie temat współbieżności, opisano opcje obsługi go, a następnie dodaj Obsługa kodowi CRUD, już napisanych dla jednej jednostki typu współbieżności.

Na koniec można znaleźć linki do innych zasobów platformy Entity Framework [ostatni samouczek z tej serii](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Poprzednie](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
