---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualizowanie powiązanych danych z platformą Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio...
ms.author: riande
ms.date: 05/01/2015
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e7f5fd725a0d151f19f49be9ceaf52b049d459c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756812"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Aktualizowanie powiązanych danych z platformą Entity Framework w aplikacji ASP.NET MVC
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [Pobierz plik PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i Visual Studio 2013. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednim samouczku wyświetlane powiązanych danych; w tym samouczku zostaną zaktualizowane powiązane dane. W przypadku większości relacji można to zrobić, aktualizując pola kluczy obcych lub właściwości nawigacji. W przypadku relacji wiele do wielu platformy Entity Framework nie ujawnia tabelę sprzężenia bezpośrednio, dzięki czemu można dodawać i usuwać jednostki do i z właściwości nawigacji odpowiednie.

Na poniższych ilustracjach przedstawiono niektóre stron którymi będziesz pracować.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Edytuj przez instruktorów dzięki kursom](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Dostosowywanie tworzenie i Edycja stron dla kursów

Po utworzeniu nowej jednostki kurs, musi on mieć relacji do istniejących działu. Aby ułatwić to zadanie, utworzony szkielet kodu zawiera metod kontrolera oraz tworzyć i edytować widoki, które zawierają listy rozwijanej, służąca do wybierania działu. Z listy rozwijanej zestawów `Course.DepartmentID` właściwości klucza obcego, i to wszystko Entity Framework wymaga, aby mogła załadować `Department` właściwość nawigacji z odpowiednią `Department` jednostki. Będziesz używać utworzony szkielet kodu, ale Zmień ją nieco na dodawanie obsługi błędów i sortowanie listy rozwijanej.

W *CourseController.cs*, Usuń cztery `Create` i `Edit` metody i Zastąp następujący kod:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Dodaj następujący kod `using` instrukcji na początku pliku:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList` Metoda pobiera listę wszystkich działach sortowane według nazwy, tworzy `SelectList` kolekcji dla listy rozwijanej, a następnie przekazuje kolekcji do widoku `ViewBag` właściwości. Metoda przyjmuje opcjonalną `selectedDepartment` parametr, który umożliwia kod wywołujący, aby określić element, który zostanie wybrany podczas renderowania listy rozwijanej. Widok będzie przekazywać nazwę jednostki `DepartmentID` do [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) następnie wie, pomocnika i pomocnika do przeszukania `ViewBag` dla obiektu `SelectList` o nazwie `DepartmentID`.

`HttpGet` `Create` Wywołania metody `PopulateDepartmentsDropDownList` metody bez ustawienia wybranego elementu, ponieważ dla nowych kursu działu nie zostanie nawiązane jeszcze:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit` Metoda ustawia wybranego elementu, na podstawie Identyfikatora działu, który jest już przypisana do kursu edytowanym:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost` Metody dla obu `Create` i `Edit` również dołączyć kod, który ustawia wybranego elementu, gdy ich ponownego wyświetlenia strony po wystąpieniu błędu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Ten kod zapewnia, że po stronie zostanie wyświetlony ponownie, aby wyświetlić komunikat o błędzie, niezależnie od działu wybrano pozostaje wybrane.

Widoki kurs są już szkieletu z listy rozwijanej w polu działu, ale nie chcesz podpis DepartmentID dla tego pola, aby zmienić upewnij następujący wyróżniony *Views\Course\Create.cshtml* plik Zmień podpis.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Wprowadź tę samą zmianę w *Views\Course\Edit.cshtml*.

Zwykle Generator szkieletu nie tworzenia szkieletu klucza podstawowego, ponieważ wartość klucza jest generowany przez bazę danych i nie można zmienić i nie jest zrozumiałą wartość ma być wyświetlany użytkownikom. Kurs jednostek Generator szkieletu zawiera pole tekstowe `CourseID` pola, ponieważ rozumie, że `DatabaseGeneratedOption.None` atrybut oznacza, że użytkownik powinien móc wprowadzić wartość klucza podstawowego. Ale nie rozpoznaje, ponieważ liczba ma znaczenie chcesz zobaczyć go w inne widoki, więc należy dodać ją ręcznie.

W *Views\Course\Edit.cshtml*, Dodaj pole Liczba kurs przed **tytuł** pola. Ponieważ klucz podstawowy, która jest wyświetlana, ale nie można zmienić.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Istnieje już ukrytego pola (`Html.HiddenFor` pomocnika) numeru kurs w widoku do edycji. Dodawanie *Html.LabelFor* pomocnika nie eliminuje potrzebę stosowania pole ukryte, ponieważ nie powoduje numer kurs, które mają zostać uwzględnione w przesłane dane, gdy użytkownik kliknie **Zapisz** na stronie edycji.

W *Views\Course\Delete.cshtml* i *Views\Course\Details.cshtml*, Zmień podpis nazwę działu z "Name" do "Dział" i dodać pola liczbowego kurs przed **tytułu**  pola.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Uruchom **Utwórz** strony (wyświetlenia strony indeksu kursów, a następnie kliknij przycisk **Utwórz nowy**) i wprowadź dane dla nowego kursu:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Kliknij przycisk **Utwórz**. Za pomocą nowego kursu dodany do listy zostanie wyświetlona strona indeksu kursu. Nazwa działu, na liście strony indeksu pochodzi z właściwości nawigacji, pokazujący, że relacja zostało ustanowione prawidłowo.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Uruchom **Edytuj** strony (wyświetlenia strony indeksu kursów, a następnie kliknij przycisk **Edytuj** na kurs).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Zmiany danych na stronie, a następnie kliknij przycisk **Zapisz**. Z danymi zaktualizowany kurs zostanie wyświetlona strona indeksu kursu.

## <a name="adding-an-edit-page-for-instructors"></a>Dodawanie strony edytowania dla instruktorów

Podczas edytowania rekordu przez instruktorów chcesz można zaktualizować przez instruktorów biuro. `Instructor` Jednostka ma relacji jeden do zero lub jeden z `OfficeAssignment` jednostki, co oznacza, musi obsługiwać następujące sytuacje:

- Jeśli użytkownik usunie zaznaczenie przypisania pakietu office i pierwotnie miały wartość, należy usunąć i Usuń `OfficeAssignment` jednostki.
- Jeśli użytkownik wprowadza wartość przydziału pakietu office i pierwotnie był pusty, należy utworzyć nowy `OfficeAssignment` jednostki.
- Jeśli użytkownik zmieni wartość biuro, konieczna jest zmiana wartości w istniejącym `OfficeAssignment` jednostki.

Otwórz *InstructorController.cs* i przyjrzyj się `HttpGet` `Edit` metody:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Nie jest utworzony szkielet kodu, co chcesz. Konfigurowanie danych dla listy rozwijanej, ale, co jest potrzebne jest polem tekstowym. Zastąp tę metodę z następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Ten kod spada `ViewBag` instrukcji i dodaje wczesne ładowanie skojarzonych z nim `OfficeAssignment` jednostki. Nie można wykonać wczesne ładowanie z `Find` metody, więc `Where` i `Single` metody umożliwiają zamiast tego wybierz instruktora.

Zastąp `HttpPost` `Edit` metoda następującym kodem. który obsługuje aktualizacje przypisania pakietu office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Odwołanie do `RetryLimitExceededException` wymaga `using` instrukcję; aby ją dodać, kliknij prawym przyciskiem myszy `RetryLimitExceededException`, a następnie kliknij przycisk **rozwiązać** - **przy użyciu System.Data.Entity.Infrastructure**.

![Rozwiąż wyjątku ponowienia próby](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Kod wykonuje następujące czynności:

- Zmienia nazwę metody do `EditPost` ponieważ podpis teraz jest taka sama jak `HttpGet` — metoda ( `ActionName` atrybut określa, że adres URL /Edit/ jest nadal używana).
- Pobiera bieżący `Instructor` jednostki z bazy danych przy użyciu wczesne ładowanie dla `OfficeAssignment` właściwości nawigacji. Jest to taka sama jak zrobiono `HttpGet` `Edit` metody.
- Aktualizuje pobrany `Instructor` jednostki z wartościami z integratora modelu. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) używane przeciążenie umożliwia *dozwolonych* właściwości, które chcesz dołączyć. Pozwala to uniknąć nadmiernego ogłaszania, zgodnie z objaśnieniem w [drugim samouczku](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Jeśli lokalizacji biura jest puste, ustawia `Instructor.OfficeAssignment` właściwości na wartość null, aby powiązane wiersza w `OfficeAssignment` tabeli zostaną usunięte.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Zapisuje zmiany w bazie danych.

W *Views\Instructor\Edit.cshtml*po `div` elementy **Data zatrudnienia** pola, Dodaj nowe pole do edycji w lokalizacji biura:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Uruchom stronę (wybierz **Instruktorzy** kartę, a następnie kliknij przycisk **Edytuj** na pod kierunkiem instruktora). Zmiana **lokalizacji biura** i kliknij przycisk **Zapisz**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Dodawanie przypisania kursu do instruktora edycji strony

Instruktorzy może nauczyć dowolnej liczby kursów. Teraz będzie ulepszenia strony edytowania przez instruktorów, dodając możliwość zmiany przypisania kurs przy użyciu grupy pól wyboru, jak pokazano na poniższym zrzucie ekranu:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Relacja między `Course` i `Instructor` jednostek jest wiele do wielu, co oznacza, że nie masz bezpośredni dostęp do właściwości klucza obcego, które znajdują się w tabeli sprzężenia. W takim przypadku dodawania i usuwania jednostek do i z `Instructor.Courses` właściwości nawigacji.

Interfejs użytkownika, który umożliwia zmianę kursy pod kierunkiem instruktora przypisane do jest grupą pola wyboru. Pole wyboru dla każdego kursu w bazie danych jest wyświetlany, a te, które instruktora jest aktualnie przypisana do są zaznaczone. Użytkownik może zaznacz lub wyczyść pola wyboru, aby zmienić przypisania kursu. Gdyby większa liczba kursów, prawdopodobnie chcesz użyć innej metody przedstawiania danych w widoku, ale zostanie wykorzystany ten sam sposób manipulowanie właściwości nawigacji, aby można było tworzenie lub usuwanie relacji.

Aby zapewnić dane do widoku listy pól wyboru, użyjesz klasy modelu widoku. Tworzenie *AssignedCourseData.cs* w *modele widoków* folder i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

W *InstructorController.cs*, Zastąp `HttpGet` `Edit` metoda następującym kodem. Zmiany są wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Ten kod dodaje wczesne ładowanie dla `Courses` właściwość nawigacji i wywołuje nową `PopulateAssignedCourseData` metodę w celu udostępnienia informacji za pomocą tablicy pole wyboru `AssignedCourseData` wyświetlić klasy modelu.

Kod w `PopulateAssignedCourseData` metoda czyta za pośrednictwem wszystkich `Course` klasa modelu jednostek, aby załadować listę kursów, korzystając z widoku. Dla każdego kursu kod sprawdza, czy kurs istnieje w instruktora `Courses` właściwości nawigacji. Aby utworzyć wydajne wyszukiwanie, podczas sprawdzania, czy kurs jest przypisany do instruktora, kursy przypisane do instruktora są umieszczane w [hashset —](https://msdn.microsoft.com/library/bb359438.aspx) kolekcji. `Assigned` Właściwość jest ustawiona na `true` kursów przypisano instruktora. Widok użyje tej właściwości w celu określenia, sprawdź, które pola muszą być wyświetlany jako zaznaczone. Na koniec listy jest przekazywany do widoku `ViewBag` właściwości.

Następnie dodaj kod, który jest wykonywany, gdy użytkownik kliknie **Zapisz**. Zastąp `EditPost` metody z następujący kod, który wywołuje nową metodę, która aktualizuje `Courses` właściwość nawigacji `Instructor` jednostki. Zmiany są wyróżnione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Podpis metody teraz różni się od `HttpGet` `Edit` metody, aby nazwa metody zmieni się z `EditPost` do `Edit`.

Ponieważ widok nie zawiera zbiór `Course` jednostki integratora modelu nie może automatycznie aktualizować `Courses` właściwości nawigacji. Zamiast używania integratora modelu do zaktualizowania `Courses` właściwość nawigacji, należy to zrobić w nowym `UpdateInstructorCourses` metody. W związku z tym należy wykluczyć `Courses` właściwości z wiązania modelu. To nie wymaga żadnych zmian do kodu, który wywołuje [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) ponieważ używasz *umieszczania na białej liście* przeciążenia i `Courses` nie ma na liście include.

Jeśli nie zostały zaznaczone pola wyboru kod w `UpdateInstructorCourses` inicjuje `Courses` właściwości nawigacji o pustej kolekcji:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Kod, a następnie przetwarza wszystkie kursy w bazie danych w pętli i sprawdza, czy każdego kursu względem nich aktualnie przypisane do przez instruktorów i te, które wybrano w widoku. W celu ułatwienia wyszukiwania wydajne, ostatnie dwie kolekcje są przechowywane w `HashSet` obiektów.

Jeśli zaznaczono pole wyboru dla danego kursu, ale kurs nie znajduje się w `Instructor.Courses` właściwość nawigacji kurs jest dodawany do kolekcji we właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Jeśli to pole wyboru dla danego kursu nie zostało zaznaczone, ale jest kurs `Instructor.Courses` właściwość nawigacji, kursu zostanie usunięta z właściwości nawigacji.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

W *Views\Instructor\Edit.cshtml*, Dodaj **kursów** pole z tablicą pola wyboru przez dodanie poniższego kodu bezpośrednio po `div` elementy `OfficeAssignment` pola i przed `div` elementu **Zapisz** przycisku:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Po wklejeniu kodu, jeśli podziały wierszy i wcięć wyglądają podobnie jak w tym miejscu, należy ręcznie rozwiązać wszystkie elementy, tak że wygląda na to, co widzicie tutaj. Wcięcie nie musi być idealna, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, i `@</tr>` linie muszą być w jednym wierszu pokazany lub otrzymasz błąd w czasie wykonywania.

Ten kod tworzy tabelę HTML, który ma trzy kolumny. W każdej kolumnie to pole wyboru, następuje podpis, który składa się z kursu numer i tytuł. Wszystkie pola wyboru mają taką samą nazwę ("selectedCourses"), która informuje o integratora modelu, które mają być traktowane jako grupa. `value` Atrybut każdego pola wyboru jest ustawiony na wartość `CourseID.` po opublikowaniu strony, integratora modelu przekazuje tablicę do kontrolera, który składa się z `CourseID` wartości dla pola wyboru, które zostały wybrane.

W przypadku pola wyboru są wstępnie renderowane, te, które są przypisane do instruktora kursów mieć `checked` atrybuty, które wybierze je (wyświetlanych ich zaznaczeniu tej opcji).

Po zmianie przypisania kursu, należy mieć możliwość weryfikacji zmian, gdy witryna powróci do `Index` strony. W związku z tym należy dodać kolumnę do tabeli na tej stronie. W takim przypadku nie trzeba używać `ViewBag` obiektu, ponieważ jest już dane, które mają być wyświetlane `Courses` właściwość nawigacji `Instructor` jednostki, przechodząc do strony jako model.

W *Views\Instructor\Index.cshtml*, Dodaj **kursów** nagłówka, natychmiast po **Office** nagłówka, jak pokazano w poniższym przykładzie:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Następnie dodaj nową komórkę szczegółów natychmiast po komórki szczegółów lokalizacji pakietu office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Uruchom **indeksu przez instruktorów** strony, aby zobaczyć kursy przypisane do każdego instruktora:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Kliknij przycisk **Edytuj** na pod kierunkiem instruktora, aby wyświetlić stronę edycji.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Zmienić niektóre przypisania kursów, a następnie kliknij przycisk **Zapisz**. Wprowadzone zmiany zostaną odzwierciedlone na stronę indeksu.

 Uwaga: w tym miejscu podejście do edycji danych kurs przez instruktorów dobrze działa w przypadku istnieje ograniczona liczba kursów. Dla kolekcji, które są znacznie większe innego interfejsu użytkownika i inną metodę aktualizacji będą wymagane.  
 

## <a name="update-the-deleteconfirmed-method"></a>Zaktualizuj metodę DeleteConfirmed

W *InstructorController.cs*, Usuń `DeleteConfirmed` metody i Wstaw następujący kod w jego miejscu.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Ten kod powoduje następujące zmiany:

- Jeśli instruktora jest przypisany jako administrator dział, usuwa przypisania przez instruktorów, z takim wydziale. Bez tego kodu otrzymamy błąd integralności referencyjnej Jeśli próbowano usunąć instruktora, który został przypisany jako administrator dla działu.

Ten kod nie obsługuje scenariusza instruktora jeden przypisany jako administrator dla wielu działów. Ostatnie samouczka dodasz kod, który uniemożliwia tego scenariusza występuje.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Na stronie Utwórz Dodaj lokalizację pakietu office i kursy

W *InstructorController.cs*, Usuń `HttpGet` i `HttpPost` `Create` metody, a następnie dodaj następujący kod w ich miejsce:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Ten kod jest podobny do przedstawionego dla metod edycji z tą różnicą, że początkowo nie zaznaczono żadnych kursów. `HttpGet` `Create` Wywołania metody `PopulateAssignedCourseData` metody nie, ponieważ może to być kursy wybrano, ale w celu Podaj pustą kolekcję dla `foreach` pętli w widoku (w przeciwnym razie Wyświetl kod zgłasza wyjątek odwołania o wartości null ).

Metoda HttpPost tworzenie dodaje każdego kursu wybrane właściwości nawigacji kursów, przed uruchomieniem kodu szablonu, który sprawdza, czy błędy weryfikacji i dodaje nowe przez instruktorów do bazy danych. Kursy są dodawane, nawet jeśli występują błędy modelu tak, aby w przypadku błędów modelu (na przykład użytkownika, kluczem utworzonym na podstawie wiadomość nieprawidłową datę) tak, aby po stronie zostanie wyświetlony ponownie, komunikat o błędzie, dowolne kurs z wybranych opcji, które zostały wprowadzone zostaną automatycznie przywrócone.

Należy zauważyć, że aby można było dodawać kursy, aby `Courses` właściwość nawigacji, musisz zainicjować właściwość jako pusta kolekcja:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Jako alternatywę w ten sposób w kodzie kontrolera nakładu pracy w modelu przez instruktorów, zmieniając metoda pobierająca właściwości, aby automatycznie tworzenie kolekcji, jeśli nie istnieje, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Jeśli zmodyfikujesz `Courses` właściwość w ten sposób można usunąć kod inicjowania właściwości w jawnej w kontrolerze.

W *Views\Instructor\Create.cshtml*, Dodaj pole tekstowe lokalizacji pakietu office i kurs pola wyboru po zatrudnienia pole daty, a jeśli tak, to przed **przesyłania** przycisku.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Po wklejeniu kodu, należy naprawić podziały wierszy i wcięć jak wcześniej dla strony edytowania.

Uruchom Utwórz stronę, a następnie dodaj pod kierunkiem instruktora.

![Utwórz przez instruktorów, z kursów](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Obsługa transakcji

Jak wyjaśniono w [samouczek podstawowych funkcji CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), domyślnie platforma Entity Framework niejawnie wykonuje transakcji. Zobacz scenariusze, w którym możesz muszą większa kontrola — na przykład, jeśli chcesz dołączyć operacje wykonywane poza programem Entity Framework w ramach transakcji — [Praca z transakcji](https://msdn.microsoft.com/data/dn456843) w witrynie MSDN.

## <a name="summary"></a>Podsumowanie

To wprowadzenie do pracy z powiązanych danych zostało zakończone. Do tej pory w tych samouczkach znasz już kod, który obsługuje synchronicznych operacji We/Wy. Może być aplikacja bardziej efektywnie używać zasobów serwera sieci web poprzez implementację kodu asynchronicznego i to, co należy to zrobić w następnym samouczku.

Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię. Możesz również poprosić o nowe tematy w [Pokaż mi jak za pomocą kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
